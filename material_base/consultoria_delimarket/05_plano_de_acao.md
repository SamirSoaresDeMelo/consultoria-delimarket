# Entregável 5 — Plano de ação e viabilidade

## 1. Critérios de priorização

- **Reduzir primeiro o maior risco com o menor custo.** A ISO 27005, 9.1, recomenda adotar logo as opções que trazem grande redução de risco com pouca despesa. As ações imediatas abaixo são quase todas configuração, processo ou decisão de gestão.
- **Proporcionalidade.** O Modelo de PSI (arts. 12 e 20) pede investimento proporcional à criticidade e ao risco. Como referência externa de linha de base para uma empresa sem equipe de segurança grande, usamos o grupo IG1 dos CIS Controls v8.1.
- **Dependências.** Investimentos maiores, como a alta disponibilidade do banco (AC22) e um SOC (AC34), **só vêm depois da BIA** (AC12).
- **Sem valores inventados.** A DeliMarket não informou orçamento. O custo é relativo:

| Custo | O que significa |
|---|---|
| **Baixo** | Processo, documento ou configuração com recursos que já existem (políticas de grupo, recursos nativos do sistema operacional e da nuvem) e horas internas |
| **Médio** | Licença ou assinatura (EDR, coletor de logs ou SIEM gerenciado), um servidor ou armazenamento adicional, recursos de nuvem extras, apoio jurídico ou técnico pontual |
| **Alto** | Novo site físico, SOC próprio 24x7 ou plataforma completa de Zero Trust |

**Prioridade:**
- **P1** = essencial: trata um risco crítico ou alto, ou é pré-requisito de outras ações.
- **P2** = recomendada.
- **P3** = melhoria de maturidade ou ação que depende de diagnóstico.

**Responsáveis:** indicados por **função**, nunca por nome.

## 2. Plano de ação

### 2.1 Imediatas (0 a 30 dias)

