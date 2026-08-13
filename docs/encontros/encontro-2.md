# Encontro 2 — Fundamentos de CSS

**Entrega prevista:** exercícios de revisão de CSS

## Visão geral

CSS transforma a árvore do documento em uma apresentação visual. Para escrever estilos previsíveis, é necessário compreender como o navegador seleciona declarações, resolve conflitos, calcula dimensões e interpreta unidades.

Este capítulo revisa cascata, herança, especificidade, box model e unidades. Esses conceitos explicam tanto o CSS autoral quanto o funcionamento das classes utilitárias do Tailwind CSS. Quando uma classe Tailwind produz uma regra CSS, ela continua submetida à cascata e ao modelo de caixa.

## Objetivos de aprendizagem

- explicar como o navegador decide qual declaração será aplicada;
- diferenciar cascata, herança e especificidade;
- analisar seletores sem aumentar especificidade desnecessariamente;
- calcular dimensões pelo box model;
- selecionar unidades conforme a propriedade e o comportamento desejado;
- investigar estilos com o DevTools.

## 1. Anatomia de uma regra CSS

```css
.cartao {
  max-width: 30rem;
  padding: 1.5rem;
  border: 1px solid #cbd5e1;
}
```

- `.cartao` é o seletor;
- `max-width`, `padding` e `border` são propriedades;
- `30rem` e os demais conteúdos após `:` são valores;
- cada par propriedade/valor é uma declaração.

O navegador associa elementos a seletores e calcula um valor final para cada propriedade.

## 2. Formas de inclusão

```html
<!-- Folha externa: forma preferencial para projetos -->
<link rel="stylesheet" href="styles.css" />
```

Um bloco `<style>` pode ser útil em exemplos isolados. Estilo inline possui alta prioridade na origem do autor e dificulta reutilização, por isso não deve ser a estratégia habitual.

## 3. Cascata

A cascata resolve declarações concorrentes. A decisão não depende apenas de “qual veio por último”. Em uma visão simplificada, o navegador considera:

1. relevância da regra e condição de aplicação;
2. origem e importância;
3. camadas de cascata;
4. especificidade;
5. proximidade na ordem do código.

```css
p { color: #334155; }
.destaque { color: #1d4ed8; }
```

Em `<p class="destaque">`, a segunda regra vence por ser mais específica, não somente por aparecer depois.

### Ordem da fonte

Quando duas declarações possuem mesma origem, importância, camada e especificidade, vence a que aparece depois.

```css
.aviso { color: darkred; }
.aviso { color: darkblue; } /* vence neste caso */
```

### `!important`

`!important` altera a prioridade da declaração e deve ser excepcional. Seu uso frequente costuma indicar dificuldade de arquitetura ou especificidade.

```css
/* Evite como correção automática */
.botao { color: white !important; }
```

## 4. Herança

Algumas propriedades recebem por padrão o valor calculado do elemento ancestral. Propriedades relacionadas a texto, como `color` e `font-family`, geralmente herdam; dimensões, margens, bordas e fundos geralmente não.

```css
body {
  color: #1e293b;
  font-family: system-ui, sans-serif;
}
```

Os descendentes usam esses valores, salvo quando outra regra define algo diferente. Isso reduz repetição.

Valores globais úteis:

- `inherit`: usa o valor do elemento pai;
- `initial`: usa o valor inicial definido pela especificação;
- `unset`: comporta-se como `inherit` em propriedade herdável e `initial` nas demais;
- `revert`: retorna ao valor de origem ou camada anterior.

```css
button,
input,
textarea,
select {
  font: inherit;
}
```

Controles de formulário nem sempre herdam tipografia como se espera; a regra os harmoniza com o documento.

## 5. Especificidade

Especificidade é o peso do seletor. Uma representação didática usa três grupos:

```text
IDs | classes, atributos e pseudoclasses | elementos e pseudoelementos
```

| Seletor | Peso aproximado |
|---|---:|
| `article` | 0-0-1 |
| `.cartao` | 0-1-0 |
| `.cartao h2` | 0-1-1 |
| `[aria-current="page"]` | 0-1-0 |
| `#conteudo` | 1-0-0 |

