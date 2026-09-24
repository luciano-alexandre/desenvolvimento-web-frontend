# Encontro 13 — Correção da Atividade Prática 2

**Unidade:** Unidade 1
**Carga horária:** 1,5h  
**Modalidade:** correção comentada e prática guiada
**Atividade de referência:** [Encontro 12 — Atividade Prática 2](encontro-12.md)
**Entrega prevista:** solução revisada, testada e compreendida

## Visão geral

Neste encontro, a turma corrigirá passo a passo a atividade sobre o acervo de uma biblioteca comunitária. A intenção não é apenas apresentar um código pronto: cada decisão será relacionada ao enunciado, aos conteúdos dos encontros 8 a 11 e aos critérios de avaliação.

A correção preserva o escopo da atividade. DOM, eventos, formulários, armazenamento e requisições assíncronas ainda não serão utilizados; esses assuntos serão estudados no encontro 14.

## Objetivos

Ao final do encontro, espera-se que o estudante consiga:

- conferir se uma solução atende integralmente ao enunciado;
- explicar conversões, validações e estruturas condicionais;
- criar funções pequenas com parâmetros e retorno;
- escolher entre `forEach`, `filter`, `map`, `find`, `some`, `every` e `reduce`;
- tratar valores inexistentes antes de acessar propriedades;
- atualizar arrays e objetos sem modificar os dados originais;
- testar caminhos diferentes de uma regra de negócio;
- usar o Console para comparar resultados esperados e observados.

## 1. Metodologia da correção

Para cada etapa, seguiremos quatro movimentos:

1. reler o requisito;
2. identificar entradas, processamento e saída;
3. implementar a menor parte possível;
4. executar e conferir o resultado antes de continuar.

Antes de substituir seu código, marque nele:

- o que estava correto;
- o que produzia resultado incorreto;
- o que estava ausente;
- o que funcionava, mas poderia ser mais claro.

> Uma solução diferente do exemplo também pode estar correta se cumprir os requisitos e se suas decisões puderem ser explicadas.

## 2. Preparação do projeto

Utilize a mesma estrutura solicitada no encontro 12:

```text
atividade-pratica-02/
├── compose.yaml
├── index.html
├── README.md
└── js/
    └── app.js
```

Suba o servidor:

```bash
docker compose up
```

Acesse `http://localhost:8080`, abra as ferramentas do desenvolvedor e selecione a aba **Console**. Confirme também se o HTML carrega o arquivo correto:

```html
<script type="module" src="./js/app.js"></script>
```

Durante a correção, salve o arquivo e recarregue a página após cada etapa.

## 3. Etapa 1 — Representar os dados obrigatórios

Cada livro é um objeto. O acervo é um array que reúne esses objetos:

```js
const livros = [
  {
    id: 1,
    titulo: "JavaScript para iniciantes",
    categoria: "Tecnologia",
    total: 8,
    emprestados: 5,
    ativo: true,
  },
  {
    id: 2,
    titulo: "Histórias do sertão",
    categoria: "Literatura",
    total: 5,
    emprestados: 5,
    ativo: true,
  },
  {
    id: 3,
    titulo: "Design para todos",
    categoria: "Design",
    total: 6,
    emprestados: 2,
    ativo: false,
  },
  {
    id: 4,
    titulo: "Ciência no cotidiano",
    categoria: "Ciências",
    total: 10,
    emprestados: 4,
    ativo: true,
  },
  {
    id: 5,
    titulo: "Memórias da cidade",
    categoria: "História",
    total: 4,
    emprestados: 1,
    ativo: true,
  },
];

const livroSelecionadoId = 1;
const novasSolicitacoesTexto = "2";
const bibliotecaAberta = true;
```

### O que conferir

- números e booleanos não devem estar entre aspas;
- os nomes das propriedades devem ser iguais em todos os objetos;
- os dados não devem ser alterados para forçar um resultado;
- `novasSolicitacoesTexto` é uma string de propósito, pois simula uma entrada textual.

