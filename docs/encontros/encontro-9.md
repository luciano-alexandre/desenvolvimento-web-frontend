# Encontro 9 — Estados e dark mode

**Carga horária:** 1,5h  
**Entrega prevista:** Componente interativo

## Visão Geral

Este encontro desenvolve **estados e dark mode** como continuidade direta dos conhecimentos anteriores. A aula parte de um problema observável, apresenta os recursos necessários e termina com uma entrega que pode ser executada e verificada.

Ao final, o estudante deverá conseguir explicar o propósito de cada recurso, implementar uma solução incremental, testar o comportamento e justificar as decisões adotadas.

## Conceitos Essenciais

- Hover.
- Focus-visible.
- Group e peer.
- Dark mode.
- Motion-safe.

## 1) Contexto do encontro

Uma interface de qualidade precisa combinar estrutura, comportamento, apresentação, acessibilidade e manutenção. O tema deste encontro não deve ser aprendido como uma lista de comandos isolados, mas como resposta a um problema concreto de desenvolvimento frontend.

Durante a aula, use quatro perguntas para orientar as decisões:

- qual problema precisa ser resolvido?
- em que parte do projeto fica essa responsabilidade?
- como verificar se a solução funciona?
- que impacto a escolha produz para usuários e manutenção?

## 2) Conceitos em detalhe

### 1) Hover

Hover deve ser identificado no exemplo, aplicado na prática e relacionado ao resultado observado. Compare uma versão incompleta com a versão corrigida, explique a sintaxe relevante e registre quando esse recurso deve ou não ser utilizado.

### 2) Focus-visible

Focus-visible deve ser identificado no exemplo, aplicado na prática e relacionado ao resultado observado. Compare uma versão incompleta com a versão corrigida, explique a sintaxe relevante e registre quando esse recurso deve ou não ser utilizado.

### 3) Group e peer

Group e peer deve ser identificado no exemplo, aplicado na prática e relacionado ao resultado observado. Compare uma versão incompleta com a versão corrigida, explique a sintaxe relevante e registre quando esse recurso deve ou não ser utilizado.

### 4) Dark mode

Dark mode deve ser identificado no exemplo, aplicado na prática e relacionado ao resultado observado. Compare uma versão incompleta com a versão corrigida, explique a sintaxe relevante e registre quando esse recurso deve ou não ser utilizado.

### 5) Motion-safe

Motion-safe deve ser identificado no exemplo, aplicado na prática e relacionado ao resultado observado. Compare uma versão incompleta com a versão corrigida, explique a sintaxe relevante e registre quando esse recurso deve ou não ser utilizado.

## 3) Exemplo inicial

Digite e execute o exemplo antes de modificá-lo. Depois, altere um elemento por vez e observe o efeito no navegador, no terminal ou nos testes.

```html
<article class="rounded-xl border border-slate-200 bg-white p-6 shadow-sm md:p-8">
  <h2 class="text-xl font-bold text-slate-900">Interface responsiva</h2>
  <p class="mt-2 text-slate-600">Componente construído com Tailwind CSS.</p>
</article>
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

**Proposta:** Criar componente interativo, acessível e com variante escura.

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

- demonstrar uso consciente de hover;
- demonstrar uso consciente de focus-visible;
- demonstrar uso consciente de group e peer;
- demonstrar uso consciente de dark mode;
- demonstrar uso consciente de motion-safe;
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

- [ ] Consigo explicar e aplicar **hover**.
- [ ] Consigo explicar e aplicar **focus-visible**.
- [ ] Consigo explicar e aplicar **group e peer**.
- [ ] Consigo explicar e aplicar **dark mode**.
- [ ] Consigo explicar e aplicar **motion-safe**.
- [ ] Consigo executar e modificar o exemplo.
- [ ] Consigo realizar a prática sem cópia integral.
- [ ] Consigo identificar um erro e explicar a correção.
- [ ] Revisei a entrega pelos critérios de aceite.

## Resumo final

Neste encontro, **estados e dark mode** foi tratado como parte de uma solução frontend completa. Conceitos, código, validação e comunicação técnica foram combinados para gerar um resultado reutilizável nos encontros seguintes e no projeto final.

## Questões de fixação

1. Como **hover** contribui para a solução desenvolvida?
<!-- Gabarito: definir hover, indicar sua finalidade e relacioná-lo ao exemplo e à prática. -->

2. Como **focus-visible** contribui para a solução desenvolvida?
<!-- Gabarito: definir focus-visible, indicar sua finalidade e relacioná-lo ao exemplo e à prática. -->

3. Como **group e peer** contribui para a solução desenvolvida?
<!-- Gabarito: definir group e peer, indicar sua finalidade e relacioná-lo ao exemplo e à prática. -->

4. Como **dark mode** contribui para a solução desenvolvida?
<!-- Gabarito: definir dark mode, indicar sua finalidade e relacioná-lo ao exemplo e à prática. -->

5. Como **motion-safe** contribui para a solução desenvolvida?
<!-- Gabarito: definir motion-safe, indicar sua finalidade e relacioná-lo ao exemplo e à prática. -->

[Voltar ao cronograma](../01-cronograma-60h.md)
