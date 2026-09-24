# Encontro 12 — Atividade Prática 2: fundamentos de JavaScript

**Unidade:** Unidade 1
**Carga horária:** 1,5h
**Modalidade:** individual, com consulta
**Valor:** 20 pontos
**Entrega prevista:** programa de análise de acervo executado no navegador

## Visão geral

Esta atividade avalia os conteúdos dos encontros 8 a 11: execução no navegador, variáveis, tipos, conversões, operadores, condicionais, funções, escopo, repetição, arrays, objetos e métodos de coleção.

A consulta serve para recuperar sintaxe, não para compartilhar soluções. DOM, eventos, formulários, armazenamento e requisições assíncronas não fazem parte desta avaliação, pois serão estudados a partir do encontro 13.

## Objetivos avaliados

- estruturar dados com arrays e objetos;
- converter e validar entradas;
- implementar condicionais e testar limites;
- criar funções com parâmetros e retorno;
- empregar repetição com controle correto;
- selecionar métodos de coleção conforme o resultado;
- evitar mutações acidentais;
- separar entrada, processamento e saída;
- diagnosticar erros com Console e DevTools;

## 1. Regras da avaliação

### Consulta permitida

- materiais dos encontros 8 a 11;
- anotações e códigos próprios;
- documentação oficial da MDN;
- professor, para esclarecer o enunciado.

### Trabalho individual

- cada estudante deve escrever e entregar seu código;
- não é permitido compartilhar arquivos, trechos ou respostas;
- não é permitido editar o projeto de outra pessoa;
- ferramentas de geração automática de código não podem ser utilizadas;
- as decisões adotadas devem ser explicáveis.

## 2. Situação-problema

Uma biblioteca comunitária precisa analisar seu acervo antes de abrir novas reservas. O programa deverá processar livros, identificar disponibilidade, localizar itens, produzir resumos, calcular indicadores e simular uma reserva.

## 3. Dados obrigatórios

Crie o array `livros`:

| id | título | categoria | total | emprestados | ativo |
|---:|---|---|---:|---:|---|
| 1 | JavaScript para iniciantes | Tecnologia | 8 | 5 | true |
| 2 | Histórias do sertão | Literatura | 5 | 5 | true |
| 3 | Design para todos | Design | 6 | 2 | false |
| 4 | Ciência no cotidiano | Ciências | 10 | 4 | true |
| 5 | Memórias da cidade | História | 4 | 1 | true |

Cada objeto deve possuir `id`, `titulo`, `categoria`, `total`, `emprestados` e `ativo`.

Use também:

```js
const livroSelecionadoId = 1;
const novasSolicitacoesTexto = "2";
const bibliotecaAberta = true;
```

Não altere os dados para facilitar os resultados.

## 4. Preparação

Estrutura:

```text
atividade-pratica-02/
├── compose.yaml
├── index.html
├── README.md
└── js/
    └── app.js
```

`compose.yaml`:

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - .:/usr/share/nginx/html:ro
```

O HTML deve carregar:

```html
<script type="module" src="./js/app.js"></script>
```

Execute `docker compose up`, abra `http://localhost:8080` e confirme uma mensagem no Console.

## 5. Etapa A — Conversão e validação

Converta `novasSolicitacoesTexto` com `Number` e implemente:

```js
function quantidadeEhValida(quantidade) {
  // retorne um booleano
}
```

A quantidade será válida somente se for finita, inteira e maior que zero. Teste:

| Entrada | Resultado |
|---|---|
| `"2"` | válida |
| `"0"`, `"-1"` | inválida |
| `"2.5"` | inválida |
| `"duas"`, `""` | inválida |

A string vazia exige atenção: `Number("")` produz zero.

## 6. Etapa B — Funções

Implemente:

```js
function calcularDisponiveis(livro) {}
function possuiExemplares(livro) {}
function podeReservar(livro, quantidade, bibliotecaAberta) {}
function criarResumo(livro) {}
```

`criarResumo` deve retornar novo objeto com `id`, `titulo`, `categoria`, `disponiveis` e `situacao`. A situação será `"disponível"`, `"esgotado"` ou `"inativo"`.

As funções devem receber dependências por parâmetros e devolver resultados com `return`.

## 7. Etapa C — Diagnóstico da reserva

Localize o livro indicado por `livroSelecionadoId` e produza exatamente um caminho:

1. biblioteca fechada;
2. quantidade inválida;
3. livro não encontrado;
4. livro inativo;
5. quantidade acima da disponibilidade;
6. reserva que utiliza o último exemplar;
7. reserva confirmada com exemplares restantes.

Use uma cadeia coerente de `if`, `else if` e `else` ou guard clauses. Não acesse propriedades antes de confirmar que o livro existe.

## 8. Etapa D — Processamento da coleção

A partir de `livros`:

1. use `for...of` ou `forEach` para exibir título e disponibilidade;
2. crie `livrosDisponiveis` com `filter`;
3. crie `resumos` com `map` e `criarResumo`;
4. localize o id selecionado com `find`;
5. busque o id 99 e trate `undefined`;
6. verifique com `some` se existe livro esgotado;
7. verifique com `every` se todos possuem título;
8. use `reduce` para somar total, emprestados e disponíveis.

Cada `reduce` deve possuir valor inicial explícito.

## 9. Etapa E — Atualização sem mutação

Quando a reserva for válida:

- crie novo objeto para o livro selecionado usando spread;
- atualize `emprestados` no novo objeto;
- crie `acervoAtualizado` com `map`;
- preserve o array e o objeto originais;
- mostre original e atualizado no Console.

## 10. Saídas obrigatórias

Identifique claramente:

```text
=== ACERVO ORIGINAL ===
=== LIVROS DISPONÍVEIS ===
=== RESUMOS ===
=== INDICADORES ===
=== DIAGNÓSTICO DA RESERVA ===
=== ACERVO ATUALIZADO ===
```

Use `console.table` para coleções e mensagens adequadas para os diagnósticos.

## 11. Cenários de teste

Registre no README:

| Cenário | Configuração |
|---|---|
| sucesso | id 1 e `"2"` |
| último exemplar | quantidade igual à disponibilidade |
| excedente | quantidade maior que a disponibilidade |
| esgotado | id 2 |
| inativo | id 3 |
| inexistente | id 99 |
| inválido | `"duas"` |
| fechada | `bibliotecaAberta = false` |

Informe resultado esperado e observado.
