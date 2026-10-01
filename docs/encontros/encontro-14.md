# Encontro 14 — JavaScript no navegador: DOM, eventos e dados

**Unidade:** Unidade 1
**Carga horária:** 1,5h

## 1. Retomar o projeto com Docker

Neste encontro, o acervo desenvolvido anteriormente passará a ser exibido e atualizado na página. Para manter o exemplo simples, todo o comportamento ficará em um único arquivo JavaScript: `js/app.js`.

A estrutura final será:

```text
encontro-14-acervo/
├── compose.yaml
├── index.html
├── dados/
│   └── livros.json
└── js/
    └── app.js
```

Comece apenas com `compose.yaml`, `index.html` e a pasta `js`. A pasta de dados será adicionada posteriormente.

Use a configuração Docker dos encontros anteriores:

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - .:/usr/share/nginx/html:ro
```

Execute:

```bash
docker compose up
```

Acesse `http://localhost:8080`. O servidor HTTP é necessário porque o navegador aplica restrições quando `fetch` é executado diretamente pelo sistema de arquivos.

## 2. Construir a página gradualmente

### 2.1 Criar o documento básico

Crie `index.html`:

```html
<!doctype html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Acervo comunitário</title>
  </head>
  <body>
    <main>
      <h1>Consulta e reserva do acervo</h1>
    </main>
  </body>
</html>
```

Salve e recarregue. O título deve aparecer antes da inclusão de qualquer JavaScript. Essa verificação separa problemas de estrutura HTML de problemas de comportamento.

### 2.2 Adicionar a área dos livros

Dentro de `main`, depois do `h1`, adicione:

```html
<section aria-labelledby="titulo-acervo">
  <h2 id="titulo-acervo">Livros</h2>

  <p id="estado-acervo" role="status" aria-live="polite">
    Aguardando carregamento.
  </p>

  <ul id="lista-livros"></ul>
</section>
```

Os identificadores possuem responsabilidades diferentes:

- `estado-acervo` receberá mensagens como “Carregando” ou “Nenhum livro encontrado”;
- `lista-livros` receberá os elementos criados pelo JavaScript;
- `aria-live="polite"` permite que tecnologias assistivas anunciem mudanças sem interromper imediatamente o usuário.

### 2.3 Adicionar o início do formulário

Antes da seção dos livros, adicione:

```html
<section aria-labelledby="titulo-reserva">
  <h2 id="titulo-reserva">Nova reserva</h2>

  <form id="form-reserva">
    <label for="livro">Livro</label>
    <select id="livro" name="livro" required>
      <option value="">Selecione um livro</option>
    </select>
  </form>
</section>
```

O `id` conecta o `label` ao controle. O `name` define a chave utilizada posteriormente pelo `FormData`.

### 2.4 Adicionar a quantidade

Dentro do formulário, depois do `select`, adicione:

```html
<label for="quantidade">Quantidade</label>
<input
  id="quantidade"
  name="quantidade"
  type="number"
  min="1"
  step="1"
  required
>
```

- `type="number"` oferece um controle numérico;
- `min="1"` impede valores menores que um na validação do navegador;
- `step="1"` indica números inteiros;
- a regra também será validada no JavaScript, pois atributos HTML não substituem regras de negócio.

### 2.5 Adicionar os botões e a mensagem

Ainda dentro do formulário, adicione:

```html
<button id="botao-reservar" type="submit">
  Reservar
</button>

<button id="botao-restaurar" type="button">
  Restaurar dados
</button>
```

Depois do fechamento do formulário, adicione:

```html
<p id="mensagem" role="status" aria-live="polite"></p>
```

O primeiro botão envia o formulário. O segundo possui `type="button"` porque não deve disparar o evento de envio.

### 2.6 Conectar o único arquivo JavaScript

Antes de fechar `body`, adicione:

```html
<script type="module" src="./js/app.js"></script>
```

Embora exista somente um arquivo JavaScript, `type="module"` mantém o código no escopo do módulo e permite usar recursos modernos. Não serão utilizados `import` ou `export` neste exemplo.

Crie `js/app.js`:

```js
console.log("Encontro 14 conectado.");
```

### Verificação

