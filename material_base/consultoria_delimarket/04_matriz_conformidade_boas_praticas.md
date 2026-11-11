# Entregável 4 — Matriz de conformidade com boas práticas

Esta matriz confronta, uma a uma, as **boas práticas dos materiais da pasta** com a proposta da DeliMarket. Ela só atribui a cada documento o que **ele de fato diz**, com a página ou seção. O que vem de fora da pasta está separado na seção 3.

**Avaliação:**
- A = Adequado
- PA = Parcialmente adequado
- I = Inadequado
- NE = Não evidenciado
- NA = Não aplicável

**Códigos de referência:**
- **TCU** = *Boas Práticas em SI*, 4ª ed., 2012. A página citada é a impressa no rodapé.
- **17799** = ABNT NBR ISO/IEC 17799:2005 (arquivo `nbr_iso_27002 segurança fisica.pdf`).
- **NC-03** = Norma Complementar 03, IF Sertão-PE.
- **PSI-M** = Modelo de PSI do PPSI 2.0.
- **27005** = apostila da ISO/IEC 27005:2011.
- **SGSI** = slides `9d205…pdf`.
- **Cart.** = cartilha da turma.
- **CAF** = `Controle_de_Acesso_Fisico.pptx`.
- **AR** = `ativos riscos.pptx`.
- **CONT** = `conteudo.pptx`.
- **321** = `3-2-1-1-0.txt`.
- **POP** = POP Senasp 2024, vol. 5.
- **AFR** = `analise_forense_ram1.txt`.

Os códigos AC indicam as ações do Entregável 5.

## 1. Natureza das referências da pasta

O TCU, a NC-03 e o PSI-M são documentos **do setor público**. Para órgãos públicos, eles são obrigatórios. Para a DeliMarket, uma empresa privada, valem **como critério de boa prática**. A ISO 17799/27002 e a 27005 são normas **voluntárias**. As obrigações legais da DeliMarket aparecem na seção 3.

## 2. Matriz

### 2.1 Governança, PSI e gestão de riscos

