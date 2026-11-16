# Resumo de contexto: consultoria de SI da DeliMarket

Para quem for continuar este trabalho sem ter visto a conversa. Gerado em 16/11/2026 a partir da sessão com o Claude Code.

## 1. O trabalho

- **Disciplina:** Segurança e Auditoria de Sistemas de Informação, "Jogo de Negócios", com o professor Luiz Antonio Antunes.
- **Tarefa:** a equipe atua como **consultoria de segurança da informação** sobre a reengenharia proposta por outra equipe (a **DeliMarket**), analisa a proposta, propõe adequações viáveis para o negócio e apresenta uma proposta de melhoria.
- **Nossa equipe:** Samir, Giovana e Emanuel. A equipe demandante é a DeliMarket (autores da proposta; na reunião falaram João Vinícius, Wesley e José Vítor).
- **A empresa (fictícia):** distribuidora nacional de perecíveis, com ERP e BI em um único data center próprio. Em 2023, uma pane elétrica deixou o ERP fora por 18 h, com perda de pedidos, backup mais recente corrompido e BI desatualizado.
- **Reunião entre as equipes:** 04/11/2026 (transcrição em `reuniao_4_11.txt`). Havia uma segunda marcada para **11/11 às 15 h**. Não sei o que aconteceu nela.
- **Pasta de trabalho:** `/home/samir/Documentos/discoC/TrabalhosDaFaculdade/seguranca_e_auditoria/`. A proposta da DeliMarket e os diagramas deles ficam em `trabalho_a_ser_analisado/`. Os nossos entregáveis ficam em `consultoria_delimarket/`.

## 2. Conclusão da auditoria (resposta à pergunta central)

**A proposta ainda não é suficientemente segura.** Ela é sólida como plano de **continuidade contra falha física** (o tipo do incidente de 2023). Quase não trata **ameaças intencionais** nem **governança**.

**Pontos fortes** (devem ser mantidos): PITR com WAL; regra 3-2-1 com cópia imutável e fita offline; testes mensais com decifragem; AES-256-GCM e TLS 1.3; BI com horário da última atualização e reconciliação; replicação distinguida de backup.

**Lacunas principais:**
- Nenhum controle de endpoint (a DeliMarket admitiu na reunião que não pensou no pendrive).
- Segmentação de rede só citada; VPN, Wi-Fi e filiais não aparecem.
- Credenciais de produção, nuvem, backup e chaves não separadas: uma credencial roubada pode apagar tudo.
- Logs sem arquitetura de proteção (risco de adulteração por administrador, levantado por Emanuel na reunião).
- Sem plano de resposta a incidentes e sem papéis definidos.
- LGPD só em diretrizes: sem classificação, sem tabela de retenção, biometria sem tratamento.
- RPO de 5 min e RTO de 1 h sem evidência. Valem, no máximo, para perda do data center, **não** para ransomware.
- Contradições internas: o texto diz banco com réplicas, o diagrama mostra um banco único, e há balanceador único.
- Faltaram itens que o estudo de caso exigia: o exemplo de criptografia da palavra "sistemas" e o cronograma.

**Síntese:** a proposta prepara a DeliMarket para o incidente de 2023, não para o próximo. As correções mais importantes são baratas.

## 3. Números

| Item | Valor |
|---|---|
| Arquivos da pasta lidos | 23 (incluindo PDF escaneado, slides só com imagem e `.ppt` antigo) |
| Controles classificados | 93: 15 adequados, 36 parcialmente adequados, 1 inadequado, 40 não evidenciados, 1 não aplicável |
| Achados | 23 (A01 a A23) |
| Riscos | 18: hoje **1 crítico, 13 altos, 4 médios**. Projetado após o plano: 11 médios e 7 baixos |
| Plano de ação | 34 ações (AC01 a AC34): 10 imediatas, 11 de curto prazo, 8 de médio, 5 de longo prazo |
| Boas práticas confrontadas | 74, com página das fontes |

**Top 5 riscos:** R01 ransomware via estação (crítico, 20) · R06 RTO descumprido (16) · R17 sem resposta a incidentes (16) · R02 credencial privilegiada que apaga réplica, backups e chaves (15) · R04 banco local como ponto único de falha (12).

**Ações imediatas de maior efeito:** AC04 (cofre de backup em conta separada, imutável, com WAL imutável) · AC03 (MFA e contas administrativas separadas) · AC05 (bloqueio de USB) · AC06 (custódia offline das chaves) · AC01 e AC02 (papéis e critérios para declarar desastre).

