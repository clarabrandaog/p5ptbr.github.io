---
title: Vídeos
---
## Vídeo como Imagens

Agora que sabemos tudo o que precisamos saber sobre [imagens](../images/), e os vídeos?

Bem, um vídeo é só um monte de imagens, uma depois da outra, então carregar e processar vídeos deve ser bem parecido.

Neste exemplo carregamos um vídeo de uma URL específica (o diretório do nosso sketch) criando primeiro um elemento html `<video>` e depois escondendo-o. Essa é a forma mais fácil de carregar arquivos de vídeo no p5.js e ter acesso aos seus frames. Só precisamos lembrar de esconder o elemento de vídeo depois de carregarmos o arquivo:

### NOTA: sketches que carregam a câmera não vão funcionar aqui. Eles têm que ser abertos em uma nova janela diretamente pelo seu [LINK](https://editor.p5js.org/tgh-p5-tutorials/sketches/6tdhpcMEH).

{% include p5-editor.html id="6tdhpcMEH" %}

Agora, acessar e manipular os dados de pixel dos frames do vídeo pode ser feito usando exatamente a mesma lógica que usamos para manipular pixels de imagens.

Vamos detectar pixels pretos e torná-los vermelhos. Como estamos tratando todos os pixels da mesma forma, não precisamos nos preocupar com suas posições $$(x, y)$$ e podemos usar apenas um loop `for()`.

Isto obtém os valores de cor R, G e B de cada pixel dentro do nosso loop `for()`:

```js
let r = mVideo.pixels[i + 0];
let g = mVideo.pixels[i + 1];
let b = mVideo.pixels[i + 2];
```

E se todos esses $$3$$ valores estiverem próximos de $$0$$, vamos exagerar o canal vermelho:

```js
if(r < 64 && g < 64 && b < 64) {
  mVideo.pixels[i + 0] += 191;
}
```

{% include p5-editor.html id="JhwwmKyeN" %}

## Vídeo da Câmera

Também podemos obter um stream de vídeo da nossa câmera. O processo é muito parecido. Usamos um elemento html `<capture>` que podemos acessar com código p5.js e depois escondemos o elemento html de fato:

### NOTA: sketches que usam a câmera não vão funcionar aqui. Eles têm que ser abertos em uma nova janela diretamente pelo seu [LINK](https://editor.p5js.org/tgh-p5-tutorials/sketches/5kRthltqb).

{% include p5-editor.html id="5kRthltqb" %}

Depois que temos um objeto capture, basta chamar `image()` com ele para desenhar um frame de vídeo por frame do nosso sketch.

Fácil.

## Processando Vídeos

E, assim como as imagens, o objeto capture tem um array de pixels e funções e variáveis membros semelhantes, então podemos simplesmente copiar e colar parte do código de um dos sketches anteriores neste sketch:

### NOTA: sketches têm que ser abertos em uma nova janela usando seu [LINK DO SKETCH](https://editor.p5js.org/tgh-p5-tutorials/sketches/weGX7Zf4h)

{% include p5-editor.html id="weGX7Zf4h" %}

E temos um exagerador de cores.

É um pouco mais difícil descobrir o canal de cor dominante em um vídeo ou foto, comparado a uma imagem de uma pintura. Há muito ruído, mas quando seguramos marcadores vermelhos, verdes ou azuis contra um fundo preto, podemos ver que está funcionando:

<div class="image-row">
  <div class="img-wrapper">
    <img src = "{{ '/assets/images/creative-coding/camera-00.jpg' | relative_url }}"/>
  </div>
  <div class="img-wrapper">
    <img src = "{{ '/assets/images/creative-coding/camera-01.jpg' | relative_url }}"/>
  </div>
  <div class="img-wrapper">
    <img src = "{{ '/assets/images/creative-coding/camera-02.jpg' | relative_url }}"/>
  </div>
</div>

## Processando Pixels de Vídeo

E, agora, finalmente, vamos copiar nosso código do padrão de círculos do [tutorial de imagens](../images/), mas usar valores de cor de pixel que vêm da câmera para desenhar nossos círculos:

### NOTA: sketches têm que ser abertos em uma nova janela usando seu [LINK DO SKETCH](https://editor.p5js.org/tgh-p5-tutorials/sketches/XJclc3KE0)
{% include p5-editor.html id="XJclc3KE0" %}
