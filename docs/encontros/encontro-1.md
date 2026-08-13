# Encontro 1 — Revisão integrada de HTML, acessibilidade e CSS

## Visão geral

Desenvolvimento Web Frontend dá continuidade a Padrões Web. HTML, CSS, acessibilidade e responsividade continuam sendo a base sobre a qual Tailwind CSS, TypeScript e Angular serão estudados. Frameworks organizam e aceleram o trabalho, mas não corrigem automaticamente uma estrutura semântica inadequada, um layout frágil ou uma interação inacessível.

Este material apresenta o modelo mental que acompanhará toda a disciplina e concentra a revisão de CSS no primeiro encontro. O diagnóstico não procura apenas saber se uma página “abre”, mas se o estudante consegue ler o código, explicar decisões, localizar defeitos e propor melhorias verificáveis.

## Objetivos de aprendizagem

- relacionar HTML, CSS e JavaScript às responsabilidades de uma interface;
- distinguir página estática, interface interativa e aplicação frontend;
- reconhecer semântica, responsividade, acessibilidade e manutenção como requisitos;
- explicar cascata, herança, especificidade e box model;
- selecionar unidades CSS e estruturar layouts com Flexbox e Grid;
- aplicar responsividade mobile-first, conteúdo fluido e media queries;
- utilizar navegador e DevTools como instrumentos de investigação;
- registrar conhecimentos consolidados e pontos que precisam ser retomados.

## 1. O que muda nesta disciplina

Em Padrões Web, o foco esteve na construção da base: documentos HTML, estilos CSS, layouts, responsividade e publicação. Agora essa base será aplicada a interfaces maiores, compostas por unidades reutilizáveis e conectadas a dados.

```text
HTML semântico      → estrutura e significado
CSS / Tailwind CSS  → apresentação e adaptação visual
JavaScript          → eventos e comportamento
TypeScript          → contratos e verificação de tipos
Angular             → componentes, rotas, serviços e aplicação
```

As camadas cooperam, mas não são intercambiáveis. Um botão precisa continuar sendo um elemento `button`; uma classe visual não substitui sua semântica. Da mesma forma, Angular não elimina o HTML, e Tailwind não elimina o CSS: ambos exigem domínio dos fundamentos que abstraem.

## 2. Página, interface e aplicação frontend

Uma **página** apresenta um documento acessado por uma URL. Uma **interface** acrescenta controles e respostas às ações do usuário. Uma **aplicação frontend** organiza diversas interfaces, estados, rotas e fontes de dados em uma arquitetura mantida no navegador.

Uma aplicação pode incluir:

- componentes como cabeçalho, cartão e formulário;
- navegação sem recarregamento completo;
- validação e mensagens de feedback;
- comunicação com APIs;
- estados de carregamento, sucesso, vazio e erro;
- regras de apresentação e comportamento;
- testes e build de produção.

O aumento de complexidade torna importantes a separação de responsabilidades, os contratos de dados e a organização do código.

## 3. Responsabilidades das tecnologias fundamentais

### HTML: estrutura e significado

HTML descreve o conteúdo. Elementos semânticos permitem que navegador, mecanismo de busca, leitor de tela e pessoa desenvolvedora compreendam a organização do documento.

```html
<article>
  <header>
    <h2>Oficina de Angular</h2>
    <p><time datetime="2026-09-15">15 de setembro</time></p>
  </header>
  <p>Introdução à criação de componentes.</p>
  <a href="/oficinas/angular">Consultar detalhes da oficina</a>
</article>
```

`article`, `header`, `time` e o texto descritivo do link comunicam significado. Trocar tudo por `div` pode manter a aparência, mas empobrece a estrutura.

### CSS: apresentação e adaptação

CSS controla tipografia, cor, espaçamento e layout. Uma folha bem organizada trabalha com o conteúdo, em vez de depender de tamanhos fixos.

```css
.oficinas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 18rem), 1fr));
  gap: 1rem;
}
```