Abra o Console e confirme a mensagem. Envie o formulário vazio e observe a validação nativa do navegador.

## 3. Conhecer o DOM com um exemplo pequeno

Antes de desenvolver a aplicação completa, criaremos um único item. Esse código será temporário e será substituído mais adiante.

### 3.1 Selecionar a lista

Substitua o conteúdo de `app.js`:

```js
const listaLivros = document.querySelector("#lista-livros");

console.log(listaLivros);
```

O navegador transforma o HTML em objetos. `querySelector` procura o primeiro objeto correspondente ao seletor CSS e devolve `null` quando não encontra.

Adicione a verificação:

```js
if (!listaLivros) {
  throw new Error("Lista de livros não encontrada.");
}
```

Troque temporariamente o seletor por `"#lista-inexistente"`, observe o erro e restaure o valor correto. Assim, fica claro por que a verificação é necessária.

### 3.2 Criar um dado temporário

Depois da verificação, adicione:

```js
const livro = {
  id: 1,
  titulo: "JavaScript para iniciantes",
  categoria: "Tecnologia",
  total: 8,
  emprestados: 5,
  ativo: true,
};
```

Esse objeto existe apenas para experimentar a criação de elementos. A coleção definitiva virá do arquivo JSON.

### 3.3 Criar os elementos em memória

Adicione:

```js
const item = document.createElement("li");
const titulo = document.createElement("h3");
const detalhes = document.createElement("p");
```

Nada aparece ainda. `createElement` cria objetos desconectados da árvore visível.

### 3.4 Preencher e inserir os elementos

Defina os textos:

```js
titulo.textContent = livro.titulo;
detalhes.textContent =
  `${livro.categoria} — ${livro.total - livro.emprestados} disponível(is)`;
```

Depois, conecte os elementos:

```js
item.append(titulo, detalhes);
listaLivros.append(item);
```

`textContent` trata o valor como texto, enquanto `append` estabelece a relação entre os elementos.

### Verificação

A página deve mostrar “JavaScript para iniciantes” e três exemplares disponíveis. Inspecione o `li` criado na aba Elements.

## 4. Preparar os dados externos

### 4.1 Criar o arquivo JSON

Adicione a pasta `dados` e crie `dados/livros.json`. Comece com uma coleção vazia:

```json
[]
```

Acesse `http://localhost:8080/dados/livros.json`. Se o navegador mostrar `[]`, o caminho está correto.

### 4.2 Adicionar o primeiro objeto

Substitua o conteúdo:

```json
[
  {
    "id": 1,
    "titulo": "JavaScript para iniciantes",
    "categoria": "Tecnologia",
    "total": 8,
    "emprestados": 5,
    "ativo": true
  }
]
```

JSON é texto estruturado:

- propriedades e textos usam aspas duplas;
- valores numéricos e booleanos não usam aspas;
- comentários não são permitidos;
- não deve existir vírgula depois do último item.

### 4.3 Completar a coleção

Use o primeiro objeto como modelo e adicione os demais dados da Atividade Prática 2:

| id | título | categoria | total | emprestados | ativo |
|---:|---|---|---:|---:|---|
| 2 | Histórias do sertão | Literatura | 5 | 5 | true |
| 3 | Design para todos | Design | 6 | 2 | false |
| 4 | Ciência no cotidiano | Ciências | 10 | 4 | true |
| 5 | Memórias da cidade | História | 4 | 1 | true |

Separe os objetos com vírgulas. Recarregue o endereço do JSON para confirmar que a estrutura continua válida.

## 5. Reconstruir app.js gradualmente

O experimento com um único livro cumpriu seu objetivo. Apague o conteúdo temporário de `app.js`. A partir daqui, o mesmo arquivo reunirá referências do DOM, estado, funções e eventos.

### 5.1 Selecionar os elementos

Comece com os controles do formulário:

```js
const formulario = document.querySelector("#form-reserva");
const campoLivro = document.querySelector("#livro");
const mensagem = document.querySelector("#mensagem");
```

Abaixo, selecione os elementos do acervo e os botões:

```js
const estadoAcervo = document.querySelector("#estado-acervo");
const listaLivros = document.querySelector("#lista-livros");
const botaoReservar = document.querySelector("#botao-reservar");
const botaoRestaurar = document.querySelector("#botao-restaurar");
```