| # | Boa prática | Referência | Situação na proposta | Aval. | Risco ou lacuna | Recomendação |
|---|---|---|---|---|---|---|
| 1 | PSI formal, aprovada pela direção, divulgada e composta por políticas interligadas (backup, senhas, controle de acesso etc.) | TCU p. 10–12 (1.3–1.6); PSI-M arts. 1º–2º, 17, 27; Clavis, passos 1–8 | Não existe. O §10 cita a ISO 27001 apenas como referência futura | NE | Sem base normativa interna para exigir controles nem aplicar sanções | AC01 (PSI enxuta adaptada do PSI-M) e AC30 |
| 2 | Responsabilidades definidas, gestor de SI, comitê e independência da SI em relação à TI | TCU p. 10 (1.4), p. 48–51 (itens 6.1.2 e 6.1.3 nos acórdãos); PSI-M arts. 18 e 21; CONT slide 12 (mostra os dois organogramas, SI independente × SI subordinada à TI; a preferência pela independência é inferência da consultoria) | Apenas "equipe" e "responsáveis" genéricos | NE | PCN impossível de executar; segregação de funções impossível de verificar (A02) | AC01 (RACI do relatório, seção 6.1) |
| 3 | Classificação da informação conforme importância, criticidade e sensibilidade, com tratamento diferenciado | TCU p. 11 (1.5), p. 62 (item 7.2 no acórdão); CONT slides 14–20 (níveis 1–5, dano potencial e tabela de proteção); Clavis, passo 3 | Diretriz no §9.3 ("deverão ter classificação"), sem esquema | PA | Retenção, criptografia e acesso não podem ser diferenciados (A18) | AC18 (esquema de 4 níveis) |
| 4 | Inventário de ativos com proprietário definido | TCU p. 58–60 (7.1.1/7.1.2); 27005, 8.2.2 | Inexistente | NE | Sem inventário não há como proteger, aplicar patches nem fazer backup de tudo (A21) | AC11 |
| 5 | Gestão de riscos: critérios, ameaças acidentais **e intencionais**, P × I, aceitação | 27005, 7.2.2–7.2.4, 8.2.3, 8.3, 9.3; SGSI ("Risco = Impacto × Probabilidade"); AR slides 10–20 | §2 com 4 categorias de disponibilidade; sem P × I e sem aceitação | PA | Ameaças intencionais ficaram fora (A01) | AC12, AC30 |
| 6 | Considerar também os controles **planejados** e verificar se funcionam; prever a falha do próprio controle | 27005, 8.2.4 | Controles planejados sem critérios de verificação | PA | Controles podem existir apenas no papel | Coluna "Como verificar" do Entregável 5 |
| 7 | Investimento proporcional ao risco; priorizar o que reduz muito o risco com pouco custo; tratar riscos improváveis porém graves | PSI-M arts. 12 e 20; 27005, 9.1 | Há consciência de custo (*warm standby*, gerador único), mas sem análise formal | PA | Decisões de custo sem BIA | AC12; sequenciamento do Entregável 5 |
| 8 | Exceções e riscos aceitos documentados, com justificativa e prazo | PSI-M art. 30; 27005, 9.3 | Gerador único justificado em texto, sem aceitação formal | PA | Risco retido sem dono | AC01 (aceitação formal pela diretoria) |
| 9 | Segurança nos contratos com terceiros; dados custodiados por terceiros cobertos em contrato | NC-03 art. 5º (p. 4); PSI-M art. 19; TCU p. 53–55, 71 | Provedor de nuvem "será contratado", sem requisitos | NE | SLA, saída, região e responsabilidade compartilhada indefinidos (A20) | AC21 |
| 10 | Gestão de mudanças, inclusive correções de vulnerabilidade como mudança | TCU p. 93–95 (12.5.1, 12.6.1); PSI-M art. 17, V | "Documentado, parcialmente automatizado" (§6.2) | NE | Divergência entre produção e contingência faz o *failover* falhar | AC11, AC23 |
| 11 | Conscientização e treinamento (inclusive sobre o PCN, com simulações) | TCU p. 35 (3.7.1), p. 98 (14.1.4 g); PSI-M art. 17, II–III | Ausente | NE | Usuários são o vetor do pendrive e do phishing (A14) | AC19, AC10 |
| 12 | Segregação de funções | TCU p. 18 (2.4), p. 30–31 (2.10), p. 69 (10.1.3); PSI-M art. 16; 17799, 10.1 (objetivo, p. 9 do PDF) | Ausente | NE | Fraude e abuso de privilégio (A11) | AC01, AC16 |

### 2.2 Controle de acesso lógico e identidade

| # | Boa prática | Referência | Situação na proposta | Aval. | Risco ou lacuna | Recomendação |
|---|---|---|---|---|---|---|
| 13 | ID único para todos, **inclusive administradores** | TCU p. 19 (2.5.2), p. 85 (item 9.2.25 no acórdão) | Não tratado (contas compartilhadas e de serviço não mencionadas) | NE | Ações sem responsável identificável | AC03 |
| 14 | Processo formal de concessão e revogação, com remoção imediata na mudança de função ou saída e revisão periódica | TCU p. 22 (2.5.8), p. 30 (2.9.3), p. 84–85 (11.2.4) | Apenas o princípio do menor privilégio | NE | Contas órfãs e acúmulo de privilégios | AC16 |
| 15 | Política de controle de acesso com "tudo proibido a menos que expressamente permitido" | TCU p. 29 (2.9.1–2.9.2); PSI-M art. 16 | Menor privilégio citado (§5.6, §7.4, §9.2) | PA | Princípio sem regra operacional | AC13, AC16 |
| 16 | Recursos críticos monitorados e acessíveis a poucas pessoas | TCU p. 18 (2.4) | MFA "para operações administrativas" | PA | Escopo do MFA indefinido | AC03, AC26 |
| 17 | Autenticação forte (tokens, cartões inteligentes, biometria) com tolerância a erro calibrada | TCU p. 23–25 (2.5.9–2.5.13); Cart. p. 14–17 | MFA administrativo; biometria só física | PA | Usuários do ERP e acesso remoto sem MFA definido | AC03, AC27 |
| 18 | Logon seguro: limite de tentativas, registro das tentativas inválidas, informação do último acesso | TCU p. 19 (2.5.1) | Ausente | NE | Força bruta sem detecção | AC27, AC15 |
| 19 | *Time-out* de sessão e limite de sessões concorrentes | TCU p. 28–29 (2.8); Cart. p. 22 | Ausente | NE | Sessão abandonada usada por outra pessoa | AC27 |
| 20 | Política de senhas | TCU p. 20–22 (**parcialmente desatualizada**: troca a cada 60/90 dias e 6–8 caracteres); Cart. p. 9 ("tamanho vence complexidade") | Ausente | NE | – | AC16 (política atual, ver referência externa NIST SP 800-63B) |

