# Entregável 9 — Arquitetura-alvo com serviços AWS

Diagrama: [diagrama_arquitetura_aws.png](diagrama_arquitetura_aws.png) · editável: [diagrama_arquitetura_aws.drawio](diagrama_arquitetura_aws.drawio)

## 0. Premissas e honestidade sobre o que é decisão nossa

- **AWS foi escolha de apresentação**, porque a equipe citou S3 e banco da Amazon. A proposta da DeliMarket **não define provedor** (§3.2: "será contratado um provedor de nuvem"). Azure ou GCP têm equivalentes diretos (tabela da seção 4).
- **Região sa-east-1 (São Paulo)**: mantém os dados no Brasil, o que evita a análise de transferência internacional (LGPD, arts. 33–36; Res. CD/ANPD 19/2024), e reduz a latência. Custo por serviço costuma ser maior que em regiões dos EUA. Isso é uma generalização: **conferir preço e disponibilidade de cada serviço na calculadora da AWS**, inclusive do Elastic Disaster Recovery.
- **Mantivemos tudo o que a DeliMarket já desenhou** (caixas brancas). Não trocamos nada que eles propuseram. O KMS, por exemplo, **já está no Diagrama 2 deles** ("Chaves protegidas em KMS") e no §11.3. O que faltava era definir **onde roda, quem custodia e como se recupera**.
- Sem orçamento informado, o custo é relativo (baixo, médio, alto). Não inventamos valores.
- Nada aqui foi implantado ou testado. É desenho.

## 1. O que muda na arquitetura (resumo)

| # | Mudança | Antes (proposta deles) | Depois |
|---|---|---|---|
| 1 | Cópia 3 imutável | "Nuvem imutável" (sem serviço, modo ou administração) | **Amazon S3 com Object Lock em modo conformidade** + versionamento, em **conta AWS separada** |
| 2 | Banco na nuvem | "Réplica do banco" | **PostgreSQL em EC2** como réplica por *streaming* (promovível), ou RDS/Aurora via replicação lógica (ver 3.2) |
| 3 | ERP de contingência | "ERP de contingência" *warm* | **EC2 em Auto Scaling atrás de ALB**; **AWS Elastic Disaster Recovery** replica os servidores do ERP |
| 4 | Failover | "Failover / Failback" | **Route 53** com *health check* e TTL baixo + *runbook* automatizado no **Systems Manager** |
| 5 | Ligação DC ⇄ nuvem | "TLS" genérico | **AWS Site-to-Site VPN** (IPsec, 2 túneis); evoluir para **Direct Connect** conforme volume |
| 6 | Contas e identidade na nuvem | Não definido | **AWS Organizations** (contas workload, backup, logs, segurança) + **IAM Identity Center + MFA** |
| 7 | Backup | "Full + incremental + WAL" | **pgBackRest** (full/incr/diff, arquivamento de WAL, AES-256, verificação) com repositório no S3 |
| 8 | Camada analítica | ETL → BD analítico → BI | **AWS DMS (CDC)** a partir da réplica → S3 → **RDS PostgreSQL ou Redshift**; **QuickSight** (ou Power BI) com segurança por linha |
| 9 | Logs | Não definido | Coletor on-prem (**Wazuh** ou Graylog) + **CloudTrail** da organização + cópia no **S3 Object Lock** (conta log-archive) |
| 10 | Chaves | KMS (sem local nem custódia) | **AWS KMS** (CMK, exclusão com espera de 30 dias) + chave de recuperação **offline** em dupla custódia |
| 11 | HA local do banco | Banco único (diagrama) | **Patroni** (réplica local, *failover* automático com *fencing*) + réplica com atraso |
| 12 | Acesso administrativo | "MFA" | **Session Manager** (sem porta aberta) + MFA FIDO2 + contas admin separadas |
| 13 | Monitoramento fora do DC | Não definido | **CloudWatch Synthetics e alarmes** + alerta ao plantão (SNS) |

## 2. Fluxos principais (o que o desenho passa a garantir)

1. **Produção → nuvem (DR):** WAL por *streaming* pelo túnel IPsec até a réplica em EC2. Em desastre, o **Route 53** redireciona após decisão no gatilho (AC02) e o *runbook* promove a réplica.
2. **Produção → backup:** o pgBackRest grava no servidor de backup isolado (fora do AD) e envia para o S3 da **conta de backup**. O WAL também vai para lá (AC04).
3. **Ransomware:** o atacante com credencial da produção **não alcança** a conta de backup (sem federação), e o Object Lock em modo conformidade impede apagar ou reduzir a retenção, **até com usuário root da conta**.
4. **Logs:** cada fonte só **envia** ao coletor. Uma cópia diária vai para o S3 imutável da conta de logs. A CloudTrail da organização registra o que acontece na nuvem.
5. **BI:** o DMS lê da **réplica** (não do primário) e alimenta o S3 e o banco analítico. O dashboard usa segurança por linha, e o identificador pessoal já chega pseudonimizado.

