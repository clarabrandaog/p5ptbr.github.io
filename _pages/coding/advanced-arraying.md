---
title: Arrays Avançados
---

Como os arrays são usados com tanta frequência na programação, e não só para guardar informações, mas também para manipular e organizar informações, a maioria das linguagens de programação tem não apenas uma forma de especificar arrays, mas também muitas funções e variáveis nativas que podem ser usadas ao trabalhar com eles.

Vimos algumas delas no [tutorial anterior sobre arrays](../arrays/): `push()` e `length`. A função [`push()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) é usada quando queremos adicionar elementos ao final do array, e a variável [`length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/length) quando queremos saber o tamanho do array:

{% include p5-editor.html id="LASWmNjIj" %}

Neste sketch estamos adicionando valores aleatórios de diâmetro a um array toda vez que o mouse é clicado, e durante o `draw()` desenhamos elipses com base nesses diâmetros. Como não temos como saber de antemão quantas vezes o mouse foi clicado, não sabemos quantos valores há no nosso array, então contamos com o `length` para definir o limite do loop `for()` no `draw()`.

Também estamos limitando o número máximo de elementos do nosso array a $$10$$. Se, durante um evento de clique, detectamos que já temos $$10$$ elementos no array com: `(myDiameters.length > 9)`, removemos o primeiro elemento antes de adicionar um novo $$10^{o}$$ elemento.

## Manipulando Arrays

A forma de remover o primeiro elemento de um array é usando a função nativa especial [`shift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift). Se continuarmos clicando e adicionando novos diâmetros, podemos ver por que (talvez?) a função se chama shift (deslocar): conforme removemos elementos do início do array (índice $$0$$) e adicionamos novos elementos ao final do array (índice $$9$$), nosso desenho parece se deslocar para a esquerda.

Podemos fazer o inverso: adicionar elementos ao início do array e remover do final com [`unshift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift) e [`pop()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop), respectivamente:

{% include p5-editor.html id="-BVml9j5Z" %}

Vamos fingir que agora, por razões estéticas, queremos desenhar nossos diâmetros em ordem, do menor para o maior. Podemos usar a função especial [`sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort):

{% include p5-editor.html id="ekJDKCRmr" %}

Depois que adicionamos um novo elemento, ordenamos o array de diâmetros do menor para o maior. Conforme continuamos clicando neste exemplo, vamos notar algo estranho: parece que todos os círculos desenhados só vão crescendo.

Se pensarmos em como estamos adicionando e removendo elementos, isso faz sentido. Estamos sempre removendo o primeiro elemento do array e, como agora usamos um array ordenado, esse elemento é sempre o menor. Conforme clicamos o mouse, é como se estivéssemos filtrando o menor elemento do nosso array e, com o tempo, ficamos apenas com os maiores.

E se não quisermos remover o menor elemento, mas ainda quisermos desenhar os círculos em ordem? Além do nosso array com os elementos na ordem original, poderíamos ter uma cópia ordenada do array que só usamos para desenhar, mas não para adicionar e remover elementos.

Por sorte, esse é um caso tão comum que o JavaScript também tem uma função que faz isso para nós:

{% include p5-editor.html id="7ZmtXjOyM" %}

A função se chama [`toSorted()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted) e é muito parecida com o `sort()`, exceto que, enquanto o `sort()` faz a ordenação no array original, o `toSorted()` primeiro faz uma cópia do array e depois retorna a cópia ordenada.

Existem algumas outras funções especiais de array que vêm em pares assim: uma versão "*in place*" (no próprio array) que faz algo com o array original, e uma versão "*copying*" (com cópia), que copia o array antes de processá-lo e retorna a cópia. Por exemplo, o [`reverse`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) "*in place*" e sua versão "*copying*" [`toReversed`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toReversed).

## Funções como Parâmetros de Funções

Agora, digamos que em vez de apenas os $$10$$ diâmetros temos um array que guarda até $$100$$ [objetos](../objects/) círculo:

{% include p5-editor.html id="cyYHQSaL5" %}

A lógica é parecida com a anterior, mas agora, a cada clique, adicionamos entre $$8$$ e $$16$$ elementos ao nosso array, até chegarmos a $$100$$ elementos, quando limpamos o array para recomeçar.

Outra diferença é que agora o nosso array guarda objetos círculo completos, com os parâmetros `x`, `y` e `d`.

O loop `for()` que desenha nossos círculos funciona. É um bom loop `for()`. Mas, se olharmos o que acontece dentro do loop `for()`, vamos notar que todos os parâmetros usados pela função `ellipse()` vêm do objeto `aCircle[i]`.

Quando temos essa situação, em que queremos pegar cada elemento de um array e fazer algo com ele, sem precisar atualizar nenhuma outra variável ou objeto, existe uma forma diferente de pedir ao array que itere sobre seus elementos:

{% include p5-editor.html id="eMHwj8fqF" %}

Primeiro, definimos uma função chamada `drawCircle()`. Essa função recebe um argumento, que será um dos nossos objetos círculo com os parâmetros `x`, `y` e `d`, e desenha uma elipse usando esses valores. Fácil. Depois, na nossa função `draw()`, quando queremos iterar sobre todos os elementos do array e desenhar nossos círculos, podemos usar a função especial [`forEach()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach).

Essa função é especial em vários sentidos: primeiro, ela é uma das funções nativas que vêm com um array JavaScript. Além disso, ela também é especial porque é uma função que recebe uma função como argumento 🤯. A forma de pensar nisso é imaginar que estamos pedindo ao nosso array que execute essa função em cada um dos seus elementos, usando os elementos como argumentos.

