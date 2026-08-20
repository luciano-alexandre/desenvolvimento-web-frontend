# Encontro 4 — Layouts com Tailwind CSS

**Unidade:** Unidade 1
**Carga horária:** 1,5h
**Entrega prevista:** página de catálogo com cabeçalho, barra de ações e grade de cartões

## Visão geral

No Encontro 3, você aplicou cores, tipografia, espaçamento, dimensões, bordas e sombras a um cartão de curso. Agora esse componente será inserido em uma página com vários elementos. O desafio deixa de ser apenas estilizar uma caixa e passa a ser **organizar relações espaciais entre conteúdos**.

Neste encontro, Flexbox e Grid serão usados de acordo com o problema que resolvem. Flexbox organiza itens principalmente em uma dimensão, como os links de uma navegação. Grid organiza linhas e colunas, como uma coleção de cartões. `gap`, alinhamento e posicionamento complementam esses sistemas, mas não substituem uma estrutura HTML adequada.

O trabalho será incremental: primeiro será analisado o fluxo normal; depois serão construídos o cabeçalho, a barra de ações e a grade. A cada mudança, identifique o contêiner, seus filhos diretos, o eixo ou as trilhas controladas e o resultado esperado.

## Objetivos de aprendizagem

- explicar como o fluxo normal organiza os elementos antes de um layout;
- reconhecer o contêiner e os itens de Flexbox ou Grid;
- escolher Flexbox para relações predominantemente unidimensionais;
- escolher Grid para estruturas em linhas e colunas;
- relacionar utilitários Tailwind às propriedades CSS correspondentes;
- distribuir espaço com `gap`;
- distinguir alinhamento no eixo principal e no transversal;
- usar posicionamento apenas quando houver sobreposição intencional;
- verificar ordem visual, foco e comportamento em larguras diferentes;
- diagnosticar overflow e alinhamentos inesperados.

## Conceitos essenciais

- fluxo normal do documento;
- contêiner e filhos diretos;
- eixo principal e eixo transversal;
- Flexbox e quebra de linha;
- Grid, colunas e frações;
- `gap` como propriedade do contêiner;
- distribuição e alinhamento;
- posicionamento estático, relativo e absoluto;
- ordem visual, ordem de leitura e acessibilidade.

## 1. Ponto de partida

Continue no projeto `encontro-02-tailwind`. Confirme as dependências e mantenha a compilação ativa:

```bash
npm install
npm run dev
```

Insira esta estrutura no `body` de `src/index.html`, ainda sem classes:

```html
<body>
  <header>
    <a href="#inicio">Frontend IFRN</a>
    <nav aria-label="Navegação principal">
      <a href="#cursos">Cursos</a>
      <a href="#agenda">Agenda</a>
      <a href="#contato">Contato</a>
    </nav>
  </header>

  <main id="inicio">
    <section aria-labelledby="titulo-cursos">
      <div>
        <div>
          <p>Formação frontend</p>
          <h1 id="titulo-cursos">Cursos em destaque</h1>
        </div>
        <a href="#todos">Ver todos</a>
      </div>

      <div id="cursos">
        <article>
          <p>Tailwind CSS</p>
          <h2>Interfaces com utilitários</h2>
          <p>Construa interfaces consistentes a partir de decisões pequenas.</p>
          <a href="#tailwind">Conhecer curso</a>
        </article>
        <article>
          <p>TypeScript</p>
          <h2>Tipos para aplicações web</h2>
          <p>Modele dados e torne contratos explícitos no código.</p>
          <a href="#typescript">Conhecer curso</a>
        </article>
        <article>
          <p>Angular</p>
          <h2>Aplicações orientadas a componentes</h2>
          <p>Organize interfaces, estado, rotas e comunicação com APIs.</p>
          <a href="#angular">Conhecer curso</a>
        </article>
      </div>
    </section>
  </main>
</body>
```

Sem `flex` ou `grid`, os blocos aparecem um depois do outro e crescem conforme o conteúdo. Esse **fluxo normal** não é um erro: ele mantém uma ordem compreensível e oferece uma base estável.

