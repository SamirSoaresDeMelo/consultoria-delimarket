# Entregável 1 — Inventário documental

**Consultoria:** Samir, Giovana e Emanuel · **Cliente (equipe demandante):** DeliMarket · **Data:** 09/11/2026 · **Versão:** 1.0

## 1. Procedimento de leitura

- Diretório percorrido recursivamente: `seguranca_e_auditoria/` e `trabalho_a_ser_analisado/`. Foram encontrados **23 arquivos** (18 na raiz e 5 na subpasta).
- PDFs com texto foram extraídos com `pdftotext` e lidos integralmente, preservando a numeração de páginas para as citações.
- O PDF `nbr_iso_27002 segurança fisica.pdf` não tem camada de texto (é escaneado). As 9 páginas foram renderizadas e lidas como imagem.
- Os arquivos `.pptx` foram lidos pelo XML interno. Os slides formados apenas por imagem foram examinados visualmente.
- O arquivo `.ppt` (formato antigo) foi convertido para PDF com o LibreOffice e depois lido.
- O `.docx` do estudo de caso foi lido pelo XML. O arquivo `comments.xml` está vazio, ou seja, o documento não tem comentários.
- Os dois diagramas da DeliMarket existem **apenas como capturas JPEG enviadas pelo WhatsApp**. O PDF da proposta não tem nenhuma imagem incorporada (verificado com `pdfimages`).
- Nenhum arquivo estava corrompido ou ilegível. As limitações de qualidade estão anotadas na coluna de relevância de cada item.
- O arquivo `cartilha-seguranca-editavel - Google Slides.pdf` não aparecia na primeira listagem. Pelo metadado, ele foi criado em 09/11/2026 às 17:30, o que indica que foi adicionado durante esta análise.

## 2. Inventário

