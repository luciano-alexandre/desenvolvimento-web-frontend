# Encontro 29 — RxJS essencial

**Carga horária:** 1,5h  
**Entrega prevista:** Fluxo assíncrono

## Visão Geral

Este encontro desenvolve **rxjs essencial** como continuidade direta dos conhecimentos anteriores. A aula parte de um problema observável, apresenta os recursos necessários e termina com uma entrega que pode ser executada e verificada.

Ao final, o estudante deverá conseguir explicar o propósito de cada recurso, implementar uma solução incremental, testar o comportamento e justificar as decisões adotadas.

## Conceitos Essenciais

- Observable.
- Pipe.
- Map.
- SwitchMap.
- CatchError.

## 1) Contexto do encontro

Uma interface de qualidade precisa combinar estrutura, comportamento, apresentação, acessibilidade e manutenção. O tema deste encontro não deve ser aprendido como uma lista de comandos isolados, mas como resposta a um problema concreto de desenvolvimento frontend.

Durante a aula, use quatro perguntas para orientar as decisões:

- qual problema precisa ser resolvido?
- em que parte do projeto fica essa responsabilidade?
- como verificar se a solução funciona?
- que impacto a escolha produz para usuários e manutenção?

## 2) Conceitos em detalhe

### 1) Observable

Observable deve ser identificado no exemplo, aplicado na prática e relacionado ao resultado observado. Compare uma versão incompleta com a versão corrigida, explique a sintaxe relevante e registre quando esse recurso deve ou não ser utilizado.

### 2) Pipe

Pipe deve ser identificado no exemplo, aplicado na prática e relacionado ao resultado observado. Compare uma versão incompleta com a versão corrigida, explique a sintaxe relevante e registre quando esse recurso deve ou não ser utilizado.

### 3) Map

Map deve ser identificado no exemplo, aplicado na prática e relacionado ao resultado observado. Compare uma versão incompleta com a versão corrigida, explique a sintaxe relevante e registre quando esse recurso deve ou não ser utilizado.

### 4) SwitchMap

SwitchMap deve ser identificado no exemplo, aplicado na prática e relacionado ao resultado observado. Compare uma versão incompleta com a versão corrigida, explique a sintaxe relevante e registre quando esse recurso deve ou não ser utilizado.

### 5) CatchError

CatchError deve ser identificado no exemplo, aplicado na prática e relacionado ao resultado observado. Compare uma versão incompleta com a versão corrigida, explique a sintaxe relevante e registre quando esse recurso deve ou não ser utilizado.

## 3) Exemplo inicial

Digite e execute o exemplo antes de modificá-lo. Depois, altere um elemento por vez e observe o efeito no navegador, no terminal ou nos testes.

```ts
resultados$ = this.termo$.pipe(
  debounceTime(300),
  distinctUntilChanged(),
  switchMap((termo) => this.api.buscar(termo)),
  catchError(() => of([])),
);
```

### O que observar

- localize entrada, transformação e saída;
- relacione cada linha aos conceitos essenciais;
- provoque um erro intencional e interprete a mensagem;
- confirme o resultado com as ferramentas adequadas;
- evite copiar o trecho sem compreender suas partes.

## 4) Demonstração orientada

1. apresente o requisito antes da solução;
2. construa a menor versão funcional;
3. inspecione o resultado e verbalize as decisões;
4. introduza os conceitos progressivamente;
5. teste um cenário alternativo ou de erro;
6. refatore nomes, estrutura e repetição;
7. registre a versão estável.

## 5) Prática guiada

**Proposta:** Criar busca que cancela requisições obsoletas.

### Etapas

1. crie uma pasta ou branch para o encontro;
2. reproduza o exemplo e confirme que ele funciona;
3. adapte nomes, conteúdo e dados ao domínio escolhido;
4. aplique os cinco conceitos essenciais;
5. teste diferentes larguras e estados aplicáveis;
6. revise console, compilação, teclado e foco;
7. prepare a entrega indicada no início da página.

## 6) Exercício aplicado

Construa uma segunda variação sem acompanhar o exemplo linha a linha. A solução deve ser autoral e compreensível para outra pessoa.

### Requisitos mínimos

- demonstrar uso consciente de Observable;
- demonstrar uso consciente de pipe;
- demonstrar uso consciente de map;
- demonstrar uso consciente de switchMap;
- demonstrar uso consciente de catchError;
- manter nomes claros e organização consistente;
- não apresentar erros de compilação ou console;
- explicar no README como executar e testar;
- registrar evidências e decisões importantes.

### Desafio adicional

Implemente um estado alternativo relevante, como vazio, erro, carregamento, tela estreita ou navegação por teclado. Explique como a solução permanece utilizável nessa condição.

## 7) Critérios de aceite

- o projeto executa conforme as instruções;
- o resultado atende ao objetivo funcional;
- os recursos do encontro foram usados com intenção;
- a interface funciona nos cenários testados;
- a entrega está organizada e pode ser avaliada sem ajustes;
- o histórico ou registro de trabalho evidencia evolução incremental.

## 8) Erros comuns

- começar pela aparência sem interpretar o requisito;
- copiar o exemplo sem adaptar semântica e dados;
- reunir responsabilidades diferentes no mesmo bloco;
- testar apenas o caminho de sucesso;
- ignorar mensagens do console ou compilador;
- abstrair antes de existir repetição real;
- entregar sem instruções de execução.

## 9) Materiais para aprofundamento

- [MDN Web Docs](https://developer.mozilla.org/pt-BR/docs/Web)
- [Documentação do Tailwind CSS](https://tailwindcss.com/docs)
- [Documentação do Angular](https://angular.dev/overview)
- [Manual do TypeScript](https://www.typescriptlang.org/docs/handbook/intro.html)

## Checklist de compreensão

- [ ] Consigo explicar e aplicar **Observable**.
- [ ] Consigo explicar e aplicar **pipe**.
- [ ] Consigo explicar e aplicar **map**.
- [ ] Consigo explicar e aplicar **switchMap**.
- [ ] Consigo explicar e aplicar **catchError**.
- [ ] Consigo executar e modificar o exemplo.
- [ ] Consigo realizar a prática sem cópia integral.
- [ ] Consigo identificar um erro e explicar a correção.
- [ ] Revisei a entrega pelos critérios de aceite.

## Resumo final

Neste encontro, **rxjs essencial** foi tratado como parte de uma solução frontend completa. Conceitos, código, validação e comunicação técnica foram combinados para gerar um resultado reutilizável nos encontros seguintes e no projeto final.

## Questões de fixação

1. Como **Observable** contribui para a solução desenvolvida?
<!-- Gabarito: definir Observable, indicar sua finalidade e relacioná-lo ao exemplo e à prática. -->

2. Como **pipe** contribui para a solução desenvolvida?
<!-- Gabarito: definir pipe, indicar sua finalidade e relacioná-lo ao exemplo e à prática. -->

3. Como **map** contribui para a solução desenvolvida?
<!-- Gabarito: definir map, indicar sua finalidade e relacioná-lo ao exemplo e à prática. -->

4. Como **switchMap** contribui para a solução desenvolvida?
<!-- Gabarito: definir switchMap, indicar sua finalidade e relacioná-lo ao exemplo e à prática. -->

5. Como **catchError** contribui para a solução desenvolvida?
<!-- Gabarito: definir catchError, indicar sua finalidade e relacioná-lo ao exemplo e à prática. -->

[Voltar ao cronograma](../01-cronograma-60h.md)
