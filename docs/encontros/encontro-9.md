# Encontro 9 — JavaScript: decisões e estruturas condicionais

**Unidade:** Unidade 1
**Carga horária:** 1,5h
**Entrega prevista:** regras de inscrição implementadas e testadas

## Visão geral

No Encontro 8, expressões de comparação produziram valores booleanos, mas o programa sempre executou todas as instruções. Agora esses resultados serão usados para escolher caminhos diferentes. O diagnóstico de inscrições passará a informar se uma solicitação pode ser confirmada, se deve entrar em espera ou se contém dados inválidos.

O foco permanece na linguagem e no console. DOM e eventos serão estudados depois. Cada condição será construída a partir de uma regra escrita primeiro em linguagem natural, testada nos limites e simplificada somente quando seu comportamento estiver comprovado.

## Objetivos de aprendizagem

- relacionar uma regra de negócio a uma expressão booleana;
- controlar caminhos com `if`, `else if` e `else`;
- distinguir condições independentes de caminhos mutuamente exclusivos;
- combinar condições com `&&`, `||` e `!`;
- utilizar comparação estrita;
- reconhecer valores truthy e falsy sem depender de coerções frágeis;
- usar retorno antecipado para reduzir aninhamento;
- escolher conscientemente entre condicional completo e operador ternário;
- testar valores comuns, limites e entradas inválidas;
- acompanhar decisões com breakpoints no DevTools.

## Conceitos essenciais

- fluxo de controle;
- condição e bloco;
- caminhos mutuamente exclusivos;
- ordem das verificações;
- operadores lógicos;
- truthy e falsy;
- guard clauses;
- operador ternário;
- casos de teste e limites.

## 1. Retomar o projeto

Continue em `encontro-08-javascript`:

```bash
cd encontro-08-javascript
docker compose up
```

Abra `http://localhost:8080`, mantenha o Console visível e confirme que `js/app.js` ainda executa. Faça uma cópia do código anterior ou registre um commit antes de modificá-lo.

O ponto de partida contém estes valores:

```js
const capacidade = 30;
const quantidadeInscritos = 23;
const novasInscricoesTexto = "4";
const inscricoesAbertas = true;

const novasInscricoes = Number(novasInscricoesTexto);
const totalPrevisto = quantidadeInscritos + novasInscricoes;
const quantidadeValida = !Number.isNaN(novasInscricoes);
const dentroDaCapacidade = totalPrevisto <= capacidade;
```

Antes de criar uma condicional, preveja os valores de `quantidadeValida` e `dentroDaCapacidade`.

## 2. Do booleano à decisão

Uma condicional executa um bloco somente quando sua condição é verdadeira:

```js
if (dentroDaCapacidade) {
  console.log("Há capacidade para confirmar a solicitação.");
}
```

Leia a estrutura em três partes:

1. `if`: inicia uma decisão;
2. `(dentroDaCapacidade)`: expressão avaliada como booleana;
3. `{ ... }`: bloco executado se o resultado for `true`.

Troque `novasInscricoesTexto` por `"8"`. A mensagem deixa de aparecer. Isso não é um erro: não foi definido um caminho para a condição falsa.

## 3. Dois caminhos com `else`

Quando existem duas possibilidades excludentes:

```js
if (dentroDaCapacidade) {
  console.log("Solicitação dentro da capacidade.");
} else {
  console.log("Solicitação excede a capacidade.");
}
```

Somente um bloco é executado. Evite escrever duas condições que podem divergir:

```js
// Repetição desnecessária da regra
if (totalPrevisto <= capacidade) {
  console.log("Dentro da capacidade.");
}

if (totalPrevisto > capacidade) {
  console.log("Acima da capacidade.");
}
```

`else` comunica que o segundo caminho é a negação do primeiro.

## 4. Vários caminhos com `else if`

Defina antes a regra:

- entrada inválida: solicitar correção;
- total acima da capacidade: lista de espera;
- total igual à capacidade: confirmar e informar lotação;
- total abaixo da capacidade: confirmar normalmente.

```js
if (!quantidadeValida) {
  console.error("Informe uma quantidade numérica.");
} else if (totalPrevisto > capacidade) {
  console.warn("Não há vagas suficientes. Solicitação em espera.");
} else if (totalPrevisto === capacidade) {
  console.log("Solicitação confirmada. A oficina ficou lotada.");
} else {
  console.log("Solicitação confirmada. Ainda existem vagas.");
}
```

A ordem importa. A entrada inválida é verificada antes dos cálculos de capacidade porque qualquer comparação com `NaN` produziria um diagnóstico enganoso.

### Tabela de decisão

