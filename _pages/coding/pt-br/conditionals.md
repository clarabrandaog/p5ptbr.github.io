---
title: Condicionais
---
## Mouse Sobre Formas

Agora que vimos como criar [variáveis](../variables/) para acompanhar valores e parâmetros que mudam no nosso código, vamos ver como elas podem ser usadas em expressões booleanas para criar formas interativas.

Vamos começar com um quadrado, centralizado no nosso canvas. Note o uso de variáveis para definir a posição e as dimensões do ```rect()```. Isso vai ser importante daqui a pouco.

{% include p5-editor.html id="jLZVEU6rq" %}

Agora, digamos que queremos detectar quando o mouse está dentro do quadrado e mudar a cor dele de branco para dourado.

A estrutura do nosso código poderia ser mais ou menos assim:
```js
if (mouse is inside the square) {
  fill(gold);
} else {
  fill(white);
}
rect(rectX, rectY, rectWidth);
```

Usamos a instrução `if()` para especificar que temos um código que não deve ser executado toda vez, mas sim apenas se certas *condições* forem satisfeitas. No nosso caso, só queremos executar o comando `fill(gold)` *se* o mouse estiver dentro do quadrado naquela iteração do nosso loop `draw()`. Caso o mouse não esteja dentro do quadrado em uma determinada iteração, a condição do if não é satisfeita e o código especificado pela instrução `else` é executado no lugar.

Agora só precisamos descobrir exatamente como calcular quando o mouse está dentro do quadrado durante uma iteração do nosso loop `draw()`.

Sabemos que as variáveis `mouseX` e `mouseY` sempre guardam a posição atual do nosso mouse, então podemos usá-las para determinar onde o mouse está em relação a cada uma das quatro bordas do quadrado.

## Bordas do Quadrado

Vamos começar pela borda mais à esquerda. Para o mouse estar dentro do quadrado, a sua posição x tem que estar à direita da borda esquerda do quadrado. Como temos uma variável para a posição x do quadrado, isso pode ser determinado perguntando ao computador:

```
if(mouseX > rectX)
```

{% include p5-editor.html id="w7AcLo3Ai" %}

Conforme movemos o mouse pelo canvas, vamos notar que, sim, o quadrado muda de cor quando o mouse vai do lado esquerdo do canvas para dentro do quadrado, mas ele também continua dourado quando o mouse sai pelo lado direito, ou quando o mouse está acima ou abaixo do quadrado. Vamos consertar isso.

Focando apenas na direção x, a lógica que queremos implementar pode ser descrita em palavras como: "preencha o quadrado de dourado enquanto o mouse estiver *entre* as bordas esquerda e direita do quadrado".

Isso poderia ser expresso pela seguinte equação:
```js
leftEdge < mouseX < rightEdge
```

Infelizmente não podemos simplesmente colocar uma condição dupla assim dentro de uma instrução `if()`, mas podemos dividi-la usando lógica booleana. Como queremos que as duas condições sejam verdadeiras, isso equivale a:
```js
leftEdge < mouseX AND mouseX < rightEdge
```

E isso pode ser reorganizado um pouco para melhorar a legibilidade, como "o mouse é maior que a borda esquerda E menor que a borda direita":
```js
mouseX > leftEdge AND mouseX < rightEdge
```

Não temos a posição da borda mais à direita, mas podemos calculá-la a partir da posição x do retângulo e da sua largura:
```js
let rightEdge = rectX + rectWidth
```

Colocando em JavaScript e usando as nossas variáveis, isso se torna:
```js
if(mouseX > rectX && mouseX < rightEdge)
```
{% include p5-editor.html id="8bEIeXtHV" %}

Melhor! Mas o quadrado ainda fica dourado quando o mouse está diretamente acima ou abaixo dele.

# 🤔

Podemos repetir o raciocínio acima, mas desta vez incluindo também a direção y.

Uma frase que poderia descrever o que queremos seria algo como: "preencha o quadrado de dourado se a posição x do mouse estiver *entre* as bordas esquerda e direita do quadrado E a posição y do mouse estiver *entre* as bordas inferior e superior".

A lógica em pseudocódigo seria então:
```js
mouseX > leftEdge AND mouseX < rightEdge AND
mouseY > topEdge AND mouseY < bottomEdge
```

As instruções lógicas da direção y (`mouseY < bottomEdge`) parecem "de cabeça para baixo". Queremos detectar quando o mouse está acima da borda inferior, não abaixo!

Mas, na verdade, isso está correto do jeito que está, porque ```mouseY``` cresce em direção à parte de baixo da tela.

Só precisamos declarar uma variável `bottomEdge` e o nosso JavaScript pode ficar assim:
```js
if (mouseX > rectX && mouseX < rightEdge &&
    mouseY > rectY && mouseY < bottomEdge)
```

