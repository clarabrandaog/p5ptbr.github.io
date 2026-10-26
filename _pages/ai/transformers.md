---
title: Transformers.js
---

A [biblioteca `ml5`](../ml5/) e os projetos do [Teachable Machine](/../teachable-machine/) são as formas mais fáceis de começar a usar modelos de IA nos nossos sketches. Eles são bem documentados e mantidos, e oferecem uma interface fácil para algumas das tarefas de ML mais comuns.

Mas existem outros modelos por aí. Muitos deles.

Muitas arquiteturas diferentes, tarefas diferentes, bibliotecas diferentes....

Podemos ter uma ideia da variedade de modelos de código aberto existentes visitando a seção _models_ do [Hugging Face](https://huggingface.co/models). O Hugging Face é como o GitHub dos modelos de IA. É uma plataforma onde empresas, pesquisadores, estudantes, qualquer pessoa, pode baixar e enviar diferentes modelos de IA.

É bem sobrecarregado e extenso, e não é fácil de navegar.

Uma coisa que ajuda é que podemos focar em modelos que rodam no navegador. Muitos dos modelos do Hugging Face exigem computadores enormes e/ou GPUs enormes para rodar. A maioria deles é distribuída para ser usada com bibliotecas escritas em `Python`, `C` ou `Lua`, mas há bastante modelos que foram portados para `JavaScript` usando uma biblioteca chamada [`Transformers.js`](https://huggingface.co/docs/transformers.js/en/index).

## Transformers.js

Esta é uma biblioteca escrita e mantida pelas mesmas pessoas que escreveram a biblioteca [transformers](https://huggingface.co/docs/transformers/en/index), muito usada em `Python`. Ela nos permite baixar e rodar modelos hospedados no Hugging Face com uma quantidade extremamente pequena de código.

Podemos procurar bibliotecas `Transformers.js` clicando na aba _Libraries_ e selecionando o ícone amarelo de JS (ou seguindo [este link](https://huggingface.co/models?library=transformers.js&sort=downloads)).

Existem mais de $$1000$$ modelos disponíveis !

Vamos começar experimentando com um modelo de descrição de imagens, encontrado [aqui](https://huggingface.co/Xenova/vit-gpt2-image-captioning).

A documentação de alguns desses modelos é bem escassa, mas o que temos que fazer é criar um objeto `pipeline` no nosso sketch e depois processar imagens com esse objeto `pipeline`. Algo assim:

```js
model = pipeline("image-to-text", "Xenova/vit-gpt2-image-captioning");
captions = model(image);
```

Fácil. Mas vamos ver como configurar isso.

## Configuração

A primeira coisa que precisamos fazer é carregar a biblioteca `Transformers.js` e ter acesso ao construtor do objeto `pipeline`.

Por causa de escolhas feitas pelos desenvolvedores da biblioteca `Transformers.js`, não podemos simplesmente carregá-la a partir de um arquivo `.js` no nosso `index.html`, e sim carregá-la dinamicamente durante a parte `preload()` do nosso sketch. Podemos fazer isso com:

```js
const module = await import("https://cdn.jsdelivr.net/npm/@xenova/transformers");
```

E então o construtor do objeto `pipeline` vai estar disponível como `module.pipeline`. Perfeito.

Agora só precisamos instanciar o modelo com:

```js
model = await pipeline("image-to-text", "Xenova/vit-gpt2-image-captioning");
```

Todas essas palavras-chave `await` ... Ainda bem que vimos elas no nosso [tutorial de concorrência](../concurrency/).

{% include p5-editor.html id="z_fQzI1DG" %}

Temos o nosso modelo configurado, só falta começar a usá-lo.

## Capturando Imagens

Vamos fazer o nosso sketch descrever o que vê pela webcam.

Embora esses modelos do `Transformers.js` sejam feitos para rodar no navegador com `JavaScript`, eles nem sempre são tão otimizados quanto os modelos do `ml5`, então não queremos rodar o nosso modelo de descrição de imagens em cada frame do nosso stream de vídeo.

Vamos criar um botão que captura um frame do stream de vídeo como imagem, e vamos usar essa imagem no nosso modelo. Assim podemos controlar quais imagens são descritas pelo modelo.

Vamos adicionar um stream de vídeo ao nosso sketch com:
```js
mCamera = createCapture(VIDEO, { flipped: true }, setupButton);
```

Isso deve parecer familiar. Desta vez estamos usando uma [função callback](../concurrency/) chamada `setupButton` para criar e posicionar um botão que podemos usar para disparar o processo de extração e descrição da imagem.

Por enquanto, quando clicado, esse botão deve apenas capturar um frame do stream de vídeo e fazer um pouco de pré-processamento para deixá-lo pronto para o modelo. Esta função pega o frame atual da nossa câmera e o transforma em um formato que o modelo entende.
```js
mCamera.canvas.toDataURL();
```

Podemos exibir isso transformando-o de volta em uma imagem do `p5.js`:
```js
mFrame = loadImage(mCamera.canvas.toDataURL());
```

{% include p5-editor.html id="KM8aYkAO-" %}

## Inferência

Ok. Estamos prontos para ter a nossa imagem descrita.

Só precisamos chamar o modelo na imagem e depois esperar o resultado.
```js
let result = await model(mCamera.canvas.toDataURL());
print(result);
```

Isso vai nos ajudar a entender o formato do resultado retornado pelo modelo. Se olharmos para `result`, vamos ver que o que realmente queremos pegar é o texto em `result[0].generated_text`. Vamos colocar isso em uma variável e exibi-lo por cima da imagem capturada:

{% include p5-editor.html id="ZuwaVgRID" %}

## Mais Modelos

Vamos explorar os nossos superpoderes recém-desenvolvidos de usar modelos do Hugging Face e criar um sketch que gera histórias automaticamente com base no conteúdo da nossa webcam. Também vamos passar a nossa história por um modelo de análise de sentimento, para ver que tipo de histórias o nosso sketch está gerando e para dar um título à história.

Precisamos adicionar alguns modelos ao nosso sketch existente. O primeiro, [llama2-stories110M](https://huggingface.co/Xenova/llama2.c-stories110M), é o modelo que gera histórias, e o segundo, [roberta-emotions](https://huggingface.co/thiagohersan/roberta-base-go_emotions), é o modelo que vai classificar a nossa história em um entre $$28$$ sentimentos.

Vamos começar instanciando os nossos novos modelos:
```js
imgModel = await module.pipeline("image-to-text", "Xenova/vit-gpt2-image-captioning");
strModel = await module.pipeline("text-generation", "Xenova/llama2.c-stories110M");
txtModel = await module.pipeline("sentiment-analysis", "thiagohersan/roberta-base-go_emotions");
```
Demora um pouco mais para começar, mas isso porque agora temos $$3$$ modelos para carregar.

{% include p5-editor.html id="-e2pGdczT" %}

Só precisamos criar variáveis para guardar os resultados do nosso modelo de histórias e do nosso modelo de análise de sentimento.

Primeiro, vamos ver o que eles retornam:
```js
let strResult = await strModel("Once upon a time, there was ");
print(strResult);

let txtResult = await txtModel("Once upon a time, there was an ogre");
print(txtResult);
```

{% include p5-editor.html id="F4FMKgiuI" %}

Ok, podemos simplesmente pegar `strResult[0].generated_text` e `txtResult[0].label`. Vamos guardá-los nas variáveis corretas e exibi-los no canvas quando estiverem prontos.

Também podemos aumentar o tamanho da nossa história adicionando um objeto de opções com uma chave `max_new_tokens` à nossa chamada de inferência:
```js
let strResult = await strModel(caption, { max_new_tokens: 256 });
```

{% include p5-editor.html id="qTC_hvDVF" %}

## Modelos de Imagem

Estamos prontos para sair e explorar o vasto, maravilhoso e, muitas vezes, mal documentado mundo dos modelos transformers de imagem do Hugging Face.

Uma coisa para ter em mente: os modelos que recebem _imagens_ como entrada não funcionam com objetos de imagem do `p5.js`, e sim com a sua representação em `dataUrl`. Podemos obter o `dataUrl` de imagens e streams de vídeo do `p5.js` usando:
```js
mCamera.canvas.toDataURL();
mImage.canvas.toDataURL();
```

## Modelos de Áudio

Modelos de áudio são um pouco mais complicados de usar. Isso se deve a duas razões: primeiro, o `p5.js` tem um processo muito complicado e pouco intuitivo para gravar áudio do microfone e, segundo, os modelos de áudio exigem que as entradas tenham formatos de áudio bem específicos, que o `p5.js` e o `JavaScript` nem sempre suportam diretamente.

Vamos dar uma olhada em um modelo de áudio chamado [whisper](https://huggingface.co/Xenova/whisper-tiny.en), que transcreve áudio.

Vamos começar configurando a gravação de áudio. Precisamos instanciar três objetos separados para isso: um stream de microfone, um gravador de som e um arquivo de som:
```js
mMic = new p5.AudioIn();
mRecSound = new p5.SoundFile();
mRecorder = new p5.SoundRecorder();
```

Depois, vamos precisar de um botão que dispare o código a seguir, para inicializar o motor de áudio do navegador, habilitar o stream do microfone e conectá-lo ao gravador:
```js
mMic.start();
mRecorder.setInput(mMic);
userStartAudio();
```

Agora, quando apertamos o botão de gravar, é só começar a gravar:
```js
mRecorder.record(mRecSound, 100, captionAudio);
```

O parâmetro `captionAudio` é uma [função callback](../concurrency/) que será chamada quando pararmos a gravação, o que podemos fazer chamando:
```js
mRecorder.stop();
```

A função `captionAudio()` eventualmente vai ter que ter algum código para transcrever o nosso áudio.

{% include p5-editor.html id="2DXn3eJYV" %}

Vamos adicionar o nosso modelo ao sketch. É parecido com a forma como instanciamos os modelos de imagem, mas com um objeto de opções diferente, que especifica o tipo de dado das amostras que serão analisadas:
```js
sttModel = await module.pipeline(
  "automatic-speech-recognition",
  "Xenova/whisper-tiny.en",
  { dtype: "q8" }
);
```

Quando paramos a gravação, devemos conseguir simplesmente chamar o modelo nas nossas amostras gravadas:
```js
let sttResult = await sttModel(mRecSound.buffer.getChannelData(0));
print(sttResult);
```

{% include p5-editor.html id="KkzenTAU5" %}

Mas, se olharmos para `sttResult.text`, vamos ver que estamos recebendo lixo.

A função `mRecSound.buffer.getChannelData(0)` definitivamente retorna as nossas amostras gravadas, mas, se imprimirmos `mRecSound.buffer.sampleRate`, vamos ver que o nosso navegador grava áudio a $$44\text{,}100$$ ou $$48\text{,}000$$ amostras por segundo, enquanto o nosso modelo quer analisar áudio gravado a $$16\text{,}000$$ amostras por segundo.

Reamostrar áudio em `JavaScript` não é fácil nem divertido. Felizmente, há uma função chamada `resample()` incluída neste sketch que faz exatamente isso. Ela recebe $$3$$ argumentos: a lista de amostras, a taxa de amostragem original e a taxa de amostragem desejada. Eventualmente (de forma assíncrona), ela retorna uma versão reamostrada do áudio, pronta para ser analisada e transcrita pelo nosso modelo:

```js
let samples16k = await resample(
  mRecSound.buffer.getChannelData(0),
  mRecSound.buffer.sampleRate,
  16_000
);
```

Usá-la antes de chamar o nosso modelo resolve o problema da taxa de amostragem e agora conseguimos obter transcrições de áudio limpas:

{% include p5-editor.html id="1VNjI7XFs" %}