Antes de modificar o exemplo, localize o `header`, a barra de título, o `div#cursos`, seus três `article` e a ordem de leitura que deverá ser preservada.

## 2. Como ler utilitários de layout

Saber **em qual elemento aplicar uma classe** é tão importante quanto escolher a classe.

| Utilitário | CSS aproximado | Aplicado a |
|---|---|---|
| `flex` | `display: flex` | contêiner |
| `flex-col` | `flex-direction: column` | contêiner Flexbox |
| `flex-wrap` | `flex-wrap: wrap` | contêiner Flexbox |
| `grid` | `display: grid` | contêiner |
| `grid-cols-3` | `grid-template-columns: repeat(3, minmax(0, 1fr))` | contêiner Grid |
| `gap-6` | `gap: 1.5rem` | contêiner Flexbox ou Grid |
| `justify-between` | `justify-content: space-between` | contêiner |
| `items-center` | `align-items: center` | contêiner |
| `self-start` | `align-self: flex-start` | item |
| `relative` | `position: relative` | referência |
| `absolute` | `position: absolute` | elemento posicionado |

`flex` e `grid` controlam principalmente os **filhos diretos**. Se a classe não produz o efeito esperado, confirme se está no contêiner correto.

## 3. Fluxo normal antes do layout

Elementos de bloco normalmente iniciam em uma nova linha; elementos inline participam da linha de texto. Essa base:

- mantém a ordem de leitura ligada ao HTML;
- permite que a altura acompanhe o conteúdo;
- oferece apresentação básica mesmo sem os estilos.

Antes de alterar `display`, pergunte:

1. quais elementos precisam ser organizados juntos?
2. a relação principal é uma linha/coluna ou uma grade?
3. o conteúdo pode crescer ou quebrar linha?
4. a ordem visual continuará coerente com teclado e leitor de tela?

Flexbox e Grid devem organizar uma estrutura lógica, não compensar HTML desordenado.

## 4. Flexbox: relações em uma dimensão

Flexbox distribui os filhos diretos ao longo de um **eixo principal**. Em `flex-row`, esse eixo é horizontal; em `flex-col`, é vertical. O eixo transversal cruza o principal.

### Construir o cabeçalho

```html
<div class="mx-auto flex max-w-6xl items-center justify-between gap-6 px-6 py-5">
  <a class="font-bold text-slate-900" href="#inicio">Frontend IFRN</a>
  <nav
    class="flex flex-wrap items-center gap-x-6 gap-y-2"
    aria-label="Navegação principal"
  >
    <a class="text-sm font-medium text-slate-600" href="#cursos">Cursos</a>
    <a class="text-sm font-medium text-slate-600" href="#agenda">Agenda</a>
    <a class="text-sm font-medium text-slate-600" href="#contato">Contato</a>
  </nav>
</div>
```

- `flex` coloca a identificação e o `nav` no mesmo eixo;
- `items-center` alinha os grupos no eixo transversal;
- `justify-between` distribui o espaço livre entre os extremos;
- `gap-6` preserva uma distância mínima;
- `flex-wrap` permite que os links mudem de linha;
- `gap-x-6` e `gap-y-2` controlam os intervalos após a quebra.

`justify-between` não garante distância mínima quando falta espaço; por isso, costuma ser combinado com `gap`.

### Flexbox dentro do cartão

```html
<article class="flex h-full flex-col rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
  <p class="text-sm font-semibold text-sky-700">Tailwind CSS</p>
  <h2 class="mt-2 text-xl font-bold">Interfaces com utilitários</h2>
  <p class="mt-3 leading-7 text-slate-600">
    Construa interfaces consistentes a partir de decisões pequenas.
  </p>
  <a class="mt-auto pt-6 font-semibold text-sky-700" href="#tailwind">
    Conhecer curso
  </a>
</article>
```

O eixo agora é vertical por causa de `flex-col`. `mt-auto` absorve o espaço livre acima do link e o leva ao final. O efeito fica visível quando a grade oferece alturas equivalentes.

## 5. Grid: linhas e colunas

