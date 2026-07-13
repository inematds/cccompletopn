# Módulo 10 — Otimização e encerramento
Timestamps fonte: 5:27:13–5:58:20

## Tópicos do módulo
1. Gerenciamento de tokens
2. Redução de custos
3. Cache de prompts
4. Reaproveitamento de contexto
5. Otimização da execução
6. Considerações finais
7. Próximos passos para se tornar AI Native

## Conteúdo condensado

### Como o custo de tokens realmente cresce (fato-chave do módulo)
Toda vez que você manda uma mensagem, Claude **relê a conversa inteira desde o início** — não é custo linear, é CUMULATIVO/exponencial: mensagem 1 pode custar 500 tokens; mensagem 30 pode custar 15.500 (31x mais), porque está relendo tudo que veio antes. Dado citado: um desenvolvedor mediu uma sessão de 100+ mensagens e viu que 98,5% de todos os tokens gastos eram só RELENDO histórico antigo, não processando coisa nova. Além disso, CADA turno recarrega invisivelmente: CLAUDE.md, servidores MCP conectados, system prompt, skills — overhead "fantasma" que também dreninha tokens sem aparecer óbvio. E além do custo: contexto inchado piora a QUALIDADE da resposta ("loss in the middle" — o modelo presta mais atenção no início e no fim da janela; o meio de sessões longas tende a ser ignorado).

### 18 hacks de gestão de tokens (organizados em 3 níveis)

**Nível 1 (básico, todo mundo deveria fazer):**
1. `/clear` entre tarefas não relacionadas — não carregue contexto do assunto A pra uma conversa sobre assunto B.
2. Desconectar servidores MCP que não estão em uso (cada um carrega definições de ferramentas em TODA mensagem — um MCP sozinho pode custar ~18.000 tokens por mensagem; prefira CLI quando existir, é mais barato).
3. Juntar pedidos numa mensagem só em vez de 3 mensagens separadas (3 mensagens custam 3x mais pelo efeito cumulativo) — e se algo saiu errado, EDITAR a mensagem original é mais barato que mandar uma correção nova (edição substitui o trecho ruim; mensagem nova empilha em cima).
4. **Plan mode** antes de qualquer tarefa real — deixa Claude mapear a abordagem e fazer perguntas antes de gastar tokens indo pro caminho errado. Regra sugerida pro CLAUDE.md: "não faça mudanças até ter 95% de confiança no que precisa construir — pergunte até chegar lá."
5. Rodar `/context` (mostra exatamente onde os tokens estão indo: histórico, overhead de MCP, arquivos carregados) e `/cost` (gasto estimado da sessão) — tornar visível o que é invisível.
6. Configurar uma **status line** (mostra modelo ativo, % da janela de contexto usada, tokens — só disponível no terminal, não no app desktop).
7. Manter o painel de uso/limite aberto numa aba, checando periodicamente.
8. Ser seletivo ao colar documentos grandes — colar só a seção/função relevante, não o arquivo inteiro, quando possível.
9. **Observar o agente trabalhando** em tarefas longas em vez de sair da aba — se ele estiver indo pro caminho errado (loop, relendo os mesmos arquivos), interromper na hora custa muito menos que deixar terminar errado e refazer tudo.

**Nível 2 (intermediário):**
1. Manter o CLAUDE.md **enxuto** (menos de ~200 linhas) — ele é relido em TODA mensagem, mesmo um simples "oi". Trate-o como ÍNDICE que aponta pra onde mais informação vive, não como enciclopédia.
2. Ser **cirúrgico** com referências de arquivo — apontar a função/arquivo exato (`@arquivo`) em vez de "procura no repositório inteiro".
3. Rodar `/compact` por volta de 60% de uso de contexto (não esperar os 95% do auto-compact automático) — e depois de 3-4 compacts seguidos, a qualidade já degradou o suficiente pra valer mais a pena um handoff + `/clear`.
4. Cuidado com pausas curtas: o cache de prompt (explicado abaixo) expira; voltar depois de mais de 1 hora sem interagir reprocessa TUDO do zero, do preço cheio.
5. Cuidado com output de comandos grandes (ex.: um `git log` retornando 200 commits) — tudo isso entra na janela de contexto e é cobrado; negar permissão de comandos desnecessários no `settings.json` do projeto ajuda.

