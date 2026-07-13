# Módulo 1 — Fundamentos e mentalidade
Timestamps fonte: 00:00–31:44

## Tópicos do módulo (usar como estrutura de seções/aulas)
1. Introdução: do iniciante ao AI Native
2. O que é Claude Code: Chat, Cowork e Code
3. Modelos vs. contexto: a analogia central
4. As 12 mudanças de mentalidade
5. Engenharia de prompts vs. engenharia de contexto
6. Repertório, iteração e construção do seu Jarvis
7. Uma paixão, muitas ramificações

## Conteúdo condensado (fatos, exemplos e citações a preservar)

### Quem é o narrador / premissa do curso
Nate Herk, sem background técnico, construiu múltiplos negócios (conteúdo, educação, certificações, eventos, consultoria) todos rodando com um time pequeno super eficiente em IA. Tese central: "uma pessoa pode fazer o trabalho que antes levava um time inteiro". O curso assume zero conhecimento técnico prévio.

### Os 3 produtos da Anthropic (Claude)
- **Claude Chat**: chatbot simples, web/telefone — não é onde se faz trabalho de verdade.
- **Claude Co-work**: camada intermediária, interface simples para automações — foi construído inteiramente usando Claude Code por poucos engenheiros em ~1 semana (o que levaria semanas/time maior).
- **Claude Code**: o mais poderoso. Não precisa saber programar. Acessa arquivos locais E qualquer coisa online (Gmail, Slack, CRM). Roda em cima dos mesmos modelos (Sonnet, Opus, Haiku, Fable) — a diferença é o "harness" (arcabouço) que dá acesso a ferramentas (busca web, fetch, arquivos locais).

### Analogia central: Modelo + Harness + Você
Três camadas concêntricas:
- **Núcleo**: o modelo de IA (Opus, Fable, GPT) — o "motor".
- **Harness**: Claude Code — o "carro" que usa o motor.
- **Você**: o motorista — seu cérebro, prompts, contexto, dados, negócio.
O motor sozinho não anda; precisa do carro (harness) e do motorista (você) dirigindo. Modelo e harness são intercambiáveis com o tempo, mas o mais importante do quadro é **você** — sem o contexto certo, a IA não faz a coisa certa.

### As 6 habilidades de IA que blindam qualquer carreira
1. **Ser "a pessoa de IA"** — é relativo: você só precisa saber mais que seu círculo. Dado IBM 2026 CEO Study: 85% dos CEOs dizem que todo líder funcional precisa virar especialista técnico em seu domínio (não só CTO/TI). Prática: escolha UMA ferramenta principal (o autor usa Claude), pegue UM workflow do seu trabalho atual, meça antes/depois.
2. **Repertório e julgamento (taste)** — quanto melhor a IA fica, mais fácil é confiar cegamente no primeiro output — essa é a armadilha. Piada: um lado usa IA pra transformar bullet point em e-mail formal; o outro lado usa IA pra transformar esse e-mail de volta em bullet point. Sinal de alerta: m-dashes em excesso (o autor nunca digitou um m-dash na vida — texto com muitos m-dashes denuncia IA não revisada). Como treinar: estude os melhores exemplos da sua área, salve uma biblioteca do que funciona, pergunte à IA "por que isso ficou bom?", e toda vez que corrigir algo, alimente a correção de volta nas instruções. Frase-chave: "taste é decidir o que merece seu nome" — o que você publica com IA carrega sua assinatura, para o bem e para o mal.
3. **Engenharia de contexto** (ver seção dedicada abaixo).
4. **Velocidade de iteração** — analogia de ensinar uma criança a andar de bicicleta: você não solta a criança sozinha; segura, corrige o peso, solta aos poucos. Raramente se acerta de primeira com IA — você itera. Depois de ensinar o 15º "filho" (agente/skill), o processo já virou ciência. Hack prático: usar ditado por voz em vez de digitar (ferramenta citada: Glido). A outra metade da habilidade é saber **quando parar de iterar**: definir uma "north star" (métrica de negócio) ANTES de começar — ex.: suporte → tickets resolvidos/dia; vendas → reuniões qualificadas/semana; ops → % de reembolso caindo X%.
5. **Construir seu próprio Jarvis** — diferença entre automação disparada por você (chatbot) e sistema que dispara sozinho (agente). Analogia vending machine vs slot machine: vending machine = determinístico (mesma moeda, mesmo resultado sempre); slot machine = não-determinístico (você "aposta" toda vez que fala com IA generativa). Regra: use IA (slot machine) só quando precisa de raciocínio/variabilidade; use workflow simples (vending machine) quando o processo é previsível — é mais barato, mais rápido, quase nunca quebra. Perguntas antes de automatizar algo: (1) preciso ser eu a disparar isso ou o sistema pode disparar sozinho? (2) esse passo precisa mesmo de IA ou um script simples resolve mais barato?
6. **"Seguro-desemprego" (unemployment insurance)** — construir múltiplas fontes de renda com IA para não depender de um único empregador/cliente. Modelo recomendado: não 5 fontes em domínios diferentes (queima e quebra), mas "uma paixão, múltiplos ramos" — mesmo expertise, embalagens diferentes (curso, newsletter, consultoria, microsass). Caveats: cuidado com contrato de trabalho/non-compete, seja transparente com o empregador. Tática default: build in public — compartilhar o que está aprendendo/construindo torna você "descobrível" (inclusive por IA quando alguém pesquisa sobre o tema).

### Engenharia de prompt vs. engenharia de contexto
Prompt engineering (dar papel, instruções claras, exemplos) está ficando menos crítico porque os modelos melhoram sozinhos. Citação de Andrej Karpathy (que entrou na Anthropic): context engineering é "a arte e ciência delicada de preencher a janela de contexto com exatamente a informação certa". Tradução: prompt é *como você pergunta*; contexto é *o que a IA sabe*. Contexto é mais durável que prompt porque, não importa quão bom o modelo fique, ele ainda precisa saber o que está na sua cabeça (negócio, calendário, prioridades). Prática: pare de abrir chat em branco — crie um "Claude Project" e alimente com dados reais (calendário de campanha, copy que funcionou, copy que flopou). Analogia: um estagiário novo precisa de onboarding (o que a empresa faz, quem é quem, projetos atuais) antes de contribuir de verdade — IA sem contexto é "um estagiário inteligente advinhando". Regra: garbage in, garbage out — contexto é dado não-público, é o que torna o output único (se todo mundo usa o mesmo modelo com o mesmo prompt genérico, todo output fica igual).

### Mentalidade (12 mudanças, destaques usados no curso)
- **AI native não é o quanto você sabe, é pra onde sua mão vai por reflexo**: ao invés de abrir 10 abas manualmente, o reflexo vira "deixa eu fazer isso com IA primeiro".
- **"Não desista no vale" (dip)**: aprender algo novo tem custo de curto prazo (fica mais devagar/pior por um tempo) antes do ganho exponencial. Gráfico mental: progresso parece linear na expectativa, mas na prática é uma curva que "afunda" antes de disparar — é exatamente nesse vale que a maioria desiste. Vale a pena trocar 20% de produtividade "no vale" por 60%+ depois.
- **Você é gerente de agentes de IA, não engenheiro**: trate a IA como um funcionário novo — onboarding gradual, instruções claras do que é "bom" e "ruim", revisão do trabalho entregue (com seu julgamento/taste), feedback que vira atualização das instruções. Tudo isso é feito em linguagem natural — se você consegue pensar claro e descrever claro, você já sabe gerenciar agentes de IA.
