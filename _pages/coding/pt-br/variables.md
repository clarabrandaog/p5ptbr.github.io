---
title: Variáveis
---
## Acompanhando valores

No sentido mais básico, variáveis são um tipo de recipiente para os nossos dados. Elas dão um nome aos nossos valores.

Elas nos ajudam a acompanhar diferentes valores no nosso código, sem que precisemos lembrá-los, o que é especialmente importante para valores que mudam. Elas nos permitem acompanhar o *significado* dos valores em vez da sua quantidade exata.

As variáveis também ajudam o computador, porque são uma forma de nós, como programadores, pedirmos ao computador que reserve um pouco de memória para o nosso programa.

Existem algumas formas de declarar uma variável em JavaScript:

```js
var x;
let y;
const z = 10;
```

As duas primeiras linhas, `var x;` e `let y;`, declaram variáveis vazias, chamadas `x` e `y` respectivamente, enquanto a terceira linha `const z = 10;` declara uma variável chamada `z` e imediatamente dá (*atribui*) a ela o valor $$10$$.

Tanto `let` quanto `var` pedem ao computador que reserve um pouco de memória para valores que serão fornecidos mais tarde e que podem mudar. Isso é importante. As variáveis `x` e `y` podem ter um valor (ou nenhum valor) no começo do nosso programa, mas o nosso código pode referenciá-las pelo nome mais tarde e atualizar seus valores conforme necessário:

```js
let inches = 60;
inches = 72;
inches = 72 + 4;
// inches tem o valor 76 agora
```

A diferença entre as palavras-chave `var` e `let` é meio sutil e tem a ver com quando/onde/como podemos acessar nossas variáveis e reutilizar seus **nomes**. Usar `var` pode levar a algumas situações pouco intuitivas, então vamos usar `let` na maioria/em todas as nossas declarações de variáveis.

Declarar uma variável com a palavra-chave `const` é diferente. Ela significa *constante*, então as variáveis declaradas com `const` recebem um valor inicial, que nunca pode mudar.

```js
const ID = 1212;
ID = 2323; // <-- isso vai dar um erro
```

Depois que temos variáveis com valores, podemos usar o nome delas no lugar do valor em expressões:

```js
let base = 14;
let height = 23;
let area = (base * height);
let perimeter = 2 * base + 2 * height;
```

Escrever `base * height` e `2 * base + 2 * height` é certamente mais fácil e mais significativo do que `14 * 23` e `2 * 14 + 2 * 23`. E usar as variáveis `area` e `perimeter` mais tarde no nosso código também é mais fácil do que ter que recalcular esses valores toda vez.

As variáveis não só fazem com que não precisemos lembrar números específicos no nosso programa, como também dão alguma semântica ao nosso código: agora sabemos o que esses valores significam.

## Parametrizando

Como já mencionado, os valores das variáveis podem mudar durante a execução do nosso programa, mas também podem servir como uma forma de definirmos parâmetros para computações repetíveis ou modulares.