## 3. Decisões que a equipe precisa conhecer (e onde se discorda)

### 3.1 Por que S3 Object Lock em modo **conformidade** e não "governança"
- **Governança:** usuários com permissão especial podem remover a retenção. Se o atacante tiver essa permissão, o controle cai.
- **Conformidade:** ninguém reduz a retenção durante o período, nem o administrador da conta. É o modo que atende ao "+1 imutável" da regra 3-2-1-1-0.
- **Trade-off:** errar o período ou gravar dado indevido significa pagar o armazenamento até o fim. Por isso definimos o período por **tabela de temporalidade** (AC18) e testamos com um *bucket* de homologação antes.
- **Alerta de compatibilidade:** o pgBackRest apaga arquivos vencidos por conta própria (*expire*). Com Object Lock em conformidade, essa remoção falha. **Verificar na versão adotada.** Solução comum: repositório principal sem *lock*, com **replicação para um segundo bucket com Object Lock**, e a retenção de longo prazo só nesse segundo. Isso deve ser validado em teste (AC23).

### 3.2 Banco na nuvem: EC2 ou RDS/Aurora?
- **Decisão recomendada para o DR: PostgreSQL em EC2.** Motivo técnico: a replicação **física** (*streaming*) do servidor local **só aceita outro PostgreSQL com acesso ao sistema**. O RDS **não pode ser réplica física** de um banco externo. A réplica em EC2 mantém o mesmo mecanismo do WAL/PITR que a DeliMarket propôs, e pode ser **promovida** em minutos.
- **RDS/Aurora é possível**, mas via **replicação lógica** (ou DMS). Perde-se parte da fidelidade: definições de estrutura (DDL) e sequências exigem tratamento, e o *failback* é mais trabalhoso. Vale para o **banco analítico**, onde o RDS faz sentido.
- **Custo:** EC2 exige operação (patches, backups, monitoramento) pela equipe. RDS reduz essa carga. É o custo de gestão contra a fidelidade do DR. **Pergunta para a DeliMarket:** quem opera o banco na nuvem? (D-01, D-04)

### 3.3 AWS Elastic Disaster Recovery (DRS) para os servidores do ERP
- Replica **continuamente em nível de bloco** os servidores de aplicação para uma área de preparação de baixo custo, e só cria as instâncias completas no momento do desastre. Encaixa no *warm standby* que eles escolheram e reduz o custo de manter servidores ligados.
- **Limites:** o *failover* ainda depende de rede, DNS, identidade e licenciamento (AC29). Conferir a disponibilidade em sa-east-1 e o licenciamento do ERP na nuvem (pergunta D-02).
- **Alternativa mais simples:** AMIs atualizadas por rotina e *runbook*, com RTO maior.

### 3.4 Site-to-Site VPN × Direct Connect
- **VPN primeiro:** configuração rápida e custo baixo. Limitada por internet, e o tráfego de replicação concorre com a operação.
- **Direct Connect** quando o volume de WAL e de restauração justificar. É a decisão que depende da banda e do volume (G-03, D-03). Para a replicação do WAL, a VPN costuma bastar.
- Em qualquer caso, **dois túneis** e a rota de volta documentada.

### 3.5 Contas separadas (o controle mais barato e mais efetivo)
- **Organizations** com contas distintas: *workload* (DR e BI), **backup**, **logs** e **segurança**. A conta de backup **não** é federada com o AD de produção e tem MFA próprio.
- Uma credencial comprometida na produção não consegue apagar o cofre. É a tradução concreta do achado A07.
- Custo: baixo (configuração). É a ação AC04 + AC21.

### 3.6 KMS (já previsto por eles) — o que falta definir
- Usar **chaves gerenciadas pelo cliente (CMK)**, com política que limita o uso aos serviços e custodiantes.
- **Exclusão com espera de 30 dias** (o máximo) e alerta no agendamento. Sem isso, um atacante agenda a exclusão e os backups cifrados viram lixo.
- **Chave de recuperação offline** em dupla custódia para o caso de a nuvem ficar indisponível ou comprometida (A09). **Esse ponto é independente da AWS.**
- Para as **fitas**, as chaves **não** podem estar só no KMS da nuvem (dependência circular).

