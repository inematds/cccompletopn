# Contrato de fragmento — Claude Code para Pessoas Normais (formato-curso-v5)

Você vai escrever o CONTEÚDO de 1 (ou alguns) dos 10 módulos do curso. Você recebeu este
contrato + o(s) brief(s) do(s) seu(s) módulo(s) (`content/briefs/modulo-NN.md`) — **não precisa
ler o shell inteiro nem a skill `formato-curso-v5`**. Este documento é autossuficiente.

Shell compartilhado (não mexer): `fragments/00-shell.html`. Assets do motor: `assets/aula.css`
+ `assets/curso.js` (este último tem 2 patches deliberados pra suportar 10 módulos — não
precisa entender, só não sobrescrever).

## 1. Vocabulário: módulo = aula

O curso chama de "módulo" o que o motor v5 chama de "aula" — é a mesma unidade, 1:1. Isso gera
**duas numerações diferentes que NÃO podem ser confundidas**:

| Onde | Formato | Exemplo pro módulo 3 | Exemplo pro módulo 10 |
|---|---|---|---|
| Comentário-placeholder no shell | 2 dígitos, zero-padded | `MODULO-03-CONTEUDO-AQUI` | `MODULO-10-CONTEUDO-AQUI` |
| `id`/`data-aula` do `<section>`, hash da URL, `id="cards-N"` | número cru, **sem** zero à esquerda | `data-aula="3"`, `#aula-3`, `id="cards-3"` | `data-aula="10"`, `#aula-10`, `id="cards-10"` |

Erro comum a evitar: usar `id="cards-03"` (motor procura exatamente `cards-`+valor de
`data-aula`, ou seja `cards-3` — com zero-padding o cartão nunca carrega).

## 2. Onde fica o seu placeholder

Em `fragments/00-shell.html`, procure o bloco:

```html
<section class="view" id="v-aula-N" data-aula="N" data-tempo="__TEMPO__">
<!-- MODULO-0N-CONTEUDO-AQUI -->
</section>
```

(N = número do seu módulo; para o módulo 10 não há zero à esquerda em `id`/`data-aula`, só no
comentário — ver tabela acima.) **Você substitui só o comentário** pelo HTML descrito na
seção 3. A tag `<section>` em volta (com `id`, `data-aula`) já está correta — não toque nela,
**exceto** o atributo `data-tempo="__TEMPO__"`: troque `__TEMPO__` pelo valor real (ex.:
`data-tempo="18min"`) como último passo, depois de calcular pela regra da seção 7.

## 3. Estrutura que substitui o comentário (4 movimentos, ordem fixa)

Copie-cole e preencha — esta é a espinha inteira de uma aula/módulo:

