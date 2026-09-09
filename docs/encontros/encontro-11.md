# Encontro 11 — JavaScript: arrays, objetos e métodos de coleção

**Unidade:** Unidade 1
**Carga horária:** 1,5h
**Entrega prevista:** catálogo de oficinas estruturado, consultado e resumido

## Visão geral

Nos encontros 9 e 10, o programa tomou decisões, repetiu tarefas e organizou cálculos em funções. Os dados, porém, continuaram espalhados em variáveis como `nomeOficina`, `capacidade` e `quantidadeInscritos`. Essa estratégia fica frágil quando existem várias oficinas.

Neste encontro, arrays representarão coleções ordenadas e objetos agruparão propriedades de uma mesma entidade. Métodos como `forEach`, `map`, `filter`, `find`, `some`, `every` e `reduce` expressarão diferentes intenções de processamento.

O resultado continuará no console. A renderização desses dados no HTML será tratada no Encontro 12.

## Objetivos de aprendizagem

- criar, acessar e atualizar arrays;
- interpretar índice e propriedade `length`;
- agrupar dados relacionados em objetos;
- acessar propriedades com ponto e colchetes;
- percorrer coleções com `for...of` e `forEach`;
- transformar arrays com `map`;
- selecionar itens com `filter`;
- localizar um item com `find`;
- verificar condições com `some` e `every`;
- acumular resultados com `reduce`;
- evitar mutações acidentais;
- decompor callbacks em funções nomeadas;
- escolher um método conforme o resultado necessário.

## Conceitos essenciais

- coleção, elemento e índice;
- array e `length`;
- objeto, propriedade e método;
- referência e mutação;
- iteração;
- callback;
- transformação, filtro e busca;
- predicado;
- acumulação.

## 1. Retomar o projeto

Continue em `encontro-08-javascript`:

```bash
cd encontro-08-javascript
docker compose up
```

Crie `js/encontro-11.js` e atualize o HTML:

```html
<script type="module" src="./js/encontro-11.js"></script>
```

Confirme no console:

```js
console.log("Encontro 11 conectado.");
```

## 2. Por que usar arrays

Variáveis separadas não representam claramente uma coleção:

```js
const oficina1 = "Fotografia urbana";
const oficina2 = "Cerâmica manual";
const oficina3 = "Desenho de observação";
```

Um array reúne os valores em ordem:

```js
const nomesOficinas = [
  "Fotografia urbana",
  "Cerâmica manual",
  "Desenho de observação"
];
```

Cada elemento possui índice iniciado em zero:

```js
console.log(nomesOficinas[0]);
console.log(nomesOficinas[2]);
console.log(nomesOficinas.length);
```

- primeiro elemento: índice `0`;
- último índice: `length - 1`;
- índice inexistente: `undefined`.

```js
const ultimoIndice = nomesOficinas.length - 1;
console.log(nomesOficinas[ultimoIndice]);
```

## 3. Alterar uma coleção

```js
const nomesOficinas = ["Fotografia urbana"];

nomesOficinas.push("Cerâmica manual");
nomesOficinas.push("Desenho de observação");
console.log(nomesOficinas);

const oficinaRemovida = nomesOficinas.pop();
console.log(oficinaRemovida);
```

`const` impede reatribuir o identificador, mas não torna o array imutável:

```js
// Permitido: modifica o array existente
nomesOficinas.push("Gravura");

// Não permitido: tenta reatribuir o identificador
// nomesOficinas = [];
```

Use mutação somente quando ela fizer parte do requisito. Métodos de transformação podem criar uma nova coleção.

## 4. Objetos representam entidades

Uma oficina possui dados relacionados:

```js
const oficina = {
  id: 1,
  titulo: "Fotografia urbana",
  capacidade: 30,
  inscritos: 23,
  ativa: true
};
```

Acesse propriedades:

```js
console.log(oficina.titulo);
console.log(oficina["capacidade"]);
```

A notação de ponto é mais direta quando o nome é conhecido. Colchetes permitem uma chave armazenada:

```js
const propriedade = "inscritos";
console.log(oficina[propriedade]);
```

Atualize uma propriedade:

```js
oficina.inscritos += 1;
```

Como nos arrays, `const` preserva a associação ao objeto, mas suas propriedades ainda podem mudar.

## 5. Array de objetos

Modele o catálogo:

```js
const oficinas = [
  {
    id: 1,
    titulo: "Fotografia urbana",
    categoria: "Imagem",
    capacidade: 30,
    inscritos: 23,
    ativa: true
  },
  {
    id: 2,
    titulo: "Cerâmica manual",
    categoria: "Artesanato",
    capacidade: 20,
    inscritos: 20,
    ativa: true
  },
  {
    id: 3,
    titulo: "Desenho de observação",
    categoria: "Imagem",
    capacidade: 25,
    inscritos: 12,
    ativa: false
  }
];
```

O array representa o conjunto; cada objeto representa uma oficina com o mesmo formato conceitual.

## 6. Percorrer com `for...of`

```js
for (const oficina of oficinas) {
  const vagas = oficina.capacidade - oficina.inscritos;
  console.log(`${oficina.titulo}: ${vagas} vagas.`);
}
```

`oficina` recebe um elemento por iteração. Diferentemente do `for` com contador, não é preciso controlar índices quando apenas o valor interessa.

## 7. `forEach`: executar para cada item

```js
oficinas.forEach((oficina, indice) => {
  console.log(`${indice + 1}. ${oficina.titulo}`);
});
```

`forEach` recebe uma função callback, executada para cada elemento. Seu propósito principal é realizar uma ação; ele retorna `undefined`, não um novo array.

Uma função nomeada pode melhorar a leitura:

```js
function exibirOficina(oficina, indice) {
  console.log(`${indice + 1}. ${oficina.titulo}`);
}

oficinas.forEach(exibirOficina);
```

## 8. `map`: transformar todos os itens

Calcule uma nova coleção de resumos:

```js
const resumos = oficinas.map((oficina) => {
  const vagas = oficina.capacidade - oficina.inscritos;

  return {
    id: oficina.id,
    titulo: oficina.titulo,
    vagas
  };
});

console.log(resumos);
```

`map`:

- executa a callback para todos os elementos;
- utiliza o retorno como elemento novo;
- devolve array com o mesmo comprimento;
- não precisa modificar os objetos originais.

## 9. `filter`: selecionar vários itens

```js
const oficinasAtivas = oficinas.filter((oficina) => {
  return oficina.ativa;
});
```

O retorno da callback é um predicado. Elementos com resultado verdadeiro entram no novo array.

Combine regras:

```js
const oficinasComVagas = oficinas.filter((oficina) => {
  const vagas = oficina.capacidade - oficina.inscritos;
  return oficina.ativa && vagas > 0;
});
```

`filter` sempre devolve array, mesmo quando nenhum item corresponde.

## 10. `find`: localizar um item

```js
const oficinaEncontrada = oficinas.find((oficina) => {
  return oficina.id === 2;
});

console.log(oficinaEncontrada);
```

`find` encerra ao encontrar o primeiro item e devolve o objeto. Se não encontrar, devolve `undefined`:

```js
const resultado = oficinas.find((oficina) => oficina.id === 99);

if (resultado === undefined) {
  console.warn("Oficina não encontrada.");
}
```

Use `find` para um item; use `filter` para uma coleção de correspondências.

## 11. `some` e `every`

```js
const existeOficinaLotada = oficinas.some((oficina) => {
  return oficina.inscritos >= oficina.capacidade;
});

const todasPossuemTitulo = oficinas.every((oficina) => {
  return oficina.titulo.length > 0;
});
```

- `some`: pelo menos um item atende?
- `every`: todos atendem?

Ambos retornam booleano e podem encerrar a busca quando o resultado já estiver determinado.

## 12. `reduce`: acumular um resultado

Some as capacidades:

```js
const capacidadeTotal = oficinas.reduce((acumulador, oficina) => {
  return acumulador + oficina.capacidade;
}, 0);
```

Partes:

- `acumulador`: resultado parcial;
- `oficina`: elemento atual;
- `0`: valor inicial;
- retorno: valor usado na próxima iteração.

Calcule a ocupação:

```js
const totalInscritos = oficinas.reduce((total, oficina) => {
  return total + oficina.inscritos;
}, 0);

const percentual =
  capacidadeTotal === 0
    ? 0
    : (totalInscritos / capacidadeTotal) * 100;
```

Não use `reduce` quando `map`, `filter` ou `find` expressar melhor a intenção.

## 13. Arrow functions

Uma arrow function pode tornar callbacks curtas:

```js
const titulos = oficinas.map((oficina) => oficina.titulo);
```

Com chaves, o retorno precisa ser explícito:

```js
const vagas = oficinas.map((oficina) => {
  return oficina.capacidade - oficina.inscritos;
});
```

Para retornar objeto diretamente, use parênteses:

```js
const opcoes = oficinas.map((oficina) => ({
  value: oficina.id,
  label: oficina.titulo
}));
```

Não transforme toda função em arrow function por regra. Priorize nomes e leitura.

## 14. Exemplo principal completo

```js
const oficinas = [
  {
    id: 1,
    titulo: "Fotografia urbana",
    categoria: "Imagem",
    capacidade: 30,
    inscritos: 23,
    ativa: true
  },
  {
    id: 2,
    titulo: "Cerâmica manual",
    categoria: "Artesanato",
    capacidade: 20,
    inscritos: 20,
    ativa: true
  },
  {
    id: 3,
    titulo: "Desenho de observação",
    categoria: "Imagem",
    capacidade: 25,
    inscritos: 12,
    ativa: false
  }
];

function calcularVagas(oficina) {
  return oficina.capacidade - oficina.inscritos;
}

function possuiVagaDisponivel(oficina) {
  return oficina.ativa && calcularVagas(oficina) > 0;
}

const oficinasComVagas = oficinas.filter(possuiVagaDisponivel);

const resumos = oficinasComVagas.map((oficina) => ({
  id: oficina.id,
  titulo: oficina.titulo,
  categoria: oficina.categoria,
  vagas: calcularVagas(oficina)
}));

const totalVagasDisponiveis = resumos.reduce((total, oficina) => {
  return total + oficina.vagas;
}, 0);

const fotografia = oficinas.find((oficina) => {
  return oficina.titulo === "Fotografia urbana";
});

console.log("Oficinas com vagas:");
console.table(resumos);
console.log(`Total de vagas disponíveis: ${totalVagasDisponiveis}`);
console.log("Busca:", fotografia);
console.log(
  "Existe oficina lotada:",
  oficinas.some((oficina) => calcularVagas(oficina) === 0)
);
```

O fluxo pode ser lido como uma sequência:

```text
dados originais
    ↓ filter
oficinas ativas com vagas
    ↓ map
resumos
    ↓ reduce
total de vagas
```

## 15. Mutação e cópia

Esta atribuição não cria objeto independente:

```js
const original = oficinas[0];
const copiaIncorreta = original;
copiaIncorreta.inscritos = 30;
```

As duas variáveis apontam para o mesmo objeto. Uma cópia superficial pode ser criada com spread:

```js
const oficinaAtualizada = {
  ...oficinas[0],
  inscritos: 30
};
```

O objeto original permanece com seu valor anterior. O spread é superficial; estruturas internas continuariam compartilhando referências.

## 16. Inspeção e diagnóstico

Use `console.table(oficinas)` para comparar propriedades. No DevTools:

1. pause dentro de uma callback;
2. observe elemento, índice e array;
3. acompanhe o acumulador de `reduce`;
4. confirme os retornos de `find` existente e inexistente;
5. compare o array original com o resultado de `map` e `filter`.

### Experimentos

- acesse índice igual a `length`;
- retire o `return` de uma callback com chaves;
- procure um `id` inexistente e tente acessar sua propriedade;
- use `filter` esperando um objeto único;
- omita o valor inicial de `reduce` em um array vazio;
- altere uma cópia por referência e observe o original.