Grid é apropriado quando os itens compartilham uma estrutura bidimensional:

```html
<div class="grid grid-cols-3 gap-6" id="cursos">
  <article>...</article>
  <article>...</article>
  <article>...</article>
</div>
```

`grid-cols-3` cria três colunas equivalentes. Na regra `repeat(3, minmax(0, 1fr))`:

- `repeat(3, ...)` repete a trilha três vezes;
- `1fr` representa uma fração do espaço disponível;
- `minmax(0, 1fr)` permite que a trilha encolha.

Em telas estreitas, três colunas fixas podem esmagar o texto. Uma grade que se ajusta ao espaço pode ser escrita assim:

```html
<div class="grid grid-cols-[repeat(auto-fit,minmax(min(100%,18rem),1fr))] gap-6">
  <!-- cartões -->
</div>
```

`auto-fit` cria quantas colunas couberem; `18rem` é a medida mínima desejada; `min(100%,18rem)` evita overflow; e `1fr` distribui o espaço restante. O Encontro 5 aprofundará as variantes responsivas.

## 6. Flexbox ou Grid?

| Situação | Escolha inicial | Motivo |
|---|---|---|
| links da navegação | Flexbox | sequência em uma linha |
| título e ação | Flexbox | dois grupos no mesmo eixo |
| coleção de cartões | Grid | linhas e colunas compartilhadas |
| interior do cartão | Flexbox em coluna | ação acompanha a altura |
| selo sobre imagem | posicionamento | sobreposição intencional |

Uma interface pode aninhar os sistemas: Grid organiza cartões; cada cartão usa Flexbox; a navegação usa outro Flexbox.

## 7. Gap e ritmo

`gap` pertence ao contêiner e cria espaço **entre** seus itens, sem adicionar espaço às bordas externas.

| Classe | Resultado |
|---|---|
| `gap-6` | intervalo nos dois eixos |
| `gap-x-6` | intervalo horizontal |
| `gap-y-4` | intervalo vertical |

```html
<!-- Itens acoplados à posição -->
<nav>
  <a class="mr-6" href="#cursos">Cursos</a>
  <a class="mr-6" href="#agenda">Agenda</a>
  <a href="#contato">Contato</a>
</nav>

<!-- O contêiner controla a relação -->
<nav class="flex gap-6">
  <a href="#cursos">Cursos</a>
  <a href="#agenda">Agenda</a>
  <a href="#contato">Contato</a>
</nav>
```

Margens continuam adequadas para separar blocos específicos, como `mt-8` entre a introdução e a grade. `gap` resolve a relação interna de um conjunto.

## 8. Alinhamento e distribuição

“Centralizar” é uma descrição incompleta: indique **o que**, **em qual eixo** e **dentro de qual espaço**.

| Utilitário | Responsabilidade |
|---|---|
| `justify-start` | início do eixo principal |
| `justify-center` | centro do eixo principal |
| `justify-between` | espaço livre entre itens |
| `items-start` | início do eixo transversal |
| `items-center` | centro do eixo transversal |
| `place-items-center` | dois eixos das células Grid |
| `self-start` | alinhamento específico de um item |

```html
<div class="flex items-end justify-between gap-6">
  <div>
    <p class="text-sm font-semibold text-sky-700">Formação frontend</p>
    <h1 id="titulo-cursos" class="mt-2 text-3xl font-bold">
      Cursos em destaque
    </h1>
  </div>
  <a class="shrink-0 font-semibold text-sky-700" href="#todos">Ver todos</a>
</div>
```

`items-end` alinha grupo e link pela extremidade transversal; `shrink-0` evita comprimir a ação. Reduza a largura e registre quando a composição deixa de ser confortável.

## 9. Posicionamento e sobreposição

Todo elemento começa com `position: static`. `relative` mantém o elemento no fluxo e cria uma referência para um descendente `absolute`. O absoluto sai do fluxo.

```html
<div class="relative">
  <img
    class="h-40 w-full rounded-lg object-cover"
    src="./img/laboratorio.jpg"
    alt="Estudantes trabalhando em um laboratório de informática"
    width="640"
    height="320"
  />
  <span class="absolute left-3 top-3 rounded-full bg-sky-700 px-3 py-1 text-sm font-semibold text-white">
    Novo
  </span>
</div>
```

