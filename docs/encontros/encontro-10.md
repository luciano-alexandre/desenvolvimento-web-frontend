# Encontro 10 — JavaScript: repetição, funções e escopo

**Unidade:** Unidade 1
**Carga horária:** 1,5h
**Entrega prevista:** conjunto de funções reutilizáveis para simular inscrições

## Visão geral

No Encontro 9, uma cadeia condicional decidiu o resultado de uma solicitação. Entretanto, repetir o mesmo bloco para várias entradas produziria código duplicado. Neste encontro, funções darão nome a processos reutilizáveis e estruturas de repetição executarão uma tarefa enquanto uma condição ou sequência exigir.

O exemplo continuará no console e ainda não utilizará arrays, objetos ou DOM. Primeiro será extraído um cálculo para uma função; depois serão introduzidos parâmetros, retorno, escopo e laços. A distinção entre **calcular**, **decidir** e **exibir** orientará a organização.

## Objetivos de aprendizagem

- identificar repetição manual no código;
- declarar e chamar funções;
- diferenciar parâmetro de argumento;
- devolver valores com `return`;
- separar cálculo, decisão e efeito de saída;
- compreender escopo global, de função e de bloco;
- escolher entre `for` e `while`;
- controlar inicialização, condição e atualização;
- utilizar `break` e `continue` somente com justificativa;
- evitar laços infinitos e dependências globais;
- rastrear chamadas e iterações no DevTools.

## Conceitos essenciais

- declaração e chamada;
- parâmetro, argumento e retorno;
- responsabilidade única;
- função pura e efeito colateral;
- escopo léxico;
- contador e acumulador;
- `for` e `while`;
- condição de parada;
- `break` e `continue`.

## 1. Retomar o projeto

Continue em `encontro-08-javascript`:

```bash
cd encontro-08-javascript
docker compose up
```

Crie `js/encontro-10.js` e altere temporariamente o `script` do HTML:

```html
<script type="module" src="./js/encontro-10.js"></script>
```

Separar o arquivo preserva a solução do encontro anterior e facilita a comparação.

## 2. Por que criar funções

Sem função, o mesmo cálculo aparece várias vezes:

```js
const vagasFotografia = 30 - 23;
const vagasCeramica = 20 - 17;
const vagasDesenho = 25 - 19;
```

A operação é a mesma; apenas os dados variam. Dê nome ao processo:

```js
function calcularVagas(capacidade, quantidadeInscritos) {
  return capacidade - quantidadeInscritos;
}
```

Agora execute:

```js
const vagasFotografia = calcularVagas(30, 23);
const vagasCeramica = calcularVagas(20, 17);
const vagasDesenho = calcularVagas(25, 19);
```

- `calcularVagas` é o nome da função;
- `capacidade` e `quantidadeInscritos` são parâmetros;
- `30` e `23` são argumentos da primeira chamada;
- `return` devolve o resultado ao ponto da chamada.

## 3. Retorno não é exibição

Compare:

```js
function calcularVagas(capacidade, inscritos) {
  console.log(capacidade - inscritos);
}

const vagas = calcularVagas(30, 23);
console.log(vagas);
```

A primeira função exibe `7`, mas não retorna valor; portanto, `vagas` recebe `undefined`.

A versão reutilizável devolve o dado:

```js
function calcularVagas(capacidade, inscritos) {
  return capacidade - inscritos;
}

const vagas = calcularVagas(30, 23);
console.log(`Vagas: ${vagas}`);
```

A função calcula; a chamada decide como apresentar. Isso permite reutilizar o resultado em comparação, mensagem ou outro cálculo.

## 4. Código após `return`

`return` encerra a execução da função:

```js
function validarQuantidade(quantidade) {
  if (Number.isNaN(quantidade)) {
    return false;
  }

  return quantidade > 0;
}
```

Não coloque instrução necessária depois de um retorno incondicional:

```js
function exemplo() {
  return "fim";
  console.log("Esta linha não executa.");
}
```

## 5. Funções com uma responsabilidade

Separe tarefas:

```js
function converterQuantidade(texto) {
  return Number(texto);
}

function quantidadeEhValida(quantidade) {
  return !Number.isNaN(quantidade) && quantidade > 0;
}

function cabeNaCapacidade(capacidade, inscritos, solicitados) {
  return inscritos + solicitados <= capacidade;
}
```

Cada nome expressa uma pergunta ou transformação. Uma função curta não é automaticamente boa: sua responsabilidade precisa ser coerente.

### Função de diagnóstico