| Prioridade | ID | Problema | Ação recomendada | Responsável sugerido | Prazo | Custo | Resultado esperado | Riscos |
|---|---|---|---|---|---|---|---|---|
| P1 | AC01 | Não há governança, papéis nem aceitação formal de riscos (A02, A05) | Aprovar uma **PSI enxuta**, adaptada do Modelo PPSI 2.0, e a **RACI** do relatório (seção 6.1). Nomear gestor de SI, coordenador de crise e suplente, administrador de backup, aprovador de restaurações e **dois** custodiantes de chaves. Registrar a aceitação formal do gerador único | Diretoria e gestor de SI | 30 dias | Baixo | Responsáveis conhecidos; riscos retidos com dono | Transversal (R03, R05, R06, R17) |
| P1 | AC02 | Não há critério para acionar o DR nem procedimento para o negócio (A06, A23) | Definir gatilhos objetivos (por exemplo: DC fora do ar há mais de 15 min, sem previsão de retorno em 30 min); decisor e suplente 24x7; árvore de contatos, incluindo fornecedores; roteiro de comunicação com clientes; procedimentos manuais mínimos (lista de separação impressa, registro de pedidos, prioridade por validade) | Coordenador de crise, com logística e comercial | 30 dias | Baixo | *Failover* decidido em até 15 min; operação mínima sem o ERP | R06 |
| P1 | AC03 | MFA restrito e sem escopo; contas administrativas misturadas (A07, A11) | Separar as contas administrativas das contas de uso diário. MFA obrigatório para todo acesso administrativo, remoto, ao console da nuvem e ao console de backup. Contas de emergência (*break-glass*) em cofre, com alerta a cada uso. Eliminar contas compartilhadas | Gerente de infraestrutura | 30 dias | Baixo a médio (licenças de MFA, se não houver) | Uma credencial roubada sozinha não basta para administrar | R01, R02, R09 |
| P1 | AC04 | Caminhos de recuperação podem ser atacados com uma única credencial; WAL sem imutabilidade (A07, A08) | Cofre de backup em **conta de nuvem separada**, sem federação com o domínio. Imutabilidade em **modo conformidade**. **WAL arquivado no mesmo cofre imutável.** Servidor de backup fora do domínio de produção. Alertas para exclusões e mudanças de retenção | Administrador de backup e gestor de SI | 30 dias | Baixo (configuração e armazenamento marginal) | RPO de minutos também em ransomware; backups que ninguém consegue apagar | R01, R02, R07 |
| P1 | AC05 | Pendrive malicioso (A14) | Bloquear o armazenamento USB por padrão (política de grupo ou EDR). Desativar a execução automática. Bloquear macros em documentos vindos da internet. Exceções apenas com aprovação e mídia corporativa cifrada | Suporte e infraestrutura | 15 dias | Baixo | Vetor USB fechado | R01 |
| P1 | AC06 | Dependência exclusiva do KMS (A09) | Custódia *offline* das chaves mestras e das chaves das fitas (dupla custódia, cofre, envelope lacrado). Período de espera antes de excluir qualquer chave. Recuperação de chave ensaiada | Gestor de SI e custodiantes | 30 dias | Baixo | Backups decifráveis mesmo sem a nuvem | R12 |
| P1 | AC07 | Escopo do backup incompleto (A21) | Incluir: diretório de identidades, DNS e DHCP, configurações de firewall e switches, hipervisor e modelos de VM, binários e configurações do ERP, certificados, código das integrações e do ETL, arquivos administrativos. Manter cópia *offline* dos *runbooks* | Administrador de backup | 30 dias | Baixo | DR reconstruível de ponta a ponta | R03, R06 |
| P2 | AC08 | Inconsistências e itens não entregues (A22) | Revisar a proposta: corrigir o diagrama (banco e balanceador); deixar explícito que o cluster é ativo-passivo; casar metas com mecanismos (logística, arquivos, BI); incluir os riscos cibernéticos no §2; incluir cronograma e o exemplo de criptografia; entregar os diagramas em arquivo-fonte | Equipe DeliMarket (autores) | 15 dias | Baixo | Documento coerente e auditável | Transversal |
| P1 | AC09 | Atraso de replicação e de arquivamento sem limites; risco de lotar o `pg_wal` (relatório, 7.2 e 7.4) | Limites de alerta (por exemplo, aviso em 60 s e crítico em 180 s); alerta de falha de arquivamento; `max_slot_wal_keep_size`; alerta de ocupação de disco; monitoramento hospedado **fora do DC** | DBA | 15 dias | Baixo | RPO monitorado; primário protegido se a nuvem cair | R04, R07 |
| P2 | AC10 | Ninguém treinado para a crise (A06, A16, A23) | Exercício de mesa com dois cenários (perda do DC e ransomware), cronometrando as decisões e registrando as lacunas | Coordenador de crise e gestor de SI | 30 dias | Baixo | Falhas do plano descobertas antes da crise real | R06, R17 |

### 2.2 Curto prazo (1 a 3 meses)