## 4. Etapa 2 — Converter e validar a quantidade

A conversão deve acontecer antes da validação:

```js
const novasSolicitacoes = Number(novasSolicitacoesTexto);

function quantidadeEhValida(quantidade) {
  return (
    Number.isFinite(quantidade) &&
    Number.isInteger(quantidade) &&
    quantidade > 0
  );
}
```

Cada verificação resolve um problema:

- `Number.isFinite` rejeita `NaN` e valores infinitos;
- `Number.isInteger` rejeita valores como `2.5`;
- `quantidade > 0` rejeita zero e números negativos.

Teste a função isoladamente:

```js
console.log(quantidadeEhValida(Number("2")));    // true
console.log(quantidadeEhValida(Number("0")));    // false
console.log(quantidadeEhValida(Number("-1")));   // false
console.log(quantidadeEhValida(Number("2.5")));  // false
console.log(quantidadeEhValida(Number("duas"))); // false
console.log(quantidadeEhValida(Number("")));     // false
```

A string vazia se transforma em zero. Por isso, verificar apenas se o resultado é um número não seria suficiente.

## 5. Etapa 3 — Criar as funções básicas

### 5.1 Calcular os exemplares disponíveis

A disponibilidade é a diferença entre o total e a quantidade emprestada:

```js
function calcularDisponiveis(livro) {
  return livro.total - livro.emprestados;
}
```

### 5.2 Verificar se há exemplares

A função devolve um booleano:

```js
function possuiExemplares(livro) {
  return calcularDisponiveis(livro) > 0;
}
```

Reutilizar `calcularDisponiveis` evita repetir a fórmula.

### 5.3 Verificar se uma reserva é permitida

```js
function podeReservar(livro, quantidade, aberta) {
  return (
    aberta &&
    livro !== undefined &&
    livro.ativo &&
    quantidadeEhValida(quantidade) &&
    quantidade <= calcularDisponiveis(livro)
  );
}
```

A expressão só será verdadeira quando todas as condições forem verdadeiras. A verificação de existência aparece antes de `livro.ativo`; assim, o JavaScript interrompe a expressão caso `livro` seja `undefined`.

### 5.4 Criar um resumo

```js
function criarResumo(livro) {
  const disponiveis = calcularDisponiveis(livro);
  let situacao;

  if (!livro.ativo) {
    situacao = "inativo";
  } else if (disponiveis === 0) {
    situacao = "esgotado";
  } else {
    situacao = "disponível";
  }

  return {
    id: livro.id,
    titulo: livro.titulo,
    categoria: livro.categoria,
    disponiveis,
    situacao,
  };
}
```

A ordem importa: o livro de id 3 possui exemplares físicos disponíveis, mas deve ser classificado como inativo.

## 6. Etapa 4 — Localizar o livro com segurança

Use `find` porque precisamos de um único objeto:

```js
const livroSelecionado = livros.find(
  (livro) => livro.id === livroSelecionadoId,
);
```

Quando nenhum item atende à condição, `find` retorna `undefined`. Este código seria inseguro:

```js
// Evite: pode causar erro se o livro não existir.
// console.log(livroSelecionado.titulo);
```

Primeiro confirme a existência do objeto; somente depois acesse suas propriedades.

## 7. Etapa 5 — Construir o diagnóstico da reserva

Uma função com retornos antecipados deixa cada regra explícita e garante apenas um resultado:

