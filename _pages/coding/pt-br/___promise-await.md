---
title: Promises, async, await
---

## Promises & async / await

Algumas das coisas que pedimos aos nossos programas levam tempo. E não é só um pouquinho de tempo: baixar um arquivo da internet pode levar vários segundos. Se o nosso programa fica esperando por uma imagem, um som ou um arquivo de dados, tudo para: as animações congelam, os botões não respondem, nossos sketches travam. Para contornar isso, usamos as ferramentas nativas do JavaScript para lidar com o que demora: **Promises** e **async / await**.

Dê uma olhada neste código:

```js
let data = loadJSON("https://data.com/rent.json");
```

Nossa intuição diz que a variável `data` agora deveria conter os dados do arquivo JSON. Mas o arquivo está em um servidor em algum lugar da internet. Baixá-lo leva tempo, muitos milissegundos na melhor das hipóteses, segundos na pior. O JavaScript não pode simplesmente parar tudo para esperar.

`loadJSON()` nos entrega uma **Promise** e, mais tarde, os dados.

## Promise

Uma **Promise** é um objeto JavaScript que representa um valor que ainda não existe, mas vai existir no futuro. É uma promissória de algo que está a caminho.

Pense em pedir comida em um restaurante. Você não recebe o prato na hora. Em vez disso, recebe um número (uma promessa) que diz "seu prato ficará pronto em breve". Você pode fazer outras coisas enquanto espera: olhar o celular, conversar com um amigo, ler um livro, etc. Quando a comida fica pronta, você ouve seu número ser chamado e vai buscá-la.

Uma Promise funciona do mesmo jeito. Ela pode estar em um destes estados:

- **pending** (pendente): os dados estão a caminho (o arquivo está sendo baixado).
- **fulfilled** (cumprida): os dados estão prontos (o arquivo chegou e está armazenado).
- **rejected** (rejeitada): algo deu errado (o arquivo não existe, ou o servidor não respondeu).

Toda função do p5.js que carrega alguma coisa retorna uma Promise: `loadJSON()`, `loadImage()`, `loadTable()`, `loadStrings()`, `loadFont()` e `loadSound()`. Para tirar o valor de verdade de dentro de uma Promise, usamos uma palavra-chave chamada `await`.

## await

A palavra-chave `await` faz o que o nome sugere: espera. Quando escrevemos `await` antes de uma Promise, nosso código pausa naquela linha até que a Promise seja cumprida.

O `await` só pode ser usado dentro de funções marcadas com a palavra-chave `async`. Uma função `async` é uma função que tem permissão para pausar e esperar: ela diz ao JavaScript "esta função pode usar `await`". Isso também significa que a própria função retorna uma Promise, um valor que estará pronto mais tarde, quando tudo o que está dentro da função terminar.

No p5.js podemos tornar a nossa função `setup()` `async`. O p5.js executa o `setup()` e espera ele terminar, incluindo todos os seus `await`s, antes de começar a chamar o `draw()`:

```js
let data;

async function setup() {
  createCanvas(640, 480);
  data = await loadJSON("https://data.com/rent.json");
  // o código abaixo desta linha só roda quando a promise do loadJSON for cumprida
}
```

Vamos por partes:

1. O sketch começa e o p5.js chama o `setup()`.
2. O `loadJSON()` começa a baixar o arquivo JSON. Por causa do `await`, o código pausa ali mesmo.
3. Quando o arquivo chega, a Promise é cumprida e os dados baixados são atribuídos a `data`.
4. O `setup()` agora pode terminar, executando qualquer código que venha depois do `loadJSON()`.

## Legal, e o que acontece se um arquivo não estiver lá?

Não há garantia de que os arquivos carreguem. O arquivo pode não existir, a URL pode estar errada, a rede pode falhar ou o servidor pode estar fora do ar. Quando isso acontece, a Promise é **rejeitada** em vez de cumprida, e a nossa linha com `await` lança um erro.

Podemos tratar isso com `try` / `catch`, que nos permite especificar o que fazer se um bloco de código falhar:

```js
let data;

async function setup() {
  createCanvas(640, 480);
  try {
    data = await loadJSON("https://data.com/rent.json");
  } catch (err) {
    print("Não foi possível carregar os dados: " + err);
    data = []; // use um array vazio como alternativa ou, melhor ainda, guarde alguns dados de exemplo
  }
}
```

Com um valor alternativo definido, o resto do nosso sketch pode continuar rodando mesmo quando o carregamento falha.

## Vi referências a callbacks, o que é isso?

Antes de o `async` / `await` existir, o JavaScript usava **funções callback**: você passava uma função como argumento, e essa função era chamada quando o valor estivesse pronto.

```js
// versão com callback
loadJSON("https://data.com/rent.json", (data) => {
  // use data aqui, dentro do callback
});
```

O problema dos callbacks é que, se precisamos carregar várias coisas em sequência, as funções precisam ficar aninhadas dentro de outras funções e o código fica difícil de ler. Com o `await`, o código se lê como código síncrono:

```js
// versão com await: lê de cima para baixo, como código síncrono
data = await loadJSON("https://data.com/rent.json");
// use data aqui, no fluxo normal do nosso código
```

As funções de carregamento do p5.js ainda aceitam callbacks, mas o `await` é o padrão que vamos usar quando escrevermos nosso próprio código. Podemos encontrar algumas bibliotecas que dependem de callbacks, mas vamos evitá-los sempre que possível.

## Carregando todo tipo de coisa

Esse padrão funciona para todas as funções de carregamento do p5.

Imagens:

```js
let mImage;

async function setup() {
  createCanvas(640, 480);
  mImage = await loadImage("photo.jpg");
}

function draw() {
  image(mImage, 0, 0);
}
```

Fontes:

```js
let myFont;

async function setup() {
  createCanvas(640, 480);
  myFont = await loadFont("font.otf");
  textFont(myFont);
}
```

Tabelas e arquivos de texto:

```js
let mTable;
let mLines;

async function setup() {
  createCanvas(640, 480);
  mTable = await loadTable("data.csv", "csv", "header");
  mLines = await loadStrings("story.txt");
}
```

Arquivos de som:

```js
let mSound;

async function setup() {
  createCanvas(640, 480);
  mSound = await loadSound("song.mp3");
}

function mousePressed() {
  mSound.play();
}
```
## Carregando várias coisas ao mesmo tempo

Se usarmos `await` em uma função de carregamento depois da outra, os arquivos são baixados em sequência: primeiro uma imagem, depois uma fonte, depois alguns dados CSV (ou como você organizar esse código). Cada arquivo teria que terminar antes de o próximo começar.

Se os seus arquivos não dependem uns dos outros, eles podem ser baixados todos ao mesmo tempo. O JavaScript nos dá um auxiliar para isso: `Promise.all()`. Ele recebe um array de Promises e retorna uma única Promise que é cumprida quando todas elas estão prontas:

```js
let images;

async function setup() {
  createCanvas(640, 480);
  let results = await Promise.all([
    loadImage("one.png"),
    loadImage("two.png"),
    loadImage("three.png")
  ]);
  // results é um array com as três imagens, na ordem
  images = results;
}
```

Usar o `Promise.all()` costuma ser mais rápido do que carregar os arquivos um de cada vez, porque os downloads se sobrepõem.


## Resumo

- Uma **Promise** representa um valor que estará disponível no futuro.
- `loadJSON()`, `loadImage()`, `loadTable()`, `loadStrings()`, `loadFont()` e `loadSound()` retornam Promises.
- O `await` pausa o nosso código até que uma Promise seja cumprida, e o `await` só funciona dentro de funções `async`.
- Uma função `async` pode usar `await` e retorna uma Promise.
- No p5.js, carregamos tudo o que precisamos no início de uma `async function setup()`, antes de o p5.js começar a chamar o `draw()`.
- Use `Promise.all()` para carregar vários arquivos ao mesmo tempo.
- Use `try` / `catch` para tratar arquivos que falham ao carregar.
- O `await` funciona com qualquer Promise, não só com funções do p5.js.

Agora que sabemos como esperar nossos arquivos, podemos colocar isso em prática. A seguir, vamos carregar dados reais nos nossos sketches (arquivos JSON e CSV) e visualizá-los.
