# Consultoria de Segurança da Informação — Reengenharia DeliMarket

**Equipe de consultoria:** Samir, Giovana e Emanuel · **Demandante:** equipe DeliMarket · **Data:** 09/11/2026 · **Próxima reunião:** quarta-feira, 11/11, às 15 h

## A resposta em 30 segundos

**A reengenharia é suficientemente segura? Ainda não.**

A proposta é **sólida como plano de continuidade contra falhas físicas**, que foi o tipo de incidente de 2023. Ela prevê:

- PITR com WAL;
- regra 3-2-1 complementada por cópia imutável e fita *offline*;
- testes mensais que incluem a decifragem;
- AES-256-GCM e TLS 1.3;
- BI com carimbo de atualização e reconciliação.

Por outro lado, a proposta **quase não trata ameaças intencionais nem governança**. Faltam:

- proteção de endpoints, como o pendrive levantado na reunião;
- segmentação da rede e VPN;
- separação de credenciais entre produção, nuvem e backups;
- logs protegidos contra adulteração;
- plano de resposta a incidentes;
- papéis definidos;
- LGPD aplicada na prática.

As metas de **RPO de 5 min e RTO de 1 h** são plausíveis apenas para a perda física do DC e **ainda não têm evidência** de que podem ser atingidas.

> **A proposta prepara a DeliMarket para o incidente de 2023, não para o próximo.** As correções mais importantes são baratas.

| Indicador | Valor |
|---|---|
| Documentos examinados | 23 arquivos, todos lidos (inventário no Entregável 1) |
| Controles classificados | 93 controles: 15 adequados, 36 parcialmente adequados, 1 inadequado, 40 não evidenciados, 1 não aplicável |
| Achados | 23, de A01 a A23 |
| Riscos | 18 riscos: **1 crítico, 13 altos e 4 médios**. Depois do plano: 0 crítico, 0 alto, 11 médios e 7 baixos |
| Plano | 34 ações: 10 imediatas, 11 de curto prazo, 8 de médio prazo e 5 de longo prazo |

**Os 5 principais riscos:**

| ID | Risco | Nível |
|---|---|---|
| R01 | Ransomware que entra por uma estação | Crítico (20) |
| R06 | RTO descumprido por falta de critério de acionamento e por dependências não mapeadas | Alto (16) |
| R17 | Incidente sem plano de resposta | Alto (16) |
| R02 | Uma credencial privilegiada que apaga réplica, backups e chaves | Alto (15) |
| R04 | Banco de dados local como ponto único de falha | Alto (12) |

**As 5 ações imediatas de maior efeito e baixo custo:**

| Ação | O que faz |
|---|---|
| AC04 | Cofre de backup em conta de nuvem separada, com imutabilidade em modo conformidade e WAL também imutável |
| AC03 | MFA e contas administrativas separadas |
| AC05 | Bloqueio de USB |
| AC06 | Custódia *offline* das chaves |
| AC01 + AC02 | Papéis definidos (RACI) e critérios para declarar desastre |

## Índice dos entregáveis

| # | Arquivo | Conteúdo |
|---|---|---|
| 1 | [01_inventario_documental.md](01_inventario_documental.md) | Os 23 arquivos com tipo, conteúdo e relevância; arquivos por finalidade; duplicidades e documentos desatualizados; detalhamento de cada referência; lacunas do conjunto documental |
| 2 | [02_relatorio_auditoria.md](02_relatorio_auditoria.md) | **Relatório principal:** escopo, metodologia, causa raiz de 2023, arquitetura reconstruída, pontos positivos, 23 achados, análise técnica de backup (cenários e orçamento de tempo do RTO) e de logs, classificação dos 93 controles, recomendações-chave, esclarecimentos técnicos, limitações, exemplo AES-256-GCM da palavra "sistemas" |
| 3 | [03_matriz_riscos.md](03_matriz_riscos.md) e [.csv](03_matriz_riscos.csv) | Critérios de P, I e níveis; mapas de calor atual e projetado; 18 riscos detalhados |
| 4 | [04_matriz_conformidade_boas_praticas.md](04_matriz_conformidade_boas_praticas.md) | 74 boas práticas dos PDFs (com página) confrontadas com a proposta; referências externas separadas por natureza (lei, norma voluntária, boa prática) |
| 5 | [05_plano_de_acao.md](05_plano_de_acao.md) | 34 ações em 4 fases, com responsável por função, prazo, custo relativo e verificação; viabilidade; sequência mínima para orçamento restrito |
| 6 | [06_divisao_atividades.md](06_divisao_atividades.md) | Divisão entre Samir, Giovana e Emanuel; pontos de integração; o que todos devem dominar; cronograma até 11/11 |
| 7 | [07_roteiro_apresentacao.md](07_roteiro_apresentacao.md) | 14 slides (mensagem, conteúdo, evidência, visual, apresentador e tempo); slides de reserva; respostas preparadas para as perguntas prováveis |
| 8 | [08_perguntas_para_delimarket.md](08_perguntas_para_delimarket.md) | Lista curta com 12 perguntas para enviar até terça; lista completa por domínio, com prioridade |

## Como ler as conclusões

- **Avaliamos o desenho, não a implantação.** A proposta é um plano, e nenhum controle foi testado nem nenhum teste foi executado. "Não evidenciado" significa que o controle não aparece na documentação, **não** que ele não existe.
- **O PostgreSQL é uma premissa da própria proposta**, não um fato confirmado. As conclusões sobre credenciais, imutabilidade e testes continuam válidas com qualquer SGBD.
- **Os custos são relativos** (baixo, médio, alto) e os tempos de restauração são ilustrativos, porque a DeliMarket não informou porte, volume nem orçamento.
- **As indicações legais** (LGPD, resoluções da ANPD, CTN) são referências que precisam ser validadas pelo jurídico. Não são parecer.

## Próximos passos da equipe

1. Ler este arquivo e as próprias seções (Entregável 6).
2. Enviar a lista curta de perguntas até terça, 10/11, às 12 h.
3. Fazer os rascunhos dos slides até terça às 18 h e a chamada de integração às 20 h.
4. Ensaiar na manhã de quarta, 11/11.
5. Depois da reunião, atualizar as hipóteses com as respostas recebidas.
