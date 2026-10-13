---
title: Carregando Dados (JSON/CSV)
---

Vimos como [arrays](../../coding/arrays/) e [objetos](../../coding/objects/) podem ser usados para organizar nossos dados, e também como usar loops `for()` para criar, armazenar e processar valores nessas estruturas. Também vimos como a função [`map()`](../maths/) pode ser usada para transformar valores de entrada, em diferentes intervalos e unidades, em parâmetros para visualizações.

Agora podemos olhar maneiras de obter, processar e visualizar dados que foram criados e armazenados anteriormente em arquivos locais ou online.

## JSON

Para fazer isso vamos usar um formato de arquivo chamado [JSON](https://en.wikipedia.org/wiki/JSON), ou JavaScript Object Notation (Notação de Objetos JavaScript). *JSON é um formato de arquivo aberto e padrão de troca de dados que usa texto legível por humanos para armazenar e transmitir objetos de dados*. Se olharmos para um arquivo JSON, vamos ver que ele é estruturado exatamente como um objeto JavaScript, com dados organizados em pares chave-valor:
```json
[
	{ "zip": 10001, "rent": 4230, "bedrooms": 2 },
	{ "zip": 10012, "rent": 1830, "bedrooms": 1 },
	{ "zip": 10217, "rent": 3400, "bedrooms": 1 },
	{ "zip": 11217, "rent": 2160, "bedrooms": 1 },
	{ "zip": 11238, "rent": 2690, "bedrooms": 2 }
]
```

Neste caso temos um array de objetos, cada um com $$3$$ pares chave-valor. A sintaxe é quase exatamente como no JavaScript, com a única exceção de que as chaves em um arquivo JSON precisam ter aspas (" ") ao redor dos seus nomes. Não é surpresa que possamos quase ler um arquivo JSON diretamente para dentro de uma variável JavaScript, e menos surpreendente ainda que o p5.js tenha algumas funções que nos ajudam a fazer exatamente isso.

A função [`loadJSON()`](https://p5js.org/reference/#/p5/loadJSON) recebe a localização de um arquivo JSON (armazenado localmente ou online) e retorna um objeto JavaScript com os dados do arquivo. Como o arquivo pode ser grande, ou vir de algum outro lugar da internet, pode levar muito tempo para baixar e fazer nosso sketch travar e ficar sem resposta.

Uma forma de evitar essa situação é definir uma função especial (como `setup()` e `draw()`) que o p5.js vai usar para buscar e carregar quaisquer dados antes de começar a executar qualquer outra parte do nosso código.

Se olharmos a documentação da função [`preload()`](https://p5js.org/reference/#/p5/preload), ela diz que é uma função chamada uma vez, imediatamente antes do `setup()`, que garante que qualquer carregamento de dados aconteça antes de começarmos o sketch. Também é importante notar que, segundo a documentação, *nada além de chamadas de carregamento (`loadImage()`, `loadJSON()`, `loadFont()`, etc.) deve ficar dentro da função preload*.

É assim que definimos nosso `preload()` para carregar um arquivo JSON da internet:
```js
let data;

function preload() {
  let url = "https://data.com/rent.json";
  data = loadJSON(url);
}
```

A variável `url` também poderia especificar um arquivo local, mas, de qualquer forma, quando a função `setup()` rodar, nossa variável `data` já terá todas as informações que estão dentro do arquivo `rent.json`, e então podemos usar loops `for()` para processá-las como quisermos:

{% include p5-editor.html id="HxGSOzMFS" %}

Algumas coisas a notar sobre o sketch acima:

1. Estamos carregando um arquivo local, então o endereço que passamos para a função `loadJSON()` especifica um arquivo chamado `Winemag_100.json` que está na mesma pasta do nosso arquivo `sketch.js`.

2. Primeiro carregamos o JSON em uma variável chamada `dataObject` e depois, no `setup()`, usamos essa variável e algumas funções do JavaScript para colocar o conteúdo do arquivo em um array que possamos iterar. Isso nem sempre é necessário, apenas quando nosso arquivo especifica um array e queremos trabalhar com um array. Se olharmos a documentação do p5.js para [`loadJSON()`](https://p5js.org/reference/#/p5/loadJSON), ela diz: *Note que mesmo que o arquivo JSON contenha um Array, um Objeto será retornado*. Usamos `Object.values(dataObject)` para transformar o Objeto em um array.

3. Durante o `setup()` estamos iterando por todos os pontos de dados do nosso conjunto de dados para determinar os valores mínimo e máximo de $$2$$ propriedades (`price` e `points`). Isso é necessário para podermos usar [`map()`](https://p5js.org/reference/#/p5/map) mais tarde no `draw()` para escalar esses valores e usar o canvas inteiro para visualizar os dados (uma explicação detalhada do `map()` pode ser encontrada [aqui](../maths/)).

Mais uma coisa sobre parâmetros/campos de um JSON: às vezes os nomes dos campos têm espaços ("final price", "CRASH TIME") ou caracteres especiais ("pm2.5", "U$D"), e acessá-los apenas com a notação de ponto não funciona:
```js
data[i].pm2.5; // erro
data[i].U$D; // erro
data[i].final price; // erro
data[i].CRASH TIME; // erro
```

Nesses casos, temos que usar colchetes, assim como ao acessar um array, só que em vez de um número como índice passamos o nome do campo de dados entre aspas (" "):
```js
// estes funcionam
data[i]["pm2.5"];
data[i]["U$D"];
data[i]["final price"];
data[i]["CRASH TIME"];
```

## CSV
Além do JSON, existe outro formato de arquivo usado com frequência para armazenar e transmitir dados estruturados: Comma-Separated Values (Valores Separados por Vírgula), ou CSV. De certa forma, esse formato é um pouco mais fácil de ler, porque é menos redundante e se parece muito com uma planilha, já que organiza os dados em linhas e colunas.

Um exemplo de arquivo CSV, com os mesmos dados do nosso arquivo JSON acima, poderia ser assim:

```csv
zip,rent,bedrooms
10001,4230,2
10012,1830,1
10217,3400,1
11217,2160,1
11238,2690,2
```

O nome dos parâmetros (às vezes chamados de *features* ou *campos*) é especificado na primeira linha do arquivo e, depois disso, cada linha representa um ponto de dados, com os valores de suas features separados por vírgulas (,).

É bom conhecer tanto o JSON quanto o CSV, porque às vezes os dados que queremos usar só estão disponíveis em um dos dois formatos.

Felizmente, o p5.js também tem funções para nos ajudar a carregar, processar e usar dados tabulares em arquivos CSV. A função [`loadTable()`](https://p5js.org/reference/#/p5/loadTable) é muito parecida com o `loadJSON()`, porque ela baixa o arquivo, lê e organiza os dados de uma forma que é mais fácil de acessar no resto do nosso sketch.

Assim como o `loadJSON()`, o `loadTable()` também deve ser colocado dentro da nossa função `preload()`.
```js
let data;

function preload() {
  let url = "https://data.com/rent.json";
  data = loadTable(url, "csv", "header");
}
```

Depois disso, os dados estarão disponíveis na variável `data`, mas, diferente do caso do arquivo JSON, os dados não estão em um objeto ou array JavaScript e sim em um objeto [`Table`](https://p5js.org/reference/#/p5.Table). Esse objeto tem algumas funções especiais que precisamos usar para iterar sobre o seu conteúdo.

Em vez de usar `data.length` para saber quantos pontos de dados há no nosso conjunto de dados, agora temos que usar `data.getRowCount()`. E em vez de acessar os dados usando indexação de array e objeto, como `data[i].price`, temos que usar `data.getNum(i, "price")`. Podemos ver como são parecidos: em ambos os casos estamos especificando qual ponto de dados queremos usando um índice numérico $$i$$, e depois qual de seus campos queremos usando um nome em texto, como "price".

Vamos ver um exemplo completo que faz a mesma coisa que o exemplo JSON acima:

{% include p5-editor.html id="DUn4Pd1-a" %}

Assim como no exemplo JSON, primeiro calculamos os valores mínimo e máximo nos campos `price` e `points`, para podermos depois escalar os valores desses campos e usar o canvas inteiro ao desenhar nossa visualização.