### 2.3 Logs, monitoramento e auditoria

| # | Boa prática | Referência | Situação na proposta | Aval. | Risco ou lacuna | Recomendação |
|---|---|---|---|---|---|---|
| 21 | Logs protegidos contra alteração e destruição, **em especial por administradores** | TCU p. 17 (2.3), p. 27 (2.7.1), p. 76 (Acórdão 1137/2012, item 1.4.2.4, 10.10.3); Cart. p. 3 | "Registrados e auditados" (§7.1, §5.6) | NE | Invasor apaga o próprio rastro (preocupação levantada na reunião) | AC15 (relatório, seção 8) |
| 22 | O que registrar: usuário, logon e logoff, estação, tentativas aceitas e rejeitadas; revisão periódica ("nada adianta ter um log se ele não é periodicamente revisado") | TCU p. 28 (2.7.2) | Genérico | PA | Sem lista de eventos e sem rotina de revisão | AC15 (eventos e alertas da seção 8.3) |
| 23 | Monitorar as atividades de administradores e o acesso direto ao banco | TCU p. 76 (1.4.2.6, 10.10.4), p. 77 (9.1.38), p. 78 (9.2.24) | Ausente | NE | DBA ou administrador sem rastreabilidade | AC26 |
| 24 | Trilhas de auditoria com data e hora, dado alterado e responsável | TCU p. 78 (9.2.28–9.2.29) | Ausente | NE | Fraude em estoque, preço ou crédito sem rastro | AC27 |
| 25 | Procedimentos operacionais documentados, inclusive backup, reinício e recuperação e gestão de trilhas de auditoria | 17799, 10.1.1 b, g, h (p. 9 do PDF) | Backup e recuperação descritos; trilhas não | PA | – | AC15, AC20 |
| 26 | Monitoramento proativo do uso da infraestrutura | TCU p. 76 (1.4.2.5, 10.10.2) | Monitoramento de rede, replicação e ambiente | PA | Monitoramento pode estar dentro do DC que cai | AC23 (monitoramento externo) |

### 2.4 Segurança física e ambiental

