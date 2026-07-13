# Módulo 4 — Construindo um sistema operacional de IA
Timestamps fonte: 1:20:31–2:03:17

## Tópicos do módulo
1. Construindo seu AI Operating System
2. Conectando o Google Workspace
3. Diferenças entre CLIs e MCPs
4. Aprofundamento em Skills
5. Janela de contexto
6. Transferência de informações entre sessões

## Conteúdo condensado

### O que é um "AI Operating System" (AIOS)
É o projeto Claude Code central de alguém (exemplo do autor: projeto "Herk 2"), conectado a TODAS as ferramentas do negócio, com contexto completo — a ponto de "saber o negócio melhor que o próprio dono" porque tem memória perfeita e busca instantânea. Piada real da equipe: se não acham o Nate, mandam mensagem pro AIOS dele, que responde melhor e mais rápido que ele. Framework dos **4 Cs**: Contexto, Conexões, Capacidades, Cadência — os dois primeiros são o "segundo cérebro" (módulos 5 e 8), os dois últimos são skills/automações/agentes/rotinas (módulos 4, 6, 9).

### Como escolher o que conectar
"Tier 1": receita, clientes, calendário, comunicações, tarefas, reuniões, conhecimento. Regra prática: olhe as abas abertas agora, seus favoritos, seu histórico de navegador — são essas as ferramentas que você mais visita e que valem a pena conectar. Depois é só: Google "[ferramenta] API documentation" → pedir pro Claude Code ler a doc e configurar → pegar a chave de API → colar no `.env`. Pronto, ferramenta conectada ao AIOS.
Mudança de hábito central: meça "que % do meu dia consigo fazer só dentro dessa janela do Claude Code?" — não é sobre ficar menos produtivo, é sobre construir mais skills e melhorar o CLAUDE.md a cada interação.

### Google Workspace CLI (GWS) — demo extensa
Ferramenta open source e gratuita do Google (GitHub, "não é produto oficialmente suportado" — beta ativo). Um único CLI dá acesso a Drive, Gmail, Calendar, Docs, Sheets, Slides, Admin — com mais de 100 "receitas"/workflows prontos (ex.: criar doc de template, ler dados de planilha e gerar relatório, achar horário livre e agendar reunião). Vantagens sobre integrações tradicionais: interface única, respostas em JSON estruturado (ótimo para IA), auto-descoberta (atualiza sozinho quando o Google adiciona endpoints), baixíssima manutenção depois de autenticar uma vez.
Setup (passo a passo real): pedir pro Claude Code ler o repositório do GitHub e instalar → criar projeto no Google Cloud Console → configurar tela de consentimento OAuth (escolher "interno" se for só para você) → criar credencial tipo "Desktop app" → baixar o JSON de credenciais para `~/.config/GWS` → rodar `gws auth login` (abre navegador pra autenticar) → habilitar as APIs necessárias no Console (Drive, Gmail, Calendar etc., uma a uma, clicando "enable"). Depois de configurado, dá pra pedir coisas como: achar um Google Doc específico de memória vaga; ler e-mails não lidos e dar nota de prioridade baseada no contexto do negócio (e marcar como não-lido os de prioridade baixa automaticamente); criar apresentações no Google Slides com validação visual (o agente tira screenshot da própria criação, compara com a marca, e corrige espaçamento/erros sozinho, em loop).

### CLI/API/.env vs. Connectors (a decisão mais importante do módulo)
No app desktop existe "Connectors" (Customize → Connectors) — botão de "conectar" com OAuth simples (Notion, Gmail, Figma, Canva, Reead etc.) — muito mais fácil que configurar `.env`. MAS: conectores ficam presos ao harness específico (app desktop). Se você trocar de harness (Codex, Hermes, outro app, VS Code), perde TODAS as conexões e precisa reconectar manualmente. Já a abordagem manual (.env + chaves de API) é só pastas e arquivos — QUALQUER agente, harness ou IA consegue ler. Recomendação explícita do autor: prefira sempre `.env`/API mesmo sendo um pouco mais trabalhoso, porque **ser agnóstico de ferramenta é a coisa mais importante** dado que modelos e harnesses mudam toda semana.

### Skills — conceito central
Skills são "receitas" reutilizáveis em markdown (ex.: analogia de receita de panqueca de chocolate — na primeira vez você segue a receita ao pé da letra; depois de fazer e ajustar algumas vezes, você já sabe de cor e ajusta ao seu gosto). Front matter YAML no topo do arquivo (name + description) é o que permite **progressive disclosure**: o agente escaneia só a descrição de TODAS as skills disponíveis (evita gastar tokens lendo cada skill inteira) e decide qual usar baseado no pedido do usuário — só aí lê a skill completa e a executa. Skills podem chamar sub-agentes e outros scripts; podem ser tão simples quanto 2 frases ou tão complexas quanto dezenas de arquivos de referência.
Duas formas de criar uma skill: (1) proativa — "ajuda a construir uma skill pra X, passo 1... passo 2..." — ou (2) reativa — fazer o processo manualmente primeiro, e DEPOIS pedir "transforma isso que acabamos de fazer numa skill". Skills podem ser instaladas via slash command (`/skill-creator`) ou baixadas de repositórios open source de terceiros (ex.: skill "grilling"/"grill me" do Matt PoCoo — interroga você repetidamente sobre um plano/design até não sobrar buraco, salvando tudo num arquivo de brainstorm).
**Demo real de criação de skill "inbox triage"**: pedido inicial (bagunçado, várias frases) → Claude faz perguntas de esclarecimento (quantos e-mails processar por rodada? o que fazer ao rotular — Gmail labels, "apenas resumo sem rotular", estrelas?) → skill criada, com pasta de referências e scripts próprios. Depois de rodar, o usuário dá feedback ("esse terceiro e-mail podia ter ido pra 'ignorar', anota que remetentes assim sempre são ignoráveis") → Claude edita o `skill.md` sozinho, adicionando a regra permanentemente.

### Janela de contexto e "context rot"
Analogia: caminhoneiros só podem dirigir X horas antes de descansar porque o julgamento degrada com o cansaço — o mesmo vale para modelos: quanto mais a janela de contexto enche, pior fica a qualidade da resposta ("dumb zone"/"context rot"). No app desktop, o indicador de contexto mostra tokens usados de 1 milhão total, com breakdown (mensagens, ferramentas do sistema, skills, system prompt). Prática do autor: ao chegar perto de 250.000 tokens (~25% de 1M), fazer reset.
**Skill "session handoff"** (dada de graça pelo autor): antes de limpar a sessão, roda essa skill, que gera um resumo estruturado (onde começou, decisões travadas, o que foi entregue, arquivos-chave, estado atual, questões em aberto, "continue daqui"). Depois: copia esse resumo → `/clear` (zera a janela pra zero) → cola o resumo de volta → sessão nova continua exatamente de onde parou, mas com contexto muito mais enxuto. `/compact` do próprio Claude Code existe mas dispara tarde demais (perto de 95%) e demora mais — o handoff manual é preferível.