| Entrada | Total | Caminho esperado |
|---|---:|---|
| `"quatro"` | `NaN` | entrada inválida |
| `"8"` | 31 | lista de espera |
| `"7"` | 30 | lotada |
| `"4"` | 27 | ainda há vagas |

Execute os quatro casos, um por vez.

## 5. Condições compostas

Uma confirmação depende de mais de uma regra:

```js
const podeConfirmar =
  inscricoesAbertas &&
  quantidadeValida &&
  novasInscricoes > 0 &&
  totalPrevisto <= capacidade;
```

- `&&` exige que todas as condições sejam verdadeiras;
- `||` aceita pelo menos uma condição verdadeira;
- `!` inverte um booleano.

Use parênteses quando grupos diferentes participarem da mesma expressão:

```js
const possuiPrioridade = participanteIdoso || participanteComDeficiencia;
const podeUsarVagaPrioritaria =
  inscricoesAbertas && (participanteIdoso || participanteComDeficiencia);
```

Não combine regras até que cada parte seja compreendida separadamente.

## 6. Truthy, falsy e comparações explícitas

Condições aceitam qualquer valor, convertendo-o para booleano. São falsy: `false`, `0`, `""`, `null`, `undefined` e `NaN`.

```js
const nomeParticipante = "";

if (nomeParticipante) {
  console.log("Nome informado.");
} else {
  console.warn("Nome ausente.");
}
```

Essa forma é útil para presença textual, mas pode ocultar intenção em regras numéricas. Se zero é um valor válido, escreva a comparação necessária:

```js
const novasInscricoes = 0;
const quantidadeNaoNegativa = novasInscricoes >= 0;
```

Não use truthy/falsy automaticamente. Pergunte se a regra trata presença, igualdade, intervalo ou tipo.

## 7. Validação em etapas

Uma regra grande pode ser verificada em etapas claras:

```js
const entradaValida =
  !Number.isNaN(novasInscricoes) && novasInscricoes > 0;

if (!inscricoesAbertas) {
  console.warn("As inscrições estão encerradas.");
} else if (!entradaValida) {
  console.error("Informe uma quantidade maior que zero.");
} else if (totalPrevisto > capacidade) {
  console.warn("Quantidade acima das vagas disponíveis.");
} else {
  console.log("Inscrições confirmadas.");
}
```

Separar `entradaValida` dá nome à regra e facilita sua inspeção.

## 8. Retorno antecipado

Dentro de uma função, `return` pode encerrar o processamento quando um requisito falha. Observe a ideia; funções serão aprofundadas no Encontro 10:

```js
function diagnosticarInscricao(quantidade) {
  if (Number.isNaN(quantidade)) {
    return "Quantidade inválida.";
  }

  if (!inscricoesAbertas) {
    return "Inscrições encerradas.";
  }

  if (quantidade <= 0) {
    return "A quantidade deve ser maior que zero.";
  }

  return "Quantidade pronta para avaliação de capacidade.";
}
```

Essas verificações iniciais são chamadas de guard clauses. Elas evitam vários níveis de aninhamento.

## 9. Operador ternário

O ternário é adequado para escolher entre dois valores curtos:

```js
const situacao = dentroDaCapacidade ? "disponível" : "em espera";
console.log(`Situação: ${situacao}`);
```

Sua anatomia:

```text
condição ? valor se verdadeiro : valor se falso
```

Evite ternários aninhados para substituir `else if`. Se a leitura exigir decifrar vários `?` e `:`, use uma estrutura condicional.

## 10. Exemplo principal completo

```js
const nomeOficina = "Fotografia urbana";
const capacidade = 30;
const quantidadeInscritos = 23;
const novasInscricoesTexto = "4";
const inscricoesAbertas = true;

const novasInscricoes = Number(novasInscricoesTexto);
const quantidadeValida =
  !Number.isNaN(novasInscricoes) && novasInscricoes > 0;
const totalPrevisto = quantidadeInscritos + novasInscricoes;

let mensagem;

if (!inscricoesAbertas) {
  mensagem = "As inscrições estão encerradas.";
} else if (!quantidadeValida) {
  mensagem = "Informe uma quantidade numérica maior que zero.";
} else if (totalPrevisto > capacidade) {
  const excedente = totalPrevisto - capacidade;
  mensagem = `Solicitação em espera: excede a capacidade em ${excedente}.`;
} else if (totalPrevisto === capacidade) {
  mensagem = "Solicitação confirmada: a oficina ficou lotada.";
} else {
  const vagasRestantes = capacidade - totalPrevisto;
  mensagem = `Solicitação confirmada: restam ${vagasRestantes} vagas.`;
}

console.log(`${nomeOficina} — ${mensagem}`);
```

`mensagem` utiliza `let` porque recebe uma atribuição diferente conforme o caminho. Em cada execução, apenas um bloco da cadeia é selecionado.