```js
function diagnosticarReserva(livro, quantidade, aberta) {
  if (!aberta) {
    return "Reserva indisponível: a biblioteca está fechada.";
  }

  if (!quantidadeEhValida(quantidade)) {
    return "Reserva recusada: informe uma quantidade inteira maior que zero.";
  }

  if (livro === undefined) {
    return "Reserva recusada: livro não encontrado.";
  }

  if (!livro.ativo) {
    return "Reserva recusada: o livro está inativo.";
  }

  const disponiveis = calcularDisponiveis(livro);

  if (quantidade > disponiveis) {
    return `Reserva recusada: existem apenas ${disponiveis} exemplar(es) disponível(is).`;
  }

  if (quantidade === disponiveis) {
    return "Reserva confirmada: esta solicitação utiliza o último exemplar.";
  }

  return `Reserva confirmada: restará(ão) ${disponiveis - quantidade} exemplar(es).`;
}
```

Observe a sequência:

1. regras que não dependem do livro são verificadas primeiro;
2. a existência é confirmada antes do acesso às propriedades;
3. disponibilidade só é calculada para um objeto existente;
4. o caso do último exemplar é separado do sucesso comum.

Agora obtenha a mensagem:

```js
const diagnostico = diagnosticarReserva(
  livroSelecionado,
  novasSolicitacoes,
  bibliotecaAberta,
);
```

## 8. Etapa 6 — Percorrer e transformar o acervo

### 8.1 Exibir todos os livros com `forEach`

```js
console.log("=== ACERVO ORIGINAL ===");

livros.forEach((livro) => {
  console.log(
    `${livro.titulo}: ${calcularDisponiveis(livro)} disponível(is)`,
  );
});
```

`forEach` executa uma ação para cada item. Ele não é utilizado para criar um novo array.

### 8.2 Filtrar livros disponíveis

O requisito considera livros ativos e com exemplares:

```js
const livrosDisponiveis = livros.filter(
  (livro) => livro.ativo && possuiExemplares(livro),
);
```

Resultado esperado: livros de ids **1, 4 e 5**.

### 8.3 Criar resumos

```js
const resumos = livros.map(criarResumo);
```

`map` devolve um novo array com a mesma quantidade de itens, mas com outra representação.

### 8.4 Tratar buscas existentes e inexistentes

```js
const livroId99 = livros.find((livro) => livro.id === 99);

if (livroId99 === undefined) {
  console.log("Busca pelo id 99: livro não encontrado.");
} else {
  console.log("Busca pelo id 99:", livroId99);
}
```

### 8.5 Verificar condições com `some` e `every`

```js
const existeLivroEsgotado = livros.some(
  (livro) => calcularDisponiveis(livro) === 0,
);

const todosPossuemTitulo = livros.every(
  (livro) => livro.titulo.trim().length > 0,
);
```

- `some` responde se ao menos um item atende à condição;
- `every` responde se todos atendem à condição.

Para os dados fornecidos, ambos os resultados são `true`.

## 9. Etapa 7 — Calcular os indicadores com `reduce`

O acumulador começa com todas as propriedades zeradas:

```js
const indicadores = livros.reduce(
  (acumulador, livro) => {
    acumulador.total += livro.total;
    acumulador.emprestados += livro.emprestados;
    acumulador.disponiveis += calcularDisponiveis(livro);
    return acumulador;
  },
  {
    total: 0,
    emprestados: 0,
    disponiveis: 0,
  },
);
```

Resultados esperados:

| Indicador | Resultado |
|---|---:|
| Total de exemplares | 33 |
| Emprestados | 17 |
| Disponíveis | 16 |

O valor inicial é obrigatório. Sem ele, o primeiro livro seria usado como acumulador e poderia ser modificado acidentalmente.

## 10. Etapa 8 — Atualizar sem mutar o acervo

Comece mantendo o acervo original. Só crie a versão atualizada quando a reserva for permitida:

```js
let acervoAtualizado = livros;

if (
  podeReservar(
    livroSelecionado,
    novasSolicitacoes,
    bibliotecaAberta,
  )
) {
  acervoAtualizado = livros.map((livro) => {
    if (livro.id !== livroSelecionadoId) {
      return livro;
    }

    return {
      ...livro,
      emprestados: livro.emprestados + novasSolicitacoes,
    };
  });
}
```

