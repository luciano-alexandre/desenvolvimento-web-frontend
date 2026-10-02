# Encontro 15 — Orientação do Projeto em Dupla



## 1. Formação das duplas e escolha do tema

- O projeto deverá ser desenvolvido em dupla.
- Cada dupla escolherá uma das 21 propostas deste documento.
- Os dois integrantes devem participar do planejamento, do código e da apresentação.
- A dupla poderá personalizar nome, textos, cores, ícones, imagens e dados.
- Os cálculos e requisitos técnicos do tema escolhido devem ser mantidos.

A personalização não deve transformar o projeto em outro problema. Por exemplo, uma dupla pode mudar disciplinas, metas e identidade visual do painel de estudos, mas ainda deverá calcular total, média, quantidade de metas atingidas e classificação final.

## 2. Escopo técnico

O projeto deve utilizar somente recursos estudados ou demonstrados até o momento. Bibliotecas de componentes prontas não são necessárias: o objetivo é praticar HTML, Tailwind e JavaScript.

### 2.1 HTML semântico

A página deverá utilizar:

- estrutura completa do HTML5;
- `header`, `nav`, `main`, `section`, `article` e `footer` quando forem semanticamente adequados;
- um único `h1` que identifique a página;
- subtítulos em ordem coerente;
- parágrafos, listas, links e, quando fizer sentido, tabela ou imagem;
- navegação interna com pelo menos três links;
- identificadores nas áreas atualizadas pelo JavaScript;
- textos próprios e relacionados ao tema;
- texto alternativo em imagens informativas;
- rótulos claros para valores, métricas e classificações.

A escolha do elemento deve ser feita pela função do conteúdo, não pela aparência. O Tailwind muda a apresentação, mas não substitui a semântica do HTML.

### 2.2 Tailwind CSS

A interface será estilizada com classes utilitárias no HTML. O projeto deverá demonstrar:

- cores de fundo, texto e borda com contraste legível;
- hierarquia tipográfica com tamanho, peso e altura de linha;
- uso intencional de `padding`, `margin` e `gap`;
- largura máxima e conteúdo centralizado;
- bordas, arredondamento e sombras aplicados com moderação;
- Flexbox em pelo menos uma área, como navegação ou grupo de indicadores;
- Grid em pelo menos uma coleção, como cards ou resultados;
- adaptação para telas estreitas e largas com prefixos responsivos;
- estados de foco visíveis em links e outros elementos interativos;
- consistência visual entre seções e componentes.


### 2.3 JavaScript

Use apenas `src/script.js`, conectado ao HTML:

```html
<script src="./script.js" defer></script>
```

O script deverá apresentar:

- `const` e `let` usados de acordo com a necessidade;
- pelo menos uma `string`, um `number`, um `boolean` e um array;
- operadores aritméticos, de comparação e lógicos;
- pelo menos uma cadeia com `if`, `else if` e `else`;
- pelo menos uma repetição com `for`, `for...of` ou `while`;
- no mínimo três funções com parâmetros e retorno;
- template literals;
- resultados com rótulos claros no Console;
- pelo menos três resultados apresentados no HTML com `textContent`;
- nomes que expressem a responsabilidade de variáveis e funções;
- ausência de erros inesperados no Console.

Os três resultados visuais podem ser, por exemplo, total, média e classificação. Calcular um valor no Console não substitui sua apresentação na interface.

Não são obrigatórios formulários, eventos, objetos, armazenamento, APIs ou bibliotecas JavaScript externas. Esses recursos podem ser utilizados somente quando a dupla compreender a implementação e ela não comprometer os requisitos principais.

## 3. Configuração do projeto com Tailwind e Docker

Reaproveite a configuração utilizada nos encontros anteriores:

```text
nome-do-projeto/
├── compose.yaml
├── Dockerfile
├── package.json
├── package-lock.json
├── .dockerignore
├── src/
│   ├── index.html
│   ├── input.css
│   ├── output.css
│   └── script.js
└── imagens/
    └── arquivos-utilizados
```