| Prioridade | ID | Problema | Ação recomendada | Responsável sugerido | Prazo | Custo | Resultado esperado | Riscos |
|---|---|---|---|---|---|---|---|---|
| P2 | AC11 | Não há inventário nem gestão de mudanças (G05, G08) | Inventário de ativos (hardware, software, dados, serviços de nuvem, contas de serviço) com dono; mapa de fluxos de dados; processo mínimo de mudanças que mantenha **produção e contingência iguais** | Gerente de infraestrutura | 60 dias | Baixo a médio | Base para patches, backup, LGPD e DR | Transversal |
| P1 | AC12 | Metas sem BIA (A03) | BIA de pedidos, separação, faturamento, entregas, BI e arquivos: custo da hora parada, perda de perecíveis, tempo máximo tolerável, dependências. Validar ou ajustar RPO e RTO. Decidir os investimentos AC22 e AC34 | Coordenador de crise, donos dos processos e controladoria | 60 dias | Baixo (horas internas) | Metas justificadas e orçamento coerente | R06, R04, R07 |
| P1 | AC13 | Rede plana (hipótese) (A13) | Zonas: usuários, aplicação, dados, **backup isolado**, gerência e segurança, DMZ, visitantes e Wi-Fi, IoT (se houver). Matriz de fluxos com **negar por padrão**. Administração somente pelo bastion com MFA. VPN para acesso remoto e para a nuvem | Equipe de redes | 90 dias | Médio (configuração; talvez troca de equipamento) | Estação comprometida não alcança o banco nem o backup | R01, R15 |
| P1 | AC14 | Sem antimalware/EDR e sem gestão de vulnerabilidades (A14) | EDR com console central e isolamento remoto. Patches com prazo por criticidade (por exemplo, críticos em até 7 dias e altos em até 30). Varredura mensal de vulnerabilidades em servidores e na borda | Infraestrutura e gestor de SI | 60 dias | Médio (licença por endpoint) | Malware detectado e contido; menos tempo de exposição | R01, R18 |
| P1 | AC15 | Logs sem proteção (A10) | Coletor central na zona de segurança, com administração segregada, NTP comum, cópia imutável diária com hash, os dez alertas iniciais e revisão semanal da atividade privilegiada. Integrar os eventos do controle de acesso físico (relatório, seção 8) | Gestor de SI | 90 dias | Médio | Rastreabilidade mesmo com um administrador comprometido | R08, R17 |
| P2 | AC16 | Gestão de identidades sem processo (A11) | Matriz de perfis (RBAC) do ERP e da infraestrutura. Admissão, mudança de função e desligamento integrados ao RH, com bloqueio no mesmo dia. Revisão trimestral dos privilegiados e semestral dos demais. Política de senhas atual. Mapeamento de conflitos de segregação de funções | Gestor de SI, RH e donos dos sistemas | 90 dias | Baixo | Acesso coerente com a função | R09, R02 |
| P1 | AC17 | Não há resposta a incidentes (A16) | Plano de resposta com papéis, classificação, contenção, erradicação, recuperação e lições aprendidas. **Playbook de ransomware**: isolar, preservar evidências **antes** de restaurar, escolher um ponto de restauração anterior ao comprometimento, trocar todas as credenciais. Procedimento de evidências (hash, cadeia de custódia). Comunicação à ANPD e aos titulares em **3 dias úteis**, e aos clientes | Gestor de SI, encarregado e jurídico | 60 dias | Baixo | Resposta coordenada e em conformidade com a LGPD | R17, R11 |
| P1 | AC18 | LGPD sem aplicação prática; sem classificação (A17, A18) | Mapa de dados e registro de operações; base legal por tratamento; encarregado e substituto nomeados; canal para o titular; **tabela de temporalidade** validada pelo jurídico e pela contabilidade, separando backup de arquivamento; classificação em 4 níveis; tratamento da biometria, se ela for mantida | Encarregado, jurídico e gestor de SI | 90 dias | Médio (pode exigir apoio jurídico externo) | Conformidade demonstrável | R11, R10 |
| P2 | AC19 | Usuários como porta de entrada (A14) | Conscientização trimestral e simulações de phishing. SPF, DKIM e DMARC no domínio de e-mail | Gestor de SI e comunicação interna | 60 dias | Baixo | Menos cliques; e-mails falsificados bloqueados | R01 |
| P1 | AC20 | Política de backup sem governança (A21) | Plano por sistema no modelo do anexo da NC-03: escopo, janela, retenção, mídia, estratégia, responsáveis, procedimentos de teste e de restauração, assinaturas. Autorização formal para restaurações. Relatório padronizado de teste ("0 erros"). Comprimir antes de cifrar. Leitura semestral das fitas fora do DC e plano para conseguir um *drive* de fita | Administrador de backup e DBA | 60 dias | Baixo | Backup gerido com evidências | R03, R10 |
| P1 | AC21 | Nuvem e fornecedores sem governança (A20) | Contas separadas (contingência, cofre de backup, logs). Linha de base de IAM e de configuração. Trilha de auditoria da nuvem. Região escolhida considerando a LGPD. Contrato com SLA, cláusulas de operador e plano de saída. Comprovar as rotas distintas dos provedores de internet. Contrato de combustível e manutenção do gerador | Gestor de SI, jurídico e compras | 90 dias | Médio | Responsabilidade compartilhada coberta | R02, R14, R15 |

