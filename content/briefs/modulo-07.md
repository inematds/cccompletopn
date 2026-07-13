# Módulo 7 — Construção e publicação de projetos
Timestamps fonte: 2:48:07–3:35:22

## Tópicos do módulo
1. Construindo sites com Claude Code
2. Clonando e adaptando sites
3. Versionando projetos com GitHub
4. Publicando aplicações com Vercel
5. A pirâmide dos sistemas de IA
6. Como confiar nos resultados da IA
7. Como validar as entregas

## Conteúdo condensado

### Setup (VS Code)
Baixar VS Code (IDE gratuita) → extensão "Claude Code" → login com assinatura Anthropic (precisa Pro/Max) → abrir uma pasta vazia (Explorer → "Open Folder") → essa pasta vira o projeto de trabalho.

### 5 hacks para sites que não parecem "vibe coded"
0. **CLAUDE.md primeiro** (hack "número zero"): definir nele, por exemplo, "sempre invoque a skill de design front-end antes de escrever qualquer código front-end, sem exceções". Sem essa regra, o resultado tende a ficar genérico.
1. **Skill de design front-end** (instalável via comando simples, citada como "front-end design skill"): eleva demais a qualidade visual/animações com um prompt mínimo — exemplo real: um prompt de uma frase pediu "toca música player app" e saiu com animações e elementos dinâmicos muito acima do padrão sem a skill.
2. **Loop de screenshot**: usando Puppeteer (configurável pedindo pro Claude Code instalar), o agente tira screenshots da própria página enquanto constrói, compara com o que devia ficar, e ajusta sozinho — fazendo o trabalho subir de ~60% pronto pra ~90%+ sem tanta correção manual seguida seguida. Ressalva: para elementos ANIMADOS/dinâmicos, o screenshot estático pode confundir o agente e ele entra em loop de "não ficou bom" — nesse caso, avisar explicitamente "não use a ferramenta de screenshot pra comparar, é uma animação, só ajuste o código".
3. **Sites de inspiração**: usar Dribbble, Godly, Awwwards (ou outros) → tirar screenshot em página inteira (DevTools F12 → Ctrl+Shift+P → "screenshot" → full page) → copiar o CSS/estilo (aba Elements → Styles → copiar tudo) → dar pro Claude Code os dois (imagem + estilo copiado) e pedir "clone este site". Depois, pedir pra trocar cores/logo/copy pela marca própria (arquivo de brand assets — logo + guia de marca — dá pra arrastar direto pro chat ou referenciar com `@`).
4. **Componentes individuais**: sites tipo 21st.dev têm componentes prontos (botões, backgrounds animados, shaders) — copiar o snippet de código de um componente específico (ex.: fundo animado atrás do texto hero) e pedir pro Claude Code "trabalhar isso" no site existente, sem trocar o site inteiro.
Modo bypass permissions (Configurações → Claude Code → "allow dangerously skip permissions") acelera builds grandes sem ficar confirmando passo a passo — usar com responsabilidade, sem deixar rodando sozinho a noite toda sem supervisão.

### GitHub + Vercel (deploy)
Fluxo: Claude Code roda tudo LOCAL (localhost — só você acessa) → precisa sincronizar pro GitHub (versionamento, permite colaboração, guarda histórico) → conectar Vercel ao repositório do GitHub (auto-deploy: toda vez que o Claude Code faz push pro GitHub, a Vercel puxa a mudança e atualiza o site ao vivo automaticamente). Passo a passo real: criar repositório no GitHub → pedir pro Claude Code (`git push`, ele autentica sozinho na primeira vez) → conta na Vercel (idealmente logando com GitHub) → "Add New Project" → importar o repositório → deploy. Site fica com domínio feio tipo `projeto.vercel.app`; domínio próprio se configura em Project Settings → Domains. Boa prática: sempre testar mudanças em localhost antes de pedir pro Claude Code commitar/dar push — regra que pode virar instrução fixa no CLAUDE.md ("sempre testar em localhost até eu confirmar, só então commitar pro GitHub").

### A pirâmide dos sistemas de IA (determinístico → agente)
Eixo de baixo pra cima = mais autonomia, mais imprevisibilidade, mais custo, mais risco:
1. **Chatbot**: humano dispara manualmente (ex.: skills que você mesmo invoca) — baixo risco, você está sempre olhando.
2. **Workflow (determinístico)**: automação sem IA nenhuma, passos fixos, disparada por evento/agenda — ex.: mover dado de uma planilha pra um banco.
3. **AI Workflow**: mesma sequência fixa de passos 1→2→3→4→5, mas UM dos passos usa IA generativa (ex.: gerar o texto do e-mail) — a ordem é sempre previsível, só o conteúdo daquele passo varia.
4. **Agent**: não existe ordem fixa — o próprio agente decide quais ferramentas usar, em que ordem, quantas vezes, dentro de um loop de raciocínio — máxima autonomia, máxima imprevisibilidade.
Regra de ouro citada e repetida: **construa a solução mais simples possível para o problema**. Se não precisa de IA, não bote IA.
Analogia complementar: **vending machine vs. slot machine** (repetida do Módulo 1) — determinístico = mesma entrada, mesma saída sempre; agente = você "aposta" toda vez.

### Exemplo real comparando as duas abordagens (inbox agent)
O autor mostra dois designs pro MESMO problema (triagem/resposta de e-mail): (a) um AGENTE com system prompt gigante cheio de regras de negócio, várias ferramentas de e-mail, decidindo sozinho o que fazer — menos confiável, mais caro, mais difícil de debugar quando erra; (b) um WORKFLOW com regras de roteamento objetivas (se domínio de e-mail = X → rótulo interno; se = Y → rótulo billing + resumo + Slack; IA entra só no passo de EXTRAIR/RESUMIR informação) — teve praticamente zero falhas, mais barato, mais rápido, e quando algo dava errado dava pra rastrear exatamente onde.

### Permissão: camada de prompt vs. camada de ferramenta
Prompt ("nunca envie e-mail, só rascunhe") é só uma SUGESTÃO — pode falhar na centésima primeira vez mesmo tendo funcionado as cem anteriores. A camada REAL de segurança é: se a ferramenta de "enviar e-mail" nem existe no conjunto de ferramentas do agente, ele fisicamente não consegue enviar, não importa o que "decida". Mesma lógica pra deletar registros/banco de dados — não dê a ferramenta se a ação nunca deveria acontecer.
**Incidente real contado pelo autor**: uma automação (rotina na nuvem) rodou sozinha por 2 meses sem ele lembrar que existia — configurada pra mandar update semanal só num DM privado, mas depois de algumas semanas começou a postar em canais públicos do ClickUp por engano, porque tinha ACESSO a todos os canais (mesmo o prompt dizendo "só mande aqui"). Conclusão do próprio autor: "prompting não é uma camada de permissão."

### Analogia da bicicleta aplicada a automação (retomada)
Não solte a automação sozinha, sem supervisão, de primeira ("rodinha, capacete, segurando o guidão") — vá removendo supervisão aos poucos conforme ganha confiança testada, nunca de uma vez.

### Evals (validação antes de produção)
Conceito citado: ter um "golden dataset" (centenas de exemplos de entrada + saída esperada), rodar o sistema contra eles, medir taxa de acerto/erro, e SÓ DEPOIS colocar em produção ou trocar de modelo/prompt. Prática recomendada: usar linguagem natural pra pedir ao Claude Code "me ajuda a pensar em todos os casos extremos que podem dar errado aqui" antes de publicar uma automação — ele ajuda a desenhar os testes e os guardrails junto com você.
