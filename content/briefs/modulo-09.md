# Módulo 9 — Sistemas multiagentes e automações
Timestamps fonte: 4:31:50–5:27:13

## Tópicos do módulo
1. Subagentes vs. equipes de agentes
2. Diferenças entre delegação e colaboração
3. Coordenação entre vários agentes
4. Automações agendadas
5. Rotinas executadas na nuvem
6. Construindo uma automação de pesquisa
7. Busca automática de informações
8. Análise e organização de pesquisas

## Conteúdo condensado

### Sub-agentes vs. Agent Teams (diferença estrutural)
Sub-agentes: sessão principal delega, cada sub-agente tem janela fresca, reporta SÓ pra sessão principal — não conversam entre si (modelo "1 para muitos", sem comunicação lateral). **Agent teams**: sessão principal cria o time, existe uma LISTA DE TAREFAS COMPARTILHADA, e os "colegas de time" conseguem se comunicar entre si e reivindicar tarefas da lista — é colaboração real, não delegação isolada. Trade-off: agent teams consomem muito mais tokens/limite de sessão que sub-agentes — usar com moderação, só quando o valor de múltiplas perspectivas debatendo entre si compensa o custo.
Feature experimental (precisa habilitar manualmente via `settings.json` / `settings.local.json`: variável de ambiente `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`, e reiniciar a sessão depois de configurar).

### Caso de uso principal do autor pra agent teams: "conselho"/"war room"
Quando quer múltiplas perspectivas debatendo um plano, ideia ou decisão importante — monta um time com personas diferentes (ex.: dono de pequena empresa, dono de grande empresa, CEO, funcionário júnior) discutindo um relatório/dado real, com várias rodadas de debate até chegar a um veredito coletivo. Base científica citada: método STORM de Stanford — pesquisa com múltiplas personas atacando o mesmo ângulo produz pesquisa mais completa e rigorosa que uma perspectiva única.
Lição prática de debug real: ao pedir "cria um time de agentes", o Claude por padrão pode confundir e só disparar sub-agentes paralelos comuns (que não conversam entre si) em vez de um Agent Team de verdade — é preciso ser EXPLÍCITO ("use a função team_create, não sub-agentes paralelos") e observar o comportamento pra corrigir.

### Artifacts (compartilhamento)
Depois de um debate/pesquisa longa, dá pra pedir "empacota isso num artifact compartilhável" — gera uma página HTML polida hospedada pela própria Anthropic (URL real, não localhost), que atualiza automaticamente se você pedir mudanças depois (sem precisar reenviar um novo arquivo pra quem recebeu o link).

### Rotinas (Routines) — automações que rodam na nuvem
Diferente de scheduled tasks locais (que exigem o computador ligado): **routines** rodam na infraestrutura web da própria Anthropic — não precisa deixar nada ligado. Configuração: nome, prompt (instrução única, tem que ser "one-shot" — ninguém vai estar lá pra responder perguntas de esclarecimento durante a execução), modelo, **repositório GitHub** (a rotina clona seu repo pra rodar), ambiente cloud (variáveis de ambiente = onde ficam as chaves de API, já que o `.env` normalmente é ignorado no Git por segurança — tem que colar as chaves manualmente nas env vars do ambiente cloud), cadência (mínimo 1x por hora), e nível de acesso de rede (**trusted** = só domínios verificados pela Anthropic; **full** = qualquer domínio, mais risco de exfiltração se o conteúdo lido for malicioso; **custom** = lista específica).
Limites por plano (na época do vídeo): Pro = 5 rotinas/dia; Max = 15/dia; Team/Enterprise = 25/dia. Cada execução roda em ambiente com 4 vCPUs, 16GB RAM, 30GB disco — depois é destruído (efêmero), exceto: mudanças de código viram branch/PR no GitHub, e o histórico de execuções (logs) fica salvo pra auditoria.
**Regra prática**: se algo é local (cookies de navegador, sessão salva) ou não está acessível via GitHub/API, a rotina NÃO CONSEGUE usar — só o que está no repositório clonado ou acessível externamente.

### Pirâmide de IA aplicada à automação em nuvem (retomada do Módulo 7)
Demo de automação **cron-based** (agendada, ex.: briefing diário 6h): desenhada deliberadamente como AI WORKFLOW (não agente livre) — passos fixos: pesquisar (Tavily) → escrever resumo (IA, via Open Router pra ter flexibilidade entre modelos) → enviar (ClickUp DM). Hardening real de segurança: o script foi travado no código pra só poder mandar pra UM DM privado específico (nunca canais públicos) — o autor testou e validou isso ANTES de ativar a automação de verdade, exatamente pra não repetir o incidente do Módulo 7 (mensagem indo parar em canal errado).
Deploy feito no **Modal** (modal.com — plataforma de deploy que só cobra por execução, centavos por rodada; recomendação de usar Open Router como camada de API pra não depender de uma única conta/fornecedor de IA). Observabilidade real: painel do Modal mostra execuções, logs, erros, permite disparar manualmente ("run now"), e guarda os segredos/chaves de API de forma centralizada e editável.

### Automação webhook-based (evento, não agenda)
Analogia: **campainha vs. checar a porta de 10 em 10 minutos (polling)**. Webhook = sistema A avisa sistema B só quando algo acontece de fato (ex.: alguém envia um formulário) — muito mais eficiente que ficar checando em loop. Demo real: formulário HTML simples de captura de lead → submissão dispara requisição pro endpoint webhook no Modal → script recebe os dados e manda notificação formatada pro ClickUp. Ponto pedagógico importante: essa automação usa **ZERO IA** de propósito — é só template preenchendo variáveis, porque IA ali só adicionaria custo e risco (ex.: "corrigir" um erro de digitação do usuário) sem nenhum ganho real. Reforça a regra da pirâmide: construa a solução mais simples que resolve.

### Remote control (bônus, controlar sessão pelo celular)
Comando `/remote control` numa sessão ativa expõe aquela sessão pro app do Claude no celular (mesma conta) — permite continuar mandando mensagens e até slash commands de longe (útil pra quando vai sair de casa/academia e quer continuar acompanhando/dirigindo o trabalho).
