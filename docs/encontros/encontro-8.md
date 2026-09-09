# Encontro 8 — JavaScript: execução, variáveis, tipos e operadores

**Unidade:** Unidade 1

## Visão geral

Os encontros 2 a 6 trataram diretamente da construção visual com Tailwind CSS. A partir deste encontro, o foco passa a ser **JavaScript**, linguagem responsável pelo comportamento das interfaces no navegador. Tailwind não será objeto direto das próximas aulas; quando alguma interface precisar de apresentação, poderá ser usado CSS básico já fornecido, sem avaliação de classes utilitárias.

Neste primeiro contato, o objetivo não é alterar elementos da página. Antes de trabalhar com DOM e eventos, é necessário compreender como um programa é carregado, como os dados são representados e como expressões produzem novos valores. O console do navegador será o ambiente principal de observação.

O exemplo central calcula a ocupação de uma oficina comunitária. A cada etapa, serão identificadas **entradas**, **processamento** e **saídas**, evitando uma sequência de comandos copiados sem modelo mental.

## Objetivos de aprendizagem

- explicar a responsabilidade do JavaScript em uma aplicação frontend;
- conectar um arquivo JavaScript externo a uma página HTML;
- interpretar a execução sequencial de um programa;
- utilizar o console e o DevTools para observar valores e erros;
- declarar dados com `const` e `let`;
- reconhecer strings, numbers, booleans, `undefined` e `null`;
- verificar tipos com `typeof`;
- aplicar operadores aritméticos, de comparação e lógicos;
- converter explicitamente valores textuais em números;
- construir mensagens com template strings;
- identificar entrada, processamento e saída;
- diagnosticar erros comuns sem alterar várias linhas ao mesmo tempo.

## Conceitos essenciais

- JavaScript no navegador;
- arquivo externo e módulo;
- instrução, expressão e valor;
- execução sequencial;
- `console.log`, `console.warn` e `console.error`;
- identificadores, `const` e `let`;
- tipos primitivos;
- conversão explícita e `NaN`;
- operadores;
- template strings;
- entrada, processamento e saída.

## 1. O papel do JavaScript

HTML representa estrutura e significado; CSS representa apresentação; JavaScript processa dados e responde a acontecimentos.

```text
HTML        → quais conteúdos e controles existem?
CSS         → como esses elementos são apresentados?
JavaScript  → quais dados mudam e o que acontece durante a interação?
```

Exemplos de responsabilidades futuras do JavaScript:

- validar dados antes do envio;
- abrir e fechar um menu;
- filtrar uma coleção;
- calcular valores;
- buscar informações em uma API;
- apresentar estados de carregamento, sucesso e erro.

Neste encontro, o resultado será mostrado no console. Isso permite estudar a linguagem sem misturar, ainda, seleção de elementos e eventos.

## 2. Preparar um projeto independente

Crie uma nova pasta. Não continue o projeto da avaliação de Tailwind:

```bash
mkdir encontro-08-javascript
cd encontro-08-javascript
mkdir js
touch index.html js/app.js compose.yaml
```

A estrutura será:

```text
encontro-08-javascript/
├── compose.yaml
├── index.html
└── js/
    └── app.js
```

Não existe etapa de compilação do JavaScript neste encontro. O navegador interpreta o arquivo-fonte diretamente. O contêiner será usado apenas para servir os arquivos por HTTP.

### Servir os arquivos com Docker

Em `compose.yaml`:

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - .:/usr/share/nginx/html:ro
```

Inicie o serviço:

```bash
docker compose up
```

Abra `http://localhost:8080`. O volume é somente leitura dentro do contêiner, indicado por `:ro`: o Nginx pode entregar os arquivos, mas não modificá-los.

Ao final da aula, encerre com `Ctrl+C` e:

```bash
docker compose down
```

## 3. Criar o documento HTML

Em `index.html`:

```html
<!doctype html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Diagnóstico de inscrições</title>
    <script type="module" src="./js/app.js"></script>
  </head>
  <body>
    <main>
      <h1>Diagnóstico de inscrições</h1>
      <p>Abra o DevTools e consulte o painel Console.</p>
    </main>
  </body>
</html>
```

