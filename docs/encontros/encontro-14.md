# Encontro 14 — Refatoração e auditoria de interfaces

**Entrega prevista:** interface Tailwind revisada e documentada

## Visão geral

Refatorar significa melhorar a estrutura interna sem alterar intencionalmente o comportamento esperado. Auditar significa verificar a solução por critérios explícitos e produzir evidências. Depois das duas primeiras atividades práticas, este encontro consolida Tailwind CSS antes da transição para JavaScript e TypeScript.

Uma interface visualmente concluída ainda pode conter repetição, classes conflitantes, problemas de foco, excesso de dependências, responsividade frágil ou conteúdo que não suporta variações. A revisão técnica transforma percepções vagas em problemas reproduzíveis e correções justificadas.

## Objetivos de aprendizagem

- identificar repetição e complexidade desnecessária;
- revisar semântica, responsividade e acessibilidade;
- verificar estados de interação e preferências do usuário;
- analisar o CSS gerado e o build de produção;
- documentar problemas, correções e evidências;
- organizar mudanças em commits coerentes.

## 1. Refatoração orientada por problema

Uma refatoração deve partir de um problema observável. Alterar todo o código apenas por preferência pessoal aumenta risco e dificulta revisão.

| Sintoma | Causa possível | Refatoração possível |
|---|---|---|
| classes repetidas em muitos elementos | padrão visual recorrente | criar componente ou abstração adequada |
| variações inconsistentes | ausência de tokens | consolidar cores, espaçamentos e tipografia no tema |
| sobrescritas frequentes | responsabilidades misturadas | reorganizar estrutura e variantes |
| layout quebra com texto longo | dimensões rígidas | usar limites fluidos e permitir quebra |
| foco invisível | estado não definido ou removido | aplicar variante `focus-visible` adequada |

Nem toda repetição precisa ser abstraída. Um componente deve representar uma unidade reconhecível, com responsabilidade e variações claras.

## 2. Legibilidade de classes utilitárias

Agrupe mentalmente as classes por responsabilidade:

```html
<article
  class="
    grid gap-4
    rounded-xl border border-slate-200 bg-white p-5 shadow-sm
    text-slate-900
    hover:border-blue-400
    focus-within:ring-2 focus-within:ring-blue-600
    dark:border-slate-700 dark:bg-slate-900 dark:text-white
  "
>
  <!-- conteúdo -->
</article>
```

A quebra de linhas pode ajudar durante o estudo, desde que o formatador e o projeto adotem uma convenção consistente. Classes contraditórias, duplicadas ou que não produzem efeito devem ser removidas.

## 3. Auditoria por dimensões

### Estrutura e conteúdo

- landmarks e títulos representam a organização;
- links e botões correspondem à função;
- textos continuam compreensíveis fora do contexto visual;
- imagens possuem alternativa e dimensões.

### Responsividade

- não existe rolagem horizontal indevida;
- texto ampliado não se sobrepõe;
- cartões aceitam conteúdo mais longo;
- breakpoints respondem ao conteúdo;
- tabelas possuem estratégia para telas estreitas.

### Acessibilidade

- todos os controles funcionam por teclado;
- o foco é visível;
- estados não dependem somente de cor;
- rótulos, instruções e erros estão associados;
- movimentos respeitam `prefers-reduced-motion`.

### Qualidade técnica

- build conclui sem erros;
- console não apresenta erros não justificados;
- apenas classes detectáveis pelo Tailwind são utilizadas;
- dependências e arquivos gerados estão organizados;
- README explica instalação, execução e verificação.

## 4. Classes construídas dinamicamente

O Tailwind detecta classes nos arquivos-fonte. Construções parciais podem não ser reconhecidas:

```js
// Evite construir fragmentos que não aparecem completos no código-fonte
const classe = `bg-${cor}-600`;
```

Prefira mapear valores para classes completas:

```js
const variantes = {
  sucesso: "bg-emerald-600 text-white",
  alerta: "bg-amber-400 text-slate-950",
  erro: "bg-red-700 text-white",
};
```

O mesmo princípio será importante em templates Angular: classes completas tornam a geração previsível.

## 5. Matriz de validação

| Cenário | Resultado esperado | Evidência | Situação |
|---|---|---|---|
| viewport estreito | conteúdo sem corte |  |  |
| viewport amplo | hierarquia equilibrada |  |  |
| teclado | todos os controles operáveis |  |  |
| foco | indicador sempre perceptível |  |  |
| tema escuro | contraste e estados preservados |  |  |
| movimento reduzido | animações não essenciais reduzidas |  |  |
| conteúdo extenso | layout permanece íntegro |  |  |
| build | saída sem erro |  |  |

## 6. Relatório de problema e correção

Cada item do relatório deve conter:

```text
Problema:
Condição de reprodução:
Impacto:
Causa identificada:
Correção aplicada:
Evidência após a correção:
Commit:
```

Esse registro diferencia uma correção baseada em evidência de uma alteração meramente estética.

## 7. Atividade de aplicação

Audite a interface produzida na Atividade Prática 2. Selecione problemas de pelo menos três dimensões distintas e gere uma versão revisada.

### Critérios de conclusão

- matriz de validação preenchida;
- correções justificadas por evidências;
- responsividade verificada de forma contínua;
- navegação por teclado e foco revisados;
- build concluído sem erros;
- README atualizado;
- commits separados por finalidade.

## 8. Erros frequentes

- reescrever toda a interface sem definir o problema;
- abstrair um padrão que ocorre apenas uma vez;
- avaliar apenas presets de dispositivos;
- esconder overflow em vez de corrigir a origem;
- ignorar estados de foco, erro e conteúdo vazio;
- confiar exclusivamente em auditorias automáticas;
- alterar aparência e comportamento no mesmo commit sem justificativa.

## Checklist de compreensão

- [ ] Distingo refatoração de alteração de requisito.
- [ ] Localizo repetição e inconsistência no uso de utilitários.
- [ ] Verifico semântica, responsividade e acessibilidade.
- [ ] Evito classes Tailwind construídas por fragmentos dinâmicos.
- [ ] Registro problemas e correções com evidências.
- [ ] Organizo o histórico em commits coerentes.

## Questões de fixação

1. O que caracteriza uma refatoração?
<!-- Gabarito: melhoria da estrutura interna sem alteração intencional do comportamento esperado. -->

2. Por que nem toda repetição deve virar componente?
<!-- Gabarito: abstrações prematuras aumentam complexidade; o padrão precisa ter responsabilidade, recorrência e variações compreensíveis. -->

3. Por que classes Tailwind montadas por fragmentos podem falhar?
<!-- Gabarito: o mecanismo de detecção pode não encontrar no código-fonte a classe completa que precisa gerar. -->

4. O que torna uma auditoria reproduzível?
<!-- Gabarito: critérios explícitos, condição de reprodução, evidências, resultado esperado e registro da correção. -->

5. Por que ferramentas automáticas não bastam para avaliar acessibilidade?
<!-- Gabarito: elas detectam apenas parte dos problemas e não substituem avaliação de conteúdo, fluxo, teclado e experiência humana. -->

## Referências

- [Tailwind CSS — Detecting classes in source files](https://tailwindcss.com/docs/detecting-classes-in-source-files)
- [Tailwind CSS — Reusing styles](https://tailwindcss.com/docs/styling-with-utility-classes#reusing-styles)
- [Chrome Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/)
- [W3C — Evaluating Web Accessibility](https://www.w3.org/WAI/test-evaluate/)

[Voltar ao cronograma](../01-cronograma-60h.md)