Sem o ancestral `relative`, o selo pode usar outra referência e aparecer longe da imagem. Não use `absolute` para estruturar a página: ele não reserva espaço e pode sobrepor conteúdo quando o texto cresce.

Antes de usá-lo, confirme:

- há sobreposição intencional?
- o ancestral correto possui `relative`?
- o texto pode crescer sem cobrir outro conteúdo?
- o elemento não cobre controles ou indicações de foco?
- a composição continua utilizável com zoom?

## 10. Exemplo principal completo

```html
<!doctype html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Catálogo de cursos</title>
    <link rel="stylesheet" href="./output.css" />
  </head>
  <body class="min-h-screen bg-slate-100 text-slate-900">
    <header class="border-b border-slate-200 bg-white">
      <div class="mx-auto flex max-w-6xl items-center justify-between gap-6 px-6 py-5">
        <a class="font-bold" href="#inicio">Frontend IFRN</a>
        <nav class="flex flex-wrap items-center gap-x-6 gap-y-2" aria-label="Navegação principal">
          <a class="text-sm font-medium text-slate-600 hover:text-sky-700" href="#cursos">Cursos</a>
          <a class="text-sm font-medium text-slate-600 hover:text-sky-700" href="#agenda">Agenda</a>
          <a class="text-sm font-medium text-slate-600 hover:text-sky-700" href="#contato">Contato</a>
        </nav>
      </div>
    </header>

    <main id="inicio" class="mx-auto max-w-6xl px-6 py-12">
      <section aria-labelledby="titulo-cursos">
        <div class="flex items-end justify-between gap-6">
          <div>
            <p class="text-sm font-semibold text-sky-700">Formação frontend</p>
            <h1 id="titulo-cursos" class="mt-2 text-3xl font-bold">Cursos em destaque</h1>
          </div>
          <a
            class="shrink-0 font-semibold text-sky-700 hover:text-sky-800 focus-visible:outline-2 focus-visible:outline-offset-4"
            href="#todos"
          >Ver todos</a>
        </div>

        <div id="cursos" class="mt-8 grid grid-cols-[repeat(auto-fit,minmax(min(100%,18rem),1fr))] gap-6">
          <article class="flex h-full flex-col rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
            <p class="text-sm font-semibold text-sky-700">Tailwind CSS</p>
            <h2 class="mt-2 text-xl font-bold">Interfaces com utilitários</h2>
            <p class="mt-3 leading-7 text-slate-600">Construa interfaces consistentes a partir de decisões pequenas.</p>
            <a class="mt-auto pt-6 font-semibold text-sky-700" href="#tailwind">Conhecer curso</a>
          </article>
          <article class="flex h-full flex-col rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
            <p class="text-sm font-semibold text-sky-700">TypeScript</p>
            <h2 class="mt-2 text-xl font-bold">Tipos para aplicações web</h2>
            <p class="mt-3 leading-7 text-slate-600">Modele dados e torne contratos explícitos no código.</p>
            <a class="mt-auto pt-6 font-semibold text-sky-700" href="#typescript">Conhecer curso</a>
          </article>
          <article class="flex h-full flex-col rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
            <p class="text-sm font-semibold text-sky-700">Angular</p>
            <h2 class="mt-2 text-xl font-bold">Aplicações orientadas a componentes</h2>
            <p class="mt-3 leading-7 text-slate-600">Organize interfaces, estado, rotas e comunicação com APIs.</p>
            <a class="mt-auto pt-6 font-semibold text-sky-700" href="#angular">Conhecer curso</a>
          </article>
        </div>
      </section>
    </main>
  </body>
</html>
```

### O que observar

- o HTML mantém ordem lógica mesmo sem CSS;
- o cabeçalho organiza dois grupos com Flexbox;
- a navegação pode quebrar linha;
- a barra de título preserva um `gap` mínimo;
- o Grid decide quantas colunas cabem;
- cada cartão usa Flexbox em coluna para alinhar a ação;
- `gap` controla relações internas e `mt-8` separa blocos;
- não há posicionamento absoluto na estrutura principal;
- a ordem de tabulação acompanha o HTML.