| # | Documento | Tipo | Conteúdo identificado | Relevância para a auditoria |
|---|---|---|---|---|
| 1 | `trabalho_a_ser_analisado/Trabalho DeliMarket (3).pdf` | **Proposta da DeliMarket** (PDF, 22 p. mais 1 em branco, sem figuras) | PCN/DRP; arquitetura híbrida (DC local e nuvem *warm standby*); redundância (rede, servidores N+1, energia, dados); política de backup (full, incremental, WAL/PITR, 3-2-1, imutável, fita); RPO/RTO; segurança física; integração ERP–BI; ciclo de vida dos dados; LGPD; criptografia (AES-256-GCM, TLS 1.3, KMS) | **Objeto principal.** O sufixo "(3)" sugere terceira versão ou cópia baixada. Não há outras versões na pasta para comparar, então é preciso confirmar que esta é a versão final (pergunta G-01). |
| 2 | `trabalho_a_ser_analisado/WhatsApp Image 2026-11-04 at 6.16.58 PM.jpeg` | **Diagrama de arquitetura** (captura de tela) | Operação → "Internet A e B + firewalls redundantes" → **um** balanceador → servidor ERP principal e reserva (falha do principal) → **um** "Banco transacional principal". "UPS + **gerador único**". Bloco "Backup 3-2-1": full semanal, incremental diário e WAL contínuo → cópia 2 (backup local dedicado) → fita offline semanal; cópia 3 (nuvem imutável). Bloco "Nuvem – contingência": réplica → ERP de contingência → failover/failback. Bloco "Camada analítica": ETL e validação → banco analítico → dashboards | **Alta.** Mostra pontos únicos de falha (BD, balanceador, gerador) e contradiz o texto em alguns pontos. Na reunião, Emanuel pediu os arquivos-fonte por causa da perda de qualidade. |
| 3 | `trabalho_a_ser_analisado/WhatsApp Image 2026-11-04 at 6.17.07 PM.jpeg` | **Diagrama de controles** (captura de tela) | Três colunas. Físico: RFID/biometria → sensores → detecção e combate a incêndio. Lógico: MFA → menor privilégio e segmentação → monitoramento e auditoria. Criptografia: AES-256 nos backups → TLS → chaves em KMS | **Média.** Confirma o conjunto de controles previstos. As setas não representam um fluxo real. Não há nada sobre endpoints, rede, incidentes ou LGPD. |
| 4 | `trabalho_a_ser_analisado/Estudo_de_Caso_DeliMarket (1).docx` | **Enunciado** recebido pela DeliMarket | Contexto: distribuição nacional de perecíveis; ERP e BI; DC único. Incidente de 2023: pane elétrica, 18 h sem ERP, backup corrompido, BI desatualizado. Três frentes de trabalho e os entregáveis exigidos, entre eles o **exemplo de criptografia da palavra "sistemas"**, um cronograma e a justificativa de custos | **Alta.** É a régua de completude da proposta. Permite apontar itens exigidos que não foram entregues (ver achado A22 no relatório). |
| 5 | `trabalho_a_ser_analisado/intrucao_do_prof_para_o_trabalho.txt` | **Instruções do professor** | Procedimento da consultoria: contato, recebimento da documentação, análise, adequações conforme o negócio, proposta de melhoria | Define o escopo da nossa entrega. |
| 6 | `reuniao_4_11.txt` | **Transcrição** (Tactiq) da reunião de 04/11/2026 | Lacunas admitidas pela DeliMarket: papéis e local do backup, testes, criptografia ("não sei como funciona muito certo"), categorias e retenção de dados ("não foi abordado"), logs da biometria, pendrive, VPN, Zero Trust ("nem passou na minha cabeça"). Próxima reunião marcada para 11/11 às 15 h | **Alta.** É evidência de que essas lacunas são de **desenho**, e não apenas de redação. Por ser transcrição automática, tem erros ("RP" = ERP, "biai" = BI, "posticlei" = PostgreSQL). |
| 7 | `cartilhas boas práticas segurança TCU.PDF` | **Referência:** TCU, *Boas Práticas em Segurança da Informação*, 4ª ed., 2012 (103 p.) | Cap. 1 PSI; cap. 2 controle de acesso lógico (inclui logs); cap. 3 PCN; cap. 4 acórdãos do TCU organizados pelas seções da NBR ISO/IEC 27002:2005 | **Critério principal** para controle de acesso, logs, PCN, backup e segurança física. Limitações: publicação de 2012, voltada à administração pública federal, com numeração da 27002:2005 (já superada) e orientações de senha desatualizadas (troca a cada 60/90 dias, 6 a 8 caracteres). |
| 8 | `Apostila-GESTÃO_DE_RISCOS ISO 27005.pdf` | **Referência:** apostila da ABNT NBR ISO/IEC 27005:2011 (81 p.) | Contexto e critérios (7.2), identificação de ativos, ameaças, controles e vulnerabilidades (8.2), análise qualitativa e quantitativa (8.3), avaliação (8.4), tratamento (9), anexos C, D e E | **Base metodológica** da matriz de riscos. Limitações: edição de 2011 (existe a ISO/IEC 27005:2022) e o próprio documento informa que "não tem valor normativo". |
| 9 | `9d205b4bf79293a1045c936e3989174922022356281.pdf` | **Slides de aula** "Segurança e Auditoria de SI – SGSI" (38 p.) | PDCA, escopo do SGSI, escala de relevância de 1 a 5, Impacto e Probabilidade, **Risco = Impacto × Probabilidade**, tratamentos (evitar, transferir, reduzir, aceitar), Declaração de Aplicabilidade (base: Campos, 2016) | Fundamenta a escala de 1 a 5 e a fórmula de risco. O metadado de título ("NBR ISO IEC 27005 Tradução") não corresponde ao conteúdo, então o nome do arquivo engana. |
| 10 | `nbr_iso_27002 segurança fisica.pdf` | **Referência escaneada** (9 p.): trecho da **ABNT NBR ISO/IEC 17799:2005**, seções 9 e 10.1.1 | Perímetro e controles de entrada; ameaças externas (9.1.4 b: mídia de backup "a uma distância segura"); utilidades (9.2.2: UPS, gerador testado, combustível); cabeamento; manutenção; descarte seguro; procedimentos operacionais | Critério para segurança física, energia e backup *offsite*. O nome diz 27002, mas o cabeçalho é 17799:2005 (o mesmo conteúdo foi renumerado como 27002 em 2007). A versão vigente é a 27002:2022. |
| 11 | `Norma-Complementar-03-Politica-de-Backup-e-Restauracao-de-Dados.pdf` | **Referência:** NC-03 do IF Sertão-PE (2021, 11 p.) | Conceitos; diretrizes; frequências e retenções mínimas (diária 1 semana, semanal 1 mês, mensal 1 ano, anual 5 anos); conteúdo obrigatório do plano de backup (art. 16); testes (arts. 24–25); responsabilidades (art. 27); modelo de plano no anexo | **Critério principal para a governança da política de backup.** Limitação: norma de órgão público de ensino, usada como referência e não como obrigação. |
| 12 | `modelo-de-politica-de-seguranca-da-informacao-v1-0-14mai2026.pdf` | **Referência:** modelo de PSI do PPSI 2.0 (SGD/MGI, 13/05/2026, 10 p.) | Princípios de proporcionalidade, menor privilégio e segregação de funções; normas mínimas (malware, vulnerabilidades, backup, incidentes, continuidade, auditoria); comitê; gestor de SI; encarregado; ETIR | Critério para governança e PSI. Limitação: modelo do setor público federal, que precisa ser adaptado a uma empresa privada. |
| 13 | `cartilha-seguranca-editavel - Google Slides.pdf` | **Material didático da turma** (Unimontes, 2026, 46 p.). Autores incluem Samir, Emanuel e Giovana | Controle de acesso (resumo do cap. 2 do TCU), senhas, engenharia social, biometria, **segregação de redes** (operacional, servidores, visitantes), criptografia (gestão de chaves), hashing, VPN, backup | Apoio didático. Limitações: material estudantil com simplificações (por exemplo, VPN apresentada como "anonimato" e hash tratado como "cifra"). |
| 14 | `Controle_de_Acesso_Fisico (1).pptx` | **Slides** (16), baseados em um blog de segurança privada | Defesa em profundidade, zoneamento, requisitos de um sistema de acesso (registro de eventos negados, bloqueio de dupla entrada, backup do próprio sistema), barreiras | Critério complementar para acesso físico. Fonte secundária. |
| 15 | `ativos riscos.pptx` | **Slides** (25, vários só com imagem) | Conceitos de ativo, vulnerabilidade, ameaça, risco e impacto. Material FEBRABAN de GCN: ISO 22300, causas de disrupção (Forrester), **categorização problema/incidente/crise** e **impactos e gatilhos de acionamento** | Critério para o acionamento do PCN e para os conceitos de risco. |
| 16 | `conteudo.pptx` | **Slides do professor** (MS. Luiz Antonio Antunes, 23, a maioria só com imagem) | CID e aspectos complementares (Lyra); categorias de ativos; vulnerabilidades (Sêmola); **níveis de sensibilidade de 1 a 5** com exemplos; tabela de proteção por nível; organogramas (SI subordinada à TI × SI independente) | Critério para classificação da informação e para governança (independência da função de SI). |
| 17 | `3-2-1-1-0.txt` | Artigo de **blog de fornecedor** (DataBackup, 13/02/2026) | Regras 3-2-1 e 3-2-1-1-0; imutabilidade; testes com "0 erros"; credenciais de backup separadas e com MFA; checklist | Critério técnico para backup. **As estatísticas e os custos são do fornecedor e não verificáveis**, por isso não foram usados como evidência. |
| 18 | `psi1.txt` | Página do **TCU** "Fiscalização de SI e Cibersegurança" (salva como texto) | Temas e instrumentos de avaliação (QACIs): backup (política, planos, testes, segurança física e lógica, recursos), AD e ransomware, phishing, incidentes, vulnerabilidades | Estrutura de critérios. O conteúdo dos QACIs não está na pasta. O nome "psi1" não corresponde ao conteúdo. |
| 19 | `politica_De_seguranca_da_informacao.txt` | Artigo de **blog comercial** (Clavis, 22/07/2024) | Definição de PSI e oito passos de elaboração | Apoio para a recomendação de PSI. Fonte comercial. |
| 20 | `referencia CPP forense.ppt` | **Slides** (26) sobre coleta de evidências digitais (Pereira & Barbieri, 2008) | Etapas da perícia, artigos do CPP, cópia bit a bit, hash, RFC 3227 | Apoio para preservação de evidências. **Desatualizado:** afirma que não há procedimento legal, mas a Lei 13.964/2019 incluiu os arts. 158-A a 158-F (cadeia de custódia) no CPP. |
| 21 | `pop-pericia-criminal-2024-informatica-forense-vol-5-pdf.pdf` | **POP** Senasp/MJSP 2024, vol. 5 (69 p.) | POP 5.03 (local de informática): isolamento físico e digital, ordem de volatilidade, *dump* de memória, hashes, cadeia de custódia. POP 5.04: análise de cabeçalhos SPF e DKIM | Apoio para o playbook de incidentes (preservar evidências antes de restaurar) e para segurança de e-mail. Escrito para o contexto pericial criminal, precisa ser adaptado. |
| 22 | `procedimentos volatily.pdf` | Artigo (Silva & Lorens) sobre análise de memória (8 p.) | *Live forensics*, Volatility | Apoio marginal (forense de memória na resposta a incidentes). |
| 23 | `analise_forense_ram1.txt` | Artigo web (CryptoID, 2017) | NIST SP 800-86 (coletar, examinar, analisar, reportar); RFC 3227 (ordem de volatilidade, inclusive "logs remotos") | Apoio para a resposta a incidentes. |

