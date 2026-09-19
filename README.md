# 🧺 Cesta Maluca

Jogo infantil para navegador: mova a cesta e pegue tudo que cai do céu. Feito com **HTML, CSS e JavaScript puros**, sem bibliotecas nem etapa de build, em um único `index.html`.

<!-- Adicione aqui o link para jogar (Vercel ou itch.io) -->

## Como jogar

- **Celular:** arraste o dedo na tela para mover a cesta.
- **Computador:** mova o mouse ou use as setas `←` `→` (ou `A` e `D`).
- Cada rodada dura **45 segundos**. O sol (ou a pérola, ou a lua) atravessa o céu e serve de relógio, então quem ainda não lê entende quanto tempo falta.
- Não existe "game over". No fim da rodada a criança ganha de 0 a 3 estrelas: **15**, **28** e **40** itens pegos.
- O recorde e as estrelas de cada mundo ficam salvos no próprio aparelho.

## Mundos

| Mundo | Personagem | O que cai | Diferença na jogabilidade |
|---|---|---|---|
| Horta 🍎 | 🐻 | frutas | jogo base, com nuvens e flores |
| Mar 🐠 | 🐙 | tesouros do mar | os itens balançam de lado enquanto caem |
| Espaço 🪐 | 👽 | estrelas, planetas e mais | queda mais rápida, com brilho atrás dos itens |

## Recursos

- Desenho no `<canvas>` 2D, com layout responsivo (celular e computador).
- Sons gerados por código com a Web Audio API, sem arquivos de áudio. Há botão para desligar o som.
- **PWA:** pode ser instalado no celular ("Adicionar à tela inicial") e funciona offline depois da primeira visita.
- Respeita `prefers-reduced-motion` (menos animações para quem prefere assim).
- Botões grandes, foco visível no teclado e rótulos de acessibilidade.

## Rodando localmente

O service worker do PWA só funciona em `http://localhost` ou `https`, então rode um servidor simples na pasta do projeto:

```bash
python3 -m http.server 8000
# ou
npx serve
```

Depois abra `http://localhost:8000`. Abrir o `index.html` direto no navegador também funciona, mas sem o modo PWA e offline.

## Estrutura

```
.
├── index.html               # o jogo inteiro (HTML, CSS e JS)
├── manifest.webmanifest     # nome, cores e ícones do PWA
├── sw.js                    # service worker (cache offline)
├── icon-192.png             # ícones do app
├── icon-512.png
└── icon-maskable-512.png
```

## Personalizando

No começo do `<script>` do `index.html` ficam as configurações principais:

- `DURATION`: duração da rodada, em segundos.
- `STAR_1`, `STAR_2`, `STAR_3`: quantos itens valem cada estrela.
- `THEMES`: os mundos. Para criar um novo, adicione um objeto em `THEMES` e o nome dele na lista `ORDER`.

Campos de cada mundo:

| Campo | Para que serve |
|---|---|
| `label`, `icon`, `hero` | nome do botão, emoji do botão e personagem |
| `items`, `noun` | emojis que caem e como aparecem no texto final (singular e plural) |
| `skyTop`, `skyBot`, `ground`, `groundLight`, `hill1`, `hill2` | cores do cenário |
| `mark`, `tint`, `tintA` | marcador do tempo (sol, pérola, lua) e tom do fim da rodada |
| `ambient` | fundo animado: `clouds`, `bubbles` ou `stars` |
| `deco` | decoração do chão: `flower`, `shell` ou `crater` |
| `basket` | cores da cesta |
| `speed`, `sway`, `spin` | velocidade da queda, balanço lateral e giro dos itens |
| `wave` | tipo de onda do som (`sine`, `triangle`...) |
| `glow` | brilho opcional atrás dos itens |

**Trocar o nome do jogo:** altere o `<title>`, o `<h1>` e a metatag `apple-mobile-web-app-title` no `index.html`, os campos `name` e `short_name` no `manifest.webmanifest` e o nome do cache no `sw.js`.

## Publicando

**Vercel:** importe o repositório e, se ele perguntar o framework, escolha **Other**. Não há comando de build. Ao publicar uma versão nova, troque o número de `CACHE` no `sw.js` (por exemplo, `cesta-maluca-v2` para `v3`), senão quem já instalou pode continuar vendo a versão antiga.

**itch.io:** use uma versão sem as partes de PWA (manifesto e service worker):

1. Copie o `index.html`.
2. Apague as metatags de PWA e os links para `manifest` e ícones no `<head>`.
3. Apague o bloco `// PWA: registra o service worker` no fim do script.
4. Compacte só o `index.html` em um ZIP.
5. No itch.io, escolha o tipo **HTML**, envie o ZIP e marque **This file will be played in the browser**.

## Privacidade

O jogo não pede cadastro, não usa anúncios nem rastreadores e não envia dados a nenhum servidor. O navegador guarda apenas o recorde, as estrelas e o último mundo escolhido (`localStorage`).

Um detalhe: a fonte **Fredoka** é carregada do Google Fonts, o que faz o navegador da criança contatar o Google. Para evitar qualquer requisição externa, baixe a fonte, hospede junto com o jogo e troque o `<link>` por uma regra `@font-face`.

## Observações

- A aparência dos emojis varia de acordo com o sistema (Android, iOS, Windows), então frutas e personagens podem ficar um pouco diferentes de um aparelho para outro.
- Alguns navegadores de celular bloqueiam o `localStorage` quando o jogo roda dentro de outra página (como no itch.io). Nesse caso o jogo funciona normalmente, mas os recordes não são guardados.
