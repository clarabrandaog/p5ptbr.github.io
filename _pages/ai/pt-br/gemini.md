---
title: API do Gemini
---

Ok. Vamos olhar para modelos ainda maiores!

Os apps do Hugging Face e do `Gradio` são formas muito boas de experimentar com modelos de código aberto, mas às vezes podemos querer olhar para alguns dos modelos comerciais, como o `chatGPT` ou o `Gemini`.

O processo/código é parecido com o de quando usamos modelos através do `Gradio`, porque vamos precisar fazer requisições de API para servidores:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/gemini-server-00.jpg' |relative_url }}">
</div>

## Gemini

Vamos ver como usar os modelos `Gemini` do Google para descrever imagens.

Vamos começar com o sketch simples de descrição de imagens com `Transformers.js` que tínhamos alguns tutoriais atrás:

{% include p5-editor.html id="ZuwaVgRID" %}

Vamos remover o código do `Transformers.js` e deixar o sketch pronto para um novo modelo:

{% include p5-editor.html id="5oLO0FGaq" %}

Também adicionamos um campo de texto para colar uma "_API key_" do Gemini.

O que é uma _API key_ ? É a forma como as empresas acompanham quem está usando suas APIs e serviços. Neste caso, é como o Google acompanha com que frequência usamos o Gemini e se não estamos passando de nenhum dos limites do plano gratuito.

Para conseguir uma _API key_, precisamos estar logados em uma conta Google e iniciar um projeto no AI Studio [aqui](https://aistudio.google.com/). Clicar no botão _Get API key_ e depois no botão _+ Create API key_ deve ser suficiente para começar um projeto e obter uma chave.

<div class="scaled-images left">
  <img src="{{ 'assets/images/ai/gemini-key-00.jpg' |relative_url }}">
</div>

Temos que guardar essa chave em algum lugar seguro. Ela é como uma senha para entrar nos servidores do Gemini.

Agora podemos ir até a documentação da API do Gemini [aqui](https://ai.google.dev/gemini-api/docs/text-generation#javascript) e ver como adicionar um cliente Gemini ao nosso sketch `p5.js`.

<div class="scaled-images left">
  <img src="{{ 'assets/images/ai/gemini-api-00.jpg' |relative_url }}">
</div>

De novo, vamos ter que adaptar isso para funcionar em um sketch `p5.js`. Não é tão difícil.

Como precisamos de uma API key para criar um objeto `GoogleGenAI`, vamos colocar a maior parte do código do modelo dentro da função `describeFrame()`. Até o código que configura o modelo.

Assim, quando apertarmos o botão _Describe_ pela primeira vez, a nossa variável `gemini` ainda não vai estar instanciada, e vamos carregar a biblioteca, ler o valor da API key da caixa de texto e criar um objeto `GoogleGenAI`:
```js
if (!gemini){
  let module = await import(gUrl);
  gemini = new module.GoogleGenAI({ apiKey: apiText.value });
}
```

Aqui está o código até agora. Igual ao anterior, mas diferente:

{% include p5-editor.html id="g_Z1QQ5yD" %}

Se procurarmos o suficiente na documentação da API, eventualmente vamos encontrar [este trecho de código](https://ai.google.dev/gemini-api/docs/image-understanding#javascript_1) que descreve como enviar imagens para o modelo.

De novo, temos que adaptar isso para o `p5.js`.

A primeira coisa que precisamos fazer é garantir que o `dataURL` da nossa imagem esteja em formato `jpeg` e remover o cabeçalho que especifica que ele está em formato `jpeg`:
```js
let imgUrl = mCamera.canvas
  .toDataURL("image/jpeg")
  .replace("data:image/jpeg;base64,", "");
```

Depois podemos criar um objeto de dados de imagem:
```js
let imgData = {
  data: imgUrl,
  mimeType: "image/jpeg",
};
```

E, finalmente, chamar o modelo com os dados da imagem e um prompt pedindo que ele descreva a imagem:
```js
let response = await gemini.models.generateContent({
  model: "gemini-2.0-flash",
  contents: [
    { text: "Describe this image" },
    { inlineData: imgData }
  ],
});
```

A descrição estará em `response.text`.

Agora podemos rodar este sketch e obter algumas descrições do Gemini. Só precisamos garantir que a nossa API key esteja na caixa de texto:

{% include p5-editor.html id="D0EjbXv30" %}

## Conversando com o Gemini

Conversar com o modelo é ainda mais fácil, em termos de código. Pode ser exatamente igual ao acima, mas não precisamos codificar uma imagem:
```js
let response = await gemini.models.generateContent({
  model: "gemini-2.0-flash",
  contents: [
    { text: "Why is the sky blue?" }
  ],
});
```

Aqui está um sketch com uma caixa de texto extra para digitar perguntas para o modelo:

{% include p5-editor.html id="FHMs5kya3" %}