## 4. Arquivos gerados (pasta `consultoria_delimarket/`)

| Arquivo | Conteúdo |
|---|---|
| `00_LEIA-ME_sumario_executivo.md` | Resposta em 30 segundos e índice (não lista o 09 nem o 10) |
| `01_inventario_documental.md` | Os 23 arquivos, relevância, desatualizados e lacunas do conjunto |
| `02_relatorio_auditoria.md` | **Relatório principal** (~1150 linhas): metodologia, causa raiz de 2023, 23 achados, análise técnica de backup (5 cenários, orçamento de 60 min do RTO) e de logs, 93 controles, Apêndice A com exemplo AES-256-GCM da palavra "sistemas" |
| `03_matriz_riscos.md` e `.csv` | Escalas, mapas de calor, 18 riscos |
| `04_matriz_conformidade_boas_praticas.md` | 74 práticas dos PDFs vs proposta, e referências externas separadas por natureza |
| `05_plano_de_acao.md` | 34 ações por fase, com responsável por função, prazo, custo relativo e verificação |
| `06_divisao_atividades.md` | Divisão entre Samir (infra, rede, endpoints, logs), Emanuel (backup, continuidade, cripto) e Giovana (governança, riscos, LGPD, BI) |
| `07_roteiro_apresentacao.md` | 14 slides com mensagem, evidência, visual e apresentador (~22 min), mais perguntas prováveis |
| `08_perguntas_para_delimarket.md` | 12 perguntas P1 e lista completa por domínio |
| `09_arquitetura_aws.md` | Arquitetura-alvo com serviços AWS, decisões e armadilhas |
| `diagrama_arquitetura_aws.png` e `.drawio` | O diagrama (seção 6) |

## 5. Convenções e cuidados

- **Códigos:** A01–A23 (achados), R01–R18 (riscos), AC01–AC34 (ações), P1–P13 (pontos positivos), AP01–AP06 (controles de aplicação) e G/D/B/C/K/I/F/M/N/E/L/BI/V (perguntas).
- **Avaliamos o desenho, não a implantação.** "Não evidenciado" significa que o controle não aparece na documentação, não que ele não exista. Nada foi testado.
- **PostgreSQL é premissa da própria proposta**, não fato confirmado (pergunta D-01). As conclusões sobre credenciais, imutabilidade e testes valem para qualquer SGBD.
- **Custos são relativos** (baixo, médio, alto) e os tempos de restauração são ilustrativos, porque a DeliMarket não informou porte, volume, banda nem orçamento.
- **Citações legais** (LGPD, Res. CD/ANPD 15, 18 e 19/2024, CTN) foram conferidas na web só em fontes secundárias e **precisam de validação jurídica**. Não são parecer.
- **Algumas referências da pasta estão desatualizadas** (TCU 2012, ISO 17799:2005, 27005:2011, slides de CPP anteriores à Lei 13.964/2019). Foram usadas como critério conceitual, com ressalvas.
- **Pontos corrigidos durante a sessão:**
  - Números de slide errados nos pptx do professor.
  - Duas atribuições que diziam mais do que a fonte: o POP 5.04 só descreve SPF e DKIM, e `conteudo.pptx` só mostra os dois organogramas.
  - Colisão de IDs, resolvida renomeando os controles de aplicação para AP01–AP06.
  - **O KMS já existe no Diagrama 2 deles** ("Chaves protegidas em KMS") e no §11.3 da proposta. Em um diagrama anterior ele estava pintado como novo, o que foi corrigido.
- **Ponto sensível de conteúdo:** na reunião se afirmou que criptografar o backup o torna "muito maior". Na verdade o AES-GCM acrescenta só 16 bytes de verificação. O custo real é que dado cifrado não comprime (comprimir antes de cifrar) e o risco real é perder a chave (relatório, 12.1). Combinem o tom com o Emanuel antes de falar disso.

## 6. O diagrama

Base: o **Diagrama 1 deles** (captura de WhatsApp em `trabalho_a_ser_analisado/`), mantendo a mesma estrutura. **Caixas brancas** são o que a DeliMarket já tem. **Caixas verdes** são o que a consultoria acrescenta, com o código da ação (ACxx).