```css
#painel .cartao h2 { color: purple; }
.cartao__titulo { color: navy; }
```

A primeira regra vence pelo ID, ainda que a segunda esteja depois. Seletores excessivamente fortes dificultam sobrescritas e reutilização. Prefira classes com intenção clara e baixa especificidade.

### Pseudoclasses funcionais

`:where()` sempre possui especificidade zero, o que ajuda a criar bases fáceis de sobrescrever.

```css
:where(ul, ol) {
  padding-inline-start: 1.5rem;
}
```

`:is()` e `:not()` assumem a especificidade do argumento mais forte. É importante conhecer essa diferença ao compor seletores.

## 6. Box model

Todo elemento gera uma caixa composta por:

```text
margem
└── borda
    └── padding
        └── conteúdo
```

No modelo padrão `content-box`, `width` define apenas o conteúdo. Uma caixa com `width: 300px`, `padding: 20px` dos dois lados e borda de `2px` ocupa 344px, sem contar margem.

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

Com `border-box`, a largura declarada inclui conteúdo, padding e borda, tornando o dimensionamento mais previsível.

### Margem e padding

- `padding` cria espaço interno e participa do fundo e da área clicável;
- `margin` cria separação externa;
- margens verticais de blocos podem colapsar;
- `gap` costuma ser melhor que margens individuais para espaçar itens de Flexbox e Grid.

### Dimensões mínimas e máximas

Prefira limites que permitam adaptação:

```css
.conteudo {
  width: min(100% - 2rem, 70rem);
  margin-inline: auto;
}

img {
  max-width: 100%;
  height: auto;
}
```

## 7. Fluxo normal e `display`

Antes de Flexbox e Grid, os elementos participam do fluxo normal:

- caixas `block` ocupam a largura disponível e iniciam nova linha;
- caixas `inline` fluem com o texto e não aceitam todas as dimensões da mesma forma;
- `inline-block` flui inline, mas aceita características de bloco;
- `display: none` remove o elemento do layout e da árvore de acessibilidade.

Compreender o fluxo normal evita aplicar posicionamento absoluto para tarefas que o layout resolveria naturalmente.

## 8. Unidades CSS

### Unidades absolutas e relativas

| Unidade | Referência | Uso comum |
|---|---|---|
| `px` | pixel CSS | bordas e detalhes precisos |
| `rem` | tamanho da fonte raiz | tipografia e espaçamento consistente |
| `em` | tamanho da fonte do contexto | componentes que escalam localmente |
| `%` | medida do bloco de referência | larguras fluidas |
| `vw`, `vh` | viewport | efeitos que dependem da tela, com cautela |
| `ch` | largura aproximada do caractere `0` | limitar linha de texto |
| `fr` | fração do espaço no Grid | distribuição de colunas |

Não existe uma unidade universalmente melhor. A escolha depende da relação desejada.

```css
:root { font-size: 100%; }

body {
  font-size: 1rem;
  line-height: 1.5;
}

h1 {
  font-size: clamp(2rem, 1.5rem + 2vw, 3.5rem);
}

.texto {
  max-width: 65ch;
}
```

Evite alterar a raiz para `62.5%`: isso reduz o tamanho definido pelo usuário e pode prejudicar acessibilidade. `clamp()` estabelece mínimo, valor fluido e máximo.

## 9. Propriedades lógicas

Propriedades lógicas acompanham o modo de escrita:

```css
.cartao {
  padding-inline: 1.5rem;
  padding-block: 1rem;
  margin-block-end: 1rem;
  border-inline-start: 0.25rem solid #2563eb;
}
```

`inline` representa o eixo do texto; `block`, o eixo de empilhamento. Elas facilitam internacionalização e tornam a intenção mais clara que `left`/`right` em muitos casos.

## 10. Exemplo integrado

```html
<article class="curso curso--destaque">
  <p class="curso__categoria">Frontend</p>
  <h2 class="curso__titulo">Angular e Tailwind CSS</h2>
  <p class="curso__descricao">Componentes, rotas, dados e interfaces responsivas.</p>
  <a class="curso__acao" href="/curso">Ver detalhes</a>
</article>
```

