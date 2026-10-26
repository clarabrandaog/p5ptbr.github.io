---
title: Objetos JavaScript
---
## Revisão de Arrays
Vamos começar com o sketch que estávamos vendo quando falamos sobre [arrays](../arrays/).

Este sketch tem $$9$$ elipses de obstáculo, e cada obstáculo tem sua própria posição `x` e `y`, guardada nos arrays `x` e `y`. Essas posições são definidas quando os arrays são declarados, fora das funções `setup()` e `draw()`.

Quando queremos desenhar um obstáculo, pegamos valores dos arrays `x` e `y` usando o mesmo valor de *índice*. Por exemplo, a posição do $$4^{o}$$ obstáculo é dada por `(x[3], y[3])`.

{% include p5-editor.html id="S--4o9Nzw" %}

Digamos que agora queremos que cada obstáculo também tenha seu próprio diâmetro.

Fácil. Vamos declarar um novo array vazio chamado `diameter` e, dentro da função `setup()`, vamos preenchê-lo com tantos valores de diâmetro quantos membros houver no array `x`. Podemos até usar a função `random()` de novo para dar a todos diâmetros diferentes entre `oDiam / 2` e `oDiam`.

```js
for (let i = 0; i < x.length; i += 1) {
  let md = random(oDiam / 2, oDiam);
  diameter.push(md);
}
```

Verificar a sobreposição fica assim:
```js
ellipseOverlap(mouseX, mouseY, mDiam, x[i], y[i], diameter[i]);
```

E desenhar todos os obstáculos fica assim:
```js
for (let i = 0; i < x.length; i += 1) {
  ellipse(x[i], y[i], diameter[i]);
}
```

{% include p5-editor.html id="XK9Pc24G0" %}

Legal. Isso funciona.

## Mais Parâmetros

E agora, se quisermos dar a cada obstáculo uma cor de preenchimento e uma cor de sobreposição diferentes... poderíamos continuar adicionando arrays:

```js
let fColor = [];
let oColor = [];
```

Mas isso pode ficar muito confuso muito rápido.

E se esquecermos de adicionar um parâmetro para um dos obstáculos?

E se trocarmos os valores de `x` e `y` de um dos obstáculos?

E se precisarmos reorganizar a ordem de um dos arrays, como replicar a mudança nos outros arrays?

Quando o objeto que estamos tentando representar começa a ter muitos parâmetros diferentes, podemos aproveitar outra estrutura de dados do JavaScript: o [Objeto](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object).

Os objetos JavaScript são parecidos com os arrays, no sentido de que ajudam a manter valores relacionados juntos usando uma única variável. Por exemplo, para os nossos obstáculos, poderíamos declarar um único objeto `obstacle` assim, com os dados organizados em pares *chave*-*valor*:

```js
let obstacle = {
  x: 80,
  y: 115,
  diameter: 80,
  fColor: color(255),
  oColor: color(100)
};
```

Isso está dizendo ao computador que queremos ter uma variável chamada `obstacle`, e que essa variável tem outras variáveis dentro dela, e que essas variáveis têm valores que devem ser mantidos próximos uns dos outros. A vantagem de fazer assim é que nós, e o computador, podemos pensar sobre esses dados de forma mais eficiente se eles estiverem juntos e rotulados.

Isto:
```js
let obstacle = {
  x: 80,
  y: 115,
  diameter: 80,
  fColor: color(255),
  oColor: color(100)
};
```

é mais fácil de pensar do que isto:
```js
let obstacle = [80, 115, 80, color(255), color(100)];
```

e mais fácil de fazer cálculos do que isto:
```js
let x = [80, 210, 355, 45];
let y = [115, 240, 55, 340];
let diameter = [80, 50, 60, 56];
let fColor = [color(255), color(255), color(255), color(255)];
let oColor = [color(100), color(100), color(100), color(100)];
```

Se os objetos são como variáveis que têm outras variáveis dentro, como acessamos seus valores?

Assim como os arrays têm uma sintaxe especial para acessar seus membros por índice usando colchetes (`[]`), os objetos têm uma sintaxe especial para acessar membros pelo nome. Se quisermos obter a posição `x` do nosso obstáculo, podemos usar: `obstacle.x` ou `obstacle["x"]`. Da mesma forma, o diâmetro seria: `obstacle.diameter` ou `obstacle["diameter"]`.

