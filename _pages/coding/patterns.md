---
title: Loops como Padrões
---
## Em Repetição

É muito comum, na programação, querer repetir cálculos com dados/entradas diferentes. Por exemplo, ao processar imagens ou som, muitas vezes vamos querer reexecutar o mesmo código em pixels/amostras diferentes. Criar padrões visuais ou sonoros também envolve repetir alguma computação várias vezes, com pequenas modificações.

Existem muitas formas de fazer o nosso programa reexecutar partes do código, como os comandos `while` e `do`/`while`, e as [list comprehensions](https://en.wikipedia.org/wiki/List_comprehension), mas a forma mais simples, e a menos propensa a bugs traiçoeiros, é o loop `for`.

## Loops For

Vamos dar uma olhada nos loops for e em como parametrizá-los e aninhá-los.

Primeiro, vamos começar com um sketch simples que desenha círculos:

{% include p5-editor.html id="DZtkQ823q" %}

Mas, 😱 ....  tem tanto código repetido e tanta coisa que dá para melhorar aqui...

Primeiro, variáveis! Vamos fazer com que possamos mudar facilmente o diâmetro dos círculos, a posição inicial e o espaçamento:

{% include p5-editor.html id="ZtfI2lnFnE" %}

Melhor ! Assim podemos mudar facilmente o tamanho dos nossos círculos (`cDiam`), onde eles começam (`xOffset`), a distância entre seus centros (`xSpacing`) e sua posição vertical (`yPos`). Reserve um tempo para mudar essas variáveis e rodar o código de novo, para ver exatamente o que cada uma faz. Divertido, né? Mas ainda estamos repetindo um monte de código!

Os loops `for()` ao resgate! Podemos refatorar o código acima usando esta estrutura:

```js
for(start; condition; increment) {
  // faz coisas
}
```

Onde:
- `start` é uma expressão que inicializa as variáveis que vamos usar no nosso loop
- `condition` é uma expressão booleana que especifica o que mantém o loop rodando
- `increment` é uma expressão que roda no final de cada iteração

{% include p5-editor.html id="dkYky6guC" %}

O desenho resultante é um pouco diferente; os círculos saem direto para fora do canvas. Podemos tentar controlar isso de algumas formas.

A primeira forma é usar a nossa variável `xOffset` para parar o loop for quando `xPos` chegar a `width - xOffset` em vez de `width`:

{% include p5-editor.html id="9d0QfkWQx" %}

A segunda forma exige mais mudanças no código, para acompanharmos o número de círculos em vez de onde eles estão sendo desenhados:

{% include p5-editor.html id="4SwKr6hBz" %}

Tem muita coisa nova aí. Declaramos uma nova variável `numCircles` que define quantos círculos queremos desenhar (7 por enquanto). Na nossa instrução `for` agora temos `cCnt`, que registra o número de círculos desenhados. Ela itera de 0 até `numCircles`. Neste caso, vai de $$0$$ a $$6$$. *Espera... como assim!? Não estávamos desenhando 7 círculos??* Sim!, mas se contarmos de $$0$$ a $$6$$ $$(0, 1, 2, 3, 4, 5, 6)$$ são na verdade $$7$$ números. Isso é meio confuso no começo, mas vamos ver muito isso e nos acostumar com o fato de que, na programação, normalmente começamos a contar do 0.

E isto: `let xPos = xOffset + cCnt * xSpacing;`?

É assim que calculamos a posição x dos nossos círculos com base em quantos círculos já desenhamos no nosso loop. Na primeira vez que o loop executa, `cCnt` é 0, então `xPos = xOffset + 0 * xSpacing` ou `xPos = xOffset`. Isso parece certo.

Na segunda iteração isso se torna `xPos = xOffset + 1 * xSpacing`. A posição x do centro do segundo círculo é o deslocamento inicial mais 1 espaçamento entre os centros dos círculos. Na terceira iteração é `xPos = xOffset + 2 * xSpacing`, porque há o deslocamento inicial mais 2 espaçamentos para os 2 círculos já desenhados. Podemos brincar com os valores das variáveis para ter uma melhor noção da matemática por trás desse cálculo.

É importante parar um momento aqui e entender a diferença entre os dois métodos de desenhar os círculos. No primeiro caso, a nossa variável de loop e a condição de parada se baseiam na posição dos círculos e, no segundo caso, se baseiam no número de círculos desenhados.

Vamos voltar à primeira forma (por posição) e adicionar algumas variáveis para fazer o loop na outra direção:

{% include p5-editor.html id="vR8b0db59" %}

Reserve um tempo para brincar com as variáveis e ver como elas afetam o nosso desenho.

## Loops For Dentro de Loops For

E agora, digamos que, quando estamos fazendo o loop na direção horizontal, em vez de desenhar um círculo para cada posição de `xPos`, queremos desenhar uma coluna inteira de círculos?

Loops `for()` aninhados ao resgate! Podemos simplesmente copiar toda a instrução de loop que desenha uma coluna de círculos e colocá-la dentro do primeiro loop, que itera na direção horizontal (e garantir que o comando `ellipse()` use as variáveis `xPos` e `yPos`):

{% include p5-editor.html id="YH8DMpfdM" %}

Agora podemos brincar com todas as variáveis e ver como elas mudam o desenho:

{% include p5-editor.html id="iY-DlVY-9" %}

Ou podemos começar a adicionar variações aos círculos com base na sua posição, como mudar a cor com base na posição x e y do círculo:

{% include p5-editor.html id="I9DpHPDAx" %}

Ou aumentar o diâmetro junto com a posição x:

{% include p5-editor.html id="xT4rSb4IC" %}

Ou a cor com base na posição x e o tamanho com base na posição y:

{% include p5-editor.html id="8KVrenWPx" %}

## Loops For Dentro de Loops For Dentro de Loops For...

E, se realmente quisermos esticar nossas habilidades com loops, podemos adicionar mais um nível de loop `for()` para desenhar círculos concêntricos em cada posição x e y:

{% include p5-editor.html id="cfOHbCjAs" %}

Reserve um tempo para brincar com este código e garantir que todas as variáveis e loops façam sentido.

Os loops são construções muito poderosas na programação. Olhando o código acima, estamos desenhando muitos, muitos círculos (quantos exatamente?), mas o nosso código tem apenas uma linha com um comando `ellipse()`.

Como sempre, como estamos usando variáveis em todo lugar, podemos mudar seus valores ou usá-las para alterar o nosso círculo a cada iteração do nosso loop:

{% include p5-editor.html id="riWYeR2Zf" %}