Agora verifique todas as dependências:

```js
if (
  !formulario ||
  !campoLivro ||
  !mensagem ||
  !estadoAcervo ||
  !listaLivros ||
  !botaoReservar ||
  !botaoRestaurar
) {
  throw new Error("Elementos obrigatórios não encontrados.");
}
```

Se algum `id` do HTML estiver incorreto, o programa será interrompido com uma mensagem compreensível.

### 5.2 Criar o estado

Depois da verificação, adicione:

```js
const estado = {
  livros: [],
};
```

O estado é a fonte de dados atual da aplicação. O DOM será atualizado a partir de `estado.livros`, em vez de servir como local principal de armazenamento.

### 5.3 Criar a chave de persistência

Adicione:

```js
const CHAVE_ACERVO = "acervo-comunitario";
```

Uma constante evita repetir o texto da chave e reduz erros de digitação.

## 6. Criar as funções de regra e interface

Todas as funções a seguir continuam em `app.js`, depois das constantes.

### 6.1 Calcular a disponibilidade

Adicione:

```js
function calcularDisponiveis(livro) {
  return livro.total - livro.emprestados;
}
```

A função recebe um livro e devolve um valor. Ela não consulta o DOM nem altera o objeto, portanto pode ser usada na validação e na renderização.

### 6.2 Validar a quantidade

Adicione:

```js
function quantidadeEhValida(quantidade) {
  const ehFinita = Number.isFinite(quantidade);
  const ehInteira = Number.isInteger(quantidade);
  const ehPositiva = quantidade > 0;

  return ehFinita && ehInteira && ehPositiva;
}
```

As variáveis intermediárias tornam cada condição visível:

- finita rejeita `NaN` e infinito;
- inteira rejeita `2.5`;
- positiva rejeita zero e negativos.

### 6.3 Validar uma reserva

Comece a função tratando a ausência do livro:

```js
function validarReserva(livro, quantidade) {
  if (livro === undefined) {
    return "Selecione um livro.";
  }
```

Continue dentro da mesma função:

```js
  if (!quantidadeEhValida(quantidade)) {
    return "Informe uma quantidade inteira maior que zero.";
  }

  if (!livro.ativo) {
    return "Este livro está inativo.";
  }
```

Finalize:

```js
  if (quantidade > calcularDisponiveis(livro)) {
    return "A quantidade supera os exemplares disponíveis.";
  }

  return null;
}
```

Cada `return` encerra a função assim que encontra um problema. `null` significa que nenhuma mensagem de erro foi produzida.

### 6.4 Criar um item visual

Comece apenas com os elementos:

```js
function criarItemLivro(livro) {
  const item = document.createElement("li");
  const titulo = document.createElement("h3");
  const detalhes = document.createElement("p");
```

Ainda dentro da função, obtenha a situação:

```js
  const disponiveis = calcularDisponiveis(livro);
  const situacao = livro.ativo
    ? `${disponiveis} disponível(is)`
    : "inativo";
```

O operador ternário escolhe entre duas mensagens. Um livro inativo é identificado antes de destacar sua quantidade física.

Finalize a função:

```js
  titulo.textContent = livro.titulo;
  detalhes.textContent = `${livro.categoria} — ${situacao}`;

  item.append(titulo, detalhes);
  return item;
}
```

A função devolve o `li` pronto, mas ainda não o insere na lista.

### 6.5 Criar a opção do seletor

Adicione uma função separada:

```js
function criarOpcaoLivro(livro) {
  const opcao = document.createElement("option");

  opcao.value = String(livro.id);
  opcao.textContent = livro.titulo;
```

O valor é convertido em string porque controles de formulário trabalham com texto.

Complete a função:

```js
  opcao.disabled =
    !livro.ativo || calcularDisponiveis(livro) === 0;

  return opcao;
}
```

Livros inativos ou esgotados aparecem na lista visual, mas não podem ser escolhidos para uma nova reserva.

### 6.6 Renderizar a coleção

Comece limpando resultados antigos:

```js
function renderizarLivros(livros) {
  listaLivros.replaceChildren();
  campoLivro.replaceChildren();
```

