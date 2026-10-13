---
title: Mais Trigonometria e Vetores
---
Acabamos de ver como usar as funções [`sin()` e `cos()`](../sincos/) para criar ciclos.

Vamos agora ver outras formas de usar essas e outras [funções trigonométricas](https://en.wikipedia.org/wiki/Trigonometric_functions) nos nossos sketches.

Como revisão: as funções trigonométricas são funções que, junto com o [teorema de Pitágoras](https://en.wikipedia.org/wiki/Pythagorean_theorem), definem relações entre os ângulos de um triângulo retângulo e o comprimento de seus lados:

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/trigonometry-00.jpg' | relative_url }}"/>
</div>

## Coordenadas Polares

Podemos usar essas relações para derivar fórmulas de tradução entre os sistemas de coordenadas [cartesiano](https://en.wikipedia.org/wiki/Cartesian_coordinate_system) e [polar](https://en.wikipedia.org/wiki/Polar_coordinate_system).

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/trigonometry-01.jpg' | relative_url }}"/>
</div>

Coordenadas cartesianas são o que usamos para especificar pontos em um plano (e pixels em uma tela) usando dois números que representam distâncias em direções perpendiculares. Coordenadas polares também especificam pontos em um plano, mas usando uma distância e um ângulo.

Assim como podemos usar loops `for()` para iterar sobre as coordenadas cartesianas $$(x, y)$$ e criar padrões em uma grade, também podemos iterar sobre as duas variáveis de um sistema de coordenadas polares para criar padrões.

Neste exemplo, nosso loop `for()` externo itera por diferentes valores de quão longe queremos estar do centro do canvas, em passos de $$20$$ pixels, e o loop `for()` interno itera por todos os ângulos possíveis a partir do eixo x positivo, em passos de $$1^{\circ}$$:

<div class="scaled-images left w100">
  <img src = "{{ '/assets/images/creative-coding/trigonometry-02.jpg' | relative_url }}"/>
</div>

{% include p5-editor.html id="bmn6wQ1G4" %}

Este é bem simples e poderia ter sido feito desenhando elipses concêntricas. Se destrinçarmos um pouco da nossa lógica, vamos ver que estamos implementando as [funções trigonométricas para definir um círculo](https://en.wikipedia.org/wiki/Unit_circle#Trigonometric_functions_on_the_unit_circle).

E é basicamente isso que os loops `for()` estão fazendo: desde que mantenhamos o ângulo usado para calcular `x` igual ao ângulo usado para calcular `y`, estamos implementando a [equação paramétrica de um círculo](https://en.wikipedia.org/wiki/Circle#Parametric_form):

$$\begin{align*} x &= r \cdot cos(a) \\
y &= r \cdot sin(a)\end{align*}$$

Mas e se movermos o ângulo no nosso cálculo de `x` $$3$$ vezes mais rápido que o ângulo de `y`?

Se mudarmos apenas isto:
```js
let x = r * cos(radians(a));
```

para isto:
```js
let x = r * cos(radians(3 * a));
```

obtemos isto:

{% include p5-editor.html id="rubKcJet3" %}

Pois é... coordenadas polares FTW! Não acho que dê para desenhar *isso* com círculos.

Podemos brincar com esse valor e ver como o desenho muda. Também podemos adicionar um multiplicador diferente ao ângulo de `y`:
```js
let x = r * cos(radians(3 * a));
let y = r * sin(radians(7 * a));
```

{% include p5-editor.html id="GEZiG_ZtF" %}

Experimente alguns valores novos para os multiplicadores de ângulo no código acima ☝️. Legal, né?

Ou use alguma outra coisa, como o valor de `r`, para incrementar o ângulo:
```js
let x = r * cos(radians(a + 2 * r));
let y = r * sin(radians(a));
```

{% include p5-editor.html id="0_Y0yn7F7" %}

Ou ambos:
```js
let x = r * cos(radians(2 * a + 2 * r));
let y = r * sin(radians(a));
```

{% include p5-editor.html id="DxBh1M06F" %}

## Dimensões

Além de criar padrões concêntricos inesperados, também podemos usar coordenadas polares para transformar sinais unidimensionais em sinais bidimensionais.

*O quê?*

Digamos que temos um sinal unidimensional, como o tempo, ou o número de pessoas em uma sala, e queremos criar uma visualização bidimensional desses dados.

Podemos facilmente usar coordenadas polares para *mapear* um único valor para um ponto no nosso canvas. Isso também é possível usando coordenadas cartesianas diretamente (como vamos ver quando começarmos a processar [imagens](../images/)), mas usar coordenadas polares é um pouco mais simples e talvez mais intuitivo.

Vamos começar usando uma única variável de um loop `for()` para derivar nossos valores de `r` e os ângulos `a`.

Se fizermos `r` crescer proporcionalmente ao ângulo, obtemos uma espiral:

{% include p5-editor.html id="CJx8B5IdN" %}

E se aumentarmos a frequência dos ângulos, como fizemos antes multiplicando-os por uma constante, e também aumentarmos o intervalo do nosso `r`:
```js
let r = map(a, 0, 360, 10, 1.4 * maxRadius);
let x = r * cos(radians(8 * a));
let y = r * sin(radians(8 * a));
```

Obtemos uma espiral mais apertada que vai até os cantos do nosso canvas:

{% include p5-editor.html id="TReOUnK3F" %}

Também podemos adicionar um deslocamento aos nossos ângulos (isso costuma ser chamado de [*fase*](https://en.wikipedia.org/wiki/Phase_(waves)) de um ângulo) que muda onde a espiral começa:
```js
let x = r * cos(radians(8 * a - phaseVal));
let y = r * sin(radians(8 * a - phaseVal));
```

É meio sutil, mas se executarmos o sketch a seguir de novo e olharmos o centro da espiral, vamos notar o efeito do valor da fase:

{% include p5-editor.html id="jHVl6Ujpf" %}

É mais fácil ver seus efeitos se esse deslocamento aumentar ou diminuir a cada frame:

```js
let x = r * cos(radians(8 * a - 8 * frameCount));
let y = r * sin(radians(8 * a - 8 * frameCount));
```

{% include p5-editor.html id="wZBKzbmpg" %}

Nosso canvas está girando devagar, mas a forma faz parecer que é uma linha sendo desenhada infinitamente, ou círculos que estão crescendo! Podemos brincar com o multiplicador de fase acima ☝️ para fazer a espiral girar mais rápido, mais devagar e até para trás.

Se deixarmos os ângulos de `x` e `y` levemente fora de sincronia, obtemos um efeito quase 3D:
```js
let x = r * cos(radians(9 * a - 8 * frameCount));
let y = r * sin(radians(8 * a - 8 * frameCount));
```

{% include p5-editor.html id="WaUKMrtyz" %}

## Equações Paramétricas

Ok, nesses últimos exemplos com animações estávamos mapeando *duas* variáveis, nosso iterador do loop e `frameCount`, para *duas* outras variáveis, `x` e `y`.

Vamos voltar a mapear *uma* variável independente para as posições `x` e `y`.

Equações que fazem isso às vezes são chamadas de [Equações Paramétricas](https://en.wikipedia.org/wiki/Parametric_equation). Nessas equações, a relação entre `x` e `y` não é definida explicitamente, mas seus valores estão relacionados por meio de uma terceira variável, o nosso sinal independente.

Alguns exemplos de equações paramétricas são:

### [Curvas de Lissajous](https://en.wikipedia.org/wiki/Lissajous_curve).
Já vimos essas antes, mas este é o seu nome oficial e suas equações:

$$\begin{align*} x &= A \cdot cos(\alpha t + \delta) \\
y &= B \cdot sin(\beta t) \end{align*}$$

onde $$A$$, $$B$$, $$\alpha$$, $$\beta$$ e $$\delta$$ são apenas constantes diferentes.

{% include p5-editor.html id="GYdDCpGed" %}

Podemos mudá-las acima ☝️ e ver o que acontece com o desenho.

### [A Curva Borboleta](https://en.wikipedia.org/wiki/Butterfly_curve_(transcendental)):

$$\begin{align*}
x &= sin(t) \cdot \left(\exp^{cos(t)} - 2 \cdot cos(4t) \right) \\
y &= cos(t) \cdot \left(\exp^{cos(t)} - 2 \cdot cos(4t) \right)
\end{align*}$$

{% include p5-editor.html id="BguyBnLPD" %}

### [Curvas Cardioides](https://en.wikipedia.org/wiki/Cardioid):

$$\begin{align*}
x &= r \cdot \left(1 - cos(\alpha \cdot t) \right) \cdot cos(t) \\
y &= r \cdot \left(1 - cos(\alpha \cdot t) \right) \cdot sin(t)
\end{align*}$$

onde $$\alpha$$ é apenas uma constante inteira que podemos mudar.

{% include p5-editor.html id="SrvjUkELi" %}

Mude `alphaVal` acima ☝️ e veja o que acontece!

Ou, melhor ainda, vamos fazer nosso `loop()` aumentar o valor de `alphaVal`:

{% include p5-editor.html id="LxBye1J6n" %}

### [Curva Bônus](https://mathworld.wolfram.com/HeartCurve.html):

E, por fim, este tipo especial de curva cardioide:

$$\begin{align*} x &= 16 \cdot sin^3(t) \\
y &= 13 \cdot cos(t) - 5 \cdot cos(2t) - 2 \cdot cos(3t) - cos(4t) \end{align*}$$

{% include p5-editor.html id="gGk2wUk3b" %}

Muito legal!

## Distâncias entre Pontos / Distâncias Euclidianas / Distâncias L2

Coordenadas polares são muito divertidas e também podem ser muito úteis quando precisamos calcular a distância ou o ângulo entre dois pontos.

Se imaginarmos dois pontos na nossa tela, com coordenadas $$(x_0, y_0)$$ e $$(x_1, y_1)$$, podemos obter a distância entre eles usando o teorema de Pitágoras:

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/trigonometry-03.jpg' | relative_url }}"/>
</div>

Neste sketch, a distância entre dois pontos em movimento é calculada usando a fórmula da [Distância Euclidiana](https://en.wikipedia.org/wiki/Euclidean_distance) $$\sqrt{(x_1 - x_0)^2 + (y_1 - y_0)^2}$$ e a função [`dist()`](https://p5js.org/reference/#/p5/dist) do p5.js. Quando essas distâncias são usadas como diâmetro para dois círculos centralizados no canvas, podemos ver que são exatamente iguais:

{% include p5-editor.html id="ZpMRkDtCM" %}

## Ângulos

Da mesma forma, podemos usar a fórmula que calcula o ângulo polar de um ponto para obter o ângulo entre dois pontos:

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/trigonometry-04.jpg' | relative_url }}"/>
</div>

Ou o ângulo entre um ponto e ele mesmo no futuro.

Se um ponto em movimento em $$(x, y)$$ tem velocidade $$v_x$$ e $$v_y$$, sua posição em um futuro próximo será $$(x + v_x, y + v_y)$$. Podemos calcular o ângulo entre o ponto agora e o ponto no futuro para obter seu ângulo de direção (heading):

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/trigonometry-05.jpg' | relative_url }}"/>
</div>

Podemos usar o ângulo de direção de um objeto em movimento para rotacionar sua forma e enfatizar sua direção de movimento:

{% include p5-editor.html id="SqaUjqGvq" %}

## Vetores

O p5.js na verdade tem uma classe chamada [`Vector`](https://p5js.org/reference/#/p5.Vector) que pode ajudar com cálculos geométricos como esses. A forma mais simples de pensar em um vetor é que ele especifica um ponto $$(x, y)$$ no espaço, relativo à origem $$(0, 0)$$. Vetores são na verdade mais do que isso, mas para fazer os cálculos de distância e ângulo que vimos, não há problema em pensar nos vetores como pontos com uma coordenada $$(x, y)$$.

Este desenho mostra dois vetores/pontos e, se subtrairmos $$_1$$ de $$v_0$$, obtemos um terceiro vetor que guarda informações sobre a distância e a direção entre eles.

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/trigonometry-06.jpg' | relative_url }}"/>
</div>

Os vetores do p5.js na verdade têm funções embutidas para calcular distâncias e ângulos. Podemos criar dois vetores e obter a distância entre eles assim:
```js
let v0 = createVector(20, 10);
let v1 = createVector(10, 20);
let d = v0.distance(v1);
```

e o ângulo assim:
```js
let v0 = createVector(20, 10);
let v1 = createVector(10, 20);
let d = v0.angleBetween(v1);
```

Vamos usar vetores para refazer alguns dos sketches acima.

## Distâncias com Vetores

Em vez de colocar objetos JavaScript com $$4$$ parâmetros $$(x, y, vx, vy)$$, agora temos objetos com dois vetores, um para a posição e outro para a velocidade dos objetos.

No draw, onde tínhamos `p.x`, agora teremos que usar `p.loc.x`. Da mesma forma, para a velocidade, `p.vx` se torna `p.vel.x`. Pode ser um pouco mais de letras para digitar, mas as quantidades relacionadas estão em um vetor, o que ajuda a fazer algumas contas, como atualizar a posição.

Em vez disto:
```js
p.x += p.vx;
p.y += p.vy;
```

Podemos simplesmente fazer:
```js
p.loc.add(p.vel);
```

E a distância entre os dois círculos agora pode ser calculada com:
```js
ps[0].loc.dist(ps[1].loc);
```

{% include p5-editor.html id="9xx13qjZP" %}

## Ângulos com Vetores

Para os ângulos, é bem parecido. Sempre podemos chamar `v0.angleBetween(v1)` para obter o ângulo entre dois vetores. Só precisamos pensar entre quais $$2$$ vetores precisamos do ângulo.

No exemplo em que estamos tentando obter ângulos de direção, o vetor que aponta na direção do movimento é `p.vel`. Como no p5.js um ângulo de $$0^\circ$$ aponta na direção ao longo do eixo x, o ângulo pelo qual precisamos rotacionar nossos objetos vai ser relativo a esse vetor de referência de rotação zero.

O código é praticamente o mesmo do sketch anterior, com a soma de vetores para atualizar a posição, mas agora fazemos:
```js
let xAxis = createVector(1, 0);
let hAngle = xAxis.angleBetween(p.vel);
```

Dessa forma obtemos os ângulos de direção relativos ao ponto de referência de rotação zero, e então usamos esse valor para rotacionar nossos objetos.

{% include p5-editor.html id="68Q5wrokg" %}
