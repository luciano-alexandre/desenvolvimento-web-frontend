# Encontro 1 — Continuidade, diagnóstico, HTML semântico e acessibilidade

## Visão geral

Desenvolvimento Web Frontend dá continuidade a Padrões Web. HTML, CSS, acessibilidade, responsividade e versionamento continuam sendo a base sobre a qual Tailwind CSS, TypeScript e Angular serão estudados. Frameworks organizam e aceleram o trabalho, mas não corrigem automaticamente uma estrutura semântica inadequada, um layout frágil ou uma interação inacessível.

Este material apresenta o modelo mental que acompanhará toda a disciplina e oferece um diagnóstico dos conhecimentos prévios. O diagnóstico não procura apenas saber se uma página “abre”, mas se o estudante consegue ler o código, explicar decisões, localizar defeitos e propor melhorias verificáveis.

## Objetivos de aprendizagem

- relacionar HTML, CSS e JavaScript às responsabilidades de uma interface;
- distinguir página estática, interface interativa e aplicação frontend;
- reconhecer semântica, responsividade, acessibilidade e manutenção como requisitos;
- utilizar navegador, DevTools e Git como instrumentos de investigação;
- registrar conhecimentos consolidados e pontos que precisam ser retomados.

## 1. O que muda nesta disciplina

Em Padrões Web, o foco esteve na construção da base: documentos HTML, estilos CSS, layouts, responsividade e publicação. Agora essa base será aplicada a interfaces maiores, compostas por unidades reutilizáveis e conectadas a dados.

```text
HTML semântico      → estrutura e significado
CSS / Tailwind CSS  → apresentação e adaptação visual
JavaScript          → eventos e comportamento
TypeScript          → contratos e verificação de tipos
Angular             → componentes, rotas, serviços e aplicação
Git                 → histórico e colaboração
```

As camadas cooperam, mas não são intercambiáveis. Um botão precisa continuar sendo um elemento `button`; uma classe visual não substitui sua semântica. Da mesma forma, Angular não elimina o HTML, e Tailwind não elimina o CSS: ambos exigem domínio dos fundamentos que abstraem.

## 2. Página, interface e aplicação frontend

Uma **página** apresenta um documento acessado por uma URL. Uma **interface** acrescenta controles e respostas às ações do usuário. Uma **aplicação frontend** organiza diversas interfaces, estados, rotas e fontes de dados em uma arquitetura mantida no navegador.

Uma aplicação pode incluir:

- componentes como cabeçalho, cartão e formulário;
- navegação sem recarregamento completo;
- validação e mensagens de feedback;
- comunicação com APIs;
- estados de carregamento, sucesso, vazio e erro;
- regras de apresentação e comportamento;
- testes e build de produção.

O aumento de complexidade torna importantes a separação de responsabilidades, os contratos de dados e a organização do código.

## 3. Responsabilidades das tecnologias fundamentais

### HTML: estrutura e significado

HTML descreve o conteúdo. Elementos semânticos permitem que navegador, mecanismo de busca, leitor de tela e pessoa desenvolvedora compreendam a organização do documento.

```html
<article>
  <header>
    <h2>Oficina de Angular</h2>
    <p><time datetime="2026-09-15">15 de setembro</time></p>
  </header>
  <p>Introdução à criação de componentes.</p>
  <a href="/oficinas/angular">Consultar detalhes da oficina</a>
</article>
```

`article`, `header`, `time` e o texto descritivo do link comunicam significado. Trocar tudo por `div` pode manter a aparência, mas empobrece a estrutura.

### CSS: apresentação e adaptação

CSS controla tipografia, cor, espaçamento e layout. Uma folha bem organizada trabalha com o conteúdo, em vez de depender de tamanhos fixos.

```css
.oficinas {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 18rem), 1fr));
  gap: 1rem;
}
```

O exemplo distribui cartões conforme o espaço disponível e impede que a largura mínima provoque overflow em telas estreitas.

### JavaScript: comportamento

JavaScript responde a eventos e modifica estado ou conteúdo. O HTML deve oferecer uma base utilizável mesmo antes da camada de comportamento, sempre que isso for possível.