### 2.3 Médio prazo (3 a 9 meses)

| Prioridade | ID | Problema | Ação recomendada | Responsável sugerido | Prazo | Custo | Resultado esperado | Riscos |
|---|---|---|---|---|---|---|---|---|
| P3 (depende da BIA) | AC22 | Banco local como ponto único de falha (A04) | Réplica local em outro servidor e outro armazenamento, com gerenciador de *failover* e *fencing*. Avaliar uma réplica com atraso de 1 a 4 h | DBA e infraestrutura | 6 meses | Médio a alto (servidor, armazenamento, horas especializadas) | Falha de um servidor resolvida localmente em minutos | R04, R07 |
| P1 | AC23 | RTO e RPO sem evidência (A06) | **Teste completo de *failover* e *failback***: o primeiro até o 3º mês e depois a cada trimestre. Cronometrar cada etapa (relatório, 7.5), incluindo identidade, DNS, certificados, integrações, NF-e e **restauração dentro da nuvem** a partir do cofre. Monitoramento fora do DC | Coordenador de crise, infraestrutura e DBA | 3 a 6 meses | Médio (horas e recursos de nuvem durante o teste) | RTO e RPO medidos e confiáveis | R06, R07, R15 |
| P2 | AC24 | Segurança física e energia (A05, A12) | Sala do DC com 2 fatores (cartão e PIN; biometria só se for justificada e tratada conforme a LGPD), bloqueio de dupla entrada, CFTV, registro de visitantes, eventos enviados ao coletor, chave de emergência lacrada. Gerador testado sob carga; ponto de conexão para gerador móvel; desligamento ordenado comandado pelo UPS. Avaliar levar o servidor de backup para outro compartimento ou site | Facilities e infraestrutura | 6 meses | Médio | Acesso físico rastreável; recuperação local sem depender de um único ponto físico | R16, R05, R08 |
| P2 | AC25 | BI (A19) | Extrair de réplica ou CDC com conta somente leitura. Segurança por linha. Pseudonimizar identificadores. Reconciliação diária. Alerta de falha do ETL. Documentar a linhagem. Definir onde o BI roda na contingência e tornar o RPO de 1 h viável (reconstrução a partir do ERP ou backup mais frequente) | Time de BI, DBA e encarregado | 6 meses | Médio | Indicadores confiáveis e menos expostos | R13, R10 |
| P2 | AC26 | Banco sem auditoria; acesso privilegiado (relatório, 6.3) | Auditoria no banco (DDL, funções e privilégios, tabelas sensíveis). Acesso de DBA só pelo bastion, com registro. Elevação de privilégio sob demanda. Nenhuma alteração direta fora de procedimento | DBA e gestor de SI | 6 meses | Médio | Operações privilegiadas rastreáveis | R09, R08 |
| P2 | AC27 | ERP e integrações (A15) | MFA para acesso remoto e perfis críticos do ERP. Trilha de auditoria das transações sensíveis. *Time-out* e bloqueio por tentativas. Cofre de segredos para as integrações. SLA de correções com o fornecedor | Dono do ERP, fornecedor e gestor de SI | 6 meses | Médio | Fraude detectável; segredos protegidos | R09, R18 |
| P2 | AC28 | Estações e dispositivos sem *hardening* (A14) | Linha de base de configuração (CIS Benchmarks). Fim do administrador local. Senha local única por máquina. Criptografia de disco nos notebooks. MDM para celulares e coletores, se existirem | Infraestrutura | 6 meses | Médio | Superfície de ataque menor | R01, R10 |
| P3 (depende de diagnóstico) | AC29 | Dependências fiscais e de integração no DR (A06, A15) | Confirmar como funcionam a NF-e e as integrações. Disponibilizar o certificado com segurança na contingência. Procedimento de contingência fiscal. Incluir no teste AC23 | Fiscal, infraestrutura e fornecedor do ERP | 6 meses | Baixo a médio | Faturamento e expedição funcionam no DR | R06 |

