# Listagem de vídeos do YouTube com Grid Layout

Desafio 03 da [Trilha de CSS da DIO](https://www.dio.me/), com foco em **Grid Layout**.

> **O desafio:** construir a página de listagem de vídeos do YouTube com CSS,
> colocando em prática os conceitos aprendidos, principalmente sobre **Grid
> Layout**. O design de referência está no
> [Figma](https://www.figma.com/design/KknwioExyqKD3D2eSVFrcW/Desafio-Grid---DIO).

**Como este projeto foi feito:** o desafio **não tem repositório base**, e o
Figma bloqueia acesso automático (HTTP 403). Então os dados do protótipo foram
**extraídos do próprio arquivo**: texto por OCR do canvas, cores por amostragem
de pixel, proporções por medição das regiões, e as imagens baixadas das URLs do
arquivo. Os números dessa extração estão documentados abaixo.

---

## Resultado

Página inicial com cabeçalho, menu lateral e grade de vídeos.

### Estrutura

```
trilha-css-desafio-03/
├── index.html
├── assets/
│   ├── css/
│   │   ├── reset.css      Normalização (53 linhas)
│   │   └── styles.css     Estilização completa (548 linhas comentadas)
│   └── images/
│       ├── youtube-logo.png
│       ├── avatar-canal.png
│       ├── avatar-usuario.jpg
│       └── thumb-01..05.png
└── README.md
```

---

## Grid Layout: onde cada técnica foi usada

Este é o ponto central do desafio. São **quatro grades aninhadas**, e o
`styles.css` comenta a decisão em cada uma.

### 1. Cabeçalho — `auto 1fr auto`

```css
.cabecalho {
    display: grid;
    grid-template-columns: auto minmax(0, 1fr) auto;
    align-items: center;
}
```

A primeira e a última coluna ficam do tamanho do conteúdo (`auto`) e a do meio
estica (`1fr`). É o jeito mais direto de centralizar a barra de busca — sem
`position: absolute` e sem largura calculada.

### 2. Layout — menu lateral fixo + conteúdo flexível

```css
.layout {
    display: grid;
    grid-template-columns: 218px minmax(0, 1fr);
    align-items: start;
}
```

O `minmax(0, 1fr)` é o detalhe importante: **`1fr` sozinho tem tamanho mínimo
automático**, ou seja, o tamanho do conteúdo. Um título longo empurraria a
coluna e estouraria o layout. Com o mínimo em `0`, a coluna pode encolher de
verdade. Usei o mesmo padrão em mais 3 lugares.

### 3. A grade de vídeos — o coração do desafio

```css
.grade {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 40px 46px;
    align-content: start;
}
```

- `auto-fill` cria quantas colunas couberem na largura disponível
- `minmax(250px, 1fr)` garante que nenhuma coluna fique menor que 250px e que
  todas dividam o espaço igualmente
- `gap: 40px 46px` é a abreviação de `row-gap` e `column-gap`

**Resultado: a grade se ajusta sozinha, sem media query.** Na largura do
protótipo (1438px) dá exatamente **4 colunas de 250px**, como no design. Em
1920px dá 5, em 1024px dá 2, no celular dá 1.

### 4. Informações do card — avatar fixo + texto flexível

```css
.card__info {
    display: grid;
    grid-template-columns: auto minmax(0, 1fr);
    gap: 12px;
}
```

### 5. Outros usos do Grid

| Elemento | Técnica |
| --- | --- |
| Botões de ícone | `display: grid` + `place-items: center` centraliza o SVG nos dois eixos em uma linha |
| Grupos do cabeçalho | `grid-auto-flow: column` coloca os itens lado a lado sem `flex` |
| Barra de busca | `grid-template-columns: minmax(0, 1fr) auto` — o campo estica, o botão fica no tamanho |
| Menu lateral | `display: grid` + `gap: 2px` para o ritmo uniforme entre os itens |
| Texto do card | `display: grid` + `gap: 4px` |
| Rodapé | `display: grid` + `place-items: center` |

### 6. Responsividade: o menu vira uma faixa horizontal

```css
@media (max-width: 900px) {
    .layout { grid-template-columns: minmax(0, 1fr); }   /* uma coluna */

    .menu {
        grid-auto-flow: column;        /* itens lado a lado */
        grid-auto-columns: max-content;
        overflow-x: auto;              /* rola na horizontal */
    }
}
```

Sem espaço para o menu ao lado, ele vai para cima e os itens se distribuem na
horizontal com rolagem — o mesmo HTML, outra disposição.

---

## Dados extraídos do protótipo

### Texto (OCR do canvas)

O conteúdo é o do design original. No menu lateral: **Início, Explorar,
Inscrições, YouTube Music, Vídeos que gostei, Assistir mais tarde, Downloads,
Filmes, Aprender, Esportes**.

E 8 vídeos na grade:

| Vídeo | Visualizações | Data |
| --- | --- | --- |
| Geração Tech Unimed - BH - Ciência de Dados | 2,1 mil | há 2 dias |
| Scrum Talks | 1,6 mil | há 3 dias |
| Machine Learning Avançado - Fast Track | 1,7 mil | há 4 dias |
| Global Skills #2 - Project Planning | 971 | há 5 dias |
| Como aprender a programar Python do ZERO | 711 | há 6 dias |
| Geração Tech Unimed - BH - Ciência de Dados | 2,1 mil | há 2 dias |
| Scrum Talks | 1,6 mil | há 3 dias |
| Machine Learning Avançado - Fast Track | 1,7 mil | há 4 dias |

### Cores (amostragem de pixel)

O design é em **modo claro**: cabeçalho e menu brancos, fundo `#f9f9f9`.

| Uso | Cor | Contraste |
| --- | --- | --- |
| Fundo da página | `#f9f9f9` | — |
| Cabeçalho e menu | `#ffffff` | — |
| Texto principal | `#0f0f0f` | 18,21:1 |
| Canal e metadados | `#606060` | 5,97:1 |
| Contador de notificações | `#cc0000` | 5,89:1 |

### Medidas (em CSS px)

| Medida | Design | Implementação |
| --- | --- | --- |
| Largura do frame | 1438 | — (layout fluido) |
| Altura do cabeçalho | 56 | **56** |
| Largura do menu lateral | 218 | **218** |
| Largura da miniatura | 250 | **251** |
| Vão entre colunas | 46 | **46** |
| Colunas na largura do design | 4 | **4** |

### Como as miniaturas foram associadas aos cards

O arquivo exporta 5 miniaturas, mas o design tem 8 cards. Para descobrir **qual
miniatura pertence a qual card**, recortei cada região de miniatura do
screenshot e comparei com as imagens baixadas, calculando o erro médio por
pixel:

| Card | Miniatura | Erro |
| --- | --- | --- |
| L1C1 — Geração Tech | `thumb-01` | 872 |
| L1C2 — Scrum Talks | `thumb-02` | 74 |
| L1C3 — Machine Learning | `thumb-03` | 115 |
| L1C4 — Global Skills | `thumb-04` | 56 |
| L2C4 — Machine Learning | `thumb-03` | 115 |

O erro baixo confirma a associação. As demais células da linha 2 reusam as
miniaturas da linha 1, que é o padrão do próprio design.

---

## Acessibilidade

| Recurso | Motivo |
| --- | --- |
| **Skip link** | Pular o cabeçalho e o menu, indo direto à grade |
| **`:focus-visible`** | Contorno de 2px azul em todos os controles |
| **`aria-label`** | Todos os botões só com ícone têm nome acessível |
| **`role="search"`** | Identifica a região de busca |
| **`<label>` oculto** | A busca tem rótulo, visível só para leitor de tela |
| **`aria-current="page"`** | Marca o item ativo do menu |
| **`aria-labelledby`** | O menu tem nome acessível |
| **`alt` nas miniaturas** | Descreve o conteúdo de cada vídeo |
| **`alt=""` nos avatares** | Decorativos, já que o nome do canal está no texto |
| **`tabindex="-1"`** | O skip link move o foco de verdade, não só rola a página |
| **`prefers-reduced-motion`** | Respeita quem desativou animações |
| **`forced-colors`** | Suporte ao alto contraste do Windows |

---

## Verificações executadas

### axe-core — WCAG 2.0/2.1 A + AA + best-practice

| Violações | Verificações aprovadas |
| --- | --- |
| **0** | **46** |

### Grid Layout — containers verificados no navegador

| Container | display | Colunas | Gap |
| --- | --- | --- | --- |
| `.cabecalho` | grid | `auto 1fr auto` | 16px |
| `.cabecalho__esquerda` | grid | flow column | 16px |
| `.cabecalho__direita` | grid | flow column | 8px |
| `.busca` | grid | `1fr auto` | — |
| `.layout` | grid | `218px 1fr` | — |
| `.menu` | grid | 1 coluna | 2px |
| `.grade` | grid | **4 × 250px** | 40px 46px |
| `.card__link` | grid | 1 coluna | 12px |
| `.card__info` | grid | `auto 1fr` | 12px |
| `.card__texto` | grid | 1 coluna | 4px |
| `.rodape` | grid | 1 coluna | — |

### Responsividade

| Largura | Overflow | Colunas da grade | Layout | Menu | Busca |
| --- | --- | --- | --- | --- | --- |
| 1920px | não | 5 × 288px | 2 colunas | ao lado | visível |
| 1438px | não | **4 × 251px** | 2 colunas | ao lado | visível |
| 1200px | não | 3 × 303px | 2 colunas | ao lado | visível |
| 1024px | não | 2 × 382px | 2 colunas | ao lado | visível |
| 900px | não | 3 × 263px | 1 coluna | acima | visível |
| 700px | não | 2 × 318px | 1 coluna | acima | oculta |
| 480px | não | 1 × 448px | 1 coluna | acima | oculta |
| 360px | não | 1 × 328px | 1 coluna | acima | oculta |

O número de colunas muda sozinho por causa do `auto-fill` — **sem nenhuma media
query de grade**. As media queries só ajustam o menu lateral, o respiro e a
busca.

### Outras verificações

| Teste | Resultado |
| --- | --- |
| Contraste da paleta (fórmula da WCAG) | todas em AA |
| Teclado: skip link primeiro e Enter move o foco | ✅ |
| Imagens carregando | 10 de 10 |
| Requisições falhadas | nenhuma |
| Erros de JavaScript | nenhum |
| Todas as miniaturas em 16:9 | ✅ nenhuma cortada |
| Textos cortados com reticências | **0** |

---

## Dois problemas encontrados depois, numa revisão visual

Os testes automáticos mediam larguras e passavam — mas eles não olhavam a
**proporção renderizada** das imagens nem se o texto estava sendo cortado. Os
dois problemas abaixo só apareceram numa inspeção visual:

### 1. As miniaturas estavam sendo cortadas em 42%

**O que acontecia:** cada miniatura aparecia quase quadrada (278×270) em vez de
16:9 (278×156), e o `object-fit: cover` cortava **42% da largura** de cada
imagem — as laterais dos cards, onde ficam os textos das capas, sumiam.

**Causa raiz:** o `<img>` tem os atributos `width="480" height="270"`, que o
navegador aplica como `height: 270px` (dica de apresentação). **Um `height`
definido anula o `aspect-ratio`**, então a regra `aspect-ratio: 16 / 9` não
fazia efeito.

**Correção:** uma linha.

```css
.card__thumb {
    width: 100%;
    height: auto;          /* deixa o aspect-ratio mandar */
    aspect-ratio: 16 / 9;
    object-fit: cover;
}
```

Depois da correção: caixa de 278×156, proporção 1,778 — igual à da imagem, sem
nenhum corte.

### 2. A linha de metadados era cortada no meio

**O que acontecia:** aparecia *"2,1 mil visualizações • Transmitido."* — sem o
"há 2 dias".

**Causa raiz:** o texto tem 278px de largura natural, mas a coluna de texto do
card tem 230px. Com `white-space: nowrap`, ele era cortado com reticências em vez
de quebrar linha.

**Correção:** remover o `nowrap` só da linha de metadados, deixando ela quebrar
em duas linhas — que é o que o YouTube faz.

```css
.card__meta {
    font-size: 14px;
    color: var(--cor-texto-suave);
    /* sem white-space: nowrap — quebra em duas linhas */
}
```

O nome do canal mantém o `nowrap`, porque é curto e não deve quebrar.

### O que isso mudou no layout

| Medida | Antes | Depois |
| --- | --- | --- |
| Miniatura | 278×270 | **278×156** |
| Proporção da miniatura | 1,030 | **1,778** |
| Corte da imagem | 42% da largura | **nenhum** |
| Altura do card | 371px | **~261px** |
| Metadados | cortados | **completos** |
| Textos cortados na página | 8 | **0** |

---

## Como visualizar

```bash
xdg-open index.html   # Linux
```

Não precisa de servidor local nem de serviço externo: os ícones são SVG
embutidos no HTML (sem Font Awesome) e as imagens são arquivos do projeto.

---

## Sobre a identidade visual

O desafio sugere dar uma identidade própria ao projeto. As cores e medidas estão
todas em variáveis CSS no bloco `:root`, então trocar a identidade é alterar um
único bloco:

```css
:root {
    --cor-fundo: #f9f9f9;
    --cor-superficie: #ffffff;
    --cor-texto: #0f0f0f;
    --cor-texto-suave: #606060;
    --largura-lateral: 218px;
    --largura-miniatura: 250px;
    --vao-coluna: 46px;
}
```

Esta versão mantém a identidade **do design do Figma** (modo claro) para que a
fidelidade ao protótipo possa ser conferida.

---

## Referências

- [DIO](https://www.dio.me/)
- [Protótipo no Figma](https://www.figma.com/design/KknwioExyqKD3D2eSVFrcW/Desafio-Grid---DIO)
- [MDN — CSS Grid Layout](https://developer.mozilla.org/pt-BR/docs/Web/CSS/CSS_grid_layout)
- [MDN — `grid-template-columns`](https://developer.mozilla.org/pt-BR/docs/Web/CSS/grid-template-columns)
- [MDN — `minmax()`](https://developer.mozilla.org/pt-BR/docs/Web/CSS/minmax)
- [MDN — `repeat()` e `auto-fill`](https://developer.mozilla.org/pt-BR/docs/Web/CSS/repeat)
- [Complete Guide to Grid (CSS-Tricks)](https://css-tricks.com/snippets/css/complete-guide-grid/)

---

Feito durante a Trilha de CSS da DIO — Desafio 03.