| # | Boa prática | Referência | Situação na proposta | Aval. | Risco ou lacuna | Recomendação |
|---|---|---|---|---|---|---|
| 27 | Controles de entrada com **cartão mais PIN** e registro seguro de todos os acessos para auditoria; revisão periódica dos direitos | 17799, 9.1.2 b e e (p. 2 do PDF); CAF slide 6 (registrar também os eventos negados; bloqueio de dupla entrada) | "RFID **ou** biometria"; acessos "registrados" (§7.1) | PA | Fator único; log local adulterável | AC24 |
| 28 | Defesa em profundidade e zoneamento (perímetro, área, edificação, ativo) | CAF slides 7–8, 10–11; 17799, 9.1.1 | Só a sala de servidores é tratada | PA | Sem camadas | AC24 |
| 29 | Evitar trabalho não supervisionado em áreas seguras | 17799, 9.1.5 b (p. 3 do PDF) | Ausente | NE | Ações de terceiros sem testemunha | AC24 (acompanhamento de terceiros) |
| 30 | Recursos redundantes **não** sujeitos aos mesmos riscos físicos; equipamentos de contingência e mídia de backup a distância segura | TCU p. 66 (Acórdão 1722/2008, 9.1.1.17); 17799, 9.1.4 b (p. 3 do PDF) | Nuvem e fita fora do local atendem; servidores N+1 e backup local no mesmo site (local do backup não informado) | PA | Incêndio destrói a produção e a cópia 2 (A05) | AC24 |
| 31 | Detecção e combate a incêndio | 17799, 9.1.4 c | §7.3 | A | – | – |
| 32 | Monitorar temperatura e umidade; proteção contra raios | 17799, 9.2.1 f e g (p. 5 do PDF) | Sensores e alertas (§7.2); raios não citados | A | Confirmar o SPDA (para-raios) | AC24 |
| 33 | UPS, **plano para falha do UPS**, gerador verificado e testado, combustível, múltiplas fontes | 17799, 9.2.2 (p. 5–6 do PDF); TCU p. 68 | UPS N+1, ATS, combustível e manutenção mencionados (§4.4); gerador único; sem testes sob carga | PA | Falha do gerador não detectada até a emergência | AC24 (testes, conexão para gerador móvel, desligamento ordenado) |
| 34 | Telecomunicações com pelo menos duas linhas e rotas alternativas | 17799, 9.2.2 (p. 6 do PDF), 9.2.3 f) 2) | Dois provedores, "preferencialmente" com rotas distintas (§4.1) | A | Comprovar em contrato | AC21 |
| 35 | Manutenção com registros, feita por pessoal autorizado | 17799, 9.2.4 (p. 7 do PDF) | Manutenção preventiva do gerador citada | PA | – | AC24 |
| 36 | Descarte seguro de mídias (destruir ou sobrescrever, não apenas formatar) e registro do descarte | 17799, 9.2.6 (p. 8 do PDF); NC-03 art. 26 | Apagamento criptográfico e destruição física (§9.5) | A | Registrar o descarte | AC20 |
| 37 | Fitas guardadas em ambiente de acesso restrito | TCU p. 68 e 75 (Acórdão 782/2004, 9.3.7); NC-03 art. 23 | "Local físico seguro, diferente do DC" (§5.4) | PA | Transporte, inventário e leitura periódica não definidos | AC20 |

### 2.5 Backup e restauração

| # | Boa prática | Referência | Situação na proposta | Aval. | Risco ou lacuna | Recomendação |
|---|---|---|---|---|---|---|
| 38 | Política de backup formal, alinhada ao negócio e ao PCN, que inclua o código-fonte e a base | TCU p. 73–74 (10.5.1); NC-03 arts. 1º, 14–15 | Política técnica sólida (§5) | PA | Escopo limitado ao ERP e ao BI (A21) | AC07, AC20 |
| 39 | Plano por sistema: escopo, tipo, frequência, retenção, mídia, **janela**, estratégia, **responsáveis**, testes, procedimento de restauração e **assinaturas** | NC-03 art. 16 e anexo (p. 5–6, 10–11) | Tipo, frequência, retenção e mídia definidos; janela, responsáveis e assinaturas, não | PA | Plano inexequível sem responsáveis | AC20 |
| 40 | Sistemas críticos especificados formalmente; requisitos diferenciados por criticidade | NC-03 arts. 7º–8º, 13 | RPO/RTO por sistema (§6.3), sem BIA | PA | Metas "gerais" | AC12 |
| 41 | Retenção mínima por frequência (sistemas críticos: anual com 5 anos) | NC-03 art. 11 (p. 4–5) | Máximo de 12 meses (cópia mensal) | PA | Retenção legal confundida com backup | AC18, AC20 (arquivamento separado do backup) |
| 42 | Escolher a mídia pela criticidade, retenção, probabilidade de restauração, **tempo esperado de restauração**, custo e vida útil | NC-03 art. 20 | Mídias definidas; tempo para restaurar a partir da nuvem não avaliado | PA | RTO de ransomware estoura com o *download* (relatório, seção 7.6) | AC23 (restaurar dentro da nuvem) |
| 43 | Compressão permitida desde que não atrase significativamente a restauração | NC-03 art. 22 | Não tratada | NE | Cifrar antes de comprimir anula a compressão | AC20 (comprimir e depois cifrar) |
| 44 | Testes de restauração iniciais e periódicos (máximo de 1 ano), **em servidor diferente da produção** | NC-03 arts. 24–25 | Mensais e trimestrais, com decifragem (§5.6, §11.3) | A | Definir ambiente isolado e critérios | AC20 |
| 45 | Cópia em local diverso da operação | TCU p. 74 (Acórdão 71/2007, 9.2.16, 10.5.1 d) | Nuvem e fita fora do local | A | – | – |
| 46 | Criptografia das mídias de backup | TCU p. 73 (1.4.2.9, 10.5.1 h) | AES-256-GCM (§11.2) | A | Gestão de chaves (item 60) | – |
| 47 | Cópia imutável ou *air-gapped* ("+1") e **0 erros** com resultados documentados | 321 (seção "A Evolução: Regra 3-2-1-1-0") | Imutável 90 dias e fita *offline* (§5.4) | A | Modo da imutabilidade e WAL fora dela (A08) | AC04 |
| 48 | Credenciais de backup independentes das de produção, com MFA | 321 (checklist; "erros comuns", item 5) | Só o servidor local tem "controle de acesso independente" | PA | Uma credencial pode apagar a nuvem (A07) | AC04 |
| 49 | Gerenciar os logs do backup, verificar os eventos e reportar falhas imediatamente | NC-03 art. 27, VIII, X, XI | Alertas e "auditoria e registro" (§5.6) | A | Depende de alguém responsável (item 2) | AC01 |
| 50 | Eixos de avaliação de backup: política, planos e procedimentos, testes, segurança física, segurança lógica, recursos | TCU (`psi1.txt`, QACIs de backup) | Fortes: testes, segurança física das cópias. Fracos: política (governança), segurança lógica, recursos (papéis) | PA | – | AC20 |