**Nível 3 (avançado):**
1. Escolher o modelo certo pra cada tarefa — Sonnet como padrão de trabalho, Haiku pra sub-agentes/formatação/tarefas simples, Opus só pra planejamento arquitetural profundo (manter Opus abaixo de ~20% do uso total). Dica extra: usar Codex (outra ferramenta) só pra REVISAR um código grande já construído no Claude, economizando tokens do Claude nessa etapa.
2. Sub-agentes custam ~7-10x mais tokens que uma sessão normal (acordam com contexto cheio do zero) — ainda assim compensam quando usados pra delegar tarefas pontuais/paralelas, especialmente em modelo mais barato.
3. Entender "horário de pico": consumo de sessão drena mais rápido em horário de pico (citado: 8h–14h horário do leste dos EUA, dias de semana) — deixar refatorações grandes/multi-agente pra fora do pico. Se está perto do reset do limite e ainda tem "sobra", vale acelerar o uso pra aproveitar; se está no início de uma nova janela mas perto do limite, vale pausar e voltar com orçamento cheio em vez de gastar os últimos % numa tarefa que vai ficar pela metade.
4. CLAUDE.md como "constituição do sistema" — guardar decisões arquiteturais estáveis e regras permanentes ali, nunca reexplicar a mesma coisa duas vezes. Sugestão de bloco de "aprendizado aplicado" no fim do CLAUDE.md: toda vez que algo falha repetidamente ou vira um workaround conhecido, adicionar 1 linha curta (menos de 15 palavras) — mas revisar esse bloco periodicamente pra não inchar.

### Cache de prompt (o mecanismo por trás de boa parte da economia)
Tokens em cache custam só **10% do preço normal de entrada**. Na assinatura do Claude Code, a janela de cache (TTL — time to live) é de **1 hora**: se você fica mais de 1h sem mandar mensagem, tudo desconecta do cache e a PRÓXIMA mensagem reprocessa a sessão inteira do zero, no preço cheio. Via API (fora da assinatura, "extra usage"), o padrão é só **5 minutos** (dá pra configurar pra 1h, com custo maior) — e sub-agentes, em qualquer plano, usam TTL de 5 minutos. O que QUEBRA o cache: trocar de modelo no meio da sessão (inclusive o modo "Opus pra planejar + Sonnet pra executar" conta como troca de modelo, reiniciando o cache a cada alternância — trade-off consciente entre economia de tokens e disciplina de cache). Editar o CLAUDE.md NÃO quebra o cache imediatamente (só aplica quando a sessão reinicia).
3 hábitos que cobrem a maior parte dos casos: (1) não deixar uma sessão pausada por mais de 1h — se precisar, feche com handoff; (2) `/clear` ou `/compact` sempre que trocar de assunto; (3) documentos grandes recorrentes (Claude.ai chat comum) — usar "Projects" em vez de colar em conversas soltas, porque projects têm cache mais otimizado pra arquivos anexados.

### Mentalidade final sobre "bater no limite"
Reformulação proposta pelo autor: bater no limite de sessão/semana NÃO é necessariamente ruim — se você está seguindo as boas práticas de token e AINDA ASSIM bate no limite, é sinal de que está usando a ferramenta pra valer, extraindo o valor real da assinatura, não desperdiçando. O problema não é "preciso de um plano maior", é geralmente "higiene de contexto" — parar de reenviar a mesma conversa inteira 30 vezes quando podia mandar 5 vezes mais enxutas.

### Encerramento / mensagem final do curso
O curso cobriu fundamentos + estrutura de projeto + segundo cérebro + sub-agentes + construção/publicação + automações + gestão de tokens — a base para "se tornar AI native": não é sobre saber mais, é sobre pra onde a mão vai por reflexo quando surge uma tarefa. Convite final: continuar praticando, construindo o próprio "AI Operating System" (4 Cs: contexto, conexões, capacidades, cadência) e tratando toda essa jornada como iterativa e nunca "terminada" — cada semana, mais um pedaço do seu conhecimento vira sistema.