### 2.4 Longo prazo (9 a 18 meses)

| Prioridade | ID | Problema | Ação recomendada | Responsável sugerido | Prazo | Custo | Resultado esperado | Riscos |
|---|---|---|---|---|---|---|---|---|
| P3 | AC30 | Não há SGSI (A01, A22) | SGSI baseado na ISO/IEC 27001, com escopo inicial de ERP, DC e nuvem: metodologia de riscos, Declaração de Aplicabilidade, comitê de SI e revisão anual dos riscos | Diretoria e gestor de SI | 12 a 18 meses | Médio a alto | Melhoria contínua | Transversal |
| P3 | AC31 | Falta verificação independente | Auditoria interna anual; teste de invasão externo anual; teste completo de DR anual e parcial trimestral; revisão anual da BIA | Gestor de SI e auditoria | 12 meses, depois recorrente | Médio | Controles verificados por terceiros | R18, transversal |
| P3 | AC32 | Confiança implícita na rede | Evolução para Zero Trust: acesso condicionado à postura do dispositivo (NAC/ZTNA) e microssegmentação dos sistemas críticos | Redes e gestor de SI | 12 a 18 meses | Alto | Movimento lateral mínimo | R01, R15 |
| P3 | AC33 | Não há indicadores | Painel de indicadores: RPO e RTO medidos, % de testes de restauração sem erro, prazo de aplicação de patches, revisões de acesso concluídas, taxa de cliques em phishing, tempo de detecção | Gestor de SI | 9 meses | Baixo | Gestão por evidência | Transversal |
| P3 (depende de diagnóstico) | AC34 | Não há monitoramento 24x7 | Avaliar um SOC ou MDR terceirizado, conforme a BIA e o orçamento | Diretoria e gestor de SI | 12 meses | Médio a alto | Detecção fora do horário comercial | R01, R17 |

## 3. Categoria e verificação de implementação

**Categorias:**

| Categoria | Ações | Total |
|---|---|---|
| Essencial | AC01–AC07, AC09, AC12–AC15, AC17, AC18, AC20, AC21, AC23 | 17 |
| Recomendada | AC08, AC10, AC11, AC16, AC19, AC24–AC28 | 10 |
| Melhoria de maturidade | AC30–AC33 | 4 |
| Depende de diagnóstico | AC22 (BIA), AC29 (fluxo fiscal), AC34 (BIA e orçamento) | 3 |

**Como verificar cada ação:**