O exemplo distribui cartões conforme o espaço disponível e impede que a largura mínima provoque overflow em telas estreitas.

### JavaScript: comportamento

JavaScript responde a eventos e modifica estado ou conteúdo. O HTML deve oferecer uma base utilizável mesmo antes da camada de comportamento, sempre que isso for possível.

```js
const botao = document.querySelector("#alternar-detalhes");
const detalhes = document.querySelector("#detalhes");

botao.addEventListener("click", () => {
  const aberto = !detalhes.hidden;
  detalhes.hidden = aberto;
  botao.setAttribute("aria-expanded", String(!aberto));
});
```

O estado visual e `aria-expanded` precisam permanecer sincronizados.

## 4. Qualidade frontend como conjunto de critérios

“Funciona no meu computador” é uma verificação insuficiente. Uma solução deve ser analisada por diferentes dimensões:

| Dimensão | Pergunta de verificação |
|---|---|
| Semântica | Os elementos representam corretamente o conteúdo? |
| Funcionalidade | A tarefa principal pode ser concluída? |
| Responsividade | O conteúdo se adapta sem cortes ou overflow? |
| Acessibilidade | Teclado, foco, nomes e contraste estão adequados? |
| Manutenção | Nomes e responsabilidades são compreensíveis? |
| Desempenho | Recursos e scripts são necessários e proporcionais? |
| Confiabilidade | Erros e estados alternativos foram previstos? |
| Versionamento | O histórico explica a evolução da solução? |

Qualidade não é uma etapa final. Uma marcação semântica correta simplifica acessibilidade, e componentes bem delimitados favorecem testes e manutenção.

Essas dimensões começam na estrutura do documento. Por isso, a revisão prossegue com HTML semântico e acessibilidade antes de chegar ao diagnóstico integrado.

## 5. HTML semântico e acessibilidade

HTML semântico descreve o significado e as relações do conteúdo. Acessibilidade busca garantir que pessoas com diferentes capacidades, tecnologias assistivas e formas de interação consigam perceber, compreender e operar a interface. Esses dois assuntos fazem parte do diagnóstico porque Angular continuará produzindo HTML no navegador: componentes acessíveis começam com marcação nativa adequada.

### Escolher elementos pelo significado

Elementos que parecem iguais depois do CSS podem oferecer experiências diferentes. Um elemento nativo já possui papel, foco e comportamento reconhecidos pelo navegador.

```html
<!-- Não possui semântica nem teclado automaticamente -->
<div class="botao" onclick="salvar()">Salvar</div>

<!-- Possui papel, foco e ativação por teclado -->
<button type="button">Salvar</button>
```

Escolhas frequentes:

- navegação para outro recurso: `a` com `href`;
- ação local: `button`;
- conjunto independente: `article`;
- região temática identificada: `section`;
- informação complementar: `aside`;
- conjunto de links importantes: `nav`;
- dados tabulares: `table`.

### Estrutura e landmarks

Landmarks permitem localizar grandes regiões da página. Um documento costuma combinar `header`, `nav`, `main`, `aside` e `footer`.

```html
<header>
  <a href="/">Portal Acadêmico</a>
  <nav aria-label="Navegação principal">
    <a href="/cursos" aria-current="page">Cursos</a>
    <a href="/agenda">Agenda</a>
  </nav>
</header>

<main id="conteudo-principal">
  <h1>Cursos disponíveis</h1>
</main>

<footer>
  <nav aria-label="Navegação do rodapé">...</nav>
</footer>
```

Deve existir um único `main` visível. Landmarks repetidos, como duas navegações, precisam de nomes acessíveis que os diferenciem.

### Hierarquia de títulos

Títulos formam o sumário lógico do documento e não devem ser escolhidos por tamanho visual.

```html
<h1>Catálogo de cursos</h1>
<section aria-labelledby="tecnologia">
  <h2 id="tecnologia">Tecnologia</h2>
  <article>
    <h3>Desenvolvimento Web Frontend</h3>
    <p>Curso voltado à construção de interfaces.</p>
  </article>
</section>
```

