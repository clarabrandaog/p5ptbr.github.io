---
title: Mais Aleatoriedade
---
Já vimos como gerar [números aleatórios](../../coding/random/) contínuos dentro de um intervalo, e também como usar esses números para escolher valores de um conjunto predeterminado de opções.

Só para revisar, vamos usar `random()` para gerar uma grade de elipses com diâmetros diferentes e imprevisíveis:

{% include p5-editor.html id="JYdWqKl2W" %}

Estamos usando `random(0.2 * gridW, 0.8 * gridW)` para escolher valores aleatórios de diâmetro das elipses entre $$20\%$$ e $$80\%$$ da largura da grade.

## Uniforme
Chamar `random(min, max)` nos dá valores uniformemente distribuídos: qualquer valor entre seus parâmetros `min` e `max` tem a mesma probabilidade de ocorrer.

Podemos visualizar isso de algumas maneiras.

Em uma dimensão, usando uma variável aleatória para definir o diâmetro de elipses. Todos os valores entre $$0$$ e `width` são igualmente prováveis:

{% include p5-editor.html id="--jfZGT-3" %}

Podemos executar o sketch de novo e obter resultados diferentes, mas, no geral, as elipses não parecem seguir nenhum padrão.

Também podemos visualizar a distribuição da função `random()` em duas dimensões, usando variáveis aleatórias para escolher as posições `x` e `y` das elipses. Todas as posições do canvas são igualmente prováveis:

{% include p5-editor.html id="I6-rCuYnY" %}

E, de novo, podemos executar o sketch outra vez e obter resultados um pouco diferentes, mas sempre vai parecer que as elipses estão igualmente espalhadas pelo nosso canvas.

Variáveis aleatórias uniformes estão fortemente relacionadas a eventos de escolha, como jogar um dado, tirar uma carta de um baralho ou pensar em um número entre $$0$$ e $$100$$.

Essa não é a única forma como a aleatoriedade ocorre, porém. Se olharmos para valores como a altura das pessoas, o tamanho de sapatos e a pressão arterial, ou até algo como os níveis de pH de vinhos e a lotação do metrô ao longo do dia, vamos ver que, embora os valores exatos não sejam previsíveis, esses eventos ou medições tendem a ter uma distribuição não uniforme, em que certos valores são mais prováveis de ocorrer.

