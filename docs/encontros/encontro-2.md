# Encontro 2 — Introdução ao Tailwind CSS

**Unidade:** Unidade 1
**Carga horária:** 1,5h
**Entrega prevista:** projeto Tailwind CSS configurado e primeira interface compilada

## Visão geral

No Encontro 1, você revisou como HTML e CSS participam da construção de uma interface semântica, acessível e responsiva. Agora esses fundamentos serão aplicados por meio do **Tailwind CSS**, um framework que oferece classes utilitárias de propósito específico.

Em vez de criar uma classe CSS como `.cartao-destaque` e reunir nela diversas declarações, você combina utilitários como `p-6`, `border`, `rounded-lg` e `shadow-sm` diretamente no HTML. Cada classe representa uma decisão visual pequena e previsível.

Tailwind não substitui CSS. O framework fornece outro modo de **escrever e organizar decisões de estilo**, mas o navegador continua processando propriedades, valores, cascata, box model, Flexbox, Grid e media queries. Compreender CSS permanece indispensável para escolher utilitários, interpretar o resultado e corrigir problemas.

Neste encontro, o foco não é memorizar classes. O objetivo é compreender o modelo utility-first, configurar o ambiente, acompanhar o processo de compilação e comprovar como o Tailwind detecta classes e gera o CSS utilizado pela página.

## Objetivos de aprendizagem

- explicar o que é Tailwind CSS e a abordagem utility-first;
- relacionar utilitários Tailwind às propriedades CSS correspondentes;
- diferenciar classes utilitárias de estilos inline e de classes semânticas autorais;
- instalar Tailwind CSS e sua CLI em um projeto com npm;
- importar o framework em uma folha CSS de entrada;
- executar os processos de desenvolvimento e build;
- explicar como as classes são detectadas nos arquivos-fonte;
- validar se o HTML carregou o CSS compilado correto;
- diagnosticar erros comuns de configuração e compilação.

## Conceitos essenciais

- abordagem utility-first;
- classes utilitárias;
- instalação local com npm;
- arquivo CSS de entrada e arquivo CSS gerado;
- detecção de classes nos arquivos-fonte;
- compilação em modo de observação;
- build reproduzível;
- separação entre código-fonte e artefato gerado.

## 1. O que é Tailwind CSS

Tailwind CSS é um framework CSS orientado a utilitários. Ele oferece classes pequenas que normalmente controlam uma propriedade ou um conjunto muito restrito de declarações.

```html
<h1 class="text-3xl font-bold text-slate-900">
  Desenvolvimento Web Frontend
</h1>
```

As classes podem ser lidas como decisões de estilo:

| Classe Tailwind | Intenção | CSS aproximado |
|---|---|---|
| `text-3xl` | tamanho do texto | `font-size: 1.875rem` |
| `font-bold` | peso da fonte | `font-weight: 700` |
| `text-slate-900` | cor do texto | valor do token `slate-900` |

O valor exato de alguns utilitários vem do tema do projeto. Isso cria um vocabulário visual compartilhado para cores, espaçamentos, tipografia, sombras e breakpoints.

### Do CSS autoral para utilitários

Uma apresentação poderia ser escrita com uma classe autoral:

```html
<article class="cartao-curso">
  <h2>Interfaces com Tailwind CSS</h2>
</article>
```

```css
.cartao-curso {
  padding: 1.5rem;
  border: 1px solid #e2e8f0;
  border-radius: 0.5rem;
  background: white;
  box-shadow: 0 1px 2px rgb(15 23 42 / 0.08);
}
```

Com Tailwind, as mesmas decisões ficam próximas do elemento:

```html
<article class="rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
  <h2>Interfaces com Tailwind CSS</h2>
</article>
```

O HTML não ficou "sem CSS". As classes são entradas para que o Tailwind gere regras CSS reutilizáveis.

## 2. Por que utility-first

Na abordagem utility-first, uma interface é construída pela composição de primitivas visuais. Essa estratégia altera algumas tarefas comuns:

- reduz a necessidade de inventar nomes para classes puramente visuais;
- torna o impacto de uma alteração mais localizado;
- favorece o uso consistente de valores do tema;
- permite compreender estrutura e apresentação no mesmo trecho;
- reutiliza as mesmas classes em diferentes partes do projeto;
- gera apenas o CSS correspondente aos utilitários detectados.

