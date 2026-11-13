# Encontro 21 — Introdução ao TypeScript

**Unidade:** Unidade 2
**Carga horária:** 1,5h
**Entrega prevista:** primeiro programa TypeScript configurado, compilado e testado

## Visão geral

Até aqui, os programas foram escritos em JavaScript. A linguagem aceita que uma variável receba valores de tipos diferentes e muitos problemas só aparecem quando determinada linha é executada. TypeScript acrescenta uma etapa de análise estática: antes de gerar JavaScript, o compilador verifica se os valores são usados de maneira coerente.

Neste encontro, construiremos um projeto pequeno do zero. Cada conceito será apresentado a partir de um problema observável, testado pelo compilador e relacionado ao JavaScript gerado. O objetivo não é adicionar tipos em todas as linhas, mas aprender onde eles tornam o código mais previsível.

## Objetivos de aprendizagem

- explicar a relação entre JavaScript e TypeScript;
- distinguir análise estática de validação em execução;
- instalar, configurar e executar o compilador;
- interpretar mensagens de erro;
- usar inferência e anotações de tipo;
- trabalhar com tipos primitivos e arrays;
- tipar parâmetros e retornos;
- gerar JavaScript para o navegador;
- evitar o uso desnecessário de `any`.

## 1. O problema que TypeScript ajuda a detectar

Considere este JavaScript:

```js
function calcularTotal(preco, quantidade) {
  return preco * quantidade;
}

console.log(calcularTotal(25, 2));
console.log(calcularTotal("vinte e cinco", 2));
```

A primeira chamada produz `50`. A segunda produz `NaN`, mas o problema só é percebido durante a execução.

Em TypeScript, declaramos o contrato:

```ts
function calcularTotal(preco: number, quantidade: number): number {
  return preco * quantidade;
}
```

Agora esta chamada é recusada antes da execução:

```ts
calcularTotal("vinte e cinco", 2);
```

O compilador não substitui testes nem valida entradas externas. Ele verifica se o código respeita os tipos conhecidos durante o desenvolvimento.

## 2. Preparar o projeto com Docker

Crie:

```text
encontro-21-typescript/
├── compose.yaml
├── Dockerfile
├── package.json
├── tsconfig.json
├── index.html
├── src/
│   └── app.ts
└── dist/
```

O código será escrito em `src/app.ts`. O compilador criará `dist/app.js`, carregado pelo navegador.

### 2.1 Configurar as dependências

Crie `package.json`:

```json
{
  "scripts": {
    "dev": "tsc --watch",
    "build": "tsc",
    "check": "tsc --noEmit"
  },
  "devDependencies": {
    "typescript": "^5.9.0"
  }
}
```

- `dev` observa e recompila;
- `build` gera uma compilação;
- `check` verifica sem gerar arquivos.

### 2.2 Criar os serviços

Crie `Dockerfile`:

```dockerfile
FROM node:22-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["npm", "run", "dev"]
```

Crie `compose.yaml`:

```yaml
services:
  typescript:
    build: .
    volumes:
      - .:/app
      - /app/node_modules

  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - .:/usr/share/nginx/html:ro
```

O serviço `typescript` compila; o serviço `web` disponibiliza os arquivos.

## 3. Configurar o compilador

Crie `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ES2022",
    "rootDir": "src",
    "outDir": "dist",
    "strict": true,
    "noEmitOnError": true
  },
  "include": ["src/**/*.ts"]
}
```

- `rootDir`: código TypeScript;
- `outDir`: JavaScript gerado;
- `strict`: verificações mais seguras;
- `noEmitOnError`: não gera código com erros;
- `include`: arquivos analisados.

Execute:

```bash
docker compose up --build
```

## 4. Criar o primeiro programa

Em `src/app.ts`:

```ts
const mensagem: string = "TypeScript conectado.";
console.log(mensagem);
```

Crie `index.html`:

```html
<!doctype html>
<html lang="pt-BR">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Introdução ao TypeScript</title>
  </head>
  <body>
    <h1>Introdução ao TypeScript</h1>
    <script type="module" src="./dist/app.js"></script>
  </body>
</html>
```

Acesse `http://localhost:8080`, confirme a mensagem e verifique se `dist/app.js` foi criado. O navegador executa JavaScript, não o arquivo `.ts`.

## 5. Inferência de tipos

TypeScript deduz tipos pelos valores:

```ts
const nomeCurso = "Desenvolvimento Frontend";
const cargaHoraria = 60;
const inscricoesAbertas = true;
```

Passe o cursor sobre as variáveis. Os tipos inferidos serão `string`, `number` e `boolean`.

```ts
let quantidadeInscritos = 18;
quantidadeInscritos = 20;
```

Agora provoque um erro:

```ts
quantidadeInscritos = "vinte";
```

Mesmo sem anotação, a variável foi inferida como `number`. Leia a mensagem e remova a linha inválida antes de continuar.

## 6. Quando escrever uma anotação

Anotações são úteis quando o valor inicial não define o tipo:

```ts
let mensagemStatus: string;

if (inscricoesAbertas) {
  mensagemStatus = "Inscrições abertas";
} else {
  mensagemStatus = "Inscrições encerradas";
}
```

Evite repetir o óbvio: `const cidade = "Fortaleza"` já é inferido como texto.

## 7. Tipos primitivos e arrays

```ts
const titulo: string = "Interfaces Web";
const vagas: number = 30;
const ativo: boolean = true;
const notas: number[] = [8, 7.5, 9];
```

Adicionar número é permitido:

```ts
notas.push(10);
```

Adicionar texto é recusado:

```ts
notas.push("dez");
```

Leia o erro e remova essa chamada. Use `string`, `number` e `boolean` em minúsculas. `String`, `Number` e `Boolean` representam objetos.

## 8. Funções tipadas

Comece pelos parâmetros:

```ts
function calcularMedia(valores: number[]) {
  const total = valores.reduce((soma, valor) => soma + valor, 0);
  return total / valores.length;
}
```

O retorno é inferido como `number`. Podemos declarar o contrato:

```ts
function calcularMedia(valores: number[]): number {
  const total = valores.reduce((soma, valor) => soma + valor, 0);
  return total / valores.length;
}
```

Uma função que produz efeito, mas não devolve valor útil, retorna `void`:

```ts
function exibirResumo(nome: string, media: number): void {
  console.log(`${nome}: média ${media.toFixed(1)}`);
}
```

```ts
const media = calcularMedia(notas);
exibirResumo(nomeCurso, media);
```

## 9. Por que evitar any

`any` desativa verificações:

```ts
function mostrarValor(valor: any): void {
  console.log(valor.toUpperCase());
}

mostrarValor(42);
```

Esse trecho demonstra o risco e não deve permanecer na versão estável.

Com `unknown`, uma verificação é obrigatória:

```ts
function mostrarValor(valor: unknown): void {
  if (typeof valor === "string") {
    console.log(valor.toUpperCase());
  }
}
```

Esse refinamento será aprofundado no encontro 22.

## 10. Prática guiada

Construa o resumo de uma oficina com:

- título textual;
- capacidade e inscritos numéricos;
- estado ativo ou inativo;
- função que calcule vagas;
- função que devolva a situação;
- função `void` que apresente o resumo.

Verifique:

```bash
docker compose exec typescript npm run check
```

## 11. Exercício aplicado

Crie um programa que analise quatro valores:

1. armazene-os em array tipado;
2. calcule total e média em funções separadas;
3. conte quantos atingiram uma referência;
4. apresente os resultados com uma função `void`;
5. compile sem erros;
6. não utilize `any`.

## 12. Erros comuns

- carregar `app.ts` diretamente no navegador;
- confundir erro de compilação com erro de execução;
- anotar todas as constantes desnecessariamente;
- usar `String` em vez de `string`;
- silenciar problemas com `any`;
- editar manualmente o arquivo de `dist`.

## 13. Critérios de aceite

- o ambiente inicia com Docker;
- o modo estrito está ativo;
- o navegador carrega `dist/app.js`;
- variáveis e arrays têm tipos coerentes;
- funções possuem parâmetros tipados;
- retornos são inferidos ou declarados conscientemente;
- o projeto passa em `npm run check`;
- não há uso injustificado de `any`.

## Checklist de compreensão

- [ ] Explico por que TypeScript gera JavaScript.
- [ ] Diferencio compilação e execução.
- [ ] Interpreto um erro simples do compilador.
- [ ] Reconheço tipos inferidos.
- [ ] Sei quando uma anotação é útil.
- [ ] Tipo arrays, parâmetros e retornos.
- [ ] Uso `void` quando adequado.
- [ ] Evito `any`.

## Questões de fixação

1. O navegador executa arquivos TypeScript?
2. Qual é a diferença entre inferência e anotação?
3. O que `strict` altera?
4. Por que `noEmitOnError` é útil?
5. Qual é a diferença entre `number[]` e `number`?
6. Quando uma função retorna `void`?
7. Por que `any` reduz a segurança?
8. TypeScript valida automaticamente dados de uma API?

## Referências

- [TypeScript — The Basics](https://www.typescriptlang.org/docs/handbook/2/basic-types.html)
- [TypeScript — Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
- [TypeScript — tsconfig.json](https://www.typescriptlang.org/tsconfig/)

[Voltar ao cronograma](../01-cronograma-60h.md)
