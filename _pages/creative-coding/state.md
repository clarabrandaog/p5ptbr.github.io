---
title: Variáveis de Estado
---

Assim que começamos a detectar entradas do mouse e do teclado, e a construir animações mais complexas, logo vamos sentir a necessidade de lembrar certas interações ou sequências de interações.

Digamos que queremos que a pessoa mude a cor de fundo usando o mouse e, para este primeiro exemplo simples, toda vez que o mouse for clicado uma cor aleatória é selecionada para o fundo.

{% include p5-editor.html id="jFjNaSoyL" %}

E se agora quisermos permitir apenas cores de um conjunto de tons pré-selecionados de que gostamos? Uma forma de fazer isso é declarar uma variável global fora do `setup()` e do `draw()` para registrar qual deve ser a cor do nosso fundo.

Neste exemplo, `bgColorIndex` é uma variável que guarda um número entre $$0$$ e $$3$$ e registra qual cor devemos usar para o fundo. Toda vez que o mouse é clicado, verificamos qual cor deve ser exibida, incrementamos o índice da cor e exibimos a nova cor. A variável `bgColorIndex` percorre ciclicamente de $$0$$ a $$3$$ $$(0, 1, 2, 3, 0, 1, 2, 3, 0, 1, ...)$$, e a cor de fundo vai da cor rosa inicial para `gold`, depois `darkGreen`, depois `darkBlue` e então volta ao rosa inicial.

{% include p5-editor.html id="XpOWN74t9" %}

Dizemos que `bgColorIndex` é uma *variável* *global* porque ela é declarada fora de qualquer função e, assim, todas as funções do nosso sketch podem lê-la e atualizá-la. Esse tipo de variável é útil para registrar valores que mudam por causa de interações do usuário enquanto nosso sketch está rodando.

E, neste caso, nossa variável `bgColorIndex` também é uma variável de *estado*, porque registra qual cor foi escolhida pela pessoa por meio da sua interação. É uma variável que está sendo usada para lembrar algo sobre o histórico do nosso sketch.

## Contando Cliques

Vamos estender o exemplo anterior: em vez de registrar um índice de cor, vamos usar uma variável para registrar os cliques do mouse e então selecionar uma cor e uma forma com base no número de vezes que o mouse foi clicado durante a execução do nosso sketch.

Neste exemplo vamos ter $$7$$ cores para percorrer e $$2$$ formas, mas se registrarmos o número de cliques e usarmos o operador módulo (`%`), podemos facilmente alternar entre as opções de forma e de cor usando apenas uma variável de estado.

{% include p5-editor.html id="MPC3wyl6u" %}

## Memória de Curto Prazo

Agora, vamos ver um exemplo em que registramos mais de um valor da pessoa.

Neste sketch registramos eventos do mouse e sua posição para desenhar um retângulo entre onde o mouse foi pressionado e onde foi solto. Para conseguir isso, criamos uma variável global chamada `mouseDown` para armazenar um objeto com a posição em que o mouse foi pressionado pela última vez. Quando o mouse é solto, podemos usar essas variáveis para definir a posição do nosso retângulo e, junto com `mouseX` e `mouseY` do momento em que foi solto, calcular a largura e a altura do retângulo.

{% include p5-editor.html id="rGd2tsHfe" %}

Vamos tentar uma interação parecida, mas agora o retângulo é desenhado entre duas posições especificadas por cliques do mouse, em vez de um clique e um soltar. Neste caso precisamos registrar a posição do primeiro clique do mouse e também se é o primeiro ou o segundo clique.

Ainda precisamos de apenas uma variável, `rectStart`, para registrar a posição em que o mouse foi clicado. E podemos usar essa mesma variável para determinar se estamos detectando o primeiro ou o segundo clique em uma sequência de cliques.

Se um dos valores de `rectStart` for $$-1$$, estamos detectando a posição inicial do retângulo; caso contrário, se `rectStart` já tem posições válidas, é o segundo clique e usamos `mouseX` e `mouseY` para desenhar o retângulo e depois reiniciar `rectStart`.

{% include p5-editor.html id="hN3FFHQjr" %}

## Momentos

Podemos usar uma variável global de estado para definir *momentos* no nosso sketch, por exemplo, se precisarmos registrar qual fase de um jogo devemos desenhar.

Como um exemplo mais simples, digamos que queremos apresentar uma tela de introdução antes de tocar uma animação. Podemos usar uma variável que registra se estamos esperando a pessoa clicar o mouse ou se a animação já começou, e desenhar canvases completamente diferentes dependendo da situação:

Neste caso exibimos uma tela de introdução e, quando a pessoa clica o mouse, começa uma animação. Depois que a animação começa, os cliques não têm efeito. O diagrama de estados para essa situação seria algo assim:

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/state-00.jpg' | relative_url }}"/>
</div>

Como temos apenas dois estados possíveis (`Intro` e `Animation`), podemos usar um valor booleano (`true` ou `false`) para registrar o estado atual e escolher qual tela desenhar.

{% include p5-editor.html id="AKfDG1IDA" %}

## Mais Estados

Se, em vez disso, quisermos permitir que a pessoa pause a animação depois que ela começou, poderíamos implementar as seguintes transições de estado:

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/state-01.jpg' | relative_url }}"/>
</div>

Agora temos $$3$$ estados e vamos ter que usar um número para registrar em qual deles estamos. Podemos usar os números $$0$$, $$1$$ e $$2$$ para os nossos estados `Intro`, `Animation` e `Pause`, respectivamente, e em cada execução de `draw()` vamos escolher o que desenhar com base no estado em que estamos, e em cada execução de `mouseClicked()` vamos atualizar nosso estado atual.

Neste sketch em particular, podemos obter o efeito de pausar simplesmente não fazendo nada. Isso acontece porque, se não chamarmos `drawIntro()` nem `drawAnimation()`, não limpamos o canvas, a posição da nossa forma não é atualizada e nada de novo é desenhado na tela.

{% include p5-editor.html id="qmarERj30" %}