{% include p5-editor.html id="EqZNExq9y" %}

A instrução `if()` no código acima é, na verdade, uma única linha longa, mas o editor a separou em mais linhas para melhorar a legibilidade.

## Além de Um Quadrado

Como usamos variáveis para definir todos os parâmetros do nosso quadrado, podemos facilmente mudar suas dimensões e movê-lo para qualquer outro lugar da tela mudando apenas os parâmetros `rectX`, `rectY`, `rectWidth` e `rectHeight`:

{% include p5-editor.html id="SG0OB3O50" %}

Podemos até estender isso para várias formas:

{% include p5-editor.html id="IOAIaoJ2b" %}

Há muito código repetido aqui, o que faz o sketch parecer bagunçado e mais complicado do que é.

Mais adiante vamos ver como usar [funções](../functions/), [arrays](../arrays/) e [objetos](../objects/) para limpar isso e facilitar o entendimento, a extensão e a manutenção:

{% include p5-editor.html id="Htlpfx91a" %}

## Colisão

Esse tipo de lógica é, na verdade, um padrão comum em certos tipos de programação. É útil não só para detectar quando o nosso mouse está sobre objetos, mas também para detectar quando formas diferentes se sobrepõem ou colidem, como em um videogame.

No sketch abaixo, conseguimos detectar quando o nosso círculo se sobrepõe ao retângulo?

{% include p5-editor.html id="ez8oUpy_o" %}

Podemos começar com a lógica de antes, só que ela não vai detectar quando o círculo se sobrepõe ao retângulo, apenas quando o centro do círculo entra no retângulo.

{% include p5-editor.html id="ZGC9WPXig" %}

Se olharmos um diagrama do que está acontecendo e do que queremos que aconteça, podemos ver que, em vez de detectar quando o centro do círculo está dentro do retângulo, devemos detectar quando o centro do círculo está a até $$r$$ pixels da borda do retângulo (onde $$r$$ é o raio do círculo, ou seja, metade do seu diâmetro):

<div class="scaled-images">
  <img src="{{ '/assets/images/p5/overlap-00.jpg' | relative_url }}">
</div>

Se estendermos o desenho para considerar os quatro lados do retângulo, obtemos algo assim:

<div class="scaled-images">
  <img src="{{ '/assets/images/p5/overlap-01.jpg' | relative_url }}">
</div>

Isso é exatamente igual à nossa situação original de detectar se o centro do círculo está dentro de um retângulo, só que agora precisamos usar um retângulo maior.

Podemos criar algumas variáveis novas para o nosso novo retângulo de sobreposição:
```js
let ellipseRadius = ellipseDiam / 2;

let oRectX = rectX - ellipseRadius;
let oRectY = rectY - ellipseRadius;
let oRightEdge = rightEdge + ellipseRadius;
let oBottomEdge = bottomEdge + ellipseRadius;
```

E atualizar as variáveis na instrução `if()`, mas a lógica é exatamente a mesma:
```js
if (mouseX > oRectX && mouseX < oRightEdge &&
    mouseY > oRectY && mouseY < oBottomEdge)
```

{% include p5-editor.html id="Pm6T8XToP" %}

Seria um bom toque fazer o círculo e o retângulo mudarem de cor quando se sobrepõem.

Poderíamos fazer isso repetindo a lógica de detecção antes de desenhar cada forma:
```js
// desenha o retângulo
if (mouseX > oRectX && mouseX < oRightEdge &&
    mouseY > oRectY && mouseY < oBottomEdge) {
  fill(rectColor);
} else {
  fill(255);
}
rect(x,y,w,h);

// desenha o círculo
if (mouseX > oRectX && mouseX < oRightEdge &&
    mouseY > oRectY && mouseY < oBottomEdge) {
  fill(rectColor);
} else {
  fill(255);
}
ellipse(mouseX, mouseY, diam, diam);
```

Mas isso é desnecessário e deixa o nosso código mais difícil de ler.

O que podemos fazer, em vez disso, é calcular a lógica de sobreposição uma vez e guardar o resultado em uma variável que podemos consultar várias vezes dentro do nosso loop `draw()`:
```js
let overlap = mouseX > oRectX && mouseX < oRightEdge &&
              mouseY > oRectY && mouseY < oBottomEdge;
```

{% include p5-editor.html id="RIBXWiq2-" %}

Isso ainda poderia ser melhorado para tratar os casos em que ele detecta uma sobreposição de forma errada quando o círculo se move perto dos cantos do retângulo (um [corner case](https://en.wikipedia.org/wiki/Corner_case) literal), mas vamos esperar até vermos [funções](../functions/) e um pouco de [trigonometria](../../creative-coding/vectors/).
