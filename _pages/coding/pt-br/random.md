---
title: Aleatoriedade
---
## Quebrando Padrões

Agora que vimos como desenhar [padrões](../counters/) no nosso canvas usando loops `for()` e como criar nossas próprias [funções](../functions/), vamos ver como usar uma das funções nativas do p5.js para deixar nossos padrões mais interessantes.

Vamos começar com uma grade simples de círculos.

Note o uso de uma variável para definir o diâmetro dos círculos:

{% include p5-editor.html id="CyTtdVfBe" %}

Uma forma de adicionar variabilidade ao nosso padrão seria escolher um valor ligeiramente diferente para o diâmetro ou a cor de cada um dos nossos círculos.

Vamos começar pela cor. Vamos ver como podemos usar [`random()`](https://p5js.org/reference/#/p5/random) para selecionar um valor de um conjunto pré-determinado de opções para a cor das nossas elipses.

Por exemplo, digamos que queremos que metade delas seja preta e a outra metade branca, mas não queremos todas as pretas em sequência e depois as brancas, porque isso pareceria um tabuleiro de xadrez e é chato.

Queremos que elas estejam misturadas e embaralhadas.

Poderíamos usar `random(0, 255)` para escolher um valor aleatório de cinza entre $$0$$ e $$255$$ para preencher cada círculo:

{% include p5-editor.html id="DjylI5wj-N" %}

Hmmmm.... tem um problema com isso: não estamos desenhando círculos pretos e brancos. Todos os valores entre $$0$$ e $$255$$ têm a mesma probabilidade de ocorrer!

Há algumas formas de consertar isso, mas vamos começar usando a função `random()` para nos dar um número entre $$0$$ e $$1.0$$. Como todos os valores entre $$0$$ e $$1.0$$ têm a mesma probabilidade de ocorrer, cerca de metade dos números será menor que $$0.5$$ e a outra metade será maior que $$0.5$$.

Se imaginarmos uma reta numérica e $$50$$ pontos aleatórios nessa reta, cerca de $$25$$ deles serão menores que $$0.5$$ e o resto será maior que $$0.5$$:

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/p5/random-lottery-00.jpg' | relative_url }}">
</div>

Colocando isso em pseudocódigo, nossa lógica poderia ser:
```js
aRandomNumber = random()
if aRandomNumber < 0.5: draw a white circle
if aRandomNumber > 0.5: draw a black circle
```

Como as duas escolhas são mutuamente exclusivas (não podem acontecer ao mesmo tempo), podemos usar uma instrução `else` e transformar isso no seguinte JavaScript:
```js
if(random() < 0.5) {
  fill(255);
} else {
  fill(0);
}
```

{% include p5-editor.html id="Myfh7bORq" %}

Legal ! E se quisermos usar $$3$$ cores agora? Como alguns círculos dourados, azuis e brancos?

Poderíamos fazer algo parecido:

Sortear um número; se for menor que a metade, desenhar um círculo preto; caso contrário, sortear outro número e repetir o processo para escolher entre dourado e azul. Isso funciona, mas o que notamos sobre a frequência das três cores?

{% include p5-editor.html id="0fZEyVwJz" %}

Os círculos dourados e azuis ocorrem com frequência parecida, mas parece que há mais círculos pretos. O dobro de círculos pretos, na verdade. Neste código estávamos pedindo ao p5.js que desenhasse círculos pretos metade das vezes, e depois dividisse a metade restante de novo ao meio, então o azul e o dourado só acontecem $$25\%$$ das vezes cada um.

Implementamos algo assim:

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/p5/random-lottery-01.jpg' | relative_url }}">
</div>

E se sortearmos um número aleatório entre $$0$$ e $$1$$ e o guardarmos em uma variável. Agora podemos usar esse número várias vezes em várias instruções `if()` e verificar se ele cai em certos intervalos. Se queremos que algo aconteça cerca de um terço das vezes, isso é o mesmo que perguntar se o número aleatório é menor que $$0.3333$$, porque, de todos os números entre $$0$$ e $$1$$, um terço deles é menor que $$0.3333$$.

Agora estamos implementando algo assim:

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/p5/random-lottery-02.jpg' | relative_url }}">
</div>

A lógica para o preto pode ser:
```js
if aRandomNumer < 0.333: draw black
```

Se o número for maior que $$0.3333$$, agora podemos apenas verificar se ele também é menor que $$0.6666$$. Fazer isso depois de verificar que ele é maior que $$0.3333$$ garante que o nosso número aleatório está entre $$0.3333$$ e $$0.6666$$, um intervalo que corresponde a outro terço dos números entre $$0$$ e $$1$$.

A lógica para o preto e o dourado:
```js
if aRandomNumer < 0.333: draw black
else if aRandomNumer < 0.666: draw gold
```

E se o número não for menor que $$0.6666$$, ficamos com o outro terço dos números entre $$0$$ e $$1$$ (todos os que estão entre $$0.6666$$ e $$1.0$$):
```js
if aRandomNumer < 0.333: draw black
else if aRandomNumer < 0.666: draw gold
else: draw blue
```

Em código:

{% include p5-editor.html id="w2bda32Ty" %}

Oba. Muito melhor.
### ✨✨🎉✨✨

## Loteria

Estamos chamando isso de método da *loteria* para escolher números aleatórios porque, toda vez que há uma escolha a fazer, estamos escolhendo uma opção de um conjunto de possibilidades. Não estamos usando o valor *contínuo* que a função `random()` retorna, mas sim usando-o para sortear de um conjunto *discreto* de opções.

Fazer uma seleção por loteria assim é um pouco trabalhoso, mas nos dá uma forma de controlar com precisão a frequência com que algo acontece. Se queremos que eventos diferentes aconteçam com probabilidades diferentes, podemos simplesmente mudar os valores que verificamos dentro da nossa lógica `if`/`else-if`/`else`.

Digamos que queremos usar seis cores: preto, branco, dourado, azul, verde, cinza, e queremos o preto um terço das vezes, o branco um quarto das vezes e as outras cores um décimo das vezes cada. Para trabalhar com valores que somam $$100\%$$, vamos usar preto: $$35\%$$, branco: $$25\%$$ e $$10\%$$ para todas as outras:

Algo assim:
<div class="scaled-images left w75">
  <img src="{{ '/assets/images/p5/random-lottery-03.jpg' | relative_url }}">
</div>

Nossa lógica poderia ser algo como:
```js
aRandomNumber = random()
if aRandomNumber < 0.35: draw black
else if aRandomNumber < 0.60: draw white
else if aRandomNumber < 0.70: draw gold
else if aRandomNumber < 0.80: draw blue
else if aRandomNumber < 0.90: draw green
else: draw grey
```

Os limites que verificamos são cumulativos. Quando estamos verificando o branco, já sabemos que o número é maior que $$0.35$$, e só verificamos se ele é menor que $$0.6$$ porque os números entre $$0.35$$ e $$0.60$$ correspondem a $$25\%$$ de todos os números entre $$0.0$$ e $$1.0$$.

Em código, a lógica acima se torna:

{% include p5-editor.html id="Rf4yj-5hf" %}

Será que podemos usar algo assim para escolher formas diferentes ou tamanhos diferentes de um conjunto de opções pré-determinadas ?

## Loteria de Tamanho

O processo é parecido, mas agora vamos ter outro número aleatório e outro bloco de instruções `if`/`else` para selecionar o tamanho.

Vamos voltar para apenas $$3$$ cores com probabilidades iguais:

{% include p5-editor.html id="gHh7Q8k7g" %}

Agora vamos adicionar esta lógica para selecionar o tamanho da elipse:

```js
aRandomNumber = random()
if aRandomNumber < 0.25: draw big
else if aRandomNumber < 0.5: draw medium
else if aRandomNumber < 0.75: draw small
else: draw extra-small
```

E vamos usar uma variável chamada `maxDiam` para registrar o diâmetro máximo das nossas elipses, para que não precisemos fixar o espaçamento que estamos usando nos loops `for`.

{% include p5-editor.html id="Ps_Z5TmvK" %}

## Loteria de Posição

Podemos usar a mesma ideia para variar ligeiramente as posições `x` e `y` das nossas elipses.

Depois de escolher um diâmetro, podemos usar outra instrução `if`/`else` para escolher a direção em que queremos mover a nossa elipse:

```js
aRandomNumber = random()
if aRandomNumber < 0.25: move left
else if aRandomNumber < 0.5: move right
else if aRandomNumber < 0.75: move up
else: move down
```

Vamos usar uma variável extra chamada `dValue` para calcular o quanto queremos mover as elipses, e variáveis chamadas `dx` e `dy` para registrar em qual direção mudar.

Este cálculo faz com que as elipses menores se movam mais, e as elipses maiores se movam menos:
```js
let dValue = (maxDiam - eDiam) / 2;
```

E em código:

{% include p5-editor.html id="Rlaj-c9Ft" %}

O que é útil nisso é que agora temos uma forma de criar padrões não regulares que não parecem realmente padrões. Os loops `for` continuam lá, então, nos bastidores da nossa lógica, temos uma grade, mas visualmente ela não necessariamente parece uma grade.

## Variações Contínuas Uniformemente Distribuídas

Temos usado os valores da função `random()` para escolher entre um conjunto de opções pré-definidas porque, em alguns casos, é bom adicionar variação aleatória mantendo algum controle sobre os resultados.

Por exemplo, um sketch que escolhe cores completamente aleatórias para cada elipse provavelmente vai escolher um monte de cores desagradáveis. A forma de escolher uma cor completamente aleatória é sortear $$3$$ números aleatórios entre $$0$$ e $$255$$ e usá-los como os valores de vermelho, verde e azul da nossa cor de preenchimento:

```js
let redRandom = random(0, 255);
let greenRandom = random(0, 255);
let blueRandom = random(0, 255);

fill(redRandom, greenRandom, blueRandom);
```

{% include p5-editor.html id="H7qqVMUwt" %}

## 🤮

Mas, talvez valores contínuos uniformemente distribuídos possam ser usados para as variações de diâmetro e de posição das elipses.

Isto vai escolher um diâmetro aleatório entre $$4$$ e $$40$$ quando `maxDiam` for $$40$$:
```js
eDiam = random(maxDiam / 4, maxDiam);
```

{% include p5-editor.html id="2JCHEP_4g" %}

E isto vai adicionar variações aleatórias nas direções `x` e `y`, com base no valor do diâmetro da elipse. A diferença é que agora *QUALQUER* número entre `-dValue` e `dValue` é possível:

```js
let dValue = (maxDiam - eDiam) / 2;
let dx = random(-dValue, dValue);
let dy = random(-dValue, dValue);
```

{% include p5-editor.html id="zcNqTwUpM" %}

Isso fica bem legal. A nossa grade subjacente quase não é perceptível.

E combinando posições e tamanhos aleatórios:

{% include p5-editor.html id="p1yFq_-SN" %}