```html
<header class="hero"><div class="hero-inner">
  <div>
    <p class="kicker">Claude Code para Pessoas Normais · Módulo N</p>
    <h1>Título do<br><em>módulo</em></h1>
    <p class="promise">Ao fim deste módulo você consegue [ação observável e concreta] — sem precisar de mais nada além do que já tem hoje.</p>
    <!-- tempo é renderizado pelo motor automaticamente logo após a .promise, não escreva nada aqui -->
    <p class="why">O problema real do profissional, em 2–4 linhas, ligado ao que ele sente hoje no trabalho.</p>
    <p class="cue"><span class="arw">↓</span> role para estudar</p>
  </div>
  <!-- registro METÁFORA-DO-MUNDO-REAL, nunca diagrama aqui -->
  <div class="herofig"><svg viewBox="0 0 300 240" fill="none" aria-hidden="true"><!-- metáfora concreta do mundo do leitor --></svg></div>
</div></header>

<div class="layout">
  <div class="col">
    <!-- 3 a 5 steps (máx 7), um por <section class="step">, ver seção 4 pros quadros opcionais -->
    <section class="step" data-fig="1" data-kind="fundamento">
      <h2><span class="n">01</span> Título-afirmação da ideia</h2>
      <p>Conceito em linguagem simples. Jargão de domínio define inline na 1ª aparição: <span class="gterm" tabindex="0" data-def="Definição curta e concreta.">termo-de-domínio</span>. Jargão de plataforma (JSON, terminal, Git, commit, script, servidor, API, CLI, diretório...) nunca se presume — defina com analogia ou elimine.</p>
      <p>Exemplo aplicado: [uma das profissões da seção 8] numa situação concreta do próprio trabalho. (Obrigatório em 100% dos steps — contável.)</p>
      <!-- rampa de acesso, só antes de artefato tecnicamente opaco: nomeia o que vai ver + traduz numa metáfora + devolve o controle -->
      <div class="mobfig" data-mobfig="1"></div>
      <button class="readbtn" data-read="s1">marcar como lido</button>
    </section>

    <!-- ... mais steps ... -->

    <!-- prática — EXATAMENTE 1 por módulo, ver seção 5 -->
    <section class="practice" data-practice="p1" data-mode="prompt">
      <p class="pk">Pratique agora <span class="pcount">0/2 feito</span></p>
      <h3 class="ph">Título da tarefa — realiza a .promise deste módulo</h3>
      <p class="pgoal">Meta observável + tempo declarado ("~10 min").</p>
      <p class="psafe">Nada aqui apaga ou quebra nada de verdade. Não gostou do resultado? Apague e recomece.</p>
      <div class="pcodewrap"><button class="pcopy" type="button">copiar</button><pre class="pcode">bloco colável, com &lt;isto você troca&gt; marcado</pre></div>
      <ol class="psteps">
        <li><label><input type="checkbox" data-ptask="1"> Passo verificável pelo próprio aluno.</label></li>
        <li><label><input type="checkbox" data-ptask="2"> Outro passo, com "o que você deve ver" como referência.</label></li>
      </ol>
      <p class="pdone">Você acabou de conseguir [a mesma capacidade da promessa] — sem depender de mais ninguém.</p>
    </section>
  </div>

  <aside class="rail"><div class="railsticky">
    <div class="figstage">
      <!-- 1 <div class="fig" data-fig="N"> por step, mesmo N do data-fig do step -->
      <div class="fig on" data-fig="1" data-cap="<b>Fig.</b> legenda que ensina — diz o que olhar e o que significa"><svg viewBox="0 0 220 200" fill="none" aria-hidden="true"><!-- registro DIAGRAMA-DE-MECANISMO, nunca metáfora aqui --></svg></div>
      <!-- ... 1 .fig por step ... -->
    </div>
    <p class="railcap"></p>
    <div class="steps-ind"></div>
    <p class="sidenote">Grife qualquer trecho para virar um cartão de revisão.</p>
  </div></aside>
</div>

<div class="fecho-wrap">
<div class="recap-autor">
  <p class="rak">Resumo</p>
  <ul>
    <li>Síntese afirmativa da ideia 1, em linguagem humana — nunca em forma de pergunta.</li>
    <li>Síntese afirmativa da ideia 2, conectando com a primeira.</li>
    <li>Nuance importante que dá segurança ao aluno.</li>
  </ul>
</div>

<div class="next-action">
  <p class="nak">Seu próximo passo</p>
  <p class="na-win">Você acabou de conseguir [microvitória nomeada].</p>
  <p class="na-action">Nos próximos 15 minutos, no seu trabalho de verdade: [microação concreta].</p>
  <p class="na-hook">No próximo módulo, você resolve [a próxima dor real que trava quem já passou por este].</p>
</div>
</div>

<nav class="lnav"><a href="#trilha">← trilha</a><a href="#trilha">concluir ✓ →</a></nav>
<footer>Módulo N · INEMA.CLUB PRO</footer>

<!-- cartões: RECUPERAÇÃO ATIVA, nunca resumo do recap acima. id = cards-<valor exato de data-aula> -->
<script type="application/json" id="cards-N">
[
  {"front":"Pergunta que exige recuperar (não reconhecer) a ideia 1?","back":"Resposta curta."},
  {"front":"Decisão: dado um cenário novo, o que você faria?","back":"Resposta com o porquê."},
  {"front":"Contraste: qual a diferença entre X e Y?","back":"Resposta que distingue os dois."}
]
</script>

<!-- manifesto de completude — obrigatório, ver seção 9 -->
```

