---
title: Interações
---

Já vimos e usamos as variáveis especiais do p5.js [`mouseX`](https://p5js.org/reference/#/p5/mouseX) e [`mouseY`](https://p5js.org/reference/#/p5/mouseY) quando olhamos [variáveis](../../coding/variables/), [condicionais](../../coding/conditionals/), [funções](../../coding/functions/) e [classes](../../coding/classes/).

Essas variáveis sempre nos dão as coordenadas `x` e `y` atuais do nosso mouse, relativas à origem $$(0, 0)$$ do nosso canvas.

{% include p5-editor.html id="F3XVsdlkv" %}

Agora vamos ver algumas outras variáveis e funções especiais que vão nos ajudar a adicionar interatividade aos nossos sketches usando o mouse e o teclado.

## Variáveis do Mouse

Outro par de variáveis especiais disponíveis no p5.js são as variáveis [`pmouseX`](https://p5js.org/reference/#/p5/pmouseX) e [`pmouseY`](https://p5js.org/reference/#/p5/pmouseY). Elas são como as variáveis `mouseX` e `mouseY`, mas guardam a posição *anterior* do nosso mouse.

Comparando as posições atual e anterior do mouse, podemos desenhar linhas ou formas baseadas na velocidade com que a pessoa move o mouse.

{% include p5-editor.html id="41CirwIOB" %}

Outra variável especial do p5.js relacionada ao mouse é a variável [`mouseIsPressed`](https://p5js.org/reference/#/p5/mouseIsPressed). Essa variável retorna um valor *booleano* que nos diz se algum dos botões do mouse está pressionado no momento.

Se consultarmos essa variável usando uma instrução `if` dentro da nossa função `draw()`, ela retorna `true` se a pessoa estiver segurando qualquer um dos botões do mouse, ou `false` caso contrário. Podemos então usar esse padrão simples de interação para mudar algo no nosso sketch, como a cor de fundo ou até a cor de preenchimento das nossas formas.

```js
if(mouseIsPressed) {
  // um botão do mouse está sendo pressionado
  fill(255);
} else {
  // nenhum botão está sendo pressionado
  fill(0);
}
```

{% include p5-editor.html id="XPa-IjLtt" %}

## Funções do Mouse

Digamos que queremos contar o número de vezes que uma pessoa clicou o mouse. Poderíamos tentar simplesmente adicionar uma instrução `if()` à nossa função `draw()` assim e incrementar uma variável toda vez que `mouseIsPressed` for `true`:

```js
function draw() {
  if (mouseIsPressed) {
    clickCount += 1;
  }
}
```

{% include p5-editor.html id="2qicG7dPq" %}

Isso não parece correto...

O que está acontecendo é que nossa função `draw()` executa mais rápido que nossos cliques, então a instrução `if()` e o comando de incremento executam várias vezes enquanto seguramos o botão do mouse, mesmo que sejamos bem rápidos.

Se quisermos ser um pouco mais precisos ao detectar ações do mouse, podemos usar algumas funções especiais disponíveis no p5.js.

A forma de usar essas funções é um pouco diferente da forma como usamos a maioria das funções do p5.js que vimos até agora.

Normalmente, uma função como `rect()` ou `fill()` foi definida pela biblioteca p5.js e nós a chamamos no nosso código. Essas novas funções de evento do mouse são quase o oposto: o p5.js sabe quando chamá-las, mas nós é que temos que defini-las.

Por exemplo, se queremos contar o número de vezes que o mouse é clicado, definimos uma função chamada `mouseClicked()` assim, e o p5.js vai chamá-la uma única vez por clique:

```js
function mouseClicked() {
  clickCount += 1;
}
```

{% include p5-editor.html id="Xo6XxohE_" %}

E agora podemos usar a variável `clickCount` para mudar alguns parâmetros no nosso sketch. Neste caso vamos usar o número de cliques para calcular o diâmetro de uma elipse. Para tornar as mudanças mais evidentes, vamos dar à elipse um diâmetro inicial de $$20$$ e incrementá-lo de $$4$$ pixels por clique:

```js
let mDiam = 20 + 4 * clickCount;
```

{% include p5-editor.html id="CPZLTJ2JP" %}

## Funções e Variáveis

Como as variáveis `mouseX` e `mouseY` estão sempre disponíveis, podemos usá-las dentro da função `mouseClicked()` para obter a posição de um clique do mouse. Além de aumentar o tamanho da nossa elipse, agora podemos usar os cliques para movê-la.

{% include p5-editor.html id="kP0syTm_8" %}

## Mais Eventos

Essas funções de *evento* dão uma granularidade melhor para as ações reais do mouse.

Existem funções separadas que são chamadas quando um botão do mouse é [pressionado](https://p5js.org/reference/#/p5/mousePressed), [solto](https://p5js.org/reference/#/p5/mouseReleased), [clicado](https://p5js.org/reference/#/p5/mouseClicked) ou [clicado duas vezes](https://p5js.org/reference/#/p5/doubleClicked).

Também existem funções que são chamadas quando o mouse é [movido](https://p5js.org/reference/#/p5/mouseMoved) ou [arrastado](https://p5js.org/reference/#/p5/mouseDragged) (pressionado + movido).

Só precisamos defini-las no nosso código!

Vamos incrementar o exemplo anterior.

Ainda vamos mover a elipse a cada clique e aumentar seu diâmetro em $$4$$ pixels por clique, mas agora também vamos aumentar o diâmetro em $$12$$ nos cliques duplos, então vamos precisar de um contador para eles e de uma maneira de detectá-los.

```js
function doubleClicked() {
  doubleClickCount += 1;
}
```

Pronto!!

{% include p5-editor.html id="hPBEKfhH5" %}

Mas fica meio chato clicar tantas vezes para chegar a um certo tamanho da elipse. Digamos que queremos poder esticá-la temporariamente arrastando o mouse.

Isso exige algumas variáveis extras e um pouco de lógica adicional.

Primeiro, precisamos detectar quando o mouse é pressionado (o botão desce), que é diferente de um clique, em que o botão desce e depois sobe. Quando detectarmos um pressionamento, vamos registrar a posição `mouseX` e `mouseY`, para podermos mover nossa elipse depois e também calcular a distância da nossa ação de arrastar.

```js
function mousePressed() {
  mX = mouseX;
  mY = mouseY;
}
```

Enquanto o mouse está sendo arrastado, vamos calcular e armazenar a soma de quanto ele se moveu a partir do local do pressionamento nas direções `x` e `y`. As variáveis `mouseX` e `mouseY` são as coordenadas atuais do nosso mouse, e `mX` e `mY` são as coordenadas de onde o mouse estava quando pressionamos o botão.

```js
function mouseDragged() {
  dragDistance = abs(mouseX - mX) + abs(mouseY - mY);
}
```

Agora, na nossa função `draw()`, podemos simplesmente combinar todas essas contribuições para o diâmetro da elipse:
```js
let mDiam = 20 + 4 * clickCount + 12 * doubleClickCount + dragDistance;
```

Só precisamos lembrar de zerar a variável `dragDistance` quando o mouse é solto, porque não há mais arrasto acontecendo:

```js
function mouseReleased() {
  dragDistance = 0;
}
```

{% include p5-editor.html id="lJKgp2IIY" %}

## Eventos do Teclado

Assim como com o mouse, o p5.js também tem funções de evento que são chamadas quando uma pessoa interage com o teclado.

As funções [`keyPressed()`](https://p5js.org/reference/#/p5/keyPressed) e [`keyReleased()`](https://p5js.org/reference/#/p5/keyReleased) podem ser definidas para especificar o comportamento e as ações que queremos que nosso sketch execute quando teclas são pressionadas ou soltas.

Se quisermos escolher um valor de cinza aleatório para a nossa elipse toda vez que uma tecla for pressionada, basta definir uma função `keyPressed()` no nosso sketch:

```js
function keyPressed() {
  fillColor = random(256);
}
```

{% include p5-editor.html id="UdSwBaV9L" %}

Assim como as variáveis `mouseX` e `mouseY`, podemos consultar as variáveis especiais do p5.js [`key`](https://p5js.org/reference/#/p5/key) e [`keyCode`](https://p5js.org/reference/#/p5/keyCode) em qualquer lugar do nosso código para determinar quais teclas estão sendo pressionadas.

A diferença entre `key` e `keyCode` é que `key` guarda a letra associada a uma tecla do teclado, enquanto `keyCode` tem um valor numérico associado a essa tecla. A variável `keyCode` é útil quando queremos detectar teclas que não são letras, como as setas esquerda, direita, para cima e para baixo, as teclas delete e escape, etc.

Podemos usar códigos de tecla para mover uma elipse pela tela e a variável `key` para redefinir sua posição:

{% include p5-editor.html id="SNBPBinYR" %}

Já quase conseguimos programar um videogame simples!

## Interações com o Tempo

A biblioteca p5.js também nos dá variáveis e funções que nos permitem interagir com o tempo.

*O QUÊ?!?!*

Sim. Digamos que queremos que algo aconteça em um horário específico do dia, ou com base na hora atual, ou depois de um certo número de segundos... é assim que interagimos com o tempo. Em breve vamos ver como usar essas funções e variáveis para criar [animações](../animations/), mas por enquanto vamos apenas ver o que são e como usá-las.

Digamos que queremos que nosso fundo mude de cor periodicamente. Para isso vamos precisar de uma variável que registre a última vez que houve uma mudança de fundo e, depois que $$1$$ segundo tiver passado, mudar a cor e atualizar a variável.

Vamos usar a função [`millis()`](https://p5js.org/reference/#/p5/millis) do p5.js para obter o número de milissegundos (milésimos de segundo) que se passaram desde o início do nosso sketch (desde que a função `setup()` foi chamada pela primeira vez).

{% include p5-editor.html id="ilyYXfwfS" %}

Também podemos usar a hora atual para mudar parâmetros do nosso sketch.

Digamos que queremos que nosso fundo seja preto à meia-noite, branco ao meio-dia e depois preto de novo à meia-noite.

Podemos usar a função [`hour()`](https://p5js.org/reference/#/p5/hour) para obter um valor entre $$0$$ e $$23$$ e então usá-lo para calcular a cor de fundo. Temos que fazer alguma matemática para transformar as horas da tarde no intervalo $$[13, 23]$$ em valores em $$[11, 1]$$, para que as cores voltem ao preto depois do meio-dia.

```js
if (currentHour > 12) {
  currentHour = 24 - currentHour;
}
```

{% include p5-editor.html id="GIkBvs3q4" %}

Vamos ver mais sobre essas funções quando olharmos as animações.

## Diversão com o Mouse

Vamos juntar alguns desses conceitos e criar alguns sketches mais interessantes.

Em vez de apenas desenhar uma elipse onde nosso mouse está com `mouseX` e `mouseY`, podemos usar um array para registrar o histórico das posições do mouse e desenhar linhas conectando cada local por onde ele passou.

```js
function mouseMoved() {
  mPos.push({ x: mouseX, y: mouseY });
}
```

Agora nossa função `mouseMoved()` é responsável por adicionar pontos ao array `mPos`, e no `draw()` percorremos cada objeto do array e desenhamos linhas conectando-os.

{% include p5-editor.html id="l7XSgtlFw" %}

Isso é legal, mas vamos adicionar um componente de tempo para fazer as posições mais antigas desaparecerem.

Quando colocarmos nossas coordenadas no array, também vamos incluir um número para a espessura do segmento de linha:

```js
function mouseMoved() {
  mPos.push({ x: mouseX, y: mouseY, w: 24 });
}
```

Agora, na nossa função `draw()` teremos dois passos: primeiro, vamos diminuir o valor `w` de cada posição que salvamos e remover as posições cujo valor `w` ficou menor que $$0.5$$. Depois, no segundo passo, vamos desenhar segmentos de linha usando as posições restantes no array, usando o parâmetro `w` na função `strokeWeight()`.

Isso dá um efeito legal de rastro aos movimentos do nosso mouse.

{% include p5-editor.html id="w3nfyQ553" %}

Podemos mudar isso um pouco para que só adicione pontos quando o mouse é arrastado, e só atualize as espessuras se o mouse não estiver pressionado.

Para evitar interações indesejadas entre linhas novas e linhas antigas, vamos ter que limpar o array `mPos` sempre que uma nova linha for iniciada:

```js
function mousePressed() {
  mPos.splice(0, mPos.length);
}
```

E vamos mudar um pouco a nossa lógica de atualização de `w` para fazer as linhas mais antigas desaparecerem mais rápido depois que o mouse é solto:
```js
let updateCount = mPos.length - i + 2;
mPos[i].w *= 0.99 ** updateCount;
```

{% include p5-editor.html id="Qjv7xnWFm" %}