O elemento `script` referencia um arquivo externo:

- `src` informa o caminho do arquivo;
- `type="module"` trata o arquivo como módulo JavaScript;
- módulos são executados em modo estrito;
- o navegador aguarda a análise do HTML antes de executar o módulo;
- o código permanece separado da estrutura do documento.

Não escreva comportamento em atributos como `onclick`. Eventos serão estudados em outro encontro.

### Checkpoint 1 — Conexão

Em `js/app.js`, escreva:

```js
console.log("JavaScript conectado.");
```

Abra o DevTools com o atalho do navegador, selecione **Console** e recarregue a página. A mensagem comprova que:

1. o servidor entregou o HTML;
2. o HTML encontrou `js/app.js`;
3. o navegador interpretou a instrução;
4. `console.log` exibiu a saída.

Se a mensagem não aparecer, verifique o endereço, a aba Console, o caminho de `src` e a extensão do arquivo.

## 4. Instruções, expressões e valores

Uma **instrução** orienta a execução de uma ação. Uma **expressão** é um trecho que produz um valor.

```js
console.log(10 + 5);
```

- `10 + 5` é uma expressão e produz `15`;
- `console.log(...)` recebe esse valor e o exibe;
- a chamada completa forma uma instrução.

O programa é executado de cima para baixo:

```js
console.log("Início");
console.log(10 + 5);
console.log("Fim");
```

Altere apenas uma linha por vez e recarregue. O console mantém a ordem das três saídas.

## 5. Dados com `const` e `let`

Uma variável associa um nome a um valor. Nomes claros comunicam o papel do dado.

```js
const nomeOficina = "Fotografia urbana";
const capacidade = 30;
let quantidadeInscritos = 23;
```

Use `const` quando a associação não será reatribuída. Use `let` quando o programa precisar atribuir outro valor posteriormente.

```js
quantidadeInscritos = 24;
```

Não interprete `const` como “valor universalmente imutável”. Neste momento, basta compreender que o identificador não pode receber outra atribuição.

### Nomes de identificadores

Prefira nomes que indiquem significado:

```js
const capacidade = 30;
const quantidadeInscritos = 23;
```

Evite abreviações vagas:

```js
const c = 30;
const qi = 23;
```

Em JavaScript, `quantidadeInscritos` e `quantidadeinscritos` são identificadores diferentes. A convenção `camelCase` inicia com letra minúscula e destaca cada palavra seguinte.

## 6. Tipos primitivos

O tipo descreve a natureza de um valor e influencia as operações disponíveis.

| Tipo | Exemplo | Uso no cenário |
|---|---|---|
| `string` | `"Fotografia urbana"` | nome e mensagens |
| `number` | `30` | capacidade e quantidades |
| `boolean` | `true` | resultado de uma verificação |
| `undefined` | `undefined` | valor ainda não atribuído |
| `null` | `null` | ausência indicada intencionalmente |

```js
const nomeOficina = "Fotografia urbana";
const capacidade = 30;
const inscricoesAbertas = true;
let nomeInstrutor;
const sala = null;

console.log(typeof nomeOficina);
console.log(typeof capacidade);
console.log(typeof inscricoesAbertas);
console.log(typeof nomeInstrutor);
console.log(sala);
```

`typeof` produz uma string com o tipo observado. Existe uma particularidade histórica: `typeof null` retorna `"object"`. Para verificar ausência intencional, compare o valor diretamente com `null`.

JavaScript possui tipagem dinâmica: o tipo pertence ao valor e é verificado durante a execução. Isso não elimina a responsabilidade de manter cada variável coerente.

## 7. Operadores aritméticos

Calcule quantas vagas existem:

```js
const capacidade = 30;
const quantidadeInscritos = 23;
const vagasDisponiveis = capacidade - quantidadeInscritos;

console.log(vagasDisponiveis);
```

Operadores frequentes:

| Operador | Operação | Exemplo |
|---|---|---|
| `+` | soma | `23 + 2` |
| `-` | subtração | `30 - 23` |
| `*` | multiplicação | `3 * 10` |
| `/` | divisão | `30 / 2` |
| `%` | resto | `23 % 2` |
| `**` | exponenciação | `2 ** 3` |

Parênteses deixam a precedência explícita:

```js
const percentualOcupacao = (quantidadeInscritos / capacidade) * 100;
console.log(percentualOcupacao);
```

Evite arredondar antes de concluir um cálculo. A formatação do resultado é uma decisão diferente do processamento.

## 8. Comparações e valores booleanos

Comparações produzem `true` ou `false`:

```js
const capacidade = 30;
const quantidadeInscritos = 23;
const novasInscricoes = 4;
const totalPrevisto = quantidadeInscritos + novasInscricoes;

const aindaHaVagas = totalPrevisto < capacidade;
const atingiuCapacidade = totalPrevisto === capacidade;
const excedeuCapacidade = totalPrevisto > capacidade;

console.log(aindaHaVagas);
console.log(atingiuCapacidade);
console.log(excedeuCapacidade);
```

| Operador | Pergunta |
|---|---|
| `===` | os valores e tipos são iguais? |
| `!==` | são diferentes? |
| `>` / `<` | um número é maior ou menor? |
| `>=` / `<=` | inclui o limite? |

Prefira igualdade estrita, `===`, porque ela não converte os valores silenciosamente. As decisões tomadas a partir desses booleanos serão estudadas no Encontro 9.

## 9. Operadores lógicos

Operadores lógicos combinam ou invertem condições:

```js
const inscricoesAbertas = true;
const idadeMinimaAtendida = true;
const possuiImpedimento = false;

const podeSolicitarInscricao =
  inscricoesAbertas && idadeMinimaAtendida && !possuiImpedimento;

console.log(podeSolicitarInscricao);
```

- `&&`: todas as condições precisam ser verdadeiras;
- `||`: pelo menos uma condição precisa ser verdadeira;
- `!`: inverte um booleano.

Leia a expressão como uma frase antes de executá-la. Em seguida, altere um valor por vez e preveja o resultado.

## 10. Strings, concatenação e template strings

O operador `+` soma numbers, mas concatena strings:

```js
console.log("Vagas: " + 7);
```

Para mensagens com vários valores, prefira template strings, delimitadas por crases:

```js
const nomeOficina = "Fotografia urbana";
const vagasDisponiveis = 7;

const mensagem =
  `A oficina ${nomeOficina} possui ${vagasDisponiveis} vagas disponíveis.`;

console.log(mensagem);
```

`${...}` aceita uma expressão. A mensagem separa o cálculo de sua apresentação, pois utiliza um valor já processado.

## 11. Conversão explícita e `NaN`

Dados vindos de campos HTML geralmente chegam como strings. Simule essa entrada:

```js
const novasInscricoesTexto = "4";

console.log(quantidadeInscritos + novasInscricoesTexto);
```

O resultado é `"234"`, porque a presença de uma string faz `+` concatenar. Converta antes do cálculo:

```js
const novasInscricoes = Number(novasInscricoesTexto);
const totalPrevisto = quantidadeInscritos + novasInscricoes;

console.log(totalPrevisto);
```

Uma string que não representa número produz `NaN`:

```js
const valorInvalido = Number("quatro");

console.log(valorInvalido);
console.log(Number.isNaN(valorInvalido));
```

`NaN` significa “Not a Number”, mas seu `typeof` é `"number"`. Use `Number.isNaN` para verificar esse resultado. O tratamento de uma entrada inválida com estruturas condicionais será feito no próximo encontro.

## 12. Exemplo principal completo

Reúna o que foi estudado em `js/app.js`:

```js
const nomeOficina = "Fotografia urbana";
const capacidade = 30;
const quantidadeInscritos = 23;
const novasInscricoesTexto = "4";
const inscricoesAbertas = true;

const novasInscricoes = Number(novasInscricoesTexto);
const totalPrevisto = quantidadeInscritos + novasInscricoes;
const vagasAposConfirmacoes = capacidade - totalPrevisto;
const percentualOcupacao = (totalPrevisto / capacidade) * 100;

const quantidadeValida = !Number.isNaN(novasInscricoes);
const dentroDaCapacidade = totalPrevisto <= capacidade;
const podeConfirmar = inscricoesAbertas && quantidadeValida && dentroDaCapacidade;

const resumo =
  `${nomeOficina}: ${totalPrevisto} de ${capacidade} vagas ocupadas.`;

console.log("Diagnóstico de inscrições");
console.log(resumo);
console.log(`Vagas restantes: ${vagasAposConfirmacoes}`);
console.log(`Ocupação prevista: ${percentualOcupacao}%`);
console.log(`Novas inscrições podem ser confirmadas: ${podeConfirmar}`);
```

### Rastrear entrada, processamento e saída

| Parte | Variáveis ou instruções |
|---|---|
| entrada | `nomeOficina`, `capacidade`, `quantidadeInscritos`, `novasInscricoesTexto`, `inscricoesAbertas` |
| conversão | `Number(novasInscricoesTexto)` |
| processamento | total, vagas, percentual e verificações booleanas |
| apresentação | template strings |
| saída | chamadas de `console.log` |

O programa ainda não decide qual mensagem mostrar. Ele calcula condições e as exibe. No Encontro 9, `if`, `else if` e `else` transformarão esses booleanos em caminhos diferentes.

## 13. Inspeção e diagnóstico

### Executar no DevTools

1. abra a aba **Sources**;
2. localize `js/app.js`;
3. adicione um breakpoint na linha de `totalPrevisto`;
4. recarregue a página;
5. avance uma instrução por vez;
6. observe quando cada variável passa a existir;
7. confira os valores antes de continuar.

### Provocar e interpretar erros

Teste um erro de cada vez e reverta antes de continuar.

#### Identificador inexistente

```js
console.log(vagasdisponiveis);
```

`ReferenceError` indica que o identificador não foi encontrado. Compare maiúsculas, minúsculas e a ordem da declaração.

#### Reatribuição de constante

```js
const capacidade = 30;
capacidade = 40;
```

`TypeError` ocorre porque uma constante não pode receber nova atribuição.

#### Operação com texto

```js
const total = 23 + "4";
```

Não há erro lançado: o resultado está logicamente incorreto. Por isso, ausência de mensagem vermelha não comprova correção.

#### Erro de sintaxe

```js
const nomeOficina = "Fotografia urbana;
```

`SyntaxError` impede que o arquivo seja interpretado. O console indica arquivo e linha próximos ao problema.

## 14. Prática guiada

Crie um diagnóstico para empréstimos de uma biblioteca:

```text
Título: Introdução ao JavaScript
Exemplares: 12
Emprestados: 8
Novas solicitações recebidas como texto: "3"
Biblioteca aberta: true
```

Calcule e exiba:

- quantidade prevista de exemplares emprestados;
- quantidade que permanecerá disponível;
- percentual previsto de empréstimo;
- booleano que informa se existem exemplares suficientes;
- booleano que combina biblioteca aberta, quantidade válida e disponibilidade;
- resumo construído com template string.

### Etapas

1. declare as entradas com nomes claros;
2. confira seus tipos;
3. converta a entrada textual;
4. realize os cálculos;
5. produza os booleanos;
6. construa as mensagens;
7. exiba as saídas;
8. teste `"5"`, `"cinco"` e `"0"`;
9. registre o que mudou em cada cenário.

## 15. Exercício aplicado

Sem copiar o exemplo linha por linha, crie um programa para diagnosticar a ocupação de um evento, estoque de um produto ou agendamento de um serviço.

### Requisitos mínimos

- pelo menos cinco entradas com tipos coerentes;
- uma entrada numérica representada inicialmente como string;
- conversão explícita com `Number`;
- três operações aritméticas;
- três comparações;
- uma expressão com operadores lógicos;
- duas mensagens com template strings;
- saídas identificadas no console;
- nomes em `camelCase`;
- teste de um valor inválido com `Number.isNaN`;
- README com instruções de execução e três cenários testados.

