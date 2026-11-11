# Entregável 8 — Perguntas para a equipe DeliMarket

**Como usar:**
- **P1** = a resposta pode mudar significativamente o diagnóstico (classificação de controles ou nível de risco).
- **P2** = refina uma recomendação.
- **P3** = completa o quadro.

O case é acadêmico, então muitas respostas serão "não foi definido". Isso também é útil: registramos a resposta como **premissa de projeto**, e a recomendação correspondente passa a ser um **requisito** da proposta revisada.

## 1. Lista curta: enviar antes da reunião (até terça, 10/11, às 12 h)

| # | Pergunta | Por que importa (o que muda) |
|---|---|---|
| 1 | Qual é o SGBD do ERP e em que versão? O PostgreSQL foi uma premissa ou uma decisão? (D-01) | Toda a análise de PITR e WAL depende disso (relatório, 7.3) |
| 2 | Existe réplica **local** do banco, ou só a réplica na nuvem? Como funciona o *failover* local? (D-04) | Confirma ou descarta o banco como ponto único de falha (R04, Alto) |
| 3 | O servidor e o console de backup, o cofre na nuvem e o KMS usam as **mesmas credenciais** do domínio de produção? Quem administra cada um? (B-03, K-02) | Define R02 (Alto) e R12 |
| 4 | O **arquivo de WAL** também fica em armazenamento imutável? Qual é o modo de imutabilidade e quem pode reduzir a retenção? (B-04, B-05) | Define se o RPO de 5 min vale em um ransomware (A08) |
| 5 | Como chegaram ao RPO de 5 min e ao RTO de 1 h? Quanto tempo a operação de perecíveis tolera sem o ERP? (C-01, C-02) | Valida as metas ou indica a necessidade de BIA (A03) |
| 6 | Quem decide o *failover* e com qual critério? Durante o DR, como usuários e filiais chegam ao ERP na nuvem? (C-03, C-05) | RTO e exposição da contingência (R06, R15) |
| 7 | Diretório de identidades, DNS, certificados, emissão de NF-e e integrações funcionam no ambiente de contingência? (C-06) | É o principal motivo para o RTO não ser cumprido (R06) |
| 8 | Onde fica o KMS e como decifrariam fitas e backups locais se a nuvem estivesse indisponível ou comprometida? (K-01, K-04) | R12 (Alto) |
| 9 | A rede é segmentada? Uma estação de usuário consegue alcançar o banco e o servidor de backup? (N-01) | R01 (Crítico) |
| 10 | Há antivírus ou EDR gerenciado? As estações aceitam pendrive? (E-01, E-02) | R01 (Crítico) |
| 11 | Quais dados pessoais de clientes e funcionários são coletados (CPF, geolocalização de motoristas, saúde)? Existe tabela de temporalidade? (L-01, L-02, L-05) | R11 e R10; dimensiona o programa LGPD |
| 12 | Onde ficarão os *templates* biométricos e os logs de acesso físico, e quem pode alterá-los? (F-02) | É a preocupação levantada na reunião (A10, A12) |

## 2. Lista completa por domínio

### G — Geral

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| G-01 | P2 | O PDF "Trabalho DeliMarket (3)" é a versão final? Há versão mais recente? | Define o objeto auditado |
| G-02 | P2 | Podem enviar os diagramas em arquivo-fonte? O balanceador e o banco são realmente únicos? | Confirma A04 e a inconsistência I04 |
| G-03 | P1 | Qual é o porte assumido: número de usuários, filiais e CDs, volume do banco, banda dos links? | Dimensiona o tempo de restauração (relatório, 7.6) e os custos |
| G-04 | P2 | Há orçamento ou apetite a risco definido (mesmo que fictício)? | Viabilidade do plano |
| G-05 | P1 | Quem são, por função, os responsáveis por backup, restauração, chaves, logs, PCN e LGPD? | A02 (transversal) |

### D — Banco de dados e ERP

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| D-01 | P1 | Qual é o SGBD e qual a versão? | Base da análise técnica de backup |
| D-02 | P1 | O ERP é de mercado ou próprio? Quem aplica as correções? A licença permite rodar na nuvem? | A15; RTO |
| D-03 | P1 | Qual é o volume do banco e a taxa de geração de WAL, inclusive nos picos (faturamento, fechamento)? | Viabilidade do RPO e do RTO; custo da nuvem |
| D-04 | P1 | Existe réplica local? O *failover* é manual ou automático? Há *fencing*? | R04 |
| D-05 | P2 | Os servidores do ERP usam armazenamento compartilhado? | Ponto único de falha no armazenamento |
| D-06 | P2 | Quais contas existem no banco (aplicação, BI, DBA)? Há auditoria das operações privilegiadas? | Relatório, 6.3; R09 |