A limpeza é necessária porque a função será chamada novamente após cada reserva.

Crie a opção inicial do seletor:

```js
  const opcaoInicial = document.createElement("option");
  opcaoInicial.value = "";
  opcaoInicial.textContent = "Selecione um livro";
  campoLivro.append(opcaoInicial);
```

Percorra a coleção:

```js
  livros.forEach((livro) => {
    listaLivros.append(criarItemLivro(livro));
    campoLivro.append(criarOpcaoLivro(livro));
  });
}
```

Para cada objeto, uma representação entra na lista e outra entra no `select`.

### 6.7 Exibir mensagens

Adicione:

```js
function exibirMensagem(texto, tipo = "informacao") {
  mensagem.textContent = texto;
  mensagem.dataset.tipo = tipo;
}
```

O atributo `data-tipo` poderá ser usado posteriormente pelo CSS. A mensagem permanece compreensível mesmo sem estilo.

Agora crie a função para o estado do acervo:

```js
function exibirEstado(tipo, texto) {
  estadoAcervo.textContent = texto;
  estadoAcervo.dataset.estado = tipo;
```

Complete controlando o botão:

```js
  botaoReservar.disabled =
    tipo === "carregando" || tipo === "erro";
}
```

Durante carregamento ou erro, impedir uma reserva evita ações sobre dados ainda indisponíveis.

## 7. Adicionar persistência no mesmo arquivo

### 7.1 Salvar os livros

Adicione em `app.js`:

```js
function salvarLivros(livros) {
  const texto = JSON.stringify(livros);
  localStorage.setItem(CHAVE_ACERVO, texto);
}
```

O `localStorage` armazena somente strings. `JSON.stringify` transforma o array em texto JSON.

### 7.2 Iniciar a recuperação

Adicione:

```js
function recuperarLivros() {
  const texto = localStorage.getItem(CHAVE_ACERVO);

  if (texto === null) {
    return null;
  }
```

`null` indica que ainda não há dados salvos e que a aplicação deverá buscar o arquivo JSON.

### 7.3 Converter o texto com segurança

Continue dentro da função:

```js
  try {
    const dados = JSON.parse(texto);

    return Array.isArray(dados) ? dados : null;
  } catch (erro) {
    console.error("Dados locais inválidos:", erro);
```

Se o texto não for JSON válido, `JSON.parse` lança um erro. O `catch` impede que a página pare completamente.

Finalize o tratamento:

```js
    localStorage.removeItem(CHAVE_ACERVO);
    return null;
  }
}
```

O valor inválido é removido para que a próxima inicialização possa buscar dados limpos.

### 7.4 Remover os dados salvos

Adicione:

```js
function removerLivrosSalvos() {
  localStorage.removeItem(CHAVE_ACERVO);
}
```

Não use `localStorage.clear()`: ele apagaria todas as chaves pertencentes à mesma origem.

## 8. Buscar o JSON no mesmo arquivo

### 8.1 Iniciar a função assíncrona

Adicione:

```js
async function buscarLivros() {
  const resposta = await fetch("./dados/livros.json");
```

`fetch` inicia a requisição e devolve uma Promise. `await` pausa apenas esta função até que a resposta chegue.

### 8.2 Verificar o status HTTP

Continue:

```js
  if (!resposta.ok) {
    throw new Error(
      `Falha ao carregar livros: HTTP ${resposta.status}`,
    );
  }
```

O `fetch` pode receber uma resposta 404 sem lançar erro automaticamente. Por isso, o status deve ser verificado.

### 8.3 Converter o corpo da resposta

Finalize:

```js
  return resposta.json();
}
```

`resposta.json()` também é assíncrono e devolve o array convertido quando o JSON é válido.

## 9. Relacionar estado e renderização

Crie:

```js
function atualizarInterface() {
  renderizarLivros(estado.livros);
```

Primeiro, a interface sempre recebe a coleção atual.

Trate a coleção vazia:

```js
  if (estado.livros.length === 0) {
    exibirEstado("vazio", "Nenhum livro encontrado.");
    return;
  }
```

O `return` impede que a mensagem de sucesso seja apresentada depois da mensagem de vazio.

