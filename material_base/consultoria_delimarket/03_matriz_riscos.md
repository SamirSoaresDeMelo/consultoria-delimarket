# Entregável 3 — Matriz de riscos

**Base metodológica:**
- Apostila ABNT NBR ISO/IEC 27005:2011: critérios (7.2), identificação (8.2), análise qualitativa (8.3.1 a), probabilidade (8.3.3), avaliação (8.4) e tratamento (9).
- Slides de SGSI da disciplina: escala de 1 a 5 e **Risco = Impacto × Probabilidade**.

A versão em planilha está em [03_matriz_riscos.csv](03_matriz_riscos.csv).

## 1. Critérios

**Premissa:** o risco foi avaliado **considerando a proposta implementada exatamente como está descrita**. Isso segue a ISO 27005, 8.2.4, que manda considerar os controles planejados. O "risco-alvo" é a projeção depois das ações do Entregável 5.

### 1.1 Probabilidade (P)

| Nível | Descritor | Critério usado |
|---|---|---|
| 1 | Raro | Exige uma combinação improvável de falhas; há controles preventivos robustos e evidenciados |
| 2 | Improvável | Pode ocorrer; há controles relevantes previstos, com lacunas menores |
| 3 | Possível | Ameaça plausível no setor; controles parciais ou não detalhados |
| 4 | Provável | Ameaça frequente ou ativa; controle preventivo não evidenciado |
| 5 | Quase certo | Ocorrência esperada em até 1 ano se nada for feito |

### 1.2 Impacto (I), calibrado para uma distribuidora de perecíveis

| Nível | Descritor | Critério usado (a calibrar pela BIA, ação AC12) |
|---|---|---|
| 1 | Insignificante | Sem efeito perceptível em pedidos ou entregas; nenhum dado pessoal exposto |
| 2 | Menor | Degradação de menos de 1 h ou retrabalho localizado; exposição de dados internos não pessoais |
| 3 | Moderado | Interrupção de 1 a 4 h em um processo crítico, ou perda de minutos de dados com retrabalho; exposição limitada de dados pessoais; não conformidade documental |
| 4 | Grave | Interrupção de 4 a 24 h, perda de horas de dados ou de perecíveis; exposição relevante de dados pessoais com notificação à ANPD e aos titulares; fraude material |
| 5 | Catastrófico | Interrupção de mais de 24 h ou perda permanente de dados; vazamento massivo; sanção relevante; ameaça à continuidade da empresa. O incidente de 2023 (18 h, com perda de dados) ficou entre 4 e 5 |

### 1.3 Nível de risco = P × I

| Faixa | Nível | Tratamento esperado |
|---|---|---|
| 1–4 | **Baixo** | Aceitável com monitoramento |
| 5–9 | **Médio** | Tratar dentro do plano, em prazo médio |
| 10–16 | **Alto** | Tratar com prioridade, no curto prazo |
| 20–25 | **Crítico** | Tratar imediatamente. Só a diretoria pode aceitar formalmente um risco desse nível |

> Os produtos possíveis de P × I são 1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, 16, 20 e 25. Por isso as faixas não deixam valores de fora.

## 2. Mapa de calor

### 2.1 Situação atual (proposta como está)

| Impacto \ Probabilidade | 1 Raro | 2 Improvável | 3 Possível | 4 Provável | 5 Quase certo |
|---|---|---|---|---|---|
| **5 Catastrófico** | | R03, R12 | R02 | **R01** | |
| **4 Grave** | | R05, R16 | R04, R07, R08, R09, R10, R15, R18 | R06, R17 | |
| **3 Moderado** | | | R13, R14 | R11 | |
| **2 Menor** | | | | | |
| **1 Insignificante** | | | | | |

**Distribuição:** 1 crítico, 13 altos e 4 médios.

### 2.2 Projeção após o plano de ação

| Impacto \ Probabilidade | 1 Raro | 2 Improvável | 3 Possível | 4 Provável | 5 Quase certo |
|---|---|---|---|---|---|
| **5** | R03, R12 | | | | |
| **4** | | | | | |
| **3** | R05, R16 | R01, R02, R06, R07, R09, R10, R15, R17, R18 | | | |
| **2** | | R04, R08, R11, R13, R14 | | | |
| **1** | | | | | |

**Distribuição:** 0 crítico, 0 alto, 11 médios e 7 baixos.

Os riscos R03 e R12 continuam médios porque o impacto deles continua catastrófico, mesmo com probabilidade baixa. A ISO 27005, 9.1, recomenda tratar "riscos improváveis porém graves" mesmo quando o tratamento não se justifica só pelo lado econômico.

## 3. Matriz detalhada