Há dois níveis de cópia:

- `map` cria um novo array;
- o operador spread cria um novo objeto para o livro alterado.

Confira a preservação do original:

```js
console.log(livros[0].emprestados);           // 5
console.log(acervoAtualizado[0].emprestados); // 7
console.log(livros === acervoAtualizado);     // false no cenário válido
```

## 11. Etapa 9 — Organizar as saídas obrigatórias

Depois de calcular todos os valores, apresente os resultados com rótulos claros:

```js
console.log("=== ACERVO ORIGINAL ===");
console.table(livros);

console.log("=== LIVROS DISPONÍVEIS ===");
console.table(livrosDisponiveis);

console.log("=== RESUMOS ===");
console.table(resumos);

console.log("=== INDICADORES ===");
console.table(indicadores);
console.log("Existe livro esgotado:", existeLivroEsgotado);
console.log("Todos possuem título:", todosPossuemTitulo);
console.log(
  "Busca pelo id 99:",
  livroId99 ?? "Livro não encontrado",
);

console.log("=== DIAGNÓSTICO DA RESERVA ===");
console.log(diagnostico);

console.log("=== ACERVO ATUALIZADO ===");
console.table(acervoAtualizado);
```

Separar cálculo e apresentação torna o programa mais fácil de ler, testar e adaptar.

## 12. Solução completa do `app.js`

Compare esta solução com a sua apenas depois de corrigir cada etapa:

```js
const livros = [
  { id: 1, titulo: "JavaScript para iniciantes", categoria: "Tecnologia", total: 8, emprestados: 5, ativo: true },
  { id: 2, titulo: "Histórias do sertão", categoria: "Literatura", total: 5, emprestados: 5, ativo: true },
  { id: 3, titulo: "Design para todos", categoria: "Design", total: 6, emprestados: 2, ativo: false },
  { id: 4, titulo: "Ciência no cotidiano", categoria: "Ciências", total: 10, emprestados: 4, ativo: true },
  { id: 5, titulo: "Memórias da cidade", categoria: "História", total: 4, emprestados: 1, ativo: true },
];

const livroSelecionadoId = 1;
const novasSolicitacoesTexto = "2";
const bibliotecaAberta = true;
const novasSolicitacoes = Number(novasSolicitacoesTexto);

function quantidadeEhValida(quantidade) {
  return Number.isFinite(quantidade)
    && Number.isInteger(quantidade)
    && quantidade > 0;
}

function calcularDisponiveis(livro) {
  return livro.total - livro.emprestados;
}

function possuiExemplares(livro) {
  return calcularDisponiveis(livro) > 0;
}

function podeReservar(livro, quantidade, aberta) {
  return aberta
    && livro !== undefined
    && livro.ativo
    && quantidadeEhValida(quantidade)
    && quantidade <= calcularDisponiveis(livro);
}

function criarResumo(livro) {
  const disponiveis = calcularDisponiveis(livro);
  let situacao;

  if (!livro.ativo) {
    situacao = "inativo";
  } else if (disponiveis === 0) {
    situacao = "esgotado";
  } else {
    situacao = "disponível";
  }

  return {
    id: livro.id,
    titulo: livro.titulo,
    categoria: livro.categoria,
    disponiveis,
    situacao,
  };
}

function diagnosticarReserva(livro, quantidade, aberta) {
  if (!aberta) {
    return "Reserva indisponível: a biblioteca está fechada.";
  }

  if (!quantidadeEhValida(quantidade)) {
    return "Reserva recusada: informe uma quantidade inteira maior que zero.";
  }

  if (livro === undefined) {
    return "Reserva recusada: livro não encontrado.";
  }

  if (!livro.ativo) {
    return "Reserva recusada: o livro está inativo.";
  }

  const disponiveis = calcularDisponiveis(livro);

  if (quantidade > disponiveis) {
    return `Reserva recusada: existem apenas ${disponiveis} exemplar(es) disponível(is).`;
  }

  if (quantidade === disponiveis) {
    return "Reserva confirmada: esta solicitação utiliza o último exemplar.";
  }

  return `Reserva confirmada: restará(ão) ${disponiveis - quantidade} exemplar(es).`;
}

const livroSelecionado = livros.find(
  (livro) => livro.id === livroSelecionadoId,
);
const livroId99 = livros.find((livro) => livro.id === 99);

const livrosDisponiveis = livros.filter(
  (livro) => livro.ativo && possuiExemplares(livro),
);

const resumos = livros.map(criarResumo);

const existeLivroEsgotado = livros.some(
  (livro) => calcularDisponiveis(livro) === 0,
);

const todosPossuemTitulo = livros.every(
  (livro) => livro.titulo.trim().length > 0,
);

const indicadores = livros.reduce(
  (acumulador, livro) => {
    acumulador.total += livro.total;
    acumulador.emprestados += livro.emprestados;
    acumulador.disponiveis += calcularDisponiveis(livro);
    return acumulador;
  },
  { total: 0, emprestados: 0, disponiveis: 0 },
);

const diagnostico = diagnosticarReserva(
  livroSelecionado,
  novasSolicitacoes,
  bibliotecaAberta,
);

let acervoAtualizado = livros;

if (
  podeReservar(
    livroSelecionado,
    novasSolicitacoes,
    bibliotecaAberta,
  )
) {
  acervoAtualizado = livros.map((livro) => {
    if (livro.id !== livroSelecionadoId) {
      return livro;
    }

    return {
      ...livro,
      emprestados: livro.emprestados + novasSolicitacoes,
    };
  });
}

console.log("=== ACERVO ORIGINAL ===");
console.table(livros);

console.log("=== LIVROS DISPONÍVEIS ===");
console.table(livrosDisponiveis);

console.log("=== RESUMOS ===");
console.table(resumos);

console.log("=== INDICADORES ===");
console.table(indicadores);
console.log("Existe livro esgotado:", existeLivroEsgotado);
console.log("Todos possuem título:", todosPossuemTitulo);
console.log(
  "Busca pelo id 99:",
  livroId99 ?? "Livro não encontrado",
);

console.log("=== DIAGNÓSTICO DA RESERVA ===");
console.log(diagnostico);

console.log("=== ACERVO ATUALIZADO ===");
console.table(acervoAtualizado);
```