### 2.1 Arquivos por finalidade

| Finalidade | Arquivos |
|---|---|
| Documentação da proposta da DeliMarket | 1 |
| Diagramas de arquitetura e infraestrutura | 2, 3 |
| Políticas e procedimentos (referências) | 11, 12, 19, 18 |
| Banco de dados | **Nenhum documento específico.** A proposta presume PostgreSQL (§5.1), mas não há documentação do banco, que é uma lacuna |
| Backup e recuperação | 1 (§5–6), 11, 17 |
| Segurança da informação (geral) | 7, 13, 14, 16 |
| Auditoria e gestão de riscos | 7 (cap. 4), 8, 9, 15 |
| Forense e resposta a incidentes | 20, 21, 22, 23 |
| Transcrição da reunião | 6 |
| Instruções do professor e enunciado | 5, 4 |

### 2.2 Duplicidades, versões e documentos desatualizados

- **Não há duplicatas idênticas.** Os sufixos "(1)" e "(3)" indicam cópias baixadas. Não é possível comparar versões da proposta porque só existe uma.
- **Desatualizados ou superados:** cartilha do TCU (2012, 27002:2005, senhas); 17799:2005 (substituída pela 27002:2013 e depois pela 27002:2022); 27005:2011 (substituída pela edição de 2022); slides de CPP (anteriores à Lei 13.964/2019). Esses documentos continuam úteis como critério conceitual, mas não como estado da arte.
- **Nomes que não batem com o conteúdo:** `psi1.txt` (é uma página do TCU), `9d205…pdf` (o metadado diz 27005, mas são slides de SGSI), `nbr_iso_27002…` (o conteúdo é da 17799:2005).