Isso não significa que todo CSS autoral seja inadequado. Regras específicas, estilos de conteúdo que não está sob seu controle e abstrações realmente repetidas ainda podem justificar CSS próprio ou componentes.

### Utility-first não é estilo inline

As duas abordagens aproximam estilo e marcação, mas não são equivalentes.

```html
<!-- Estilo inline -->
<button style="background: #0369a1; padding: 0.75rem 1rem; color: white">
  Salvar
</button>

<!-- Classes utilitárias -->
<button class="bg-sky-700 px-4 py-3 text-white hover:bg-sky-800 focus-visible:outline-2">
  Salvar
</button>
```

Classes utilitárias:

- utilizam uma escala de design compartilhada;
- podem representar estados como `hover:` e `focus-visible:`;
- podem responder a breakpoints como `md:`;
- podem ser combinadas e reutilizadas;
- são processadas pelo framework durante o build.

Estilo inline ainda pode ser pertinente para valores realmente dinâmicos recebidos de dados, mas não deve substituir um sistema visual coerente.

## 3. Como o Tailwind gera CSS

O Tailwind não entrega ao navegador uma folha estática contendo antecipadamente todas as combinações possíveis. Durante a compilação, ele:

1. lê a folha CSS de entrada;
2. encontra a importação do Tailwind;
3. examina os arquivos-fonte do projeto;
4. identifica tokens que podem representar classes;
5. gera as regras correspondentes aos utilitários reconhecidos;
6. escreve o resultado em um arquivo CSS de saída;
7. repete o processo quando um arquivo muda, se estiver em modo `--watch`.

```text
src/input.css + classes em HTML/JS/TS
                    │
                    ▼
              Tailwind CLI
                    │
                    ▼
             src/output.css
                    │
                    ▼
                 navegador
```

O navegador conhece apenas o CSS gerado. Ele não interpreta Tailwind diretamente.

## 4. Pré-requisitos e estrutura do projeto

Para este encontro, confirme no terminal:

```bash
node --version
npm --version
```

O projeto utilizará esta estrutura:

```text
encontro-02-tailwind/
├── package.json
├── package-lock.json
└── src/
    ├── index.html
    ├── input.css
    └── output.css  # arquivo gerado
```

Responsabilidades:

- `src/index.html`: estrutura e classes utilizadas pela interface;
- `src/input.css`: ponto de entrada para Tailwind e futuros estilos autorais;
- `src/output.css`: resultado produzido pela CLI;
- `package.json`: dependências e comandos reproduzíveis;
- `package-lock.json`: versões resolvidas das dependências.

Não edite `output.css` manualmente. Qualquer mudança direta será perdida na próxima compilação.

## 5. Instalação passo a passo

### Criar o projeto

```bash
mkdir encontro-02-tailwind
cd encontro-02-tailwind
npm init -y
mkdir src
```

### Instalar Tailwind CSS e a CLI

```bash
npm install -D tailwindcss @tailwindcss/cli
```

As dependências são locais ao projeto. Isso torna a instalação reproduzível em outra máquina por meio de `npm install`.

### Criar a folha de entrada

Em `src/input.css`:

```css
@import "tailwindcss";
```

Essa importação inclui o tema padrão, estilos de base e utilitários gerados. Na versão atual do Tailwind CSS, esse é o ponto de partida; configurações antigas com as diretivas `@tailwind base`, `@tailwind components` e `@tailwind utilities` pertencem ao fluxo de versões anteriores.

### Executar a compilação

```bash
npx @tailwindcss/cli -i ./src/input.css -o ./src/output.css --watch
```

Parâmetros:

| Opção | Função |
|---|---|
| `-i` | define o arquivo CSS de entrada |
| `-o` | define o arquivo CSS de saída |
| `--watch` | recompila quando os arquivos mudam |

Mantenha esse processo ativo durante o desenvolvimento. Encerrá-lo interrompe a atualização automática, mas o CSS já gerado continua existindo.

### Criar comandos no `package.json`

Substitua a seção `scripts` por:

```json
{
  "scripts": {
    "dev": "tailwindcss -i ./src/input.css -o ./src/output.css --watch",
    "build": "tailwindcss -i ./src/input.css -o ./src/output.css --minify"
  }
}
```

Depois disso:

```bash
npm run dev
npm run build
```

O script evita depender de um comando memorizado e documenta como o projeto deve ser executado.

## 6. Conectar o CSS ao HTML

Crie `src/index.html`:

```html
<!doctype html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Introdução ao Tailwind CSS</title>
    <link rel="stylesheet" href="./output.css" />
  </head>
  <body>
    <main class="p-6">
      <h1 class="text-3xl font-bold text-slate-900">
        Meu primeiro projeto com Tailwind CSS
      </h1>
      <p class="mt-3 text-slate-600">
        Esta página usa classes detectadas e compiladas localmente.
      </p>
    </main>
  </body>
</html>
```

Abra o HTML no navegador e confirme:

- `output.css` foi criado;
- a aba **Network** não mostra erro ao carregar a folha;
- o título recebeu tamanho, peso e cor;
- o parágrafo possui margem superior e cor diferente;
- alterar uma classe com `npm run dev` ativo atualiza o resultado.

## 7. Lendo classes utilitárias

Uma sequência de classes deve ser lida por categorias, não como um texto único.

```html
<article class="mx-auto max-w-xl rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
  <p class="text-sm font-semibold text-sky-700">Tailwind CSS</p>
  <h2 class="mt-2 text-2xl font-bold text-slate-900">Utility-first</h2>
  <p class="mt-3 leading-7 text-slate-600">
    Classes pequenas são combinadas para formar um componente completo.
  </p>
</article>
```

| Categoria | Classes | Responsabilidade |
|---|---|---|
| dimensão | `max-w-xl` | limita a largura |
| posicionamento | `mx-auto` | centraliza horizontalmente |
| espaçamento | `p-6`, `mt-2`, `mt-3` | controla espaços internos e externos |
| superfície | `bg-white`, `border`, `shadow-sm` | diferencia o cartão do fundo |
| forma | `rounded-lg` | arredonda os cantos |
| tipografia | `text-2xl`, `font-bold`, `leading-7` | controla hierarquia e leitura |
| cor | `text-sky-700`, `text-slate-900` | comunica hierarquia visual |

Mesmo usando utilitários, preserve a semântica. `article`, `h2` e `p` foram escolhidos pelo significado; as classes controlam apresentação.

## 8. Variantes: uma primeira leitura

Variantes aplicam um utilitário quando uma condição é atendida.

```html
<a
  class="inline-flex rounded-md bg-sky-700 px-4 py-3 font-semibold text-white hover:bg-sky-800 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-sky-700 md:px-6"
  href="#conteudo"
>
  Explorar conteúdo
</a>
```

- `hover:bg-sky-800`: altera o fundo quando o ponteiro está sobre o link;
- `focus-visible:outline-2`: mostra contorno quando o foco precisa estar visível;
- `md:px-6`: aumenta o espaçamento horizontal a partir do breakpoint `md`;
- utilitários sem prefixo formam a base aplicada a todas as larguras.

Estados e responsividade serão aprofundados nos encontros seguintes. Aqui, o importante é reconhecer que variantes geram CSS condicional e não dependem de JavaScript.

## 9. Exemplo principal do encontro

Com o processo `npm run dev` ativo, substitua o conteúdo de `src/index.html` pelo exemplo:

```html
<!doctype html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Trilha Frontend</title>
    <link rel="stylesheet" href="./output.css" />
  </head>
  <body class="min-h-screen bg-slate-100 text-slate-900">
    <main class="mx-auto max-w-5xl p-6 md:p-10">
      <header class="max-w-2xl">
        <p class="font-semibold text-sky-700">Desenvolvimento Web Frontend</p>
        <h1 class="mt-2 text-3xl font-bold md:text-4xl">
          Primeiros passos com Tailwind CSS
        </h1>
        <p class="mt-4 leading-7 text-slate-600">
          Uma interface simples para observar utilitários, variantes e o processo de compilação.
        </p>
      </header>

      <section class="mt-8 grid gap-4 md:grid-cols-3" aria-labelledby="titulo-etapas">
        <h2 id="titulo-etapas" class="sr-only">Etapas do encontro</h2>

        <article class="rounded-lg border border-slate-200 bg-white p-5 shadow-sm">
          <p class="text-sm font-semibold text-sky-700">Etapa 1</p>
          <h3 class="mt-2 text-lg font-bold">Instalar</h3>
          <p class="mt-2 text-slate-600">Adicionar Tailwind CSS e a CLI ao projeto.</p>
        </article>

        <article class="rounded-lg border border-slate-200 bg-white p-5 shadow-sm">
          <p class="text-sm font-semibold text-sky-700">Etapa 2</p>
          <h3 class="mt-2 text-lg font-bold">Compilar</h3>
          <p class="mt-2 text-slate-600">Transformar classes detectadas em CSS para o navegador.</p>
        </article>

        <article class="rounded-lg border border-slate-200 bg-white p-5 shadow-sm">
          <p class="text-sm font-semibold text-sky-700">Etapa 3</p>
          <h3 class="mt-2 text-lg font-bold">Validar</h3>
          <p class="mt-2 text-slate-600">Conferir carregamento, resultado visual e acessibilidade.</p>
        </article>
      </section>

      <a
        class="mt-8 inline-flex rounded-md bg-sky-700 px-4 py-3 font-semibold text-white hover:bg-sky-800 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-sky-700"
        href="https://tailwindcss.com/docs"
      >
        Consultar documentação
      </a>
    </main>
  </body>
</html>
```

### O que observar

- o HTML permanece semântico apesar da quantidade de classes;
- `sr-only` mantém o título disponível para tecnologias assistivas;
- as classes sem prefixo definem a experiência inicial;
- `md:grid-cols-3` altera o Grid em telas maiores;
- `hover:` e `focus-visible:` tratam estados distintos;
- somente classes reconhecidas e detectadas geram utilitários;
- mudar ou remover uma classe altera apenas a responsabilidade associada.

## 10. Validação do ambiente

### Verificar o terminal

- o comando permanece ativo sem erro;
- a recompilação ocorre depois de salvar HTML ou CSS;
- `src/output.css` foi criado e possui conteúdo;
- `npm run build` termina com sucesso.

### Verificar o navegador

- `output.css` responde com sucesso na aba **Network**;
- não existem erros relacionados à folha no console;
- as mudanças aparecem depois da recompilação;
- o layout continua legível em viewport estreito e amplo;
- o link possui foco visível e pode ser operado pelo teclado.

### Confirmar a geração sob demanda

1. adicione temporariamente `uppercase` ao título;
2. salve o HTML;
3. confirme a recompilação no terminal;
4. procure `.uppercase` em `output.css`;
5. remova a classe e compile novamente;
6. observe que o artefato acompanha as classes utilizadas.

## 12. Exercício aplicado

Crie uma página de apresentação para uma disciplina, curso, evento ou projeto. O resultado deve conter:

- cabeçalho com categoria, título e descrição;
- uma seção com três cartões semânticos;
- um link ou botão com estado de foco visível;
- fundo, borda, espaçamento, tipografia e cor definidos por utilitários;
- pelo menos uma variante `hover:` ou `focus-visible:`;
- folha CSS gerada pela CLI;
- scripts `dev` e `build` funcionais.

Não copie o exemplo principal integralmente. Altere domínio, conteúdo, hierarquia visual e combinações de utilitários.

## 13. Critérios de aceite

- o projeto é instalado com `npm install`;
- `npm run dev` observa mudanças sem erro;
- `npm run build` gera o CSS de saída;
- o HTML referencia o caminho correto de `output.css`;
- as classes utilizadas aparecem completas nos arquivos-fonte;
- a página usa HTML semântico;
- os utilitários aplicados podem ser explicados por categoria;
- a interface funciona com teclado e em diferentes larguras;
- arquivos de entrada e saída não são confundidos.

## 14. Erros comuns