## 13. Testar todos os caminhos

Altere temporariamente apenas as três entradas da reserva, execute cada cenário e registre o resultado no README:

| Cenário | id | quantidade | aberta | Resultado esperado |
|---|---:|---:|---|---|
| sucesso | 1 | `"2"` | `true` | confirma e resta 1 |
| último exemplar | 1 | `"3"` | `true` | confirma e utiliza o último |
| excedente | 1 | `"4"` | `true` | recusa: apenas 3 disponíveis |
| esgotado | 2 | `"1"` | `true` | recusa: 0 disponível |
| inativo | 3 | `"1"` | `true` | recusa: livro inativo |
| inexistente | 99 | `"1"` | `true` | recusa: não encontrado |
| inválido | 1 | `"duas"` | `true` | recusa: quantidade inválida |
| fechada | 1 | `"1"` | `false` | informa biblioteca fechada |

Depois de cada teste, responda:

- a mensagem corresponde ao cenário?
- o acervo só é atualizado quando a reserva é válida?
- o acervo original permanece com os mesmos valores?
- algum erro aparece no Console?

Ao terminar, restaure os valores originais do enunciado.

## 14. Erros frequentes e como corrigi-los

### Usar a string sem conversão

```js
// Problema: a entrada continua textual.
const quantidade = novasSolicitacoesTexto;

// Correção:
const quantidade = Number(novasSolicitacoesTexto);
```

### Aceitar qualquer número

