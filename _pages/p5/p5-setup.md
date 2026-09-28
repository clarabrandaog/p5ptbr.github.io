---
title: Configurando o p5.js
---
Vimos anteriormente como configurar nosso [ambiente de desenvolvimento local](../../intro/ide/).

Agora, vamos ver como iniciar um projeto p5.js.

## Arquivos

A forma mais básica de começar um projeto é simplesmente criar um diretório vazio em algum lugar do nosso computador:

{% include video.html url="p5/setup-00.webm" width="66" %}

Em seguida, podemos abrir esse diretório no VSCode e criar dois arquivos vazios dentro dele: `index.html` e `sketch.js`.

{% include video.html url="p5/setup-01.webm" %}

### html

Vamos começar pelo arquivo `index.html`, já que este é o arquivo que é carregado primeiro quando acessamos nosso projeto em um navegador. Esse arquivo é responsável por carregar alguns outros arquivos com código JavaScript e por configurar alguns elementos `html` básicos onde os resultados do nosso código JavaScript podem ser desenhados.

[ESTE](https://github.com/IDMp5/IDMp5.github.io/blob/main/_pages/p5js-template/index.html) é o aspecto de um arquivo `index.html` básico de um projeto p5.js. Podemos simplesmente copiar o conteúdo desse arquivo para o arquivo `index.html` vazio no nosso diretório local.

Não precisamos entender tudo o que há nesse arquivo, mas algumas linhas merecem destaque:

```html
<script src="https://cdn.jsdelivr.net/npm/p5@2.3.1/lib/p5.min.js"></script>
<script src="sketch.js"></script>
```

Essas duas linhas carregam o código JavaScript usado pelo nosso projeto. A primeira linha carrega a biblioteca p5.js de uma CDN (Content Delivery Service, ou Serviço de Entrega de Conteúdo: um servidor online). Esse arquivo contém um monte de código pré-escrito que vamos usar no nosso projeto.

A segunda linha carrega nosso arquivo JavaScript `sketch.js` do mesmo diretório em que está nosso arquivo `index.html`.

Antes de olharmos o arquivo JavaScript, mais algumas linhas de `html`:

```html
<body>
  <main id="main"></main>
</body>
```

Essas linhas configuram uma página html em branco, com um componente [`<main>`](https://www.w3schools.com/tags/tag_main.asp) vazio. Esse componente também tem um atributo `id` com valor `main`, que é o que o nosso código JavaScript vai procurar quando começar a desenhar coisas na tela.

### JavaScript

Vamos escrever o código do nosso projeto no arquivo `sketch.js`, e, eventualmente, devemos entender tudo o que ele contém.

Um arquivo `sketch.js` bem simples com o qual podemos começar pode ser assim:

```js
function setup() {
  createCanvas(windowWidth, windowHeight);
}

function draw() {
  background(220, 20, 20);
  ellipse(120, 120, 50, 50);
}
```

Podemos copiar essas linhas para o nosso arquivo `sketch.js` vazio ou baixar o arquivo [AQUI](https://github.com/IDMp5/IDMp5.github.io/blob/main/_pages/p5js-template/sketch.js).

Esse arquivo tem duas seções, uma chamada `setup` e outra chamada `draw`. Os comandos da seção `setup` ficam agrupados entre suas chaves (`{` `}`), assim como os comandos da seção `draw` ficam agrupados entre chaves.

A maioria dos nossos projetos p5.js será organizada dessa forma, com uma seção `setup` que geralmente configura alguns parâmetros do nosso projeto, seguida de uma seção `draw`, que roda repetidamente e é responsável por desenhar de fato (formas, imagens etc.) na tela e lidar com a interatividade do usuário, entre outras coisas.

Neste momento, nossa seção `setup` apenas especifica que queremos um canvas tão grande quanto a janela do nosso navegador, e nossa seção `draw` apenas preenche nosso canvas com um fundo vermelho e desenha uma elipse em algum lugar perto do canto superior esquerdo da janela da nossa página.

Como podemos conferir? Vamos fazer um navegador carregar nosso projeto e ver.

## Servidor Local & IDE

A forma mais fácil de visualizar nossos projetos p5.js enquanto os desenvolvemos localmente é abri-los em um navegador.

Mas, como o navegador trata arquivos *locais* do nosso computador de forma diferente de arquivos que ele abre a partir da internet, e como eventualmente vamos querer que nossos projetos estejam disponíveis na internet, precisamos enganar nosso navegador para que ele abra nossos arquivos locais como se eles viessem da internet.

Em outras palavras, para ver nosso projeto em um navegador, precisamos *servir* os arquivos do nosso projeto como se fossem uma página web completa hospedada em um [servidor](../../intro/javascript/).

Felizmente, podemos usar nossa IDE VSCode e a extensão [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) para criar facilmente um servidor local para o nosso projeto.

Tudo o que precisamos fazer é navegar até o diretório do nosso projeto no VSCode e clicar no botão "Go Live" na parte inferior direita da janela. Isso vai iniciar um servidor local e abrir um navegador com o nosso projeto:

{% include video.html url="p5/setup-02.webm" %}

E agora que o servidor está rodando, qualquer mudança que fizermos no código do nosso projeto será refletida no navegador:

{% include video.html url="p5/setup-03.webm" %}

E essa URL do nosso projeto, `http://127.0.0.1`, só é acessível a partir do nosso próprio computador, então o projeto está pronto para ser hospedado online, mas ainda não está na internet.