### B — Backup

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| B-01 | P2 | Qual ferramenta de backup será usada? | Cadeia incremental, criptografia, verificação |
| B-02 | P1 | Onde fica fisicamente o servidor de backup (mesma sala? outro prédio?) | A05; R16 |
| B-03 | P1 | O backup e o cofre na nuvem usam credenciais do domínio de produção? | A07; R02 |
| B-04 | P1 | O WAL é imutável? Por quanto tempo? | A08 |
| B-05 | P1 | Qual é o modo de imutabilidade e quem pode alterar a retenção? | R02 |
| B-06 | P2 | Quem pode solicitar, aprovar e executar uma restauração? Isso fica registrado? | A21; R10 |
| B-07 | P2 | Como as fitas são transportadas e guardadas? Há *drive* de fita disponível se o DC for destruído? As fitas são lidas periodicamente? | A21 |
| B-08 | P2 | O que define um teste de restauração "bem-sucedido"? Onde ficam os registros? | "0 erros" |
| B-09 | P2 | O que entra no backup além do ERP e do BI (diretório, configurações, arquivos, certificados)? | A21 |
| B-10 | P3 | A compressão é feita antes ou depois da criptografia? | Relatório, 12.1 |

### C — Continuidade, RPO e RTO

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| C-01 | P1 | Como chegaram ao RPO de 5 min e ao RTO de 1 h? Estimaram o custo por hora parada? | A03 |
| C-02 | P1 | Quanto tempo a operação tolera sem o ERP antes de perder entregas ou mercadorias? | Calibra as metas (BIA) |
| C-03 | P1 | Quem declara o desastre e com qual critério? | A06 |
| C-04 | P2 | Qual é a capacidade do ambiente *warm* (percentual da produção)? Quanto tempo leva para escalar? | RTO |
| C-05 | P1 | Como usuários e filiais acessam o ERP na nuvem durante o *failover*? Qual é o TTL do DNS? | A06, A13; R15 |
| C-06 | P1 | Identidade, DNS, certificados, NF-e/SEFAZ e integrações ficam disponíveis na contingência? | R06 |
| C-07 | P2 | Existem procedimentos manuais para operar sem o ERP? | A23 |
| C-08 | P2 | Como impedem que o banco antigo volte a aceitar gravações quando a energia voltar? | *Split-brain* |
| C-09 | P2 | O que acontece se a nuvem ficar indisponível (retenção de WAL, alertas)? | Cenário E |
| C-10 | P2 | Com que frequência o gerador é testado sob carga? Qual é a autonomia? Há contrato de combustível? | R05 |
| C-11 | P3 | Onde roda o monitoramento que detecta a queda do DC? | Relatório, 7.5 |

### K — Criptografia e chaves

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| K-01 | P1 | Onde fica o KMS (nuvem, local, HSM)? | A09; R12 |
| K-02 | P1 | Quem administra as chaves? É a mesma pessoa que administra os backups? | Segregação de funções |
| K-03 | P2 | Qual é a política de rotação e de revogação? | A09 |
| K-04 | P1 | Sem a nuvem, como decifram as fitas e os backups locais? Há custódia *offline*? | R12 |
| K-05 | P2 | Como será a criptografia em repouso do banco (disco, volume ou coluna)? | D04 |
| K-06 | P3 | A ferramenta escolhida suporta AES-256-GCM? Como a integridade é verificada? | B05 |

### I — Identidades e acessos

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| I-01 | P2 | Qual é o diretório de identidades? Ele é federado com a nuvem? | A07 |
| I-02 | P1 | Os administradores usam contas separadas? Existem contas compartilhadas? | R02, R09 |
| I-03 | P1 | Qual é o escopo do MFA: administradores, acesso remoto, console da nuvem, usuários do ERP? | R02 |
| I-04 | P2 | Como funcionam admissão, desligamento e mudança de função? Em quanto tempo o acesso é bloqueado? | R09 |
| I-05 | P2 | Há revisão periódica de acessos? | TCU, 11.2.4 |
| I-06 | P3 | Há bastion, gestão de acesso privilegiado e contas de emergência? | N06 |

### F — Segurança física

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| F-01 | P1 | Será RFID ou biometria (ou os dois)? Haverá PIN? | A12 |
| F-02 | P1 | Onde ficam os *templates* e os logs de acesso físico? Quem pode alterá-los? | Preocupação da reunião |
| F-03 | P2 | Qual é a alternativa se o leitor falhar ou faltar energia? | Contingência do acesso |
| F-04 | P3 | Há CFTV e registro de visitantes e terceiros? | F03 |
| F-05 | P3 | Onde o DC fica no prédio (andar, risco de inundação, compartimentação contra incêndio)? | A05 |