## 4. Estrutura mínima da página

Independentemente do tema, o site deverá conter:

1. cabeçalho com nome e apresentação do projeto;
2. menu com pelo menos três links internos;
3. seção que explique o propósito da aplicação;
4. seção com informações, cards ou orientações;
5. seção de resultados calculados pelo JavaScript;
6. seção que apresente ou explique os seis dados processados;
7. rodapé com nomes dos integrantes e turma.


### 4.1 Cabeçalho e navegação

O cabeçalho deve apresentar o nome do projeto e permitir acesso às seções principais. Use Flexbox para organizar os elementos quando houver espaço e permita quebra ou empilhamento em telas menores.

### 4.2 Conteúdo informativo

A apresentação precisa explicar o que os seis valores representam e qual meta, limite ou referência será utilizada. Uma pessoa que não participou do projeto deve compreender os resultados sem consultar o código.

### 4.3 Cards e resultados

Os cards não devem servir apenas como decoração. Cada um precisa comunicar uma informação relevante, como dado de entrada, indicador calculado, dica ou estado final. Use Grid para organizar a coleção e mantenha o mesmo padrão visual entre itens equivalentes.

### 4.4 Rodapé

Informe os integrantes e a turma. Garanta contraste, legibilidade e separação visual em relação ao conteúdo principal.

## 5. Ideias de projeto

As 21 propostas abaixo mantêm a mesma base técnica, mas mudam o domínio, os textos, os valores, a referência usada nas comparações e a identidade visual.

### Ideia 1 — Painel de rotina de estudos

Crie um site para acompanhar as horas estudadas em seis disciplinas. Apresente cards com dicas de organização, a lista de disciplinas, a meta semanal e uma área de indicadores. No JavaScript, armazene as horas em um array, calcule total e média, conte quantas disciplinas atingiram a meta mínima e classifique a rotina como **Excelente**, **Regular** ou **Precisa melhorar**.

Com Tailwind, diferencie disciplinas e indicadores por hierarquia tipográfica, organize os cards em Grid responsivo e destaque a classificação final sem depender somente de cor.

### Ideia 2 — Acompanhamento de leitura

Desenvolva um site para registrar páginas lidas em seis sessões. Inclua apresentação do livro ou desafio, benefícios da leitura, meta por sessão e cards de progresso. O JavaScript deve calcular o total de páginas, a média por sessão, quantas sessões alcançaram a meta e uma classificação como **Leitura intensa**, **Bom ritmo** ou **Ritmo inicial**.

A interface pode usar cards que lembrem fichas de leitura, mantendo contraste, espaçamentos consistentes e uma seção de resumo visual adaptável a telas pequenas.

### Ideia 3 — Controle diário de hidratação

Crie uma página educativa com seis registros fictícios de copos ou mililitros consumidos. Explique a unidade usada e a meta definida pela própria atividade. Calcule consumo total, média por período, quantidade de registros que atingiram a meta e classifique o acompanhamento como **Meta atingida**, **Quase lá** ou **Atenção à hidratação**.

Use tons e ícones apenas como apoio: os resultados precisam permanecer claros por texto. Informe que o projeto é didático e não substitui orientação médica.

### Ideia 4 — Diário de atividades físicas

Produza um site com orientações gerais sobre movimento e o acompanhamento fictício de minutos praticados em seis dias. Calcule total, média diária, quantidade de dias acima de uma meta e classifique a regularidade como **Muito ativa**, **Ativa** ou **Pouco ativa**.

Organize cada dia em um card e apresente os indicadores em uma grade. A interface deve deixar explícito que se trata de um exemplo educacional, não de recomendação profissional.

### Ideia 5 — Planejador de economia pessoal

Monte uma página com dicas de organização financeira e seis valores economizados. Apresente a meta geral, os depósitos considerados e a unidade monetária. Calcule total guardado, média, quantos depósitos atingiram um valor mínimo e a diferença até a meta. Classifique o progresso como **Meta alcançada**, **Em andamento** ou **Início da economia**.