Não use `if`, loops, arrays, objetos, DOM ou eventos. Esses recursos serão apresentados nos encontros seguintes.

## 16. Critérios de aceite

- o projeto abre em `http://localhost:8080`;
- o arquivo JavaScript está conectado como módulo externo;
- o console não apresenta erros não explicados;
- `const` e `let` são utilizados conforme a necessidade de reatribuição;
- os tipos das entradas são identificáveis;
- conversões ocorrem antes dos cálculos;
- operadores produzem os resultados solicitados;
- mensagens usam valores processados;
- entrada, processamento e saída estão separados;
- os cenários testados estão documentados.

## 17. Erros comuns

- escrever JavaScript dentro do HTML sem necessidade;
- usar nome de variável que não explica o dado;
- reatribuir uma constante;
- confundir `=` com `===`;
- somar string e number esperando uma soma aritmética;
- confiar apenas em `typeof` para detectar `NaN`;
- usar `var` em vez de `const` ou `let`;
- alterar várias linhas antes de observar o resultado;
- considerar o programa correto apenas porque não apareceu erro;
- copiar uma solução sem prever suas saídas.

## Materiais para aprofundamento

- [MDN — JavaScript](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript)
- [MDN — Gramática e tipos](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Grammar_and_types)
- [MDN — Expressões e operadores](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Expressions_and_operators)
- [MDN — Console](https://developer.mozilla.org/pt-BR/docs/Web/API/console)

## Checklist de compreensão

- [ ] Consigo explicar como o arquivo JavaScript chega ao navegador.
- [ ] Consigo verificar a conexão pelo console.
- [ ] Consigo diferenciar instrução, expressão e valor.
- [ ] Consigo escolher entre `const` e `let`.
- [ ] Consigo reconhecer os tipos primitivos apresentados.
- [ ] Consigo verificar um tipo com `typeof`.
- [ ] Consigo converter uma string antes do cálculo.
- [ ] Consigo usar operadores aritméticos, comparativos e lógicos.
- [ ] Consigo construir uma mensagem com template string.
- [ ] Consigo identificar `NaN`.
- [ ] Consigo localizar arquivo e linha de um erro.
- [ ] Consigo separar entrada, processamento e saída.

## Resumo final

JavaScript adiciona processamento e comportamento às interfaces. Neste encontro, um módulo externo foi carregado pelo navegador e observado com DevTools. Dados foram declarados com `const` e `let`, classificados por tipo, convertidos e combinados por operadores. Template strings transformaram resultados em mensagens compreensíveis.

Esses fundamentos serão usados no Encontro 9 para criar caminhos de execução. As comparações que hoje apenas produzem `true` ou `false` passarão a controlar decisões com estruturas condicionais.

## Questões de fixação

1. Qual é a diferença entre uma expressão e uma instrução?
<!-- Gabarito: expressão produz um valor; instrução orienta uma ação do programa. -->

2. Quando utilizar `let` em vez de `const`?
<!-- Gabarito: quando o identificador precisar receber outra atribuição durante a execução. -->

3. Por que `23 + "4"` produz `"234"`?
<!-- Gabarito: a presença da string faz o operador + realizar concatenação. -->

4. Por que converter uma entrada antes de realizar o cálculo?
<!-- Gabarito: entradas textuais precisam virar numbers explicitamente para evitar concatenação ou resultados inválidos. -->

5. O que uma comparação como `total <= capacidade` produz?
<!-- Gabarito: um valor booleano, true ou false. -->

6. Qual é a função de `Number.isNaN`?
<!-- Gabarito: verificar se um valor é especificamente NaN após uma operação ou conversão inválida. -->

7. Por que ausência de erro no console não garante que o programa está correto?
<!-- Gabarito: erros lógicos, como concatenação acidental, podem produzir valores sem lançar exceção. -->

8. Como comprovar que `app.js` foi carregado?
<!-- Gabarito: observar uma saída conhecida no console e localizar a requisição/arquivo no DevTools. -->

[Voltar ao cronograma](../01-cronograma-60h.md)
