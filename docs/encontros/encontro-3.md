# Encontro 3 — Layouts responsivos, Git e DevTools

**Entrega prevista:** layout revisado, testado e versionado

## Visão geral

Design responsivo não significa criar versões separadas para celular e desktop. Significa permitir que conteúdo, controles e layout se adaptem a diferentes larguras, preferências, tamanhos de fonte e formas de entrada.

Este capítulo integra mobile-first, conteúdo fluido, Flexbox, Grid, media queries, investigação no DevTools e registro no Git. Ele encerra a revisão de Padrões Web e prepara a transição para Tailwind CSS, cujas classes responsivas representam os mesmos conceitos de CSS apresentados aqui.

## Objetivos de aprendizagem

- reconhecer problemas de layout rígido e overflow;
- construir uma base mobile-first;
- escolher Flexbox ou Grid conforme o problema;
- utilizar media queries orientadas pelo conteúdo;
- tornar imagens, textos e contêineres fluidos;
- validar diferentes condições no DevTools;
- registrar correções em commits coerentes.

## 1. Responsividade além da largura da tela

Uma interface responsiva considera:

- viewport estreito ou amplo;
- zoom e tamanho de fonte aumentado;
- conteúdo curto, longo ou traduzido;
- orientação retrato e paisagem;
- mouse, toque e teclado;
- preferências de contraste, tema e redução de movimento;
- densidade e resolução de imagem;
- estados vazios, carregando e erro.

O objetivo não é “caber”, mas preservar leitura, operação e hierarquia.

## 2. Viewport e base do documento

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

Sem essa configuração, navegadores móveis podem simular uma viewport larga e reduzir visualmente a página. Ela não torna o site responsivo sozinha; apenas estabelece uma relação correta com a largura do dispositivo.

Uma base CSS útil:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  font-family: system-ui, sans-serif;
  line-height: 1.5;
}

img,
svg,
video {
  display: block;
  max-width: 100%;
}
```

## 3. Mobile-first

Mobile-first define estilos básicos para condições restritas e acrescenta mudanças quando há espaço suficiente.

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
```

Isso não significa projetar apenas para celulares. Significa começar com uma base simples e adicionar complexidade progressivamente. Breakpoints devem surgir onde o conteúdo deixa de funcionar bem, não de uma lista de modelos de aparelhos.

## 4. Conteúdo fluido

Um contêiner pode crescer até um limite de leitura:

```css
.container {
  width: min(100% - 2rem, 72rem);
  margin-inline: auto;
}
```

O conteúdo ocupa a largura disponível menos margens laterais e para de crescer em `72rem`.

### Texto fluido com limites

```css
h1 {
  font-size: clamp(2rem, 1.5rem + 2vw, 3.75rem);
  line-height: 1.1;
}

.introducao {
  max-width: 65ch;
}
```

`clamp()` impede texto pequeno ou grande demais. `ch` limita o comprimento da linha para favorecer leitura.

### Imagens responsivas

```html
<img
  src="campus-960.jpg"
  srcset="campus-480.jpg 480w, campus-960.jpg 960w, campus-1440.jpg 1440w"
  sizes="(min-width: 64rem) 50vw, 100vw"
  alt="Fachada do campus ao entardecer"
  width="1440"
  height="960"
/>
```

`srcset` oferece candidatos; `sizes` informa o espaço aproximado ocupado. `width` e `height` ajudam o navegador a reservar proporção e reduzir deslocamento de layout.

## 5. Flexbox: distribuição em um eixo

Flexbox é adequado quando a preocupação principal está em uma linha ou coluna: navegação, grupo de botões, etiquetas ou alinhamento interno de um cartão.

```css
.acoes {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  align-items: center;
}

.acoes a {
  flex: 1 1 12rem;
}
```

Conceitos principais:

- `flex-direction`: define o eixo principal;
- `justify-content`: distribui no eixo principal;
- `align-items`: alinha no eixo transversal;
- `flex-wrap`: permite quebra;
- `gap`: espaça itens;
- `flex-grow`, `flex-shrink` e `flex-basis`: controlam crescimento, redução e base.

Um erro comum é usar `justify-content` esperando alinhamento no eixo errado. Sempre identifique primeiro `flex-direction`.