## Gaussianas Normais
Lotação do metrô, pH de vinhos, tamanho de sapatos e pressão arterial são exemplos de variáveis que seguem uma [distribuição normal](https://en.wikipedia.org/wiki/Normal_distribution) (às vezes também chamada de distribuição gaussiana). Isso pode soar familiar das aulas de álgebra ou estatística, mas só para revisar: em uma distribuição normal, nem todos os números são igualmente prováveis de ocorrer; a distribuição tem um *valor esperado* e uma *dispersão* específicos.

O *valor esperado* é o valor médio da distribuição, geralmente denotado pela letra grega *mi* $$\mu$$. Se amostrarmos vários números de um evento normalmente distribuído, esperaríamos que metade dos números fosse maior que a média e a outra metade menor.

A *dispersão* de uma distribuição é oficialmente chamada de *desvio padrão*. Ela é comumente denotada pela letra grega *sigma* $$\sigma$$ e especifica quão prováveis são os números distantes da média. Este gráfico mostra a relação entre a média e o desvio padrão para uma distribuição gaussiana com média $$0$$ e desvio padrão $$\sigma$$:

<div class="scaled-images w75">
  <img src = "{{ '/assets/images/creative-coding/gaussian-00.jpg' | relative_url }}"/>
</div>

O que esse gráfico mostra é que cerca de $$68\%$$ dos valores escolhidos de uma distribuição gaussiana estarão a até $$1$$ desvio padrão da média, cerca de $$95\%$$ estarão a até $$2$$ desvios padrão, e quase todos os valores ($$99.6\%$$ deles) estarão a até $$3$$ desvios padrão da média.

Podemos verificar isso com a função [`randomGaussian()`](https://p5js.org/reference/#/p5/randomGaussian) do p5.js.

Vamos usá-la para repetir as mesmas duas visualizações que usamos acima para a função `random()`.

Em uma dimensão, vamos usar $$0$$ como média e $$\frac{width}{3}$$ como desvio padrão para os diâmetros das nossas elipses. Uma forma de escolher o valor do desvio padrão é pegar o maior valor que gostaríamos que o `randomGaussian()` retornasse e dividi-lo por $$3$$. Queremos que nossas elipses tenham um diâmetro máximo de `width`, então nosso desvio padrão deve ser $$\frac{1}{3}$$ desse valor. Isso deve fazer com que a maioria das elipses fique dentro do canvas:

{% include p5-editor.html id="5X5OnNGP_" %}

Em duas dimensões, queremos que nossas elipses fiquem agrupadas ao redor do centro e, na maioria, permaneçam dentro do canvas. Isso faz com que nossas médias sejam $$\frac{width}{2}$$ e $$\frac{height}{2}$$ e, como gostaríamos que nossa posição `x` ficasse no máximo a $$\frac{width}{2}$$ da média (em $$0$$ ou `width`), dividimos $$\frac{width}{2}$$ por $$3$$ para obter o desvio padrão. A mesma lógica é usada para calcular o desvio padrão da posição `y`, e os valores que vamos usar para $$\sigma$$ são: $$\frac{width}{6}$$ e $$\frac{height}{6}$$.

Do raciocínio acima, podemos ver que uma forma mais geral de escolher valores de desvio padrão ao usar `randomGaussian()` é: $$\sigma = \frac{max - mean}{3}$$.

{% include p5-editor.html id="6f5jIIUOj" %}

Podemos até desenhar alguns marcadores em $$1$$, $$2$$ e $$3$$ desvios padrão para verificar se os valores retornados pela função `randomGaussian()` seguem as distribuições esperadas de $$68\%$$, $$95\%$$ e $$99.6\%$$.

{% include p5-editor.html id="dds08wUTa" %}

No sketch acima, $$68\%$$ das elipses devem estar dentro do menor círculo branco, $$27\%$$ devem estar entre os dois menores círculos brancos, $$4\%$$ entre os dois maiores círculos brancos, e muito raramente vamos ver uma elipse fora do maior círculo branco. Então, $$68\%$$ das elipses estão a $$1$$ $$\sigma$$ da média, $$95\%$$ ($$68\% + 27\%$$) estão a $$2$$ $$\sigma$$ e quase todas estão a $$3$$ $$\sigma$$ da média de $$0$$.

Podemos ver algo semelhante no nosso exemplo bidimensional:

{% include p5-editor.html id="jArRRDRhN" %}

Por fim, vamos ver o que acontece se usarmos uma distribuição gaussiana para escolher os diâmetros das elipses no padrão de grade que vimos no começo desta seção. Vamos tentar $$0$$ como média e $$\frac{gridW}{3}$$ como desvio padrão (já que queremos que o maior diâmetro possível seja `gridW`):

{% include p5-editor.html id="yxSDQp5CL" %}

Como é muito improvável que tenhamos $$2$$ elipses de diâmetro máximo lado a lado, podemos até experimentar valores um pouco maiores de $$\sigma$$, como $$\frac{gridW}{2.5}$$ ou até $$\frac{gridW}{2}$$.

Experimente acima ☝️ e veja se a diferença é perceptível!

## Noise

Distribuições uniformes e gaussianas não são as únicas formas de criar números aleatórios.

Os números aleatórios gerados pelas funções `random()` e `randomGaussian()` são imprevisíveis porque saber algo sobre um dos números não diz nada sobre o próximo. Mas e se quisermos usar sequências de números aleatórios em que cada número esteja de alguma forma relacionado ao valor anterior da sequência?

Claro que o p5.js tem outra função para gerar esse tipo de número aleatório: [`noise()`](https://p5js.org/reference/#/p5/noise).

O noise é diferente das outras funções porque seus parâmetros não são usados para especificar nada sobre a distribuição dos números, ou seu intervalo, e sim para determinar quão próximos devem ser os números escolhidos em sequência.

Por exemplo, chamar o noise com o mesmo parâmetro durante um sketch:
```js
print(noise(1010));
print(noise(1010));
print(noise(1010));
```

Vai dar o mesmo número (sempre entre $$0$$ e $$1$$):
```js
0.63951621
0.63951621
0.63951621
```

Mas o parâmetro pode ser usado para determinar quão *parecidos* dois ou mais números aleatórios consecutivos devem ser.

Por exemplo, chamar o noise com números inteiros consecutivos:
```js
print(noise(1010));
print(noise(1011));
print(noise(1012));
```

Dá uma sequência de números diferentes que estão todos a até $$0.2$$ uns dos outros:
```js
0.40770879
0.49712939
0.29742239
```

Chamando o noise com números que variam apenas $$0.1$$:
```js
print(noise(1010.0));
print(noise(1010.1));
print(noise(1010.2));
```

obtemos uma sequência de números diferentes que são mais parecidos e variam em, talvez, $$0.02$$:
```js
0.36829471
0.38614744
0.39177833
```

*Então... Como usamos isso?*

Bem, o `noise()` é útil porque podemos usar valores que temos no nosso sketch para controlar o quanto os números da sequência vão variar.

Um exemplo simples, parecido com os de cima, seria usar `noise()` para escolher posições `x` e `y` aleatórias para algumas elipses.

Digamos que começamos com algo assim:
```js
for (let i = 0; i < width; i++) {
  let x = width * noise(i);
  let y = height * noise(i);
  ellipse(x, y, 10);
}
```

Como os valores retornados por `noise()` estão no intervalo $$[0, 1]$$, nós os multiplicamos por `width` e `height` para obter valores que se estendem por todo o nosso canvas:

{% include p5-editor.html id="Go0Xi5bXM" %}

## 🤔

Como `noise()` retorna o mesmo valor quando chamado com o mesmo parâmetro, as variáveis $$x$$ e $$y$$ no sketch acima sempre terão o mesmo valor e nossas elipses são desenhadas em uma linha diagonal. Chato.

Uma forma de resolver isso é dar à função `noise()` de cada variável um deslocamento diferente e fixo, assim:
```js
for (let i = 0; i < width; i++) {
  let x = width * noise(1010 + i);
  let y = height * noise(2020 + i);
  ellipse(x, y, 10);
}
```

{% include p5-editor.html id="VOGOgVAa6" %}

Isso é melhor, mas ainda se parece bastante com as funções `random()` ou `randomGaussian()`. Isso acontece porque o parâmetro que estamos passando para a função `noise()` varia de $$1$$ entre as iterações, e da nossa exploração acima vimos que chamar `noise()` com números inteiros consecutivos retorna números que mudam cerca de $$0.2$$ entre chamadas consecutivas.

Isso é bastante. $$0.2$$ é $$\frac{1}{5}$$ do nosso intervalo total, e a sequência resultante pode se parecer muito com `random()` ou `randomGaussian()`.

Podemos deixar números consecutivos mais parecidos incrementando o parâmetro do noise de $$0.01$$ em vez de $$1$$:
```js
for (let i = 0; i < width; i++) {
  let x = width * noise(1010 + i / 100);
  let y = height * noise(2020 + i / 100);
  ellipse(x, y, 10);
}
```

{% include p5-editor.html id="7xkp7ELGy" %}

Uau. Agora tanto a posição `x` quanto a `y` das nossas elipses variam por pequenas quantidades entre cada iteração e, como cada valor está levemente relacionado ao anterior, obtemos algo que parece o rastro de uma elipse em movimento.

Muito legal !

Podemos até aumentar o número de iterações acima de `width` para `4 * width` ou `10 * width` para obter rastros mais longos.

Experimente ☝️ !

Vamos ver como o `noise()` fica na visualização em uma dimensão. Vamos usar `noise(i / 100)` de novo para que nosso diâmetro não varie demais entre as iterações:

{% include p5-editor.html id="FwqCTeqRU" %}

Podemos ver que os diâmetros variam em passos pequenos entre as iterações e quase obtemos padrões quando todas as elipses se sobrepõem.

Vamos experimentar `noise()` no nosso exemplo da grade:

{% include p5-editor.html id="HPub93Kv5" %}

## 🤔

Talvez se usarmos `y / 100` ? ...

{% include p5-editor.html id="oib4-fdue" %}

Ou `(x + y) / 100` ?

{% include p5-editor.html id="Qjh-iKRFm" %}

Mesmo que o último pareça legal, todos esses são previsíveis: ou suas linhas, ou colunas, ou diagonais são iguais. O que faz sentido, já que estamos chamando `noise()` com valores repetidos vindos de `x`, `y` ou `x + y`.

Diferente do caso 2D acima, em que usamos chamadas separadas a `noise()` para obter valores para as posições `x` e `y`, agora gostaríamos de obter *um* valor de diâmetro com base em *dois* valores de posição. Felizmente, podemos simplesmente chamar `noise()` com dois parâmetros, assim:
```js
let d = noise(x / 100, y / 100);
```

Agora, a função `noise()` vai levar as duas sequências em consideração ao retornar um valor, então mesmo que um dos valores permaneça o mesmo, o outro muda e o valor resultante vai mudar:

{% include p5-editor.html id="cLIOZhjLs" %}

Muito legal. Em breve vamos ver como usar essa versão de noise 2D e até 3D.

## Revisão

`random(min, max)`: nos dá valores uniformemente distribuídos entre `min` e `max`, em que todo valor tem a mesma probabilidade de ocorrer.

`randomGaussian(`$$\mu$$`, `$$\sigma$$`)`: nos dá valores normalmente distribuídos agrupados ao redor de $$\mu$$, com a maioria dos valores entre $$\mu - 3\sigma$$ e $$\mu + 3\sigma$$.

`noise(i, j)`: nos dá sequências de valores entre $$0$$ e $$1$$ que não variam muito entre chamadas consecutivas quando `i` e `j` são sequências de valores que aumentam (ou diminuem) gradualmente.

{% include random-examples.html %}