```js
// Problema: aceita negativos e decimais.
return !Number.isNaN(quantidade);

// Correção:
return Number.isFinite(quantidade)
  && Number.isInteger(quantidade)
  && quantidade > 0;
```

### Acessar um resultado inexistente

```js
// Problema:
const titulo = livros.find((livro) => livro.id === 99).titulo;

// Correção:
const encontrado = livros.find((livro) => livro.id === 99);
const titulo = encontrado?.titulo ?? "Livro não encontrado";
```

### Usar o método de coleção inadequado

- use `find` para obter um item;
- use `filter` para obter vários itens selecionados;
- use `map` para transformar todos os itens;
- use `some` e `every` para obter booleanos;
- use `reduce` para acumular valores.

### Modificar diretamente o objeto original

```js
// Problema:
livroSelecionado.emprestados += novasSolicitacoes;

// Correção:
const livroAtualizado = {
  ...livroSelecionado,
  emprestados: livroSelecionado.emprestados + novasSolicitacoes,
};
```

## 15. Revisão orientada pela rubrica

Use a distribuição de 20 pontos do encontro 12 para revisar a entrega. Para cada critério:

1. localize no código a evidência do requisito;
2. execute um teste que demonstre seu funcionamento;
3. explique a decisão com suas palavras;
4. anote o ajuste necessário, caso exista.

Não atribua pontos apenas porque determinada palavra aparece no código. O requisito deve funcionar nos cenários previstos.

## 16. Desafio de refatoração

Após concluir a correção obrigatória, escolha uma melhoria:

- criar uma função para gerar o objeto atualizado;
- evitar mutação também dentro do acumulador do `reduce`;
- centralizar os cenários de teste em um array;
- exibir uma tabela comparando valores originais e atualizados.

Exemplo de `reduce` com novo acumulador a cada volta:

```js
const indicadoresSemMutacao = livros.reduce(
  (acumulador, livro) => ({
    total: acumulador.total + livro.total,
    emprestados: acumulador.emprestados + livro.emprestados,
    disponiveis:
      acumulador.disponiveis + calcularDisponiveis(livro),
  }),
  { total: 0, emprestados: 0, disponiveis: 0 },
);
```

A melhoria não substitui os requisitos obrigatórios; ela serve para comparar estratégias.

## Checklist final

- [ ] Mantive os dados obrigatórios do enunciado.
- [ ] Converti a entrada textual com `Number`.
- [ ] Rejeitei valores não finitos, decimais, zero e negativos.
- [ ] Implementei as quatro funções solicitadas.
- [ ] Tratei o resultado de `find` antes de acessar propriedades.
- [ ] Implementei os sete caminhos do diagnóstico.
- [ ] Usei corretamente os métodos de coleção pedidos.
- [ ] Informei valor inicial no `reduce`.
- [ ] Atualizei o acervo com `map` e spread.
- [ ] Preservei o array e o objeto originais.
- [ ] Organizei todas as saídas obrigatórias.
- [ ] Executei e registrei os oito cenários de teste.
- [ ] Consigo explicar cada parte da solução.

## Questões de fixação

1. Por que `Number.isInteger` é necessário mesmo depois de usar `Number`?
2. Por que a existência do livro deve ser verificada antes de `livro.ativo`?
3. Qual é a diferença entre os resultados de `find` e `filter`?
4. Por que `map` é adequado para criar o acervo atualizado?
5. O que o spread preserva no novo objeto?
6. Por que o `reduce` precisa de um valor inicial explícito?
7. Em qual cenário a reserva é válida, mas exige uma mensagem especial?
8. Como provar pelo Console que o objeto original não foi modificado?

## Encerramento

A correção está concluída quando o código atende ao enunciado, produz os resultados previstos e pode ser explicado pelo estudante. No encontro 14, os conhecimentos de JavaScript serão levados à página com DOM, eventos e formulários e serão integrados a módulos, armazenamento, assincronismo e Fetch API.