| ID | Como verificar se foi implementada corretamente | Depende de |
|---|---|---|
| AC01 | PSI e RACI assinadas pela diretoria; atos de nomeação; registro de aceitação do gerador único | – |
| AC02 | Documento de gatilhos aprovado; árvore de contatos testada (ligação simulada); decisão tomada em até 15 min no exercício AC10 | AC01 |
| AC03 | Relatório de contas privilegiadas: 100% nominais e com MFA; zero contas compartilhadas ativas; alerta disparado no uso de uma conta *break-glass* | – |
| AC04 | Teste controlado: excluir um objeto sob retenção **falha**; alerta recebido; PITR feito apenas com WAL e backup tirados do cofre | AC01 |
| AC05 | Pendrive de teste bloqueado em uma amostra de estações; relatório de exceções aprovadas | – |
| AC06 | Ensaio registrado: uma chave recuperada da custódia decifra um backup | AC01 |
| AC07 | Lista de escopo comparada com o inventário (AC11); restauração de teste do diretório de identidades em ambiente isolado | – |
| AC08 | Nova versão da proposta, sem as inconsistências da seção 4.3 do relatório | – |
| AC09 | Alerta disparado ao interromper a replicação em teste; parâmetro `max_slot_wal_keep_size` configurado; painel do monitoramento hospedado fora do DC | – |
| AC10 | Relatório do exercício, com tempos e lacunas | AC01, AC02 |
| AC11 | Inventário com dono para cada ativo; mudanças registradas com aprovação | – |
| AC12 | BIA aprovada; RPO e RTO revisados e assinados pela diretoria | AC01 |
| AC13 | Varredura a partir da rede de usuários não alcança as portas do banco, do backup nem de gerência; regras conferem com a matriz de fluxos | AC11 |
| AC14 | Cobertura de EDR acima de 95%; teste de isolamento remoto; relatório de patches dentro do prazo | AC11 |
| AC15 | Teste: administrador apaga um log local e o evento continua no coletor; alerta de silêncio dispara; relógios sincronizados | AC13 |
| AC16 | Ata da revisão trimestral; amostra de desligados bloqueados no mesmo dia | AC01 |
| AC17 | Plano aprovado; simulação de ransomware com registro do tempo de notificação | AC01 |
| AC18 | Registro de operações; ato de nomeação do encarregado; tabela de temporalidade aprovada; canal do titular funcionando | AC11 |
| AC19 | Taxa de cliques em queda entre campanhas; DMARC com política de quarentena ou rejeição | – |
| AC20 | Planos assinados; relatórios mensais de teste; registro de cada restauração aprovada | AC01 |
| AC21 | Contas separadas; contrato com SLA e cláusulas de operador; trilha de auditoria da nuvem ativa | AC01 |
| AC22 | Desligamento do primário em teste: a réplica assume sem *split-brain* | AC12 |
| AC23 | Relatório com os tempos de cada etapa; RTO e RPO medidos; desvios tratados | AC02, AC04, AC07 |
| AC24 | Relatório do controle de acesso (2 fatores, eventos no coletor); relatório do teste do gerador sob carga | AC15 |
| AC25 | Relatório de reconciliação diário; teste de acesso negado por linha; identificadores pseudonimizados | AC18 |
| AC26 | Trilha do banco para alterações de função e privilégio, com amostra revisada | AC15 |
| AC27 | Trilha das transações sensíveis; MFA nos perfis críticos | AC16 |
| AC28 | Conformidade com a linha de base acima de 90% das máquinas; discos dos notebooks cifrados | AC14 |
| AC29 | NF-e emitida no ambiente de contingência durante o teste AC23 | AC23 |
| AC30–AC34 | Escopo e Declaração de Aplicabilidade aprovados; relatórios de auditoria e de teste de invasão; painel de indicadores ativo; contrato de SOC (se aprovado) | AC12 |

## 4. Viabilidade das recomendações principais

