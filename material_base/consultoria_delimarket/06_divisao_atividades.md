# Entregável 6 — Divisão de atividades da equipe de consultoria

## 1. Critérios usados

- **Volume da proposta.** Continuidade, backup, RPO/RTO e criptografia ocupam cerca de 60% das 22 páginas (§3–6 e §11). Segurança física, BI e LGPD ocupam o restante. Rede, endpoints, logs e resposta a incidentes **quase não aparecem na proposta**, mas formam a maior parte das lacunas.
- **Afinidade demonstrada na reunião de 04/11** (sem presumir competências que não apareceram):
  - **Emanuel** conduziu a discussão sobre política de backup, criptografia, retenção por categoria, separação dos logs da biometria, pendrive e o princípio de não confiar em dispositivos.
  - **Samir** levantou a anonimização e a minimização de dados (com experiência de estágio) e foi coautor da seção "Segregação de redes" da cartilha da turma.
  - **Giovana** não fala na transcrição. É coautora da cartilha. A atribuição dela segue o critério de equilíbrio de carga e coesão temática.
- **Equilíbrio:** cada pessoa fica com um eixo de complexidade técnica parecida e com cerca de 7 minutos de apresentação.
- **Integração:** os eixos se tocam em pontos definidos (seção 3). Cada ponto tem um "dono" para evitar contradição nos slides.

> A divisão pode ser ajustada. Uma alternativa natural é **trocar LGPD e redes entre Samir e Giovana**, aproveitando a experiência de Samir com anonimização. Nesse caso, Samir apresenta os slides 9 e 12, Giovana apresenta os slides 7, 8 e 10, e o restante continua igual.

## 2. Divisão proposta

| Integrante | Eixos de análise | Entregáveis sob sua responsabilidade | Pontos de integração |
|---|---|---|---|
| **Emanuel** | **Eixo B — Backup, continuidade e recuperação:** estratégia full, incremental e WAL/PITR; 3-2-1-1-0; replicação × backup × HA × DR; RPO/RTO e orçamento de tempo; os cinco cenários de recuperação; criptografia e gestão de chaves; energia e gerador no papel de continuidade; alta disponibilidade do banco | Relatório: seções 6.2 (A04–A06, A23), 6.4, 6.5, 7 e 12.1–12.3, além do Apêndice A. Plano: AC02, AC04, AC06, AC07, AC09, AC20, AC22, AC23 e AC29. Perguntas B, C, D e K. Slides 3, 4, 6 e 11 | Com **Samir**: rede isolada do backup, contas separadas na nuvem (AC04 × AC21) e MFA (AC03). Com **Giovana**: retenção por categoria (B02 × AC18), RACI de restauração e de custódia de chaves (AC01), BIA que valida as metas (AC12) |
| **Samir** | **Eixo A — Infraestrutura de segurança, redes, endpoints e auditoria técnica:** segmentação e zonas; VPN e conexão com a nuvem; Zero Trust em passos; EDR, USB, patches e *hardening*; gestão de identidades técnica (MFA, contas administrativas, PAM, bastion); **arquitetura de logs** (coletor, integridade, NTP, alertas); acesso físico (2 fatores, CFTV, eventos no coletor); segurança do ERP e das integrações | Relatório: seções 6.3, 6.6 (A11–A12), 6.7, 6.8, 6.9, 6.10 (A10), 8 e 12.4. Plano: AC03, AC05, AC13, AC14, AC15, AC24, AC26, AC27, AC28 e AC32. Perguntas I, F, M (logs), N e E. Slides 7, 8, 10 e 14 | Com **Emanuel**: backup isolado na segmentação e logs do backup e do KMS como fontes do coletor. Com **Giovana**: dados biométricos (LGPD) × controle físico; logs como dado pessoal (retenção); playbook de incidentes × alertas (AC15 × AC17) |
| **Giovana** | **Eixo C — Governança, riscos, LGPD, dados e BI:** PSI e RACI; metodologia e matriz de riscos; BIA; classificação da informação (4 níveis); LGPD (categorias, bases legais, encarregado, temporalidade, biometria, transferência internacional); BI (acesso, pseudonimização, reconciliação); **processo** de resposta a incidentes e comunicação com a ANPD; nuvem e fornecedores (contratos) | Relatório: seções 0, 3, 6.1 (A01–A03), 6.11, 6.12, 6.13, 6.10 (A16) e 9. Entregável 3 (matriz de riscos) e Entregável 4 (conformidade). Plano: AC01, AC08, AC10, AC11, AC12, AC16, AC17, AC18, AC19, AC21, AC25, AC30, AC31, AC33 e AC34. Perguntas G, L, BI, V e M (incidentes). Slides 1, 2, 5, 9, 12 e 13 | Com **Emanuel**: a BIA define as metas e o investimento em alta disponibilidade; a retenção por categoria chega ao backup. Com **Samir**: a RACI define quem administra os logs; os gatilhos do plano de incidentes vêm dos alertas |

