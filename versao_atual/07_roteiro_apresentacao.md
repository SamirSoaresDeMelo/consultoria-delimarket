# Entregável 7 — Roteiro da apresentação da consultoria

**Público:** equipe DeliMarket, que é a demandante, e o professor.
**Duração-alvo:** cerca de 22 minutos, mais perguntas.
**Regra de ouro:** uma mensagem por slide, pouco texto, e a fonte da evidência no rodapé (por exemplo, "Proposta §5.4 · TCU 2012, p. 76").

**Tom:** consultoria parceira. A própria DeliMarket disse na reunião que não tinha visto o conteúdo de segurança. Por isso, a apresentação deve mostrar o que está certo antes do que falta.

| # | Título | Mensagem principal | Conteúdo essencial | Evidência que sustenta | Visualização sugerida | Quem apresenta | Tempo |
|---|---|---|---|---|---|---|---|
| 1 | **DeliMarket: da pane de 2023 à reengenharia** | A reengenharia nasceu de uma falha de disponibilidade, e é preciso garantir que ela também seja segura | Distribuidora nacional de perecíveis; ERP e BI; DC único. 2023: 18 h sem ERP, pedidos perdidos, backup corrompido, BI desatualizado. Resposta: arquitetura híbrida (DC e nuvem *warm*) | Estudo de caso §1–2; Proposta §1 | Linha do tempo do incidente com três ícones de impacto (entregas, dados, decisão) | Giovana | 1 min |
| 2 | **Como auditamos** | É uma auditoria de **desenho** com critérios dos materiais da disciplina, e não uma opinião | Fontes: proposta, 2 diagramas, reunião de 04/11. Critérios: TCU, ISO 27005, 17799, NC-03, Modelo de PSI. 93 controles classificados (A/PA/I/NE/NA). Riscos com P × I (1–5). Limitação: avaliamos um plano, não um ambiente implantado | Entregáveis 1, 2 (seção 2) e 3 | Funil: documentos → controles → riscos → plano | Giovana | 1,5 min |
| 3 | **A arquitetura proposta** | A arquitetura é boa para disponibilidade, mas o desenho tem 3 pontos únicos de falha | DC local com links e firewalls redundantes, ERP N+1, backup 3-2-1, réplica e ERP *warm* na nuvem, camada analítica. Marcados em vermelho: **banco único, balanceador único, gerador único** | Diagrama 1; Proposta §3–4 (contradição no §4.2) | Diagrama reconstruído (relatório, 4.1) com 3 marcadores vermelhos | Emanuel | 1,5 min |
| 4 | **O que a proposta acerta** | Estas decisões estão corretas e **devem ser mantidas** | Replicação ≠ backup; PITR com WAL; 3-2-1, imutável e fita (equivale a 3-2-1-1-0); testes mensais com decifragem; AES-256-GCM e TLS 1.3; BI com carimbo de atualização e reconciliação; consciência de custo | Relatório, seção 5 (P1–P13) | Seis *cards* com ícone de "check" | Emanuel | 1,5 min |
| 5 | **Onde está o risco** | O risco dominante é **intencional e de governança**, não elétrico | 1 crítico, 13 altos, 4 médios. Destaque para R01 (ransomware via endpoint, 20). Top 5: R01, R06, R17, R02, R04. Riscos que a reengenharia **criou**: nuvem, credenciais e chaves (R02, R12, R15) | Entregável 3 | Dois mapas de calor 5×5 lado a lado (atual × projetado) | Giovana | 1,5 min |
| 6 | **Backup e continuidade: forte no físico, frágil contra ataque** | RPO de 5 min e RTO de 1 h são plausíveis para a perda do DC, mas **não têm evidência** e **não valem para ransomware** sem cofre isolado e WAL imutável | Tabela dos 5 cenários com semáforo (A: banco falha · B: corrupção · C: ransomware · D: perda do site · E: nuvem fora). Uma credencial pode apagar réplica, backups e chaves. Orçamento de 60 min por etapa. Restaurar **dentro da nuvem** | Relatório, 7.4–7.6; A07–A08; Proposta §5.4, §6.2 | Tabela de cenários com semáforo e barra horizontal do orçamento de tempo | Emanuel | 2,5 min |
| 7 | **Infraestrutura e redes** | Sem segmentação, um pendrive na recepção pode alcançar o banco e o backup | A segmentação é apenas citada (§7.4); VPN, filiais e Wi-Fi não aparecem. Proposta: zonas (usuários, aplicação, dados, **backup isolado**, gerência, DMZ, visitantes), "negar por padrão", bastion. Zero Trust em 5 passos concretos | A13; Cartilha p. 25–27; TCU p. 29 | Diagrama de zonas antes (rede plana) × depois | Samir | 2 min |
| 8 | **Controle de acesso e auditoria** | Os princípios estão certos, mas falta processo, e **os logs podem ser apagados por quem é auditado** | MFA e contas administrativas separadas. RACI resumida (quem declara desastre, aprova restauração, guarda chaves, administra logs). Arquitetura de logs: coletor segregado, cópia imutável, NTP, dez alertas. Acesso físico: cartão + PIN como base | A10–A12; TCU p. 17, 27, 76; reunião (Emanuel) | Fluxo fontes → coletor → cópia imutável → revisão | Samir | 2 min |
| 9 | **LGPD e proteção dos dados** | Os princípios estão certos, mas falta operação, e a biometria traz **dado sensível** | Mapa de dados e bases legais; encarregado com substituto; tabela de temporalidade (**backup ≠ arquivamento**); classificação em 4 níveis; anonimização × pseudonimização × criptografia; incidente comunicado à ANPD em **3 dias úteis** | A17–A18; reunião ("não foi abordado"); Res. CD/ANPD 15/2024 | Tabela dos 4 níveis e mini tabela anonimização/pseudonimização/criptografia | Giovana | 1,5 min |
| 10 | **O que ficou de fora** | O ataque mais provável não foi tratado | Cadeia do pendrive em 5 passos, com o controle que quebra cada elo (USB → privilégio local → EDR → segmentação → credencial do backup). Sem plano de incidentes, restaurar depressa destrói evidências. ERP e NF-e no DR | A14–A16; POP 5.03; reunião (pendrive, VPN) | Cinco caixas em sequência, cada uma com um "cadeado" | Samir | 2 min |
| 11 | **Recomendações priorizadas** | As correções mais importantes **são baratas** | Top 10 (relatório, seção 11) no quadrante esforço × impacto. Destaques: AC04 cofre e WAL imutável, AC03 MFA, AC05 USB, AC01/AC02 papéis e gatilhos | Entregável 5; ISO 27005, 9.1 | Matriz 2×2 esforço × impacto com as ações posicionadas | Emanuel | 2 min |
| 12 | **Plano de implantação** | Quatro fases, com a BIA **antes** de qualquer investimento grande | 0–1 mês: 10 ações. 1–3 meses: 11. 3–9 meses: 8. 9–18 meses: 5. Sequência mínima viável para orçamento restrito | Entregável 5, seções 2 e 4.1 | *Roadmap* horizontal com marcos de saída | Giovana | 1,5 min |
| 13 | **Benefícios esperados** | Elimina o risco crítico e transforma metas em evidência | De 1/13/4 (crítico/alto/médio) para 0/0/11 médios e 7 baixos. RTO e RPO medidos em teste. Conformidade demonstrável com a LGPD. Indicadores (AC33) | Entregável 3, seção 2 | Antes × depois e 4 indicadores | Giovana | 1 min |
| 14 | **Conclusão** | "A proposta prepara a DeliMarket para o incidente de 2023. Com cerca de 10 medidas de baixo custo, ela passa a se preparar também para o próximo." | Resposta à questão central. Próximos passos: respostas às perguntas P1 e revisão da proposta (AC08) | Relatório, seções 0 e 13 | Frase-síntese e três próximos passos | Samir | 1 min |

