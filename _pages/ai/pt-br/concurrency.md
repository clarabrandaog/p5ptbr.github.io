---
title: Concorrência
---

Antes de realmente falarmos sobre modelos de IA e como usá-los, precisamos falar sobre concorrência.

Em geral, os programas são processos sequenciais: o código que escrevemos é executado uma linha de cada vez, seguindo a ordem em que foi escrito.

Tradicionalmente, esse modelo mental também funcionava bem com a forma como os processadores executavam o código: uma instrução/comando de cada vez.

Hoje em dia, nenhuma dessas suposições/modelos é totalmente verdadeira o tempo todo: os processadores conseguem fazer várias coisas ao mesmo tempo, e podemos precisar que o código rode fora de ordem.

## Interatividade

Um lugar em que o modelo sequencial de execução de código realmente desmorona é onde há muita interação do usuário, como em um navegador. Digamos que temos uma página com uma animação e um botão para baixar um snapshot da animação. Quando o usuário clica no botão, o botão gira e muda de cor, e também dispara um pop up pedindo o email do usuário.

Se estivéssemos executando essas tarefas sequencialmente, uma ordem possível para elas seria esta:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/concurrency-00.jpg' |relative_url }}">
</div>

Cada um desses passos tem que terminar antes que o próximo possa começar e, durante todo o tempo em que eles estão sendo executados, a animação fica pausada na tela e o usuário não consegue clicar em mais nada. Se for uma animação interativa, e a imagem demorar a ficar pronta e a ser baixada, o usuário tem que esperar e, em alguns casos, pode até ficar sem controle do mouse.

Se, no entanto, tivermos alguma forma de dizer ao computador que algumas dessas tarefas podem ser executadas de forma independente umas das outras e em paralelo, o grafo de execução das ações acima pode ficar assim:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/concurrency-01.jpg' |relative_url }}">
</div>

Parte do código que executa a animação do botão, exibe o formulário de download e prepara a imagem pode rodar ao mesmo tempo, de forma concorrente. E, mais importante, a animação pode continuar rodando em segundo plano. Ela não precisa parar e esperar que as outras ações terminem de ser executadas.

Quando a imagem estiver pronta e o usuário tiver preenchido o formulário, ela será baixada em paralelo com qualquer outra ação que o usuário esteja realizando. A animação continua interativa o tempo todo e em nenhum momento o usuário perdeu o controle do mouse.

## Concorrência em JavaScript

Um pequeno, mas extremamente importante detalhe, que passou despercebido na descrição acima, estava implícito na frase "_quando a imagem estiver pronta e o usuário tiver preenchido o formulário_". Na verdade, precisamos de um mecanismo que detecte quando certas ações terminam de ser executadas, ou que dispare novas ações paralelas quando uma certa ação termina de ser executada.

Já vimos algo assim em algumas das funções do `p5.js` que carregam dados de arquivos, como: `loadImage()`, `loadStrings()` e `loadJSON()`. Se olharmos a [documentação](https://p5js.org/reference/p5/loadImage/) delas, vamos ver que todas têm uma assinatura parecida:

`loadImage(path, [successCallback], [failureCallback])`

onde `successCallback` e `failureCallback` são argumentos opcionais que especificam funções a serem executadas quando o arquivo do argumento `path` tiver sido baixado, ou tiver falhado ao carregar.

Baixar um arquivo pode demorar bastante, dependendo do tamanho e da localização do arquivo, e como não queremos pausar o nosso programa enquanto isso acontece, dizemos ao computador para começar a baixar o arquivo e especificamos o que ele deve fazer quando terminar.

<div class="scaled-images left w50">
  <img src="{{ 'assets/images/ai/concurrency-02.jpg' |relative_url }}">
</div>

Essas funções que serão chamadas no futuro são chamadas de funções _callback_, e esse é um padrão muito comum para código interativo em JavaScript.

Considere o seguinte código:

{% include p5-editor.html id="tHDUhP9Nb9" %}

A função `takesLongTimeSync()` faz algo que demora muito para terminar e, como é uma função síncrona, o nosso código não faz mais nada enquanto espera ela terminar. Chamamos a função uma vez no frame $$61$$ e imprimimos `DONE!` quando ela termina de rodar.

Conseguimos ver claramente quando a função `takesLongTimeSync()` é executada, porque a nossa animação simplesmente para.

Agora, considere rodar uma versão assíncrona da função, que recebe um callback como argumento: `takesLongTime(callback)`.

{% include p5-editor.html id="ppUUVBffy" %}

O código é quase o mesmo, mas temos que criar a função que será chamada quando `takesLongTime()` terminar. Neste caso, ela pode ser tão simples quanto uma função que apenas imprime `DONE!`, como antes:

```js
function printDone() {
  pEl.html("DONE!");
}
```

A grande diferença agora é que, enquanto `takesLongTime()` está sendo executada, o nosso código continua rodando e o nosso loop `draw()` pode continuar desenhando.

## `async` / `await`

O padrão _callback_ para programação concorrente é bom para casos simples, mas pode levar a algo assim se tivermos que encadear várias chamadas de funções assíncronas:

```js
function evenMoreCallback() {
  print("DONE!");
}

function anotherCallback() {
  evenMoreAsyncFunction(evenMoreCallback);
}

function secondCallback() {
  anotherAsyncFunction(anotherCallback);
}

function firstCallback() {
  secondAsyncFunction(secondCallback);
}

takesLongTime(firstCallback);

```

Isso é confuso de ler e de escrever. A versão em que usamos funções anônimas é ainda pior:

```js
takesLongTime(function() {
  secondAsyncFunction(function() {
    anotherAsyncFunction(function() {
      print("DONE!");
    })
  })
});
```

Como encadear funções assíncronas assim é um padrão tão comum em código interativo/de navegador, o `JavaScript` nos dá outra forma de fazer isso, usando funções `async`.

Uma função `async` diz ao motor de execução do `JavaScript` que ele deve ir executar a função e depois voltar ao código principal quando a execução terminar.

Usando isso no nosso exemplo acima, fica assim:

{% include p5-editor.html id="iJ93v33Tx" %}

É uma forma de fingir que as funções assíncronas são síncronas e escrever código que parece síncrono, mas que na realidade roda de forma assíncrona.

Não parece muito diferente para este caso bem simples, mas, se tivéssemos que encadear várias chamadas de funções assíncronas, ficaria assim:

```js
await takesLongTime();
await secondAsyncFunction();
await anotherAsyncFunction();
await evenMoreAsyncFunction();
print("DONE!");
```

Sem chamadas de função aninhadas nem definições de função complicadas. As funções assíncronas `takesLongTime()`, `secondAsyncFunction()`, etc. todas precisam retornar um objeto especial do `JavaScript` chamado `Promise`, mas, quando isso está configurado direito, o nosso código parece código síncrono.

Esta foi uma introdução rápida à execução concorrente / assíncrona de código em JavaScript.

Vamos ver como isso aparece quando começamos a trabalhar com modelos de ML / IA.
