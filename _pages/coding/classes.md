---
title: Classes
---

Temos aprendido sobre [arrays](../arrays/) e [objetos](../objects/), e essas estruturas funcionam muito bem para organizar dados: valores que se relacionam entre si podem ser agrupados e acessados de diferentes formas.

Vamos revisar o nosso exercício com objetos. Temos um array de obstáculos, e cada obstáculo é um objeto que guarda suas posições `x` e `y`, `diameter`, `fill color` e `overlap color`.

{% include p5-editor.html id="X7DALVA_q" %}

## Código com Classe

E se também quisermos agrupar comportamento e funcionalidade junto com esses valores? Ou seja, não queremos apenas guardar o `diameter` e as posições `x` e `y` de um obstáculo, mas também definir *como* eles se comportam e o que acontece quando colidem entre si ou com o mouse.

Agora, a função `draw()` é a responsável por verificar a sobreposição. Isso pode ficar muito bagunçado se continuarmos adicionando funcionalidade ou complexidade aos nossos obstáculos, como desenhar várias formas quando há sobreposição.

Este é um caso perfeito para usar classes. Podemos pensar nas classes como super objetos: objetos guardam valores indexados por nomes; as classes também fazem isso, mas também guardam suas próprias funções para manipular esses valores.

A estrutura geral para definir uma classe é mais ou menos assim:

```js
class ClassName {
  constructor(param0, param1) {
    // inicializa alguns valores e propriedades internos
  }

  functionName() {
    // faz coisas com os valores e propriedades internos
  }
}
```

As definições de classe têm uma função especial chamada `constructor()` que é usada para inicializar *instâncias* da nossa classe e definir variáveis internas. As definições de classe também contêm outras funções específicas da classe, usadas para manipular os valores e parâmetros internos das *instâncias* da classe.

O que são *instâncias*?

Vamos ver um exemplo. Vamos redefinir os nossos obstáculos do exemplo acima para usar uma classe `Obstacle`. Não precisamos definir nenhum comportamento para esses objetos, então a nossa classe terá apenas um `constructor()` por enquanto.

No construtor, quando queremos atribuir valores aos parâmetros `x`, `y`, `diameter` e `color` de uma instância, colocamos a palavra `this` antes desses nomes de variáveis, para deixar explícito que esses valores pertencem a uma *instância* da classe `Obstacle`, mais especificamente, que esses valores pertencem ao novo obstáculo que estamos criando com a função `constructor()`.

E, como os obstáculos agora estão em uma classe, a forma de criar um novo obstáculo com base na definição da nossa classe `Obstacle` é usando a palavra `new`. Essa linha de código vai executar a função `constructor()` da definição da nossa classe `Obstacle` e retornar uma nova *instância* de um `Obstacle`, com seus próprios valores para os parâmetros `x`, `y`, `diameter` e color:
```js
let anObstacle = new Obstacle();
```

Podemos até colocar isso dentro de um loop para criar várias *instâncias* de objetos `Obstacle`, cada uma com seus próprios parâmetros. A forma de acessar os parâmetros de uma instância de classe é exatamente como acessamos os parâmetros de um objeto JavaScript simples: usando a *notação de ponto*.

Se tivermos isto no construtor:
```js
this.x = 10;
this.y = 100;
this.r = 24;
```

Podemos instanciar um objeto e acessar suas variáveis *membro* assim:
```js
let mObs = new Obstacle();
print("x is: ", mObs.x, " y is: ", mObs.y);
```

{% include p5-editor.html id="zspwKXIMx" %}

## Comportamento com Classe

Então, além da definição da classe e de como instanciamos nossos obstáculos no `setup()`, não mudou muita coisa: o resto do código, especialmente o código do `draw()`, permaneceu exatamente o mesmo. A única diferença é que agora os parâmetros de cada obstáculo foram colocados dentro de um objeto de classe. A função `draw()` ainda faz todo o trabalho de verificar a sobreposição com a elipse do mouse, mas agora os parâmetros de cada obstáculo vêm de um objeto de classe, e não de um objeto JavaScript simples. Vamos mover essa lógica para funções dentro da nossa classe.

Como só nos importa a sobreposição com a elipse do mouse, vamos chamar essa função de `hovered()` e verificar apenas a sobreposição com a elipse do mouse. Como todas as informações sobre o obstáculo estão disponíveis dentro dele mesmo através do operador `this`, a função `hovered()` não precisa de nenhum parâmetro de entrada.

{% include p5-editor.html id="CmGNj7rt2" %}

O comportamento geral resultante é o mesmo, mas toda a lógica necessária para definir a funcionalidade dos nossos obstáculos agora está dentro da definição da nossa classe `Obstacle`, e isso facilita ler e atualizar o nosso código e adicionar novas funcionalidades e comportamentos aos nossos obstáculos.

## Encapsulamento com Classe

Agora que temos uma forma de *encapsular* parâmetros, comportamento e funcionalidade dos nossos obstáculos dentro de uma classe, podemos desenhar formas um pouco mais complexas quando há uma sobreposição com a elipse do mouse.

Para *encapsular* isso por completo dentro dos objetos de classe, vamos criar uma função dentro da nossa classe que será responsável por verificar a sobreposição e por desenhar a forma com base no resultado da verificação. A lógica será parecida com a que está implementada no `draw()`, mas agora será responsabilidade do objeto verificar a sobreposição e se desenhar.

{% include p5-editor.html id="FFQ9SNAk5" %}

É isso que *encapsulamento* quer dizer: separamos a lógica interna dos nossos obstáculos das nossas funções principais `setup()` e `draw()`. Se olharmos os loops nas nossas funções `setup()` e `draw()`, eles são muito fáceis de ler e entender. Um cria novos objetos `Obstacle`, o outro desenha cada objeto `Obstacle`.

```js
setup() {
  // ...
  for (let i = 0; i < numObs; i++) {
    let anObstacle = new Obstacle();
    allObstacles.push(anObstacle);
  }
}

draw() {
  // ...
  for (let i = 0; i < allObstacles.length; i++) {
    let anObstacle = allObstacles[i];
    anObstacle.draw();
  }
}
```

Se tivermos interesse nos detalhes do que acontece dentro dos nossos objetos `Obstacle`, podemos olhar a implementação da classe separadamente.

## Desenhos com Classe

Vamos adicionar desenhos mais sofisticados aos nossos obstáculos quando o mouse passa por cima deles.

Vamos desenhar $$4$$ elipses concêntricas em vez de apenas uma, com valores variados de opacidade e diâmetro, em que as elipses maiores são mais transparentes e são desenhadas primeiro.

Este cálculo transforma a sequência $$(0, 1, 2, 3)$$ nos seguintes valores de transparência $$(64, 128, 192, 256)$$:
```js
let mAlpha = ((i + 1) / 4) * 256;
```

E este transforma a sequência $$(0, 1, 2, 3)$$ nos seguintes multiplicadores para o valor original do diâmetro $$(4, 3, 2, 1)$$:
```js
let mDiameterMultiplier = 4 - i;
```

Agora, quando o mouse passa por cima de um obstáculo, círculos concêntricos são desenhados, mas toda a lógica está implementada dentro da classe `Obstacle`: nossas funções principais `setup()` e `draw()` permaneceram iguais.

{% include p5-editor.html id="gG464xVSH" %}

Agora fica mais fácil adicionar ainda mais obstáculos ao nosso sketch e ter obstáculos com números diferentes de *anéis*.

{% include p5-editor.html id="q565eCcti" %}
