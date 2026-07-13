# Módulo 2 — Primeiros passos com Claude Code
Timestamps fonte: 31:44–48:10

## Tópicos do módulo
1. Instalando o Claude Code
2. Criando os primeiros prompts
3. Trabalhando com arquivos locais
4. Manipulando arquivos Excel e HTML
5. Engenharia de prompts
6. Verificação dos resultados
7. Tokens e escolha do modelo

## Conteúdo condensado

### Instalação
Precisa de assinatura paga da Anthropic (Free não dá acesso a Claude Code; Pro e Max sim). Duas formas de instalar: comandos de terminal (Google "claude code install", segue quick start docs) ou o **app desktop do Claude** (mais recomendado para iniciantes — interface amigável). Também dá pra usar via extensão no VS Code (terminal ou extensão com interface). Não importa onde você roda — por baixo é a mesma coisa; tudo (arquivos, sessões, projetos) continua acessível se trocar de interface depois.
Planos: Pro (~entrada) e Max ($100 ou $200/mês). Argumento de valor: por US$200/mês você tem "um funcionário de IA completo" — um bom engenheiro de software custaria muito mais de US$100k/ano.

### Primeiro contato: arquivos locais
Demo real: pedir para achar um arquivo de logo ("AIS Live Black") no Downloads sem saber o caminho exato — Claude Code busca, usa visão pra confirmar que é o arquivo certo, usa PowerShell pra localizar, devolve o caminho, e ainda oferece organizar/copiar o arquivo. Mostra que ele acessa e manipula arquivos locais como um assistente real, não só um chat.

### Demo: relatório de analytics do YouTube em Excel
Prompt (SLG goal — roda até a condição ser satisfeita): "quero uma avaliação do Q2 do canal, puxando dados reais do YouTube e fazendo análise profunda como se fosse meu estrategista de conteúdo, não só mostrar números". Resultado: Claude foi ao YouTube, tirou screenshots para verificar a si mesmo, e gerou uma planilha Excel completa (dashboard executivo, scorecard por vídeo, tendências mensais, pilares de conteúdo, formato/duração, audiência e tráfego) em ~10 minutos — trabalho que levaria horas manualmente. Tudo com um único prompt em linguagem natural.

### Engenharia de prompt (4 alavancas práticas)
1. **Papel + contexto**: definir quem a IA é e o contexto do seu negócio/situação (ex.: "você é estrategista de conteúdo mestre, ajudando Nate, cujo negócio é X, métricas importantes são Y, avatar de cliente é Z").
2. **Contexto específico da tarefa**: dar a situação real, não só o pedido genérico. Exemplo: em vez de "me ajuda a escrever esse e-mail pro meu chefe", dar o contexto emocional/situacional completo (já foi repreendido 2x no mês, está pedindo folga, chefe é compreensivo mas você se sente culpado) — isso muda completamente a qualidade do output.
3. **Prompt negativo**: dizer explicitamente o que NÃO fazer. Analogia: ensinar alguém de 45 anos a fazer ovo mexido não precisa dizer "não encoste a mão no fogão" (ele já sabe); uma mente inocente precisa. Prompt negativo mantém o agente dentro dos trilhos.
4. **Verificação / prova de trabalho** ("MAKE THE AI PROVE ITS WORK" — em caixa alta no vídeo original). Gráfico conceitual: sem verificação, você começa em ~60% de acerto e sobe manualmente por rodadas de feedback até 100%; com a IA verificando o próprio trabalho (ex.: abrir o site e testar 100 submissões de formulário para achar bugs de validação de e-mail), o primeiro resultado já sai em ~80%, fechando a distância. Pergunta-guia: "se um humano me entregasse isso, o que eu faria pra aprovar? Abriria e usaria? Reveria X vezes? Testaria casos extremos?" — o que você faria manualmente, você pode pedir pra IA fazer também.

### Tokens e modelos
Token ≈ 3/4 de uma palavra (não é regra exata, varia por pontuação etc). Preços (na época do vídeo, jul/2026):
- **Haiku**: rápido e barato — US$1/milhão tokens de entrada, US$5/milhão de saída.
- **Sonnet 5**: balanceado — US$3/milhão entrada, US$15/milhão saída.
- **Opus 4.8**: mais capaz — US$5/milhão entrada, US$25/milhão saída (Fable custa ainda mais, ~o dobro do Opus).
Saída é sempre mais cara que entrada porque é o que o modelo "gera" ativamente. Exemplo real: gerar a planilha Excel do Q2 custou ~25.000 tokens de saída em Opus ≈ US$0,63 real, dentro de uma sessão que usou 428.000 tokens de um limite de 1 milhão.
No plano de assinatura (não API), você não paga por token diretamente — tem limites: sessão atual (janela rolante de 5h), limite semanal (todos os modelos), limite semanal específico do Fable. No plano Max de US$200/mês, encher toda sessão/limite semanal equivaleria a ~US$8.000 de inferência — grande desconto sobre pagar por token via API.