O `h1` identifica o assunto principal; `h2` divide grandes seções; `h3` representa uma subseção. A aparência deve ser controlada pelo CSS sem alterar essa relação.

### Nomes acessíveis

Tecnologias assistivas interpretam controles por nome, papel, valor e estado. O nome pode vir do texto visível, de `label`, de `alt` ou, quando necessário, de ARIA.

```html
<button type="button" aria-label="Fechar janela">
  <svg aria-hidden="true" viewBox="0 0 24 24">...</svg>
</button>
```

Se o botão já contém texto visível suficiente, `aria-label` é desnecessário. HTML nativo deve ser preferido a recriações com ARIA.

### Imagens

O texto alternativo depende da função da imagem no contexto:

- imagem informativa: descreve a informação relevante;
- imagem funcional: descreve destino ou ação;
- imagem decorativa: utiliza `alt=""`;
- não se repete no `alt` uma legenda já disponível.

```html
<img
  src="laboratorio.jpg"
  alt="Estudantes trabalhando em computadores no laboratório"
  width="960"
  height="640"
/>
```

### Formulários

Placeholders não substituem rótulos. `label` permanece visível, aumenta a área de interação e identifica programaticamente o campo.

```html
<form aria-labelledby="titulo-inscricao">
  <h2 id="titulo-inscricao">Inscrição na oficina</h2>

  <label for="email">E-mail institucional</label>
  <input
    id="email"
    name="email"
    type="email"
    autocomplete="email"
    aria-describedby="ajuda-email"
    required
  />
  <p id="ajuda-email">Exemplo: estudante@escolar.ifrn.edu.br</p>

  <fieldset>
    <legend>Turno preferencial</legend>
    <label><input type="radio" name="turno" value="manha" /> Manhã</label>
    <label><input type="radio" name="turno" value="tarde" /> Tarde</label>
  </fieldset>

  <button type="submit">Enviar inscrição</button>
</form>
```

Mensagens de erro devem indicar o campo, o problema e como corrigi-lo. Cor isolada não é suficiente.

### Navegação por teclado e foco

Uma interface operável por teclado permite percorrer controles em ordem lógica, mantém foco visível e usa os comportamentos nativos de links, botões e campos.

```css
:focus-visible {
  outline: 3px solid #1d4ed8;
  outline-offset: 3px;
}
```

Evite `tabindex` positivo, que cria uma ordem artificial difícil de manter. Um teste básico percorre a página com `Tab` e `Shift + Tab`, ativa controles e confirma que não há armadilhas de foco.

## 6. Revisão completa de CSS

CSS transforma a árvore do documento em uma apresentação visual. Mesmo quando Tailwind CSS for adotado, as regras geradas continuarão submetidas à cascata, ao modelo de caixa e aos algoritmos de layout da plataforma Web.

### Anatomia e inclusão

```css
.cartao {
  max-width: 30rem;
  padding: 1.5rem;
  border: 1px solid #cbd5e1;
}
```

`.cartao` é o seletor; `max-width`, `padding` e `border` são propriedades; os termos após `:` são valores. Em projetos, prefira uma folha externa vinculada com `link`; estilos inline dificultam reutilização e manutenção.

### Cascata, herança e especificidade

A cascata resolve declarações concorrentes considerando, de forma simplificada:

1. relevância e condição de aplicação;
2. origem e importância;
3. camadas de cascata;
4. especificidade;
5. ordem no código.

```css
p { color: #334155; }
.destaque { color: #1d4ed8; }
```

Em `<p class="destaque">`, a classe vence por ser mais específica. Quando os demais critérios empatam, vence a declaração posterior. `!important` deve ser excepcional, pois costuma dificultar a evolução dos estilos.

Propriedades de texto, como `color` e `font-family`, geralmente são herdadas; dimensões, margens, bordas e fundos geralmente não. Os valores globais `inherit`, `initial`, `unset` e `revert` permitem controlar esse comportamento.

