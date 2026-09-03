# Encontro 6 — Atividade Prática 1: interface com Tailwind CSS

**Unidade:** Unidade 1
**Carga horária:** 1,5h
**Modalidade:** individual, com consulta
**Valor:** 20 pontos
**Entrega prevista:** página de campanha comunitária implementada com Tailwind CSS

## Visão geral

Este encontro é uma avaliação prática dos conhecimentos de Tailwind CSS estudados nos encontros 2 a 5. Cada estudante deverá construir individualmente uma interface a partir dos requisitos fornecidos, executar o projeto com Docker Compose e demonstrar que consegue transformar decisões de estrutura e apresentação em classes utilitárias.

A atividade é **com consulta**: poderão ser usados os materiais da disciplina, anotações pessoais, projetos produzidos anteriormente e a documentação oficial das tecnologias. A consulta serve para recuperar sintaxe e confirmar propriedades; a seleção, combinação e explicação das soluções continua sendo responsabilidade de cada estudante.


## 1. Regras da avaliação

- cada estudante deve escrever e entregar seu próprio código;
- não é permitido enviar ou receber arquivos, trechos prontos ou respostas durante a avaliação;
- não é permitido editar o projeto de outra pessoa;
- dúvidas sobre o enunciado devem ser dirigidas ao professor;
- ferramentas externas de geração automática de código não poderão ser utilizadas.

Consultar uma classe na documentação é permitido. Copiar a implementação de outra pessoa não é consulta: é compartilhamento de solução.


## 2. Situação-problema

Uma organização comunitária realizará a campanha **Bairro Verde**, destinada ao plantio de árvores em espaços públicos. Sua tarefa é construir uma página que apresente a campanha, seus indicadores e os pontos de plantio disponíveis.

A interface deve conter:

1. cabeçalho com nome da campanha e navegação;
2. apresentação com categoria, título, descrição e ação principal;
3. resumo com três indicadores;
4. coleção com três pontos de plantio;
5. um selo “Vagas limitadas” sobre um dos cartões;
6. rodapé simples com identificação da organização.

## 3. Conteúdo obrigatório

Utilize os textos abaixo. Você pode corrigir quebras de linha, mas não deve remover informações.

### Cabeçalho

- nome: **Bairro Verde**;
- links: **Sobre**, **Pontos de plantio** e **Orientações**.

### Apresentação

- categoria: **Mutirão comunitário**;
- título: **Uma manhã para transformar os espaços do bairro**;
- descrição: **Participe do plantio coletivo e ajude a criar ruas mais verdes, frescas e acolhedoras.**;
- ação: **Quero participar**.

### Indicadores

| Termo | Valor |
|---|---|
| Data | 26 de setembro |
| Horário | 8h às 12h |
| Meta | 120 árvores |

### Pontos de plantio

| Horário e local | Título | Descrição | Situação |
|---|---|---|---|
| 8h · Praça das Mangueiras | Recuperação da praça | Plantio de espécies nativas nas áreas de convivência. | 18 vagas |
| 9h · Avenida Central | Corredor de sombra | Arborização do percurso entre a escola e o posto de saúde. | Vagas limitadas |
| 10h · Parque do Riacho | Proteção das margens | Reforço da vegetação próxima ao curso d’água. | 12 vagas |

Cada cartão deve conter também o link **Ver orientações**.

### Rodapé

- texto: **Associação Comunitária do Bairro · Projeto Bairro Verde**.

## 4. Preparação do projeto

Crie uma pasta independente chamada `atividade-pratica-01`. O projeto deverá conter:

```text
atividade-pratica-01/
├── .dockerignore
├── compose.yaml
├── Dockerfile
├── package.json
├── package-lock.json
├── README.md
└── src/
    ├── index.html
    ├── input.css
    └── output.css
```

Você pode consultar a configuração feita nos encontros 3 e 5. O projeto deve iniciar com:

```bash
docker compose up
```

Caso a imagem ainda precise ser construída:

```bash
docker compose up --build
```

Antes de estilizar, confirme:

- o contêiner está ativo;
- `src/output.css` foi criado;
- `src/index.html` referencia `./output.css`;
- salvar o HTML provoca nova compilação;
- não existem erros no terminal.

## 5. Etapa A — Estrutura semântica

Construa primeiro o HTML, sem se preocupar com a aparência. A estrutura deverá utilizar:

- `header` para o cabeçalho;
- `nav` com nome acessível para a navegação;
- `main` para o conteúdo principal;
- `section` para a campanha;
- apenas um `h1`;
- `article` e `h2` para cada ponto de plantio;
- `dl`, `dt` e `dd` para os indicadores;
- `time` com `datetime` para data ou horários quando aplicável;
- `footer` para a identificação final;
- elementos `a` com `href` para as ações.

Não substitua botões ou links por `div`. A ordem do HTML deve continuar compreensível sem CSS.

## 6. Etapa B — Identidade visual

Crie uma identidade coerente usando utilitários do Tailwind. Sua solução precisa demonstrar:

- cor de fundo da página;
- cores distintas para texto principal, secundário e destaque;
- título principal com hierarquia evidente;
- altura de linha confortável na descrição;
- largura máxima para evitar linhas excessivamente longas;
- margens e preenchimentos com funções distinguíveis;
- cartões com borda e raio;
- sombra utilizada com moderação;
- ação principal visualmente identificável.

Não existe uma paleta obrigatória. O contraste precisa permitir leitura confortável.

## 7. Etapa C — Flexbox

Use Flexbox em pelo menos duas relações unidimensionais:

- identificação e navegação no cabeçalho;
- grupos de indicadores ou elementos internos de um cartão.

A solução deve demonstrar conscientemente:

- `flex`;
- direção do eixo, explícita quando necessário;
- alinhamento transversal com `items-*`;
- distribuição ou agrupamento no eixo principal;
- intervalo mínimo com `gap-*`;
- quebra com `flex-wrap` quando o conteúdo puder exceder a linha.

O estudante deve conseguir apontar o contêiner e seus filhos diretos. Aplicar `flex` a um elemento sem explicar a relação não garante pontuação integral.

## 8. Etapa D — Grid

Organize os três pontos de plantio com Grid:

- `#pontos-de-plantio` deve ser o contêiner;
- os três `article` devem ser filhos diretos;
- a coleção deve possuir três colunas;
- o intervalo deve ser controlado por `gap`;
- cartões com textos diferentes não podem ser cortados;
- as ações devem manter alinhamento visual coerente.

É permitido usar Flexbox vertical dentro dos cartões. Se utilizar `mt-auto`, explique qual espaço ele absorve e por que `flex-col` é necessário.


## 11. Entrega

Entregue a pasta completa ou o endereço definido pelo professor. O projeto deverá incluir:

- configuração Docker reproduzível;
- arquivos-fonte;
- CSS gerado;
- `README.md`;
- uma captura da interface final.

O README deve informar:

- nome do estudante;
- como iniciar e encerrar o projeto;
- onde está o HTML principal;
- uma decisão em que Flexbox foi escolhido;
- uma decisão em que Grid foi escolhido;
- como o posicionamento do selo foi implementado;
- quais testes foram realizados.

Finalize o serviço com:

```bash
docker compose down
```
