---
title: Introdução ao p5.js
---
Agora que vimos como [configurar um projeto p5.js](../p5-setup/) no nosso computador, vamos olhar mais de perto como o p5.js funciona e como podemos começar a escrever código para nossos projetos.

[ESTE](https://github.com/IDMp5/IDMp5.github.io/blob/main/_pages/p5js-template/sketch.js) é um arquivo `sketch.js` básico com o qual podemos começar:

```js
function setup() {
  createCanvas(windowWidth, windowHeight);
}

function draw() {
  background(220, 20, 20);
  ellipse(120, 120, 50, 50);
}
```
{% include p5-editor.html id="bvmSu50O6a" %}


## Setup & Draw

Como mencionado anteriormente, o código do nosso projeto, ou *sketch*, é dividido em duas seções principais, ou *funções*, uma função `setup()` e uma função `draw()`.

Especificamos o que cada uma dessas funções fará adicionando código dentro das suas chaves (`{` `}`).

No exemplo acima, o comando `createCanvas(windowWidth, windowHeight)` pertence à função `setup()` e os outros dois comandos, `background(220, 20, 20)` e `ellipse(120, 120, 50, 50)`, estão dentro da função `draw()`.

A diferença entre essas duas partes do nosso código é que o código dentro da função `setup()` só é executado uma vez, enquanto o código dentro da nossa função `draw()` roda repetidamente, de novo e de novo e de novo e de novo...

### `setup()`
A função `setup()` roda quando carregamos nossa página pela primeira vez e o arquivo `html` inclui a biblioteca p5.js, e depois o nosso arquivo `sketch.js`. Os comandos que colocamos dentro da função `setup()` geralmente têm a ver com *configurar* nosso ambiente e canvas: Qual tamanho queremos para o nosso canvas? Precisamos de um canvas? Qual modo de cor estamos usando? Devemos especificar o posicionamento das imagens usando seus cantos ou o centro? Qual é o estilo e o tamanho padrão da nossa fonte? Precisamos carregar algum arquivo externo?

Nem sempre precisamos responder a todas essas perguntas na nossa função `setup()`, e sempre podemos mudar como fazemos as coisas mais tarde no nosso código, mas para parâmetros e configurações que são fixos, é mais eficiente configurá-los uma única vez, no início do nosso programa, colocando comandos na função `setup()`.

No exemplo acima, nossa função `setup()` apenas define que a área onde podemos desenhar e detectar interações, nosso canvas, seja tão grande quanto nossa janela.

Se, por exemplo, no nosso projeto só fôssemos desenhar formas azuis, com contornos laranja grossos, podemos adicionar os comandos para configurar isso na nossa função `setup()`:

```js
strokeWeight(8);
stroke('orange');
fill('blue');
```

{% include p5-editor.html id="Z6jv5gVTo" %}


### `draw()`
É aqui que vamos querer colocar comandos que de fato *desenham* qualquer coisa na tela, sejam formas, imagens, quadros de filmes ou animações.

Por exemplo, os comandos para desenhar as elipses no código acima:

```js
ellipse(120, 120, 50, 50);
ellipse(150, 200, 50, 50);
ellipse(220, 120, 50, 50);
ellipse(250, 250, 50, 50);
```

Embora pareçam estáticas, essas elipses estão, na verdade, sendo redesenhadas na tela muitas vezes por segundo.

Diferente da função `setup()`, a função `draw()` roda repetidamente enquanto a página web do nosso projeto estiver aberta, e qualquer código que colocarmos dentro de suas `{ }` será executado cerca de 60 vezes por segundo. Isso é o que nos permite criar animações e lidar com interações.

Sem nos preocuparmos muito com os detalhes, mas só para verificar que tudo que colocamos dentro do `draw()` está sempre rodando, vamos modificar o código acima e usar o número de vezes que a função `draw()` foi executada para mover as elipses pelo canvas:

{% include p5-editor.html id="tmnnt_GsQ" %}

Aquela palavra-chave especial `frameCount` mantém a contagem de quantas vezes nosso código foi executado, e podemos usar esse valor para desenhar nossas elipses em uma posição um pouco diferente a cada vez.

Às vezes vamos nos referir a cada execução da função `draw()` como um *frame* (quadro), porque ela costuma ser usada para redesenhar todo o nosso canvas a cada execução, assim como um quadro de um filme ou de uma animação em flip-book.