### 2.6 Continuidade de negócios

| # | Boa prática | Referência | Situação na proposta | Aval. | Risco ou lacuna | Recomendação |
|---|---|---|---|---|---|---|
| 51 | Antes do PCN: riscos, consequências da interrupção **de cada sistema**, priorização, tempo limite e alternativas com custo e benefício (BIA) | TCU p. 33 (3.4) | Metas sem análise de impacto | NE | Metas sem fundamento (A03) | AC12 |
| 52 | Conteúdo do PCN: condições de ativação, responsáveis e suplentes, contatos, instalação reserva especificada, prioridade das aplicações, dependências externas, contratos | TCU p. 34 (3.5) | Instalação reserva (nuvem) e prioridade (tabela §6.3); demais itens ausentes | PA | Sem decisor nem gatilho (A06) | AC01, AC02, AC21 |
| 53 | Responsabilidade e orçamento da alta administração | TCU p. 34–35 (3.6) | Ausente | NE | Plano sem patrocinador | AC01 |
| 54 | Testes integrais, parciais e por simulação; treinamento; manutenção via gestão de mudanças | TCU p. 35–37 (3.7); p. 98 (14.1.4 g, 14.1.5) | Testes técnicos previstos (§6.5); simulações e treinamento, não | PA | Pessoas não sabem o que fazer na crise | AC10, AC23 |
| 55 | Serviços temporários ou com restrições para as necessidades mais críticas | TCU p. 33 (3.3) | Ausente | NE | Operação parada mesmo com o DR funcionando (A23) | AC02, AC10 |
| 56 | Categorizar eventos (problema, incidente, crise) e definir **gatilhos de acionamento** por dimensão de impacto | AR slides 6–7 (FEBRABAN) | Ausente | NE | Acionamento tardio ou desnecessário | AC02 |

### 2.7 Redes, endpoints e criptografia