Finalize:

```js
  exibirEstado(
    "sucesso",
    `${estado.livros.length} livro(s) carregado(s).`,
  );
}
```

Essa função não busca nem salva dados. Ela apenas sincroniza o DOM com o estado existente.

## 10. Inicializar a aplicação

### 10.1 Mostrar o carregamento

Comece:

```js
async function iniciar() {
  exibirEstado("carregando", "Carregando o acervo...");
```

A mensagem aparece antes da operação assíncrona, oferecendo retorno imediato ao usuário.

### 10.2 Procurar dados locais

Ainda dentro da função, abra o `try`:

```js
  try {
    const livrosSalvos = recuperarLivros();
```

Se existir uma coleção salva, ela representa as reservas feitas anteriormente.

### 10.3 Escolher a fonte

Continue:

```js
    if (livrosSalvos !== null) {
      estado.livros = livrosSalvos;
    } else {
      estado.livros = await buscarLivros();
    }

    atualizarInterface();
```

O `if` foi usado no lugar de uma expressão compacta para tornar a decisão mais explícita.

### 10.4 Tratar falhas

Finalize a função:

```js
  } catch (erro) {
    console.error(erro);
    exibirEstado(
      "erro",
      "Não foi possível carregar o acervo.",
    );
  }
}
```

Erros de rede, HTTP ou JSON chegam ao mesmo `catch`. O detalhe técnico fica no Console e uma mensagem simples fica na página.

## 11. Processar o formulário

Os eventos ficam depois das funções, próximos ao final de `app.js`.

### 11.1 Escutar o envio

Adicione:

```js
formulario.addEventListener("submit", (evento) => {
  evento.preventDefault();
```

O evento `submit` representa a intenção de enviar o formulário e funciona tanto pelo botão quanto pela tecla Enter. `preventDefault()` evita o recarregamento da página.

### 11.2 Ler os campos

Continue dentro do callback:

```js
  const dados = new FormData(formulario);
  const livroIdTexto = dados.get("livro");
  const quantidadeTexto = dados.get("quantidade");
```

`FormData` usa os atributos `name` do HTML. Os valores são obtidos como texto.

Converta-os:

```js
  const livroId = Number(livroIdTexto);
  const quantidade = Number(quantidadeTexto);
```

A conversão ocorre antes da busca e da validação para que comparações numéricas sejam coerentes.

### 11.3 Localizar e validar

Adicione:

```js
  const livro = estado.livros.find(
    (item) => item.id === livroId,
  );

  const erro = validarReserva(livro, quantidade);
```

`find` devolve o primeiro objeto correspondente ou `undefined`.

Trate o erro:

```js
  if (erro !== null) {
    exibirMensagem(erro, "erro");
    return;
  }
```

O retorno antecipado garante que dados inválidos não sejam persistidos.

### 11.4 Criar o novo estado

Continue:

```js
  estado.livros = estado.livros.map((item) => {
    if (item.id !== livroId) {
      return item;
    }
```

Os itens não selecionados são reutilizados. Para o selecionado, crie um novo objeto:

```js
    return {
      ...item,
      emprestados: item.emprestados + quantidade,
    };
  });
```

O `map` cria um novo array e o spread cria o objeto alterado. O acervo anterior não é modificado diretamente.

### 11.5 Salvar e renderizar

Ainda no callback, adicione:

```js
  salvarLivros(estado.livros);
  atualizarInterface();
```

Salvar preserva a alteração após uma recarga. Renderizar faz a página refletir imediatamente o novo estado.

Finalize o evento:

```js
  formulario.reset();
  exibirMensagem(
    "Reserva registrada com sucesso.",
    "sucesso",
  );
});
```

O formulário é limpo somente após a operação válida.

## 12. Restaurar os dados originais

Adicione o segundo evento:

```js
botaoRestaurar.addEventListener("click", async () => {
  removerLivrosSalvos();
  exibirMensagem("");
```

A remoção afeta somente a chave do acervo. Continue:

```js
  await iniciar();
});
```

A nova inicialização não encontra dados locais e volta a carregar `livros.json`.

## 13. Executar a aplicação

Na última linha de `app.js`, adicione:

```js
iniciar();
```

