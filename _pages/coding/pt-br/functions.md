---
title: Funções
---
## Organizar e Repetir

Vimos como usar loops [`for()`](../patterns/) para executar o mesmo código várias vezes e criar padrões.

Existe outro método que podemos usar para repetir ações no nosso código: funções.

Funções são sequências de comandos pré-definidas e nomeadas que podem ser facilmente reexecutadas. Além desse *encapsulamento* que facilita repetir sequências de comandos, as funções também são *parametrizáveis*, o que permite que tenham comportamentos parecidos, mas diferentes, dependendo das informações que são enviadas a elas durante a execução. Mais sobre isso daqui a pouco.

É assim que definimos uma função em JavaScript:
```js
function functionName(param0, param1, ...) {
  command0;
  command1;
  ...
}
```

Já vimos muitas funções pré-definidas do p5.js, como `rect()`, `ellipse()`, `line()`, etc. Todas elas recebem parâmetros que afetam como e onde as formas são desenhadas, como as posições x e y e a largura e altura da forma. Mas agora vamos ver como definir nossas próprias funções.

Vamos ver um caso em que definir uma função pode simplificar o código que escrevemos e diminuir o número de variáveis que precisamos manter.

Vimos como desenhar formas simples como quadrados, retângulos e elipses, mas e se quiséssemos desenhar um coração?

Podemos começar tentando montar algumas formas simples como triângulos e elipses ou arcos:

{% include p5-editor.html id="VLcKw3Lu1" %}

Não fica tão mal se desligarmos os contornos das formas:

{% include p5-editor.html id="d0RXiJS3K" %}

E se agora quisermos desenhar muitos corações, possivelmente de tamanhos diferentes, em lugares diferentes?

Poderíamos tentar descobrir as posições exatas em pixels de todos os nossos corações assim:

{% include p5-editor.html id="0Ma3DR5HQ" %}

Mas isso é tedioso e propenso a erros.

Seria uma ideia melhor encontrar uma forma genérica de descrever as formas do coração e depois repetir esses passos em diferentes lugares do nosso canvas.

Uma maneira possível de fazer isso é escolher um ponto mais ou menos no centro da forma do nosso coração e depois definir todos os outros pontos em termos da posição desse ponto. Por exemplo, no desenho abaixo, o ponto preto é o nosso ponto de referência ($$x_0$$, $$y_0$$), e vamos descrever todos os pontos vermelhos em relação a ele:

<div class="scaled-images">
  <img src="{{ '/assets/images/p5/heart-00.jpg' | relative_url }}">
</div>

Para o nosso coração, vamos definir $$x_0 = 200$$ e $$y_0 = 150$$ e expressar todos os parâmetros `x` e `y` das nossas formas em termos dessas duas variáveis:

{% include p5-editor.html id="fWqOx2u9U" %}

Agora poderíamos copiar e colar as linhas que desenham as formas e apenas mudar os valores de $$x_0$$ e $$y_0$$ antes de chamá-las:

{% include p5-editor.html id="8UQDXlWOb" %}

Isso já é mais fácil de ler e modificar, mas digamos que agora queremos parametrizar o tamanho da nossa forma de coração. Teríamos que encontrar com muito cuidado todos os lugares do nosso sketch onde estamos desenhando corações para atualizar as linhas de código corretas com as mudanças.

As funções podem ajudar aqui. Vamos usar uma função para *encapsular* todos os comandos e valores usados ao desenhar um coração e, assim, quando precisarmos atualizar como desenhamos o coração mais tarde, só teremos que mudar o nosso código em um único lugar.

Vamos envolver os nossos comandos atuais de desenho do coração dentro de uma função `heart()` que recebe dois parâmetros, as posições x e y, e desenha um coração:

```js
function heart(x0, y0) {
  ellipse(x0 - 40, y0 - 15, 100, 100);
  ellipse(x0 + 40, y0 - 15, 100, 100);
  triangle(x0 - 88, y0, x0 + 88, y0, x0, y0 + 135);
}
```