| # | Boa prática | Referência | Situação na proposta | Aval. | Risco ou lacuna | Recomendação |
|---|---|---|---|---|---|---|
| 57 | Segregação de redes (operacional, servidores, visitantes) para conter movimento lateral | Cart. p. 25–27 | "Segmentação de redes" citada (§7.4) | PA | Rede plana (hipótese) (A13) | AC13 |
| 58 | Criptografia do tráfego de rede | TCU p. 76 e 92 (10.9.2 c, 11.5.1 i, 12.3.1) | TLS 1.3 entre o DC e a nuvem e entre sistemas (§11.2) | A | VPN para filiais e acesso remoto não evidenciados | AC13, AC21 |
| 59 | VPN corporativa fora da rede da empresa; não desativar antivírus, VPN nem criptografia de disco | Cart. p. 31, 35–37 | Ausente | NE | Acesso remoto inseguro | AC13, AC28 |
| 60 | Gestão de chaves: armazenar, rotacionar e revogar em KMS; nunca deixar *hardcoded* | Cart. p. 43 | KMS e chaves separadas (§11.3); rotação "controlar"; revogação e custódia, não | PA | Dependência circular e perda de chaves (A09) | AC06, AC21 |
| 61 | Algoritmos atuais (AES-256; RSA 2048+ ou ECC) e modelo híbrido (assimétrico para trocar chaves, simétrico para os dados) | Cart. p. 41–43 | AES-256-GCM, certificados e TLS (§11.1–11.2) | A | – | – |
| 62 | Hash para verificar a integridade (por exemplo, SHA-256) | Cart. p. 38–40; POP 5.03 (4.4–4.6) | "Verificação automática da integridade" dos backups (§5.6) | A | Detalhar o algoritmo e o manifesto | AC20 |
| 63 | Defesas contra códigos maliciosos | PSI-M art. 17, VIII; TCU p. 11 (1.5) | Ausente | NE | Pendrive e ransomware (A14) | AC05, AC14 |
| 64 | Gestão de vulnerabilidades técnicas | PSI-M art. 17, VII; TCU p. 95 (12.6.1); `psi1.txt` | "Atualizações de segurança" (§7.4) | PA | Sem processo nem prazo | AC14 |
| 65 | Defesas contra phishing (conscientização mais controles administrativos e técnicos); autenticação de e-mail | `psi1.txt` (4 QACIs de phishing); POP 5.04 (descreve SPF e DKIM como mecanismos de autenticação de e-mail; não é uma recomendação do POP) | Ausente | NE | Phishing como vetor inicial | AC19 |
| 66 | Proteção do diretório de identidades (AD), incluindo contingência e recuperação | `psi1.txt` (QACIs de AD; "AD: por onde o ransomware acontece") | Ausente | NE | Sem identidade, não há DR (A06, A21) | AC07, AC23 |

### 2.8 Resposta a incidentes e evidências

| # | Boa prática | Referência | Situação na proposta | Aval. | Risco ou lacuna | Recomendação |
|---|---|---|---|---|---|---|
| 67 | Gestão de incidentes: notificação, responsabilidades, procedimentos, coleta de evidências, equipe designada (ETIR) | TCU p. 95–97 (13.x); PSI-M art. 17, X, art. 21, I, b, art. 26; `psi1.txt` (7 QACIs) | Ausente | NE | A16 | AC17 |
| 68 | Preservar evidências: isolamento físico e digital, ordem de volatilidade, *dump* de memória quando pertinente, hashes, cadeia de custódia; em servidores, pedir apoio ao responsável de TI que não seja suspeito | POP 5.03 (4.2–4.5); CPP ppt (cópia bit a bit, hash); AFR (RFC 3227) | Ausente | NE | Restaurar destrói as evidências | AC17 |
| 69 | Fases da forense em incidentes: coletar, examinar, analisar, reportar | AFR (NIST SP 800-86) | Ausente | NE | – | AC17 |

### 2.9 Dados, BI e privacidade

| # | Boa prática | Referência | Situação na proposta | Aval. | Risco ou lacuna | Recomendação |
|---|---|---|---|---|---|---|
| 70 | Consistência entre bases, verificada periodicamente | TCU p. 92 (12.2.2) | Reconciliação ERP–BI e validação (§8) | A | Regras concretas a definir | AC25 |
| 71 | Evitar alterações diretas na base de produção; procedimentos formais para utilitários | TCU p. 70 (9.4.9), p. 89 (11.5.4) | Ausente | NE | Alteração sem rastro | AC26 |
| 72 | Ciclo de vida da informação (origem, aproveitamento, destinação) | AR slide 9 | §9 (coleta, uso, armazenamento, arquivamento, descarte) | A | – | – |
| 73 | Segurança integrada à proteção de dados pessoais; encarregado no comitê de SI | PSI-M art. 13, art. 18, II, art. 21, II | §10 (diretrizes) | PA | Encarregado não nomeado (A17) | AC18 |
| 74 | Ativos de informação incluem pessoas, software, físico, serviços e intangíveis; vulnerabilidades humanas e de comunicação | CONT (Lyra; Sêmola); AR slides 10–12 | Foco em hardware e energia | PA | Pessoas e comunicação esquecidas | AC11, AC19 |