## 11. Inspeção e diagnóstico

### Verificar no DevTools

1. selecione o cabeçalho e confirme `display: flex`;
2. ative a visualização de Flexbox e identifique os eixos;
3. desative `justify-content` e observe o espaço livre;
4. selecione `#cursos`, ative a sobreposição de Grid e conte as colunas;
5. altere `gap` e compare o ritmo;
6. confirme que um cartão é item do Grid e contêiner Flexbox;
7. reduza a viewport e procure overflow horizontal;
8. percorra os links com `Tab` e confirme ordem e foco.

### Experimentos orientados

- remova `flex` do cabeçalho e identifique o fluxo normal;
- troque a direção por `flex-col` e localize os novos eixos;
- remova `flex-wrap` da navegação e reduza a largura;
- substitua a grade fluida por `grid-cols-3` e encontre seu limite;
- remova `h-full` ou `mt-auto` e compare as ações;
- aplique `items-center` à grade e observe o que é alinhado;
- teste um `absolute` sem ancestral `relative` e investigue a referência.

Reverta cada experimento e explique qual propriedade produziu o comportamento.

## 12. Prática guiada

### Etapa 1 — Estrutura e fluxo

- insira o HTML sem classes de layout;
- confira a hierarquia de títulos e o nome da navegação;
- percorra os links com teclado;
- descreva a ordem no fluxo normal.

### Etapa 2 — Flexbox

- transforme cabeçalho e navegação em contêineres Flexbox;
- distribua título e ação da seção;
- reduza a largura e registre o primeiro desconforto.

### Etapa 3 — Grid

- transforme `#cursos` em Grid;
- comece com `grid-cols-3 gap-6`;
- observe as trilhas no DevTools;
- troque para a grade fluida e compare.

### Etapa 4 — Cartões

- aplique `flex h-full flex-col`;
- use `mt-auto pt-6` nas ações;
- aumente o texto de um cartão;
- confirme que nada é cortado.

## 13. Exercício aplicado

Crie uma página para eventos, projetos, notícias, produtos ou serviços sem copiar integralmente o exemplo.

### Requisitos mínimos

- usar HTML semântico e ordem de leitura coerente;
- usar Flexbox em duas relações unidimensionais;
- usar Grid em uma coleção com pelo menos quatro itens;
- controlar intervalos com `gap`;
- explicar as escolhas de alinhamento;
- variar o texto para testar crescimento;
- usar posicionamento somente se houver sobreposição real;
- manter foco visível e ordem de teclado previsível;
- não apresentar overflow nem erros de compilação ou console;
- registrar no README a execução e a escolha entre Flexbox e Grid.

### Desafio adicional

Adicione imagem e selo sobreposto a um cartão. Use `relative` e `absolute`, teste com zoom e confirme que o selo não cobre texto, ação ou foco.

## 14. Critérios de aceite

- o projeto executa com `npm run dev`;
- os contêineres e itens de layout são identificáveis;
- Flexbox e Grid correspondem à relação entre os itens;
- intervalos do conjunto são controlados pelo contêiner;
- conteúdos diferentes não são cortados nem sobrepostos;
- a estrutura principal não depende de `absolute`;
- ordem visual, leitura e teclado permanecem coerentes;
- não há rolagem horizontal indevida;
- o README registra execução, testes e decisões.

## 15. Erros comuns

### Aplicar `flex` ou `grid` ao elemento errado

As propriedades organizam filhos diretos. Inspecione a árvore e mova a classe ao ancestral que reúne os itens.

### Confundir `justify-*` com `items-*`

Identifique primeiro o eixo principal. Em `flex-col`, `justify-*` atua verticalmente e `items-*`, horizontalmente.

### Usar `justify-between` sem `gap`

Quando há pouco espaço livre, a distribuição não garante distância mínima.

### Fixar colunas sem testar o conteúdo

