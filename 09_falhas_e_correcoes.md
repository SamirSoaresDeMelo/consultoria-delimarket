# DeliMarket — Principais falhas da proposta e correções

Legenda: **Omite** = a proposta não fala · **Parcial** = fala, mas incompleto · **Incorreto** = contradiz a si mesma ou não se sustenta.

## 1. Riscos e governança

| # | Tipo | Falha | Correção |
|---|---|---|---|
| 1 | Omite | Só trata falhas de energia, servidor, rede e dados. Ameaças intencionais (pendrive, invasão, ransomware) ficam em uma frase. | Refazer a análise de riscos incluindo ameaças intencionais, com probabilidade, impacto e critério de aceitação. |
| 2 | Omite | Ninguém tem papel definido ("a equipe", "os responsáveis"). Não se sabe quem declara desastre, restaura, guarda chaves ou notifica a ANPD. | Definir papéis (RACI) e suplentes. Quem opera não pode ser quem administra os logs. |
| 3 | Parcial | RPO 5 min / RTO 1 h sem análise de impacto no negócio (BIA) e sem prova de que são atingíveis. Sem meta para ransomware. | Fazer BIA por sistema, definir metas por cenário (incluindo ransomware) e testar. |
| 4 | Omite | Gerador único aceito sem registro formal de risco. Não cita desligamento ordenado pelo UPS (causa da corrupção de dados em 2023). | Formalizar a aceitação do risco. Adicionar desligamento automático pelo UPS, teste sob carga e contrato de combustível. |

## 2. Infraestrutura e continuidade

| # | Tipo | Falha | Correção |
|---|---|---|---|
| 5 | Incorreto | Texto fala em banco com réplicas; o diagrama mostra um banco único. Se ele cai, o ERP para. | Definir banco com standby/réplica local e mostrar no diagrama. |
| 6 | Parcial | Servidores, banco, rede e backup local ficam no mesmo prédio, mesmo gerador e mesma zona de incêndio. | Garantir uma cópia fora do site. Declarar onde cada coisa fica. |
| 7 | Parcial | RTO de 1 h sem critério de acionamento, sem decisor e sem dependências (DNS/TTL, identidade, certificados, SEFAZ, acesso das filiais, monitoramento fora do DC). | Definir quem decide o failover e com qual critério. Listar e testar as dependências. Monitoramento fora do DC. |
| 8 | Omite | "PCN" só cobre TI. Nada diz como armazém e entregas operam com o ERP fora (em 2023 as entregas pararam). | Criar procedimento manual: listas de separação impressas, registro por planilha, prioridade para perecíveis, emissão fiscal em contingência. |

## 3. Backup e chaves

| # | Tipo | Falha | Correção |
|---|---|---|---|
| 9 | Omite | Credenciais de produção, nuvem, backup e chaves não são separadas. Uma credencial roubada pode apagar tudo. | Contas administrativas separadas, MFA, cofre de backup em conta de nuvem distinta. |
| 10 | Parcial | Imutabilidade só vale para o backup diário. O arquivo de WAL não é imutável. Em ransomware, perda real pode ser de até 24 h, não 5 min. | Tornar o WAL também imutável (modo conformidade). |
| 11 | Parcial | Backup cobre só ERP e BI. Faltam diretório de identidades, DNS/DHCP, firewall, certificados, binários do ERP, código de ETL e runbooks. Sem responsável nem aprovação para restaurar. Leitor de fita fica no próprio DC. | Ampliar o escopo. Runbooks em cópia offline. Restauração só com aprovação e registro. Ter leitor de fita fora do site. |
| 12 | Parcial | Algoritmo (AES-256-GCM) está certo, mas não diz onde fica o KMS, quem guarda as chaves, rotação ou revogação. Se as chaves existem só na nuvem e ela cai, nada é decifrado. Criptografia em repouso do banco indefinida. | Definir KMS, dois custodiantes, cópia offline das chaves mestras, rotação e proteção contra exclusão. Cifrar volume do banco. |
| 13 | Incorreto | Retenção de 12 meses "por obrigação legal". Guarda fiscal é de cerca de 5 anos e não deve ser feita por backup. | Separar arquivamento legal de backup. Validar prazos com a contabilidade. |

## 4. Acesso, rede e endpoints