Apenas copiamos o código de antes, mas agora que essas linhas estão dentro de uma função, os parâmetros `x0` e `y0` referenciam posições x e y específicas que são passadas para a função cada vez que ela é *chamada*. Desenhar o coração em lugares diferentes agora é tão fácil quanto isto:

{% include p5-editor.html id="qBPTEhE6I" %}

E agora podemos investir em adicionar um parâmetro de tamanho à forma do nosso coração:

{% include p5-editor.html id="xTVlTFlBH" %}

A matemática não é muito intuitiva, mas só precisou ser descoberta uma vez, e agora podemos chamar a nossa função `heart()` repetidamente em lugares diferentes e com tamanhos diferentes:

{% include p5-editor.html id="VUMYTzsIT" %}

Ou usar loops [`for()`](../patterns/) para criar padrões no nosso canvas:

{% include p5-editor.html id="v096x_jMT" %}

Que variam de tamanho com base na distância ao centro do canvas:

{% include p5-editor.html id="pVStO6jxX" %}

## Calcular e Retornar

Acabamos de ver como definir uma função para *capturar* um procedimento (uma lista de comandos), que pode ser chamado de novo mais tarde.

As funções também podem ser definidas para agir um pouco mais como as [funções matemáticas](https://en.wikipedia.org/wiki/Function_(mathematics)) que talvez tenhamos aprendido nas aulas de álgebra: um conjunto de operações que mapeia uma lista de entradas para valores de saída.

Vamos ver o que isso significa revisitando uma situação de detecção de colisão:

{% include p5-editor.html id="UB-XWHKuP" %}

Se quisermos detectar quando o círculo do mouse está se sobrepondo ao círculo maior, podemos primeiro calcular a distância entre o centro dos dois círculos e ver se esse valor é menor que a soma dos seus raios:

<div class="scaled-images">
  <img src="{{ '/assets/images/p5/overlap-02.jpg' | relative_url }}">
</div>

<div class="scaled-images">
  <img src="{{ '/assets/images/p5/overlap-03.jpg' | relative_url }}">
</div>

Podemos calcular a [distância *euclidiana*](https://en.wikipedia.org/wiki/Euclidean_distance#Two_dimensions) entre os dois círculos usando esta fórmula, onde $$x_0$$ e $$y_0$$ são as posições do círculo grande e $$x_m$$ e $$y_m$$ são as posições do círculo do mouse:

$$\displaystyle distance = \sqrt{(x_m - x_0)^2 + (y_m - y_0)^2} $$

É meio complicada, mas não é tão difícil de implementar em JavaScript. Existe uma função nativa `sqrt()`, e podemos elevar números a uma potência usando um asterisco duplo `**`, então $$3^{12}$$, por exemplo, vira `3 ** 12`:
```js
let d = sqrt((mouseX - x0) ** 2 + (mouseY - y0) ** 2);
```

Por fim, podemos verificar a sobreposição comparando essa distância com a soma dos *raios* dos círculos, que é metade dos seus diâmetros:
```js
if(d < oDiam / 2 + mDiam / 2){
  // sobreposição !
}
```

{% include p5-editor.html id="8ZRnyGWwr" %}


Funciona, mas... e se tivermos mais de um círculo de obstáculo?

{% include p5-editor.html id="444Zq22any" %}

Poderíamos colocar um monte de expressões `sqrt((mouseX - xi)**2 + (mouseY - yi)**2)` espalhadas pelo nosso código, mas isso rapidamente fica difícil de ler e ajustar:

{% include p5-editor.html id="iKnvWbH94" %}

Do jeito que está, não seria muito simples atualizar esse código para também mudar a cor dos círculos de obstáculo durante a colisão.

Vamos usar funções para simplificá-lo.

A primeira função que vamos declarar é uma função que recebe dois pares de coordenadas ($$x$$, $$y$$) e calcula a distância euclidiana entre eles:
```js
function euclidean(x0, y0, x1, y1) {
  let d = sqrt((x1 - x0) ** 2 + (y1 - y0) ** 2);
  return d;
}
```

Isso faz exatamente o mesmo cálculo que fazíamos acima, só que agora os valores do segundo círculo vêm dos parâmetros da função `x1` e `y1`, e não dos valores [*fixos no código* (hardcoded)](https://en.wikipedia.org/wiki/Hard_coding) `mouseX` e `mouseY`. Assim podemos reutilizar essa função para calcular qualquer distância entre qualquer par de pontos do nosso canvas.

A outra coisa a notar é a última linha da função: `return d;`. Ela diz à função que o valor da variável `d` deve ser usado em outro lugar, então, depois de fazer o cálculo, ela deve disponibilizar esse valor para qualquer parte do código que invocou (ou chamou) a função. Então, se chamarmos a função assim: `let mDist = euclidean(3, 6, 6, 2)`, a variável `mDist` teria o valor `5`;

Colocando isso no nosso código:

{% include p5-editor.html id="dZFmGMkbe" %}

Isso já é melhor, mas ainda temos algum código repetido.

Talvez possamos simplificar ainda mais criando uma função que recebe *seis* valores, as posições e os diâmetros de dois círculos, e retorna um booleano que diz se os dois círculos estão se sobrepondo ou não.

Podemos reutilizar a função `euclidean()` que já temos e escrever uma função de sobreposição assim:
```js
function ellipseOverlap(x0, y0, diam0, x1, y1, diam1) {
  let d = euclidean(x0, y0, x1, y1);
  return (d < diam0 / 2 + diam1 / 2);
}
```

Desta vez a instrução `return` vai calcular e retornar o valor booleano da expressão `(d < diam0 / 2 + diam1 / 2)`. Ele será `true` se os círculos se sobrepõem, ou `false` caso contrário.

{% include p5-editor.html id="Z4nagBNTZ" %}

Agora que temos variáveis separadas que nos dizem se o nosso círculo do mouse está se sobrepondo a algum dos círculos de obstáculo, fica mais fácil mudar a cor dos círculos de obstáculo e adicionar outras indicações visuais ao nosso sketch:

{% include p5-editor.html id="cJ8ryLXID" %}

O `||` na instrução `if (o0 || o1 || o2 || o3 || o4)` é o operador lógico *OU*. Uma expressão construída com o operador `||` resulta em `true` se qualquer uma de suas condições for verdadeira. No nosso caso, estamos perguntando se há sobreposição com *qualquer* um dos obstáculos e, se houver, para mudar a cor do círculo do mouse.

Agora podemos refatorar o desenho dos obstáculos em outra função que não precisa retornar nada, mas vai desenhar o obstáculo com a cor certa dependendo de haver ou não sobreposição:
```js
function obstacle(x, y, overlap) {
  if (overlap) {
    // desenha com as cores e estilos de sobreposição
  } else {
    // desenha um círculo branco
  }
}
```

E agora é fácil adicionar mais obstáculos:

{% include p5-editor.html id="Z7H4RJXv4" %}

E, olhando o código, podemos até colocar a chamada à função `ellipseOverlap()` dentro da função `obstacle()`, desde que a nova função retorne se há ou não sobreposição:
```js
function obstacle(x, y) {
  let overlap = ellipseOverlap(mouseX, mouseY, mDiam, x, y, oDiam);

  if (overlap) {
    // desenha com as cores e estilos de sobreposição
  } else {
    // desenha um círculo branco
  }

  return overlap;
}
```

O código da função de sobreposição ficou um pouco mais complicado, mas só precisamos escrevê-lo uma vez. O benefício é que o nosso código principal fica ainda mais fácil de ler e manter, e de adicionar e remover obstáculos, etc.:

{% include p5-editor.html id="2A9QJrOcR" %}

Vamos ver como [arrays](../arrays/) que guardam listas de números podem nos ajudar a adicionar mais obstáculos a um sketch como este, sem precisar enumerar seus valores individualmente como estamos fazendo aqui com `x0`, `x1`, `x2`, etc.