| ID | Ativo afetado | Ameaça | Vulnerabilidade | Controle existente na proposta | P | I | Risco | Nível | Evidência | Tratamento e ações | Alvo |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **R01** | Estações, servidores, banco, backup local, diretório | Ransomware que entra por pendrive ou phishing e se espalha lateralmente | Nenhum controle de endpoint; segmentação apenas citada; credenciais possivelmente compartilhadas com o backup e a nuvem | Imutável de 90 dias; fita *offline*; MFA administrativo (genérico) | 4 | 5 | 20 | **Crítico** | Proposta §7.4 (única menção); nenhuma ocorrência de EDR, USB ou phishing; Reunião (pendrive; "não passou na minha cabeça") | Reduzir: AC03, AC04, AC05, AC13, AC14, AC19, AC28. Compartilhar: avaliar seguro cibernético | 2×3 = 6 |
| **R02** | Réplica, contingência, cofre de backup e KMS na nuvem | Uso indevido de uma credencial privilegiada roubada (diretório ou nuvem) | Planos de administração e credenciais não separados; modo de imutabilidade e administração do KMS não definidos | "Controle de acesso independente" do servidor local de backup; imutabilidade | 3 | 5 | 15 | Alto | §5.3, §5.4, §11.3; `3-2-1-1-0.txt` (credenciais separadas) | Reduzir: AC03, AC04, AC06, AC21 | 2×3 = 6 |
| **R03** | Backups e procedimentos de restauração | Falha na restauração por backup corrompido ou incompleto, ou por procedimento não testado (repetição de 2023) | Sem responsáveis, sem critérios de sucesso nem evidências; escopo incompleto (A21) | Verificação automática, alertas e testes mensais e trimestrais (ponto forte) | 2 | 5 | 10 | Alto | §5.6; Estudo de caso §2; NC-03, arts. 16, 24–27 | Reduzir: AC01, AC07, AC20, AC23 | 1×5 = 5 |
| **R04** | Banco transacional local | Falha do servidor ou do armazenamento do banco | Banco único no diagrama; armazenamento compartilhado (hipótese) | Réplica assíncrona na nuvem; restauração local | 3 | 4 | 12 | Alto | Diagrama 1 × §4.2/§4.5 | Reduzir: AC22 (após AC12); AC08 | 2×2 = 4 |
| **R05** | Todo o DC | Falta prolongada de energia com falha do gerador único; desligamento abrupto | Gerador único; sem rotina de testes sob carga nem desligamento ordenado | UPS N+1, ATS, fontes duplas, *failover* para a nuvem | 2 | 4 | 8 | Médio | Diagrama 1 ("gerador único"); §4.4; 17799, 9.2.2 | Aceitar o gerador único **formalmente**, com compensações; Reduzir: AC24 | 1×3 = 3 |
| **R06** | ERP e logística (todos os processos) | RTO de 1 h descumprido em um desastre no DC | Sem critério de acionamento, sem decisor definido; dependências não mapeadas (identidade, DNS, certificados, NF-e, integrações, capacidade); monitoramento possivelmente dentro do DC | Roteiro de 5 etapas; testes trimestrais previstos | 4 | 4 | 16 | Alto | §6.2, §3.2; Reunião (Wesley: "meta geral") | Reduzir: AC02, AC10, AC12, AC23, AC29 | 2×3 = 6 |
| **R07** | Dados do ERP | Corrupção lógica, erro humano ou exclusão replicada; PITR lento e perda das transações válidas posteriores ao evento | Sem réplica com atraso; sem RTO para PITR; WAL sem imutabilidade | PITR com WAL (ponto forte) | 3 | 4 | 12 | Alto | §5.5, §6.4 | Reduzir: AC04, AC09, AC22 (réplica com atraso), AC23 | 2×3 = 6 |
| **R08** | Logs (lógicos e físicos) | Adulteração ou exclusão de logs por administrador ou invasor | Logs sem centralização, segregação, integridade nem NTP | "Registrados e auditados" (genérico) | 3 | 4 | 12 | Alto | §7.1, §5.6; Reunião (Emanuel); TCU p. 17, 27, 76 | Reduzir: AC15, AC24, AC26 | 2×2 = 4 |
| **R09** | ERP, banco, estoque e faturamento | Abuso de privilégio ou fraude interna (ajuste de estoque, perdas por validade, crédito) | Sem RBAC formal, revisão de acessos, segregação de funções nem trilha de transações | Menor privilégio (princípio); acesso por departamento (diretriz) | 3 | 4 | 12 | Alto | §9.2, §5.6; TCU p. 18, 30, 78, 84–85 | Reduzir: AC16, AC26, AC27 | 2×3 = 6 |
| **R10** | Dados pessoais e comerciais (ERP, BI, backups) | Vazamento por exportação no BI, restauração indevida, configuração errada na nuvem ou ambiente de teste | BI sem controle de acesso nem pseudonimização; restauração sem aprovação; IAM da nuvem não definido | Criptografia de backups e em trânsito | 3 | 4 | 12 | Alto | §8, §11; A19, A21 | Reduzir: AC18, AC20, AC21, AC25, AC28. Compartilhar: avaliar seguro | 2×3 = 6 |
| **R11** | Conformidade com a LGPD | Sanção ou dano por tratamento sem base legal, sem encarregado, sem retenção definida ou com incidente comunicado fora do prazo; biometria sem tratamento | LGPD só em diretrizes | §9–10 (princípios corretos) | 4 | 3 | 12 | Alto | §9–10; Reunião ("não foi abordado") | Reduzir: AC17, AC18 | 2×2 = 4 |
| **R12** | Chaves criptográficas | Perda de acesso às chaves (KMS indisponível, comprometido ou chave excluída) que torna os backups irrecuperáveis | Sem custódia *offline*, sem proteção contra exclusão; dependência circular com a nuvem | KMS; chaves separadas; teste de decifragem (ponto forte) | 2 | 5 | 10 | Alto | §11.3; A09 | Reduzir: AC06, AC21 | 1×5 = 5 |
| **R13** | BI e indicadores | Decisão errada tomada com indicador desatualizado ou inconsistente | Regras de validação não definidas; BI extrai do banco primário; continuidade do BI incoerente (RPO 1 h × backup diário) | Carimbo de atualização, reconciliação e deduplicação (ponto forte) | 3 | 3 | 9 | Médio | §8, §6.3, §5.2; Estudo de caso §2 | Reduzir: AC25 | 2×2 = 4 |
| **R14** | Serviços de terceiros (nuvem, provedores de internet, manutenção do gerador, combustível, ERP) | Falha ou indisponibilidade de um fornecedor sem SLA ou contrato adequado | Sem gestão de fornecedores; rotas "preferencialmente" distintas | Dois provedores de internet; nuvem contratada | 3 | 3 | 9 | Médio | §3.2, §4.1, §4.4; NC-03, art. 5º | Compartilhar (contratos e SLA) e Reduzir: AC21 | 2×2 = 4 |
| **R15** | Ambiente de contingência na nuvem | Exposição ou divergência de configuração do ambiente de DR (regras permissivas, ERP exposto à internet, controles diferentes da produção) | Sem gestão de mudanças, sem linha de base de segurança na nuvem e sem desenho de acesso durante o *failover* | Nenhum específico | 3 | 4 | 12 | Alto | §3.2, §6.2; A06, A13, A20 | Reduzir: AC13, AC21, AC23 | 2×3 = 6 |
| **R16** | Sala do DC, servidor de backup local, mídias | Acesso físico indevido ou incêndio que destrói a produção e a cópia local juntas | Acesso com fator único; CFTV e visitantes não evidenciados; localização do backup local não informada | Sensores, prevenção de incêndio, RFID ou biometria, fitas fora do local | 2 | 4 | 8 | Médio | §7.1–7.3; 17799, 9.1.2 e 9.1.4 b); TCU p. 66 | Reduzir: AC24 | 1×3 = 3 |
| **R17** | Toda a organização | Incidente de segurança sem plano de resposta: contenção tardia, evidências destruídas na restauração, comunicação à ANPD fora do prazo | Não há plano de resposta, papéis, playbooks nem procedimento de evidências | Nenhum (o DRP trata apenas indisponibilidade) | 4 | 4 | 16 | Alto | §6.2; Modelo de PSI, art. 17 X; POP 5.03 | Reduzir: AC10, AC15, AC17 | 2×3 = 6 |
| **R18** | Servidores, ERP, firewalls/VPN, estações | Exploração de vulnerabilidade não corrigida | Sem processo de gestão de vulnerabilidades e patches | "Atualizações de segurança" (menção) | 3 | 4 | 12 | Alto | §7.4; Modelo de PSI, art. 17 VII; TCU p. 95 | Reduzir: AC14, AC27, AC31 | 2×3 = 6 |

## 4. Observações para a apresentação

- **Os riscos que mais cresceram com a reengenharia** são R02, R15 e R12. A nuvem, a replicação e o KMS trazem benefícios, mas também criam uma superfície de ataque que a proposta não protege. Vale mostrar esse efeito: a solução para um risco cria riscos novos que também precisam de tratamento.
- **Os riscos que a proposta reduz bem** são os de 2023: energia (R05), falha de restauração (R03) e BI desatualizado (R13). Sem a proposta, esses três estariam entre altos e críticos.
- **Aceitação formal:** o gerador único (R05) é uma decisão legítima de custo, mas precisa constar como **risco aceito pela diretoria**, com as compensações descritas (ISO 27005, 9.3).
- **Subjetividade:** as notas de P e I são qualitativas e precisam ser recalibradas com a BIA e com o histórico de incidentes da empresa (ISO 27005, 8.3.1).