Uma representação didática da especificidade usa três grupos:

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

Prefira classes de baixa especificidade. `:where()` tem especificidade zero, enquanto `:is()` e `:not()` assumem a especificidade do argumento mais forte.

### Box model e fluxo normal

Cada elemento gera uma caixa formada por conteúdo, padding, borda e margem. No modelo `content-box`, `width` mede apenas o conteúdo. Uma largura de `300px`, padding lateral de `20px` e bordas de `2px` resultam em 344px.

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

Com `border-box`, a largura declarada inclui conteúdo, padding e borda. `padding` cria espaço interno; `margin`, separação externa; `gap` costuma ser a melhor opção entre itens de Flexbox e Grid.

No fluxo normal, caixas `block` iniciam uma linha e ocupam o espaço disponível; caixas `inline` acompanham o texto; `inline-block` flui em linha e aceita dimensões de bloco. `display: none` remove o elemento do layout e da árvore de acessibilidade.

### Unidades, limites e propriedades lógicas

| Unidade | Referência | Uso comum |
|---|---|---|
| `px` | pixel CSS | bordas e detalhes precisos |
| `rem` | fonte raiz | tipografia e espaçamento |
| `em` | fonte do contexto | componentes que escalam localmente |
| `%` | bloco de referência | dimensões fluidas |
| `vw`, `vh` | viewport | efeitos dependentes da tela, com cautela |
| `ch` | largura aproximada do caractere `0` | comprimento de linha |
| `fr` | fração do Grid | distribuição de colunas |

```css
.conteudo {
  width: min(100% - 2rem, 70rem);
  margin-inline: auto;
}

h1 {
  font-size: clamp(2rem, 1.5rem + 2vw, 3.5rem);
}
```

Não existe uma unidade ideal para todas as situações. Use limites flexíveis e preserve as preferências de tamanho de texto. Propriedades lógicas como `padding-inline`, `margin-block` e `border-inline-start` acompanham o modo de escrita e favorecem internacionalização.

### Flexbox

Flexbox distribui e alinha itens prioritariamente em um eixo. É adequado para navegação, grupos de botões, etiquetas e alinhamento interno de componentes.

```css
.acoes {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  align-items: center;
}

.acoes > * {
  flex: 1 1 12rem;
}
```

`flex-direction` define o eixo principal; `justify-content` distribui nesse eixo; `align-items` atua no eixo transversal; `flex-wrap` permite quebra; e `flex-grow`, `flex-shrink` e `flex-basis` controlam crescimento, redução e base.

### Grid

Grid organiza linhas e colunas e é adequado para grades de cartões, painéis e layouts de conteúdo com barra lateral.

```css
.grade {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 17rem), 1fr));
  gap: 1rem;
}
```

A expressão cria o número de colunas que couber e impede que a largura mínima ultrapasse telas estreitas. Flexbox e Grid podem ser combinados: Grid distribui componentes, e Flexbox organiza o interior de cada um.

### Responsividade e mobile-first

Design responsivo preserva leitura, operação e hierarquia em diferentes larguras, tamanhos de fonte, orientações, formas de entrada e preferências do usuário. O documento deve declarar:

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

Mobile-first parte de uma base simples e acrescenta complexidade quando o conteúdo dispõe de espaço. Breakpoints devem nascer do conteúdo, não de modelos específicos de aparelho.

```css
.cabecalho {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

@media (min-width: 48rem) {
  .cabecalho {
    flex-direction: row;
    align-items: center;
    justify-content: space-between;
  }
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
  }
}
```

Mídias devem permanecer fluidas (`max-width: 100%; height: auto`). Para imagens, `srcset` e `sizes` permitem oferecer arquivos adequados ao espaço ocupado; `width` e `height` reservam a proporção antes do carregamento.

### Overflow e validação

Overflow costuma surgir de larguras fixas, imagens sem limite, palavras longas, `white-space: nowrap`, colunas que não encolhem ou `100vw` somado a padding. Corrija a causa em vez de esconder o sintoma com `overflow-x: hidden` no `body`.