```js
function diagnosticarSolicitacao(
  capacidade,
  inscritos,
  quantidadeSolicitada,
  inscricoesAbertas
) {
  if (!inscricoesAbertas) {
    return "Inscrições encerradas.";
  }

  if (!quantidadeEhValida(quantidadeSolicitada)) {
    return "Quantidade inválida.";
  }

  if (!cabeNaCapacidade(capacidade, inscritos, quantidadeSolicitada)) {
    return "Solicitação em espera.";
  }

  const vagasRestantes =
    capacidade - inscritos - quantidadeSolicitada;

  return `Solicitação confirmada. Restam ${vagasRestantes} vagas.`;
}
```

## 6. Escopo

Variáveis declaradas dentro da função pertencem ao escopo daquela função:

```js
function calcularTotal(inscritos, solicitados) {
  const total = inscritos + solicitados;
  return total;
}

console.log(calcularTotal(23, 4));
// console.log(total); // ReferenceError
```

Blocos também criam escopo para `const` e `let`:

```js
if (true) {
  const mensagem = "Disponível apenas dentro do bloco.";
  console.log(mensagem);
}

// console.log(mensagem); // ReferenceError
```

Evite funções que dependem silenciosamente de muitas variáveis globais:

```js
// Dependência escondida
function calcularVagasFragil() {
  return capacidade - quantidadeInscritos;
}

// Dependências explícitas
function calcularVagas(capacidade, quantidadeInscritos) {
  return capacidade - quantidadeInscritos;
}
```

## 7. Repetição manual

Exibir números de inscrição manualmente não escala:

```js
console.log("Inscrição 1");
console.log("Inscrição 2");
console.log("Inscrição 3");
```

Um laço repete um bloco segundo uma regra.

## 8. Laço `for`

Use `for` quando a quantidade de iterações é conhecida:

```js
for (let numero = 1; numero <= 3; numero += 1) {
  console.log(`Inscrição ${numero}`);
}
```

Anatomia:

```text
for (inicialização; condição; atualização) {
  bloco repetido
}
```

1. `let numero = 1` executa uma vez;
2. `numero <= 3` é testada antes de cada iteração;
3. o bloco executa quando a condição é verdadeira;
4. `numero += 1` atualiza o contador;
5. o processo retorna à condição.

Teste os limites `< 3` e `<= 3` e explique a diferença.

## 9. Contadores e acumuladores

Um contador registra ocorrências; um acumulador combina valores:

```js
let quantidadeConfirmada = 0;
let totalVagasOcupadas = 0;

for (let numero = 1; numero <= 4; numero += 1) {
  const quantidadeSolicitada = numero;
  quantidadeConfirmada += 1;
  totalVagasOcupadas += quantidadeSolicitada;
}

console.log(quantidadeConfirmada);
console.log(totalVagasOcupadas);
```

Os acumuladores são declarados antes do laço porque seu resultado precisa existir depois das iterações.

## 10. Laço `while`

Use `while` quando a repetição depende de uma condição cuja duração não é conhecida antecipadamente:

```js
let vagasDisponiveis = 7;
let numeroConfirmacao = 1;

while (vagasDisponiveis > 0) {
  console.log(`Confirmação ${numeroConfirmacao}`);
  vagasDisponiveis -= 1;
  numeroConfirmacao += 1;
}
```

Uma variável usada na condição precisa mudar dentro do processo. Se `vagasDisponiveis -= 1` for removido, a condição continuará verdadeira e criará um laço infinito.

## 11. `break` e `continue`

`break` encerra o laço:

```js
for (let tentativa = 1; tentativa <= 10; tentativa += 1) {
  if (tentativa === 4) {
    console.log("Condição encontrada.");
    break;
  }
}
```

`continue` ignora o restante da iteração atual:

```js
for (let numero = 1; numero <= 5; numero += 1) {
  if (numero === 3) {
    continue;
  }

  console.log(numero);
}
```

Não use esses comandos para compensar uma condição confusa. O fluxo deve continuar legível.

## 12. Exemplo principal completo

Simule solicitações unitárias até preencher as vagas disponíveis:

```js
function calcularVagas(capacidade, inscritos) {
  return capacidade - inscritos;
}

function podeConfirmar(capacidade, inscritos, solicitados) {
  const quantidadeValida =
    !Number.isNaN(solicitados) && solicitados > 0;
  const dentroDaCapacidade =
    inscritos + solicitados <= capacidade;

  return quantidadeValida && dentroDaCapacidade;
}

function criarMensagem(nomeOficina, confirmadas, vagasRestantes) {
  return (
    `${nomeOficina}: ${confirmadas} solicitações confirmadas; ` +
    `${vagasRestantes} vagas restantes.`
  );
}

const nomeOficina = "Fotografia urbana";
const capacidade = 30;
let quantidadeInscritos = 26;
let quantidadeConfirmada = 0;

while (podeConfirmar(capacidade, quantidadeInscritos, 1)) {
  quantidadeInscritos += 1;
  quantidadeConfirmada += 1;
  console.log(`Confirmada inscrição ${quantidadeInscritos}.`);
}

const vagasRestantes =
  calcularVagas(capacidade, quantidadeInscritos);
const resumo =
  criarMensagem(nomeOficina, quantidadeConfirmada, vagasRestantes);

console.log(resumo);
```

