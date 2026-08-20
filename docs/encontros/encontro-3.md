# Encontro 3 — Utilitários fundamentais

**Unidade:** Unidade 1
**Carga horária:** 1,5h
**Entrega prevista:** página de perfil de curso estilizada com utilitários fundamentais

## Antes de começar: Tailwind CSS com Docker

Sim, o Tailwind CSS pode ser usado com Docker. Nesse caso, o contêiner fornece o Node.js, instala as dependências e executa a CLI do Tailwind. Assim, não é necessário instalar Node.js e npm diretamente no computador; basta ter Docker e Docker Compose.

Esta configuração é opcional. Quem já preparou o ambiente no Encontro 2 pode continuar usando `npm install` e `npm run dev` normalmente.

### Criar um novo projeto usando Node.js com Docker

Caso o projeto do Encontro 2 ainda não exista, crie uma pasta e entre nela:

```bash
mkdir encontro-02-tailwind
cd encontro-02-tailwind
```

Em Linux ou macOS, use um contêiner temporário do Node.js para criar `package.json` sem instalar Node.js na máquina:

```bash
docker run --rm --user "$(id -u):$(id -g)" \
  --volume "$PWD:/app" \
  --workdir /app \
  node:22-alpine npm init -y
```

O contêiner é removido depois do comando por causa de `--rm`, mas o arquivo criado permanece na pasta compartilhada. A opção `--user` faz com que os arquivos pertençam ao usuário atual, evitando problemas de permissão.

Instale Tailwind CSS e sua CLI da mesma forma:

```bash
docker run --rm --user "$(id -u):$(id -g)" \
  --volume "$PWD:/app" \
  --workdir /app \
  node:22-alpine npm install -D tailwindcss @tailwindcss/cli
```

Crie a pasta `src` e os arquivos iniciais:

```bash
mkdir src
touch src/index.html src/input.css
```

Em `src/input.css`, adicione:

```css
@import "tailwindcss";
```

No `package.json`, substitua a seção `scripts` pelos comandos usados no Encontro 2:

```json
{
  "scripts": {
    "dev": "tailwindcss -i ./src/input.css -o ./src/output.css --watch",
    "build": "tailwindcss -i ./src/input.css -o ./src/output.css --minify"
  }
}
```

Esses passos produzem a estrutura mínima necessária para continuar a configuração com Docker Compose. No Windows, os comandos de volume e identificação do usuário variam conforme PowerShell, Prompt de Comando ou WSL; usando WSL, os comandos anteriores podem ser executados sem alteração.

### Preparar o contêiner de desenvolvimento

No projeto `encontro-02-tailwind`, crie um arquivo chamado `Dockerfile`:

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .

CMD ["npm", "run", "dev"]
```

Crie também o arquivo `.dockerignore` para não enviar dependências e arquivos gerados ao contexto de construção:

```text
node_modules
src/output.css
.git
```

Em seguida, crie `compose.yaml`:

```yaml
services:
  tailwind:
    build: .
    volumes:
      - .:/app
      - /app/node_modules
