---
title: Teachable Machine
---

Vimos como usar a [biblioteca ml5](https://ml5js.org/) para detectar rostos e mãos em imagens, e mencionamos rapidamente que ela pode ser usada para detectar objetos ou classificar imagens vindas de streams de vídeo.

Vamos montar um sketch para fazer classificação em um stream de vídeo.

Vamos começar modificando o nosso sketch de detecção de mãos para funcionar com a câmera. Vamos ter que adicionar um stream de captura e, em vez de chamar `detect()` só uma vez em uma imagem estática, vamos ter que continuar chamando `detect()` toda vez que terminarmos de detectar. Então a nossa função callback de detecção vai ficar assim agora:

```js
function doneDetecting(results) {
  mResults = results;
  model.detect(mCamera, doneDetecting);
}
```

Como estamos desenhando o stream de vídeo a cada frame do `draw()`, e podemos estar desenhando mais rápido do que detectando, não podemos mais contar com o desenho dos pontos de referência dentro da função callback. O que podemos fazer é copiar os resultados da detecção para uma variável global, para garantir que eles estejam disponíveis na nossa função `draw()`. É isso que `mResults = results;` está fazendo.

O resto do sketch deve parecer bem familiar: rodamos `detect()`, disponibilizamos os resultados globalmente e rodamos `detect()` de novo. Depois, no `draw()`, iteramos pelos resultados mais recentes e desenhamos pontos de referência para as mãos detectadas:

{% include p5-editor.html id="xQrvjIfCT" %}

Isso funciona e poderíamos usar localizações específicas dos pontos de referência para detectar gestos básicos, como pinças ou toques.

## Modelo de Classificação

Vamos testar o modelo de classificação de imagens do `ml5`.

Como a biblioteca `ml5` embrulha vários modelos diferentes em interfaces consistentes, podemos trocar o nosso modelo de mãos por um modelo de classificação com algumas mudanças pequenas.

O novo modelo é instanciado assim:
```js
model = ml5.imageClassifier("MobileNet");
```

E usado assim:
```js
model.classify(mCamera, doneDetecting);
```

Os resultados não são listas de mãos ou rostos e seus objetos de pontos de referência, e sim listas de objetos detectados e o quão confiante o modelo está sobre a classificação da imagem. Por exemplo:

```js
[
  {
    "label": "wig",
    "confidence": 0.195567
  },
  {
    "label": "mask",
    "confidence": 0.026581
  },
  {
    "label": "neck tie",
    "confidence": 0.025564
  },
]
```

Por padrão, o modelo só retorna as 3 melhores classes (top-3), mas isso pode ser ajustado com o parâmetro `{topk: K}` no construtor do modelo, onde `K` é um inteiro que especifica quantas classes reportar.

{% include p5-editor.html id="C-ZVXe2Xm" %}

Como mencionado anteriormente, a documentação do `ml5` não especifica de fato qual versão do `MobileNetV2` foi usada, nem quais classes esse modelo detecta.

O resultado `wig, mask, neck tie` foi para uma foto de um rosto humano 🤔.

Outras imagens dão resultados um pouco mais precisos, ou melhor, razoáveis:

<div class="scaled-images left">
  <img src="{{ 'assets/images/ai/tm-classification-00.jpg' |relative_url }}">
</div>

Em alguns casos eles são muito específicos e, em outros, variam bastante.

Isso não é um problema, mas, se vamos usar esse modelo, ajudaria saber exatamente quais classes ele foi treinado para identificar. Também podemos querer adicionar, remover ou editar as classes do modelo.

Felizmente, existe um projeto separado que nos ajuda justamente com isso.

## Teachable Machine

O [Teachable Machine](https://teachablemachine.withgoogle.com/) é uma plataforma web que nos ajuda a coletar dados e fazer o ajuste fino (finetuning) de certos tipos de modelos.

Quando acessamos o site, primeiro temos que escolher o tipo de projeto no qual estamos trabalhando, para que ele configure o framework e o modelo inicial corretos para o nosso ajuste. Vamos ver como usá-lo para ampliar um modelo de classificação de imagens.

De novo, esses são modelos que olham uma imagem e preveem a qual classe de objetos ela corresponde. Podem ser usados para diferenciar raças de cachorros, ou classificar dígitos escritos em envelopes. O que queremos fazer, no entanto, é usar um modelo de classificação para detectar e interpretar sinais da Língua de Sinais Americana (ASL).

Vimos o modelo de pose de mão do `ml5`, e poderíamos usá-lo para detectar pontos de referência das mãos, mas aí precisaríamos criar manualmente relações específicas entre pontos de referência para cada uma das letras que queremos detectar. Com os dados certos, podemos simplesmente treinar um modelo de classificação de imagens para fazer a mesma coisa, só que de forma mais automatizada.

### Os Dados

Vamos até o [Teachable Machine](https://teachablemachine.withgoogle.com/), selecionar _Image Project_ e depois _Standard image model_. Somos recebidos por esta interface:

<div class="scaled-images left">
  <img src="{{ 'assets/images/ai/tm-interface-00.jpg' |relative_url }}">
</div>

Podemos começar adicionando uma classe para a letra 'A'. Depois de dar um nome a ela, poderíamos começar a enviar imagens do sinal dessa letra que encontrarmos na internet, mas a interface nos dá uma forma mais simples de fazer isso: vídeo. Se clicarmos no botão Webcam, podemos usar o vídeo para gerar uma série de imagens do sinal da letra 'A'.

<div class="scaled-images left">
  <img src="{{ 'assets/images/ai/tm-interface-01.jpg' |relative_url }}">
</div>

Ao gerar imagens para treinar um modelo de classificação, é bom ter a maior variedade possível de imagens. O sinal da mão deve ser sempre o da letra 'A', mas a sua posição na tela, a distância da câmera, a iluminação, o fundo, etc... devem ser todos diferentes de imagem para imagem.

Queremos que o modelo capte os elementos em comum de todas as imagens.

{% include video.html url="ai/tm-capture-00.webm" %}

Às vezes é uma boa ideia adicionar uma classe "_empty_" (vazia) ao nosso conjunto de dados, com imagens que não fazem parte de nenhuma outra classe. Para este exemplo, pode ser uma série de imagens sem mãos, ou com mãos que não estão mostrando nenhum dos sinais do alfabeto.

<div class="scaled-images left">
  <img src="{{ 'assets/images/ai/tm-interface-02.jpg' |relative_url }}">
</div>

Quando tivermos coletado imagens variadas o suficiente para todas as classes que queremos detectar, podemos apertar o botão _Train Model_. Isso vai usar as nossas imagens para modificar um modelo pré-treinado, fazendo com que ele detecte apenas as classes que estamos especificando.

Quando o modelo estiver pronto, o Teachable Machine nos dá uma interface para testá-lo e, mais importante, uma forma de adicioná-lo a um sketch `p5.js` usando o `ml5`.

### O Modelo

Quando clicamos no botão _Export Model_, vemos uma tela assim:

<div class="scaled-images left">
  <img src="{{ 'assets/images/ai/tm-export-00.jpg' |relative_url }}">
</div>

Queremos selecionar a aba _Tensorflow.js_ e a opção _Upload_, e depois clicar no botão _Upload my model_.

Quando o modelo terminar de ser enviado, queremos copiar a url que ele nos dá para o nosso modelo com ajuste fino.

Para este exemplo foi:
```
https://teachablemachine.withgoogle.com/models/hrStMX1V2/
```

Guarde a url em algum lugar. Não se preocupe muito com o código de exemplo em `p5.js` que o Teachable Machine fornece. Ele foi escrito para uma versão mais antiga do `p5.js` e uma versão mais antiga do `ml5`, e não funciona, então vamos simplesmente modificar o código que já temos funcionando.

### O Código `p5.js`

Se começarmos com o exemplo simples de classificador de imagens acima, tudo o que precisamos fazer é mudar a linha que instancia o nosso modelo para usar o modelo que ajustamos.

Fazemos isso com:
```js
model = ml5.imageClassifier(modelUrl + "/model.json");
```

Onde `modelUrl` é a url que recebemos do Teachable Machine acima.

E é isso ! Pronto ! Agora temos um modelo de classificação de imagens personalizado, com ajuste fino.

{% include p5-editor.html id="3plC1NlxY" %}

Podemos mudar um pouco a saída, adaptá-la para fazer algo visual ou sonoro acontecer com base nas letras detectadas, mas a maior parte do código para configurar a câmera, o modelo e a detecção continua igual.

<div class="scaled-images left">
  <img src="{{ 'assets/images/ai/tm-result-00.jpg' |relative_url }}">
</div>