## 17. Prática guiada

Crie uma coleção de quatro livros com:

- `id`, `titulo`, `autor`, `categoria`, `total`, `emprestados` e `ativo`.

Implemente:

1. função que calcula exemplares disponíveis;
2. filtro de livros ativos com disponibilidade;
3. transformação em resumos contendo título e disponibilidade;
4. busca por `id`;
5. verificação de algum livro esgotado;
6. verificação de todos possuírem autor;
7. soma dos exemplares disponíveis;
8. saída final com `console.table`.

## 18. Exercício aplicado

Modele produtos, eventos ou serviços.

### Requisitos mínimos

- array com pelo menos cinco objetos;
- objetos com seis propriedades e tipos coerentes;
- uma função reutilizável para valor derivado;
- um `for...of` ou `forEach`;
- um `map`;
- dois filtros com critérios diferentes;
- duas buscas, incluindo uma inexistente;
- um `some` e um `every`;
- dois acumuladores com `reduce`;
- atualização sem modificar o objeto original;
- resultados identificados no console;
- README explicando a escolha de cada método.

## 19. Critérios de aceite

- dados relacionados estão agrupados em objetos;
- a coleção possui formato consistente;
- índices são usados sem ultrapassar os limites;
- cada método corresponde ao tipo de resultado esperado;
- callbacks sempre retornam quando necessário;
- busca inexistente é tratada;
- reduções possuem valor inicial coerente;
- dados originais não são modificados acidentalmente;
- funções possuem responsabilidades claras;
- console e README permitem verificar os resultados.

## 20. Erros comuns

- esperar que o primeiro índice seja 1;
- acessar `array[array.length]`;
- confundir `filter` com `find`;
- usar `forEach` esperando novo array;
- esquecer `return` em callback com chaves;
- modificar objeto compartilhado sem perceber;
- usar `reduce` para qualquer processamento;
- acessar propriedade de `undefined`;
- criar objetos com propriedades inconsistentes;
- misturar processamento da coleção com futura renderização.

## Materiais para aprofundamento

- [MDN — Array](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [MDN — Trabalhando com objetos](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Guide/Working_with_objects)
- [MDN — Métodos iterativos](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Array#iterative_methods)

## Checklist de compreensão

- [ ] Acesso primeiro e último elemento corretamente.
- [ ] Agrupo propriedades em objetos.
- [ ] Explico mutação por referência.
- [ ] Percorro coleção com intenção clara.
- [ ] Diferencio `map`, `filter` e `find`.
- [ ] Uso `some` e `every` para perguntas booleanas.
- [ ] Defino acumulador e valor inicial no `reduce`.
- [ ] Trato uma busca inexistente.
- [ ] Crio cópia superficial com spread.
- [ ] Inspeciono coleções com DevTools.

## Resumo final

Arrays organizam coleções; objetos modelam entidades; funções dão nome às regras aplicadas aos elementos. Métodos de coleção expressam intenções diferentes: percorrer, transformar, selecionar, localizar, verificar ou acumular. No Encontro 12, essas coleções serão usadas para criar e atualizar conteúdo no DOM e responder a eventos.

## Questões de fixação

1. Qual é o índice do último item?
<!-- Gabarito: array.length - 1. -->

2. Qual é a diferença entre `map` e `forEach`?
<!-- Gabarito: map constrói e retorna novo array; forEach executa uma ação e retorna undefined. -->

3. Quando usar `filter` ou `find`?
<!-- Gabarito: filter para várias correspondências em array; find para a primeira, ou undefined. -->

4. O que `some` e `every` retornam?
<!-- Gabarito: booleanos sobre pelo menos um ou todos os elementos. -->

5. Para que serve o valor inicial de `reduce`?
<!-- Gabarito: define o acumulador inicial e torna o comportamento previsível, inclusive em array vazio. -->

6. Por que atribuir um objeto a outra variável não cria cópia?
<!-- Gabarito: ambas passam a referenciar o mesmo objeto. -->

[Voltar ao cronograma](../01-cronograma-60h.md)
