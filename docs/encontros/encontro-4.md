# Encontro 4 — Layouts com Tailwind CSS

**Unidade:** Unidade 1
**Carga horária:** 1,5h

## Visão geral

No Encontro 3, você aplicou cores, tipografia, espaçamento, dimensões, bordas e sombras a um cartão de curso. Agora esse componente será inserido em uma página com vários elementos. O desafio deixa de ser apenas estilizar uma caixa e passa a ser **organizar relações espaciais entre conteúdos**.

Neste encontro, Flexbox e Grid serão usados de acordo com o problema que resolvem. Flexbox organiza itens principalmente em uma dimensão, como os links de uma navegação. Grid organiza linhas e colunas, como uma coleção de cartões. `gap`, alinhamento e posicionamento complementam esses sistemas, mas não substituem uma estrutura HTML adequada.

## Objetivos

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

Continue no projeto `encontro-02-tailwind`, já configurado com Docker no Encontro 3. A raiz do projeto deve conter `Dockerfile`, `compose.yaml`, `package.json` e `package-lock.json`; os arquivos da interface permanecem em `src`.

Se `package.json` e `package-lock.json` não foram alterados desde o encontro anterior, inicie o contêiner com:

```bash
docker compose up
```

O serviço `tailwind` executa `npm run dev` dentro do contêiner. O volume definido em `compose.yaml` compartilha o diretório do projeto com `/app`; por isso, as alterações feitas em `src/index.html` são detectadas pela CLI e o arquivo `src/output.css` é atualizado no computador.

Se a imagem ainda não tiver sido construída ou se as dependências tiverem mudado, reconstrua antes de iniciar:

```bash
docker compose up --build
```

Mantenha esse terminal aberto durante a aula. Uma mensagem de compilação no log indica que a CLI está observando os arquivos. Em outro terminal, confirme o estado do serviço quando necessário:

```bash
docker compose ps
```

Abra `src/index.html` no navegador e verifique se ele referencia a folha gerada:

```html
<link rel="stylesheet" href="./output.css" />
```

Ao adicionar ou remover uma classe Tailwind, salve o HTML e confirme que a data de modificação de `src/output.css` mudou. Não edite esse arquivo manualmente: ele é um artefato produzido dentro do contêiner.

Ao final do encontro, pressione `Ctrl+C` no terminal que executa o serviço e remova o contêiner:

```bash
docker compose down
```

O código e o CSS gerado permanecem no projeto porque estão no diretório compartilhado. O comando `down` remove o contêiner e a rede do Compose, não os arquivos da atividade.

## 2. Retomada do código do Encontro 3

O ponto de partida deste encontro não é um arquivo vazio. Abra `src/index.html` e confirme que ele ainda contém a página de perfil construída no Encontro 3:

```html
<body class="min-h-screen bg-slate-100 text-slate-900">
  <main class="mx-auto max-w-2xl p-6">
    <article class="w-full rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
      <p class="text-sm font-semibold text-sky-700">Formação frontend</p>

      <h1 class="mt-2 text-3xl font-bold text-slate-900">
        Interfaces com Tailwind CSS
      </h1>

      <p class="mt-4 max-w-prose text-base leading-7 text-slate-600">
        Aprenda a transformar decisões de design em utilitários pequenos,
        previsíveis e reutilizáveis.
      </p>

      <p class="mt-4 text-sm font-medium text-slate-500">
        12 encontros · nível introdutório
      </p>

      <a
        class="mt-6 inline-flex rounded-md bg-sky-700 px-4 py-3 font-semibold text-white hover:bg-sky-800 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-sky-700"
        href="#conteudo"
      >
        Ver conteúdo
      </a>
    </article>
  </main>
</body>
```

Antes de continuar, abra a página e confirme que cores, espaçamentos, largura, borda, sombra e foco continuam iguais ao resultado anterior. Esse é o **checkpoint inicial**: se a interface não estiver correta agora, verifique o log do contêiner e a referência a `output.css` antes de acrescentar o layout.

### Alterando o exemplo anterior

No Encontro 3, o nome do curso era o assunto principal da página e, por isso, utilizava `h1`. No catálogo, a página terá o título principal “Cursos em destaque”, enquanto cada curso será uma subseção. Faça estas alterações no cartão existente:

1. troque o `h1` do cartão por `h2`;
2. altere o texto introdutório “Formação frontend” para a categoria “Tailwind CSS”;
3. troque o texto do link por “Conhecer curso” e o destino por `#tailwind`;
4. remova `max-w-prose` do parágrafo, pois a largura passará a ser limitada pela coluna do Grid;
5. preserve as classes de cor, tipografia, espaçamento, borda e sombra estudadas no encontro anterior.

O cartão adaptado deve ficar assim:

```html
<article class="w-full rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
  <p class="text-sm font-semibold text-sky-700">Tailwind CSS</p>
  <h2 class="mt-2 text-xl font-bold text-slate-900">
    Interfaces com utilitários
  </h2>
  <p class="mt-3 text-base leading-7 text-slate-600">
    Construa interfaces consistentes a partir de decisões pequenas.
  </p>
  <p class="mt-4 text-sm font-medium text-slate-500">
    12 encontros · nível introdutório
  </p>
  <a
    class="mt-6 inline-flex rounded-md bg-sky-700 px-4 py-3 font-semibold text-white hover:bg-sky-800 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-sky-700"
    href="#tailwind"
  >
    Conhecer curso
  </a>
</article>
```

### Expandir a página

Agora acrescente o cabeçalho, o título geral e dois novos cartões. Duplique o `article` adaptado e altere apenas categoria, título, descrição e destino do link para TypeScript e Angular. Envolva os três cartões com `div id="cursos"`.

```html
<body class="min-h-screen bg-slate-100 text-slate-900">
  <header>
    <a href="#inicio">Frontend IFRN</a>
    <nav aria-label="Navegação principal">
      <a href="#cursos">Cursos</a>
      <a href="#agenda">Agenda</a>
      <a href="#contato">Contato</a>
    </nav>
  </header>

  <main id="inicio" class="mx-auto max-w-2xl p-6">
    <section aria-labelledby="titulo-cursos">
      <div>
        <div>
          <p>Formação frontend</p>
          <h1 id="titulo-cursos">Cursos em destaque</h1>
        </div>
        <a href="#todos">Ver todos</a>
      </div>

      <div id="cursos">
        <article class="w-full rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
          <p class="text-sm font-semibold text-sky-700">Tailwind CSS</p>
          <h2 class="mt-2 text-xl font-bold text-slate-900">
            Interfaces com utilitários
          </h2>
          <p class="mt-3 text-base leading-7 text-slate-600">
            Construa interfaces consistentes a partir de decisões pequenas.
          </p>
          <p class="mt-4 text-sm font-medium text-slate-500">
            12 encontros · nível introdutório
          </p>
          <a
            class="mt-6 inline-flex rounded-md bg-sky-700 px-4 py-3 font-semibold text-white hover:bg-sky-800 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-sky-700"
            href="#tailwind"
          >
            Conhecer curso
          </a>
        </article>

        <article class="w-full rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
          <p class="text-sm font-semibold text-sky-700">TypeScript</p>
          <h2 class="mt-2 text-xl font-bold text-slate-900">
            Tipos para aplicações web
          </h2>
          <p class="mt-3 text-base leading-7 text-slate-600">
            Modele dados e torne contratos explícitos no código.
          </p>
          <p class="mt-4 text-sm font-medium text-slate-500">
            10 encontros · nível introdutório
          </p>
          <a
            class="mt-6 inline-flex rounded-md bg-sky-700 px-4 py-3 font-semibold text-white hover:bg-sky-800 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-sky-700"
            href="#typescript"
          >
            Conhecer curso
          </a>
        </article>

        <article class="w-full rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
          <p class="text-sm font-semibold text-sky-700">Angular</p>
          <h2 class="mt-2 text-xl font-bold text-slate-900">
            Aplicações orientadas a componentes
          </h2>
          <p class="mt-3 text-base leading-7 text-slate-600">
            Organize interfaces, estado, rotas e comunicação com APIs.
          </p>
          <p class="mt-4 text-sm font-medium text-slate-500">
            18 encontros · nível intermediário
          </p>
          <a
            class="mt-6 inline-flex rounded-md bg-sky-700 px-4 py-3 font-semibold text-white hover:bg-sky-800 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-sky-700"
            href="#angular"
          >
            Conhecer curso
          </a>
        </article>
      </div>
    </section>
  </main>
</body>
```

## 3. Como ler utilitários de layout

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

## 4. Flexbox: relações em uma dimensão

Flexbox distribui os filhos diretos ao longo de um **eixo principal**. Em `flex-row`, esse eixo é horizontal; em `flex-col`, é vertical. O eixo transversal cruza o principal.

### Construir o cabeçalho

Parta do cabeçalho sem classes criado na expansão. Não aplique todas as classes de uma vez.

#### Passo 1 — Recuperar decisões visuais conhecidas

Borda, fundo, tipografia e espaçamento já foram estudados no Encontro 3. Aplique primeiro apenas essas decisões:

```html
<header class="border-b border-slate-200 bg-white">
  <div class="px-6 py-5">
    <a class="font-bold text-slate-900" href="#inicio">Frontend IFRN</a>
    <nav aria-label="Navegação principal">...</nav>
  </div>
</header>
```

O novo `div` agrupa o conteúdo interno. `px-6 py-5` cria o preenchimento nesse grupo, enquanto a borda e o fundo permanecem no `header` e podem ocupar toda a largura.

#### Passo 2 — Definir largura e centralização

Ainda com utilitários conhecidos, limite o conteúdo do cabeçalho com a mesma lógica de dimensões usada no encontro anterior:

```html
<div class="mx-auto max-w-6xl px-6 py-5">
  <!-- identificação e navegação -->
</div>
```