```

Na raiz do projeto, construa a imagem e inicie o processo de desenvolvimento:

```bash
docker compose up --build
```

O diretório do projeto é compartilhado com o contêiner. Portanto, ao editar `src/index.html`, a CLI em execução detecta as classes e atualiza `src/output.css` no próprio projeto. Abra `src/index.html` no navegador e mantenha o terminal ativo durante as alterações.

Para encerrar, pressione `Ctrl+C` e remova o contêiner criado:

```bash
docker compose down
```

Nas próximas execuções, quando `package.json` e `package-lock.json` não tiverem mudado, basta usar:

```bash
docker compose up
```

Se as dependências forem alteradas, execute novamente `docker compose up --build`. O Docker isola o ambiente de execução, mas não muda o funcionamento do Tailwind: as classes continuam sendo identificadas nos arquivos-fonte e transformadas em CSS pela CLI.

## Visão geral

No Encontro 2, você configurou o Tailwind CSS, acompanhou a geração do arquivo CSS e aprendeu a ler classes utilitárias por responsabilidade. Agora o mesmo projeto será usado para aprofundar as decisões visuais mais frequentes em uma interface: **cores, tipografia, espaçamento, dimensões, bordas e sombras**.

O objetivo não é decorar um catálogo de classes. Cada utilitário deve responder a uma necessidade observável: criar hierarquia, melhorar a leitura, separar conteúdos, limitar uma medida ou distinguir uma superfície. Antes de adicionar uma classe, identifique qual propriedade CSS precisa mudar e qual resultado visual será usado para verificar a escolha.

Ao longo do encontro, uma página sem hierarquia visual será transformada de modo incremental. A cada etapa, compare antes e depois, inspecione o elemento no navegador e relacione a classe aplicada à regra CSS gerada.

## Objetivos de aprendizagem

- relacionar utilitários fundamentais às propriedades CSS correspondentes;
- usar cores com função comunicativa e contraste adequado;
- criar hierarquia tipográfica com tamanho, peso e altura de linha;
- diferenciar margem, preenchimento e espaçamento entre elementos;
- controlar largura e altura sem comprometer o conteúdo;
- combinar bordas, raios e sombras para distinguir superfícies;
- interpretar as escalas de valores do tema do Tailwind;
- identificar utilitários redundantes, conflitantes ou inadequados;
- validar as escolhas com DevTools, teclado e diferentes larguras.

## Conceitos essenciais

- tokens e escalas do tema;
- cor de texto, fundo e borda;
- tamanho, peso e altura de linha;
- margem, preenchimento e `gap`;
- largura, altura e limites de dimensão;
- espessura, cor e raio de borda;
- sombras como indicação de elevação;
- hierarquia visual, legibilidade e contraste.

## 1. Ponto de partida

Continue no projeto `encontro-02-tailwind` criado na aula anterior. Confirme as dependências e inicie o processo de desenvolvimento:

```bash
npm install
npm run dev
```

Mantenha a CLI ativa enquanto edita `src/index.html`. O arquivo `src/output.css` deve ser atualizado a cada alteração.

Use este conteúdo inicial no `body`:

```html
<body>
  <main>
    <article>
      <p>Formação frontend</p>
      <h1>Interfaces com Tailwind CSS</h1>
      <p>
        Aprenda a transformar decisões de design em utilitários pequenos,
        previsíveis e reutilizáveis.
      </p>
      <p>12 encontros · nível introdutório</p>
      <a href="#conteudo">Ver conteúdo</a>
    </article>
  </main>
</body>
```

O HTML possui estrutura e conteúdo, mas quase nenhuma hierarquia visual. Essa versão permite observar o efeito de cada grupo de utilitários sem confundir várias mudanças simultâneas.

## 2. Como ler uma classe fundamental

Em muitos utilitários, o nome combina uma **propriedade** e um **valor da escala**:

```text
text-slate-700
└─ propriedade ─┘ └ token do tema

p-6
└ propriedade: padding
  └ valor da escala de espaçamento
```

Nem todo número representa pixels. Em `p-6`, por exemplo, `6` identifica uma posição da escala padrão e corresponde a `1.5rem`. Usar a escala cria repetição intencional e evita escolher valores arbitrários para cada elemento.

Quando um valor previsto pelo tema não resolve uma necessidade legítima, o Tailwind aceita valores arbitrários, como `max-w-[42rem]`. Eles devem ser exceções justificadas, pois o uso excessivo enfraquece a consistência.

## 3. Cores com função

| Classe | Responsabilidade | CSS correspondente |
|---|---|---|
| `text-slate-900` | cor do texto | `color` |
| `bg-white` | cor de fundo | `background-color` |
| `border-slate-200` | cor da borda | `border-color` |
| `outline-sky-700` | cor do contorno | `outline-color` |

Uma família como `slate` possui gradações numeradas. Em geral, números menores são mais claros e números maiores são mais escuros. A escala não determina sozinha se uma combinação possui contraste suficiente; contexto, tamanho do texto e fundo também importam.

### Aplicação incremental

```html
<body class="bg-slate-100 text-slate-900">
  <main>
    <article class="border-slate-200 bg-white">
      <p class="text-sky-700">Formação frontend</p>
      <h1>Interfaces com Tailwind CSS</h1>
      <p class="text-slate-600">
        Aprenda a transformar decisões de design em utilitários pequenos,
        previsíveis e reutilizáveis.
      </p>
      <p class="text-slate-500">12 encontros · nível introdutório</p>
      <a class="bg-sky-700 text-white" href="#conteudo">Ver conteúdo</a>
    </article>
  </main>