### 3.7 O que **não** mudamos
- A fita offline continua física (LTO criptografada). O ganho é a **leitura semestral** e o **drive fora do DC** (AC20). Uma alternativa de nuvem para fita virtual existe (Storage Gateway), mas muda o desenho que eles já têm e não vimos necessidade.
- WAF, Shield Advanced, Control Tower e Security Lake ficam fora: **desproporcionais** ao porte e sem justificativa na proposta. Reavaliar depois da BIA.

## 4. Tabela de equivalência e alternativas

| Função | AWS (no diagrama) | Azure | GCP | Sem nuvem pública / *open source* |
|---|---|---|---|---|
| Armazenamento imutável | S3 Object Lock (conformidade) | Blob Storage com imutabilidade (WORM) | Cloud Storage com Bucket Lock | MinIO com Object Lock |
| Réplica do banco para DR | PostgreSQL em EC2 | PostgreSQL em VM | PostgreSQL em Compute Engine | Outro servidor PostgreSQL |
| Banco gerenciado (analítico) | RDS PostgreSQL / Redshift | Azure Database for PostgreSQL | Cloud SQL / BigQuery | PostgreSQL |
| DR de servidores | Elastic Disaster Recovery | Azure Site Recovery | (Backup and DR Service) | Veeam / Proxmox |
| DNS com *failover* | Route 53 | Traffic Manager / Azure DNS | Cloud DNS | DNS com *health check* |
| Chaves | KMS | Key Vault | Cloud KMS | HashiCorp Vault / HSM |
| Auditoria da nuvem | CloudTrail | Activity Log | Cloud Audit Logs | – |
| Identidade | IAM Identity Center | Entra ID | Cloud Identity | Keycloak |
| Acesso administrativo | Session Manager | Bastion | IAP | Bastion (Teleport) |
| Monitoramento externo | CloudWatch Synthetics | Application Insights | Cloud Monitoring | Uptime Kuma |
| Logs / SIEM | CloudTrail + coletor | Sentinel | Chronicle | Wazuh / Graylog |
| BI | QuickSight | Power BI | Looker | Metabase / Power BI |

> **Os produtos de EDR, patches e varredura** citados no diagrama (Defender for Endpoint, CrowdStrike, WSUS/Intune, Nessus/OpenVAS) são **exemplos**. A escolha depende do que a DeliMarket já tem licenciado.

## 5. Custo relativo (sem valores)

| Componente | Custo relativo | Observação |
|---|---|---|
| Contas separadas, IAM Identity Center, CloudTrail | **Baixo** | Configuração |
| S3 Object Lock | **Baixo a médio** | Cresce com o volume e a retenção (90 dias na proposta) |
| Site-to-Site VPN | **Baixo** | Direct Connect é **médio a alto** |
| Réplica EC2 (instância dimensionada para receber o WAL) | **Médio** | Instância ligada o mês todo e disco |
| Elastic Disaster Recovery | **Médio** | Cobra por servidor replicado, mais o armazenamento de preparação |
| Servidores completos no desastre | **Variável** | Pagos só no desastre ou nos testes |
| RDS/Redshift para BI | **Médio a alto** | Avaliar o volume do BI antes (BI-01) |
| EDR e SIEM gerenciado | **Médio** | Licença por *endpoint* ou assinatura |

**Para orçamento restrito:** manter a sequência mínima do Entregável 5 (seção 4.1), contas separadas + S3 Object Lock + VPN + réplica em EC2. Elastic Disaster Recovery, RDS/Redshift e Direct Connect entram **depois da BIA** (AC12).

## 6. Como isso se liga ao plano de ação

| Mudança | AC |
|---|---|
| S3 Object Lock, conta de backup separada, WAL imutável | AC04 |
| pgBackRest, relatórios de teste, fitas | AC20 |
| Organizations, IAM, CloudTrail, contrato, região | AC21 |
| VPN, segmentação em VLAN, bastion | AC13, AC03 |
| Patroni, réplica com atraso | AC22 |
| Route 53, Elastic DR, monitoramento externo, teste de *failover* | AC02, AC09, AC23, AC29 |
| KMS: espera de exclusão e chave offline | AC06 |
| DMS, QuickSight, pseudonimização | AC25 |
| Wazuh/Graylog, S3 de logs | AC15 |

## 7. Pontos que **precisam** de teste antes de afirmar (não afirmamos que funcionam)

1. **Compatibilidade do pgBackRest com Object Lock** na versão escolhida (3.1).
2. **RTO de 60 min** com Elastic DR, Route 53 e identidade na nuvem. Cronometrar cada etapa (relatório, 7.5).
3. **Tempo de promoção** da réplica em EC2 e **RPO real** com a banda da VPN.
4. **Restauração dentro da nuvem** a partir do bucket de backup (relatório, 7.6).
5. **Disponibilidade** de cada serviço em sa-east-1, **preços** e **licenciamento do ERP** na nuvem.
