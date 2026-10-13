---
title: Síntese de Som
---
## NOTA:
Para usar a biblioteca de som, temos que incluir a [biblioteca p5.sound](https://p5js.org/reference/#/libraries/p5.sound) no arquivo `index.html` do nosso projeto, depois do arquivo do p5.js, assim:

```html
<script src="https://cdn.jsdelivr.net/npm/p5@1.7.0/lib/p5.js"></script>
<script src="https://cdn.jsdelivr.net/npm/p5@1.7.0/lib/addons/p5.sound.js"></script>
```

## Osciladores

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-synthesis-00.jpg' | relative_url }}"/>
</div>

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-synthesis-01.jpg' | relative_url }}"/>
</div>

{% include p5-editor.html id="nXn6zbfSE" %}

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-synthesis-02.jpg' | relative_url }}"/>
</div>

{% include p5-editor.html id="q8TkMheW5" %}

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-synthesis-03.jpg' | relative_url }}"/>
</div>

{% include p5-editor.html id="wmTPsEiRC" %}

## Monosynth

Monosynth faça-você-mesmo a partir de osciladores

{% include p5-editor.html id="CKv3B6So_" %}

## Ataque, Decaimento, Sustentação, Liberação (Attack, Decay, Sustain, Release)

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-synthesis-04.jpg' | relative_url }}"/>
</div>

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-synthesis-05.jpg' | relative_url }}"/>
</div>

{% include p5-editor.html id="52N5KRgX4" %}

## Sequenciador

{% include p5-editor.html id="xVCALCTDo" %}