**Tempo por pessoa:**

| Integrante | Tempo |
|---|---|
| Giovana | ~8 min (6 slides, sendo 3 curtos) |
| Emanuel | ~7,5 min |
| Samir | ~7 min |
| **Total** | **~22,5 min** |

## Slides de reserva (só para perguntas)

| # | Conteúdo | Fonte |
|---|---|---|
| R1 | Exemplo AES-256-GCM da palavra "sistemas" (chave, *nonce*, resultado, *tag*, teste de adulteração) | Relatório, Apêndice A |
| R2 | Tabela completa dos cenários de recuperação | Relatório, 7.4 |
| R3 | Matriz de riscos detalhada | Entregável 3, seção 3 |
| R4 | Gráfico da classificação dos 93 controles por domínio (adequados concentrados em backup e BI; não evidenciados em endpoint, logs, LGPD e governança) | Relatório, seção 9 |
| R5 | Tempo de restauração por volume e banda | Relatório, 7.6 |
| R6 | O que a replicação propaga e o que não propaga | Relatório, 12.3 |

## Perguntas prováveis da DeliMarket e respostas preparadas

| Pergunta provável | Resposta curta (quem responde) |
|---|---|
| "Nós colocamos MFA e menor privilégio. Por que vocês dizem que falta controle de acesso?" | Os princípios estão lá (Parcialmente adequado). Falta o **processo**: quem concede, revisa e remove o acesso, e onde há conflito de funções (TCU p. 30, item 2.9.3). (Samir) |
| "A replicação contínua não resolve o ransomware?" | Depende do ataque. Comando destrutivo no banco é replicado em segundos. Cifragem de arquivos no sistema operacional não é replicada como tal, mas, com a credencial da nuvem, o atacante apaga a réplica diretamente. O que protege é a **separação de credenciais** (relatório, 12.3). (Emanuel) |
| "Criptografar não deixa o backup muito maior e lento?" | O AES-GCM mantém o tamanho e acrescenta só a *tag* de 16 bytes (Apêndice A). O problema real é **perder a compressão** se a cifragem vier antes. A ordem certa é comprimir e depois cifrar. O tempo deve ser medido nos testes. (Emanuel) |
| "Por que não biometria?" | Pode ser usada, mas é **dado pessoal sensível** (LGPD, art. 5º, II), com custo de conformidade. Cartão + PIN já tem 2 fatores, custa menos e não envolve dado sensível (17799, 9.1.2 b). (Giovana e Samir) |
| "Isso tudo não é caro demais?" | A sequência mínima viável é quase toda de baixo custo: configuração e processo. Investimentos grandes, como alta disponibilidade do banco e SOC, **só depois da BIA** (ISO 27005, 9.1; Modelo de PSI, art. 20). (Giovana) |
| "O RPO de 5 minutos está errado?" | Não está errado. É plausível na perda do DC, mas precisa de **teste** e não vale para ransomware enquanto o WAL não estiver no cofre imutável. (Emanuel) |
| "Se mandarmos os logs para outro servidor, resolve?" | Só se esse servidor tiver administração separada, cópia imutável, relógio sincronizado e alertas. Sem isso, o problema apenas muda de lugar (relatório, 8.2). (Samir) |
| "Zero Trust não é caro?" | Os primeiros passos (MFA, contas separadas, segmentação, bastion) são baratos. NAC e ZTNA ficam para o longo prazo (relatório, 12.4). (Samir) |
| "O gerador único é um erro?" | Não. É uma decisão legítima de custo, mas precisa ser **aceita formalmente** pela diretoria e acompanhada de compensações: teste sob carga, contrato de combustível, ponto para gerador móvel e desligamento ordenado. (Emanuel) |
| "O que é BIA?" | É a análise de impacto no negócio: quanto custa cada hora parada de cada processo. É dela que saem RPO, RTO e orçamento (TCU p. 33, item 3.4). (Giovana) |