## 11. Inspeção e testes

No DevTools:

1. adicione um breakpoint no primeiro `if`;
2. observe os valores usados na condição;
3. avance uma instrução;
4. identifique qual bloco foi selecionado;
5. confirme que os blocos seguintes foram ignorados;
6. repita com um caso de limite e um inválido.

### Erros intencionais

- coloque a verificação de capacidade antes de `quantidadeValida`;
- troque `===` por `=` e interprete o erro ou comportamento;
- remova `else` entre dois caminhos exclusivos;
- use `"0"`, `"7"`, `"8"`, `""` e `"sete"`;
- inverta uma condição com `!` e verbalize a nova regra.

Reverta cada alteração antes de iniciar a próxima.

## 12. Prática guiada

Implemente regras para empréstimo de livros:

- biblioteca aberta;
- 12 exemplares;
- 8 emprestados;
- solicitações recebidas como texto;
- máximo de 2 exemplares por solicitação.

O programa deve distinguir:

1. biblioteca fechada;
2. entrada inválida ou menor que 1;
3. solicitação acima do limite individual;
4. quantidade acima do estoque disponível;
5. empréstimo que utiliza o último exemplar;
6. empréstimo confirmado com exemplares restantes.

Crie uma tabela com ao menos seis entradas e resultados esperados antes de executar.

## 13. Exercício aplicado

Crie regras para um estacionamento, estoque ou agenda de atendimento.

### Requisitos mínimos

- ao menos quatro caminhos mutuamente exclusivos;
- uma validação de `NaN`;
- uma validação de limite inferior;
- uma validação de capacidade;
- uma expressão com `&&`;
- uma regra com `||`;
- uma negação com `!`;
- um ternário usado apenas para selecionar um valor curto;
- seis casos de teste documentados;
- saídas identificadas no console.

Não use loops, arrays, objetos ou DOM.

## 14. Critérios de aceite

- o projeto executa com Docker Compose;
- cada condição corresponde a uma regra explicável;
- verificações inválidas aparecem antes do caminho de sucesso;
- caminhos exclusivos utilizam uma única cadeia;
- limites exatos foram testados;
- nenhuma coerção acidental determina o resultado;
- mensagens correspondem ao caminho executado;
- console não apresenta erros não explicados;
- README registra casos, entradas e resultados.

## 15. Erros comuns

- escrever a condição sem prever os casos;
- confundir atribuição com comparação;
- usar `==` quando o tipo também importa;
- ordenar condições gerais antes das específicas;
- criar vários `if` para caminhos exclusivos;
- aninhar blocos que poderiam ser encerrados antes;
- usar ternário para muitas alternativas;
- tratar zero como inválido sem conferir a regra;
- testar somente o caminho de sucesso.

## Materiais para aprofundamento

- [MDN — Estruturas condicionais](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Scripting/Conditionals)
- [MDN — Operadores lógicos](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators)
- [MDN — Operador condicional](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Operators/Conditional_operator)

## Checklist de compreensão

- [ ] Traduzo uma regra em expressão booleana.
- [ ] Diferencio `if`, `else if` e `else`.
- [ ] Ordeno validações antes do sucesso.
- [ ] Combino condições com operadores lógicos.
- [ ] Sei quando truthy/falsy comunica a intenção.
- [ ] Uso igualdade estrita.
- [ ] Reconheço um caso apropriado para ternário.
- [ ] Testo valor comum, limite e entrada inválida.
- [ ] Acompanho o caminho com breakpoint.

## Resumo final

Condicionais transformam booleanos em caminhos de execução. Uma implementação confiável começa pela regra, prioriza entradas inválidas, diferencia alternativas exclusivas e testa limites. No próximo encontro, as regras serão organizadas em funções e executadas repetidamente sem duplicação manual.

## Questões de fixação

1. Quando usar `else if` em vez de outro `if` independente?
<!-- Gabarito: quando apenas um entre vários caminhos mutuamente exclusivos deve executar. -->

2. Por que a validação de `NaN` deve ocorrer antes da capacidade?
<!-- Gabarito: comparações com NaN não representam uma quantidade válida e podem levar a diagnóstico enganoso. -->

3. O que `&&`, `||` e `!` fazem?
<!-- Gabarito: exigem todas, aceitam pelo menos uma e invertem uma condição, respectivamente. -->

4. Quando o ternário é apropriado?
<!-- Gabarito: para escolher entre dois valores ou expressões curtas com leitura clara. -->

5. Por que testar exatamente o valor da capacidade?
<!-- Gabarito: limites costumam seguir caminho diferente e revelam erros entre < e <=. -->

[Voltar ao cronograma](../01-cronograma-60h.md)
