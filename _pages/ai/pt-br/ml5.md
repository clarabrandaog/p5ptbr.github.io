---
title: ml5
---

A forma mais fácil de começar a rodar modelos de IA no `p5.js` é usando a [biblioteca `ml5`](https://ml5js.org/).

A biblioteca `ml5` nos ajuda a integrar vários modelos úteis de machine learning aos nossos sketches, oferecendo uma interface comum para carregar e rodar modelos para tarefas diferentes.

Podemos carregar a biblioteca nos nossos projetos adicionando algo assim ao cabeçalho do nosso arquivo `index.html`:

```html
<script src="https://cdn.jsdelivr.net/npm/ml5@1.2.1/dist/ml5.min.js"></script>
```

Digamos que queremos detectar rostos em uma imagem. Podemos começar com um código que carrega uma imagem com rostos a partir de uma URL:

{% include p5-editor.html id="FhxxKB7-g5" %}

Legal.

Agora podemos dar uma olhada na documentação da classe [`FaceMesh`](https://docs.ml5js.org/#/reference/facemesh?id=step-by-step-guide) do `ml5` para ver como instanciar e usar um modelo que não só detecta rostos e onde eles estão em uma imagem, mas também onde ficam certos "componentes" ou pontos-chave dos rostos.

Este sketch carrega a biblioteca e instancia um objeto `FaceMesh` com parâmetros padrão:

{% include p5-editor.html id="6i0DCr0hUk" %}

Para de fato detectar rostos, temos que chamar a função `detect()` em uma imagem, assim:

```js
faceMesh.detect(img, callback);
```

Ohhh... _callback_ ! Como detectar rostos, ou rodar qualquer tipo de modelo de ML, é uma operação demorada, e não queremos que o nosso sketch congele enquanto o modelo roda, geralmente vamos usar callbacks ou funções `async`/`await`. Ainda bem que acabamos de aprender sobre elas, [aqui](../concurrency/).

Podemos criar uma função _callback_ `doneDetecting()` para imprimir os resultados da nossa detecção:

{% include p5-editor.html id="WcVfByzKU" %}

Rodando o código acima, vemos que a função `detect()` retorna uma lista de objetos que se parecem com isto:

```js
{
  box: Object,
  keypoints: Array,
  faceOval: Object,
  leftEye: Object,
  ...
}
```

E, se explorarmos os objetos dessa lista, vamos começar a ver membros chamados: `box`, `lips`, `leftEye`, `faceOval`, etc... que são objetos que contêm a localização de pontos de referência do rosto ou informações gerais sobre o rosto detectado.

Por exemplo, podemos desenhar uma caixa em volta do rosto chamando `rect()` com os valores apropriados:
```js
rect(box.xMin, box.yMin, box.width, box.height);
```

Ou desenhar elipses nos olhos, com:
```js
ellipse(leftEye.centerX, leftEye.centerY, 16);
ellipse(rightEye.centerX, rightEye.centerY, 16);
```

{% include p5-editor.html id="Qy5cK8xwf" %}

## Mais Rostos !

Por padrão, o objeto `FaceMesh` detecta apenas um rosto por imagem. Se quisermos que ele detecte mais, temos que dar ao seu construtor um objeto com alguns parâmetros:

```js
ml5.faceMesh({ maxFaces: 5 });
```

E agora só precisamos lembrar de iterar pelos resultados na função callback:

{% include p5-editor.html id="Tj7HLq7b9" %}

## Malhas

O modelo `FaceMesh` é, na verdade, bom para obter uma malha (mesh) com todos os pontos de um determinado rosto.

Podemos ver como essas malhas se parecem iterando pelo membro `keypoints` de cada um dos objetos de rosto resultantes:

{% include p5-editor.html id="-Kp0n6Mfg" %}

## Mais do que Rostos Bonitos

O código que temos até agora pode ser facilmente estendido para funcionar com um stream de vídeo em vez de imagens estáticas, e trocar o modelo por outro dos modelos do `ml5` também é muito fácil.

Digamos que agora queremos detectar mãos em imagens. Tudo o que precisamos fazer é instanciar um objeto `HandPose` e fazer algumas mudanças bem pequenas no código acima:

```js
handPose = ml5.handPose({ maxHands: 5 });

// e depois ...
handPose.detect(mImg, doneDetecting);
```
E é isso. O resto continua igual:

{% include p5-editor.html id="_PonOkHI3" %}

Este modelo pode ser super útil para detectar gestos ou sinais feitos com as mãos.

## Mais Modelos

Além desses dois modelos para mãos e rostos, a biblioteca `ml5` também tem um modelo para detectar [pose corporal](https://docs.ml5js.org/#/reference/bodypose), um modelo que [classifica](https://docs.ml5js.org/#/reference/sentiment) textos como `positive` ou `negative`, e modelos que fazem classificação de [imagens](https://docs.ml5js.org/#/reference/image-classifier) e [áudio](https://docs.ml5js.org/#/reference/sound-classifier) com base em classes pré-determinadas.

Esses dois últimos são limitados pelos conjuntos de imagens/sons que foram usados durante o treinamento. O classificador de som detecta apenas os dez dígitos de _zero_ a _nove_ e as palavras _up_, _down_, _left_, _right_, _stop_, _go_, _yes_ e _no_.

O classificador de imagens usa alguns modelos pré-construídos, incluindo o MobileNetV2, mas não especifica de fato a lista de classes que usa. Pode ser [esta](https://deeplearning.cms.waikato.ac.nz/user-guide/class-maps/IMAGENET/).

Se quisermos classificar outros tipos de imagens ou sons, temos que treinar os nossos próprios modelos.

Felizmente, o `ml5` tem uma forma de carregar modelos que podemos treinar rapidamente usando algo chamado [Teachable Machine](https://teachablemachine.withgoogle.com/).

Vamos ver isso [a seguir](../teachable-machine/).
