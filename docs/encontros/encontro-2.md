# Encontro 2 — Oficina diagnóstica de HTML e CSS

**Entrega prevista:** página semântica, acessível e responsiva revisada

## Visão geral

Este encontro aplica o diagnóstico e os conceitos revisados no Encontro 1. A turma parte de uma interface com problemas observáveis, confirma cada problema com evidências e implementa correções integradas de HTML e CSS.

Não se trata de acrescentar novos fundamentos, mas de demonstrar domínio sobre semântica, acessibilidade, cascata, box model, unidades, Flexbox, Grid e responsividade.

## Objetivos de aprendizagem

- transformar o diagnóstico do Encontro 1 em correções verificáveis;
- preservar semântica e ordem de leitura ao alterar o layout;
- reduzir conflitos de cascata e especificidade;
- escolher Flexbox ou Grid segundo o problema;
- corrigir overflow e adaptar a interface com abordagem mobile-first;
- validar teclado, foco, zoom e diferentes larguras.

## 1. Ponto de partida

Utilize uma página que contenha:

- cabeçalho e navegação;
- conteúdo principal com títulos e cartões;
- imagem ou outra mídia;
- formulário com pelo menos dois campos;
- grupo de ações;
- folha de estilos externa.

A versão inicial deve possuir problemas suficientes para exigir análise: elementos genéricos no lugar de controles, rótulos ausentes, seletores excessivamente fortes, dimensões fixas, layout frágil e foco pouco visível.

## 2. Ordem da oficina

### Etapa 1 — Estrutura e acessibilidade

1. identifique o assunto principal e a hierarquia de títulos;
2. revise `header`, `nav`, `main`, `section`, `article` e `footer`;
3. diferencie links de botões;
4. associe `label`, instruções e mensagens aos campos;
5. verifique nomes acessíveis, texto alternativo e ordem de foco.

### Etapa 2 — Cascata e modelo de caixa

1. localize declarações sobrescritas;
2. remova `!important` e seletores fortes sem necessidade;
3. adote `border-box` e confirme as dimensões calculadas;
4. substitua espaçamentos repetitivos por `gap` quando pertinente;
5. troque dimensões rígidas por limites fluidos.

### Etapa 3 — Layout

- use Flexbox para um agrupamento unidimensional;
- use Grid para uma organização bidimensional;
- mantenha a ordem visual compatível com a ordem do documento;
- permita quebra e redução dos itens;
- evite posicionamento absoluto para estruturar a página.

### Etapa 4 — Responsividade

1. comece com uma apresentação funcional em viewport estreito;
2. deixe contêiner, texto e mídia fluidos com limites;
3. acrescente breakpoint somente onde o conteúdo exigir;
4. corrija a origem de qualquer overflow;
5. respeite `prefers-reduced-motion` se houver animação.

## 3. Exemplo de direção para a solução

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}

body {
  margin: 0;
  color: #172033;
  font-family: system-ui, sans-serif;
  line-height: 1.5;
}

.container {
  width: min(100% - 2rem, 72rem);
  margin-inline: auto;
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

img {
  display: block;
  max-width: 100%;
  height: auto;
}

:focus-visible {
  outline: 3px solid #1d4ed8;
  outline-offset: 3px;
}
```

O exemplo é uma referência, não uma solução para ser copiada sem análise. Cada regra deve responder a um problema demonstrado na interface.

## 4. Validação

| Cenário | Resultado esperado | Evidência | Situação |
|---|---|---|---|
| HTML sem CSS | estrutura e ordem compreensíveis |  |  |
| teclado | controles alcançáveis e foco visível |  |  |
| viewport estreito | uma coluna, sem cortes |  |  |
| viewport intermediário | reorganização legível |  |  |
| viewport amplo | uso equilibrado do espaço |  |  |
| zoom ampliado | texto sem sobreposição |  |  |
| conteúdo longo | sem overflow indevido |  |  |

## 5. Entrega

A entrega deve conter:

- página revisada;
- folha CSS sem estilos inline e sem `!important` desnecessário;
- ao menos um uso justificado de Flexbox e um de Grid;
- comportamento responsivo mobile-first;
- tabela de validação preenchida;
- lista breve das correções e respectivas evidências.

## Critérios de verificação

- estrutura semântica e acessível;
- estilos previsíveis e de baixa especificidade;
- box model e unidades adequados;
- layout coerente com o conteúdo;
- responsividade sem ocultar defeitos;
- evidências reproduzíveis.

## Checklist de compreensão

- [ ] Justifico cada correção a partir de uma evidência.
- [ ] Preservo semântica e ordem de leitura.
- [ ] Resolvo conflitos sem elevar desnecessariamente a especificidade.
- [ ] Uso Flexbox e Grid segundo seus eixos.
- [ ] Defino breakpoints pelo conteúdo.
- [ ] Testo teclado, zoom, larguras e conteúdo longo.

## Referências

- [MDN — CSS](https://developer.mozilla.org/pt-BR/docs/Web/CSS)
- [MDN — Responsive design](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Responsive_Design)
- [WAI — Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/)

[Voltar ao cronograma](../01-cronograma-60h.md)