## 3. Documentos × partes da reengenharia que eles permitem avaliar

| Parte da reengenharia | Documentos de avaliação |
|---|---|
| Diagnóstico de riscos e metodologia | 8, 9, 15, 7 (p. 33) |
| Arquitetura híbrida, redundância e HA | 1 (§3–4), 2, 7 (p. 66), 10 (9.1.4, 9.2.2) |
| Energia e ambiente físico | 1 (§4.4, §7), 10 (9.1–9.2), 14, 7 (p. 66–68) |
| Backup, RPO/RTO e DR | 1 (§5–6), 2, 11, 17, 18, 7 (p. 32–37, 73–74), 15 |
| Criptografia e chaves | 1 (§11), 13 (p. 43), 7 (p. 73, 76) |
| Controle de acesso lógico, MFA e privilégios | 1 (§5.6, §7.4, §9.2), 3, 7 (cap. 2; p. 84–85), 12 (art. 16), 13 |
| Biometria e acesso físico | 1 (§7.1), 3, 7 (p. 23–25), 10 (9.1.2), 14 |
| Logs, auditoria e monitoramento | 1 (§5.6, §7.1, §7.4), 3, 6, 7 (p. 17, 27–28, 76–78), 10 (10.1.1 h) |
| Redes, VPN e Zero Trust | 1 (§4.1, §7.4), 2, 6, 13 (p. 25–27, 35–37), 7 (p. 29) |
| Endpoints e malware | 6 (pendrive), 12 (art. 17 VIII), 7 (p. 11), 18 (phishing) |
| Resposta a incidentes e evidências | 12 (art. 17 X, art. 26), 7 (p. 95–97), 18, 20, 21, 23 |
| LGPD, classificação e retenção | 1 (§9–10), 4, 6, 16 (níveis de sensibilidade), 12 (art. 13), 11 (arts. 10–13) |
| BI e integridade | 1 (§3.3, §8), 2, 4, 7 (p. 92) |
| Governança, PSI e papéis | 7 (cap. 1; p. 34–35), 11 (art. 27), 12, 16 (organograma), 19 |

