---
title: Loops como Contadores
---
## Loop para Preencher

Vimos alguns exemplos de como usar loops `for()` para desenhar padrões em grade [aqui](../patterns).

Agora vamos explorar outra forma de pensar sobre loops, que nos permitirá fazer coisas um pouco diferentes.

Vimos como desenhar formas repetidas usando loops com uma posição inicial (ou deslocamento), uma posição final e um espaçamento fixo:

```js
for(let x = startPosition; x < endPosition; x += spacing) {
  drawShape();
}
```

{% include p5-editor.html id="KjD5w25YP" %}

Neste exemplo especificamos onde no nosso canvas queremos que o desenho comece e termine. É como se estivéssemos especificando a área que queremos preencher com formas.

E se, em vez disso, quisermos especificar o número de formas que devem **CABER** em um espaço?

## Loop para Contar

Uma forma de pensar nisso é que o comando `for()` é um contador, contando de `startPosition` até `endPosition` em incrementos de `spacing`. Pensar no comando `for()` como um "contador" facilita ver como usá-lo em situações que não têm a ver com a posição de formas.

Podemos usá-lo para repetir uma ação um número determinado de vezes:

{% include p5-editor.html id="JFR877sol" %}

Ou para fazer contas com sequências de números, como encontrar a soma de todos os números entre $$N_{0}$$ e $$N_{1}$$;

{% include p5-editor.html id="YBCKfi7Ct" %}

