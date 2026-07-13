# Plano de produção — Claude Code for Normal People (formato v5)

Fonte: transcript do vídeo "Claude Code for Normal People (6 Hour Course)" (Nate Herk, YouTube).
Formato de destino: `formato-curso-v5` (INEMA, dark editorial, página única com roteamento por hash, retenção com portão de completude).

## Estratégia de tokens
- Transcript (gigante) foi lido **uma única vez** na sessão principal e já foi condensado em 10 briefs por módulo (`content/briefs/modulo-XX.md`). Nenhuma fase depois desta precisa reler o transcript bruto.
- A sessão principal (orquestradora) **não carrega** a skill `formato-curso-v5` nem HTML grande — isso fica só dentro de subagentes novos (não-fork), cada um com contexto mínimo: seu brief + o "contrato de fragmento" da Fase 1.
- Fase 1 gera o shell compartilhado (head/CSS/JS/nav/índice da trilha) **uma vez** + um contrato curto (classes, atributos, esqueleto de fragmento) que os módulos seguintes reusam sem precisar reler o shell inteiro.
- Módulo 1 é construído sozinho como **exemplar dourado**, revisado, e só depois vira template concreto para os agentes que fazem os módulos 2–10 (evita 9 módulos divergentes e retrabalho caro).
- Fan-out final em lotes de 2–3 módulos por agente (amortiza custo de carregar skill + contrato).
- Montagem final = concatenação determinística dos fragmentos (sem LLM).
- Cada fase termina com commit git — nada se perde se a sessão cair.

## Fases

- [x] Fase 0 — Preparação: estrutura de pastas, git init (autor NeiMaldaner), PLAN.md
- [x] Fase 0.1 — Extração dos 10 briefs condensados do transcript → `content/briefs/`
- [ ] Fase 1 — Shell v5: head/nav/CSS/JS/índice da trilha + `content/CONTRATO-FRAGMENTO.md`
- [ ] Fase 2 — Módulo 1 (exemplar dourado) — revisão humana antes de prosseguir
- [ ] Fase 3 — Módulos 2–10 (fan-out em lotes, usando módulo 1 como template)
- [ ] Fase 4 — Montagem final (`curso.html`), verificação com agent-browser (hash routing, portão de completude)
- [ ] Fase 5 — Publicação (git push) — só quando o usuário confirmar

## Mapeamento de conteúdo por módulo (timestamps do vídeo original)

1. Fundamentos e mentalidade — 00:00–31:44
2. Primeiros passos com Claude Code — 31:44–48:10
3. Estruturação de projetos — 48:10–1:20:31
4. Construindo um sistema operacional de IA — 1:20:31–2:03:17
5. Memória e segundo cérebro — 2:03:17–2:20:23
6. Subagentes — 2:20:23–2:48:07
7. Construção e publicação de projetos — 2:48:07–3:35:22
8. Segundo cérebro avançado — 3:35:22–4:31:50
9. Sistemas multiagentes e automações — 4:31:50–5:27:13
10. Otimização e encerramento — 5:27:13–5:58:20

## Status
Última atualização: Fase 0 e 0.1 concluídas e commitadas (`2301fd8`). Todos os 10 briefs em `content/briefs/`.
Próximo passo: Fase 1 (shell v5) + Fase 2 (módulo 1 exemplar) — aguardando confirmação do usuário antes do fan-out dos módulos 2–10.
