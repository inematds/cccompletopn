# Módulo 5 — Memória e segundo cérebro
Timestamps fonte: 2:03:17–2:20:23

## Tópicos do módulo
1. Memória da Inteligência Artificial
2. Construção de um segundo cérebro
3. Restrições e regras
4. Definição de métricas
5. Mapeamento de processos
6. Transformando atividades em processos executáveis

## Conteúdo condensado

### Automemory
Claude Code tem uma feature de memória automática: depois de um certo número de sessões/tempo, ele escreve sozinho fatos aprendidos numa pasta global de memória — no exemplo real do autor, 72 arquivos markdown individuais, um fato por arquivo (ex.: `feedback-noel-hooks.md`, `reference-corporate-structure.md`). Cada arquivo tem front matter com tags (`user`, `feedback`, `project`, `reference`) e um arquivo índice `memory.md`. Ativar/verificar: comando `/memory` no terminal (não disponível no app desktop) mostra on/off — vem ligado por padrão.
Diferença de camadas: CLAUDE.md = **regras** (o que sempre fazer); memory.md = **fatos aprendidos** (preferências corrigidas, decisões, referências); CLAUDE.md funciona como **roteador** que aponta pra onde mais procurar (memória, wiki, projetos) quando o agente não sabe algo de cara.

### Segundo cérebro (conceito introdutório — aprofundado no Módulo 8)
Ideia central de Andrej Karpathy (post viral no X) sobre "LLM knowledge bases": dar para o LLM documentos organizados em markdown simples (sem banco vetorial, sem embeddings, sem infraestrutura complexa) — o próprio LLM mantém índices e resumos e consegue ler os dados relacionados com eficiência, mesmo em escala de ~100 artigos / meio milhão de palavras. Resultado citado por um usuário do X: transformar 383 arquivos soltos + 100+ transcrições de reunião numa wiki compacta reduziu uso de tokens em 95% nas consultas.

### Teoria das restrições (constraint theory) — aplicada a "o que automatizar"
Analogia central do módulo: **o cano (pipe)**. Água entra na frente (clientes/demanda), atravessa o negócio, sai do outro lado (lucro). Nesse cano existem "entupimentos" (constraints) de tamanhos e prioridades diferentes — são eles que limitam o quanto passa. Regra: trabalhe SEMPRE de frente pra trás — pergunte "se amanhã eu tivesse 5x mais clientes, o que quebraria primeiro?" — isso força mapear o processo real e achar o gargalo mais urgente. Depois de destravar um gargalo, mais água passa e o PRÓXIMO gargalo aparece — é um ciclo sem fim, mas dá um roteiro claro de por onde começar. Frase-chave: "a restrição é o único lugar onde o trabalho compõe" — automatizar processos que não são o gargalo é "ficar ocupado", não crescer.

### North star / métrica antes de construir
Regra: TODO projeto/automação precisa de UMA métrica definida ANTES de começar a construir, que todo mundo concorda que prova sucesso. Exemplo do cano: gargalo identificado = poucos leads entrando (5 leads/semana) → north star = 15 leads/semana. Sem esse alinhamento prévio, ninguém sabe se o ROI aconteceu — produtividade de automação não é tão direta de medir quanto "gastei X em anúncios, entrou Y de receita".

### Quando você ainda não tem escala pra pensar em "constraints" formais
Prática simples: literalmente escrever num papel — que processos/ferramentas uso toda semana? o que se repete? quais são meus "gatilhos" (trigger events — lead chega, ticket de suporte abre, o que eu faço)? Isso revela ~5-10 processos candidatos; escolher o que acontece com mais frequência (a menos que seja alto risco demais pra mudar agora).

### "Você não consegue automatizar o que não consegue mapear"
Não importa quão bom o modelo/harness seja — sem o entendimento profundo do processo (expertise de domínio) alimentado no sistema, a IA não faz nada significativo com os dados do seu negócio. Frases citadas: "garbage in, garbage out", "contexto é rei", e a citação de Abraham Lincoln — "se eu tivesse 6 horas pra derrubar uma árvore, passaria as primeiras 4 afiando o machado" — ou seja: o tempo gasto mapeando e planejando ANTES de automatizar vale muito mais do que pular direto pra execução.
