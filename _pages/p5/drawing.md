---
title: "Desenhando: Formas e Cores"
---

Vimos brevemente alguns comandos para desenhar formas no nosso canvas na [seção anterior](../p5-intro/), quando olhamos a organização geral de um projeto p5.js.

Vamos dar uma olhada mais detalhada no nosso canvas, nos comandos que podemos usar para desenhar formas e em como representar cores nos nossos sketches p5.js.

## O Canvas

Primeiro, uma breve introdução ao canvas.

O canvas é a seção da nossa página onde podemos de fato desenhar coisas. Existe um elemento `html` [`<canvas>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) real na nossa página, responsável por exibir nossos desenhos, mas não precisamos nos preocupar em interagir diretamente com ele. Quando usamos o comando [`createCanvas()`](https://p5js.org/reference/#/p5/createCanvas), o p5.js cuida automaticamente de criar e posicionar esse elemento `<canvas>` na nossa página.

O comando `createCanvas()` recebe dois parâmetros, ou seja, dois números, que especificam a largura (`width`) e a altura (`height`) da nossa área de desenho. Sempre podemos dar a ele valores específicos em pixels, como: `createCanvas(640, 480)` ou `createCanvas(1920, 1080)`, mas se quisermos que nosso canvas seja proporcional à janela do navegador, podemos usar as palavras-chave especiais do p5.js `windowWidth` e `windowHeight` para fazer o canvas ocupar o máximo de espaço possível na nossa página: `createCanvas(windowWidth, windowHeight)`.

Podemos ver a diferença executando os dois sketches a seguir:

{% include p5-editor.html id="AHvLeXMJM" %}
{% include p5-editor.html id="FJCJwnz7V" %}


E, seja nosso canvas criado com dimensões específicas em pixels ou usando `(windowWidth, windowHeight)`, sempre podemos perguntar ao p5.js o tamanho exato do nosso canvas acessando as *variáveis* [`width`](https://p5js.org/reference/#/p5/width) e [`height`](https://p5js.org/reference/#/p5/height).

(feche qualquer aviso de cookies e olhe a seção *Console* depois de executar o sketch abaixo)


<div class="editor-block-wrapper">
  <div class="p5-editor-wrapper editor-wrapper">
    <iframe class="editor" src="https://editor.p5js.org/shfitz/sketches/M45a9yw5w"></iframe>
  </div>
  <a class="editor-link" href="https://editor.p5js.org/shfitz/sketches/M45a9yw5w">
    abrir exemplo em uma nova janela
  </a>
</div>

## Sistema de Coordenadas

Antes de começarmos a desenhar, precisamos entender como o canvas está orientado e como especificar localizações dentro dos seus limites.

Assim como o `createCanvas()` exigiu dois números, $$(width, height)$$, para definir o tamanho do nosso canvas, especificar localizações no canvas também exige dois números, ou coordenadas, $$(x, y)$$. Isso acontece porque, no p5.js, nosso canvas é um [plano cartesiano](https://en.wikipedia.org/wiki/Cartesian_coordinate_system#Two_dimensions) bidimensional, em que a primeira dimensão representa a distância horizontal a partir da origem, e a segunda dimensão a distância vertical. Diferente do sistema de coordenadas cartesiano tradicional da geometria, no p5.js, e na maioria dos outros contextos de computação gráfica, a origem do nosso canvas fica no canto superior esquerdo e a dimensão vertical cresce para baixo.

<div class="scaled-images">
  <img src="{{ '/assets/images/p5/canvas-00.jpg' | relative_url }}">
</div>

E agora, as variáveis [`width`](https://p5js.org/reference/#/p5/width) e [`height`](https://p5js.org/reference/#/p5/height) do p5.js podem ser muito úteis quando queremos especificar posições relativas ao tamanho geral do nosso canvas. Por exemplo, o pixel que está exatamente no centro do nosso canvas sempre pode ser especificado com as coordenadas $$(\frac{width}{2}, \frac{height}{2})$$, independentemente do tamanho exato do canvas.

Da mesma forma, o pixel mais distante da origem tem coordenadas $$(width - 1, height - 1)$$. O $$-1$$ é necessário porque, embora nosso canvas tenha $$width$$ pixels de largura e $$height$$ pixels de altura, temos um pixel em $$(0, 0)$$, e se o primeiro pixel ao longo da direção $$x$$ está na coordenada $$0$$, o segundo pixel na coordenada $$1$$, ..., etc, ..., o último pixel estará na coordenada $$width - 1$$.

<div class="scaled-images">
  <img src="{{ '/assets/images/p5/canvas-01.jpg' | relative_url }}">
</div>

## Desenhando Formas

Agora que sabemos usar coordenadas para especificar localizações no nosso canvas, podemos começar a desenhar.

Os comandos [`rect()`](https://p5js.org/reference/#/p5/rect) e [`ellipse()`](https://p5js.org/reference/#/p5/ellipse) do p5.js podem ser usados para desenhar retângulos e elipses, respectivamente. Eles são muito parecidos em vários aspectos, mas também têm algumas diferenças que vale a pena notar.

Na sua forma mais simples, ambos recebem $$3$$ parâmetros: `x-location` (posição x), `y-location` (posição y) e `size` (tamanho).

```js
rect(10, 10, 80);
ellipse(200, 200, 100);
```

Se quisermos que as formas tenham proporções diferentes, basta usar um quarto parâmetro para a altura (`height`) da forma:

```js
rect(10, 100, 80, 40);
ellipse(200, 300, 100);
```

{% include p5-editor.html id="TyHTKL3db" %}

Podemos brincar com as coordenadas e os tamanhos no sketch acima ☝️ para ganhar familiaridade e intuição sobre o sistema de coordenadas e essas duas funções.

Agora, algumas das diferenças entre `rect()` e `ellipse()`. Digamos que queremos desenhar uma elipse à direita de um retângulo. Eles ficarão lado a lado, na mesma posição vertical, então poderíamos tentar algo assim:

```js
rect(210, 300, 80);
ellipse(310, 300, 80);
```

{% include p5-editor.html id="2iZYx1nuv" %}

# 🤔

Embora os primeiros $$2$$ parâmetros de `rect()` e `ellipse()` especifiquem coordenadas `x` e `y`, o que eles significam é diferente. Para o `rect()`, especificamos o canto superior esquerdo da nossa forma, e para o `ellipse()` especificamos o seu centro.

Desenhá-los lado a lado exige alguns ajustes nas coordenadas. Podemos deslocar a posição `x` e `y` da elipse pela metade do seu diâmetro:

```js
rect(210, 300, 80);
ellipse(350, 340, 80);
```

{% include p5-editor.html id="KiSnvsQhf" %}

Também podemos usar as funções [`rectMode()`](https://p5js.org/reference/#/p5/rectMode) e [`ellipseMode()`](https://p5js.org/reference/#/p5/ellipseMode) do p5.js para mudar como os retângulos e as elipses são desenhados.

Para desenhar retângulos especificando sua posição central, podemos usar
```js
rectMode(CENTER);
```

Para desenhar elipses especificando seu canto superior esquerdo, podemos usar:
```js
ellipseMode(CORNER);
```

{% include p5-editor.html id="3frUheLXu" %}

Uma coisa a notar é que, depois que chamamos `rectMode()` ou `ellipseMode()`, toda forma que desenharmos em seguida será desenhada usando o modo especificado. Para desfazer isso, podemos chamar:

```js
rectMode(CORNER);
ellipseMode(CENTER);
```

{% include p5-editor.html id="WTddwWpvG" %}


Ou, melhor ainda, podemos simplesmente escolher um modo no início, o que acharmos mais útil para o nosso sketch, e mantê-lo durante todo o sketch.

Digamos que queremos desenhar uma grade de quadrados, retângulos e círculos. Nessa situação, em que começamos no canto superior esquerdo do nosso canvas e desenhamos para a direita e para baixo, pode ser mais fácil fazer as contas para as posições dos cantos superiores esquerdos das nossas formas. Como manteremos o mesmo modo durante todo o sketch, podemos simplesmente colocar `ellipseMode(CORNER)` dentro da nossa função `setup()`.

{% include p5-editor.html id="NL-wqSSL1" %}

Mas, por outro lado, se estivermos desenhando formas concêntricas, ou posicionando-as em relação ao centro do canvas, pode ser mais fácil usar `rectMode(CENTER)` durante todo o sketch:

{% include p5-editor.html id="hmROElyh4" %}

## Mais Formas

O p5.js tem comandos para várias [outras formas](https://p5js.org/reference/#group-Shape) além de retângulos e elipses.

A função [`quad()`](https://p5js.org/reference/#/p5/quad) pode ser usada para desenhar quadriláteros que não são retângulos, especificando $$4$$ pares de coordenadas `x` e `y`.

De forma semelhante, a função [`triangle()`](https://p5js.org/reference/#/p5/triangle) desenha um triângulo a partir de $$3$$ pares de coordenadas `x` e `y`.

{% include p5-editor.html id="rkWRuOQ26" %}

A função [`arc()`](https://p5js.org/reference/#/p5/arc) desenha elipses parciais, e seus primeiros $$4$$ parâmetros são iguais aos parâmetros do `ellipse()` para as coordenadas `x` e `y`, largura e altura, mas o 5$$^{o}$$ e o 6$$^{o}$$ parâmetros especificam os ângulos onde o arco começa e termina, respectivamente.

Os ângulos no p5.js são medidos em [radianos](https://en.wikipedia.org/wiki/Radian) em relação à direção positiva de `x`. E como nossos valores de `y` aumentam conforme descemos no canvas, ângulos crescentes também vão em direção a essa direção positiva de `y`.

Como os ângulos são medidos no p5.js e equivalências entre graus e radianos para alguns ângulos comuns:

<div class="scaled-images">
  <img src="{{ '/assets/images/p5/drawing-angles.jpg' | relative_url }}">
</div>

Então agora podemos usar este desenho como referência para nos ajudar a desenhar algumas elipses parciais:

{% include p5-editor.html id="12qrmbjku" %}

## Formas Irregulares e Personalizadas

O p5.js tem um método que nos permite desenhar formas personalizadas e irregulares.

Primeiro, chamamos a função [`beginShape()`](https://p5js.org/reference/#/p5/beginShape), depois adicionamos quantos vértices quisermos à nossa forma, com a função [`vertex()`](https://p5js.org/reference/#/p5/vertex), na ordem em que devem ser desenhados, e por fim avisamos ao p5.js que terminamos nossa forma chamando a função [`endShape()`](https://p5js.org/reference/#/p5/endShape).

Podemos chamar `endShape(CLOSE)` para fechar nossa forma sem precisar replicar o primeiro vértice como último vértice.

{% include p5-editor.html id="_ewE9wElh" %}

## Cores

Vimos algumas possibilidades para desenhar formas.

Vamos falar sobre cores.

O modo de cor padrão dos sketches p5.js é `RGB`, ou `RGBA`, o que significa que as cores são especificadas usando $$3$$ ou $$4$$ valores entre $$0$$ e $$255$$.

O primeiro valor corresponde à quantidade de vermelho na cor, o segundo à quantidade de verde e o terceiro à quantidade de azul. Esses são os $$3$$ canais de cor no modo `RGB` porque correspondem aos pixels físicos de um monitor, que têm pequenas luzes vermelhas, verdes e azuis.

O quarto valor, quando especificado, corresponde à opacidade da nossa cor, onde $$0$$ é uma cor totalmente transparente e $$255$$ totalmente opaca.

Também podemos especificar cores `RGB` usando apenas $$1$$ valor. Esse é um atalho para especificar que os três valores dos canais vermelho, verde e azul são iguais, e o resultado é uma cor em escala de cinza.

Além do comando `background()`, que temos usado para especificar a cor rosa do nosso fundo, também podemos usar os comandos [`fill()`](https://p5js.org/reference/#/p5/fill) e [`stroke()`](https://p5js.org/reference/#/p5/stroke) para especificar as cores de preenchimento e de contorno das nossas formas.

E, assim como os comandos `rectMode()` e `ellipseMode()`, depois que chamamos `fill()` ou `stroke()`, tudo que for desenhado em seguida terá a mesma cor.

{% include p5-editor.html id="oCr-eh9CB" %}

As cores também podem ser especificadas usando [nomes de cores html](https://www.w3schools.com/tags/ref_colornames.asp), ou [notação hexadecimal](https://www.w3schools.com/html/html_colors_hex.asp).

A notação hexadecimal pode ser familiar de softwares de edição de imagem. Ela contém exatamente a mesma informação do formato `RGB`, mas representada em [notação hexadecimal](https://byjus.com/maths/hexadecimal-number-system/), onde cada um dos $$3$$ valores de canal entre $$0$$ e $$255$$ é representado como um número hexadecimal entre `00` e `FF`, sendo `FF` a notação hexadecimal para o número $$255$$.

{% include p5-editor.html id="4ycW7yWmV" %}

### Modos de Cor

Além do modo de cor `RGB` padrão, o p5.js também nos permite descrever cores usando o modo de cor `HSB`.

`HSB` significa Hue (matiz), Saturation (saturação) e Brightness (brilho), e às vezes também é chamado de [`HSV`](https://en.wikipedia.org/wiki/HSL_and_HSV), de Hue-Saturation-Value.

O valor de Hue descreve a cor em si: se é vermelha, azul, roxa, laranja etc. Os componentes de Saturation e Brightness são atributos da cor, onde a Saturation descreve o quão "*colorida*" a cor é e o Brightness sua "*luminosidade*". Diminuir o valor de saturação deixa a cor mais cinza, enquanto diminuir seu Brightness a deixa mais preta.

Para ativar o modo de cor `HSB` no p5.js, precisamos chamar a função [`colorMode()`](https://p5js.org/reference/#/p5/colorMode) com `HSB` como parâmetro: `colorMode(HSB)`.

Depois disso, todos os comandos de cor como `background()`, `fill()` e `stroke()` vão interpretar seus $$3$$ parâmetros como valores `HSB`.

No modo `HSB`, o valor de Hue vai de $$0$$ a $$359$$, e Saturation e Brightness vão de $$0$$ a $$100$$. A unidade de Saturation e Brightness é $$\%$$, enquanto o valor de Hue é representado em graus. Isso significa que os valores de hue dão a volta no seu intervalo, e um valor de hue de $$359$$ está, na verdade, logo ao lado do valor de hue $$0$$.

Este sketch demonstra como você pode descrever a cor vermelha de várias maneiras diferentes
<div class="editor-block-wrapper">
  <div class="p5-editor-wrapper editor-wrapper">
    <iframe class="editor" src="https://editor.p5js.org/shfitz/sketches/uzKrIICkm"></iframe>
  </div>
  <a class="editor-link" href="https://editor.p5js.org/shfitz/sketches/uzKrIICkm">
    abrir exemplo em uma nova janela
  </a>
</div>


Algumas pessoas acham mais fácil interpolar entre cores e criar transições de cor no espaço `HSB`, porque podemos percorrer uma ampla paleta de cores apenas variando o valor de hue. Já no `RGB`, sempre precisamos considerar todos os $$3$$ canais ao criar transições ou interpolar cores.