**Contagem (74 práticas):**

| Avaliação | Quantidade |
|---|---|
| A | 14 |
| PA | 31 |
| I | 0 |
| NE | 29 |

> A matriz de controles do relatório (seção 9) tem um item "Inadequado" (BI07), que é uma inconsistência **interna** da proposta e não uma boa prática dos PDFs.

## 3. Referências externas (fora da pasta), separadas por natureza

| Natureza | Referência | Aplicação à DeliMarket | Obrigatória? |
|---|---|---|---|
| **Lei** | LGPD (Lei 13.709/2018): arts. 5º (definições, dado sensível, anonimização), 6º (princípios), 7º e 11 (bases legais), 12 (dado anonimizado), 18 (direitos), 33–36 (transferência internacional), 37 (registro), 38 (relatório de impacto), 39 (operador), 41 (encarregado), 46 (segurança), 48 (incidentes), 52 (sanções) | Dados de clientes, funcionários e biometria; nuvem como operador; notificação de incidentes | **Sim** (empresa privada que trata dados pessoais). Validar com o jurídico |
| **Regulamento** | Res. CD/ANPD nº 15/2024 (incidentes: **3 dias úteis** a partir do conhecimento de que dados pessoais foram afetados); nº 18/2024 (encarregado: ato formal e substituto); nº 19/2024 (transferência internacional e cláusulas-padrão); nº 2/2022 (agentes de pequeno porte) | Plano de incidentes (AC17); nomeação do encarregado (AC18); região da nuvem (AC21) | **Sim**, nas hipóteses de cada uma |
| **Lei** | CTN, art. 195, parágrafo único (livros e comprovantes guardados até a prescrição) e legislação fiscal estadual da NF-e | Retenção de documentos fiscais (em geral ≥ 5 anos); tabela de temporalidade | **Sim**. Validar prazos com a contabilidade |
| **Lei** (condicional) | Marco Civil da Internet (Lei 12.965/2014), art. 15 (guarda de registros de acesso a aplicações por 6 meses) | Só se a DeliMarket for provedora de aplicação de internet (por exemplo, portal B2B de pedidos) | Verificar a aplicabilidade |
| **Lei** (contexto penal) | CPP, arts. 158-A a 158-F (cadeia de custódia, incluídos pela Lei 13.964/2019) | Preservação de evidências se o incidente for levado à polícia ou à Justiça | Quando houver persecução penal |
| Norma voluntária | ISO/IEC 27001:2022 e 27002:2022 (controles de backup, *logging*, sincronização de relógio, criptografia, *malware*, vulnerabilidades, segregação de redes, serviços em nuvem, continuidade de TIC) | Estrutura do SGSI (AC30); versão atual da 17799/27002:2005 da pasta | Não (salvo exigência contratual de clientes) |
| Norma voluntária | ISO 22301:2019 (gestão de continuidade de negócios) | PCN além do DRP (A23) | Não |
| Boa prática | NIST CSF 2.0; SP 800-34 Rev. 1 (contingência, BIA); SP 800-61 Rev. 3 (resposta a incidentes, abril de 2025); SP 800-63B (autenticação e senhas); SP 800-207 (Zero Trust); SP 800-88 (sanitização de mídias) | Fundamentação técnica complementar | Não |
| Boa prática | CIS Critical Security Controls v8.1, grupo IG1 ("higiene cibernética essencial") | Linha de base **proporcional** para uma empresa sem equipe de segurança grande | Não |
| Documentação técnica | PostgreSQL: arquivamento contínuo e PITR, replicação por *streaming*, `max_slot_wal_keep_size`, `recovery_min_apply_delay`, `pg_rewind`, *checksums*, backup incremental (17+) | Validação da estratégia do §5 (se o SGBD for confirmado) | – |