Use Tailwind para diferenciar dados de entrada, indicadores e classificação. Valores monetários precisam de rótulos claros e alinhamento visual consistente.

### Ideia 6 — Monitor de gastos de uma viagem

Crie um site de planejamento de viagem com seis categorias de despesas e seus valores no JavaScript. Explique o orçamento e apresente cada categoria na interface. Calcule gasto total, média, quantas categorias ultrapassaram um limite e o saldo restante. Classifique como **Dentro do orçamento**, **Próximo do limite** ou **Acima do orçamento**.

A coleção de despesas pode usar Grid, enquanto o saldo e a classificação recebem maior destaque tipográfico. Não comunique excesso apenas com vermelho; mantenha uma mensagem textual.

### Ideia 7 — Catálogo de filmes avaliados

Desenvolva uma página com seis filmes ou categorias cinematográficas e suas notas. Apresente critérios de avaliação, pequenos resumos e uma área de recomendação. O script deverá calcular média das notas, quantidade de avaliações acima de uma referência e classificar o catálogo como **Muito recomendado**, **Recomendado** ou **Seleção em revisão**.

Crie cards com estrutura repetida para título, categoria e nota. Use bordas, sombras e espaçamento de modo consistente, preservando a ordem semântica dos títulos.

### Ideia 8 — Painel de músicas de uma playlist

Crie um site que apresente uma playlist temática e a duração, em minutos, de seis músicas. Inclua nome, proposta da seleção e critério usado para considerar uma faixa curta ou longa. Calcule duração total, média, quantidade de faixas em cada condição e classifique a playlist como **Longa**, **Média** ou **Curta**.

Use uma lista ou cards responsivos para as faixas e destaque o tempo total em uma seção de resultados. A aparência pode remeter a uma plataforma musical sem copiar uma marca existente.

### Ideia 9 — Guia de pontos turísticos

Monte um site com informações sobre seis pontos turísticos e use no JavaScript os tempos estimados de visita. Explique o tipo de roteiro e a referência de duração. Calcule tempo total, média, quantos locais exigem mais tempo e classifique como **Dia completo**, **Meio período** ou **Passeio rápido**.

Apresente os locais em cards com nome, descrição e duração. Use uma grade responsiva e mantenha imagens com texto alternativo quando forem informativas.

### Ideia 10 — Organizador de receitas

Crie um site de receitas com seis etapas ou preparações e seus tempos. Inclua ingredientes, orientações, cuidados e rendimento. Calcule tempo total, média por etapa, quantas etapas ultrapassam um limite e classifique a receita como **Demorada**, **Moderada** ou **Rápida**.

Use listas semânticas para ingredientes e etapas. Tailwind deve organizar e hierarquizar o conteúdo sem transformar cada parágrafo em um card desnecessário.

### Ideia 11 — Painel da feira de ciências

Desenvolva uma página para divulgar seis projetos e suas pontuações. Inclua programação, orientações ao público e uma explicação dos critérios. Calcule total e média das notas, quantidade de projetos acima da referência e classifique o resultado como **Destaque**, **Bom desempenho** ou **Em desenvolvimento**.

Organize projetos em Grid e use uma área separada para programação e indicadores. Cada nota deve possuir contexto para não parecer um número isolado.

### Ideia 12 — Campeonato de jogos digitais

Crie um site para apresentar seis rodadas ou partidas e os pontos obtidos por uma equipe. Explique o sistema de pontuação. Calcule total, média por rodada, quantidade de vitórias segundo um limite e classifique como **Campeã**, **Competitiva** ou **Em treinamento**.

A interface pode usar cards de partidas e um painel de placar. Garanta leitura confortável e não use animações ou cores como única forma de informar o resultado.

### Ideia 13 — Campanha de arrecadação solidária

Produza um site para divulgar uma campanha e registrar seis quantidades arrecadadas. Apresente objetivo, itens aceitos, meta e formas de participação. Calcule total, média por coleta, quantidade de coletas acima da meta parcial e classifique como **Meta alcançada**, **Próxima da meta** ou **Precisamos de apoio**.