Vamos ver um exemplo. Imagine que acabamos de criar este logo usando algumas formas básicas [`arc()`](https://p5js.org/reference/#/p5/arc) do p5.js:
{% include p5-editor.html id="tcLVwsWin" %}

Algumas das formas transbordam a área do nosso canvas, mas tudo bem. O nosso logo pode ser incorporado em qualquer área que tenha exatamente $$400$$ por $$400$$ pixels.

Digamos que agora vamos usá-lo em um contexto maior:
{% include p5-editor.html id="wNiBFMPwe" %}

Ou que temos que encaixá-lo em uma área menor:
{% include p5-editor.html id="4x_z841nbO" %}

Os dois ficam ruins; além de não estarem centralizados, o pequeno tem muito transbordamento.

Uma forma de *parametrizar* o nosso código é usar uma variável para registrar o centro do nosso canvas e definir os outros parâmetros do nosso logo em relação ao centro do canvas:
{% include p5-editor.html id="5civphEan" %}

Agora podemos apenas atualizar essas duas variáveis: `centerX` e `centerY` toda vez que mudarmos o tamanho do nosso canvas, e as formas vão continuar centralizadas. Experimente acima ☝️ !

Isso é melhor, mas podemos melhorar usando variáveis para tornar todo o nosso design mais flexível e independente de tamanhos específicos.

Por exemplo, em vez de digitar os valores da posição central do nosso canvas, o p5.js fornece variáveis que acompanham o tamanho do nosso canvas para nós. As variáveis do p5.js [`width`](https://p5js.org/reference/#/p5/width) e [`height`](https://p5js.org/reference/#/p5/height) sempre terão as dimensões do nosso canvas, mesmo que elas mudem, então é sempre uma ideia melhor usá-las ao desenhar formas em posições relativas no nosso canvas:

```js
centerX = width / 2;
centerY = height / 2;
```

Também podemos usar a função [`min()`](https://p5js.org/reference/#/p5/min) do p5.js para registrar a menor das dimensões (`width` ou `height`) e usá-la para limitar a circunferência do nosso maior `arc()`:
{% include p5-editor.html id="14yHQ5x-t" %}

Agora podemos mudar o tamanho do nosso canvas e o nosso logo vai acompanhar, mais ou menos. Tente mudar a dimensão do canvas acima para ver o que acontece ☝️ .

Vamos continuar ajustando o nosso design, mudando o nosso código para usar variáveis em mais parâmetros do `arc()`.

As circunferências de $$100$$, $$200$$ e $$300$$ no código original podem ser transformadas em `minDim / 4`, `minDim / 2` e `3 * minDim / 4`, respectivamente, para um quarto, metade e três quartos da menor dimensão do canvas.

Também vamos usar `minDim / 12` para o nosso [strokeWeight()](https://p5js.org/reference/#/p5/strokeWeight).

Essas mudanças devem fazer todo o nosso logo caber no nosso canvas, independente do tamanho exato do canvas:
{% include p5-editor.html id="Or8g11cpC" %}

Uma última pequena modificação é reorganizar (*refatorar*) o nosso cálculo `3 * minDim / 4` e colocá-lo em uma variável, só para deixar o código um pouco mais legível. As variáveis também podem ajudar com isso. E agora que o nosso logo está praticamente definido usando variáveis, em termos de valores relativos, ele vai mudar e se ajustar às mudanças no tamanho do nosso canvas:

{% include p5-editor.html id="gm7YG-6C2" %}

Mesmo que tenhamos que usar valores bem particulares, como $$386$$ e $$494$$ para as dimensões do nosso canvas, o nosso logo vai se adaptar. Experimente e mude o tamanho do canvas no código acima  ☝️ !

## Outros tipos de parâmetros

As variáveis podem guardar muitos tipos de valores, não só números simples.

Por exemplo, elas também podem guardar informações de [`color()`](https://p5js.org/reference/#/ p5/color) do p5.js.

Assim podemos experimentar com paletas diferentes sem ter que acompanhar valores exatos de cor por todo o nosso código:
{% include p5-editor.html id="FZ1zns8HD" %}

Mude os valores de cor de `highColor` e `lowColor` acima  ☝️ . Fácil, né?

## Acompanhando o tempo

Vamos ver muitos exemplos disso quando falarmos de animações no p5.js, mas as variáveis também podem ser usadas para acompanhar o tempo.

A variável do p5.js `frameCount` nos dá o número de frames que se passaram desde que o nosso programa começou a rodar, e podemos fazer uma pequena conta para transformar esse valor em um incremento para o ângulo `stop` dos nossos arcos.

Sem saber muito sobre os detalhes deste sketch, podemos ver que há uma variável chamada `fs` que registra um valor que não para de aumentar e é escalado para definir a posição final dos nossos arcos:

{% include p5-editor.html id="28HuTR3di" %}

Tente mudar os valores na expressão de `fs` ☝️ !

## Outras variáveis do p5.js

Além de `width` e `height`, para as dimensões do nosso canvas, e `frameCount`, para o número de frames decorridos desde o começo da execução do programa, o p5.js também nos dá as variáveis `mouseX` e `mouseY`.

Elas sempre guardam os valores da posição horizontal e vertical atual do nosso mouse quando ele está sobre o canvas, que podemos usar nos nossos desenhos:

{% include p5-editor.html id="1bWdFTAeT" %}


Já que estamos falando de variáveis, vamos criar algumas para deixar a forma da nossa elipse mais interessante.

Primeiro, vamos calcular a que distância o mouse está do centro do canvas:
```js
distX = abs(mouseX - centerX);
distY = abs(mouseY - centerY);
```

E depois vamos fazer a nossa elipse ficar bem pequena quando estiver o mais longe possível do centro, e maior quando estiver perto do centro. Vamos usar metade da `width` como o nosso diâmetro máximo e calcular diâmetros horizontais e verticais individuais subtraindo nossas medidas de distância desse valor:
```js
maxDiam = width / 2;
diamX = maxDiam - distX;
diamY = maxDiam - distY;
```

Isso faz com que os diâmetros sejam $$0$$ nas distâncias máximas e `width / 2` no centro do canvas:

{% include p5-editor.html id="D6TWJlXxq" %}

Podemos continuar usando variáveis e um pouco de matemática para adicionar mais elipses ao nosso sketch:
```js
reflectX = width - mouseX;
reflectY = height - mouseY;
```

Essas variáveis nos dão a posição do mouse, mas *refletida* em relação ao centro do canvas: quando `mouseX` está na posição horizontal `width`, por exemplo, `reflectX` vai estar em $$0$$, e vice-versa. E o mesmo com `mouseY` e `reflectY`.

Se usarmos essas $$2$$ novas variáveis para desenhar uma segunda elipse, obtemos algo assim:

{% include p5-editor.html id="5-XDVKZk7" %}

Existem outras variáveis disponíveis para nós nos nossos sketches p5.js, como [`key`](https://p5js.org/reference/#/p5/key), que guarda o valor da última tecla pressionada no teclado, ou [`pixels`](https://p5js.org/reference/#/p5/pixels), que é uma variável que guarda uma lista de valores de cor, um para cada pixel do nosso canvas.

Vamos ver essas em detalhe quando começarmos a trabalhar com [arrays](../arrays), [imagens](../../creative-coding/images) e [texto](../strings).