Vamos dar preferência à *notação de ponto*, mas, se tivermos objetos cujos membros têm nomes com espaços ou caracteres especiais, vamos ter que usar a notação com colchetes `[]`.

## Muitos Objetos

Agora vamos combinar arrays e objetos e criar um array vazio para guardar os nossos objetos obstáculo:

```js
let allObstacles = [];
```

Depois, na nossa função `setup()`, podemos usar um loop `for()` para adicionar $$9$$ obstáculos a esse array assim:

```js
for (let i = 0; i < 9; i += 1) {
  let anObstacle = {
    x: random(width),
    y: random(height),
    diameter: random(40, 80),
    fColor: color(random(255)),
    oColor: color(255, 190, 90)
  };

  allObstacles.push(anObstacle);
}
```

O loop executa $$9$$ vezes e, a cada vez, valores aleatórios são escolhidos para as coordenadas `x` e `y`, a cor de preenchimento e o diâmetro. Além disso, todos os obstáculos têm a mesma cor de sobreposição.

O resto do nosso código é bem parecido, mas agora, em vez de acessar parâmetros em arrays diferentes usando um índice, como:

```js
ellipse(x[i], y[i], diameter[i]);
```

Primeiro pegamos o nosso objeto do array e depois acessamos suas propriedades pelo nome:

```js
let anObstacle = obstacles[i];
ellipse(anObstacle.x, anObstacle.y, anObstacle.diameter);
```

O resto é bem parecido. Só precisamos lembrar de também pegar as cores de preenchimento e de sobreposição do próprio objeto antes de desenhar a elipse.

```js
if (mOverlap) {
  fill(anObstacle.oColor);
} else {
  fill(anObstacle.fColor);
}

ellipse(anObstacle.x, anObstacle.y, anObstacle.diameter);
```

{% include p5-editor.html id="7v8q-YF94" %}

Isso é poderoso porque agora temos uma forma de endereçar cada um dos nossos objetos de uma vez, com todos os seus parâmetros. Não precisamos manter vários arrays com parâmetros separados espalhados pelo nosso código.

Aqui está uma lista de obstáculos. Cada membro dessa lista tem todos os seus parâmetros dentro dele.

## Mantendo Tudo Junto

Uma vantagem de manter todas as informações sobre um obstáculo juntas é que fica mais fácil acessá-las todas de uma vez, ou fazê-las ter alguma relação entre si, ou reorganizar a ordem em que são desenhadas.

Digamos que, em vez de $$9$$ obstáculos, queremos desenhar $$590$$.

E também queremos que a cor dos obstáculos esteja relacionada ao seu diâmetro, e queremos desenhar os obstáculos maiores primeiro e os menores por cima deles.

Vamos começar ajustando a cor.

Quando criamos o obstáculo e escolhemos um valor aleatório para o diâmetro, também vamos criar uma variável que guarda o diâmetro como uma porcentagem do diâmetro máximo. Quando o diâmetro aleatório está perto do máximo, `pctDiam` vai estar perto de $$1.0$$; quando é menor, vai estar mais perto de $$0$$.

```js
let oDiam = random(maxDiam / 2, maxDiam);
let pctDiam = oDiam / maxDiam;
```

{% include p5-editor.html id="FScgsqLKX" %}

Ok. A cor funciona. Mas ainda queremos desenhar os diâmetros maiores primeiro.

Para fazer isso, temos que ordenar o array de obstáculos pelo diâmetro. Os arrays JavaScript têm uma função nativa para isso chamada [`sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort). Se o nosso array tivesse apenas números ou palavras, tudo o que teríamos que fazer era chamar `allObstacles.sort()`, mas, como o nosso array tem um monte de objetos personalizados, temos que dizer à função `sort()` como ordená-los.

Vamos ver como fazer isso em detalhe mais tarde, mas envolve dar ao `sort()` uma função que especifica por qual parâmetro dos nossos objetos queremos ordenar.

É isso que esta função faz:

```js
function byDiameter(obsA, obsB) {
  return obsB.diameter - obsA.diameter;
}
```

E quando chamamos o `sort()` assim, ele vai ordenar os obstáculos pelo diâmetro, do maior para o menor:
```js
allObstacles.sort(byDiameter);
```

{% include p5-editor.html id="l27k2dsat" %}

A seguir, vamos ver como organizar nossos objetos para mudar mais facilmente suas formas e cores quando eles colidem.