- instalar apenas `tailwindcss` e esquecer `@tailwindcss/cli`;
- usar instruções de instalação pertencentes a uma versão anterior;
- esquecer `@import "tailwindcss";` em `input.css`;
- apontar o `link` para `input.css`, em vez de `output.css`;
- editar `output.css` manualmente;
- encerrar o processo `--watch` e esperar recompilação automática;
- executar o comando em uma pasta diferente da raiz do projeto;
- construir nomes de classes por concatenação;
- usar classes inexistentes e esperar uma mensagem de erro para cada uma;
- adicionar utilitários conflitantes para a mesma propriedade;
- atribuir ao Tailwind um problema causado por HTML inválido ou CSS mal compreendido;
- copiar muitas classes sem conseguir explicar sua responsabilidade.

## Materiais para aprofundamento

- [Tailwind CSS — instalação com CLI](https://tailwindcss.com/docs/installation/tailwind-cli)
- [Tailwind CSS — classes utilitárias](https://tailwindcss.com/docs/styling-with-utility-classes)
- [Tailwind CSS — detecção de classes](https://tailwindcss.com/docs/detecting-classes-in-source-files)
- [Tailwind CSS — estados e variantes](https://tailwindcss.com/docs/hover-focus-and-other-states)
- [Tailwind CSS — design responsivo](https://tailwindcss.com/docs/responsive-design)
- [npm Docs — scripts](https://docs.npmjs.com/cli/using-npm/scripts)

## Checklist de compreensão

- [ ] Explico Tailwind CSS sem dizer que ele substitui CSS.
- [ ] Relaciono classes utilitárias a propriedades e valores CSS.
- [ ] Diferencio utility-first de estilo inline.
- [ ] Instalo `tailwindcss` e `@tailwindcss/cli` localmente.
- [ ] Distingo o arquivo CSS de entrada do arquivo gerado.
- [ ] Executo os scripts de desenvolvimento e build.
- [ ] Explico por que classes construídas dinamicamente podem não ser detectadas.
- [ ] Confirmo no navegador que o CSS correto foi carregado.
- [ ] Leio uma sequência de utilitários por categorias.
- [ ] Preservo semântica, responsividade e foco visível.

## Resumo final

Neste encontro, você iniciou o uso do Tailwind CSS pela compreensão do processo completo: classes utilitárias são escritas nos arquivos-fonte, detectadas durante a compilação e transformadas em uma folha CSS que o navegador consegue interpretar.

O modelo utility-first aproxima decisões visuais da marcação e oferece uma escala compartilhada, variantes e geração sob demanda. Ainda assim, a qualidade da interface continua dependendo de HTML semântico, fundamentos de CSS, acessibilidade e validação consciente.

Nos próximos encontros, esse ambiente será utilizado para aprofundar cores, tipografia, espaçamentos, layouts, responsividade, estados e temas.

## Questões de fixação

1. Por que Tailwind CSS não substitui o conhecimento de CSS?
<!-- Gabarito: porque os utilitários geram propriedades e valores CSS sujeitos à cascata, box model e algoritmos de layout; compreender esses fundamentos permite escolher classes e diagnosticar resultados. -->

2. Qual é a diferença entre `input.css` e `output.css`?
<!-- Gabarito: input.css é a fonte editável que importa o Tailwind; output.css é o artefato gerado pela CLI e carregado pelo navegador. -->

3. O que o parâmetro `--watch` modifica no processo de compilação?
<!-- Gabarito: mantém a CLI observando alterações nos arquivos para recompilar o CSS automaticamente durante o desenvolvimento. -->

4. Por que `` `bg-${cor}-600` `` pode não gerar a classe esperada?
<!-- Gabarito: Tailwind examina os arquivos como texto e não executa a interpolação; o nome completo da classe precisa aparecer estaticamente no código-fonte. -->

5. Cite duas diferenças entre classes utilitárias e estilos inline.
<!-- Gabarito: utilitários usam uma escala compartilhada, aceitam variantes de estado e responsividade e são processados no build; inline usa valores diretamente no atributo style. -->

6. Como você comprova que uma classe foi detectada e compilada?
<!-- Gabarito: salva o arquivo com o modo watch ativo, confirma a recompilação, verifica o resultado no navegador e, quando necessário, procura a regra correspondente no CSS gerado. -->

[Voltar ao cronograma](../01-cronograma-60h.md)