Observe:

- funções recebem dependências por parâmetros;
- funções de cálculo retornam valores;
- o `while` modifica o estado usado na condição;
- a saída ocorre fora das funções de cálculo;
- o laço termina exatamente na capacidade.

## 13. Inspeção e diagnóstico

No DevTools:

1. coloque breakpoint dentro de `podeConfirmar`;
2. observe parâmetros e variáveis locais;
3. avance até o `return`;
4. acompanhe a pilha de chamadas;
5. continue até a última iteração;
6. confirme por que a próxima condição é falsa.

### Experimentos

- retire um `return` e observe `undefined`;
- tente acessar uma variável local fora da função;
- mude `<=` para `<`;
- remova a atualização de um `while` e interrompa rapidamente a execução;
- inicie um `for` em zero e confira a contagem;
- passe string onde a função espera comportamento numérico.

## 14. Prática guiada

Crie funções para uma biblioteca:

- `calcularDisponiveis(total, emprestados)`;
- `quantidadeEhValida(quantidade)`;
- `podeEmprestar(total, emprestados, solicitados)`;
- `criarResumo(titulo, emprestimosConfirmados, disponiveis)`.

Depois, use um `while` para confirmar solicitações unitárias até restarem dois exemplares reservados. Registre cada confirmação e o resumo final.

## 15. Exercício aplicado

Escolha estoque, estacionamento ou agenda. Implemente:

- quatro funções com parâmetros;
- três funções que retornam valores;
- ao menos uma guard clause;
- um `for` com limite definido;
- um `while` com condição de parada;
- um contador e um acumulador;
- um uso justificado de `break` ou `continue`;
- três cenários documentados;
- nenhuma dependência global oculta nas funções de cálculo.

Não utilize arrays, objetos ou DOM.

## 16. Critérios de aceite

- funções possuem nomes e responsabilidades compreensíveis;
- parâmetros tornam dependências explícitas;
- resultados úteis são devolvidos com `return`;
- variáveis respeitam o menor escopo necessário;
- cada laço possui condição de parada;
- limites não executam uma vez a mais ou a menos;
- contador e acumulador começam com valor coerente;
- cálculo e saída estão separados;
- console não apresenta erros não explicados;
- README registra execução e testes.

## 17. Erros comuns

- declarar função e nunca chamá-la;
- confundir parâmetro com argumento;
- exibir dentro da função quando o valor deveria ser retornado;
- esperar valor de função sem `return`;
- acessar variável fora de seu escopo;
- depender de variável global sem necessidade;
- esquecer atualização no `while`;
- usar condição `<=` quando o limite deveria ser exclusivo;
- modificar o contador no sentido errado;
- escolher laço antes de definir sua condição de parada.

## Materiais para aprofundamento

- [MDN — Funções](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Functions)
- [MDN — Laços e iterações](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Loops_and_iteration)
- [MDN — Escopos](https://developer.mozilla.org/pt-BR/docs/Glossary/Scope)

## Checklist de compreensão

- [ ] Declaro e chamo uma função.
- [ ] Diferencio parâmetro e argumento.
- [ ] Sei quando devolver um valor.
- [ ] Reconheço escopo de função e bloco.
- [ ] Escolho entre `for` e `while`.
- [ ] Identifico contador e acumulador.
- [ ] Defino condição de parada.
- [ ] Justifico `break` ou `continue`.
- [ ] Acompanho a pilha de chamadas.
- [ ] Testo limites de repetição.

## Resumo final

Funções encapsulam processos, tornam dependências explícitas e devolvem resultados reutilizáveis. Laços repetem uma tarefa segundo uma condição controlada. Juntos, esses recursos eliminam repetição manual e preparam o processamento de coleções. No Encontro 11, arrays reunirão vários valores e objetos representarão dados relacionados.

## Questões de fixação

1. Qual é a diferença entre parâmetro e argumento?
<!-- Gabarito: parâmetro é o nome na declaração; argumento é o valor fornecido na chamada. -->

2. Por que `console.log` não substitui `return`?
<!-- Gabarito: log apenas exibe; return devolve um valor que pode ser reutilizado. -->

3. Quando escolher `for` ou `while`?
<!-- Gabarito: for quando a sequência é controlada; while quando a duração depende de uma condição. -->

4. O que causa um laço infinito?
<!-- Gabarito: a condição permanece verdadeira porque seu estado não progride para a parada. -->

5. Por que passar dados por parâmetros?
<!-- Gabarito: torna dependências explícitas e permite reutilizar/testar a função com outros valores. -->

[Voltar ao cronograma](../01-cronograma-60h.md)