## 4. Quadros opcionais de retenção — classes exatas + mini-exemplo

Todos são HTML+CSS estático (já existe em `assets/aula.css`, não precisa CSS novo). Vivem
dentro de `.step`, entre a prosa.

**Erro comum** (`.qerr`) — máx. 2/módulo, só se o erro for real e frequente:
```html
<div class="qerr">
  <p class="qk">Erro comum</p>
  <p class="qerr-txt"><b>O erro em 1 frase.</b> Por que costuma acontecer, e como evitar.</p>
</div>
```

**Antes/depois** (`.qbefore-after`) — máx. 1/módulo, sempre com saldo mensurável:
```html
<div class="qbefore-after">
  <div class="qba-col qba-before"><p class="qba-lbl">Antes</p><p class="qba-txt">Situação sem o recurso.</p></div>
  <div class="qba-col qba-depois"><p class="qba-lbl">Depois</p><p class="qba-txt">Situação com o recurso.</p></div>
  <p class="qba-saldo"><b>Saldo:</b> ganho mensurável em 1 linha.</p>
</div>
```

**Frase-âncora** (`.qanchor`) — máx. **1 por módulo**, regra dura:
```html
<div class="qanchor"><p>A frase serifada, grande, que resume a virada de chave do módulo.</p></div>
```

**Aplicação multi-profissão** (`.qapply`) — só com ≥2 profissões nomeadas dentro do bloco:
```html
<div class="qapply">
  <p class="qak">Isso muda pra você se...</p>
  <ul>
    <li><b>Contador(a) autônomo(a):</b> aplicação concreta na rotina dele(a).</li>
    <li><b>Dono(a) de pequeno negócio:</b> aplicação concreta diferente.</li>
  </ul>
</div>
```

**Checklist de passos** (`.qsteps`) — só quando há ação sequencial real:
```html
<div class="qsteps">
  <p class="qsk">Passo a passo</p>
  <ol><li><b>Primeiro</b> — o que fazer.</li><li><b>Depois</b> — o que fazer.</li></ol>
</div>
```

**Termo de glossário** (`.gterm`, inline, sem limite formal mas use com parcimônia) — sempre
que houver jargão de domínio (não de plataforma) na 1ª aparição:
```html
<span class="gterm" tabindex="0" data-def="Definição curta e concreta.">termo-de-domínio</span>
```

**Nota lateral** (`.sidenote`, dentro do `<aside class="rail">`, 1 por módulo é o normal):
```html
<p class="sidenote">Nota curta e opcional que complementa sem interromper a leitura principal.</p>
```

**Demo real de comando** (`.termdemo`) — 2ª opção pro slot de figura (`data-fig`), só quando o
step tem um comando real pra mostrar; par certo+erro só se existir um erro plausível (senão,
1 `.termdemo-cmd` só, sem o botão de toggle). Vai no rail (com `class="fig termdemo"`) **e**
uma cópia idêntica sem a classe `fig` dentro do `.mobfig` do step, pro mobile:
```html
<div class="fig termdemo" data-fig="2" data-cap="<b>Fig.</b> rode o comando e veja a diferença">
  <button class="termdemo-reveal" type="button">ver o que acontece →</button>
  <div class="termdemo-body" hidden>
    <div class="termdemo-cmd on" data-variant="ok">
      <p class="termdemo-k">comando certo</p><pre class="termdemo-in">$ comando --certo</pre><pre class="termdemo-out">saída esperada</pre>
    </div>
    <div class="termdemo-cmd" data-variant="erro">
      <p class="termdemo-k">erro comum</p><pre class="termdemo-in">$ comando --errado</pre><pre class="termdemo-out termdemo-err">a mensagem de erro real</pre>
    </div>
    <button class="termdemo-toggle" type="button">ver o erro comum</button>
  </div>
</div>
```

