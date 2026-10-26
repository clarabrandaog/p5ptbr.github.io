---
title: Arrays
---
## Revisão

Antes de começarmos a olhar os *arrays*, o tema principal desta página, vamos revisar alguns conceitos que vão facilitar visualizar o que eles são e como usá-los.

Vimos as variáveis do p5.js [`mouseX`](https://p5js.org/reference/#/p5/mouseX) e [`mouseY`](https://p5js.org/reference/#/p5/mouseY) quando estudamos [variáveis](../variables/), [condicionais](../conditionals/) e [funções](../functions/).

Essas variáveis sempre guardam a posição `x` e `y` do nosso mouse durante a execução do sketch, então, se as usarmos dentro da função `draw()` para desenhar uma elipse, vai parecer que a elipse está seguindo o mouse.

{% include p5-editor.html id="04ZAOiwGT" %}

Ótimo.

Quando estudamos [condicionais](../conditionals/) e [funções](../functions/), também vimos código que detectava a sobreposição entre o mouse e diferentes formas.

Como este código, que muda a cor dos retângulos quando o mouse passa por cima deles:

{% include p5-editor.html id="IOAIaoJ2b" %}

Ou este código, que muda as formas que desenha quando detecta colisão entre a elipse do mouse e um *obstáculo*:

{% include p5-editor.html id="2A9QJrOcR" %}

Também ótimo.

Mas, agora, não estamos tão interessados na lógica de detecção de colisão ou sobreposição. Em vez disso, queremos ver como podemos reescrever o código que define os *obstáculos* nesses sketches de uma forma que facilite adicionar novos obstáculos e mudar os que já existem.

## Listas

Vamos dar uma olhada em uma versão simplificada de um desses sketches, que só desenha elipses. Aqui queremos $$5$$ elipses de obstáculo e, como mais tarde vamos querer usar suas posições para detectar colisão, usaremos $$5$$ variáveis para guardar suas posições `x` e outras $$5$$ variáveis para suas posições `y`.

{% include p5-editor.html id="LAIYvwNxQ" %}

Está tudo certo e funciona bem... para $$5$$ elipses. E se este for o protótipo inicial de um jogo que estamos fazendo e, eventualmente, vamos querer ter $$100$$ obstáculos?

Escrever `x0`, `x1`, `x2`, ..., `x99` para definir todos os valores simplesmente não é prático. E aí, chamar `rect(x0, y0, d)`, `rect(x1, y1, d)`, `rect(x2, y2, d)`, ..., `rect(x99, y99, d)` dentro do `draw()` dá o mesmo trabalho.

Por sorte, o JavaScript, como a maioria das linguagens de programação, tem uma estrutura de dados nativa chamada [`Array`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) que pode nos ajudar a organizar dados assim.

Assim como podemos declarar variáveis para guardar números:
```js
let x = 10;
let y = 20;
let z = 90;
```

Podemos declarar variáveis para guardar arrays:
```js
let a = [];
let b = [];
```

Mas, ao contrário de uma variável comum que guarda apenas um valor, os arrays guardam sequências de valores.

A sintaxe com os colchetes `[` e `]` acima tem uma aparência meio engraçada, mas ela está dizendo ao computador que queremos que as variáveis `a` e `b` sejam arrays, e que esses arrays comecem vazios.

Se, em vez disso, quiséssemos declarar arrays já com valores dentro, esta é a sintaxe:
```js
let ints = [0, 1, 2, 3, 4];
let evens = [0, 2, 4, 6, 8];
let odds = [1, 3, 5, 7, 9];
```

A variável `ints` guarda um array, e esse array tem $$5$$ elementos, os primeiros $$5$$ inteiros não negativos: $$0$$, $$1$$, $$2$$, $$3$$ e $$4$$.

Os arrays `evens` e `odds` também guardam $$5$$ elementos cada, os primeiros $$5$$ números inteiros não negativos pares e ímpares, respectivamente.

Depois de declarar e inicializar um array, podemos acessar seus membros usando colchetes `[` e `]`:

```js
evens[0]; // primeiro valor do array evens: 0
evens[1]; // segundo valor do array evens: 2
evens[2]; // terceiro valor do array evens: 4
// ... etc
```

Vamos ver como podemos usar isso no nosso sketch com os obstáculos.

Em vez de `x0`, `x1`, `y0`, `y1`, etc., vamos declarar arrays para guardar as posições `x` e `y` dos nossos obstáculos, e usar a notação com colchetes para acessar elementos individuais na hora de desenhá-los.

{% include p5-editor.html id="2TzVXz8uN" %}

## Iteradores

No começo isso não parece uma vantagem, porque apenas trocamos `x0`, `x1`, etc. por `x[0]`, `x[1]`, etc., o que dá ainda mais trabalho para digitar.

A vantagem completa de usar arrays vem do fato de que podemos percorrê-los com loops `for()` e evitar escrever código repetitivo como o de cima.

Por exemplo, se quisermos imprimir todos os valores de um array chamado `x`, podemos usar um código como este:

```js
for (let i = 0; i < x.length; i += 1) {
  print(x[i]);
}
```

Isso é novo e pode parecer intimidador no começo, então vamos ver em detalhe.

Primeiro, criamos um loop `for()` que conta de $$0$$ até `x.length` em incrementos de $$1$$. É igual a qualquer loop `for()` que vimos [aqui](../patterns/) ou [aqui](../counters/). A única diferença é que a condição de término do loop (a expressão `i < x.length`) não usa um valor fixo, nem uma das variáveis que têm a ver com as dimensões do canvas, como `width` e `height`.

Em vez disso, nosso loop está usando uma variável que pertence ao nosso array `x` e nos diz quantos elementos há no array. Então, se o nosso array tem $$5$$ elementos, o loop vai executar $$5$$ vezes. Se o nosso array tem $$50$$ elementos, vai executar $$50$$ vezes. Uma vantagem de usar a variável `x.length` em vez de um valor fixo na expressão de término, como `i < 5`, é que assim podemos adicionar ou remover elementos do nosso array sem precisar mudar este código.

No exemplo específico acima, a única coisa que o corpo do loop faz é `print(x[i])`, que imprime o valor do elemento na posição `i` do array. Como `i` é a nossa variável de loop que conta de $$0$$ a $$5$$, o resultado do loop é o mesmo que:

```js
print(x[0]);
print(x[1]);
print(x[2]);
print(x[3]);
print(x[4]);
```

Só que não precisamos escrever todas essas linhas e correr o risco de errar a digitação.

Vamos usar isso no nosso sketch com os obstáculos para ver como podemos evitar escrever código repetitivo.

Depois de declarar nossos arrays com as posições `x` e `y`, vamos iterar por eles para desenhar as elipses.

{% include p5-editor.html id="pycWGw-Rw" %}

Mesmo resultado. Muito menos código.

E, mais importante, um código mais elegante e fácil de manter.

## Estendendo

Agora que estamos desenhando nossos elementos iterando sobre o array com um loop `for()`, podemos facilmente adicionar mais obstáculos ao nosso sketch.

Tudo o que precisamos fazer é adicionar elementos aos arrays `x` e `y`; o resto do código em `draw()` continua exatamente igual.

{% include p5-editor.html id="Af1aWsAuo" %}

Podemos facilmente trazer de volta a função de detecção de sobreposição e mudar a lógica de seleção das cores das elipses para funcionar com os nossos arrays.

Vamos usar uma versão mais simples da função `ellipseOverlap()`:

```js
function ellipseOverlap(x0, y0, diam0, x1, y1, diam1) {
  let d = sqrt((x1 - x0) ** 2 + (y1 - y0) ** 2);
  return d < diam0 / 2 + diam1 / 2;
}
```

Essa função implementa esta lógica do tutorial de [funções](../functions/):

<div class="scaled-images">
  <img src="{{ '/assets/images/p5/overlap-03.jpg' | relative_url }}">
</div>

Ela usa distâncias euclidianas para determinar se duas elipses estão mais próximas do que a soma dos seus raios, e retorna `true` se estão sobrepostas e `false` caso contrário. Ela não desenha os círculos concêntricos como a original porque vamos ver como fazer isso de forma mais elegante quando estudarmos [classes](../classes/).

{% include p5-editor.html id="S--4o9Nzw" %}

## Fora de Controle

E, se enjoarmos de escrever as coordenadas `x` e `y` à mão, podemos usar o que aprendemos sobre [`random()`](../random/) para criar posições aleatórias para os nossos obstáculos dentro do `setup()`.

Podemos começar com arrays `x` e `y` vazios:
```js
let x = [];
let y = [];
```

Depois, podemos usar um loop `for()` para contar até $$5$$, e `random()` para obter $$5$$ pares de coordenadas aleatórias. Quando tivermos as coordenadas, podemos adicioná-las ao final dos arrays usando a função especial de array [`push()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push).

Por exemplo, assim adicionamos os números $$10$$, $$12$$ e $$14$$ ao final de um array chamado `evens`:
```js
evens.push(10);
evens.push(12);
evens.push(14);
```

E assim adicionamos uma coordenada aleatória entre $$0$$ e `width` ao final do nosso array `x`:
```js
let mx = random(0, width);
x.push(mx);
```

Podemos usar algo assim para as coordenadas `x` e `y` no nosso sketch. E, além de mudar como as coordenadas das elipses são inicializadas, não precisamos mudar mais nada no nosso sketch. A função `draw()` é exatamente a mesma de antes.

{% include p5-editor.html id="w1xoYIYbz" %}

Vamos ver como fazer com que os obstáculos não se sobreponham quando falarmos de [classes](../classes/), mas, por enquanto, arrays FTW!