- `max-w-6xl` estabelece uma largura máxima maior, adequada ao catálogo;
- `mx-auto` distribui as margens horizontais quando sobra espaço;
- `px-6 py-5` mantém o conteúdo afastado das bordas.

Altere também o `main` de `max-w-2xl` para `max-w-6xl`. Essa mudança amplia a área disponível para as colunas que serão criadas com Grid.

#### Passo 3 — Introduzir Flexbox no grupo principal

Agora adicione somente `flex` ao `div` interno:

```html
<div class="mx-auto flex max-w-6xl px-6 py-5">
  <a class="font-bold text-slate-900" href="#inicio">Frontend IFRN</a>
  <nav aria-label="Navegação principal">...</nav>
</div>
```

Compare antes e depois. `flex` muda o `display` do contêiner e coloca seus dois filhos diretos — o link de identificação e o `nav` — no eixo principal horizontal. Ele não organiza diretamente os links que estão dentro do `nav`.

#### Passo 4 — Distribuir e alinhar os dois grupos

Acrescente uma classe por vez ao mesmo contêiner:

```html
<div class="mx-auto flex max-w-6xl items-center justify-between gap-6 px-6 py-5">
  <!-- identificação e navegação -->
</div>
```

- `items-center` alinha os dois filhos no centro do eixo transversal;
- `justify-between` coloca o espaço livre entre os dois grupos;
- `gap-6` garante um intervalo mínimo quando o espaço livre diminui.

Após cada classe, salve o arquivo e observe a mudança antes de adicionar a próxima.

#### Passo 5 — Organizar os links da navegação

O `flex` externo não alcança os netos. Para organizar os links, transforme o próprio `nav` em outro contêiner Flexbox:

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

- o segundo `flex` afeta somente os links, que são filhos diretos do `nav`;
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

## 6. Grid: linhas e colunas

Grid é apropriado quando os itens compartilham uma estrutura bidimensional:

### Passo 1 — Transformar o contêiner em Grid

Adicione primeiro apenas `grid` ao contêiner dos cartões:

```html
<div class="grid" id="cursos">
  <!-- três cartões preservados -->
</div>
```

O contêiner passa a usar `display: grid`, mas ainda possui uma única coluna implícita. Por isso, a aparência muda pouco. A inspeção no DevTools confirma que os três `article` agora são itens do Grid.

### Passo 2 — Criar as colunas

Acrescente `grid-cols-3`:

```html
<div class="grid grid-cols-3" id="cursos">
  <!-- três cartões preservados -->
</div>
```

Agora os cartões ocupam três colunas equivalentes, mas ainda não existe intervalo entre elas.

### Passo 3 — Controlar o intervalo no contêiner

Por último, adicione `gap-6`:

```html
<div class="grid grid-cols-3 gap-6" id="cursos">
  <article>...</article>
  <article>...</article>
  <article>...</article>
</div>
```

`gap-6` cria o espaço entre linhas e colunas sem adicionar margens aos cartões. Caso alguma margem provisória tenha sido adicionada aos `article`, remova-a agora.

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

## 7. Flexbox ou Grid?

| Situação | Escolha inicial | Motivo |
|---|---|---|
| links da navegação | Flexbox | sequência em uma linha |
| título e ação | Flexbox | dois grupos no mesmo eixo |
| coleção de cartões | Grid | linhas e colunas compartilhadas |
| interior do cartão | Flexbox em coluna | ação acompanha a altura |
| selo sobre imagem | posicionamento | sobreposição intencional |

Uma interface pode aninhar os sistemas: Grid organiza cartões; cada cartão usa Flexbox; a navegação usa outro Flexbox.

## 8. Gap e ritmo

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

## 9. Alinhamento e distribuição

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

## 10. Posicionamento e sobreposição

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

## 11. Exemplo principal completo

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

## 12. Inspeção e diagnóstico

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

## 13. Prática guiada

### Etapa 1 — Estrutura e fluxo

- parta do cartão concluído no Encontro 3 e faça a adaptação semântica para `h2`;
- amplie o HTML sem classes de layout nos novos contêineres, preservando as classes visuais dos cartões;
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

## 14. Exercício aplicado

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
- registrar no README a execução com Docker Compose e a escolha entre Flexbox e Grid.

### Desafio adicional

Adicione imagem e selo sobreposto a um cartão. Use `relative` e `absolute`, teste com zoom e confirme que o selo não cobre texto, ação ou foco.

## 15. Critérios de aceite

- o projeto executa com `docker compose up` e recompila o CSS após alterações no HTML;
- o primeiro cartão reutiliza e adapta o componente concluído no Encontro 3;
- os contêineres e itens de layout são identificáveis;
- Flexbox e Grid correspondem à relação entre os itens;
- intervalos do conjunto são controlados pelo contêiner;
- conteúdos diferentes não são cortados nem sobrepostos;
- a estrutura principal não depende de `absolute`;
- ordem visual, leitura e teclado permanecem coerentes;
- não há rolagem horizontal indevida;
- o README registra os comandos `docker compose up`, `docker compose up --build` e `docker compose down`, além dos testes e decisões.

## 16. Erros comuns

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