```css
*, *::before, *::after { box-sizing: border-box; }

:root {
  color: #1e293b;
  font-family: system-ui, sans-serif;
}

.curso {
  width: min(100%, 32rem);
  padding: 1.5rem;
  border: 1px solid #cbd5e1;
  border-radius: 0.75rem;
}

.curso__categoria {
  color: #1d4ed8;
  font-weight: 700;
}

.curso__titulo {
  margin-block: 0.5rem;
  font-size: clamp(1.5rem, 1.25rem + 1vw, 2rem);
}

.curso__descricao { max-width: 55ch; }

.curso__acao {
  display: inline-block;
  margin-block-start: 1rem;
  padding: 0.75em 1em;
}
```

Observe a baixa especificidade, as dimensões flexíveis e a relação entre `em` no botão e seu tamanho de fonte.

## 11. Investigação com DevTools

Para descobrir por que um estilo não aparece:

1. confirme se o seletor corresponde ao elemento;
2. veja se a declaração está riscada por outra regra;
3. compare origem, importância e especificidade;
4. observe o valor calculado no painel **Computed**;
5. examine o diagrama do box model;
6. verifique se uma media query está ativa;
7. experimente valores no inspetor e valide a hipótese antes de editar o arquivo.

Não acrescente `!important` antes de identificar a causa.

## 12. Exercícios de revisão

### Exercício A — Cascata

Dado um elemento com três regras concorrentes, identifique a declaração vencedora e justifique por origem, especificidade e ordem.

### Exercício B — Box model

Calcule a largura externa de uma caixa em `content-box` e compare com `border-box`.

### Exercício C — Unidades

Refatore um layout rígido em pixels usando `rem`, `%`, `ch`, `min()` ou `clamp()` onde houver uma relação justificável.

### Critérios de conclusão

- justificativas usam vocabulário técnico correto;
- a solução não depende de `!important`;
- a página mantém legibilidade com zoom;
- imagens e contêineres não geram overflow;
- o DevTools foi usado para registrar ao menos uma evidência.

## 13. Erros frequentes

- afirmar que a última regra sempre vence;
- confundir herança com aplicação de seletor descendente;
- aumentar especificidade até “forçar” o estilo;
- esquecer padding e borda ao calcular largura;
- usar `100vw` em contêiner interno e provocar rolagem horizontal;
- fixar altura para blocos com texto variável;
- usar `em` sem considerar o contexto de fonte;
- remover preferências do usuário ao reduzir a fonte raiz.

## Checklist de compreensão

- [ ] Explico a ordem geral da cascata.
- [ ] Distingo herança de especificidade.
- [ ] Calculo o box model em `content-box` e `border-box`.
- [ ] Escolho unidades conforme a relação desejada.
- [ ] Uso dimensões mínimas e máximas para acomodar conteúdo.
- [ ] Investigo regras vencidas no DevTools.

## Questões de fixação

1. Em que situação a ordem da fonte decide o conflito?
<!-- Gabarito: quando as declarações têm mesma relevância, origem/importância, camada e especificidade. -->

2. Qual é a diferença entre herança e cascata?
<!-- Gabarito: cascata escolhe entre declarações concorrentes; herança fornece ao descendente o valor calculado do ancestral quando aplicável. -->

3. O que `box-sizing: border-box` altera?
<!-- Gabarito: faz width e height incluírem padding e borda, em vez de medirem somente o conteúdo. -->

4. Quando `rem` e `em` produzem comportamentos diferentes?
<!-- Gabarito: rem referencia a fonte raiz; em depende da fonte do próprio elemento ou do contexto conforme a propriedade. -->

5. Por que `!important` não deve ser a primeira resposta a um conflito?
<!-- Gabarito: mascara a causa, aumenta dificuldade de sobrescrita e cria uma disputa de prioridade difícil de manter. -->

## Referências

- [MDN — Cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Cascade)
- [MDN — Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Specificity)
- [MDN — Box model](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Box_model)
- [MDN — CSS values and units](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Values_and_units)

[Voltar ao cronograma](../01-cronograma-60h.md)