## 4. Detalhamento das referências usadas como critério

### 4.1 TCU — Boas Práticas em Segurança da Informação (4ª ed., 2012)
- **Assunto:** PSI, controle de acesso lógico, PCN e acórdãos do TCU organizados pelas seções da 27002:2005.
- **Conceitos e procedimentos:** logon seguro, ID único, senhas, tokens, biometria (taxas de erro), controle por menus, funções e arquivos, ACL, logs e trilhas, política de controle de acesso ("tudo proibido a menos que expressamente permitido"), responsabilidades segregadas; conteúdo do PCN, papel da alta administração, testes e manutenção.
- **Recomendações aplicáveis à DeliMarket:**
  - logs protegidos contra alteração inclusive por administradores (p. 17, 27, 76);
  - registro de operações diretas no BD e de DBAs (p. 77–78);
  - revisão periódica de acessos e ID único também para administradores (p. 84–85);
  - recursos redundantes não podem estar sujeitos aos mesmos riscos físicos (p. 66);
  - política de backup que inclua o código e a base, com criptografia das mídias (p. 73) e mídias guardadas fora do local (p. 74);
  - PCN com condições de ativação, responsáveis e suplentes, dependências externas e contratos (p. 34);
  - consistência entre bases (p. 92).
- **Critérios de auditoria:** itens das seções 2.4 a 2.9 e 3.4 a 3.7, além dos itens de acórdão citados.
- **Limitações:** contexto da administração pública e numeração da 27002:2005. As orientações de senha estão desatualizadas em relação às práticas atuais (referência externa: NIST SP 800-63B, que privilegia senhas longas e desaconselha troca periódica sem indício de comprometimento).

