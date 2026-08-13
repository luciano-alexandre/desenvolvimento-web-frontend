# Encontro 3 — Git, GitHub e DevTools

**Entrega prevista:** correções validadas e versionadas

## Visão geral

Este encontro usa a página revisada na oficina anterior para consolidar um fluxo profissional de investigação e versionamento. DevTools fornece evidências sobre o comportamento da interface; Git registra mudanças coerentes; GitHub apoia compartilhamento e revisão.

## Objetivos de aprendizagem

- investigar HTML e CSS com os painéis do DevTools;
- reproduzir e documentar problemas de interface;
- conferir alterações antes de adicioná-las ao histórico;
- separar correções por finalidade;
- escrever commits que descrevam resultados;
- publicar uma branch e preparar a revisão no GitHub.

## 1. Investigação com DevTools

Use o painel **Elements** para:

- conferir a árvore e a ordem dos elementos;
- editar classes e declarações temporariamente;
- identificar regras sobrescritas;
- inspecionar estilos computados e box model;
- localizar o elemento que provoca overflow;
- verificar estados como `:hover` e `:focus-visible`.

Na simulação responsiva:

- redimensione continuamente, além de usar presets;
- teste orientação e zoom;
- emule preferências de cor e redução de movimento;
- confirme o comportamento em larguras intermediárias;
- registre dimensão, ação e resultado de cada teste.

Alterações feitas no DevTools são experimentais. Depois de confirmar uma hipótese, aplique a correção no arquivo-fonte e repita o teste.

## 2. Preparar o repositório

```bash
git status
git switch -c revisao-interface
git diff
```

`git status` mostra a situação do diretório de trabalho. A branch isola a atividade. `git diff` permite revisar o conteúdo real das alterações antes do commit.

Arquivos gerados, dependências e configurações locais não devem ser adicionados por acidente. Confira o `.gitignore` e o diff antes de preparar cada mudança.

## 3. Commits por finalidade

Separe correções que podem ser explicadas e verificadas de modo independente:

```bash
git add index.html
git commit -m "Corrige semantica e rotulos do formulario"

git add styles.css
git commit -m "Adapta grade para telas estreitas"

git add styles.css
git commit -m "Torna foco dos controles visivel"
```

Uma mensagem de commit deve indicar o resultado, e não apenas o arquivo modificado. Antes de cada commit:

1. execute `git diff`;
2. confirme que as mudanças possuem a mesma finalidade;
3. teste o comportamento afetado;
4. prepare apenas os arquivos pertinentes;
5. confira o diff preparado com `git diff --staged`.

## 4. Histórico e correções

```bash
git log --oneline --decorate -5
git show --stat HEAD
```

O histórico deve permitir compreender a evolução da solução. Se algo ainda não foi commitado, corrija o arquivo normalmente. Evite reescrever ou apagar histórico compartilhado sem compreender o impacto.

## 5. Compartilhamento no GitHub

```bash
git push -u origin revisao-interface
```

Ao preparar uma revisão, informe:

- problema observado;
- mudança implementada;
- passos de validação;
- larguras e formas de interação testadas;
- limitações ou pontos ainda pendentes.

Capturas podem complementar a explicação visual, mas não substituem passos reproduzíveis nem testes por teclado.

## 6. Atividade de aplicação

Retome a entrega do Encontro 2 e:

1. reproduza três problemas ou riscos no DevTools;
2. registre a evidência de cada um;
3. implemente as correções nos arquivos-fonte;
4. valide novamente os cenários afetados;
5. organize as mudanças em pelo menos dois commits coerentes;
6. publique a branch no GitHub;
7. produza uma descrição curta para revisão.

## Registro de validação

| Problema | Evidência no DevTools | Correção | Teste posterior | Commit |
|---|---|---|---|---|
|  |  |  |  |  |
|  |  |  |  |  |
|  |  |  |  |  |

## Erros frequentes

- modificar arquivos sem conferir `git diff`;
- agrupar mudanças sem relação em um único commit;
- usar mensagens como "ajustes" ou "correções" sem indicar resultado;
- tratar uma alteração temporária no DevTools como solução persistida;
- validar somente com presets de dispositivos;
- adicionar arquivos gerados ou dados locais ao repositório;
- publicar sem repetir os testes depois da correção.

## Checklist de compreensão

- [ ] Uso DevTools para formular e confirmar hipóteses.
- [ ] Distingo experimentos no navegador de alterações no código-fonte.
- [ ] Confiro diffs antes de preparar e registrar mudanças.
- [ ] Separo commits por finalidade.
- [ ] Escrevo mensagens que descrevem resultados.
- [ ] Registro passos de validação reproduzíveis.
- [ ] Publico uma branch sem incluir arquivos indevidos.

## Referências

- [Chrome DevTools](https://developer.chrome.com/docs/devtools/)
- [Git — documentação](https://git-scm.com/doc)
- [GitHub Docs — pull requests](https://docs.github.com/en/pull-requests)

[Voltar ao cronograma](../01-cronograma-60h.md)