Essa chamada é o ponto de entrada. As funções anteriores apenas descrevem comportamentos; `iniciar()` começa o fluxo.

### Ordem final do arquivo

Use esta lista para conferir a organização sem receber o código completo pronto:

1. seletores do DOM;
2. verificação dos elementos;
3. `estado` e `CHAVE_ACERVO`;
4. funções de regra;
5. funções de criação e renderização;
6. funções de mensagens;
7. funções do `localStorage`;
8. `buscarLivros`;
9. `atualizarInterface`;
10. `iniciar`;
11. evento de envio;
12. evento de restauração;
13. chamada `iniciar()`.

### Fluxo da inicialização

```text
iniciar
├── mostra "carregando"
├── procura dados no localStorage
├── se não existirem, busca livros.json
├── atualiza estado.livros
└── renderiza sucesso, vazio ou erro
```

### Fluxo da reserva

```text
submit
├── impede o recarregamento
├── lê e converte os campos
├── localiza e valida o livro
├── cria um novo array
├── salva no localStorage
└── renderiza novamente
```

## 14. Testar antes de avançar

Teste um cenário de cada vez:

| Cenário | Como provocar | Resultado esperado |
|---|---|---|
| carregando | recarregar a página | mensagem durante a busca |
| sucesso | manter o JSON válido | lista e seletor preenchidos |
| reserva válida | id 1 e quantidade 2 | disponibilidade passa de 3 para 1 |
| campo vazio | enviar sem preencher | validação do navegador |
| quantidade decimal | informar 1.5 | reserva recusada |
| quantidade excessiva | solicitar acima do disponível | mensagem e nenhuma alteração |
| persistência | recarregar depois de reservar | alteração permanece |
| restauração | clicar no botão correspondente | valores voltam ao JSON |
| vazio | usar temporariamente `[]` no JSON | mensagem de acervo vazio |
| HTTP 404 | alterar temporariamente o caminho | estado de erro |
| JSON inválido | remover uma vírgula | erro tratado no `catch` |

Use as ferramentas do navegador:

- **Console:** mostra erros técnicos e ajuda a localizar a linha;
- **Network:** mostra a requisição ao JSON, o status e a resposta;
- **Application:** permite inspecionar a chave do `localStorage`;
- **Elements:** mostra os elementos criados dinamicamente.

Restaure o código correto depois dos testes intencionais.

## 15. Prática guiada — filtro por categoria

Implemente um filtro sem criar outro arquivo JavaScript:

1. adicione ao HTML um `select` com `id="categoria"`;
2. selecione o controle no início de `app.js`;
3. verifique se ele foi encontrado;
4. crie as opções a partir das categorias existentes;
5. registre o evento `change`;
6. filtre sem modificar `estado.livros`;
7. envie o resultado filtrado para `renderizarLivros`;
8. apresente uma mensagem quando o resultado estiver vazio.

A coleção filtrada é apenas uma visão temporária:

```js
const livrosFiltrados = categoriaSelecionada
  ? estado.livros.filter(
      (livro) => livro.categoria === categoriaSelecionada,
    )
  : estado.livros;
```

Não substitua `estado.livros` pelo resultado do filtro, pois isso perderia a referência à coleção completa.

## 16. Exercício aplicado — cancelar uma reserva

Adicione o cancelamento de uma reserva no mesmo `app.js`.

### Requisitos

- incluir um botão de cancelamento para cada livro;
- identificar o livro com `data-id` ou estratégia equivalente;
- impedir que `emprestados` fique negativo;
- atualizar com `map` e spread;
- salvar no `localStorage`;
- renderizar novamente;
- comunicar sucesso ou erro por texto;
- manter o funcionamento por teclado.

### Desafio adicional

Adicione um botão “Tentar novamente” ao estado de erro. Ele deve chamar `iniciar` sem recarregar a página inteira.

## 17. Erros comuns

### Abrir o HTML sem servidor

Módulos e requisições podem ser bloqueados quando o arquivo é aberto diretamente. Use `http://localhost:8080`.

### Usar um id diferente do HTML

`querySelector("#lista-livro")` não encontra `id="lista-livros"`. Confira nomes e observe a verificação inicial.

