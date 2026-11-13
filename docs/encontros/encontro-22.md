# Encontro 22 — TypeScript: modelagem de dados e tipos compostos

**Unidade:** Unidade 2
**Carga horária:** 1,5h
**Entrega prevista:** catálogo modelado com aliases, interfaces, unions e narrowing

## Visão geral

No encontro 21, tipos primitivos, arrays e funções foram verificados pelo compilador. Agora o desafio é representar entidades com várias propriedades e estados possíveis. Um contrato nomeado torna explícito quais dados pertencem a uma oficina, quais são opcionais e quais não devem ser alterados.

O projeto anterior será ampliado gradualmente. Começaremos com um objeto, extrairemos um alias, compararemos alias e interface, representaremos estados com unions e usaremos narrowing antes de acessar informações específicas.

## Objetivos de aprendizagem

- modelar objetos com `type` e `interface`;
- distinguir propriedades obrigatórias, opcionais e `readonly`;
- criar arrays de objetos tipados;
- representar alternativas com unions;
- usar tipos literais para limitar valores;
- refinar tipos com `typeof` e propriedades discriminantes;
- criar funções que recebem e devolvem objetos;
- atualizar objetos sem mutação;
- reconhecer quando um generic simples é útil;
- interpretar erros de incompatibilidade estrutural.

## 1. Retomar o projeto

Continue em `encontro-21-typescript`:

```bash
docker compose up
```

Crie `src/encontro-22.ts`. A configuração do encontro anterior já inclui os arquivos TypeScript da pasta `src`.

Em `index.html`, carregue:

```html
<script type="module" src="./dist/encontro-22.js"></script>
```

Confirme:

```ts
console.log("Encontro 22 conectado.");
```

## 2. O limite de objetos inferidos

Comece com um objeto:

```ts
const oficina = {
  id: 1,
  titulo: "Fotografia urbana",
  capacidade: 30,
  inscritos: 23,
  ativa: true,
};
```

TypeScript infere o formato. Esta atribuição falha:

```ts
oficina.capacidade = "trinta";
```

Se várias funções e objetos precisam do mesmo formato, um tipo nomeado comunica melhor o domínio.

## 3. Criar um type alias

Declare:

```ts
type Oficina = {
  id: number;
  titulo: string;
  capacidade: number;
  inscritos: number;
  ativa: boolean;
};
```

Substitua a declaração anterior do objeto por esta versão com o contrato:

```ts
const oficina: Oficina = {
  id: 1,
  titulo: "Fotografia urbana",
  capacidade: 30,
  inscritos: 23,
  ativa: true,
};
```

Remova uma propriedade, altere um tipo e acrescente uma chave inesperada. Leia cada erro antes de restaurar.

## 4. Propriedades readonly e opcionais

Evolua o contrato:

```ts
type Oficina = {
  readonly id: number;
  titulo: string;
  capacidade: number;
  inscritos: number;
  ativa: boolean;
  descricao?: string;
};
```

- `readonly` impede reatribuição pelo contrato;
- `?` indica que a propriedade pode estar ausente;
- opcional não significa texto vazio.

Esta linha produz erro:

```ts
oficina.id = 2;
```

Observe o erro e remova a atribuição antes de continuar.

Considere a ausência:

```ts
const descricao = oficina.descricao ?? "Descrição não informada";
console.log(descricao);
```

## 5. Modelar com interface

A entidade também pode ser descrita por interface:

```ts
interface Oficina {
  readonly id: number;
  titulo: string;
  capacidade: number;
  inscritos: number;
  ativa: boolean;
  descricao?: string;
}
```

Para objetos, `type` e `interface` frequentemente atendem ao mesmo objetivo. Mantenha consistência, use unions com `type` e use `interface` quando quiser destacar o contrato de uma entidade.

Não mantenha as duas declarações `Oficina` ao mesmo tempo durante a comparação.

## 6. Array de objetos tipados

Crie:

```ts
const oficinas: Oficina[] = [
  {
    id: 1,
    titulo: "Fotografia urbana",
    capacidade: 30,
    inscritos: 23,
    ativa: true,
  },
  {
    id: 2,
    titulo: "Cerâmica manual",
    capacidade: 20,
    inscritos: 20,
    ativa: true,
  },
];
```

Todos os itens respeitam `Oficina`. O editor oferece autocomplete coerente nos métodos de coleção.

## 7. Funções que usam o contrato

Calcule vagas:

```ts
function calcularVagas(oficina: Oficina): number {
  return oficina.capacidade - oficina.inscritos;
}
```

Filtre as disponíveis:

```ts
function possuiVagas(oficina: Oficina): boolean {
  return oficina.ativa && calcularVagas(oficina) > 0;
}

const disponiveis = oficinas.filter(possuiVagas);
```

## 8. Union de tipos literais

Uma situação não deve aceitar qualquer texto:

```ts
type Situacao = "disponivel" | "lotada" | "inativa";
```

Use no retorno:

```ts
function obterSituacao(oficina: Oficina): Situacao {
  if (!oficina.ativa) {
    return "inativa";
  }

  if (calcularVagas(oficina) === 0) {
    return "lotada";
  }

  return "disponivel";
}
```

Retornar `"aberta"` produziria erro porque não pertence à union.

## 9. Union com tipos diferentes

Uma busca pode receber id ou título:

```ts
function buscarOficina(termo: number | string): Oficina | undefined {
  if (typeof termo === "number") {
    return oficinas.find((oficina) => oficina.id === termo);
  }

  const termoNormalizado = termo.toLowerCase();

  return oficinas.find((oficina) =>
    oficina.titulo.toLowerCase().includes(termoNormalizado),
  );
}
```

O teste `typeof` realiza narrowing. O retorno inclui `undefined` porque a busca pode falhar.

## 10. Tratar undefined

```ts
const encontrada = buscarOficina(99);
```

Refine antes de acessar:

```ts
if (encontrada === undefined) {
  console.log("Oficina não encontrada.");
} else {
  console.log(encontrada.titulo);
}
```

Optional chaining serve para um valor opcional:

```ts
console.log(encontrada?.titulo);
```

Ele não substitui a comunicação do estado “não encontrado”.

## 11. Union discriminada

Uma inscrição pode ter estados diferentes:

```ts
type ResultadoInscricao =
  | { tipo: "sucesso"; oficina: Oficina; vagasRestantes: number }
  | { tipo: "erro"; mensagem: string };
```

Crie a operação:

```ts
function inscrever(oficina: Oficina): ResultadoInscricao {
  if (!possuiVagas(oficina)) {
    return {
      tipo: "erro",
      mensagem: "Não há vaga disponível.",
    };
  }

  return {
    tipo: "sucesso",
    oficina,
    vagasRestantes: calcularVagas(oficina) - 1,
  };
}
```

Refine pela propriedade:

```ts
const resultado = inscrever(oficinas[0]);

if (resultado.tipo === "sucesso") {
  console.log(resultado.vagasRestantes);
} else {
  console.log(resultado.mensagem);
}
```

O compilador conhece as propriedades de cada caminho.

## 12. Atualização sem mutação

```ts
function adicionarInscricao(oficina: Oficina): Oficina {
  return {
    ...oficina,
    inscritos: oficina.inscritos + 1,
  };
}
```

Atualize a coleção:

```ts
const oficinaAtualizada = adicionarInscricao(oficinas[0]);

const catalogoAtualizado = oficinas.map((oficina) =>
  oficina.id === oficinaAtualizada.id ? oficinaAtualizada : oficina,
);
```

## 13. Introdução a generics

Uma função para obter o primeiro item não precisa conhecer o tipo específico:

```ts
function obterPrimeiro<T>(itens: T[]): T | undefined {
  return itens[0];
}
```

Use com coleções diferentes:

```ts
const primeiraOficina = obterPrimeiro(oficinas);
const primeiraNota = obterPrimeiro([8, 9, 10]);
```

Os retornos são `Oficina | undefined` e `number | undefined`. O generic preserva a relação entre entrada e saída sem `any`.

## 14. Prática guiada

Modele um catálogo de eventos:

1. crie uma `interface Evento`;
2. use `readonly` no id;
3. inclua propriedade opcional;
4. crie uma union para três estados;
5. armazene três eventos;
6. crie funções de disponibilidade e busca;
7. trate `undefined`;
8. atualize sem mutação.

Verifique:

```bash
docker compose exec typescript npm run check
```

## 15. Exercício aplicado

Escolha livros, cursos, filmes ou tarefas e implemente:

- entidade com cinco propriedades;
- uma propriedade `readonly`;
- uma propriedade opcional;
- array com três objetos;
- union de três estados;
- busca com retorno opcional;
- narrowing;
- atualização sem mutação;
- função genérica simples.

## 16. Erros comuns

- usar propriedade opcional como obrigatória;
- usar `as` apenas para silenciar o compilador;
- criar unions sem narrowing;
- esquecer que `find` pode retornar `undefined`;
- alterar propriedade `readonly`;
- duplicar `type` e `interface` com o mesmo nome;
- usar `any` onde um generic bastaria;
- confundir `readonly` com imutabilidade profunda.

## 17. Critérios de aceite

- a entidade possui contrato nomeado;
- propriedades obrigatórias e opcionais são coerentes;
- ids usam `readonly`;
- estados são limitados por union;
- buscas tratam `undefined`;
- narrowing precede propriedades específicas;
- atualizações não modificam o original;
- o projeto passa em `npm run check`;
- não há uso injustificado de `any` ou asserções.

## Checklist de compreensão

- [ ] Modelo objetos com `type` ou `interface`.
- [ ] Distingo propriedade obrigatória de opcional.
- [ ] Explico o limite de `readonly`.
- [ ] Crio unions literais.
- [ ] Uso `typeof` para narrowing.
- [ ] Trato `undefined`.
- [ ] Uso uma propriedade discriminante.
- [ ] Atualizo objetos com spread.
- [ ] Entendo um generic simples.

## Questões de fixação

1. Quando um contrato nomeado melhora o código?
2. O que `?` significa em uma propriedade?
3. `readonly` impede alterações no JavaScript executado?
4. Qual vantagem uma union literal possui sobre `string`?
5. Como `typeof` refina uma union?
6. Por que `find` inclui `undefined`?
7. O que torna uma union discriminada?
8. Por que `T` é mais seguro que `any`?

## Referências

- [TypeScript — Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html)
- [TypeScript — Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
- [TypeScript — Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)
- [TypeScript — Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html)

[Voltar ao cronograma](../01-cronograma-60h.md)