```js
const botao = document.querySelector("#alternar-detalhes");
const detalhes = document.querySelector("#detalhes");

botao.addEventListener("click", () => {
  const aberto = !detalhes.hidden;
  detalhes.hidden = aberto;
  botao.setAttribute("aria-expanded", String(!aberto));
});
```

O estado visual e `aria-expanded` precisam permanecer sincronizados.

## 4. Qualidade frontend como conjunto de critérios

“Funciona no meu computador” é uma verificação insuficiente. Uma solução deve ser analisada por diferentes dimensões:

| Dimensão | Pergunta de verificação |
|---|---|
| Semântica | Os elementos representam corretamente o conteúdo? |
| Funcionalidade | A tarefa principal pode ser concluída? |
| Responsividade | O conteúdo se adapta sem cortes ou overflow? |
| Acessibilidade | Teclado, foco, nomes e contraste estão adequados? |
| Manutenção | Nomes e responsabilidades são compreensíveis? |
| Desempenho | Recursos e scripts são necessários e proporcionais? |
| Confiabilidade | Erros e estados alternativos foram previstos? |
| Versionamento | O histórico explica a evolução da solução? |

Qualidade não é uma etapa final. Uma marcação semântica correta simplifica acessibilidade; componentes bem delimitados favorecem testes; commits pequenos facilitam localizar regressões.

Essas dimensões começam na estrutura do documento. Por isso, a revisão prossegue com HTML semântico e acessibilidade antes de chegar ao diagnóstico integrado.

## 5. HTML semântico e acessibilidade

HTML semântico descreve o significado e as relações do conteúdo. Acessibilidade busca garantir que pessoas com diferentes capacidades, tecnologias assistivas e formas de interação consigam perceber, compreender e operar a interface. Esses dois assuntos fazem parte do diagnóstico porque Angular continuará produzindo HTML no navegador: componentes acessíveis começam com marcação nativa adequada.

### Escolher elementos pelo significado

Elementos que parecem iguais depois do CSS podem oferecer experiências diferentes. Um elemento nativo já possui papel, foco e comportamento reconhecidos pelo navegador.

```html
<!-- Não possui semântica nem teclado automaticamente -->
<div class="botao" onclick="salvar()">Salvar</div>

<!-- Possui papel, foco e ativação por teclado -->
<button type="button">Salvar</button>
```

Escolhas frequentes:

- navegação para outro recurso: `a` com `href`;
- ação local: `button`;
- conjunto independente: `article`;
- região temática identificada: `section`;
- informação complementar: `aside`;
- conjunto de links importantes: `nav`;
- dados tabulares: `table`.

### Estrutura e landmarks

Landmarks permitem localizar grandes regiões da página. Um documento costuma combinar `header`, `nav`, `main`, `aside` e `footer`.

```html
<header>
  <a href="/">Portal Acadêmico</a>
  <nav aria-label="Navegação principal">
    <a href="/cursos" aria-current="page">Cursos</a>
    <a href="/agenda">Agenda</a>
  </nav>
</header>

<main id="conteudo-principal">
  <h1>Cursos disponíveis</h1>
</main>

<footer>
  <nav aria-label="Navegação do rodapé">...</nav>
</footer>
```

Deve existir um único `main` visível. Landmarks repetidos, como duas navegações, precisam de nomes acessíveis que os diferenciem.

### Hierarquia de títulos

Títulos formam o sumário lógico do documento e não devem ser escolhidos por tamanho visual.

```html
<h1>Catálogo de cursos</h1>
<section aria-labelledby="tecnologia">
  <h2 id="tecnologia">Tecnologia</h2>
  <article>
    <h3>Desenvolvimento Web Frontend</h3>
    <p>Curso voltado à construção de interfaces.</p>
  </article>
</section>
```

O `h1` identifica o assunto principal; `h2` divide grandes seções; `h3` representa uma subseção. A aparência deve ser controlada pelo CSS sem alterar essa relação.

### Nomes acessíveis

Tecnologias assistivas interpretam controles por nome, papel, valor e estado. O nome pode vir do texto visível, de `label`, de `alt` ou, quando necessário, de ARIA.

```html
<button type="button" aria-label="Fechar janela">
  <svg aria-hidden="true" viewBox="0 0 24 24">...</svg>
</button>
```

