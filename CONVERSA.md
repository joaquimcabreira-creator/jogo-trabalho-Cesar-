# Conversa e plano — Jogo "Chefe de IA"

Registro da sessão de ideação e do plano fechado para o trabalho.
Data: 23/10/2026

---

## 1. Pedido inicial

> "Me dê ideias para fazer um jogo fácil de fazer mas diferente."

Contexto: trabalho de faculdade (repo `jogo-trabalho-Cesar-`), sem Scratch.
Stack disponível no ambiente: navegador, Python, VS Code, Atom.

---

## 2. Ideias levantadas

| Ideia | O "diferente" | Por que é fácil |
|---|---|---|
| **Gravidade** | Você não move o personagem, gira o mundo 90° e a bola cai | Só rotação + queda livre, zero colisão complexa |
| **Eco** | Você grava 5s de movimento e um clone repete; precisa de 3 clones pra abrir a porta | Gravar input = um array; o efeito parece mágica |
| **3 Segundos** | Cada fase dura exatamente 3s e reinicia; você decora o caminho | Timer + várias fases pequenas |
| **Você é o obstáculo** | Tower defense invertido: você desenha paredes pro inimigo *não* passar | Desenho com mouse + pathfinding simples |
| **Co-op assimétrico** | Jogador 1 vê o mapa, jogador 2 vê o personagem — só vencem conversando | Duas metades de tela, mesmo teclado |
| **Anti-speedrun** | Ganha quem terminar mais **lento**, sem parar | Física de plataforma + regra invertida |
| **Inventário de verbos** | Point & click onde os "itens" são ações: molhar, acender, empurrar | Sem física; o charme é o humor |
| **Terminal falso** | Você "hackeia" o jogo digitando comandos numa UI falsa | Só texto + CSS |

### Variação "jogo de IA" (algoritmo local, sem API)

1. **A IA que aprende você** — Markov de ordem 2 prevê seu próximo movimento e atira lá. Quanto mais repetitivo você for, mais ela acerta. ~200 linhas.
2. **Neuroevolução ao vivo** — população de 50 agentes com redes neurais minúsculas evoluindo na tela. ~120 linhas, efeito "uau" na apresentação.
3. **Inimigo que aprende onde você morre** — cada morte vira peso num heatmap; o inimigo patrulha os pontos quentes.

**Escolhido:** *Chefe de IA* — o humor vem do sistema, não de piadas soltas, e roda 100% offline com gerador combinatório (templates + bancos de palavras + estado do chefe).

---

## 3. Conceito escolhido

> **Chefe de IA**
> Você trabalha para um chefe virtual que inventa tarefas absurdas, muda as regras
> e avalia suas decisões. Seu objetivo é sobreviver a uma semana de trabalho.

- **Dificuldade:** fácil
- **Gênero:** humor / escolhas
- **Diferencial:** situações engraçadas geradas a partir das escolhas do jogador

---

## 4. O loop (uma jogada)

1. O chefe manda uma mensagem absurda no "chat" (com horário, tipo 09:02).
2. Você escolhe entre 3 respostas: **obedecer**, **questionar** ou **blefar** (dizer que já fez).
3. O chefe avalia, muda de humor e às vezes **inventa uma regra nova** que contradiz a anterior.
4. Os medidores mexem. Fim do dia → reunião → regra muda de novo.

Exemplo do tom:

> **SEGUNDA, 09:02 — CHEFE**
> "Preciso de um relatório de 40 páginas explicando por que não precisamos de
> relatórios. Hoje, 11h. Em vídeo."
>
> `[ Claro, chefe. ]`  `[ Isso é contraditório? ]`  `[ Já enviei. ]`

Se obedecer: `Sanidade -15`, `Confiança +2` → o chefe responde:
*"Ótimo. Cancelei o projeto. Abri outro igual, mas com prazo ontem."*

---

## 5. Sistemas

| Sistema | Como funciona |
|---|---|
| **4 medidores** | `Sanidade` (0 = burnout), `Confiança do Chefe` (quanto maior, **mais** tarefas absurdas — twist), `Café` (recurso pra pular uma tarefa), `Strikes` (3 regras quebradas = demissão) |
| **Chefe com manias** | 2–3 "quirks" sorteados por partida: ama planilhas, odeia a palavra "sinergia", fala de si na terceira pessoa, responde só com emoji. Mudam a pontuação e injetam falas |
| **Regras mutantes** | Cada dia entra uma regra ("reuniões agora têm 7 minutos"). A do dia seguinte anula a anterior — seguir a regra velha dá strike. É a piada central |
| **Gerador de situações** | Template com slots: `{tarefa}` `{prazo}` `{formato}` `{desculpa}` + banco de palavras por tema. 15 templates × bancos = centenas de combinações |
| **Avaliação procedural** | O feedback é montado pelo que você fez + quirk ativo + humor atual. Nada de texto fixo por botão |

---

## 6. Estrutura

- **5 dias × (3 tarefas + 1 reunião) = 20 cenas.** Partida de ~10 min.
- **6 finais:** Demitido, Burnout, Promovido a Gerente (o pior final), Herói do Escritório, Home Office Eterno, e **Você vira o Chefe**.
- **UI:** cara de chat de trabalho (estilo Teams/Slack) + relógio, calendário da semana, xícara de café e um "chefe está digitando…". Tudo CSS, sem arte.
- **Arquivos:** `index.html` + `style.css` + `game.js` + `content.js`. Sem build, abre com dois cliques. Progresso em `localStorage`.

### Ordem de implementação

1. Motor: estado, telas, chat UI, `localStorage`
2. Sistema de regras + relógio/calendário
3. Gerador de situações (templates + bancos) — 15 templates
4. Chefe com manias + avaliação procedural
5. Finais e balanceamento
6. Polimento: som de notificação, efeito de digitação, screen shake quando o chefe grita

---

## 7. Decisões em aberto

1. **Arquivo único** `index.html` (fácil de entregar) ou a pastinha com 4 arquivos (melhor pra mostrar o código)?
2. **Humor:** escritório brasileiro genérico (planilha, reunião, "alinhamento") ou com contexto de faculdade/TI?
3. Incluir **modo apresentação** (partida rápida de 3 dias) pra caber no tempo da demo?

**Padrão proposto:** arquivo único `index.html`, humor de escritório brasileiro, com modo demo de 3 dias.
