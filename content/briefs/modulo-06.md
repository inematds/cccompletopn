# Módulo 6 — Subagentes
Timestamps fonte: 2:20:23–2:48:07

## Tópicos do módulo
1. O que são subagentes
2. Como os subagentes funcionam
3. Construindo um subagente
4. Criando o Plan Roaster
5. Quando usar subagentes
6. Quando não usar subagentes
7. Instalação pelo terminal
8. Recursos gratuitos para continuar aprendendo

## Conteúdo condensado

### Conceito e demo de abertura
Sessão principal = "orquestradora": só ela fala com você; ela delega tarefas a sub-agentes, que só respondem pra ela. Demo real: pedir 5 sub-agentes com personas diferentes (iniciante completo, engenheiro de software, dono de negócio, editora) pra revisar os capítulos de um livro em paralelo, cada um com lente distinta — cada sub-agente roda numa sessão/janela de contexto totalmente separada e fresca.

### Por que sub-agentes existem (3 motivos)
1. **Mantêm o contexto principal limpo**: pesquisa/leitura extensa que "polui" a sessão principal pode ser delegada — o sub-agente processa tudo numa janela fresca e só devolve o resumo final.
2. **Modelos diferentes por custo**: a sessão principal pode rodar em Opus (caro) e delegar pesquisa/tarefas simples pra sub-agentes em Haiku/Sonnet (baratos) — "chefe inteligente com um time de trabalhadores baratos".
3. **Paralelismo**: vários sub-agentes rodando ao mesmo tempo, cada um com sua própria janela de contexto.

### Built-in vs. custom
Sub-agentes "general purpose" (built-in, sem YAML customizado) vs. sub-agentes CUSTOM — um arquivo markdown dentro de `.claude/agents/`, estruturalmente idêntico a uma skill: front matter YAML (name, description, model, color, tools permitidos/negados, MCPs permitidos) + corpo com instruções. A **description** é o gatilho de disparo automático (progressive disclosure) — quanto mais precisa, menos "misfires" (disparar quando não devia, ou não disparar quando devia).

### Demo: construindo o "Plan Roaster" (agente advogado do diabo)
Criado via slash command de criação de agente (`/agents` → novo → "gerar com Claude" ou config manual): descrição do que faz ("crítica adversarial de plano/ideia/estratégia — dispara com frases como 'roast my plan' ou 'review my plan'"), tools = **somente leitura** (read-only — não deixa esse agente editar nada), modelo = Haiku (barato, tarefa simples), cor = rosa (identifica visualmente quando está ativo), memória = escopo "project". Lição real de debug: a primeira tentativa de disparo não ativou o Plan Roaster (usou uma skill "roast" já existente em vez do sub-agente) — o autor pediu pro Claude ler a própria description do agente e explicar por que não disparou; causa raiz descoberta = erro mecânico (front matter YAML sem aspas fechadas, quebrando o parsing) — corrigido, e depois ainda havia sobreposição de gatilho com a skill "roast" existente, resolvida sendo explícito no prompt ("use o sub-agente Plan Roaster, não a skill roast").

### Sub-agentes vs. skills (diferença central)
Skills e sub-agentes são estruturalmente quase idênticos (markdown + YAML front matter) — a diferença real é: sub-agente roda em **janela de contexto isolada e fresca**, permite **paralelismo real**, e permite **modelo diferente** da sessão principal. Skill normalmente é algo que você dispara DENTRO da própria sessão principal (mas skill pode chamar sub-agente, e sub-agente pode chamar skill — eles se compõem).

### Global vs. projeto (sub-agentes)
Mesma lógica de CLAUDE.md/skills: sub-agentes em `.claude/agents/` do projeto só existem ali; sub-agentes na pasta global do usuário funcionam em qualquer projeto na máquina. Repositórios open source de sub-agentes prontos existem (ex.: "awesome-claude-code-subagents" no GitHub) — cuidado ao baixar: revisar por prompt injection/conteúdo malicioso antes de usar (pode-se até usar um sub-agente read-only dedicado só pra auditar esses arquivos baixados).

### Quando usar sub-agentes (sinais práticos)
- Vai ler muitos arquivos ou gerar uma "parede" de output que você nunca vai reler → delega.
- Tarefa que se repete → vira sub-agente customizado permanente.
- Trabalho independente/paralelizável (ex.: 15 capítulos de um livro, todos revisáveis ao mesmo tempo, sem ordem).
- Quer um revisor "sem viés" (sub-agente acorda sem contexto/memória prévia — visão honesta).

### Quando NÃO usar
- Edição rápida e pontual.
- Passos que dependem uns dos outros em sequência (1→2→3→4) — nesse caso é mais "time de agentes"/orquestração, não sub-agente isolado (sub-agentes NÃO conversam entre si, só reportam pra sessão principal — 1 para 1, nunca 1 para muitos).
- Quando o sub-agente precisaria do contexto completo da conversa principal ou precisasse te fazer uma pergunta no meio (sub-agentes não interagem com você diretamente).

### Nota sobre "dynamic workflows" (feature mais avançada, citada rapidamente)
Desde o Opus 4.8, existe disparo automático de múltiplos sub-agentes em paralelo por fases (workflows dinâmicos) — pode escalar de poucos a **dezenas ou centenas** de sub-agentes numa tarefa grande. Isso consome MUITO limite de sessão rapidamente (exemplo citado: 210 sub-agentes numa única rodada de teste). Depois de reports de uso excessivo, a Anthropic mudou a palavra-gatilho de "workflow" para "ultra code" para evitar disparos acidentais. Use com cautela e de olho no limite.

### Resumo prático (mapa mental do módulo)
- Uma coisa rápida → não precisa de sub-agente.
- Compartilhar com o time → guarda no repositório do projeto.
- Só pra você, em qualquer projeto → guarda na pasta pessoal/global.
- Quer economizar → 1 líder caro + vários trabalhadores baratos.
- Quer qualidade → deixa um agente fresco revisar ou paralelizar o trabalho.
- Job gigante em paralelo → considerar dynamic workflow, de olho no limite de sessão.