</body>
```

As cores possuem papéis distintos:

- `slate-900` identifica o texto principal;
- `slate-600` reduz a ênfase sem prejudicar a leitura;
- `sky-700` destaca categoria e ação;
- `white` separa a superfície do fundo `slate-100`.

Evite usar apenas cor para comunicar informação importante. Um erro, por exemplo, precisa de texto ou ícone compreensível, não apenas de uma borda vermelha.

## 4. Tipografia e hierarquia

| Intenção | Utilitários de exemplo | Propriedade CSS |
|---|---|---|
| tamanho | `text-sm`, `text-xl`, `text-3xl` | `font-size` |
| peso | `font-medium`, `font-semibold`, `font-bold` | `font-weight` |
| altura de linha | `leading-6`, `leading-7` | `line-height` |
| alinhamento | `text-left`, `text-center` | `text-align` |
| transformação | `uppercase` | `text-transform` |

Aplique hierarquia sem alterar a ordem semântica:

```html
<p class="text-sm font-semibold text-sky-700">Formação frontend</p>
<h1 class="text-3xl font-bold text-slate-900">
  Interfaces com Tailwind CSS
</h1>
<p class="text-base leading-7 text-slate-600">
  Aprenda a transformar decisões de design em utilitários pequenos,
  previsíveis e reutilizáveis.
</p>
<p class="text-sm font-medium text-slate-500">
  12 encontros · nível introdutório
</p>
```

O `h1` continua sendo `h1` por representar o título principal, e não porque recebeu `text-3xl`. A semântica define a estrutura; os utilitários definem a apresentação.

Use altura de linha para favorecer textos com mais de uma linha. Evite reduzir demais informações secundárias: menor ênfase não deve significar texto ilegível.

## 5. Espaçamento: margem, preenchimento e gap

| Necessidade | Utilitário | Resultado |
|---|---|---|
| espaço dentro do cartão | `p-6` | preenchimento em todos os lados |
| espaço acima de um elemento | `mt-4` | margem superior |
| espaço horizontal da ação | `px-4` | preenchimento lateral |
| espaço vertical da ação | `py-3` | preenchimento vertical |
| espaço entre filhos de um layout | `gap-4` | intervalo controlado pelo contêiner |

```html
<main class="p-6">
  <article class="p-6">
    <p class="text-sm font-semibold text-sky-700">Formação frontend</p>
    <h1 class="mt-2 text-3xl font-bold">Interfaces com Tailwind CSS</h1>
    <p class="mt-4 leading-7 text-slate-600">...</p>
    <p class="mt-4 text-sm font-medium text-slate-500">...</p>
    <a class="mt-6 inline-flex bg-sky-700 px-4 py-3 text-white" href="#conteudo">
      Ver conteúdo
    </a>
  </article>
</main>
```

Margem separa o elemento de seus vizinhos. Preenchimento cria espaço entre o conteúdo e a borda do próprio elemento. `gap` pertence ao contêiner Flexbox ou Grid e controla o intervalo entre seus filhos.

Não use espaços no texto, elementos `<br>` ou `&nbsp;` para resolver layout. Essas opções misturam conteúdo com apresentação e produzem resultados frágeis.

## 6. Dimensões e limites

| Classe | Efeito |
|---|---|
| `w-full` | ocupa toda a largura disponível |
| `max-w-xl` | limita a largura máxima |
| `min-h-screen` | ocupa pelo menos a altura da viewport |
| `size-10` | define largura e altura com o mesmo valor |
| `max-w-prose` | limita texto a uma medida favorável à leitura |

Centralize o cartão e limite sua linha de leitura:

```html
<body class="min-h-screen bg-slate-100 text-slate-900">
  <main class="mx-auto max-w-2xl p-6">
    <article class="w-full border-slate-200 bg-white p-6">
      <!-- conteúdo -->
    </article>
  </main>
</body>
```

`w-full` acompanha o espaço do contêiner, enquanto `max-w-2xl` impede que o conteúdo fique largo demais. `mx-auto` distribui as margens horizontais quando existe espaço disponível.

Evite alturas fixas em blocos de texto. Se o conteúdo crescer, uma classe como `h-40` pode causar corte ou sobreposição. Prefira altura automática ou `min-h-*` quando for necessária apenas uma altura mínima.

## 7. Bordas, raios e sombras

```html
<article class="rounded-lg border border-slate-200 bg-white p-6 shadow-sm">
  <!-- conteúdo -->
