---
title: Ciclos
---
Vimos como percorrer elementos e repetir a execução de código usando loops `for()`. Essa estrutura é boa quando temos repetições com um começo e um fim bem definidos, mas às vezes podemos querer repetir alguma computação ou ação por um tempo indefinido, com um certo ritmo ou frequência. Então... ciclos.

Uma das formas mais simples de criar um ciclo é pensar em uma maneira de reiniciar periodicamente uma variável que só cresce e cresce. Pense no funcionamento de um relógio analógico: o tempo só avança e cresce, mas o ponteiro maior volta à posição inicial a cada $$60$$ minutos, e o ponteiro menor reinicia a cada $$12$$ horas:

<div class="scaled-images left w33">
  <img src="{{ 'assets/images/creative-coding/clock.jpg' |relative_url }}">
</div>

Antes de criarmos qualquer ciclo, vamos apenas visualizar uma variável que cresce e cresce no p5.js: [`frameCount`](https://p5js.org/reference/#/p5/frameCount). Essa variável registra quantas vezes a função `draw()` foi executada durante a execução do nosso programa.

No nosso sketch estamos usando-a para definir a posição horizontal do círculo, mas como `frameCount` cresce sem limite, a posição `x` também vai continuar crescendo e, eventualmente, o círculo sai da tela.

{% include p5-editor.html id="06bX4FQ5h" %}

Agora, vamos transformar essa variável que cresce e cresce e cresce em uma variável cíclica que se repete com uma certa frequência.

Acabamos de ver como fazer isso usando o [operador módulo](../maths/) ('%'), mas vamos revisar.

Como queremos criar uma sequência que se repete depois de `width` elementos, como: $$[0,1,2,...,width - 2,width - 1, 0,1,2,...,width - 2,width-1, 0,1,...]$$, podemos simplesmente usar o resto dos números naturais quando divididos por `width`. E no nosso caso, usando p5.js, a variável [`frameCount`](https://p5js.org/reference/#/p5/frameCount) nos dá uma sequência dos números naturais.

{% include p5-editor.html id="IWnsMnLdZ" %}

Neste exemplo, o número `width` está atuando como a *amplitude* do nosso ciclo (a distância que o círculo percorre antes de se repetir), e não há *deslocamento* (offset) na sua posição inicial, mas poderíamos adicionar um:

{% include p5-editor.html id="pPNtbCHT8" %}

Agora, esse círculo percorre uma distância de $$\frac{width}{2}$$ pixels, começando em $$\frac{width}{4}$$, então a amplitude desse ciclo é $$\frac{width}{2}$$ e seu deslocamento é $$\frac{width}{4}$$.

Sempre podemos fazê-lo se mover mais rápido multiplicando a variável `frameCount` por uma constante, de modo que em vez de crescer de $$1$$ em $$1$$ ela cresça de $$2$$ em $$2$$ ou de $$3$$ em $$3$$, etc.

{% include p5-editor.html id="PN1Ka5QAq" %}

## Período e Amplitude

Isso é bom, mas às vezes é útil controlar a frequência de uma ação repetitiva usando unidades de tempo reais, como: *segundos*, *milissegundos* ou *minutos*. E no nosso código, embora possamos controlar com que frequência a ação se repete, não está claro como mudá-la se quisermos, por exemplo, fazer a ação se repetir a cada $$1$$ segundo, ou a cada $$2$$ segundos, ou a cada $$15$$ segundos.

Para fazer isso neste caso, precisamos acompanhar a frequência com que nosso frame é atualizado em termos de segundos e, de forma mais geral, precisamos de alguma maneira de definir o período da nossa repetição em termos de segundos.

Vamos começar criando uma variável chamada `periodSec` que registra a frequência com que queremos repetir nossa ação (em *segundos*), e como estamos atualizando nosso sketch a cada frame, vamos calcular a frequência com que repetimos nossa ação em termos de frames, usando nossa `frameRate`:

$$periodFrames = periodSeconds \times frameRate$$

`period` é o número de frames que nossa ação vai durar antes de se repetir. A `frameRate` pode ser obtida no p5.js usando a função [`getTargetFrameRate()`](https://p5js.org/reference/#/p5/getTargetFrameRate).

Agora, podemos pensar na velocidade do nosso círculo em unidades de $$\frac{pixels}{frames}$$ como:

$$speed = \frac{amplitude}{periodFrames}$$

E sua posição:

$$ x = frameCount \times speed$$

Colocando isso no código com o módulo e o deslocamento, temos:

{% include p5-editor.html id="Qw8RJhauV" %}

## Triângulos e Outras Funções

Agora, a razão pela qual é vantajoso expressar nossos ciclos em termos de amplitude, período e deslocamento é que podemos encontrar facilmente outras equações para movimento repetitivo expressas nesses termos.

Como esta equação de uma [onda triangular](https://en.wikipedia.org/wiki/Triangle_wave#Definition) da wikipedia, que define uma onda triangular de período $$p$$ que cobre o intervalo $$[0, 1]$$ como:

$$x(t) = 2 \left\lvert\frac{t}{p} - \left \lfloor \frac{t}{p} + \frac{1}{2} \right\rfloor\right\rvert$$

A variável $$t$$ é a que continua crescendo, que no nosso caso é o `frameCount`.

Agora, podemos facilmente traduzir isso para p5js:
```js
x = 2 * amplitude * abs(frameCount / periodFrames - floor(frameCount / periodFrames + 0.5));
```

Precisamos multiplicar por `amplitude` para transformar o intervalo $$[0, 1]$$ da equação original no intervalo $$[0, amplitude]$$.

E essa é a única mudança!

Fora isso, apenas copiamos a equação da wikipedia e ela funciona:

{% include p5-editor.html id="vLW_lAv3P" %}

Isso funciona para qualquer função periódica que seja expressa em termos de $$amplitude$$, $$period$$ e $$t$$. Podemos facilmente transformá-las em código p5.js.

Esta equação, por exemplo:

<div class="scaled-images">
  <img src="{{ 'assets/images/creative-coding/cycles-00.jpg' |relative_url }}">
</div>

Só precisamos lembrar que $$t$$ é o nosso `frameCount` e $$mod()$$ é o operador `%`, e isso se torna:
```js
x = amplitude * (2 * (frameCount % periodFrames) / periodFrames - 1) ** 2;
```

{% include p5-editor.html id="epm33qbdE" %}

E esta:
<div class="scaled-images">
  <img src="{{ 'assets/images/creative-coding/cycles-01.jpg' |relative_url }}">
</div>

se torna:
```js
x = amplitude * (2 * (frameCount % periodFrames) / periodFrames - 1) ** 4;
```

{% include p5-editor.html id="2F1O5mE0h" %}

A seguir, vamos dar uma olhada em algumas funções periódicas da trigonometria. Elas são tão comuns e úteis que ganham sua própria [página](../sincos/).
