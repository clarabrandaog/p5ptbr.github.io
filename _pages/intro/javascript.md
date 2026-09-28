---
title: O Navegador e o JavaScript
---
## A Internet

Já que o JavaScript é uma linguagem originalmente projetada para rodar em navegadores pela internet, pode ser útil entender um pouco mais sobre a internet e como ela funciona.

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/internet-00.jpg' | relative_url }}">
</div>

Para simplificar, a Internet é apenas os computadores de outras pessoas. Não existe nuvem nem matrix, apenas um monte de computadores grandes, quentes e bem conectados, chamados servidores, que ficam em galpões e armazenam arquivos que nossos celulares e computadores podem baixar.

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/internet-01.jpg' | relative_url }}">
</div>

O processo é parecido para outros tipos de serviços, mas quando acessamos uma URL com nosso navegador, nosso computador envia uma solicitação para um desses servidores, pedindo um arquivo específico no endereço indicado. Solicitações feitas a URLs como `p5js.org` ou `nyu.edu` estão na verdade pedindo um arquivo chamado `index.html` que fica nos servidores de `p5js.org` (ou `nyu.edu`).

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/request-00.jpg' | relative_url }}">
</div>

## O Navegador

Quando nosso navegador faz uma solicitação correta e autorizada a um servidor, pedindo um arquivo `html`, o servidor responde com um arquivo de texto com código `html`:

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/request-01.jpg' | relative_url }}">
</div>

O código `html` pode se parecer com algo assim:
```html
<html>
  <head>
    <title>My Homepage</title>
    <link href="style.css">
    <script src="sketch.js"></script>
  </head>
  <body>
    Page Content
    <img src="image.gif">
  </body>
</html>
```

Não é muito importante agora entender o `html` em detalhes. Só precisamos saber que é uma linguagem usada principalmente para especificar o conteúdo que nosso navegador deve exibir e como esse conteúdo deve ser organizado na tela.

Boa parte desse conteúdo é texto e links, mas, na maioria das vezes, o arquivo `html` também vai referenciar outros arquivos, como imagens ou vídeos. Quando o navegador encontra essas referências no código `html`, ele faz solicitações adicionais ao servidor, pedindo esses arquivos.

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/request-02.jpg' | relative_url }}">
</div>

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/request-03.jpg' | relative_url }}">
</div>

Outros tipos comuns de arquivos referenciados pelo `html` são os arquivos `css` e `JavaScript`. Eles são especiais porque, diferente dos arquivos de mídia que o navegador só precisa nos mostrar, são arquivos que mudam *como* o navegador exibe o conteúdo e como ele se comporta.
Os arquivos `css` geralmente especificam o estilo das páginas web. Eles dizem ao navegador como formatar o conteúdo do arquivo `html`: quais fontes usar, qual tamanho o texto deve ter, as cores dos diferentes elementos, etc.

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/request-04.jpg' | relative_url }}">
</div>

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/request-05.jpg' | relative_url }}">
</div>

Se uma página web fosse um apartamento, o arquivo `html` definiria onde ficam as paredes, portas e janelas, enquanto o arquivo `css` especificaria seus materiais e cores. Depois que o arquivo `css` é baixado, o navegador precisa passar novamente pelo conteúdo do arquivo `html` e aplicar os estilos especificados.

Os arquivos JavaScript, por sua vez, descrevem como o navegador deve se comportar e o que fazer com o conteúdo do arquivo `html` conforme o usuário interage com ele. Nossa analogia pode estar ficando sem fôlego, mas se páginas web fossem apartamentos, o JavaScript seria a especificação do que deve acontecer quando diferentes interruptores de luz são acionados em um cômodo.

## JavaScript

Nos primeiros dias da internet, antes de o JavaScript ser uma linguagem totalmente desenvolvida e reconhecida por todos os navegadores, os sites eram bastante *estáticos*. Uma vez baixados os arquivos `html` e `css` e estilizado e exibido o conteúdo da página, o trabalho do navegador estava concluído, e tínhamos em nossas telas o equivalente digital de um jornal impresso.

O JavaScript foi gradualmente permitindo que os sites se tornassem mais dinâmicos, possibilitando que o conteúdo e o estilo de uma página web mudassem conforme a interação do usuário.

Depois que um arquivo JavaScript (geralmente um arquivo `.js`) é baixado por uma página, e enquanto o arquivo `html` e o arquivo `css` terminam de definir como exibir o conteúdo da página, o navegador inicia um processo paralelo separado para ler e *interpretar* o conteúdo do arquivo JavaScript.

Esse interpretador (ou motor) de JavaScript é uma parte interna do navegador responsável por percorrer o arquivo JavaScript linha por linha e executar seus comandos. É isso que significa o JavaScript ser uma linguagem *interpretada*: em vez de rodar diretamente no hardware do computador, um programa em JavaScript precisa de outro programa para executá-lo. Isso não é algo exclusivo do JavaScript, mas o diferencia de algumas linguagens de programação, e é algo que devemos ter em mente.

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/JavaScript.jpg' | relative_url }}">
</div>

Hoje em dia, o JavaScript é considerado uma linguagem robusta e de propósito geral, que pode ser usada para criar praticamente qualquer tipo de programa, dentro ou fora de um navegador (mas sempre com um interpretador). Por isso, podemos encontrar muitos recursos em forma de bibliotecas (código reutilizável) e frameworks (código reutilizável e metodologias predefinidas) que estendem a linguagem e facilitam a escrita de certos tipos de programas sem precisar começar do zero.

O [p5js](https://p5js.org/) é um exemplo de biblioteca JavaScript. Ele é focado em creative coding e em facilitar para todos o aprendizado de como criar experiências interativas usando JavaScript.