**Estrutura mantida (branco):** Operação → Internet A e B + firewalls redundantes → balanceador → ERP principal e reserva → banco transacional único; UPS + gerador único; bloco Backup 3-2-1 (full semanal, incremental diário, WAL contínuo; cópia 3 nuvem imutável; cópia 2 backup local; fita offline semanal); bloco Nuvem (réplica do banco, ERP de contingência, failover/failback); bloco Camada analítica (ETL, banco analítico, dashboards BI); e o KMS do Diagrama 2.

**Acréscimos (verde), por região:**
- **Usuários:** VPN com MFA para filiais e acesso remoto; estações com EDR, USB bloqueado por GPO, sem admin local, BitLocker.
- **Data center:** segmentação em VLANs (zonas de usuários, aplicação e dados); segundo balanceador (HAProxy + Keepalived); Patroni com réplica local e failover automático com *fencing*; réplica com atraso (1–4 h); ponto para gerador móvel, teste sob carga e desligamento ordenado; **AWS Site-to-Site VPN** (2 túneis IPsec) para a nuvem.
- **Backup:** pgBackRest; **Amazon S3 com Object Lock (modo conformidade)** em conta AWS separada, com o WAL também imutável; servidor de backup em VLAN isolada, fora do AD; leitura periódica das fitas e drive fora do DC.
- **Nuvem (AWS, sa-east-1):** PostgreSQL em EC2 como réplica promovível; EC2 em Auto Scaling atrás de ALB com **AWS Elastic Disaster Recovery**; Route 53 com health check e runbook no Systems Manager; AWS Organizations com contas separadas e IAM Identity Center; CloudWatch Synthetics (monitoramento de fora do DC); Managed Microsoft AD, ACM e Secrets Manager (certificado NF-e).
- **Analítica:** AWS DMS (CDC) a partir da réplica → S3 → RDS PostgreSQL ou Redshift; QuickSight (ou Power BI) com segurança por linha e pseudonimização.
- **Faixa de segurança (verde, exceto o KMS):** bastion com Session Manager e MFA FIDO2; diretório de identidades com RBAC; coletor de logs (Wazuh ou Graylog); cópia imutável dos logs no S3 e CloudTrail; AWS KMS com espera de 30 dias para exclusão e chave de recuperação offline; EDR, patches e varredura de vulnerabilidades.

**Decisões do diagrama a conhecer:**
- **AWS foi escolha de apresentação**, porque a equipe citou S3 e banco da Amazon. A proposta não define provedor. Há tabela de equivalência com Azure, GCP e open source no `09`.
- **Banco de DR em EC2, não RDS**, porque o RDS não pode ser réplica física de um PostgreSQL externo. O RDS entra só no banco analítico.
- **Object Lock em modo conformidade** (e não governança), porque nem o administrador consegue encurtar a retenção.
- **Possível conflito pgBackRest × Object Lock** (o pgBackRest apaga vencidos por conta própria). **Precisa de teste.** O `09` traz uma saída provável.
- **Não verificado:** disponibilidade e preço dos serviços em sa-east-1, licenciamento do ERP na nuvem e abertura do `.drawio` (só validei o XML).

## 7. Pendências e próximos passos

1. **Drive:** foi criada a pasta [Consultoria DeliMarket – Segurança e Auditoria](https://drive.google.com/drive/folders/1jipRYgBBPrjFEU5ZN4lPhsWeT6fxtr4r) (privada), mas só o `00_LEIA-ME` subiu, em versão resumida. O conector não envia arquivo do disco. Arrastem os arquivos de `consultoria_delimarket/` para dentro.
2. **Perguntas à DeliMarket:** a lista curta de 12 perguntas (`08`) deveria ter ido antes da reunião de 11/11. Atualizar as hipóteses do relatório com as respostas, se houver.
3. **Índice:** incluir o `09` e o `10` no `00_LEIA-ME`.
4. **Validar:** o jurídico e a contabilidade (citações legais e prazos fiscais) e a disponibilidade e o custo dos serviços AWS.
5. **Testar (se possível, em laboratório):** pgBackRest com Object Lock, o RTO de 60 min e a restauração dentro da nuvem.
6. **Os scripts que geraram os diagramas** estavam em uma pasta temporária (`/tmp/claude-…/scratchpad/`) e podem desaparecer. O `.drawio` é a fonte editável. Para refazer o PNG, exporte pelo próprio draw.io.

## 8. Como retomar no Claude Code

Abram a pasta `seguranca_e_auditoria/` e peçam, por exemplo: "leia `consultoria_delimarket/10_resumo_contexto_conversa.md` e continue a partir daí". O resumo acima e os arquivos `00` a `09` dão o contexto completo.
