---
title: Strings
---
## Introdução: Desenhando Palavras

Vimos variáveis que guardam *números*, *booleanos*, *arrays* e *objetos*. Agora vamos dar uma olhada em um tipo de dado diferente do JavaScript: as *strings*.

Uma string guarda dados de texto. É uma lista/array de caracteres, especificada entre aspas simples (' '), duplas (" ") ou crases (``` ` ` ```). E, assim como outros valores, pode ser atribuída a variáveis:
```js
let s0 = "This is a string";
let s1 = 'This is also a string!';
let s2 = `Another string`;
```

No p5.js usamos a função [`text()`](https://p5js.org/reference/#/p5/text) para desenhar strings na tela.

Além da string (ou da variável que guarda a string), também temos que dar à função `text()` as coordenadas `x` e `y` onde o texto deve ser desenhado:
```js
let s0 = "This is a string";
text(s0, 10, 10);
```

{% include p5-editor.html id="N7dBcauPF" %}

Além do `text()`, o p5.js também tem um monte de [funções](https://p5js.org/reference/#group-Typography) para controlar a tipografia e as propriedades do texto, como tamanho da fonte, tipo de fonte, alinhamentos, etc.:

{% include p5-editor.html id="cy5No5rwW" %}

O [alinhamento](https://p5js.org/reference/#/p5/textAlign) do texto é algo que vale a pena ler e experimentar:

{% include p5-editor.html id="B7k5Ytn_v" %}

Nos casos em que o nosso texto é muito longo, podemos fazê-lo quebrar e ser exibido em várias linhas dando à função `text()` alguns parâmetros extras:

{% include p5-editor.html id="dZpYMQAEJ" %}

Isso é como o `rect()`, em que especificamos as posições `x`, `y` e a `width`, `height`, só que agora a largura e a altura são de uma caixa de texto.

## Strings como Arrays

Como as strings são arrays de caracteres, e são muito usadas, o JavaScript também nos oferece funções e variáveis especiais para manipulá-las.

Por exemplo, a variável [`length`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/length) pode ser usada para verificar quantos caracteres há na string:
```js
let s0 = "This is a string";
print(s0.length); // deve imprimir: 16
```

A função [`toUpperCase()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase), por exemplo, pode ser usada para transformar cada letra de uma string em letra maiúscula:
```js
let s0 = "This is a string";
print(s0.toUpperCase()); // deve imprimir: THIS IS A STRING
```

Existe uma porção de [funções](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) para strings.

E, assim como com um array de números, podemos iterar sobre uma string e desenhar seus caracteres um de cada vez:

{% include p5-editor.html id="ic4ok1ann" %}

Independente do tamanho da string, sempre podemos iterar sobre ela usando `length` e acessando as letras com a notação de índice de array:

{% include p5-editor.html id="tVJ12ATX2" %}

Isso está tentando encaixar todas as letras de uma frase longa na diagonal do nosso canvas, mas, em vez disso, poderíamos primeiro dividir a frase em palavras e depois iterar sobre o array de palavras:

{% include p5-editor.html id="2M95jO8ab" %}

Se olharmos a documentação da função [`split()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split), vamos ver que ela recebe uma string como parâmetro, e que essa string é usada para dividir a string original em várias substrings. Usamos o caractere de espaço (`" "`) para dividir a frase em palavras, mas poderíamos ter usado qualquer string como parâmetro de divisão.

E, se quisermos evitar que palavras ultrapassem a borda do nosso canvas, usamos a função do p5.js [`textWidth()`](https://p5js.org/reference/#/p5/textWidth) para calcular a largura de cada palavra e usamos isso para ajustar a posição x da palavra caso ela vá transbordar:

{% include p5-editor.html id="JkBUZgdQr" %}

## Mais Divisões

Vamos criar um sketch que sorteia um vencedor de uma lista de nomes.

Primeiro, definimos uma string com todos os nomes:
```js
let s0 = "thiago, alice, bob, chewbacca, dany, elisa";
```

E a dividimos em um array de nomes:
```js
words = s0.split(", ");
```

Agora, toda vez que rodarmos o sketch de novo, ele vai selecionar o vencedor com base em um valor aleatório que representa o índice daquele nome no nosso array:
```js
function mouseClicked() {
  chosenIndex = random(0, words.length);
}
```

O problema do código acima é que os números retornados pelo `random()` podem ser fracionários e ter uma parte decimal, como: $$1.3, 2.62, 0.78$$, etc., e esses não são índices de array válidos. Para usar valores retornados pela função `random()` como índices de arrays, primeiro temos que transformá-los em números inteiros, descartando a parte fracionária.

Uma forma de fazer isso é usar a função [`floor()`](https://p5js.org/reference/#/p5/floor), que calcula o valor inteiro mais próximo que é menor ou igual ao número dado como parâmetro. Ou, em outras palavras, ela simplesmente descarta a parte fracionária, que é exatamente o que queremos.

Então, em vez de:
```js
chosenIndex = random(0, words.length);
```

Vamos usar:
```js
chosenIndex = floor(random(0, words.length));
```

{% include p5-editor.html id="4efFuRnvn" %}

## Carregando Fontes

Podemos especificar a fonte a ser usada passando uma string com o nome da fonte para o `textFont()`, mas isso só funciona se essa fonte estiver instalada no computador onde o sketch está rodando, ou se usarmos nomes genéricos de famílias de fontes (`serif`, `sans-serif`, `monospace`).

Se quisermos usar fontes personalizadas muito especiais, temos que dizer ao p5.js para carregar o arquivo da fonte primeiro e depois passá-lo para o `textFont()`.

O primeiro passo é encontrar ou criar um arquivo de fonte. Para fontes gratuitas e de código aberto, dê uma olhada no [League of Moveable Type](https://www.theleagueofmoveabletype.com/) e no [Open Font Library](https://fontlibrary.org/).

Quando tivermos um arquivo de fonte (.otf ou .ttf), podemos colocá-lo ao lado do nosso arquivo `sketch.js` e carregá-lo usando [`loadFont()`](https://p5js.org/reference/#/p5/loadFont) na parte `preload()` do nosso sketch.

Depois podemos usá-lo para desenhar nossos textos:

{% include p5-editor.html id="JW97r1_s4" %}

Então, no p5js, não só as nossas strings são instâncias de uma classe, como também temos uma classe [Font](https://p5js.org/reference/#/p5.Font) com suas próprias funções especiais para manipular texto. Uma dessas funções, [`textBounds()`](https://p5js.org/reference/#/p5.Font/textBounds), retorna uma caixa delimitadora (bounding box) para o texto dado.

A caixa que é retornada é um objeto com $$4$$ parâmetros: $$x$$, $$y$$, $$w$$, $$h$$ para a posição `x` e `y`, a `width` e a `height` do texto.

Podemos usar essa informação para desenhar retângulos ao redor do nosso texto:

{% include p5-editor.html id="TA1GRq1Dw" %}

Outra função especial da classe Font é [`textToPoints()`](https://p5js.org/reference/#/p5.Font/textToPoints). Essa função retorna um array de pontos que seguem o texto especificado. Quando temos esses pontos, podemos fazer pequenas modificações nos seus valores antes de usá-los para criar uma forma.

Neste sketch estamos adicionando um pequeno número aleatório às posições `x` e `y` de cada ponto antes de desenhá-los:

{% include p5-editor.html id="z3QI6kJyQ" %}

## Carregando Arquivos de Texto

Mais uma última coisa sobre trabalhar com textos e strings.

Assim como existem as funções `loadJSON()` e `loadTable()`, o p5.js também tem uma função [`loadStrings()`](https://p5js.org/reference/#/p5/loadStrings) que pode ser usada para ler um arquivo de texto (do nosso computador ou da internet).

O resultado de chamar a função `loadStrings()` é um array com as linhas do arquivo de texto, então, se usarmos um loop `for()`, podemos iterar pelo array resultante e desenhar uma linha do arquivo de texto de cada vez:

{% include p5-editor.html id="O-q2k8pNX" %}
