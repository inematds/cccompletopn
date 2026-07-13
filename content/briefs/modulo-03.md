# Módulo 3 — Estruturação de projetos
Timestamps fonte: 48:10–1:20:31

## Tópicos do módulo
1. Configurando um projeto
2. Utilizando o arquivo claude.md
3. Configurando o settings.json
4. Conectando ferramentas externas
5. Utilizando chaves de API
6. Configurando o arquivo .env
7. Modos de permissão
8. Chaves de API com escopo limitado
9. Privacidade e segurança

## Conteúdo condensado

### O arquivo CLAUDE.md
É o "system prompt" do agente — lido automaticamente ANTES de processar qualquer mensagem sua, toda vez. Fluxo: você digita "oi" → Claude lê o CLAUDE.md inteiro (contexto do negócio, projetos ativos, avisos) → só depois responde já contextualizado (ex.: "Ei Nate, pronto quando você estiver. Alguns lembretes: evento X essa sexta, projeto Y abre dia tal..."). Estrutura típica de um CLAUDE.md real (exemplo "Herk 2"): papel do agente ("você é o assistente executivo do Nate, seu trabalho é..."), um **mapa de roteamento** — "se precisar de algo sobre X, o caminho é Y" — para cada domínio (negócio/equipe, estrutura corporativa/jurídico, voz e estilo, conhecimento de curso, projetos). CLAUDE.md deve ser tratado como um **roteador**, não uma enciclopédia: ele diz onde procurar, não guarda tudo.
Loop de melhoria: você lê um output ruim da IA → usa seu julgamento pra apontar o que faltou (ex.: "só usou 4 fontes", "tem m-dash demais") → pede pra Claude **atualizar o próprio CLAUDE.md** com essa instrução → da próxima vez sai melhor. É assim que o sistema fica mais inteligente com o uso.
MD = markdown (linguagem simples de formatação com # para títulos, - para listas). Outros formatos comuns: .py (Python), .txt (texto puro) — a extensão do arquivo indica o tipo.

### Global vs. Projeto
Existem dois níveis de CLAUDE.md (e também de skills e sub-agentes):
- **Global** (ex.: `C:\Users\<user>\.claude\claude.md` no Windows): lido em QUALQUER projeto na máquina. Bom para preferências pessoais universais — ex.: uma "lista negra de frases de IA" (expressões que soam robóticas e nunca devem aparecer em textos publicados), coisas que a IA aprendeu sobre você ao longo do tempo.
- **Projeto** (`.claude/claude.md` dentro da pasta do projeto): só vale ali. Quando você abre uma pasta nova no Claude Code, ele já pergunta se confia no diretório (Claude pode ler/escrever/executar arquivos ali — só prossiga se confiar).
Ao criar um projeto novo, o próprio Claude Code pode inicializar a estrutura: basta pedir "inicializa com um CLAUDE.md e uma pasta .claude".

### A pasta .claude (settings, agents, skills)
Três coisas mais importantes dentro de `.claude/`:
- **settings.json**: permissões e preferências (o que o agente PODE e NÃO PODE fazer) — ex.: bash permitido, web search permitido, mas "remover"/"deletar" negado explicitamente para diretórios sensíveis. Você não precisa saber escrever JSON — descreve em linguagem natural o que quer permitir/proibir e o Claude edita o arquivo.
- **agents/**: sub-agentes customizados (aprofundado no Módulo 6).
- **skills/**: skills empacotadas em markdown, carregadas automaticamente quando relevante (aprofundado no Módulo 4).

### Conectando ferramentas: API, .env e chaves
**API** (Application Programming Interface) = método de um software falar com outro (ex.: Gmail "conversando" com Claude Code). Chave de API = uma senha que dá acesso aos SEUS dados privados numa plataforma (ex.: sem a chave do YouTube, Claude só acessa dados públicos de qualquer canal, não as métricas privadas do seu).
Demo real: conectar a API da **Tavily** (busca web melhor pra agentes, primeiros 1.000 créditos grátis). Passo a passo: pega a chave no site da Tavily → pede pro Claude Code criar um arquivo `.env` no projeto (arquivo seguro para segredos, NUNCA vai pro GitHub — o próprio Claude já cria um `.gitignore` pra garantir isso) → cola a chave lá → pede pra Claude testar a chamada. Claude sozinho pesquisa o endpoint certo da Tavily e faz a chamada de teste — sem você precisar saber a documentação técnica.
Teste de robustez: o autor trocou a chave por uma inválida de propósito → Claude tentou, recebeu erro 400, e (sem instrução extra) caiu de volta pra busca nativa do Claude em vez de avisar do erro — lição: seja explícito ("não use a ferramenta nativa de busca, use a Tavily") senão o agente pode silenciosamente usar um caminho alternativo pior. Depois de corrigir a chave, pediu pra Claude **salvar o endpoint e o método de autenticação no CLAUDE.md** — assim da próxima vez ele não precisa "redescobrir" a API, economizando tempo e tokens.
**MCP** (Model Context Protocol) é citado brevemente: parecido com API mas com "ferramentas" em vez de "endpoints" — conecta a QuickBooks, Google Sheets, Gmail etc.

### Modos de permissão
No app: **auto mode** (padrão — bom para a maioria), **manual permissions**, **accept edits**, **plan mode**, e **bypass permissions** ("dangerously skip permissions" — precisa ativar em Configurações → Claude Code → "allow bypass permissions mode"). Bypass faz o agente nunca parar para perguntar "posso fazer isso?" — use com cautela. História de horror citada: um agente do próprio time do autor mandou e-mail com cupom de desconto para 150.000 pessoas sem deveria — não foi o "modo de permissão" que falhou, foi o agente TER ACESSO à ferramenta de enviar e-mail quando só devia ter a de rascunhar. Regra de ouro: a camada de permissão real são as FERRAMENTAS que o agente tem acesso, não só o prompt dizendo "não faça isso" (aprofundado no Módulo 7).

### Chaves de API com escopo limitado (scoped keys)
Demo com ElevenLabs: ao criar uma chave nova, dá pra restringir por (1) limite de crédito/gasto e (2) endpoints/capacidades específicas — ex.: uma chave que só pode gerar efeitos sonoros, sem acesso a dublagem, agentes, etc. Analogia: "você daria um cartão de crédito sem limite pra um funcionário novo? Não. Trate chaves de API assim." Pense sempre: "se meu agente PUDER tocar nesse dado, assuma que ele VAI tocar — mesmo sem eu pedir."

### Privacidade e dados
Depende do plano: em planos de consumidor (Free/Pro/Max, não Team), a partir de 8 de outubro (mudança de política citada), treinar com seus dados é opt-in — mas é uma "escolha forçada" (você precisa ativamente escolher sim/não). Configuração: Configurações → Privacidade → "Help improve Claude" (liga/desliga). Mesmo desligado, a Anthropic ainda TEM seus dados (só não treina com eles) — verifique as políticas da sua empresa antes de colar documentos sensíveis/contratos/dados de clientes. Regra prática do autor: nunca envia dados de cartão de crédito ou PII de clientes; para clientes com exigências rígidas, usa deployments on-premise com modelos open source (tema para vídeos futuros, fora do escopo deste curso).