**Carga estimada** (preparação dos slides e domínio do tema):

| Integrante | Slides | Tempo de fala | Peso técnico |
|---|---|---|---|
| Emanuel | 4 | ~7,5 min | Alto (backup e PITR) |
| Samir | 4 | ~7 min | Alto (rede e logs) |
| Giovana | 6, sendo 3 curtos | ~8 min | Médio-alto (risco e LGPD, com mais integração) |

## 3. Pontos de integração e quem decide

| Tema de fronteira | Dono | Consultado | Risco de contradição a evitar |
|---|---|---|---|
| Retenção (backup × temporalidade legal) | Giovana | Emanuel | Dizer "12 meses bastam" num slide e "5 anos" no outro. Mensagem única: **backup ≠ arquivamento** |
| Isolamento do backup (rede e credenciais) | Emanuel | Samir | AC04 e AC13 devem citar a mesma zona de backup |
| Logs do backup e do KMS | Samir | Emanuel | – |
| Biometria (LGPD × segurança física) | Giovana (LGPD) | Samir (físico) | Mensagem única: **cartão + PIN como base; biometria opcional** |
| Plano de incidentes × alertas e evidências | Giovana (processo) | Samir (técnico) | Prazo da ANPD: **3 dias úteis** (Res. 15/2024) |
| RPO/RTO × BIA | Emanuel | Giovana | As metas são **objetivos a validar**, não garantias |
| Nuvem (contas, IAM, contrato) | Giovana (contrato) | Samir (IAM) e Emanuel (cofre) | – |

## 4. O que **todos** precisam dominar (para perguntas na reunião)

1. **Resposta à questão central:** "ainda não". É sólida em continuidade física e incompleta em segurança contra ameaças intencionais e em governança.
2. **Os 5 principais riscos:** R01 ransomware via endpoint (crítico), R06 RTO, R17 incidentes, R02 credenciais e R04 banco como ponto único de falha.
3. **Os 5 principais pontos fortes:** PITR; 3-2-1-1-0; testes com decifragem; AES-256-GCM e TLS 1.3; BI com carimbo de atualização e reconciliação.
4. **Os conceitos:** replicação ≠ backup; RPO/RTO são objetivos e não garantias; anonimização ≠ pseudonimização ≠ criptografia.
5. **As 3 ações imediatas mais baratas e de maior efeito:** AC04 (cofre isolado e WAL imutável), AC03 (MFA e contas separadas), AC05 (bloqueio de USB).
6. **Postura:** reconhecer o mérito do trabalho, porque a própria DeliMarket disse que não tinha visto o conteúdo de segurança. Somos consultoria, não banca.

## 5. Cronograma até a reunião de quarta-feira, 11/11, às 15 h

| Quando | Atividade | Responsável |
|---|---|---|
| Seg 09/11 (noite) | Todos leem o `00_LEIA-ME` e as próprias seções do relatório; anotam dúvidas | Todos |
| Ter 10/11, até 12 h | Enviar à DeliMarket as **perguntas P1** (Entregável 8) para que as respostas cheguem antes ou durante a reunião | Giovana (envio), com revisão de todos |
| Ter 10/11, até 18 h | Rascunho dos slides de cada um, no modelo do roteiro (Entregável 7) | Cada integrante |
| Ter 10/11, 20 h | Chamada de integração (30 min): conferir os pontos de fronteira da seção 3 e unificar o visual | Todos |
| Qua 11/11, manhã | Ensaio cronometrado (meta de 20 a 22 min mais perguntas) | Todos |
| Qua 11/11, 15 h | Reunião com a DeliMarket | Todos |
| Depois da reunião | Atualizar o relatório com as respostas (as hipóteses viram constatações ou são descartadas) | Cada um no seu eixo |
