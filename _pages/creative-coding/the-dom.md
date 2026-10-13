---
title: O DOM
---
Vamos agora olhar as funções do p5js para acessar e manipular o DOM.

Antes de começarmos, pode ser útil revisar nosso material sobre [Navegador e JavaScript](../../intro/javascript/).

Primeiro, o DOM (Document Object Model, ou Modelo de Objetos do Documento) é apenas a forma oficial de se referir à coleção de elementos HTML que compõem uma página web. São os elementos `<p>`, `<img>` e `<a>`, que guardam texto, imagens e links, e também alguns elementos mais complexos, como os elementos `<canvas>`, onde nosso sketch p5.js é desenhado, ou os elementos `<video>` e `<audio>`, para exibir diferentes tipos de mídia em uma página web.

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/dom-00.jpg' | relative_url }}"/>
</div>

Normalmente, esses elementos e seu conteúdo são definidos no arquivo html da página web, e qualquer comportamento especial é especificado usando um arquivo JavaScript separado. Podemos ter um botão definido no nosso arquivo html e então um arquivo JavaScript especificaria o que acontece quando esse botão é clicado.

Em algum lugar do html poderíamos ter algo assim:
```html
<button onclick="displayTime()">Display Time</button>
```

E então em um arquivo JavaScript:
```js
function displayTime() {
  let time = new Date();
  alert(time);
}
```

Neste exemplo, a função `displayTime()` usa a função `alert()` do JavaScript para mostrar um popup com a data e hora atuais. Não se preocupe com os detalhes; é só um exemplo simples de html com JavaScript para mostrar que, tradicionalmente, o html define o *quê* das nossas páginas web e o JavaScript define o *como*.

Elementos de uma página web que são *estáticos*, ou seja, que não precisam se mover ou mudar, serão definidos no html. Elementos que são criados com base na interação do usuário podem ser criados no arquivo JavaScript e adicionados ao DOM da página web *dinamicamente*.

Hoje em dia ficou mais comum usar frameworks como [Angular](https://angular.io/), [React](https://react.dev/), [Vue](https://vuejs.org/) ou [Svelte](https://svelte.dev/) para criar páginas web com elementos que têm comportamentos mais complexos, como enviar formulários, acessar bancos de dados, validar senhas, etc.

E, embora o p5.js não seja a melhor ferramenta para criar esses tipos de sites e interações, ele nos oferece um [monte de funções](https://p5js.org/reference/#group-DOM) para criar e manipular elementos html na página.

Uma coisa importante a notar é que o código do nosso sketch é desenhado em um elemento `<canvas>` que faz parte da nossa página html. Todos os elementos que adicionamos com as funções de DOM do p5.js são adicionados à nossa página html, e *NÃO* ao nosso canvas. São dois mundos separados, mas conectados. E embora possamos fazer elementos do DOM aparecerem por cima do nosso canvas, eles não estão *dentro* do nosso canvas.

Tenha isso em mente enquanto passamos por esses exemplos e começamos a usar elementos do DOM nos nossos sketches.

## Links e Texto
Podemos adicionar links ao nosso sketch assim:

{% include p5-editor.html id="IhgQ06zCM" %}

E podemos usar nossas funções `draw()` e `mouseClick()` do p5.js para calcular dinamicamente a posição de um link, ou para continuar adicionando links dinamicamente conforme clicamos:

{% include p5-editor.html id="cVl54_Chi" %}

Isso é bom e divertido, mas a menos que haja uma boa razão para criar esses tipos de elementos estáticos dinamicamente no nosso p5js, o melhor é defini-los e posicioná-los no arquivo html, e estilizá-los com css.

Criar elementos para exibir conteúdo dinâmico que vem do sketch p5.js pode ser mais útil. Por exemplo, podemos criar elementos para exibir a posição do nosso mouse e a taxa de quadros atual:

{% include p5-editor.html id="zpsAqlRi8" %}

A mesma coisa pode ser obtida com a função `text()` do p5.js, mas o legal de usar um [`p5.Element`](https://p5js.org/reference/#/p5.Element) é que é fácil detectar interações nesses elementos associando listeners aos seus eventos `mousePressed()`, `mouseOver()`, etc:

{% include p5-editor.html id="OiLGOTucx" %}

## Entradas
Os [`p5.Elements`](https://p5js.org/reference/#/p5.Element) se tornam realmente úteis em situações em que queremos criar alguns elementos simples de entrada do usuário.

Podemos usar a função `createSlider()` para criar um elemento html `<input>` do tipo `range`. Então, no `draw()`, usamos a função `value()` do objeto para acessar seu valor atual e usá-lo para mudar as cores do fundo e do texto.

{% include p5-editor.html id="v7Y9dy88s" %}

Também podemos criar elementos html `<button>` de forma parecida e, como vimos acima com os elementos `<p>`, associar uma função ao seu evento `mouseClicked()`:

{% include p5-editor.html id="ozXc5GHd4" %}

Podemos combinar nosso botão com os sliders de modo que, toda vez que o botão for clicado, os sliders sejam atualizados com os novos valores RGB do fundo:

{% include p5-editor.html id="GV0s_G5CA" %}

A forma mais correta de fazer isso em 2024 é usando um elemento `<input>` do tipo `color`, que cria um seletor de cores específico do navegador com muitas opções:

{% include p5-editor.html id="SsC8rWpog" %}