## 6. Grid: linhas e colunas

Grid é adequado para organização bidimensional e alinhamento entre linhas e colunas.

```css
.grade {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 17rem), 1fr));
  gap: 1rem;
}
```

Essa grade cria quantas colunas couberem. A expressão `min(100%, 17rem)` impede que a largura mínima de `17rem` ultrapasse uma tela menor.

### Grid explícito com breakpoint

```css
.painel {
  display: grid;
  gap: 1.5rem;
}

@media (min-width: 64rem) {
  .painel {
    grid-template-columns: minmax(0, 2fr) minmax(16rem, 1fr);
  }
}
```

`minmax(0, 2fr)` permite que conteúdo longo encolha dentro da coluna. Sem o zero, itens com largura mínima intrínseca podem causar overflow.

## 7. Como escolher entre Flexbox e Grid

| Situação | Recurso provável |
|---|---|
| alinhar ícone e texto | Flexbox |
| distribuir botões com quebra | Flexbox |
| grade de cartões | Grid |
| layout de conteúdo e barra lateral | Grid |
| alinhamento interno de cartão | Flexbox |
| sobreposição controlada | Grid ou posicionamento, conforme o caso |

Os recursos podem ser combinados. Uma grade pode distribuir cartões e cada cartão pode usar Flexbox internamente.

## 8. Media queries

Media queries aplicam regras quando uma condição do ambiente é verdadeira.

```css
@media (min-width: 48rem) {
  /* alteração de layout quando o conteúdo dispõe de espaço */
}

@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    scroll-behavior: auto !important;
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
  }
}

@media (prefers-color-scheme: dark) {
  :root {
    color-scheme: dark;
  }
}
```

Breakpoints em `rem` acompanham melhor mudanças de escala de texto. Preferências do usuário não devem ser tratadas como detalhes estéticos; podem afetar conforto e acesso.

## 9. Overflow e conteúdo resistente

Fontes comuns de overflow:

- largura fixa maior que o viewport;
- `white-space: nowrap` sem necessidade;
- imagens sem limite;
- URLs ou palavras longas;
- colunas Grid que não podem encolher;
- `100vw` somado a padding;
- elementos posicionados fora do fluxo.

Soluções dependem da causa:

```css
.texto-longo {
  overflow-wrap: anywhere;
}

.tabela-container {
  overflow-x: auto;
}

.coluna-grid {
  min-width: 0;
}
```

Não aplique `overflow-x: hidden` ao `body` apenas para esconder o defeito. Isso pode cortar conteúdo e mascarar a origem.

## 10. Exemplo integrado

```html
<main class="container">
  <header class="hero">
    <div>
      <p class="etiqueta">Desenvolvimento Web</p>
      <h1>Interfaces que se adaptam ao conteúdo</h1>
      <p class="introducao">Uma coleção de práticas para criar experiências legíveis e operáveis.</p>
    </div>
    <div class="acoes">
      <a href="#recursos">Explorar recursos</a>
      <a href="#atividade">Consultar atividade</a>
    </div>
  </header>

  <section id="recursos" aria-labelledby="titulo-recursos">
    <h2 id="titulo-recursos">Recursos</h2>
    <div class="grade">
      <article class="cartao"><h3>HTML</h3><p>Estrutura semântica.</p></article>
      <article class="cartao"><h3>CSS</h3><p>Layout adaptável.</p></article>
      <article class="cartao"><h3>JavaScript</h3><p>Comportamento da interface.</p></article>
    </div>
  </section>
</main>
```

```css
.container {
  width: min(100% - 2rem, 72rem);
  margin-inline: auto;
}

.hero {
  display: grid;
  gap: 1.5rem;
  padding-block: clamp(2rem, 8vw, 6rem);
}

.acoes {
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
}

.grade {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 16rem), 1fr));
  gap: 1rem;
}

.cartao {
  padding: 1.25rem;
  border: 1px solid #cbd5e1;
  border-radius: 0.75rem;
}

@media (min-width: 48rem) {
  .hero {
    grid-template-columns: minmax(0, 2fr) minmax(15rem, 1fr);
    align-items: end;
  }
}
```

## 11. Validação com DevTools

Uma inspeção técnica deve incluir:

- redimensionamento contínuo, não apenas presets de aparelhos;
- viewport estreito, intermediário e amplo;
- zoom do navegador;
- conteúdo maior que o exemplo original;
- orientação e simulação de toque quando pertinente;
- painel **Computed** e box model para investigar dimensões;
- aba **Network** para verificar imagens e recursos;
- emulação de `prefers-reduced-motion` e tema escuro;
- navegação por teclado em todas as larguras.

Presets são amostras, não uma lista completa de dispositivos suportados.

## 12. Versionamento das correções

Organize mudanças por finalidade:

```bash
git switch -c revisao-responsiva
git status
git diff
git add index.html styles.css
git commit -m "Adapta grade de cartões para telas estreitas"

git add styles.css
git commit -m "Corrige overflow de conteúdo longo"
```

Antes do commit, `git diff` permite conferir alterações acidentais. O nome da branch comunica seu propósito; as mensagens descrevem resultados.

## 13. Atividade de aplicação

Revise uma página existente que contenha cabeçalho, ações e cartões. A solução deve:

- partir de uma estrutura mobile-first;
- usar Flexbox em um agrupamento unidimensional;
- usar Grid em uma organização bidimensional;
- possuir contêiner e tipografia fluidos com limites;
- evitar overflow com conteúdo extenso;
- manter foco visível e ordem de leitura;
- respeitar redução de movimento, se houver animação;
- registrar pelo menos duas correções em commits separados.

### Relatório de validação

| Cenário | Resultado esperado | Evidência | Situação |
|---|---|---|---|
| viewport estreito | uma coluna, sem corte |  |  |
| viewport intermediário | reorganização legível |  |  |
| viewport amplo | uso equilibrado do espaço |  |  |
| zoom ampliado | texto sem sobreposição |  |  |
| teclado | foco e ordem coerentes |  |  |
| conteúdo longo | sem overflow indevido |  |  |

## 14. Erros frequentes

- criar breakpoint para cada aparelho conhecido;
- usar posições absolutas para estruturar a página;
- fixar altura em blocos de texto;
- esconder overflow sem corrigir a causa;
- inverter a ordem visual e prejudicar a ordem de leitura;
- usar Grid para tudo ou Flexbox para tudo;
- testar somente na largura final;
- misturar várias correções não relacionadas em um commit;
- considerar a emulação equivalente a testes em todos os dispositivos reais.

## Checklist de compreensão

- [ ] Explico mobile-first sem associá-lo apenas a celulares.
- [ ] Construo contêineres, textos e mídias fluidos.
- [ ] Escolho Flexbox ou Grid conforme o eixo do problema.
- [ ] Defino breakpoints a partir do conteúdo.
- [ ] Identifico e corrijo a origem de overflow.
- [ ] Testo zoom, teclado e preferências do usuário.
- [ ] Registro correções em commits claros.

## Questões de fixação

1. Por que breakpoints não devem depender apenas de modelos de dispositivos?
<!-- Gabarito: dispositivos e condições variam; o breakpoint deve responder ao ponto em que o conteúdo perde legibilidade ou funcionalidade. -->

2. Quando Flexbox é mais adequado que Grid?
<!-- Gabarito: quando o problema principal é distribuição e alinhamento em um eixo, como linha de ações ou conteúdo interno. -->

3. O que `auto-fit` com `minmax()` oferece a uma grade?
<!-- Gabarito: cria automaticamente o número de colunas que cabe, respeitando limites mínimo e máximo. -->

4. Por que `overflow-x: hidden` no body pode ser uma correção ruim?
<!-- Gabarito: esconde sintomas, pode cortar conteúdo e não resolve o elemento que excede a largura. -->

5. Que vantagem existe em separar correções responsivas em commits coerentes?
<!-- Gabarito: facilita revisão, compreensão do histórico, localização de regressões e reversão seletiva. -->

## Referências

- [MDN — Responsive design](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design)
- [MDN — Flexbox](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Flexbox)
- [MDN — CSS Grid](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Grids)
- [web.dev — Responsive design](https://web.dev/learn/design/)
- [Git — documentação](https://git-scm.com/doc)

[Voltar ao cronograma](../01-cronograma-60h.md)