Ou para percorrer uma coleção de itens, em um [array](#):

{% include p5-editor.html id="yqdys8Sff" %}

Vamos explorar a situação em que queremos repetir algo um número determinado de vezes e, em vez de desenhar quadrados por todo o canvas, queremos apenas linhas com $$5$$ quadrados:

{% include p5-editor.html id="drA7SCEvq" %}

Isso é diferente de antes. Não estamos iterando por valores de pixel e contando de uma *posição* inicial até uma *posição* final; agora estamos contando o número de quadrados a desenhar, de $$0$$ a $$4$$.

Para cada quadrado que queremos desenhar, temos que fazer uma pequena conta antes para descobrir onde colocá-lo: se o primeiro está em $$0$$, o próximo deve estar em $$0 + spacing$$, e o seguinte em $$0 + spacing + spacing$$... e assim por diante... em outras palavras, o $$i$$-ésimo quadrado está na posição $$i * spacing$$. Se houver um deslocamento para o primeiro quadrado, ele pode simplesmente ser somado à equação, como se vê no caso da segunda linha de quadrados no código acima.

## Loop para Encaixar

Se, em vez disso, queremos desenhar um número **FIXO** de quadrados em uma quantidade **FIXA** de espaço, precisamos descobrir como espaçar os quadrados. Em outras palavras, o nosso contador vai de $$0$$ a $$10$$ e queremos posicionar nossos quadrados entre $$0$$ e `width` ou entre `offset` e `width`.

Poderíamos fazer a conta à mão. No caso mais simples, a posição $$x$$ dos nossos quadrados é dada por:

$$x = \frac{i}{10} \times width$$.

No caso mais complexo, com o deslocamento, fica:

$$x = \frac{i}{10} \times (width - offset) + offset$$.

Mas também podemos usar a função [`map()`](https://p5js.org/reference/#/p5/map) do p5.js para fazer essa conta por nós e manter o nosso código mais fácil de ler e manter (uma explicação detalhada do `map()` pode ser encontrada [aqui](../../creative-coding/maths/)).

{% include p5-editor.html id="sbt2pVmzK" %}

Agora, e se quisermos desenhar $$15$$ quadrados em vez de $$10$$?

{% include p5-editor.html id="23jR5o-ij" %}

Usar apenas o `map()` não é suficiente; vemos que há espaços vazios na linha de cima e sobreposição na de baixo.

Para consertar isso, antes do loop temos que calcular uma nova largura para cada um dos dois casos, com base no número de quadrados que queremos e no espaço que temos para preencher:

{% include p5-editor.html id="TKAcfArNy" %}

## Revisão de Todas as Possibilidades

Nos casos em que estamos desenhando formas entre uma posição inicial e uma posição final pré-definidas e específicas, com um espaçamento fixo, podemos simplesmente iterar sobre os valores de pixel e desenhar:

```js
for(let pos = start; pos < end; pos += spacing) {
  // desenha
}
```

Quando queremos desenhar um número específico de formas, com um espaçamento *dado*, e não nos importamos com quanto espaço isso pode ocupar, podemos usar um loop `for()` como contador e calcular a posição a partir do espaçamento:

```js
for(let count = 0; count < maxCount; count += 1) {
  let pos = offset + count * spacing;
  // desenha
}
```

Quando queremos desenhar um número específico de formas, dentro de uma quantidade *fixa* de espaço, sem saber o espaçamento de antemão, podemos usar um loop `for()` como contador e usar o `map()` para calcular a posição de cada forma:

```js
for(let count = 0; count < maxCount; count += 1) {
  let pos = map(count, 0, maxCount, minPos, maxPos);
  // desenha
}
```

Se queremos desenhar um número específico de formas, dentro de uma quantidade *fixa* de espaço, sem saber o espaçamento de antemão, e evitar sobreposições ou espaços vazios, temos que calcular o espaçamento com base na largura disponível e no número de formas; então podemos usar um loop `for()` como contador e fazer uma pequena conta para calcular a posição a partir do espaçamento:

```js
let spacing = availableWidth / maxCount;
for(let count = 0; count < maxCount; count += 1) {
  let pos = offset + count * spacing;
  // desenha
}
```

## Tabuleiro de Xadrez

Vamos praticar um pouco disso e desenhar um tabuleiro de xadrez, centralizado no nosso canvas, com alguma margem ao redor.

Vamos começar desenhando uma linha do nosso tabuleiro. Como o número de quadrados é fixo ($$8$$), a quantidade de espaço que eles ocupam também é fixa ($$width - 2 * margin$$) e queremos evitar sobreposições, vamos usar o último método:

```js
let spacing = availableWidth / maxCount;
for(let count = 0; count < maxCount; count += 1) {
  let pos = offset + count * spacing;
  // desenha
}
```

{% include p5-editor.html id="y-LqGkGbt" %}

O espaçamento e o tamanho estão bons. Só precisamos consertar a cor. Queremos que os quadrados pretos e brancos se alternem, o que significa pintar os quadrados de número par de preto e os de número ímpar de branco. Como temos uma variável que registra qual quadrado estamos desenhando (`col`), podemos simplesmente verificar se ela é um número par (`col % 2 == 0`) e selecionar a cor de acordo:

{% include p5-editor.html id="syltJiw2g" %}

Agora só precisamos aninhar alguns loops `for()` para obter todas as nossas linhas, e terminamos:

{% include p5-editor.html id="mIbZgIy0y" %}

As cores dos quadrados estão se alternando na direção horizontal porque estamos verificando se `col` é par, mas, para obter o padrão correto, também devemos alternar o padrão por linha.

Se enumerarmos todas as combinações possíveis de pares entre par/ímpar da coluna e da linha, o pseudocódigo da lógica poderia ser algo como:

```js
if (col is even && row is even) {
  fill(black);
} else if (col is even && row is odd) {
  fill(white);
} else if (col is odd && row is even) {
  fill(white);
} else if (col is odd && row is odd) {
  fill(white);
}
```

Então, se o índice da linha e o da coluna são ambos pares, ou ambos ímpares, pintamos de preto. Se um é ímpar e o outro é par, pintamos de branco.

O pseudocódigo acima pode ser simplificado para:

```js
if (col evenness == row evenness) {
  fill(black);
} else {
  fill(white);
}
```

E em JavaScript `if(col % 2 == row % 2)` vai verificar se a *paridade* dos valores é a mesma:

{% include p5-editor.html id="rbQjyjGJf" %}

Tente mudar o tamanho do canvas no sketch acima. Desde que o canvas seja um quadrado, o nosso código deve funcionar como esperado.
