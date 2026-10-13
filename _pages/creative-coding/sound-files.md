---
title: Arquivos de Som
---
## NOTA:
Para usar a biblioteca de som, temos que incluir a [biblioteca p5.sound](https://p5js.org/reference/#/libraries/p5.sound) no arquivo `index.html` do nosso projeto, depois do arquivo do p5.js, assim:

```html
<script src="https://cdn.jsdelivr.net/npm/p5@2.3.1/lib/p5.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/p5.sound@0.4.1/dist/p5.sound.min.js"></script>
```

## Tocando Sons
Vamos usar alguns elementos html em um sketch para criar um player de som simples.

Primeiro, vamos carregar um arquivo de áudio usando a função [`loadSound()`](https://p5js.org/reference/#/p5/loadSound) e testá-lo. Isso é parecido com a forma como carregamos imagens, arquivos de texto, conjuntos de dados, etc, usando a função `preload()`.

A função `loadSound()` retorna um objeto [`SoundFile`](https://p5js.org/reference/#/p5.SoundFile), e podemos usar sua função [`play()`](https://p5js.org/reference/#/p5.SoundFile/play) para tocar a música sempre que clicarmos o mouse:

{% include p5-editor.html id="kZLiBslTX" %}

O que acontece se clicarmos o mouse várias vezes? Parece que o objeto `song` começa a tocar o arquivo várias vezes, uma por cima da outra.

Existem duas formas de resolver isso.

Primeiro, podemos especificar o [`playMode()`](https://p5js.org/reference/#/p5.SoundFile/playMode) do arquivo como `restart` ou `untilDone`, e isso fará com que ele toque apenas uma instância do arquivo, recomeçando a música ou não fazendo nada até que ela toque até o fim:

{% include p5-editor.html id="U8GL6qdUD" %}

A outra forma é verificar primeiro se o objeto já está tocando o arquivo e só chamar `play()` se não estiver. Também podemos usar [`isPlaying()`](https://p5js.org/reference/#/p5.SoundFile/isPlaying) para mudar a cor de fundo como indicação do estado de reprodução:

{% include p5-editor.html id="hABEaYEA1" %}

## Player de Som
Agora que sabemos como tocar um arquivo de som, vamos organizar nosso player de som.

Vamos ter $$3$$ botões: voltar ao início, tocar/pausar e parar. Nossa música também tem $$3$$ estados em que pode estar: `PLAYING` (tocando), `PAUSED` (pausada) e `STOPPED` (parada). O botão de voltar ao início sempre reinicia a posição de reprodução da música, mas não muda o estado em que ela está. Os outros botões fazem a música mudar de estado entre `PLAYING`, `PAUSED` e `STOPPED`.

Este diagrama de estados especifica as mudanças relacionadas a $$2$$ dos nossos botões:

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/sound-files-00.jpg' | relative_url }}"/>
</div>

Podemos *ler* o diagrama de estados começando pelo seu estado inicial e considerando suas transições.

Este diagrama de estados começa em um estado chamado `STOPPED` e transiciona para o estado `PLAYING` sempre que o botão `play` é pressionado. Se o botão `stop` for pressionado, ele permanece no estado `STOPPED`.

Quando está no estado `PLAYING`, pressionar o botão `stop` muda o estado para `STOPPED` e pressionar o botão `pause` muda para `PAUSED`.

Da mesma forma, no estado `PAUSED`, pressionar o botão `stop` muda o estado para `STOPPED` e pressionar o botão `play` muda para `PLAYING`.

Agora podemos adicionar o que acontece quando o botão `back` é pressionado:

<div class="scaled-images left w75">
  <img src = "{{ '/assets/images/creative-coding/sound-files-01.jpg' | relative_url }}"/>
</div>

O que esse diagrama está dizendo é que, não importa em que estado estejamos, se o botão `back` for pressionado, devemos reiniciar a música ao seu começo e permanecer no estado em que estamos.

Vamos escrever um pseudocódigo:

{% include p5-editor.html id="r5r5Y3QRq" %}

Vamos ter que verificar algumas coisas que não ficam tão claras na documentação, como se a função [`stop()`](https://p5js.org/reference/#/p5.SoundFile/stop) também reinicia a posição da faixa, e o que acontece se chamarmos [`jump(0)`](https://p5js.org/reference/#/p5.SoundFile/jump) em um arquivo que não está tocando.

Vamos usar este código para verificar:

{% include p5-editor.html id="SLCmRcdbd" %}

Algumas coisas a notar sobre o código:

1. A função [`jump()`](https://p5js.org/reference/#/p5.SoundFile/jump) só funciona se o arquivo estiver tocando, então tivemos que usar uma variável global extra para registrar se o arquivo deve recomeçar depois de uma pausa.
2. Os botões provavelmente poderiam estar em uma classe, já que têm o mesmo tamanho e estilo e agora há muito código repetido.

Vamos criar uma [classe](../../coding/classes/) para os nossos botões para encapsular seu estilo e comportamento:

{% include p5-editor.html id="H4SNKePvH" %}

Agora, vamos adicionar um quarto botão para pular para a próxima música e detectar cliques duplos no botão de voltar para pular para a música anterior:

{% include p5-editor.html id="gBeE2zqYi" %}

De novo, há alguma lógica extra que precisamos usar para definir `shouldRestart` toda vez que mudamos de música ou de estado enquanto está pausado.

## Vendo o Som

Mais uma última coisa, que esperamos que seja divertida.

Vamos visualizar as músicas enquanto elas tocam.

O objeto `SoundFile` do p5.js parece não ter nenhuma função ou variável que nos diga os valores exatos das amostras que estão tocando no momento. O mais próximo que ele tem é uma função [`getPeaks()`](https://p5js.org/reference/#/p5.SoundFile/getPeaks) que nos dá uma versão simplificada e reamostrada dos valores do arquivo de som inteiro.

O tamanho padrão do array retornado por `getPeaks()` é $$5$$ vezes a largura (`width`) do canvas, então, não importa quão longa seja a música, o tamanho do array de picos será sempre o mesmo. Os números nesse array representam amostras, que são como os pixels dos arquivos de som. Neste caso, elas foram normalizadas para o intervalo $$[-1, 1]$$, em que números com valores absolutos maiores representam amostras mais altas.

Podemos usar isso para mapear os valores de amostra da música atual de $$[-1, 1]$$ para valores entre $$[-width, width]$$ que podemos usar para desenhar algumas elipses:
```js
function toWidth(_peakVal) {
  return map(abs(_peakVal), 0, 1, 0, width);
}

waveDiameters = song.getPeaks().map(toWidth);
```

Como `song.getPeaks()` retorna um array de amostras, podemos usar a função especial de arrays do JavaScript [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) para transformar cada amostra do array. A função `toWidth()` que usamos para fazer o mapeamento de cada valor do array usa internamente a função [`map()`](https://p5js.org/reference/#/p5/map) do p5.js. Ambas se chamam `map()`, ambas transformam números de um intervalo de valores em outro, mas são funções diferentes.

Depois que temos esse array de diâmetros de elipses, só precisamos saber qual deles desenhar em cada frame. Esse é o código aqui no `draw()`:
```js
let tPos = song.currentTime() / song.duration();
let dIndexDelay = floor(tPos * waveDiameters.length + DELAY);
let dIndex = constrain(dIndexDelay, 0, waveDiameters.length - 1);
let diam = waveDiameters[dIndex];
ellipse(width / 2, height / 2, diam, diam);
```

A variável `tPos` usa as funções [`currentTime()`](https://p5js.org/reference/#/p5.SoundFile/currentTime) e [`duration()`](https://p5js.org/reference/#/p5.SoundFile/duration) da música para determinar quão adiante estamos na música que está tocando. Seu valor sempre estará entre $$[0, 1]$$ e representa a porcentagem da faixa que já foi tocada.

A variável `dIndexDelay` transforma o valor de `tPos` em um valor de índice, multiplicando a porcentagem pelo tamanho do array de diâmetros. `DELAY` é uma constante que adicionamos para compensar atrasos entre a tela e os alto-falantes do computador. Podemos brincar com essa variável no começo do arquivo para deslocar qual índice usamos em um dado frame e compensar atrasos ao usar fones ou caixas de som bluetooth.

Esse valor então passa por `floor()` e `constrain()` para garantir que estejamos sempre usando um número inteiro (sem decimais) que esteja dentro dos limites do array.

Depois disso, usamos esse valor para indexar nossos arrays de diâmetros e desenhar uma elipse cujo tamanho é proporcional ao volume da amostra que está tocando:

{% include p5-editor.html id="DnPLOOgea" %}

Experimente nas duas faixas, e lembre-se de ajustar o valor de `DELAY` para obter uma sincronização melhor se estiver usando caixas de som/fones bluetooth ou se o navegador estiver sobrecarregado.