### 4.2 Apostila ISO/IEC 27005:2011
- **Assunto:** processo de gestão de riscos de SI.
- **Conceitos:** critérios de avaliação, impacto e aceitação (7.2.2–7.2.4); identificação de ativos, ameaças, controles existentes ou planejados e vulnerabilidades (8.2.2–8.2.5); análise qualitativa quando faltam dados numéricos (8.3.1); probabilidade (8.3.3); tratamento por modificação, retenção, evitação ou compartilhamento (9.2–9.5).
- **Recomendações aplicáveis:**
  - considerar também os **controles planejados** e verificar se funcionam (8.2.4), o que é o caso, já que a proposta é um plano;
  - implementar primeiro as opções que trazem grande redução de risco com pouco custo (9.1);
  - tratar riscos improváveis mas graves, mesmo sem justificativa estritamente econômica (9.1).
- **Critérios:** a matriz de riscos (Entregável 3) segue 8.3 e 8.4.
- **Limitações:** edição de 2011 e não normativa. O documento não impõe uma escala específica.

### 4.3 Slides SGSI (arquivo `9d205…pdf`)
- **Assunto:** implantação de SGSI pelo ciclo PDCA.
- **Conceitos:** escala de 1 a 5; Probabilidade = f(ameaça, vulnerabilidade); Impacto = f(relevância de ativo, serviço e processo); **Risco = Impacto × Probabilidade**; quatro respostas ao risco; Declaração de Aplicabilidade.
- **Aplicação:** escala e fórmula da matriz de riscos; recomendação de Declaração de Aplicabilidade no SGSI (AC30).
- **Limitações:** material de aula, com pouco detalhe.

### 4.4 ABNT NBR ISO/IEC 17799:2005 (arquivo `nbr_iso_27002 segurança fisica.pdf`)
- **Assunto:** segurança física e do ambiente e procedimentos operacionais.
- **Itens aplicáveis:**
  - 9.1.2 b): cartão mais PIN e registro seguro de todos os acessos para auditoria;
  - 9.1.2 e): revisão periódica dos direitos de acesso físico;
  - 9.1.4 b): equipamentos de contingência e mídia de backup a distância segura;
  - 9.1.4 c): detecção e combate a incêndio;
  - 9.1.5 b): evitar trabalho não supervisionado em áreas seguras;
  - 9.2.1 f): monitorar temperatura e umidade;
  - 9.2.1 g): proteção contra raios;
  - 9.2.2: UPS, plano para falha do UPS, gerador testado regularmente, combustível, múltiplas fontes ou subestação separada;
  - 9.2.3: rotas alternativas de cabeamento;
  - 9.2.4: registros de manutenção;
  - 9.2.6: descarte seguro;
  - 10.1.1: procedimentos documentados de backup, reinício, recuperação e trilhas de auditoria.
- **Limitações:** edição de 2005 e documento escaneado.

### 4.5 NC-03 — Política de Backup e Restauração (IF Sertão-PE, 2021)
- **Assunto:** diretrizes, responsabilidades e requisitos de backup.
- **Itens aplicáveis:**
  - contratos para dados custodiados por terceiros (art. 5º, p. 4), aplicável ao provedor de nuvem;
  - requisitos diferenciados por criticidade (art. 8º);
  - soluções automatizadas (art. 9º);
  - retenção mínima para sistemas críticos de 5 anos na cópia anual (art. 11, p. 4–5);
  - conteúdo do plano (art. 16, p. 5–6);
  - impacto na rede e janela (arts. 17–19);
  - critérios de escolha de mídia, inclusive tempo esperado de restauração (art. 20);
  - compressão sem prejudicar a restauração (art. 22);
  - mídias em local seguro e com acesso restrito (art. 23);
  - testes em servidor diferente do de produção (art. 25);
  - descarte registrado (art. 26);
  - atribuições dos responsáveis (art. 27, p. 8–9);
  - modelo de plano com assinaturas (anexo, p. 10–11).
- **Limitações:** norma de órgão público. As retenções mínimas são referência e não obrigação para a DeliMarket.