Destaque formas de participação e resultados com componentes simples. O texto deve ser respeitoso e evitar imagens ou mensagens que exponham pessoas atendidas.

### Ideia 14 — Controle de empréstimos da biblioteca

Crie uma página sobre a biblioteca com seis registros numéricos de empréstimos por categoria ou período. Inclua serviços, cuidados com livros e explicação do intervalo analisado. Calcule total, média, quantidade de registros acima da referência e classifique o movimento como **Alto**, **Moderado** ou **Baixo**.

Use cards ou tabela para apresentar os registros e uma grade para os indicadores. Mantenha o conteúdo acessível mesmo sem elementos decorativos.

### Ideia 15 — Avaliação do cardápio escolar

Monte um site para apresentar seis opções ou dias de cardápio e suas notas fictícias. Explique a escala de avaliação e o período considerado. Calcule média, total das notas, quantidade de avaliações positivas e classifique como **Ótima**, **Boa** ou **Precisa melhorar**.

Organize dias ou opções em cards e apresente os indicadores em uma seção própria. Não inclua recomendações médicas ou nutricionais.

### Ideia 16 — Consumo de energia de uma residência fictícia

Crie uma página educativa sobre economia de energia e seis registros fictícios de consumo. Identifique a unidade, o período e o limite adotado. Calcule total, média, quantidade de períodos acima do limite e classifique como **Econômico**, **Moderado** ou **Elevado**.

Use uma interface sóbria, com boa hierarquia entre orientações, registros e resultados. Deixe claro que os valores são fictícios e destinados ao exercício.

### Ideia 17 — Campanha de coleta seletiva

Desenvolva um site sobre reciclagem com seis registros de materiais coletados. Explique as categorias, a unidade e a meta. Calcule total, média, quantidade de registros que alcançaram a meta e classifique como **Excelente resultado**, **Bom resultado** ou **Vamos ampliar**.

Apresente orientações sobre separação de materiais em conteúdo próprio e os registros em cards ou tabela. Cor deve complementar, e não substituir, os nomes dos materiais.

### Ideia 18 — Progresso em um curso on-line

Crie uma página para acompanhar percentual ou pontos obtidos em seis módulos. Explique os critérios de conclusão e a nota mínima. Calcule total, média, quantidade de módulos concluídos e classifique como **Concluída com destaque**, **Em progresso** ou **Precisa revisar**.

Use cards para módulos e um resumo para os resultados gerais. Garanta que progresso e classificação estejam escritos, sem depender apenas de barras ou cores.

### Ideia 19 — Guia de segurança digital

Produza um site com orientações de segurança digital e seis pontuações fictícias relacionadas a boas práticas. Explique a escala adotada. Calcule total, média, quantidade de práticas em nível seguro e classifique como **Proteção forte**, **Proteção intermediária** ou **Proteção básica**.

Organize as orientações por assunto e diferencie visualmente conteúdo educativo e resultado calculado. Evite solicitar ou exibir dados pessoais reais.

### Ideia 20 — Agenda de eventos do campus

Crie um site para divulgar seis eventos e use no JavaScript suas durações ou quantidades previstas de participantes. Inclua data, local fictício e descrição. Calcule total, média, quantidade de eventos acima da referência e classifique como **Intensa**, **Equilibrada** ou **Compacta**.

Use cards responsivos para os eventos e uma navegação interna para programação, indicadores e orientações. Datas e locais precisam estar associadas claramente aos respectivos eventos.

### Ideia 21 — Cuidados com animais de estimação

Monte uma página educativa sobre uma rotina fictícia de cuidados e seis durações ou pontuações de tarefas. Explique a referência usada. Calcule total, média, quantidade de tarefas que atingiram o planejado e classifique como **Rotina completa**, **Rotina parcial** ou **Rotina a organizar**.

A interface pode organizar tarefas em cards e destacar o resumo da rotina. Informe que o conteúdo é didático e não substitui orientações de profissionais de saúde animal.
