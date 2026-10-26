---
title: Modelos Gradio
---

Não dá para falar de IA sem falar de LLMs e modelos de difusão.

Um passo acima em complexidade dos modelos [que acabamos de ver](../transformers/), esses modelos generativos geralmente exigem mais recursos para rodar. Isso não quer dizer que não possamos usá-los nos nossos sketches, mas temos que transferir o processamento e a execução deles para computadores com grandes quantidades de memória, processadores mais rápidos e, normalmente, algumas GPUs.

Vamos fazer isso chamando _APIs_ de servidores ou serviços que rodam esses modelos maiores:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/gradio-server-00.jpg' |relative_url }}">
</div>

Vamos ver alguns exemplos.

## Hugging Face / Gradio

Além de ser o GitHub dos modelos de código aberto, o Hugging Face também hospeda modelos com os quais podemos interagir por meio de código e APIs. 

Isso acontece principalmente através de uma biblioteca chamada [`Gradio`](https://www.gradio.app/). Essa biblioteca foi desenvolvida para facilitar a criação de interfaces web simples para modelos complexos, mas, quando publicados no Hugging Face, os apps `Gradio` também expõem _endpoints_ de API que nos permitem rodar modelos remotamente.

Podemos começar olhando [aqui](https://huggingface.co/spaces) para ver diferentes modelos que as pessoas configuraram para rodar nos servidores do Hugging Face, e depois descobrir como nos conectar e interagir com eles a partir dos nossos sketches.

Vamos revisitar o nosso exemplo de descrição de imagens:

{% include p5-editor.html id="ZuwaVgRID" %}

Neste exemplo, rodar o modelo no navegador não só congelou o nosso sketch, como os resultados também não foram muito precisos, porque é um modelo mais antigo.

Vamos usar a mesma estrutura para o nosso novo sketch, para podermos reaproveitar uma boa parte do código acima:

{% include p5-editor.html id="7Dp5UhgtS" %}

Mas, desta vez, vamos testar o modelo de descrição de imagens hospedado [aqui](https://huggingface.co/spaces/aifeifei798/Image-To-Flux-Prompt).

A primeira coisa a fazer é visitar a [página de interface](https://huggingface.co/spaces/aifeifei798/Image-To-Flux-Prompt) do modelo e rolar até o final, até vermos o botão _Use via API_. Clicando nesse botão e depois selecionando a opção `JavaScript`, vemos algo assim:

<div class="scaled-images left">
  <img src="{{ 'assets/images/ai/gradio-api-00.jpg' |relative_url }}">
</div>

De novo, não conseguimos importar a biblioteca `Gradio` usando o código que eles mostram ali, mas podemos carregá-la durante o `preload()` usando:
```js
let gUrl = "https://cdn.jsdelivr.net/npm/@gradio/client/dist/index.min.js";
let module = await import(gUrl);
```

Depois, podemos seguir o resto do código para configurar um cliente:
```js
client = await module.Client.connect("aifeifei798/Image-To-Flux-Prompt");
```

De novo, muitas chamadas de função com `await`... (veja o [tutorial de concorrência](../concurrency/) para relembrar).

{% include p5-editor.html id="5HjsmM_qP" %}

Estamos prontos para chamar a inferência na nossa imagem com:
```js
result = await client.predict("/feifeichat", {
  image: exampleImage, 
});
```

Mas !! o modelo espera um `blob` de imagem (_binary large object_), e não uma imagem do `p5.js`, nem um `dataURL` de imagem como antes.

Para transformar o frame da nossa câmera em um `blob`, podemos chamar o método `toBlob()` do nosso frame, mas, como essa é uma operação que pode demorar, temos que fazer isso de forma assíncrona com uma função _callback_:
```js
mCamera.canvas.toBlob(callModel);
```

(veja o [tutorial de concorrência](../concurrency/) para mais informações sobre funções _callback_)

Onde `callModel()` é a função que será chamada quando o `blob` da imagem estiver pronto:
```js
async function callModel(imgBlob) {
  let result = await client.predict("/feifeichat", {
    image: imgBlob,
  });
  print(result);
}
```

A legenda de fato está dentro de `result.data[0]`. E podemos simplesmente atribuir isso à variável `caption` para vê-la no canvas:

{% include p5-editor.html id="1JE1qkiWd" %}

Este modelo demora um pouco mais para rodar, mas é muito mais preciso e descritivo.

## Mais Modelos!!

Já que temos um bom modelo de descrição de imagens para a nossa câmera, podemos adicionar uma chamada de API para outro modelo e criar uma descrição em áudio do frame da nossa câmera.

Vamos testar o modelo hospedado [aqui](https://huggingface.co/spaces/mrfakename/MeloTTS).

Se formos ao botão _Use via API_ e depois ao exemplo em `JavaScript`, vamos ver como usar a API deste modelo:
```js
const client = await Client.connect("mrfakename/MeloTTS");
const result = await client.predict("/synthesize", {
  text: "Hello!!",
  speaker: "EN-US",
  speed: 1,
  language: "EN",
});
print(result);
```

Bem simples.

Primeiro, vamos importar a biblioteca `Gradio` exatamente como fizemos antes, mas, desta vez, vamos criar dois clientes separados para os nossos modelos:
```js
let gUrl = "https://cdn.jsdelivr.net/npm/@gradio/client/dist/index.min.js";
let module = await import(gUrl);
imgClient = await module.Client.connect("aifeifei798/Image-To-Flux-Prompt");
ttsClient = await module.Client.connect("mrfakename/MeloTTS");
```

Vamos chamar o modelo de text-to-speech assim que tivermos a descrição da imagem em `callImgModel()`:
```js
let imgResult = await imgClient.predict("/feifeichat", {
  image: imgBlob,
});
caption = imgResult.data[0];

let ttsResult = await ttsClient.predict("/synthesize", {
  text: caption,
  speaker: "EN-US",
  speed: 1,
  language: "EN",
});
print(ttsResult);
```

{% include p5-editor.html id="faeo3Hqs_" %}

Se olharmos para `ttsResult`, vamos ver que dentro de `ttsResult.data[0]` recebemos uma url para um arquivo de áudio:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/gradio-result-00.jpg' |relative_url }}">
</div>

Vamos carregar esse áudio em um player:
```js
mPlayer = loadSound(ttsResult.data[0].url, playSound);
```

O parâmetro `playSound` é uma função _callback_ que será chamada quando o arquivo estiver pronto para tocar:
```js
function playSound() {
  mPlayer.play();
}
```

E é isso. Agora temos uma ferramenta de descrição em áudio em tempo real:

{% include p5-editor.html id="bVZR4sZSr" %}


## Um Exemplo de Geração de Imagens

Os modelos de geração de imagens consomem muitos recursos, então é raro encontrar um modelo bem completo que possamos rodar de graça.

Existem alguns modelos de geração de imagens que rodam mais rápido, mas têm menos qualidade ou variedade na saída.

Um desses modelos _Turbo_ está hospedado no Hugging Face, [aqui](https://huggingface.co/spaces/diffusers/unofficial-SDXL-Turbo-i2i-t2i).

Se olharmos a documentação da API, vamos ver um código familiar:
```js
const client = await Client.connect("diffusers/unofficial-SDXL-Turbo-i2i-t2i");
const result = await client.predict("/predict", {
	init_image: exampleImage,
  prompt: "Hello!!",
	strength: 0,
  steps: 1,
  seed: 0,
});

print(result);
```

Vamos adicionar isso a um sketch e ver se conseguimos forçar sorrisos nas imagens que capturamos com a câmera.

Ainda temos uma função `captureFrame()`, que inicia o processo de transformar o frame da nossa câmera em um `blob`:
```js
function captureFrame() {
  mFrame = loadImage(mCamera.canvas.toDataURL());
  mCamera.canvas.toBlob(callImgModel);
}
```

E depois a função `callImgModel()` de fato chama o modelo quando o `blob` está pronto:
```js
let imgResult = await imgClient.predict("/predict", {
  init_image: imgBlob,
  prompt: "Make them smile",
  strength: 0.7,
  steps: 2,
  seed: 0,
});
print(imgResult);
```

{% include p5-editor.html id="-L2D1wbdn" %}

De novo, a parte do resultado que nos interessa está em `imgResult.data[0].url`, então vamos carregá-la na nossa variável `mFrame`:
```js
mFrame = loadImage(imgResult.data[0].url);
```

E ...

{% include p5-editor.html id="mcFK8sjbm" %}

Os resultados podem variar. Este modelo não é super inteligente, nem controlável, mas costuma fazer as pessoas na imagem meio que sorrirem, mesmo que distorça um pouco o rosto delas. Isso faz parte do custo-benefício desses modelos _Turbo_:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/gradio-diffusion-01.jpg' |relative_url }}">
</div>

Adaptar alguns dos parâmetros também pode ajudar:

O `strength` determina o quanto do prompt o modelo de imagem leva em conta, e os `steps` determinam quantas passadas o modelo usa para gerar a imagem final.

```js
prompt: "Make the person smile really big",
strength: 0.5,
steps: 3,
```

ou

```js
prompt: "person smiling really happy",
strength: 0.35,
steps: 2,
```