### Esquecer resposta.ok

O `fetch` pode receber 404 sem lançar erro automaticamente. Verifique `resposta.ok`.

### Renderizar sem limpar

Sem `replaceChildren()`, cada atualização acrescenta outra cópia dos livros.

### Confiar apenas no HTML

Os atributos `min` e `step` ajudam na interface, mas a regra também precisa existir no JavaScript.

### Salvar o array diretamente

Este código não preserva a estrutura:

```js
localStorage.setItem(CHAVE_ACERVO, estado.livros);
```

Converta antes:

```js
const texto = JSON.stringify(estado.livros);
localStorage.setItem(CHAVE_ACERVO, texto);
```

### Limpar todo o armazenamento

Evite `localStorage.clear()`. Remova apenas `CHAVE_ACERVO`.

### Misturar estado e DOM

Não tente descobrir o valor atual lendo textos da página. Use `estado.livros` como fonte e renderize o DOM a partir dele.

## 18. Critérios de aceite

- o projeto executa com `docker compose up`;
- existe somente um arquivo JavaScript: `js/app.js`;
- o HTML carrega esse arquivo com `type="module"`;
- elementos obrigatórios são verificados;
- o acervo é carregado com `fetch`;
- respostas HTTP malsucedidas são tratadas;
- existem estados de carregamento, sucesso, vazio e erro;
- o formulário funciona pelo botão e por Enter;
- regras são validadas antes da alteração;
- a atualização usa `map` e spread;
- os dados persistem depois de recarregar;
- a restauração remove apenas a chave da aplicação;
- não existem erros inesperados no Console;
- mensagens são compreensíveis sem depender de cor.

## Checklist de compreensão

- [ ] Explico a diferença entre HTML e DOM.
- [ ] Sei que `querySelector` pode retornar `null`.
- [ ] Crio elementos com `createElement`.
- [ ] Uso `textContent` para inserir textos.
- [ ] Registro eventos com `addEventListener`.
- [ ] Uso `submit`, `preventDefault` e `FormData`.
- [ ] Uso um objeto para representar o estado.
- [ ] Explico `async`, `await` e `fetch`.
- [ ] Verifico `resposta.ok`.
- [ ] Trato falhas com `try/catch`.
- [ ] Uso `JSON.stringify` e `JSON.parse`.
- [ ] Consigo localizar cada responsabilidade em `app.js`.
- [ ] Testo estados alternativos antes de concluir.

## Questões de fixação

1. Por que `querySelector` pode devolver `null`?
2. Qual é a diferença entre criar um elemento e inseri-lo no DOM?
3. Por que `submit` é preferível a ouvir apenas o clique?
4. O que `preventDefault` impede?
5. Por que os valores do formulário são convertidos?
6. Qual é o papel de `estado.livros`?
7. Por que o `localStorage` exige JSON?
8. Quando `getItem` devolve `null`?
9. Por que verificar `resposta.ok`?
10. O que `await` faz na função `buscarLivros`?
11. Como `map` e spread evitam alteração direta?
12. Qual é a diferença entre estado vazio e estado de erro?

## Referências

- [MDN — Introdução ao DOM](https://developer.mozilla.org/pt-BR/docs/Web/API/Document_Object_Model/Introduction)
- [MDN — Eventos](https://developer.mozilla.org/pt-BR/docs/Learn_web_development/Core/Scripting/Events)
- [MDN — FormData](https://developer.mozilla.org/pt-BR/docs/Web/API/FormData)
- [MDN — localStorage](https://developer.mozilla.org/pt-BR/docs/Web/API/Window/localStorage)
- [MDN — Fetch API](https://developer.mozilla.org/pt-BR/docs/Web/API/Fetch_API/Using_Fetch)
- [MDN — async function](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/async_function)

## Encerramento

Neste encontro, o JavaScript passou a controlar uma interface real usando apenas `app.js`. O arquivo reúne seleção do DOM, regras, renderização, eventos, armazenamento e carregamento assíncrono de dados. A separação foi mantida por funções pequenas e por uma ordem clara, sem introduzir vários módulos antes de a turma dominar o fluxo completo.

[Voltar ao cronograma](../01-cronograma-60h.md)