**Teste-se** (`.quiz`, obrigatório ≥1 em módulo tipo FUNDAMENTO):
```html
<div class="quiz" data-answer="b">
  <p class="qk">Teste-se</p><p class="q">Pergunta de checagem sobre o que acabou de ler?</p>
  <button class="opt" data-k="a">alternativa errada</button>
  <button class="opt" data-k="b" data-fb="Por que esta é a certa, em 1 frase.">alternativa certa</button>
  <button class="opt" data-k="c">alternativa errada</button>
  <p class="qfb"></p>
</div>
```

## 5. Prática multi-modo — escolha 1 modo, nunca "codigo" (público leigo)

| Modo | Quando usar | Critério de pronto no `.pgoal` |
|---|---|---|
| `data-mode="prompt"` | tarefa colável num chat de IA | "a resposta ficou parecida com isto?" |
| `data-mode="tarefa"` | ação no próprio trabalho, sem terminal | "ficou pronto quando [condição observável]" |
| `data-mode="analise"` | fazer de verdade é impossível/arriscado — mini-caso + gabarito em `<details>` | "comparou com o gabarito" |

Regras duras: exatamente 1 `.practice` por módulo · 5–15min sempre declarado no `.pgoal` ·
`.psafe` é obrigatória em 100% das práticas (por que é seguro + o que fazer se parecer errado)
· a prática precisa realizar literalmente a `.promise` do módulo.

## 6. Dosagem — teto ≤5 quadros opcionais por módulo (fora da espinha)

**Espinha** (sempre 1×, fora de qualquer teto, os automáticos do motor não contam):
`.recap-autor` · `.practice` · `.next-action` · exemplo-por-profissão (em CADA step).

**Pool dosado** (obrigatórios do tipo + opcionais escolhidos, teto **≤5 por módulo**):

| Recurso | FUNDAMENTO | FERRAMENTA | PRÁTICA/INTEGRAÇÃO |
|---|---|---|---|
| Teste-se | obrigatório 1–2 | opcional 0–1 | opcional 0–1 |
| Cartões novos (`cards-N`) | obrigatório 3–6 | opcional 2–4 | — |
| `.qerr` | opcional 0–1 | obrigatório 1–2 | opcional 0–1 |
| `.qbefore-after` | obrigatório 1 | opcional 0–1 | opcional 0–1 |
| `.qanchor` | obrigatório 1 | opcional 0–1 | opcional 0–1 |
| `.qsteps` | — | obrigatório 1 | obrigatório 1 |
| `.qapply` | opcional 0–1 | opcional 0–1 | opcional 0–1 |

Classifique seu módulo como FUNDAMENTO (conceito estável), FERRAMENTA (interface/passos
específicos que podem mudar de versão) ou PRÁTICA/INTEGRAÇÃO (junta módulos anteriores), e
marque cada `.step` com `data-kind="fundamento"` ou `data-kind="ferramenta"` (100% dos steps
classificados). **Densidade visual, regra independente do teto:** no máximo 1 quadro funcional
a cada 2 steps — não empilhe 2 quadros em steps adjacentes mesmo dentro do teto de 5.

**Teste-se em módulo FUNDAMENTO não é opcional** — é item autônomo do manifesto (seção 9).

## 7. Tempo (`data-tempo`) — cálculo e onde entra

Fórmula: `(palavras do módulo / 200) + tempo da prática (minutos do .pgoal) + ~30s por
pergunta de teste-se`, arredondado **para cima**, formato `"18min"`. Preencha em 2 lugares:
o atributo `data-tempo` do `<section>` (seção 2) e o tempo dentro do `.pgoal` da prática.
Nunca deixe parcial — módulo sem `data-tempo` é pior que não ter o dado.

## 8. Protocolo de descoberta (já resolvido — reuse, não invente outro)

- Público: leigos em tecnologia, 40+, "pessoas normais", profissionais de conhecimento (não
  devs), baixa/nenhuma familiaridade com tecnologia (nunca abriu terminal).
