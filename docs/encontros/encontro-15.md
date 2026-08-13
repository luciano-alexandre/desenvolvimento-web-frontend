# Encontro 15 — TypeScript aplicado ao DOM

**Carga horária:** 1,5h  
**Entrega prevista:** interface com eventos e estados tipados

## Visão geral

Este encontro conecta os fundamentos de TypeScript ao navegador. A proposta é selecionar elementos com segurança, tipar eventos, modelar estados da interface e separar leitura do DOM, transformação de dados e renderização.

## Objetivos de aprendizagem

- tipar consultas e eventos do DOM;
- tratar a possibilidade de elementos ausentes;
- modelar estados com unions discriminadas;
- criar funções pequenas para atualizar a interface;
- validar compilação, console, teclado e foco.

## 1. Seleção segura de elementos

`querySelector` pode retornar `null`. O código deve confirmar a existência do elemento antes de utilizá-lo.

```ts
const formulario = document.querySelector<HTMLFormElement>("#filtro");
const campo = document.querySelector<HTMLInputElement>("#busca");
const resultado = document.querySelector<HTMLElement>("#resultado");

if (!formulario || !campo || !resultado) {
  throw new Error("Elementos obrigatorios nao encontrados");
}
```

Evite usar asserções `as` apenas para silenciar o compilador. A verificação em tempo de execução documenta uma dependência real da interface.

## 2. Eventos tipados

```ts
formulario.addEventListener("submit", (evento: SubmitEvent) => {
  evento.preventDefault();
  const termo = campo.value.trim();
  resultado.textContent = termo
    ? `Busca por: ${termo}`
    : "Informe um termo para pesquisar.";
});
```

O tipo do evento informa quais propriedades estão disponíveis. O elemento de origem deve ser obtido de uma referência já validada ou refinado com `instanceof`.

## 3. Estados da interface

```ts
type EstadoBusca =
  | { tipo: "inicial" }
  | { tipo: "carregando" }
  | { tipo: "sucesso"; quantidade: number }
  | { tipo: "vazio" }
  | { tipo: "erro"; mensagem: string };

function mensagemDoEstado(estado: EstadoBusca): string {
  switch (estado.tipo) {
    case "inicial": return "Digite um termo para iniciar.";
    case "carregando": return "Buscando...";
    case "sucesso": return `${estado.quantidade} resultado(s).`;
    case "vazio": return "Nenhum resultado encontrado.";
    case "erro": return estado.mensagem;
  }
}
```

A union discriminada impede combinações incoerentes e permite narrowing pelo campo `tipo`.

## 4. Prática guiada

Implemente uma interface de filtro que:

1. leia um termo de um formulário acessível;
2. filtre uma coleção tipada;
3. represente os estados inicial, sucesso e vazio;
4. atualize uma região de resultados;
5. preserve foco visível e operação por teclado;
6. não apresente erros de compilação ou console.

## 5. Critérios de aceite

- consultas ao DOM tratam valores nulos;
- eventos e dados possuem tipos coerentes;
- estados impossíveis não podem ser representados;
- funções possuem responsabilidades claras;
- a interface comunica estados por texto, não apenas por cor;
- a solução funciona com teclado.

## Checklist de compreensão

- [ ] Trato o retorno potencialmente nulo de `querySelector`.
- [ ] Tipo eventos sem recorrer a `any`.
- [ ] Uso narrowing para acessar dados específicos de cada estado.
- [ ] Separo dados, regras e atualização do DOM.
- [ ] Testo compilação, console, teclado e estados alternativos.

## Referências

- [TypeScript — DOM Manipulation](https://www.typescriptlang.org/docs/handbook/dom-manipulation.html)
- [TypeScript — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [MDN — EventTarget](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget)

[Voltar ao cronograma](../01-cronograma-60h.md)