| Recomendação | Benefício de segurança | Risco mitigado | Complexidade | Custo relativo (justificativa) | Equipe especializada? | Impacto na operação | Dependências | Prazo | Prioridade | Implantação gradual? |
|---|---|---|---|---|---|---|---|---|---|---|
| AC04 + AC06: proteger os caminhos de recuperação | Muito alto: elimina o cenário "uma credencial destrói tudo" | R01, R02, R07, R12 | Baixa a média | **Baixo**: recursos nativos de imutabilidade e de contas da nuvem; armazenamento marginal | Não (administrador de nuvem) | Nenhum | AC01 (papéis) | 30 dias | P1 | Sim: primeiro o cofre, depois o WAL e as chaves |
| AC03: MFA e contas separadas | Alto | R02, R09, R01 | Baixa | **Baixo a médio**: MFA nativo do diretório ou da nuvem; licença se faltar | Não | Pequeno (hábito de usar duas contas) | – | 30 dias | P1 | Sim: primeiro os administradores, depois o acesso remoto e por fim os usuários do ERP |
| AC05: bloqueio de USB | Alto para o vetor pendrive | R01 | Baixa | **Baixo**: política de grupo ou EDR | Não | Pequeno (processo de exceção) | – | 15 dias | P1 | Sim: piloto, depois toda a empresa |
| AC02 + AC10: acionamento e exercício de mesa | Alto para o RTO | R06, R17 | Baixa | **Baixo**: horas internas | Não | Nenhum | AC01 | 30 dias | P1/P2 | – |
| AC12: BIA | Alto (orienta todo o investimento) | R06, R04 | Média | **Baixo**: horas internas e da controladoria | Desejável apoio de quem já fez uma BIA | Nenhum | AC01 | 60 dias | P1 | – |
| AC13: segmentação | Muito alto contra movimento lateral | R01, R15 | Média a alta | **Médio**: configuração de VLANs e firewall; talvez troca de equipamento | Sim (redes) | Médio: exige janelas de mudança | AC11 | 90 dias | P1 | Sim: backup e gerência primeiro, depois dados, depois usuários |
| AC14: EDR e patches | Alto | R01, R18 | Média | **Médio**: licença por endpoint | Parcial (pode ser serviço gerenciado) | Pequeno | AC11 | 60 dias | P1 | Sim: servidores primeiro |
| AC15: logs protegidos | Alto para a rastreabilidade | R08, R17 | Média | **Médio**: coletor *open source* (horas) ou serviço gerenciado (assinatura) | Sim (mínima) | Nenhum | AC13 | 90 dias | P1 | Sim: dez alertas primeiro, depois ampliar fontes |
| AC17: resposta a incidentes | Alto | R17, R11 | Baixa a média | **Baixo**: documento e treinamento | Desejável | Nenhum | AC01 | 60 dias | P1 | Sim: ransomware primeiro, depois vazamento |
| AC18: LGPD mínima | Alto (conformidade legal) | R11, R10 | Média | **Médio**: apoio jurídico pontual | Sim (jurídico e encarregado) | Pequeno | AC11 | 90 dias | P1 | Sim: encarregado e registro de operações, depois temporalidade |
| AC20: plano de backup (NC-03) | Alto (evita repetir 2023) | R03 | Baixa | **Baixo**: documento | Não | Nenhum | AC01 | 60 dias | P1 | – |
| AC23: teste de DR cronometrado | Muito alto (transforma a meta em evidência) | R06, R07, R15 | Média a alta | **Médio**: horas e nuvem em escala durante o teste | Sim | Médio: janela planejada; pode ser feito em ambiente isolado | AC02, AC04, AC07 | 3 a 6 meses | P1 | Sim: componente por componente, depois o ensaio completo |
| AC22: alta disponibilidade local do banco | Alto para falhas de servidor | R04, R07 | Alta | **Médio a alto**: servidor, armazenamento e especialista | Sim (DBA) | Médio na implantação | **AC12** | 6 meses | P3 | Sim: primeiro a réplica, depois o *failover* automático |
| AC24: físico e energia | Médio | R16, R05 | Média | **Médio**: leitores, CFTV, adequações elétricas | Parcial (facilities) | Pequeno | AC15 | 6 meses | P2 | Sim |
| AC32: Zero Trust avançado | Médio a alto | R01, R15 | Alta | **Alto**: plataforma NAC/ZTNA | Sim | Médio | AC13, AC14, AC16 | 12 a 18 meses | P3 | Sim, por definição |

### 4.1 Se o orçamento for muito restrito: sequência mínima viável

**AC01 → AC03 → AC04 → AC05 → AC06 → AC07 → AC09 → AC02 → AC20 → AC12 → AC14 (EDR básico) → AC17 → AC18 (encarregado e registro de operações).**

Essas 13 ações custam pouco e, sozinhas, levam R01 de **crítico** para **alto/médio** e R02, R03 e R12 para **médio**. O restante pode ser escalonado depois da BIA.

## 5. Linha do tempo resumida

| Fase | Meses | Marco de saída |
|---|---|---|
| Imediata | 0–1 | Papéis aprovados; credenciais isoladas; USB bloqueado; chaves sob custódia; gatilhos de DR; primeiro exercício de mesa |
| Curto prazo | 1–3 | BIA concluída; segmentação básica; EDR; logs centralizados; plano de incidentes; LGPD mínima; plano de backup assinado; nuvem governada |
| Médio prazo | 3–9 | Primeiro teste completo de DR com tempos medidos; decisão e implantação da alta disponibilidade do banco; físico, BI, auditoria do banco e do ERP |
| Longo prazo | 9–18 | SGSI; auditoria e teste de invasão anuais; Zero Trust avançado; indicadores; SOC/MDR (se justificado) |