- **Profissões-alvo para os exemplos (≥2 por conceito, citar por nome):** contador(a)
  autônomo(a) · dono(a) de pequeno negócio/loja · coordenador(a) de RH · professor(a) ·
  consultor(a) freelancer. Use pelo menos 2 diferentes ao longo do módulo; não repita sempre a
  mesma dupla.
- Resultado esperado: sair usando Claude Code no próprio trabalho sem depender de ninguém.
- Sessão: 15–25min por módulo.

## 9. Manifesto de completude — cole no fim do módulo, ANTES de fechar `</section>`

```html
<!-- COMPLETUDE aula-N | tipo=FUNDAMENTO|FERRAMENTA|PRATICA | steps=K
  [OK/AUSENTE] promise            .promise no cold-open (1)
  [OK/AUSENTE] why                .why após a promessa (1)
  [OK/AUSENTE] tempo              data-tempo no view + render no cold-open (1)
  [OK/AUSENTE] exemplo-profissao  exemplo aplicado em 100% dos steps (K/K)
  [OK/AUSENTE] apoio-visual       figura conceitual legendada em 100% dos steps (K/K)
  [OK/AUSENTE] practice           .practice[data-mode] (exatamente 1)
  [OK/AUSENTE] psafe              .psafe dentro da prática (1)
  [OK/AUSENTE] pgoal-tempo        .pgoal com tempo declarado (1)
  [OK/AUSENTE] recap-autor        .recap-autor no fecho (1)
  [OK/AUSENTE] next-action        .next-action no fecho (1)
  [OK/AUSENTE] cards              <script id="cards-N"> presente (1)
  [OK/AUSENTE] anti-duplicacao    nenhum cartão repete ≥6 palavras seguidas do recap; cartões são pergunta/decisão/contraste/diagnóstico/aplicação, nunca afirmação
  [OK/N/A] teste-se-FUND          SE tipo=FUNDAMENTO: ≥1 quiz data-fb obrigatório (OK/AUSENTE) | SE FERRAMENTA/PRATICA: sempre [N/A], mesmo com teste-se opcional presente
-->
```

Qualquer `[AUSENTE]` = módulo não está pronto. `teste-se-FUND` nunca é `[OK]` fora de
FUNDAMENTO — é sempre `[N/A]` nesse caso (mesmo com teste-se opcional presente).

## 10. Regras de tom — resumo executável

Zero emoji/exclamação dupla nos fechos (recap, next-action, prática concluída). Nunca
infantilizar/hype/condescendência. Jargão de domínio (assunto do curso) define inline com
`.gterm`; jargão de plataforma (JSON, terminal, Git, commit, branch, pipeline, "arquivo de
configuração", instalar, script, servidor, API, deploy, CLI, diretório, plugin, encoding)
nunca se presume — traduz com analogia ou elimina a palavra. Antes de qualquer bloco de código
(`<pre>`)/print de configuração, 1 frase de rampa de acesso: nomeia o que vai ver + traduz numa
metáfora do mundo do leitor + devolve o controle ("você só vai mexer nesta parte aqui").

## 11. O que NÃO fazer

Não editar `assets/aula.css` nem `assets/curso.js`. Não criar `id` além de `cards-N` (regra da
seção 1); se precisar de algum `id` extra por algum motivo excepcional, prefixe com `mNN-`
(N = seu módulo) — mas isso raramente é necessário, pois o motor escopa tudo (`.step`,
`.readbtn`, `.practice`, `data-fig`) pelo `.view[data-aula]` mais próximo, então `data-read`,
`data-fig`, `data-practice`, `data-ptask` **podem se repetir entre módulos sem colisão**
(ex.: `data-read="s1"` existe em todo módulo, sem problema). Não adicionar gamificação (pontos,
badges, confete, streak). Não usar `data-mode="codigo"` (só para cursos técnicos — não é o
caso). Não inventar profissões fora da lista da seção 8 sem necessidade.