Se o botão já contém texto visível suficiente, `aria-label` é desnecessário. HTML nativo deve ser preferido a recriações com ARIA.

### Imagens

O texto alternativo depende da função da imagem no contexto:

- imagem informativa: descreve a informação relevante;
- imagem funcional: descreve destino ou ação;
- imagem decorativa: utiliza `alt=""`;
- não se repete no `alt` uma legenda já disponível.

```html
<img
  src="laboratorio.jpg"
  alt="Estudantes trabalhando em computadores no laboratório"
  width="960"
  height="640"
/>
```

### Formulários

Placeholders não substituem rótulos. `label` permanece visível, aumenta a área de interação e identifica programaticamente o campo.

```html
<form aria-labelledby="titulo-inscricao">
  <h2 id="titulo-inscricao">Inscrição na oficina</h2>

  <label for="email">E-mail institucional</label>
  <input
    id="email"
    name="email"
    type="email"
    autocomplete="email"
    aria-describedby="ajuda-email"
    required
  />
  <p id="ajuda-email">Exemplo: estudante@escolar.ifrn.edu.br</p>

  <fieldset>
    <legend>Turno preferencial</legend>
    <label><input type="radio" name="turno" value="manha" /> Manhã</label>
    <label><input type="radio" name="turno" value="tarde" /> Tarde</label>
  </fieldset>

  <button type="submit">Enviar inscrição</button>
</form>
```

Mensagens de erro devem indicar o campo, o problema e como corrigi-lo. Cor isolada não é suficiente.

### Navegação por teclado e foco

Uma interface operável por teclado permite percorrer controles em ordem lógica, mantém foco visível e usa os comportamentos nativos de links, botões e campos.

```css
:focus-visible {
  outline: 3px solid #1d4ed8;
  outline-offset: 3px;
}
```

Evite `tabindex` positivo, que cria uma ordem artificial difícil de manter. Um teste básico percorre a página com `Tab` e `Shift + Tab`, ativa controles e confirma que não há armadilhas de foco.

## 6. Erros frequentes

- avaliar apenas a aparência visual;
- confundir ausência de erro no console com qualidade completa;
- usar `div` clicável no lugar de controles nativos;
- aplicar JavaScript para resolver algo disponível em HTML ou CSS;
- testar somente com mouse e em uma largura;
- alterar vários problemas em um único commit genérico;
- propor correções sem registrar como foram validadas.
- usar ARIA para imitar um elemento nativo disponível;
- escolher títulos pelo tamanho visual;
- ocultar o contorno de foco sem substituição visível;
- usar placeholder como único rótulo;
- escrever texto alternativo genérico ou redundante;
- considerar auditoria automática equivalente a teste humano.

## Checklist de compreensão

- [ ] Distingo as responsabilidades de HTML, CSS, JavaScript, TypeScript e Angular.
- [ ] Consigo explicar por que Tailwind e Angular não substituem fundamentos Web.
- [ ] Avalio semântica, funcionalidade, responsividade e acessibilidade.
- [ ] Uso o DevTools para buscar evidências.
- [ ] Escrevo commits que descrevem mudanças coerentes.
- [ ] Registro pontos consolidados e lacunas de aprendizagem.
- [ ] Estruturo landmarks e títulos de forma coerente.
- [ ] Diferencio links, botões e elementos genéricos.
- [ ] Associo rótulos, instruções e erros aos campos.
- [ ] Verifico nomes acessíveis, foco e navegação por teclado.

## Referências

- [MDN — Aprendizado de desenvolvimento Web](https://developer.mozilla.org/pt-BR/docs/Learn_web_development)
- [Chrome DevTools](https://developer.chrome.com/docs/devtools/)
- [Git — documentação](https://git-scm.com/doc)
- [WAI — fundamentos de acessibilidade](https://www.w3.org/WAI/fundamentals/accessibility-intro/)
- [WAI — estrutura de páginas](https://www.w3.org/WAI/tutorials/page-structure/)
- [WAI — formulários](https://www.w3.org/WAI/tutorials/forms/)
- [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)

[Voltar ao cronograma](../01-cronograma-60h.md)