### 4.6 Modelo de PSI — PPSI 2.0 (SGD/MGI, 2026)
- **Itens aplicáveis:**
  - proporcionalidade à criticidade (art. 12, p. 5);
  - integração com a proteção de dados pessoais (art. 13);
  - menor privilégio e segregação de funções (art. 16, p. 6);
  - lista mínima de normas, entre elas defesa contra códigos maliciosos, vulnerabilidades, backup, incidentes, continuidade e auditoria (art. 17);
  - comitê com o encarregado (art. 18);
  - cláusulas de segurança em contratos (art. 19);
  - investimento conforme o risco (art. 20, p. 7);
  - demonstrar a adoção de medidas por meio de evidências (art. 23, parágrafo único, p. 8);
  - comunicação de incidentes (art. 26);
  - exceções documentadas com prazo (art. 30, p. 9).
- **Limitações:** foi feito para órgãos do SISP.

### 4.7 Demais materiais
- **Cartilha da turma:** segregação em rede operacional, de servidores e de visitantes, para conter movimento lateral (p. 25–27); boas práticas de chaves: armazenar, rotacionar, revogar e nunca deixar *hardcoded* (p. 43); hash para integridade (p. 38–40).
- **Controle de Acesso Físico (pptx):**
  - eventos negados também registrados e bloqueio de dupla entrada (slide 6);
  - defesa em profundidade (slide 7) e zoneamento (slide 8);
  - "política mais importante que tecnologia" (slide 16).
- **Ativos e riscos (pptx, FEBRABAN):**
  - categorização em problema, incidente e crise (slide 6);
  - tabela de impactos e **gatilhos de acionamento**, do caráter de emergência ao caráter de crise (slide 7);
  - ciclo de vida da informação (slide 9).
- **Slides do professor (conteudo.pptx):**
  - CID e aspectos complementares segundo Lyra (slide 8), categorias de ativos (slide 9) e vulnerabilidades segundo Sêmola (slide 10);
  - organogramas com a SI subordinada à TI ou independente (slide 12);
  - níveis de sensibilidade de 1 a 5, de "pública" a "ultrassecreta" (slides 14–18);
  - estimativa de dano potencial (slide 19) e tabela de proteção e custo por nível (slide 20).
- **3-2-1-1-0 (blog):** cópia imutável ou *air-gapped*, "0 erros" com resultados documentados, credenciais de backup independentes das de produção e MFA (checklist).
- **TCU – página de fiscalização (psi1.txt):** eixos de avaliação de backup (política; planos e procedimentos; testes de restauração; segurança física; segurança lógica; recursos) e o tema "AD: por onde o ransomware acontece".
- **POP Senasp 2024 e material forense:**
  - isolar física e digitalmente, coletar do mais volátil ao menos volátil e registrar hashes e cadeia de custódia (POP 5.03, itens 4.2–4.5);
  - em servidores, pedir apoio ao responsável de TI depois de confirmar que ele não é suspeito (POP 5.03, item 4.3.1);
  - NIST SP 800-86 e RFC 3227, citados em `analise_forense_ram1.txt`.

## 5. Lacunas do conjunto documental (afetam as conclusões)

| Lacuna | Efeito na análise |
|---|---|
| Não há documentação do banco de dados. O uso de PostgreSQL é uma premissa da proposta ("Considerando o uso de PostgreSQL no ERP", §5.1) | A análise técnica de PITR e WAL vale **se** o banco for PostgreSQL. Com outro SGBD, os mecanismos equivalentes mudam (pergunta D-01). |
| Não há inventário de ativos, número de usuários, filiais, volume de dados, banda contratada nem orçamento | As estimativas de RTO e de custo são **qualitativas**. Usamos tabelas de sensibilidade com volumes hipotéticos, sem atribuí-los à empresa. |
| Os diagramas existem apenas como *print* | Algumas leituras (por exemplo, se o balanceador é único) dependem da fidelidade do desenho (pergunta G-02). |
| A empresa é fictícia e acadêmica e a proposta é um **plano** | Toda a avaliação é de **adequação do desenho**, não de eficácia operacional. Nenhum controle foi testado e nenhum teste foi executado. |