```css
.texto-longo { overflow-wrap: anywhere; }
.tabela-container { overflow-x: auto; }
.coluna-grid { min-width: 0; }
```

No DevTools, verifique estilos computados, box model, regras sobrescritas, media queries e overflow. Teste redimensionamento contínuo, zoom, teclado, conteúdo maior que o previsto, tema escuro e redução de movimento.

## 7. Diagnóstico integrado

Analise uma página que contenha cabeçalho, navegação, formulário e grade de cartões. Registre, sem corrigir ainda, evidências sobre:

- semântica, landmarks, títulos e nomes acessíveis;
- cascata, especificidade, box model e unidades;
- adequação do uso de Flexbox e Grid;
- comportamento responsivo, media queries e overflow;
- teclado, foco, contraste e preferências do usuário;
- problemas que podem ser confirmados pelo DevTools.

O diagnóstico orienta as retomadas necessárias e prepara a transição para Tailwind CSS no Encontro 2.

## 8. Erros frequentes

- avaliar apenas a aparência visual;
- confundir ausência de erro no console com qualidade completa;
- usar `div` clicável no lugar de controles nativos;
- aplicar JavaScript para resolver algo disponível em HTML ou CSS;
- testar somente com mouse e em uma largura;
- propor correções sem registrar como foram validadas.
- usar ARIA para imitar um elemento nativo disponível;
- escolher títulos pelo tamanho visual;
- ocultar o contorno de foco sem substituição visível;
- usar placeholder como único rótulo;
- escrever texto alternativo genérico ou redundante;
- considerar auditoria automática equivalente a teste humano.
- aumentar a especificidade até forçar um estilo;
- usar posicionamento absoluto como ferramenta principal de layout;
- escolher breakpoints por aparelho, e não pelo conteúdo;
- usar Grid para tudo ou Flexbox para tudo;
- esconder overflow sem corrigir sua origem;
- testar apenas em uma largura de viewport.

## Checklist de compreensão

- [ ] Distingo as responsabilidades de HTML, CSS, JavaScript, TypeScript e Angular.
- [ ] Consigo explicar por que Tailwind e Angular não substituem fundamentos Web.
- [ ] Avalio semântica, funcionalidade, responsividade e acessibilidade.
- [ ] Uso o DevTools para buscar evidências.
- [ ] Registro pontos consolidados e lacunas de aprendizagem.
- [ ] Estruturo landmarks e títulos de forma coerente.
- [ ] Diferencio links, botões e elementos genéricos.
- [ ] Associo rótulos, instruções e erros aos campos.
- [ ] Verifico nomes acessíveis, foco e navegação por teclado.
- [ ] Explico a ordem geral da cascata e diferencio herança de especificidade.
- [ ] Calculo dimensões nos modelos `content-box` e `border-box`.
- [ ] Seleciono unidades e limites de acordo com o comportamento desejado.
- [ ] Escolho Flexbox ou Grid conforme o eixo do problema.
- [ ] Aplico mobile-first e defino breakpoints a partir do conteúdo.
- [ ] Identifico a origem de overflow e valido a interface no DevTools.

## Referências

- [MDN — Aprendizado de desenvolvimento Web](https://developer.mozilla.org/pt-BR/docs/Learn_web_development)
- [Chrome DevTools](https://developer.chrome.com/docs/devtools/)
- [WAI — fundamentos de acessibilidade](https://www.w3.org/WAI/fundamentals/accessibility-intro/)
- [WAI — estrutura de páginas](https://www.w3.org/WAI/tutorials/page-structure/)
- [WAI — formulários](https://www.w3.org/WAI/tutorials/forms/)
- [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
- [MDN — Cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Cascade)
- [MDN — Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Specificity)
- [MDN — Box model](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Box_model)
- [MDN — Responsive design](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design)
- [MDN — Flexbox](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Flexbox)
- [MDN — CSS Grid](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Grids)

[Voltar ao cronograma](../01-cronograma-60h.md)