### M — Logs, monitoramento e incidentes

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| M-01 | P1 | Quais eventos serão registrados, onde serão guardados e quem administra esse armazenamento? | A10 |
| M-02 | P2 | Os relógios dos servidores são sincronizados por NTP? | M03 |
| M-03 | P2 | Por quanto tempo os logs serão guardados? | M04 |
| M-04 | P2 | Quem recebe alertas fora do horário comercial? | Detecção |
| M-05 | P1 | Existe um plano de resposta a incidentes de segurança, diferente do DRP? | A16 |
| M-06 | P2 | Em um vazamento de dados pessoais, quem comunica a ANPD e os titulares, e em quanto tempo? | L09 (3 dias úteis) |

### N — Redes

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| N-01 | P1 | A rede é segmentada? Uma estação alcança o banco e o backup? | A13; R01 |
| N-02 | P1 | Como filiais e CDs se conectam ao DC? | A13 |
| N-03 | P1 | Há acesso remoto (home office, fornecedores)? Por qual meio? | A13 |
| N-04 | P2 | Como o DC se conecta à nuvem (VPN, link dedicado, internet com TLS)? | R15 |
| N-05 | P2 | O ERP de contingência ficará exposto à internet? | R15 |
| N-06 | P3 | Há Wi-Fi de visitantes isolado? | N03 |
| N-07 | P3 | Há dispositivos IoT/OT (câmaras frias, telemetria de caminhões, coletores)? Em qual rede ficam? | Superfície de ataque (hipótese) |

### E — Endpoints

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| E-01 | P1 | Há antivírus ou EDR gerenciado centralmente? | A14; R01 |
| E-02 | P1 | O USB é permitido nas estações? | Cenário do pendrive |
| E-03 | P2 | Os usuários têm administrador local? | E03 |
| E-04 | P2 | Como são aplicadas as atualizações (sistema operacional, navegador, Office, cliente do ERP)? | R18 |
| E-05 | P3 | Os notebooks têm criptografia de disco? Celulares e coletores têm MDM? | E05 |
| E-06 | P2 | Há filtro de e-mail, SPF, DKIM e DMARC? Há treinamento contra phishing? | E06 |

### L — LGPD

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| L-01 | P1 | Quais dados pessoais de clientes são coletados? Os clientes são PJ, PF ou ambos? O CPF é necessário? | A17; minimização |
| L-02 | P1 | Quais dados de funcionários e motoristas são tratados (geolocalização, jornada, saúde)? | Dados sensíveis e de alto risco |
| L-03 | P1 | Quais bases legais foram consideradas? | L03 |
| L-04 | P2 | Há encarregado nomeado, com substituto? | L04 (Res. 18/2024) |
| L-05 | P1 | Existe tabela de temporalidade? Quais prazos por categoria (fiscal, trabalhista, comercial)? | Lacuna confirmada na reunião |
| L-06 | P1 | Em qual região ou país a nuvem ficará? | Transferência internacional |
| L-07 | P2 | Há dados pessoais no BI? Eles são pseudonimizados? | L12 |
| L-08 | P2 | Que técnica será usada na anonimização? Como garantem que é irreversível? | L07 |
| L-09 | P3 | Como o titular exerce seus direitos (canal e prazo)? | L05 |
| L-10 | P2 | Como evitarão reintroduzir dados eliminados ao restaurar um backup antigo? | L08 |

### BI

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| BI-01 | P2 | Com que frequência o BI é atualizado? | RPO do BI |
| BI-02 | P2 | O ETL lê do banco primário ou de uma réplica? Com qual conta? | BI08 |
| BI-03 | P2 | Quais regras de validação e reconciliação existem? | BI02 |
| BI-04 | P2 | Quem acessa os dashboards e quem pode exportar dados? | BI05 |
| BI-05 | P2 | Onde o BI roda durante a contingência? | BI07 |
| BI-06 | P1 | O BI pode ser reconstruído a partir do ERP ou da área bruta? | Se sim, o controle BI07 sai de "Inadequado" |

### V — Nuvem e fornecedores

| ID | Prioridade | Pergunta | Por que importa |
|---|---|---|---|
| V-01 | P1 | Qual é o provedor e a região? Há contas separadas por função? | A20 |
| V-02 | P2 | Qual é o SLA contratado e quais são as penalidades? | R14 |
| V-03 | P2 | O contrato tem cláusulas de proteção de dados (operador), direito de auditoria e plano de saída? | L11 |
| V-04 | P2 | Os dois provedores de internet têm rotas físicas comprovadamente distintas até o prédio? | I01 |
| V-05 | P3 | Há contratos de manutenção do gerador, do UPS e da climatização, com SLA? | R05 |

## 3. Como as respostas atualizam o relatório

| Se a resposta for... | Efeito |
|---|---|
| "Sim, já existe", com descrição | A hipótese vira constatação positiva. O controle sobe de classificação e o risco é recalculado |
| "Não existe" | A lacuna é confirmada e a recomendação ganha prioridade |
| "Não foi definido" | A resposta é registrada como premissa, e a recomendação passa a ser requisito da proposta revisada (AC08) |
