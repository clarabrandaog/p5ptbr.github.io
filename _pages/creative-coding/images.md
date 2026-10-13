---
title: Imagens
---
## Carregando Imagens

Vamos ver como trabalhar com imagens e arrays de pixels no p5js.

Colocar uma imagem no canvas não é muito difícil. Vamos seguir passos parecidos com os que usamos para carregar arquivos de dados ou fontes, e depois desenhar a imagem é parecido com desenhar um `rect()`:

{% include p5-editor.html id="V24PFFw_K" %}

Depois que nossa imagem é carregada em uma variável, ela se torna um objeto [`p5.Image`](https://p5js.org/reference/#/p5.Image), e podemos usar suas variáveis `width` e `height` para verificar seu tamanho em pixels. Note a saída no console; nossa imagem é bem maior que o nosso canvas de $$400$$ x $$400$$.

Se especificarmos apenas dois valores para a função `image()`, a imagem é desenhada na sua resolução original, e neste caso ela transborda.

Vamos redimensioná-la para que caiba no nosso canvas, e depois centralizá-la.

Podemos usar [`resize()`](https://p5js.org/reference/#/p5.Image/resize), outra função membro do objeto `p5.Image`, para escalar uma imagem corretamente. Se olharmos a documentação, vamos ver que se passarmos $$0$$ como um dos dois parâmetros do `resize()`, ele redimensiona a imagem em uma direção mantendo sua proporção original:

{% include p5-editor.html id="dSNYc-RFv" %}

Depois de escalar nossa imagem, podemos calcular alguns deslocamentos para centralizá-la no canvas. Esses deslocamentos são metade da diferença entre o tamanho da imagem e o tamanho do canvas:

<div class="scaled-images left w100">
  <img src = "{{ '/assets/images/creative-coding/images-00.jpg' | relative_url }}"/>
</div>

## Processando Imagens

Também podemos usar algumas das variáveis e métodos membros do `p5.Image` para acessar e manipular os pixels de uma imagem. Mas, primeiro, vamos ver como eles estão organizados.

Se olharmos a documentação, vamos ver que objetos da classe `p5.Image` têm uma variável membro [`pixels`](https://p5js.org/reference/#/p5.Image/pixels) que contém os valores de cor de todos os pixels da imagem.

E se imprimirmos algumas informações sobre esse array, vamos notar algumas coisas:
```js
print("pixel array size: ", mImage.pixels.length);
```

resulta em:<br>`pixel array size: 0`

Isso acontece porque, antes de podermos usar o array de pixels, temos que pedir à imagem que o prepare para nós com a função `loadPixels()`. Então agora:
```js
mImage.loadPixels();
print("pixel array size: ", mImage.pixels.length);
```

resulta em:<br>`pixel array size: 550400`

Há duas coisas importantes a notar aqui:

1. O array de pixels tem apenas uma dimensão. Embora nossa imagem tenha uma largura (`width`) e uma altura (```height`), na memória todos os pixels são armazenados em uma sequência contínua de números, uma linha por vez. Quando temos um array unidimensional que guarda todos os pixels da primeira linha, depois todos os pixels da segunda linha, depois da terceira linha, etc ... dizemos que os pixels estão armazenados em ordem row-major (por linhas), e vamos ter que fazer alguma matemática para descobrir qual índice de pixel corresponde a uma posição $$(x, y)$$ específica na imagem, e vice-versa (dado um índice de pixel, qual é sua posição $$(x, y)$$ na imagem).

2. Há mais valores no array do que pixels na imagem. Nossa imagem escalada tem<br>$$344$$ x $$400$$, ou $$137,600$$, pixels, mas nosso array é exatamente $$4$$ vezes maior que isso. Isso também tem a ver com a forma como o JavaScript armazena números na memória. Em vez de armazenar os valores de $$R$$, $$G$$, $$B$$ e $$alpha$$ em um único número de 4 bytes, o JavaScript usa 4 números diferentes. Então nosso array é $$4$$ vezes maior que o número de pixels, porque os valores de $$R$$, $$G$$, $$B$$ e $$alpha$$ de cada pixel são armazenados separadamente em índices diferentes desse array.

Se tivéssemos uma imagem de $$3$$ x $$3$$ com $$9$$ pixels, o começo do seu array de pixels seria algo assim:

<div class="scaled-images left w100">
  <img src = "{{ '/assets/images/creative-coding/images-01.jpg' | relative_url }}"/>
</div>

E a associação entre os índices $$(x, y)$$ dos pixels da imagem e o índice $$i$$ do array:

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/images-02.jpg' | relative_url }}"/>
</div>

Isso significa que, se quisermos percorrer nossa imagem e fazer algo com cada pixel, vamos ter que fazer alguma matemática para traduzir entre a coordenada $$(x, y)$$ e a coordenada $$i$$ do array de pixels.

Vamos começar com o caso mais simples, em que não nos importamos com a posição $$(x, y)$$ e só queremos manipular cada pixel do nosso array com a mesma lógica, independente da sua localização. Vamos percorrer todos os valores do array de pixels, mas em vez de incrementar o índice do nosso loop de $$1$$ em $$1$$, vamos incrementá-lo de $$4$$ em $$4$$. Dessa forma, nossa variável `i` sempre aponta para o primeiro dos $$4$$ valores de cor de um pixel.

<div class="scaled-images left w100">
  <img src = "{{ '/assets/images/creative-coding/images-03.jpg' | relative_url }}"/>
</div>

Vamos mudar o valor de verde de cada pixel da nossa imagem usando a posição `mouseY`. Como cada pixel recebe a mesma mudança, não nos importamos com sua localização $$(x, y)$$ dentro da imagem, e podemos simplesmente percorrer `mImage.pixels` diretamente com um loop `for()`, cujo índice incrementa de $$4$$ em $$4$$.

Sempre que mudamos os valores do array `pixels` de um `p5.Image`, temos que chamar `updatePixels()` para que o p5.js possa atualizar alguns valores internos para desenhar de fato a nossa nova imagem.

{% include p5-editor.html id="JbAHwnWwI" %}

Mesmo que nem todos os pixels sejam alterados na mesma quantidade, desde que cada pixel receba a mesma lógica, podemos usar esse método para percorrer os valores de pixel da imagem.

Em vez de incrementar o valor de verde de todos os pixels, vamos exagerar sua cor dominante. Como vamos precisar consultar a imagem original para saber qual era sua cor dominante, primeiro teremos que fazer uma cópia da imagem:

{% include p5-editor.html id="ehdyLdUOG" %}

Agora, embora cada pixel esteja sendo alterado em uma quantidade diferente, de acordo com seus valores de cor, a lógica aplicada a cada pixel é a mesma, independente da localização $$(x, y)$$, então ainda podemos usar o método com apenas um loop `for()`.

## Processando Pixels

Até agora, estivemos executando a mesma lógica na imagem inteira. Mesmo processando a imagem por seus pixels, a imagem toda é processada da mesma forma, independente da localização de cada pixel.

Vamos ver uma situação em que queremos desenhar formas na tela com base nos valores de pixel de uma imagem.

Agora nos importamos com a posição $$(x, y)$$ do pixel dentro da imagem, porque ela vai corresponder a onde colocamos nossa forma no canvas.

Vamos começar construindo a lógica para desenhar um padrão de círculos no nosso canvas usando loops `for()` aninhados. Toda a lógica para carregar e escalar a imagem está nas funções `preload()` e `setup()`, mas ainda não estamos usando a imagem. Só queremos garantir que a lógica para desenhar nosso padrão de círculos funciona.

Esses loops `for()` devem parecer familiares. Começamos em `mRadius` e desenhamos círculos com<br>`2 * mRadius` pixels de largura, com um espaçamento que também é de `2 * mRadius` pixels:
```js
for (let y = mRadius; y < mImage.height - mRadius; y += 2 * mRadius) {
  for (let x = mRadius; x < mImage.width - mRadius; x += 2 * mRadius) {
    fill(128);
    ellipse(x, y, 2 * mRadius, 2 * mRadius);
  }
}
```

Vamos adicionar um pouco de lógica para fazer `mRadius` variar de acordo com a nossa posição `mouseY`:
```js
let mRadius = floor(map(mouseY, 0, height, 2, 32, true));
```

E obtemos algo assim (o diâmetro do círculo foi reduzido para `1.75 * mRadius` para dar um visual mais agradável):

{% include p5-editor.html id="EZU_o4JNl" %}

Agora a única coisa que temos que fazer é pegar os valores de cor na posição $$(x, y)$$ da imagem para usar no nosso `fill()`. A matemática parece um pouco intimidadora no começo, mas vamos passar por ela com imagens e construir nosso entendimento de como ir do nosso valor bidimensional $$(x, y)$$ para um único índice $$i$$ que nos dará os valores de cor corretos do array de pixels.

Vamos considerar a seguinte imagem de $$4$$ x $$3$$ com índices de pixel $$(x, y)$$, e seu array correspondente de valores de pixel com índice $$i$$:

<div class="scaled-images left w100">
  <img src = "{{ '/assets/images/creative-coding/images-04.jpg' | relative_url }}"/>
</div>

É fácil descobrir o valor de $$i$$ para a primeira linha da imagem quando $$y = 0$$: $$i$$ é simplesmente igual ao valor de $$x$$. Então, $$(0, 0)$$, $$(1, 0)$$, $$(2, 0)$$, $$(3, 0)$$ correspondem a $$[0, 1, 2, 3]$$.

Para a segunda linha, quando $$y = 1$$, temos $$(0, 1)$$, $$(1, 1)$$, $$(2, 1)$$, $$(3, 1)$$ correspondendo a valores de $$i$$ de $$[4, 5, 6, 7]$$. Bem, podemos obter esses mesmos números somando $$4$$ a cada valor de $$i$$ da primeira linha.

E se olharmos a última linha, quando $$y = 2$$, temos $$(0, 2)$$, $$(1, 2)$$, $$(2, 2)$$, $$(3, 2)$$ correspondendo a valores de $$i$$ de $$[8, 9, 10, 11]$$. Esses números são $$4$$ unidades maiores que os valores de $$i$$ da segunda linha $$([4, 5, 6, 7])$$ e $$8$$ unidades maiores que os valores de $$i$$ da primeira linha $$([0, 1, 2, 3])$$.

Então, toda vez que movemos $$x$$ de $$1$$, nosso valor de $$i$$ também aumenta de $$1$$, mas quando aumentamos $$y$$ de $$1$$, nosso valor de $$i$$ aumenta de $$4$$, o que nos dá a seguinte equação:

$$i = 4 \times y + x$$

De forma mais geral, quando movemos $$y$$ de $$1$$, nosso índice $$i$$ aumenta pela largura da imagem:

$$i = width \times y + x$$

Só mais um ajuste, porque infelizmente nosso array de pixels da imagem não tem apenas $$1$$ valor por pixel, e sim $$4$$.

Quando incrementamos $$x$$ de $$1$$, na verdade queremos incrementar nosso $$i$$ de $$4$$ para chegar ao primeiro valor de cor do próximo pixel. Da mesma forma, quando $$y$$ aumenta de $$1$$, queremos incrementar $$i$$ de $$4 \times width$$, para novamente cairmos no primeiro valor de cor de um pixel:

<div class="scaled-images left w100">
  <img src = "{{ '/assets/images/creative-coding/images-05.jpg' | relative_url }}"/>
</div>

Então, para obter o índice do primeiro valor de cor de um pixel em $$(x, y)$$, precisamos fazer:
```js
let pIndex = 4 * (y * mImage.width + x);
```

Isso nos dá o valor de $$red$$ do pixel, enquanto os valores de $$green$$ e $$blue$$ estão em `pIndex + 1` e `pIndex + 2`, respectivamente.

{% include p5-editor.html id="ff3i5NEoJ" %}

Um pouco de matemática, mas depois que sabemos, não é tão ruim.
