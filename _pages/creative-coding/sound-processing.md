---
title: Processamento de Som
---
## NOTA:
Para usar a biblioteca de som, temos que incluir a [biblioteca p5.sound](https://p5js.org/reference/#/libraries/p5.sound) no arquivo `index.html` do nosso projeto, depois do arquivo do p5.js, assim:

```html
<script src="https://cdn.jsdelivr.net/npm/p5@1.7.0/lib/p5.js"></script>
<script src="https://cdn.jsdelivr.net/npm/p5@1.7.0/lib/addons/p5.sound.js"></script>
```

## Entradas & Saídas

Já vimos como usar a biblioteca [p5.sound](https://p5js.org/reference/#/libraries/p5.sound) para [tocar sons pré-gravados](../sound-files/) de arquivos. Agora, vamos ver como usar outras partes da biblioteca para manipular áudio gravado ou ao vivo.

A biblioteca p5.sound, junto com muitos outros toolkits de processamento de áudio para creative coding, foi projetada para imitar de certa forma uma configuração física de processamento de áudio. Os objetos têm portas de entrada e saída que recebem/enviam o mesmo tipo de informação (amostras de áudio digital); cada módulo faz algum tipo de processamento ou manipulação nas suas entradas antes de enviá-las às suas saídas; e os módulos podem ser facilmente encadeados para criar efeitos sonoros mais complexos.

Existem objetos especiais que nos permitem capturar áudio ao vivo das portas de entrada do nosso computador (microfone, line-in), e outros objetos que nos permitem enviar nosso áudio processado para as saídas do computador (alto-falantes, line-out).

Também existem objetos de "_exibição_" que não emitem nenhum sinal de som, mas são usados para obter informações específicas sobre nossos sinais de áudio, que podemos usar para analisar nosso áudio visualmente.

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-00.jpg' | relative_url }}"/>
</div>

As saídas desses objetos/módulos podem ser roteadas para várias entradas, e alguns módulos podem receber várias entradas:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-01.jpg' | relative_url }}"/>
</div>

Vamos começar olhando um dos módulos mais simples.

## Amplitude

[Amplitude](https://p5js.org/reference/#/p5.Amplitude) é um dos módulos de "_exibição_" que não emitem áudio, mas podem ser usados para mostrar informações sobre o nosso sinal.

Neste caso, o módulo Amplitude vai nos dar a amplitude de um sinal de áudio (quão alto ele está), como um número entre $$0$$ e $$1$$:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-02.jpg' | relative_url }}"/>
</div>

Por padrão, qualquer objeto [`p5.SoundFile`](https://p5js.org/reference/#/p5.SoundFile) que criamos envia sua saída para o módulo/objeto [`p5.soundOut`](https://p5js.org/reference/#/p5/soundOut), que é a nossa saída final: o sinal que vai para o nosso alto-falante.

E, também por padrão, o módulo Amplitude recebe sua entrada desse mesmo objeto [`p5.soundOut`](https://p5js.org/reference/#/p5/soundOut).

Então, tecnicamente, instanciar esses dois objetos assim já seria suficiente para tê-los conectados corretamente:

```js
mSound = loadSound("./sound-file.mp3");
mAmp = new p5.Amplitude();
```

Mas não é má ideia praticar como fazer essas conexões nós mesmos. Isso evita comportamentos inesperados e depuração desnecessária quando nossos pipelines de processamento de áudio começarem a ficar mais complexos.

Podemos usar o código a seguir para rerrotear manualmente o sinal do nosso objeto `p5.SoundFile` tanto para o objeto `p5.soundOut` quanto para um módulo `p5.Amplitude`:

```js
mSound.disconnect();
mSound.connect(p5.soundOut);
mSound.connect(mAmp);
```

Essas são exatamente as conexões mostradas no diagrama acima.

Nosso objeto `p5.Amplitude` agora pode ser usado a cada iteração da nossa função `draw()` para obter a amplitude do som e exibi-la visualmente usando elipses:

{% include p5-editor.html id="-DxgXbZeG" %}

## Filtros

Agora que podemos visualizar nosso som, vamos adicionar um módulo de processamento de verdade para manipular a qualidade e as características do nosso áudio:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-03.jpg' | relative_url }}"/>
</div>

O módulo [`p5.Filter`](https://p5js.org/reference/#/p5.Filter) nos permite filtrar nossos sinais de áudio com base em frequências.

Alguns tipos comuns de filtro que podemos implementar com esse módulo são: `lowpass`, `highpass`, `bandpass` e `notch`.

Como o nome sugere, o filtro `lowpass` deixa passar as frequências baixas (graves) enquanto bloqueia as frequências altas:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-04.jpg' | relative_url }}"/>
</div>

O `highpass` age de forma oposta, filtrando os componentes de baixa frequência do som e deixando as frequências altas passarem para a saída:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-05.jpg' | relative_url }}"/>
</div>

O filtro `notch` é usado para atenuar uma faixa específica de frequências do sinal de áudio, enquanto o `bandpass` faz o oposto e só deixa uma faixa específica de frequências passar para a sua saída:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-06.jpg' | relative_url }}"/>
  <img src="{{ '/assets/images/creative-coding/sound-processing-07.jpg' | relative_url }}"/>
</div>

A frequência $$f$$, às vezes chamada de frequência de corte, frequência de canto ou frequência de quebra, é um parâmetro do objeto de filtro e determina quais frequências passam e quais são filtradas. Os filtros `bandpass` e `notch` também têm outro parâmetro para controlar sua largura de banda, ou o quão largas são suas faixas de corte ou de passagem.

Com isso em mente, podemos instanciar um filtro e implementar o seguinte sistema:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-03.jpg' | relative_url }}"/>
</div>

Com algo assim:

```js
mSound = loadSound("./sound-file.mp3");
mFilter = new p5.Filter("bandpass");
mAmp = new p5.Amplitude();

mSound.disconnect();
mFilter.disconnect();

mSound.connect(mFilter);
mFilter.connect(p5.soundOut);
mFilter.connect(mAmp);
```

E usar `mouseX` para escolher a frequência central $$f$$ do filtro:

{% include p5-editor.html id="iM1QxgAch" %}

## FFT

Com certeza conseguimos ouvir as diferenças no som conforme movemos o mouse e mudamos a frequência de corte do filtro, mas vamos ver um módulo que também nos permitirá visualizar o efeito do filtro.

A classe [`p5.FFT`](https://p5js.org/reference/#/p5.FFT) implementa o algoritmo da Transformada Rápida de Fourier (Fast Fourier Transform), que pode ser usado para separar nosso sinal de áudio em componentes individuais de frequência.

Podemos substituir o módulo `Amplitude` do último exemplo pelo módulo `FFT`:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-08.jpg' | relative_url }}"/>
</div>

E agora, quando chamamos [`FFT.analyze()`](https://p5js.org/reference/#/p5.FFT/analyze), esse módulo calcula um array de $$1024$$ valores, onde cada valor corresponde a quanto de uma determinada frequência audível estava presente no sinal de áudio original.

Então, o primeiro valor do array corresponde a frequências entre $$0$$ e $$20$$ Hz, o segundo valor é para frequências entre $$20$$ e $$40$$ Hz, e assim por diante, até o 1024º valor, que corresponde a frequências maiores que $$22,000$$ Hz ou $$22$$ kHz.

Se o valor em uma determinada posição for $$0$$, significa que o sinal de áudio original não tinha som naquela frequência. Por outro lado, se for $$255$$, significa que o sinal original tinha um som muito forte com aquela frequência.

O objeto `p5.FFT` também tem uma função [`getEnergy()`](https://p5js.org/reference/#/p5.FFT/getEnergy) que retorna a quantidade de uma frequência específica ou de uma faixa de frequências presente no sinal de áudio. Ela também pode ser chamada com uma de cinco strings de faixa predefinidas, para obter a quantidade de energia nas faixas de frequência `bass`, `lowMid`, `mid`, `highMid` e `treble`.

Sabendo disso, podemos usar o objeto `p5.FFT` e as funções `FFT.analyze()` e `getEnergy()` para visualizar os efeitos do filtro do exemplo anterior:

{% include p5-editor.html id="5qBkXuygI" %}

Em vez de desenhar apenas um círculo, agora desenhamos cinco, um para cada uma das faixas de frequência predefinidas, e conforme movemos o mouse da esquerda para a direita vamos ver o movimento ir dos círculos de baixo para os de cima, que correspondem às faixas de frequência mais altas.

Vamos experimentar outro módulo/objeto de efeito

## Delay

O módulo [`p5.Delay`](https://p5js.org/reference/#/p5.Delay) adiciona uma espécie de efeito de eco a qualquer som, reproduzindo o sinal de áudio novamente depois de alguns milissegundos e depois reproduzindo de novo, atrasado por mais alguns milissegundos, e assim por diante... para criar um rastro de som, em que cada cópia atrasada também é atenuada (volume mais baixo) em uma certa quantidade.

Podemos simplesmente substituir o módulo `p5.Filter` dos exemplos acima por um objeto `p5.Delay` assim:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-09.jpg' | relative_url }}"/>
</div>

E inicializar o objeto com um [`delayTime()`](https://p5js.org/reference/#/p5.Delay/delayTime) apropriado:

```js
mDelay = new p5.Delay();
mDelay.delayTime(0.15);
```

Mas, não importa como ajustemos esse parâmetro, o sinal resultante simplesmente não vai soar como um eco _natural_. Experimente:

{% include p5-editor.html id="564xTeOZl" %}

Isso acontece porque tudo o que estamos ouvindo é o som "molhado" (wet), o som com o delay, enquanto em uma situação do mundo real qualquer tipo de eco é uma combinação do som atrasado (sinal "wet") com o som original (sinal "dry", ou seco).

Para simular isso, temos que conectar nossos módulos de processamento de som assim:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-10.jpg' | relative_url }}"/>
</div>

Onde a saída recebe uma mistura do som original mais o som atrasado:

```js
mSound.amp(0.7);
mDelay.amp(0.3);

mSound.connect(mDelay);
mSound.connect(p5.soundOut);
mDelay.connect(p5.soundOut);
```

{% include p5-editor.html id="F6AAOMTLS" %}

Agora podemos brincar com os parâmetros para ajustar o delay e vamos ter um pouco mais de controle sobre como o sinal final vai soar.

## Reverb

Outro módulo muito parecido com o [`p5.Delay`](https://p5js.org/reference/#/p5.Delay), que é conectado da mesma forma e usado de maneira semelhante, é o efeito [`p5.Reverb`](https://p5js.org/reference/#/p5.Reverb).

O reverb também adiciona eco a um som, mas em vez de adicionar uma versão atrasada do sinal, como se viesse do mesmo local da fonte original, o reverb é como adicionar um monte de versões atrasadas da fonte original, todas vindo de locais diferentes. Isso tem o efeito geral de fazer o som parecer que está ocorrendo em um espaço físico com características acústicas particulares.

Este vídeo explica e mostra a diferença entre delay e reverb em sons vocais e instrumentais:

<div class="youtube-container s16x9">
  <iframe width="800" height="450" src="https://www.youtube.com/embed/-jPPJEHMepA?si=uLK5BJ-KszAwW8G_" frameborder="0" allowfullscreen></iframe>
</div>

No p5js, se conectarmos para ouvir apenas o reverb, assim:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-11.jpg' | relative_url }}"/>
</div>

o sinal _wet_ vai soar assim:

{% include p5-editor.html id="ojgRZ3kNN" %}

Mas, como no delay, se conectarmos assim, misturando os sinais _wet_ e _dry_:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-12.jpg' | relative_url }}"/>
</div>

e ajustarmos alguns dos parâmetros, podemos fazer os sinais originais soarem como se viessem de uma sala grande e vazia:

{% include p5-editor.html id="NQ1N_LQCO" %}

## Forçando o Delay

Agora que sabemos como os módulos [`p5.Delay`](https://p5js.org/reference/#/p5.Delay) e [`p5.Reverb`](https://p5js.org/reference/#/p5.Reverb) funcionam, talvez possamos começar a usá-los de maneiras inesperadas.

O que acontece se encadearmos vários módulos de delay em sequência? Ou misturarmos delays e reverbs?

Vamos começar construindo o seguinte pipeline de processamento:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-13.jpg' | relative_url }}"/>
</div>

Vamos usar um loop for para criar os módulos e colocá-los em um array, e depois podemos conectar os casos extremos:

```js
for (let i = 0; i < NUM_DELAYS; i++) {
  let mDelay = new p5.Delay();
  mDelay.delayTime(DELAY_TIME);

  // conecta a saída do delay anterior à entrada
  mDelays[i - 1].connect(mDelay);

  mDelays.push(mDelay);
}

// conecta o módulo do arquivo de áudio ao primeiro delay
mSound.connect(mDelays[0]);

// conecta o último delay à saída
mDelays[mDelays.length - 1].connect(p5.soundOut);
```

E o sketch completo, com alguns parâmetros ajustáveis:

{% include p5-editor.html id="Sn5kPD97Y" %}

## Entrada de Áudio

Podemos facilmente mudar o sketch acima para usar um microfone em vez de um arquivo pré-gravado, se quisermos aplicar o efeito à nossa própria voz em tempo real:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-14.jpg' | relative_url }}"/>
</div>

Basta adicionar um objeto [`p5.AudioIn`](https://p5js.org/reference/#/p5.AudioIn) para obter o som do microfone e uma variável booleana `isMicOn` para ligar e desligar o microfone:

### NOTA: sketches que usam o microfone não vão funcionar aqui. Eles têm que ser abertos em uma nova janela diretamente pelo seu [LINK](https://editor.p5js.org/thiagohersan/sketches/94vJFO1dT_).

{% include p5-editor.html id="8Ic-7bIUq" %}

## Efeito Completo

O exemplo final consiste em montar algo que combina muito do que vimos até agora.

A ideia é adicionar um delay perceptível nas frequências mais baixas, para adicionar artificialmente mais batidas de bumbo, e ao mesmo tempo adicionar alguns efeitos de reverb ao chimbal (hi-hat), para que soe como um instrumento extra.

Vamos trabalhar em direção a este pipeline, um efeito por vez:

<div class="scaled-images left">
  <img src="{{ '/assets/images/creative-coding/sound-processing-15.jpg' | relative_url }}"/>
</div>

Primeiro, o caminho das baixas frequências:

```js
mSound.connect(mFilterLow);
mFilterLow.connect(mDelay);
mDelay.connect(p5.soundOut);
```

Com um botão de alternância, para verificar o efeito:

{% include p5-editor.html id="JLZd2lAgV" %}

Agora, o caminho das altas frequências:

```js
mSound.connect(mFilterHigh);
mFilterHigh.connect(mReverb);
mReverb.connect(p5.soundOut);
```

Com um botão de alternância:
{% include p5-editor.html id="7he8PvvNq" %}

E, juntando tudo, com um botão de alternância:
{% include p5-editor.html id="azmXfYrYo" %}
