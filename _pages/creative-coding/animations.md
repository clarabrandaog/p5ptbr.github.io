---
title: Animações
---
## Frames

Já vimos essa estrutura básica do p5.js muitas vezes:
```js
function setup() {
  // executa uma vez
}

function draw() {
  // executa de novo e de novo e de novo
}
```

Só para reforçar o que o p5.js faz com esse código: todos os comandos dentro da função `setup()` rodam apenas uma vez. É aqui que definimos o tamanho do canvas, talvez atribuímos valores iniciais a algumas variáveis, desenhamos o fundo pela primeira vez e configuramos os modos de desenho, como [`rectMode`](https://p5js.org/reference/#/p5/rectMode), [`colorMode`](https://p5js.org/reference/#/p5/colorMode), etc.

Depois que a função `setup()` roda, o p5.js pega nossa função `draw()` e executa todos os comandos que estão dentro dela de novo e de novo e de novo, a uma taxa específica. Na maior parte do código que executamos até agora, não percebemos esse redesenho porque desenhamos as mesmas formas toda vez que a função `draw()` roda.

Com que frequência a função `draw()` roda depende de muitas coisas, mas para a maioria dos navegadores e telas será $$30$$ vezes por segundo ou $$60$$ vezes por segundo. Isso é chamado de taxa de quadros (frame rate), e às vezes esses valores são dados como $$30$$ *fps* ou $$60$$ *fps*, onde *fps* significa *frames per second* (quadros por segundo). $$30$$ e $$60$$ *fps* são taxas de atualização comuns em monitores e são rápidas o bastante para que possamos usar redesenhos consecutivos para criar animações sem que nossos olhos percebam saltos ou descontinuidades.

Podemos chamar cada execução do `draw()` de um frame, e variando levemente posições, tamanhos e cores entre os frames podemos criar animações.

*Animações?* Sim!

Se, em vez de desenhar um círculo exatamente no mesmo lugar toda vez que o `draw()` executa, usarmos uma variável para mudar sua posição, podemos fazê-lo se mover!

{% include p5-editor.html id="txJBbZKnX" %}

Vamos brincar um pouco com esse conceito... e se desenharmos o fundo só uma vez, mas continuarmos desenhando elipses a cada frame?

{% include p5-editor.html id="C0SDPIm2f" %}

Ou, e se em vez de desenhar um fundo totalmente opaco, adicionarmos um pouco de alpha à nossa cor?

{% include p5-editor.html id="anLS0_0Rx" %}

Massaaa !!!

## Contagem de Frames

Em vez de usar uma variável chamada `xPos`, poderíamos ter usado a variável do p5.js [`frameCount`](https://p5js.org/reference/#/p5/frameCount). Essa variável registra quantas vezes a função `draw()` foi executada e pode ser facilmente usada para criar animações:

{% include p5-editor.html id="L9FIYPY2_" %}

A cada frame estamos redesenhando a elipse, mas antes de redesenhar estamos rotacionando nosso canvas em `frameCount` graus: no primeiro frame ele rotaciona $$0^\circ$$, na segunda vez $$1^\circ$$, depois $$2^\circ$$, depois $$3^\circ$$, etc, e quando vemos todos esses frames um após o outro percebemos uma elipse animada girando.

Como a variável `frameCount` só cresce e cresce, é muito fácil usá-la para algo assim, porque rotações são "cíclicas", elas se repetem depois de $$360^\circ$$. Note também que estamos usando a função `radians()` porque `rotate()` espera ângulos em radianos, e se simplesmente passássemos nosso `frameCount` para o `rotate()` ele giraria muito rápido. Experimente!!

Vamos voltar ao nosso exemplo do círculo:

{% include p5-editor.html id="txJBbZKnX" %}

Brinque com o valor que determina a velocidade do círculo no sketch acima ☝️ e descubra como fazê-lo se mover mais rápido. À medida que ele fica mais rápido, perdemos a impressão de animação em algum momento?

## Definindo Limites

Agora, e se não quisermos que ele vá embora e queiramos que ele volte quando atingir a borda do nosso canvas?

Vamos adicionar uma variável para registrar nossa posição e direção, e a cada frame, depois de incrementarmos a posição do nosso círculo, podemos verificar se sua posição ficou maior que a largura (`width`) do canvas e, se necessário, mudar sua direção:

{% include p5-editor.html id="mH4M23PZV" %}

Agora nosso círculo está desaparecendo no lado esquerdo da tela. Podemos corrigir isso da mesma forma que corrigimos o lado direito:

{% include p5-editor.html id="V_d91eHez" %}

Vamos limpar isso um pouco e refatorar a variável `xDir` para que ela guarde de fato a nossa velocidade. O código continua o mesmo, porque a lógica é a mesma, mas o nome da variável fica um pouco mais significativo e agora sabemos que podemos usá-la para controlar a velocidade da nossa elipse: uma velocidade positiva significa que ela está se movendo para a direita e uma velocidade negativa significa que está se movendo para a esquerda.

{% include p5-editor.html id="v6y32pYHP" %}

Olhando a lógica, podemos simplificá-la apenas detectando quando ele chega a qualquer uma das bordas e invertendo o `xVel`:

{% include p5-editor.html id="u28VL-98_" %}

Quase perfeito. O círculo só muda de direção quando seu centro passa do canvas, e não quando sua borda chega ao limite. Vamos colocar o raio do círculo em uma variável e usá-la para detectar quando o círculo atinge o fim do canvas:

{% include p5-editor.html id="HR0Adpc-8" %}

Agora temos uma animação simples e infinita usando nosso loop `draw()`.

## Mais Dimensões de Animação

Tanto o operador `mod(%)` quanto a função `map()` que vimos [aqui](../maths/) podem ser muito úteis ao fazer animação.

Podemos usar a função `map()` para criar uma animação baseada no tempo. Neste exemplo mapeamos os segundos que passaram no minuto atual para o diâmetro de um círculo. Sabemos que os segundos têm um intervalo $$[0, 59]$$, então basta escolher os menores e maiores valores para o diâmetro.

Além de mapear $$[0, 59] \rightarrow [20, width]$$ para o diâmetro, também podemos mapear o valor dos segundos para a quantidade de verde na cor do nosso `fill()`. Sabemos que cada um dos $$3$$ parâmetros de cor tem um valor máximo de $$255$$, e se quisermos sempre um pouco de verde na nossa cor, podemos mapear $$[0, 59] \rightarrow [128, 255]$$:

{% include p5-editor.html id="thbVYgT-l" %}

Podemos adicionar outros elementos a isso mapeando também a quantidade de milissegundos para o diâmetro de uma elipse diferente. Infelizmente não temos uma função que nos dê os milissegundos do horário atual, mas [`millis()`](https://p5js.org/reference/#/p5/millis) nos dá o número de milissegundos que se passaram desde que iniciamos nosso sketch. Esse valor só cresce, então vamos usar `mod(%)` para mantê-lo entre $$[0, 999]$$.

Então, em vez de mapear $$[0, 999] \rightarrow [20, width]$$, vamos mapear esse valor para ficar entre o diâmetro da outra elipse e `width`, ou seja, $$[0, 999] \rightarrow [secondDiam, width]$$.

Isso deve nos dar um efeito interessante:

{% include p5-editor.html id="620VoCQoG" %}

Também podemos usar [loops `for()`](../../coding/counters/), [arrays](../../coding/arrays/) e [objetos](../../coding/objects/) para criar algumas animações bem abstratas.

Vamos criar um array de objetos linha. Cada linha tem uma posição `x`, um comprimento `l` e uma velocidade `v`.

A cada frame atualizamos o comprimento de cada linha usando sua velocidade, e garantimos que ele volte a $$0$$ quando crescer além da altura (`height`) do canvas.

{% include p5-editor.html id="5HEH8eW-r" %}
