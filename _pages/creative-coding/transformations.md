---
title: Translações, Rotações e Transformações
---
Agora que cobrimos o básico de JavaScript e construções fundamentais de programação como [variáveis](../../coding/variables/), [loops](../../coding/patterns/), [funções](../../coding/functions/), [arrays](../../coding/arrays/), [objetos](../../coding/objects) e [classes](../../coding/classes), vamos começar a olhar alguns conceitos mais específicos de creative coding e aplicações audiovisuais.

## Translações
Vamos voltar a desenhar com formas simples.

Digamos que temos uma coleção de formas simples que compõem uma forma mais complicada, como uma estrela, um rosto sorridente ou um [coração](../../coding/functions/).

Podemos desenhá-la em uma posição específica especificando todas as coordenadas das nossas formas básicas:

{% include p5-editor.html id="xGwHq_J9j" %}

Se quisermos desenhar essa forma em uma posição diferente, vamos ter que recalcular *manualmente* todas as coordenadas `x` e `y` das formas básicas:

{% include p5-editor.html id="9-CBxr1ca" %}

Podemos usar variáveis, o que ajuda um pouco:

{% include p5-editor.html id="7AeMM-06s" %}

Tudo isso funciona, mas existe outra forma de fazer isso, que pode parecer mais difícil no começo, mas que, depois que nos acostumamos, vai nos permitir realizar outros tipos de transformações nas nossas formas, como rotações e escalas.

Em vez de mudar as posições `x` e `y` de onde queremos desenhar nossa forma no canvas, podemos mudar a posição da origem do nosso canvas. Vamos sempre desenhar nossas formas na posição $$(0, 0)$$, mas vamos mudar onde essa posição está no nosso canvas. Podemos usar a função [`translate()`](https://p5js.org/reference/#/p5/translate) do p5.js para mudar a posição da nossa origem.

{% include p5-editor.html id="5rd2HyU5c" %}

O código que de fato desenha o coração é exatamente o mesmo. Só mudamos onde, no nosso canvas, está o ponto de referência $$(0, 0)$$.

<div class="scaled-images left w100">
  <img src = "{{ '/assets/images/creative-coding/transform-00.jpg' | relative_url }}"/>
</div>

<div class="scaled-images left w100">
  <img src = "{{ '/assets/images/creative-coding/transform-01.jpg' | relative_url }}"/>
</div>

## Empilhando (Push) & Desempilhando (Pop)

Uma coisa a notar é que as chamadas a `translate()` são cumulativas e permanentes, então chamar `translate(150, 150)` duas vezes na verdade move a origem para $$(300, 300)$$ e tudo o que for desenhado depois vai usar a nova posição da origem. É como quando chamamos `fill(0)` e tudo o que é desenhado depois é preenchido de preto.

{% include p5-editor.html id="KmoPjGRfu" %}

Sempre podemos *desfazer* nossas translações usando valores negativos, então chamar `translate(150, 150)` seguido de `translate(-150, -150)` vai mover a origem de volta para o canto superior esquerdo do nosso canvas.

{% include p5-editor.html id="Xc1_4qBVQ" %}

Uma forma mais fácil de fazer isso é usar as funções [`push()`](https://p5js.org/reference/#/p5/push) e [`pop()`](https://p5js.org/reference/#/p5/pop). Quando chamamos `push()`, estamos dizendo ao p5.js para prestar atenção redobrada nas nossas transformações e acompanhá-las para nós. Depois que terminamos de desenhar, chamamos `pop()`, que agora diz ao p5.js para desfazer todas as transformações feitas depois do `push()`. Ou, em outras palavras, a função `push()` diz ao p5.js para *salvar* o estado atual das nossas transformações, e a função `pop()` diz ao p5.js para voltar a esse estado.

Então, em vez de fazer isto para recuperar nossa origem inicial:
```js
translate(150, 150);
// desenha formas
translate(-150, -150);
```

Podemos fazer isto:
```js
push();
translate(150, 150);
// desenha formas
pop();
```

É uma boa prática sempre *envolver* nossas translações e transformações com chamadas a `push()` e `pop()`. Dessa forma, sempre podemos recuperar a posição inicial da nossa origem.

{% include p5-editor.html id="T5i7Fe0NJ" %}

## Rotação

A outra vantagem de usar `translate()` com `push()`/`pop()` é que essa é a forma mais fácil de rotacionar nossas formas personalizadas.

A função [`rotate()`](https://p5js.org/reference/#/p5/rotate) do p5.js rotaciona nosso canvas ao redor da sua origem.

Se quisermos desenhar dois corações rotacionados em $$45^{\circ}$$ em $$(100, 100)$$ e $$(250, 250)$$, podemos tentar:
```js
rotate(PI / 4);
translate(100, 100);
// desenha o primeiro coração
translate(150, 150);
// desenha o segundo coração
```

Mas isso pode não ter o efeito desejado, já que `rotate()` rotaciona o canvas inteiro, e não as nossas formas!

{% include p5-editor.html id="uKQuVIr3U" %}

É importante lembrar disso: todas essas funções de transformação mudam o canvas subjacente, relativo à sua origem atual, e deixam as nossas formas como estão.

<div class="scaled-images left w100">
  <img src = "{{ '/assets/images/creative-coding/transform-00.jpg' | relative_url }}"/>
</div>

<div class="scaled-images left w100">
  <img src = "{{ '/assets/images/creative-coding/transform-02.jpg' | relative_url }}"/>
</div>

Então, se quisermos desenhar um coração rotacionado em $$(100, 100)$$ e outro em $$(250, 250)$$, primeiro transladamos nossa origem, rotacionamos, desenhamos nossas formas, desfazemos todas as transformações e então repetimos na posição diferente:
```js
push();
translate(100, 100);
rotate(PI / 4);
// desenha o primeiro coração
pop();

push();
translate(250, 250);
rotate(PI / 8);
// desenha o segundo coração
pop();
```

{% include p5-editor.html id="jXO-XHvb8" %}

## Mais Transformações

Depois que nos acostumamos com o funcionamento de `translate()`, `push()` e `pop()`, podemos até usá-los em conjunto com [`scale()`](https://p5js.org/reference/#/p5/scale) para mudar o tamanho dos nossos objetos sem precisar recalcular novas coordenadas e comprimentos para as nossas formas básicas.

De novo, só precisamos lembrar que `scale()` está na verdade escalando o nosso canvas e deixando as formas como estão, e que ele funciona relativo à origem, de onde faz nossas formas crescerem (ou encolherem) para longe (ou em direção) da origem. De forma semelhante a como usamos `rotate()`, se quisermos mudar o tamanho de formas que são desenhadas em lugares diferentes do nosso canvas, primeiro temos que usar `translate()` para essas posições, depois `scale()` no canvas e então desenhar:
```js
push();
translate(100, 100);
scale(0.6);
// desenha o coração pequeno
pop();

push();
translate(250, 250);
scale(3.0);
// desenha o coração grande
pop();
```

E como estamos usando `push()` e `pop()`, podemos facilmente combinar chamadas a `scale()` e `rotate()`.

{% include p5-editor.html id="TKHl8dijK" %}

Agora podemos combinar tudo com alguns loops `for()` para criar padrões imprevisíveis em que o código que de fato desenha nossas formas é sempre exatamente o mesmo, mas a posição, o tamanho e a rotação de cada forma são determinados por chamadas a `translate()`, `scale()` e `rotate()`, respectivamente.

{% include p5-editor.html id="RMOpkEKK0" %}
