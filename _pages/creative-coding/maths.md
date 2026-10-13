---
title: "Matemática: mod e map"
---
## Condições de Contorno

Agora que sabemos configurar animações usando o ciclo do `draw()`, e também detectar entradas do usuário usando funções de evento, com frequência vamos nos deparar com *condições de contorno*. Às vezes, as condições de contorno envolvem limites físicos no nosso canvas, como detectar e decidir o que fazer quando uma forma se move além da borda da nossa tela. Outras vezes, essas condições envolvem definir limites para os valores mínimo e máximo que uma variável pode ter.

Neste tutorial vamos ver $$2$$ estratégias/técnicas/funções para lidar com condições de contorno de forma elegante.

## Mod

Vamos considerar uma animação simples de um círculo que se move pelo nosso canvas.

Vamos usar a variável do p5.js [`frameCount`](https://p5js.org/reference/#/p5/frameCount) para definir a posição `x` do nosso círculo. Essa variável registra quantas vezes a função `draw()` foi executada no nosso programa e, como ela começa em $$0$$ e cresce indefinidamente, nosso círculo vai começar em $$0$$ e seguir infinitamente para a direita, eventualmente saindo da tela.

{% include p5-editor.html id="sq5gwzjWb" %}

Se quisermos manter o círculo sempre na tela, podemos criar uma variável separada que incrementa uma vez por frame, assim como o `frameCount`, mas que volta a $$0$$ assim que fica maior que `width`.

Isso também tem a vantagem de nos deixar controlar a velocidade do círculo mudando quanto o `x` cresce a cada `draw()`.

{% include p5-editor.html id="s8NNTlNKp" %}

Experimente! Mude `x += 4` para `x += 8`, ou `x += 12` no sketch acima ☝️.

Outra maneira de escrever exatamente essa mesma lógica, sem uma instrução `if()`, é usando o [operador módulo](https://betterexplained.com/articles/fun-with-modular-arithmetic/) (`%`). Esse operador calcula o resto da divisão entre dois números.

$$\frac{10}{2} = 5$$ (resto: $$0$$)

$$\frac{11}{3} = 3$$ (resto: $$2$$)

$$\frac{13}{6} = 2$$ (resto: $$1$$)

Em JavaScript:

```js
10 % 2; // 0
11 % 3; // 2
13 % 6; // 1
```

E isso é útil para nós porque podemos usar o operador módulo para criar sequências repetitivas de números.

Digamos que temos uma variável que cresce e cresce e cresce, e queremos que ela *reinicie* em $$0$$ quando chegar a $$10$$. Então temos uma sequência $$[0,1,2,3,4,...,10,11,12,13,...,20,21,22,23,...]$$ e queremos obter outra sequência $$[0,1,2,3,4,5,6,7,8,9, 0,1,2,3,4,5,6,7,8,9, 0,1,2,...]$$. A segunda sequência é simplesmente a lista de valores que obtemos ao calcular o *resto* da divisão de todos os números da primeira sequência por $$10$$.

De forma mais geral, se temos uma variável `X` que cresce e cresce e cresce e queremos uma sequência que se repita depois de $$N$$ valores, basta calcular `X % N`.

Se quisermos repetir a sequência depois de `width` elementos, ou seja, transformar $$[0,1,2,3,4,...,10,11,12,13,...,20,21,22,23,...]$$ em $$[0,1,2,...,width - 2,width - 1, 0,1,2,...,width - 2,width-1, 0,1,...]$$, podemos simplesmente usar o resto dos números naturais quando divididos por `width`.

No nosso caso, usando p5.js, a variável [`frameCount`](https://p5js.org/reference/#/p5/frameCount) já nos dá uma sequência dos números naturais. E, como neste exemplo queremos que o círculo volte a $$0$$ quando chegar a `width`, basta definir a posição `x` do nosso círculo como `frameCount % width`.

{% include p5-editor.html id="KanH50nEv" %}

E se agora quisermos que ele se mova mais rápido, basta multiplicar a variável `frameCount` por uma constante, de modo que em vez de crescer de $$1$$ em $$1$$ ela cresça de $$2$$ em $$2$$ ou de $$3$$ em $$3$$, etc:

```js
2 * frameCount; // [0, 2, 4, 6, 8, ...]
3 * frameCount; // [0, 3, 6, 9, 12, ...]
```

De qualquer forma, não importa a rapidez com que nossa sequência inicial cresça, desde que calculemos seu resto quando dividida por `width`, a posição `x` do nosso círculo vai voltar a $$0$$ assim que ficar maior que `width`.

{% include p5-editor.html id="-QT45J7JU" %}

É bom ter o operador módulo (`%`) em mente sempre que estivermos trabalhando com *sequências*, ou *ciclos*, e quisermos criar uma sequência de valores que *recomeça*, ou *reinicia*.

Por exemplo, digamos que queremos alternar nossas cores de fundo entre $$3$$ valores, mudando de cor toda vez que houver um clique no canvas.

Podemos simplesmente criar uma variável que conta o número de cliques e usar o operador módulo (`%`) para calcular qual cor mostrar. Neste caso, a variável `clickCount` é a sequência que só cresce e cresce e cresce, mas como o que queremos é uma sequência que se repita depois de $$3$$ valores, vamos usar `% 3`.

{% include p5-editor.html id="ZK24vFjmJ" %}

E se quisermos que a cor de fundo mude automaticamente a cada segundo?

Existe uma função no p5.js chamada [`second()`](https://p5js.org/reference/#/p5/second) que nos dá o número de segundos do relógio do nosso computador. Esse número em si se repete depois de $$60$$ valores, mas como ainda temos apenas $$3$$ opções de cores, vamos continuar usando `% 3`.

{% include p5-editor.html id="0-XZ391uE" %}

Este é um exemplo de conversão de um sinal de *entrada* que não controlamos em um parâmetro de *saída* para controlar nossa animação.

Vamos ver isso com mais detalhes.

## Ligando Variáveis

Não é incomum querer usar um valor lido ou gerado em uma parte do seu programa para controlar as propriedades físicas de objetos em outras partes do programa. É assim que podemos criar desenhos que reagem à entrada do usuário, ou animações que variam de acordo com dados que vêm de um sensor ou de um banco de dados em algum lugar da internet.

Nessas situações, vamos ter que definir relações entre nossos sinais de *entrada* e os parâmetros que queremos usar para a nossa *saída*. E, muito frequentemente, os sinais de entrada e os parâmetros de saída nem têm os mesmos intervalos, unidades ou tipos de dados. Por exemplo, podemos querer converter temperaturas medidas em `Celsius` em diâmetros de círculo em `pixels`, ou a passagem do tempo medida em `segundos` em uma cor de preenchimento especificada em `RGB`.

Vamos ver um exemplo.

Este é um sketch que desenha um círculo em `(mouseX, mouseY)` e define o diâmetro do círculo como igual ao valor de `mouseY`: quanto mais embaixo no canvas, maior o círculo; o círculo quase não é visível quando o mouse está perto do topo do canvas e, da mesma forma, fica meio grande demais quando está bem embaixo.

{% include p5-editor.html id="J9txjcvGj" %}

Uma forma de tornar isso um pouco mais controlável é definir uma variável `maxDiameter` e então calcular o diâmetro real como uma porcentagem desse valor com base na posição do mouse:

$$diameter = \frac{mouseY}{height} \times maxDiameter$$

O termo $$\frac{mouseY}{height}$$ é a posição do mouse como uma porcentagem da altura do canvas: ele vai de $$0\%$$ quando `mouseY` é $$0$$, até $$100\%$$ quando `mouseY` é igual a `height`.

Agora podemos usar isso para definir o diâmetro:

{% include p5-editor.html id="PEHUu0w-y" %}

E podemos fazer algo parecido se quisermos definir um valor mínimo para o diâmetro do círculo:

$$diameter = \frac{mouseY}{height} \times (maxDiameter - minDiameter) + minDiameter$$

O termo $$(maxDiameter - minDiameter)$$ é a magnitude da nossa saída (o quanto ela pode variar), e multiplicá-lo por $$\frac{mouseY}{height}$$ nos dá o quanto devemos avançar ao longo dessa magnitude. Ainda temos que somar o termo $$minDiameter$$ para garantir que nossa saída esteja no intervalo $$[minDiameter, maxDiameter]$$:

{% include p5-editor.html id="t1C6SSfop" %}

Acabamos de criar uma relação muito precisa que define o diâmetro do nosso círculo com base em um sinal de entrada.

## Map

Digamos que agora queremos inverter a lógica: círculo maior no topo da tela e círculo menor embaixo. Poderíamos usar esta lógica:

```js
let mDiamInv = (mouseY / height) * (maxDiam - minDiam) + minDiam;
let mDiam = maxDiam - mDiamInv + minDiam;
```

{% include p5-editor.html id="6AdmlEjeR" %}

A lógica para calcular o diâmetro do círculo está ficando um pouco complicada...

É aqui que entra o [`map()`](https://p5js.org/reference/#/p5/map). É uma função que mapeia um valor de um intervalo para outro.

Se quisermos mapear o valor de `mouseY` de valores de `height` para valores de `diameter`, $$[0, height] \rightarrow [40, 200]$$, basta escrever:

```js
let mDiam = map(mouseY, 0, height, minDiam, maxDiam);
```

{% include p5-editor.html id="XJlqeKXnW" %}

Da mesma forma, se quisermos inverter a lógica e ter um círculo maior no topo da tela e um círculo menor embaixo, basta trocar `minDiam` e `maxDiam`:

```js
let mDiam = map(mouseY, 0, height, maxDiam, minDiam);
```

{% include p5-editor.html id="1xiOT--Ci" %}

## Mapeando Entre Intervalos

O [`map()`](https://p5js.org/reference/#/p5/map) também é útil em outras situações. Digamos que queremos desenhar linhas verticais igualmente espaçadas no nosso canvas.

Temos um loop `for()` que conta de $$0$$ a $$16$$ e, com o `map()`, podemos simplesmente transformar a variável contadora na posição que queremos, porque sabemos que nosso contador tem um intervalo $$[0, 16]$$ e as posições das nossas linhas terão um intervalo $$[0, width]$$:

{% include p5-editor.html id="NqqsgY5IA" %}

Podemos mapear o mesmo contador para dois intervalos de saída diferentes: $$[0, 16] \rightarrow [0, width]$$ e $$[0, 16] \rightarrow [width, 0]$$. O primeiro vai da esquerda para a direita e o outro da direita para a esquerda, conforme o contador aumenta:

{% include p5-editor.html id="2CbfzdpbU" %}

Usando o mesmo contador podemos mapear $$[0, 16] \rightarrow [0, width]$$ e $$[0, 16] \rightarrow [0, \frac{width}{10}]$$:

{% include p5-editor.html id="zJ9WBx5Mz" %}

Ou, com um deslocamento em um dos intervalos de saída, mapear $$[0, 16] \rightarrow [0, width]$$ e $$[0, 16] \rightarrow [\frac{width}{4}, \frac{width}{3}]$$:

{% include p5-editor.html id="xoCvCTcuz" %}

Podemos até fazer algo em que nosso contador $$[0, 16]$$ é mapeado para *três* intervalos de saída diferentes:

{% include p5-editor.html id="xwmzj1UIx" %}

## Mapeando Entre Unidades

Também é muito útil usar a função `map()` se quisermos desenhar o tempo.

Neste exemplo mapeamos o valor dos segundos do relógio do nosso computador para o diâmetro de um círculo. Sabemos que os segundos têm um intervalo $$[0, 59]$$, então é só escolher os menores e maiores valores que queremos para o nosso diâmetro.

Vamos mapear $$[0, 59] \rightarrow [20, width]$$ para o diâmetro e, já que estamos nisso, vamos também mapear o valor dos segundos para a quantidade de verde na cor do nosso `fill()`. Sabemos que cada um dos $$3$$ parâmetros de cor tem um valor máximo de $$255$$, e se quisermos sempre um pouco de verde na nossa cor, podemos mapear $$[0, 59] \rightarrow [128, 255]$$.

{% include p5-editor.html id="thbVYgT-l" %}
