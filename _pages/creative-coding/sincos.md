---
title: Seno & Cosseno
---

Na [seção](../cycles/) anterior vimos como definir um ciclo em termos de $$amplitude$$, $$period$$ e $$\mathit{offset}$$:

{% include p5-editor.html id="Qw8RJhauV" %}

Uma pequena revisão:

$$Amplitude$$: a distância que o círculo percorre antes de voltar ao começo.

$$Período$$: quanto tempo leva para um ciclo se completar e recomeçar (pode ser em segundos, mas o número de frames é mais útil ao usar `frameCount`).

$$\mathit{Offset}$$ (deslocamento): posição inicial do círculo.

Além do operador módulo e da onda triangular, outra forma fácil de gerar ciclos é usando as funções trigonométricas [seno e cosseno](https://en.wikipedia.org/wiki/Sine_and_cosine). Isso pode não ficar imediatamente claro na definição da wikipedia: *No contexto de um triângulo retângulo: para um dado ângulo, seu seno é a razão entre o comprimento do cateto oposto a esse ângulo e o comprimento da hipotenusa; e o cosseno é a razão entre o comprimento do cateto adjacente e o da hipotenusa*.

Mas o que importa aqui é lembrar que tanto a função seno quanto a função cosseno se repetem a cada 360º ou $$2\pi$$ radianos.

No caso do seno, conforme o ângulo $$\theta$$ cresce de $$0$$ a $$\frac{\pi}{2}$$, $$sin(\theta)$$ cresce de $$0$$ a $$1$$. Conforme $$\theta$$ cresce de $$\frac{\pi}{2}$$ a $$\pi$$, $$sin(\theta)$$ diminui de $$1$$ a $$0$$. Conforme $$\theta$$ cresce de $$\pi$$ a $$\frac{3\pi}{2}$$, $$sin(\theta)$$ vai de $$0$$ a $$-1$$ e, por fim, conforme $$\theta$$ cresce de $$\frac{3\pi}{2}$$ a $$2\pi$$, $$sin(\theta)$$ aumenta de $$-1$$ a $$0$$. Depois disso, conforme $$\theta$$ continua crescendo, a $$3\pi$$, $$4\pi$$, $$5\pi$$, etc, os valores de $$sin(\theta)$$ vão continuar se repetindo e ficando sempre entre $$[-1 , 1]$$. Em outras palavras: \$$sin(\theta + 2\pi) = sin(\theta)$$

Visualmente:
<div class="scaled-images w75">
  <img src="{{ '/assets/images/creative-coding/sine-00.jpg' | relative_url }}">
</div>

Se procurarmos equações que relacionem as funções seno e cosseno ao movimento cíclico ou [harmônico](https://phys.libretexts.org/Bookshelves/Classical_Mechanics/Classical_Mechanics_(Dourmashkin)/23%3A_Simple_Harmonic_Motion/23.01%3A_Introduction_to_Periodic_Motion#Simple_Harmonic_Motion:_Quantitative), podemos encontrar algo assim:

$$y(t) = A \times sin\left(\frac{2\pi}{P}\times t\right)$$

onde $$A$$ é a amplitude e $$P$$ é o período.

No p5.js isso poderia ser reescrito como:
```js
let x = A * sin(2 * PI / periodFrames * frameCount);
```

onde $$A$$ é a amplitude, $$P$$ é o período em número de frames e `frameCount` é a variável que continua crescendo indefinidamente:

{% include p5-editor.html id="nN5UJPZkB" %}

Hmmm.... isso meio que funciona. Como o intervalo de `sin()` é $$[-1, 1]$$, nosso círculo está oscilando entre $$[-amplitude, amplitude]$$, o que não é exatamente o que queremos. Uma forma fácil de corrigir isso é somar 1 à função `sin()`, para termos um intervalo que vai de $$[0, 2]$$, depois dividir por $$2$$ para reduzir o intervalo a $$[0, 1]$$ e, finalmente, multiplicar pela amplitude:

$$x = \frac{A}{2}\times \left(sin\left(\frac{2\pi}{P}\times frameCount\right) + 1\right)$$

{% include p5-editor.html id="92DpmSO0y" %}

Muito melhor. E essa mesma equação funciona para outros parâmetros, como o raio do círculo. Só precisamos definir uma $$amplitude$$, um $$period$$ e um $$\mathit{offset}$$ que façam sentido para valores de raio:

{% include p5-editor.html id="rlVJhxgnD" %}

Podemos tirar um tempo para brincar com os parâmetros aqui e construir alguma intuição sobre como eles afetam o movimento do círculo. Tente mudar `xPeriodSec`, `rPeriodSec`, `rAmplitude` e `rOffset`.

Se quisermos, podemos facilmente mudar os nomes das variáveis e as equações para fazer o círculo oscilar verticalmente:

{% include p5-editor.html id="TtPK-g8nM" %}

Aqui a $$amplitude$$ é a `height` inteira da tela e não há $$\mathit{offset}$$, então ele começa em $$0$$.

Agora, podemos combinar *movimento harmônico* nas direções `x` e `y` para obter *ciclos elípticos*:

{% include p5-editor.html id="kSxgebmnj" %}

Só precisamos garantir que usamos $$sin()$$ para uma direção e $$cos()$$ para a outra, senão vamos obter apenas movimento em linha reta diagonal.

Se fizermos as funções de `x` e `y` terem períodos diferentes, vamos obter um belo movimento de [Lissajous](https://en.wikipedia.org/wiki/Lissajous_curve):

{% include p5-editor.html id="9P_BKTs_5" %}

Juntando tudo, e usando $$sin()$$ e $$cos()$$ para os parâmetros `x`, `y` e `radius`, nosso sketch acaba ganhando um efeito 3D:

{% include p5-editor.html id="qN37xEOH9" %}