Isso é manipulação de arrays bem avançada! E o código fica um pouco mais conciso e, possivelmente, mais "semântico" (os comandos que usamos descrevem o significado do que queremos que o computador faça). Em vez de um loop `for()` genérico com uma variável contadora, uma instrução condicional e uma atualização, agora estamos dizendo ao array "para cada um dos seus elementos, faça isto: drawCircle()".

## Processando Arrays

Depois que sabemos passar funções para funções, podemos começar a fazer um processamento mais sofisticado dos dados dos nossos arrays, como filtrar nosso array pelos valores dentro dos elementos:

{% include p5-editor.html id="KBK3zfcnR" %}

Se olharmos a documentação da função [`filter()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter), vamos ver que ela recebe uma função como parâmetro e retorna uma cópia do array, mas apenas com os elementos que passam pela função de filtro. Isso significa que ela executa a função em cada elemento do array e, se a função retorna verdadeiro, aquele elemento é copiado para o novo array; se não, ele não entra no array.

No exemplo acima, há uma função chamada `fullyInside()` que retorna `true` se um elemento círculo está inteiramente dentro do canvas. Quando passamos essa função para o `filter()`, ele chama a função com cada elemento do nosso array e mantém apenas os círculos cuja forma está totalmente visível no canvas.

No nosso código, primeiro chamamos a função `filter()` no array original e, como o `filter()` retorna uma cópia do array, nosso array original (`myCircles`) ainda tem círculos que estão ligeiramente fora do canvas. E, como o `filter()` retorna um array, esse array tem uma função especial `forEach()` que pode ser chamada com a nossa função `drawCircle()` definida anteriormente para desenhar todos os círculos filtrados:

```js
let filteredCircles = myCircles.filter(fullyInside);
filteredCircles.forEach(drawCircle);
```

Tente escrever sua própria função de filtro. Talvez uma que remova elementos que estão muito perto do centro do canvas... ou todos os círculos pequenos...

## Ordenação Personalizada

Também podemos ordenar nosso array por uma das propriedades dos círculos. Isso é parecido com filtrar, mas um pouco mais avançado e, como é uma operação tão comum, vale a pena dar uma olhada e, mesmo que não fique totalmente claro agora, sempre podemos voltar a este tutorial quando precisarmos fazer algo assim.

Se os elementos do nosso array são todos números ou palavras, o `sort()` simplesmente funciona sem nenhum argumento, mas como o que estamos tentando ordenar aqui são objetos círculo, o JavaScript não sabe muito bem como lidar com eles (deveria ordená-los pelos valores de `x`? de `y`? de `d`?).

Primeiro, vamos mudar um pouco o nosso sketch e desenhar nossos círculos perto do centro do canvas:

{% include p5-editor.html id="tW5TKn97o" %}

Então, agora não estamos mais guardando posições x e y absolutas, e sim pequenas variações aleatórias a serem somadas a $$\frac{width}{2}$$ e $$\frac{height}{2}$$.

Se você clicar várias vezes, vai ver que a maioria dos círculos fica coberta por círculos maiores; às vezes vemos muitos círculos, outras vezes bem poucos, mas nunca vemos todos os nossos círculos (você pode adicionar `noFill()` à função draw para ver). É por isso que queremos ordená-los do maior para o menor antes de desenhar.

Se olharmos a documentação do [`sort()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort), ela diz que podemos passar uma função de comparação para o `sort()` que define a ordem. Essa função de comparação recebe $$2$$ argumentos `A` e `B`, e o valor de retorno deve ser um número cujo sinal indica a ordem relativa dos dois elementos: negativo se `A` é menor que `B`, positivo se `A` é maior que `B` e zero se são iguais.

Então algo assim deve funcionar para ordenar nossos objetos usando seus diâmetros:

```js
function orderByDiameter(circleA, circleB) {
  return circleA.d - circleB.d;
}
```

{% include p5-editor.html id="BPNIo4WX_" %}

Hmmm.... ah. Certo. Como seguimos a documentação para criar nossa função `orderByDiameter()`, estamos na verdade dizendo ao `sort()` para ordenar do menor para o maior. Há duas formas de consertar isso. A primeira, e talvez a mais simples, é usar o [`reverse()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) no array depois de ele ser ordenado:

```js
myCircles.sort(orderByDiameter).reverse();
```

Isso funciona, mas estamos processando o array duas vezes, uma para executar o `sort()` e outra para o `reverse()`. E, sem saber exatamente como o JavaScript faz essas operações, ele pode estar fazendo muito trabalho desnecessário. Seria melhor ordenar apenas uma vez, mas na ordem inversa.

Para isso, podemos criar uma função de ordenação diferente que retorna o oposto da anterior:

```js
function orderByDiameterReverse(circleA, circleB) {
  return -(circleA.d - circleB.d);
}
```

E aqui está o resultado, desenhando os círculos ordenados do maior para o menor para que possamos ver todos eles:

{% include p5-editor.html id="tIIb9LqVZ" %}

As duas opções estão no sketch acima, e podemos ligá-las e desligá-las com comentários para ver que fazem a mesma coisa.

## Possibilidades

E agora podemos adicionar alguns novos parâmetros ao nosso objeto círculo, como cor e velocidade de rotação, para criar uma espécie de máquina hipnotizante:

{% include p5-editor.html id="IA_Vdmxm7" %}