| # | Tipo | Falha | Correção |
|---|---|---|---|
| 14 | Omite | Nenhum controle de estação: antivírus/EDR, USB, phishing, hardening. Redundância não ajuda: servidores redundantes são cifrados junto. | Bloquear USB, instalar EDR, remover privilégio local, treinar usuários, configurar SPF/DKIM/DMARC. |
| 15 | Parcial | "Segmentação de redes" citada uma vez, sem desenho. Nada sobre VPN, Wi-Fi, filiais ou como chegam à nuvem no failover. | Desenhar zonas (usuários, servidores, backup, gerência, visitantes). VPN com MFA. Não expor o ERP de contingência direto à internet. |
| 16 | Parcial | Menor privilégio e MFA citados como princípio, sem processo. Sem revisão de acessos, sem segregação de funções (ex.: quem ajusta estoque aprova perda por validade). | Processo de concessão, revisão periódica e remoção imediata de desligados. Separar funções conflitantes. |
| 17 | Parcial | Acesso físico por "RFID **ou** biometria" é fator único. Biometria é dado sensível (LGPD) sem tratamento. Log do leitor pode ser apagado localmente. CFTV e visitantes não citados. | Cartão + PIN para sala de servidores. Se usar biometria, base legal e minimização. Enviar eventos em tempo real para o coletor central de logs. |
| 18 | Omite | Nada sobre acesso ao banco: contas, alterações diretas, trilha de auditoria. Nada sobre perfis do ERP e segredos das integrações. | Contas segregadas, auditoria de operações privilegiadas, trilha em transações sensíveis (estoque, preço, crédito), segredos fora de arquivos de configuração. |

## 5. Logs, incidentes e LGPD

| # | Tipo | Falha | Correção |
|---|---|---|---|
| 19 | Parcial | Diz que tudo será "registrado e auditado", mas não protege os logs. Quem é auditado pode apagá-los. | Coletor central separado, somente leitura, administrado por quem não é auditado. Revisão periódica. |
| 20 | Omite | Não há resposta a incidentes de segurança. "Detecção" só vê queda de serviço. Restaurar sem conter pode restaurar o malware e destruir evidências. Prazo de notificação à ANPD: 3 dias úteis. | Criar plano e playbook de incidentes. Definir antes: isolar, preservar evidência, depois restaurar. |
| 21 | Parcial | LGPD só em princípios. Sem mapa de dados, base legal, encarregado, retenção por categoria, atendimento a titulares, contratos com operadores. | Nomear encarregado (ato formal e substituto). Mapear dados, definir base legal e prazos, contratar cláusulas com a nuvem. |
| 22 | Parcial | Classificação da informação só como diretriz. | Adotar 4 níveis (público, interno, restrito, confidencial). Backup herda o nível mais alto. |

## 6. BI, nuvem e consistência

| # | Tipo | Falha | Correção |
|---|---|---|---|
| 23 | Incorreto | BI com RPO de 1 h, mas backup diário. A meta não se sustenta. | Reconstruir o BI a partir do ERP/área bruta (e dizer isso) ou aumentar a frequência do backup. |
| 24 | Parcial | BI sem controle de acesso a dashboards e exportações, com dados pessoais sem pseudonimização, extraindo do banco primário. | Segurança por perfil, pseudonimizar, extrair de réplica com conta só leitura, alerta de falha de atualização. |
| 25 | Omite | Nuvem sem contrato, SLA, região, IAM, auditoria nem plano de saída. Cenário "nuvem indisponível" não tratado. Dois provedores de internet podem compartilhar o mesmo duto. | Contrato com SLA e cláusulas de segurança, região no Brasil (ou regras de transferência internacional), plano para nuvem indisponível, comprovar rotas distintas. |
| 26 | Incorreto | Inconsistências internas: balanceador ativo-ativo × cluster ativo-passivo; "balanceadores" × um só no diagrama; metas para logística e arquivos sem mecanismo; ISO 27001 citada sem política nem declaração de aplicabilidade. | Revisar a proposta e deixar texto e diagramas coerentes. |
| 27 | Omite | Itens exigidos pelo estudo de caso não entregues: exemplo prático de criptografia da palavra "sistemas", cronograma, justificativa de custo por decisão (só qualitativa). | Entregar os três. |

## Ações de maior efeito e menor custo

1. Cofre de backup em conta de nuvem separada, imutável, incluindo WAL (itens 9 e 10).
2. MFA e contas administrativas separadas (9).
3. Bloquear USB (14).
4. Chaves em custódia offline com dois responsáveis (12).
5. Definir papéis e critério para declarar desastre (2 e 7).