`grid-cols-3` sempre solicita três colunas e pode comprimir conteúdo em telas estreitas.

### Colocar margens em todos os itens

Isso acopla cada item à posição. Para intervalos internos, centralize a decisão com `gap`.

### Reordenar apenas a apresentação

A ordem visual pode divergir da leitura assistiva e do foco. Organize primeiro o HTML.

### Usar `absolute` para construir a página

Elementos absolutos saem do fluxo e podem se sobrepor quando o conteúdo cresce.

### Fixar a altura de textos

Alturas rígidas podem cortar traduções, zoom ou dados maiores. Prefira altura automática.

## Materiais para aprofundamento

- [Tailwind CSS — Flexbox](https://tailwindcss.com/docs/flex)
- [Tailwind CSS — Grid Template Columns](https://tailwindcss.com/docs/grid-template-columns)
- [Tailwind CSS — Gap](https://tailwindcss.com/docs/gap)
- [MDN — Conceitos básicos de Flexbox](https://developer.mozilla.org/pt-BR/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox)
- [MDN — Conceitos básicos de Grid](https://developer.mozilla.org/pt-BR/docs/Web/CSS/CSS_grid_layout/Basic_concepts_of_grid_layout)
- [MDN — Position](https://developer.mozilla.org/pt-BR/docs/Web/CSS/position)

## Checklist de compreensão

- [ ] Consigo identificar contêiner e itens de cada layout.
- [ ] Consigo explicar o fluxo normal.
- [ ] Consigo escolher Flexbox para uma dimensão.
- [ ] Consigo escolher Grid para linhas e colunas.
- [ ] Consigo explicar os eixos principal e transversal.
- [ ] Consigo usar `gap` como decisão do contêiner.
- [ ] Consigo distinguir distribuição e alinhamento.
- [ ] Consigo explicar quando `relative` e `absolute` são apropriados.
- [ ] Consigo diagnosticar overflow e desalinhamento com DevTools.
- [ ] Consigo manter leitura, apresentação e foco coerentes.
- [ ] Revisei a entrega pelos critérios de aceite.

## Resumo final

Layouts começam pela relação entre elementos. O fluxo normal fornece uma estrutura estável; Flexbox organiza sequências em um eixo; Grid coordena linhas e colunas; `gap` mantém o ritmo; alinhamento distribui itens; e posicionamento resolve sobreposições específicas.

No exemplo, Flexbox aparece no cabeçalho e nos cartões, Grid organiza a coleção e o fluxo normal separa as grandes seções. Essa composição prepara a interface para o Encontro 5, quando variantes responsivas adaptarão o layout ao espaço disponível.

## Questões de fixação

1. Qual é a diferença entre um contêiner de layout e seus itens diretos?
<!-- Gabarito: o contêiner recebe flex ou grid e define regras coletivas; seus filhos diretos participam do layout. -->

2. Por que Flexbox foi usado no cabeçalho e Grid na coleção?
<!-- Gabarito: o cabeçalho organiza grupos em uma linha; a coleção compartilha linhas e colunas. -->

3. Como os eixos mudam com `flex-col`?
<!-- Gabarito: o principal passa a ser vertical e o transversal, horizontal. -->

4. Quando `gap` é preferível a margens?
<!-- Gabarito: quando o espaço representa a relação interna entre itens do mesmo contêiner. -->

5. Por que combinar `justify-between` e `gap`?
<!-- Gabarito: space-between distribui apenas o espaço livre e não garante distância mínima. -->

6. Qual problema pode ocorrer com `grid-cols-3` em qualquer largura?
<!-- Gabarito: as colunas podem comprimir conteúdo ou provocar overflow em telas estreitas. -->

7. Por que não corrigir a ordem do HTML apenas com classes visuais?
<!-- Gabarito: apresentação, leitura assistiva e sequência de foco podem divergir. -->

8. Quando `absolute` é adequado e qual referência deve ser verificada?
<!-- Gabarito: em sobreposição intencional; deve existir um ancestral posicionado, normalmente relative. -->

[Voltar ao cronograma](../01-cronograma-60h.md)