</article>
```

- `border` adiciona a espessura padrão da borda;
- `border-slate-200` define sua cor;
- `rounded-lg` arredonda os cantos;
- `shadow-sm` cria uma elevação discreta.

Uma sombra intensa em cada bloco reduz a hierarquia, porque tudo parece igualmente elevado. Para superfícies estáticas, uma borda clara ou sombra discreta costuma ser suficiente. Use raios consistentes em elementos com função semelhante.

## 8. Exemplo principal completo

Depois de compreender cada grupo, reúna as decisões em `src/index.html`:

```html
<!doctype html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Perfil do curso</title>
    <link rel="stylesheet" href="./output.css" />
  </head>
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
</html>
```

### O que observar

- cada cor possui uma função, em vez de servir apenas como decoração;
- tamanho, peso e altura de linha produzem hierarquia tipográfica;
- margens controlam o ritmo entre conteúdos relacionados;
- o preenchimento preserva espaço dentro do cartão e da ação;
- `max-w-2xl` limita a medida geral sem fixar a largura;
- borda, fundo e sombra distinguem o cartão do restante da página;
- o link mantém foco visível e área de interação confortável;
- nenhuma classe modifica o significado semântico do HTML.

## 9. Inspeção e diagnóstico

### Verificar no DevTools

1. selecione o `article` no inspetor;
2. localize as regras de `padding`, `border-radius` e `box-shadow`;
3. desative uma declaração por vez e observe seu efeito;
4. selecione o parágrafo e compare `font-size` com `line-height`;
5. altere a largura da viewport e confirme que não há rolagem horizontal.

### Comparar decisões

- substitua `max-w-2xl` por `max-w-sm` e observe a quebra das linhas;
- troque `p-6` por `p-2` e compare a proximidade entre conteúdo e borda;
- remova `leading-7` e examine o ritmo do parágrafo;
- troque `shadow-sm` por `shadow-xl` e avalie a elevação percebida;
- remova `inline-flex` do link e observe como margem e caixa se comportam.

O objetivo não é encontrar uma classe universalmente correta, mas justificar qual opção atende ao conteúdo e ao contexto.

## 10. Prática guiada

Transforme o cartão de curso em um cartão de evento:

1. altere o conteúdo e preserve a estrutura semântica;
2. defina fundo, superfície, texto principal, texto secundário e destaque;
3. estabeleça hierarquia entre categoria, título, descrição e informações auxiliares;
4. aplique espaçamentos usando margem, preenchimento e, se necessário, `gap`;
5. limite a largura sem definir altura fixa para os textos;
6. escolha borda, raio e sombra coerentes com a superfície;
7. verifique o resultado com teclado e em uma viewport estreita;
8. remova classes redundantes ou que você não consegue explicar.

Altere apenas um grupo de propriedades por vez. Essa sequência torna a relação entre decisão e resultado mais fácil de observar.

## 11. Exercício aplicado

Crie uma página de perfil para um curso, evento, serviço ou projeto diferente do exemplo. A página deve conter:

- uma região principal semanticamente identificável;
- título, descrição e pelo menos duas informações secundárias;
- uma ação representada pelo elemento HTML adequado;
- cores de fundo, texto, borda e destaque com funções explicáveis;
- pelo menos três níveis de hierarquia tipográfica;
- margem, preenchimento e `gap` usados em situações apropriadas;
- largura fluida com um limite máximo;
- borda e raio; sombra apenas se houver justificativa visual;
- foco visível na ação interativa;
- conteúdo legível sem rolagem horizontal em tela estreita.

Não copie integralmente o exemplo principal. Modifique o domínio, a composição, o conteúdo e as decisões visuais.

### Desafio adicional

Adicione uma lista de três características usando Flexbox ou Grid apenas para organizar os itens. Use `gap` no contêiner, mantenha a hierarquia tipográfica e evite repetir margens individuais em cada item.

## 12. Critérios de aceite

- `npm run dev` e `npm run build` executam sem erro;
- o HTML referencia o CSS compilado correto;
- a estrutura HTML comunica o significado do conteúdo;
- todos os utilitários podem ser relacionados a uma propriedade ou intenção;
- cores possuem contraste e não são o único meio de transmitir informação;
- tipografia apresenta hierarquia e leitura confortável;
- margem, preenchimento e `gap` são usados de acordo com suas responsabilidades;
- dimensões não cortam nem sobrepõem conteúdo;
- bordas, raios e sombras são usados com consistência;
- a ação possui foco visível e funciona pelo teclado;
- a página permanece legível em viewport estreita e ampla.

## 13. Erros comuns

- escolher classes por tentativa sem relacioná-las a propriedades CSS;
- interpretar números da escala como valores diretos em pixels;
- usar cores claras demais para textos sobre fundo branco;
- comunicar estados somente por cor;
- usar tamanho de fonte para substituir a hierarquia semântica;
- criar espaçamento com `<br>`, espaços ou `&nbsp;`;
- confundir margem, preenchimento e `gap`;
- aplicar largura e altura fixas a conteúdo variável;
- combinar utilitários conflitantes para a mesma propriedade;
- adicionar borda, raio e sombra intensa a todos os elementos;
- usar valores arbitrários antes de consultar a escala do tema;
- copiar sequências de classes sem conseguir explicar cada grupo.

## Materiais para aprofundamento

- [Tailwind CSS — cores](https://tailwindcss.com/docs/colors)
- [Tailwind CSS — tamanho de fonte](https://tailwindcss.com/docs/font-size)
- [Tailwind CSS — preenchimento](https://tailwindcss.com/docs/padding)
- [Tailwind CSS — margem](https://tailwindcss.com/docs/margin)
- [Tailwind CSS — largura](https://tailwindcss.com/docs/width)
- [Tailwind CSS — bordas](https://tailwindcss.com/docs/border-width)
- [Tailwind CSS — sombras](https://tailwindcss.com/docs/box-shadow)
- [WCAG — contraste mínimo](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html)

## Checklist de compreensão

- [ ] Relaciono utilitários fundamentais às propriedades CSS correspondentes.
- [ ] Entendo números de classes como posições de uma escala, não pixels diretos.
- [ ] Escolho cores por função e verifico contraste.
- [ ] Crio hierarquia tipográfica sem abandonar a semântica.
- [ ] Diferencio margem, preenchimento e `gap`.
- [ ] Uso limites de largura para preservar a leitura.
- [ ] Evito alturas fixas em conteúdos que podem crescer.
- [ ] Uso bordas, raios e sombras com intenção e consistência.
- [ ] Inspeciono no navegador o CSS produzido pelos utilitários.
- [ ] Verifico teclado, foco e diferentes larguras antes da entrega.

## Resumo final

Neste encontro, os utilitários fundamentais foram tratados como traduções de decisões de CSS e design. Cores comunicam papéis, tipografia cria hierarquia, espaçamentos organizam relações, dimensões controlam a medida do conteúdo e bordas ou sombras distinguem superfícies.

O uso consciente dessas classes depende de observar o problema antes de escolher a solução. A escala do tema oferece consistência, mas a qualidade final ainda exige semântica, contraste, legibilidade e validação no navegador.

No próximo encontro, esses fundamentos serão combinados com Flexbox e Grid para construir layouts com mais de uma região ou componente.

## Questões de fixação

1. Por que o número `6` em `p-6` não deve ser interpretado como seis pixels?
<!-- Gabarito: porque identifica uma posição na escala de espaçamento; na escala padrão, p-6 corresponde a 1.5rem. -->

2. Qual é a diferença entre `margin`, `padding` e `gap`?
<!-- Gabarito: margin separa a caixa dos vizinhos; padding separa conteúdo e borda; gap espaça os filhos de um contêiner Flexbox ou Grid. -->

3. Por que `max-w-2xl` pode ser mais adequado que uma largura fixa?
<!-- Gabarito: limita a medida em telas amplas, mas permite que o cartão encolha conforme o espaço disponível. -->

4. Tamanho visual e nível de título HTML representam a mesma responsabilidade?
<!-- Gabarito: não; h1-h6 expressa hierarquia semântica, enquanto text-3xl controla apenas apresentação. -->

5. Em que situação uma altura fixa pode causar problemas?
<!-- Gabarito: quando conteúdo variável cresce com zoom, tradução ou quebra de linha e pode transbordar ou ser cortado. -->

6. Como comprovar que uma classe Tailwind produziu a propriedade esperada?
<!-- Gabarito: inspecionar o elemento no DevTools, localizar a regra gerada, ativá-la ou desativá-la e observar a mudança. -->

[Voltar ao cronograma](../01-cronograma-60h.md)
