---
title: Introdução às Redes Neurais
---
É impossível falar de creative coding, experiências interativas ou design generativo sem falar de IA e modelos generativos.

O que são eles ? De onde vêm ?

A maioria dos grandes modelos generativos de hoje é, na verdade, construída combinando algumas ideias básicas que já existem há um tempo, como sistemas preditivos e modelos de Machine Learning.

Vamos olhar alguns dos blocos de construção desses modelos e como eles são criados.

## Machine Learning

Machine Learning é o processo de usar dados para ajustar automaticamente parâmetros no nosso código.

Digamos que temos uma loja online e só queremos exibir anúncios nos horários em que sabemos que temos muitos visitantes no nosso site.

Poderíamos chutar quais são esses horários e escrever um código que se parece com isto:
```js
if (time == "10am" or time == "4pm" or time == "8pm") {
  displayAds();
}
```

Ou, em vez disso, poderíamos fazer o nosso código usar informações sobre as visitas de ontem para ativar os anúncios. Poderíamos descobrir que na verdade queremos mostrar os anúncios às 10:15, 4:33 e 8:10. Isso é Machine Learning.

## ML em Resumo

Existem duas etapas no processo de modelagem:

Treinamento, quando criamos um modelo, e inferência, quando usamos o modelo em dados novos.

Durante o treinamento, mostramos muitos e muitos dados para um pedaço de código que se ajusta automaticamente para detectar padrões e melhorar seu entendimento sobre os dados que estão sendo mostrados. Se também mostrarmos ao modelo a resposta correta que queremos que ele aprenda, estamos fazendo aprendizado supervisionado.

Aqui, estamos mostrando uma variedade de formas de desenhar o número $$8$$ para um pedaço de código que aprende padrões nos formatos que vê nas imagens:

<div class="scaled-images w66">
  <img src="{{ 'assets/images/ai/nn-training-inference-00.jpg' |relative_url }}">
</div>

Durante a inferência, mostramos ao modelo um novo dado e esperamos que ele consiga prever algo sobre esse dado novo com base no que aprendeu anteriormente no treinamento.

Dado um novo desenho do número $$8$$, queremos que o modelo ainda consiga reconhecê-lo como o número $$8$$:

<div class="scaled-images w66">
  <img src="{{ 'assets/images/ai/nn-training-inference-01.jpg' |relative_url }}">
</div>

## Tipos de Tarefas

Esse tipo de tarefa (prever um valor numérico a partir da imagem de um dígito) é chamado de _classificação_ e é um dos dois grandes tipos de tarefas que os modelos supervisionados executam bem.

Classificação é quando treinamos um modelo para agrupar itens semelhantes em classes pré-definidas. Embora possa parecer que um modelo de classificação consegue "detectar" objetos em uma imagem, o que ele realmente está fazendo é aprender a dividir o espaço de todas as entradas possíveis em subgrupos ou categorias.

Se estamos treinando um modelo de classificação de dígitos, o que o modelo realmente está fazendo é aprender a dividir o espaço de todas as imagens possíveis de dígitos em $$10$$ subcategorias, uma para cada dígito possível. Se mostrarmos ao modelo uma imagem de um gato, ele vai classificá-la como um dígito, já que é isso que ele sabe fazer; foi assim que ele dividiu o seu espaço de entrada.

Modelo de classificação depois do treinamento:

<div class="scaled-images w66">
  <img src="{{ 'assets/images/ai/nn-classification-00.jpg' |relative_url }}">
</div>

Inferência do modelo de classificação:

<div class="scaled-images w66">
  <img src="{{ 'assets/images/ai/nn-classification-01.jpg' |relative_url }}">
</div>

O outro tipo de tarefa para o qual o aprendizado supervisionado costuma ser usado é chamado de _regressão_.

É quando um modelo aprende tendências e padrões em dados numéricos para produzir uma previsão de valor contínuo a partir de novos dados de entrada. Por exemplo, podemos treinar um modelo de regressão que aprende sobre preços de casas com base na idade, localização, número de quartos e distância até o transporte público. Depois, quando mostramos ao modelo informações sobre a idade, localização, número de quartos e distância até o transporte público de casas novas, o modelo prevê quanto essas casas novas valem.

Dados de regressão:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-regression-00.jpg' |relative_url }}">
</div>

Treinamento do modelo de regressão, ou ajuste (fitting):

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-regression-01.jpg' |relative_url }}">
</div>

Inferência do modelo de regressão:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-regression-02.jpg' |relative_url }}">
</div>

A regressão produz valores contínuos, enquanto a classificação produz valores discretos.

É comum ver modelos de classificação tão específicos nas suas previsões que parece que estão aprendendo conforme veem dados novos, ou que estão interpolando entre classes, mas normalmente não é isso que acontece. Um modelo de classificação assim provavelmente foi treinado para separar imagens em milhares ou dezenas de milhares de classes.

## Redes Neurais

Mas, como conseguimos modelos ?

Existem muitas formas diferentes de construir modelos, dependendo do tipo e da quantidade de dados que temos disponíveis para o treinamento. As pessoas ajustam modelos de classificação e regressão há muito tempo. Alguns dos primeiros [computadores mecânicos](https://en.wikipedia.org/wiki/Tide-predicting_machine) foram construídos para aprender padrões do oceano e prever o ritmo das marés.

Hoje, dados os avanços da computação paralela com GPUs e a quantidade enorme de dados que coletamos sobre nós mesmos, é mais comum construir modelos usando redes neurais.

Esse é um assunto para uma série inteira de tutoriais, mas, por enquanto, o que é importante saber sobre as redes neurais é que elas são feitas de muitas e muitas unidades computacionais muito simples, chamadas "neurônios", que ligam (`on`) ou desligam (`off`) com base nas entradas que recebem.

Esses neurônios podem ter qualquer número de entradas e o cálculo que realizam geralmente é uma soma ponderada seguida de uma função de ativação. Para o neurônio abaixo, a saída é calculada primeiro pegando metade de $$X_0$$, somando $$0.3$$ de $$X_1$$, $$1.2$$ vezes $$X_2$$ e $$0.1$$ de $$X_3$$. Depois, se essa soma for maior que $$W_T$$ ($$2.2$$), o neurônio será ativado e o seu valor de ativação será propagado para o próximo neurônio da rede.

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-neuron-00.jpg' |relative_url }}">
</div>

Isso pode parecer muito simples, e é, mas, quando conectamos de forma inteligente centenas, milhares, milhões deles, eles podem ser treinados para ativar sob condições e padrões bem específicos.

Mas... como ?

Bem... aqueles parâmetros $$W$$ na imagem acima, às vezes chamados de pesos das entradas, são aprendidos durante o treinamento.

## Construindo uma Rede Neural

O processo simplificado de criar um modelo de rede neural pode ser mais ou menos assim:

Primeiro, criar uma rede. Elas podem ter quantas camadas e neurônios quisermos, mas existem algumas arquiteturas de rede específicas que funcionam melhor para certos tipos de tarefas e certos tipos de dados. Além disso, quanto mais neurônios tivermos para treinar, mais dados vamos precisar, então é uma troca que temos que fazer.

Dois aspectos importantes da rede que precisam ser considerados aqui são as camadas de entrada e de saída. A camada de entrada tem que ter neurônios suficientes para acomodar o tipo de dado com o qual estamos trabalhando, e a camada de saída tem que ter tantos neurônios quanto sinais de saída.

Neste exemplo, as nossas imagens de dígitos de entrada têm todas $$28$$ x $$28$$ pixels, para um total de $$784$$ pixels, então precisamos de $$784$$ neurônios na entrada:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-layers-01.jpg' |relative_url }}">
</div>

Se estivéssemos trabalhando com palavras da língua inglesa, poderíamos precisar de um neurônio para cada palavra possível da língua.

Como aqui estamos tentando prever um entre $$10$$ dígitos, precisamos de $$10$$ neurônios de saída. Quando damos à rede uma imagem de um dígito, a previsão dela é codificada nessa camada de saída. Normalmente conseguimos decodificar qual classe está sendo prevista olhando a posição do neurônio mais fortemente ativado: se o primeiro neurônio é o mais forte, a rede está prevendo um $$0$$, se o segundo neurônio é o mais forte, então é um $$1$$, e assim por diante.

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-layers-02.jpg' |relative_url }}">
</div>

Se estivéssemos prevendo preços de casas, precisaríamos de apenas $$1$$ neurônio de saída de valor contínuo. Se estivéssemos prevendo preços de venda e de aluguel separadamente para essas casas, precisaríamos de $$2$$ neurônios de saída.

Depois que a arquitetura da rede está selecionada e conectada, podemos começar a treiná-la.

## Treinando uma Rede

Treinar a nossa rede de classificação de dígitos pode ser mais ou menos assim:

Dar à rede uma imagem de um número e ver o que ela prevê:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-training-00.jpg' |relative_url }}">
</div>

Ajustar os pesos/parâmetros da camada de saída para que, da próxima vez que a rede vir essa imagem, ela faça uma previsão um pouco melhor:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-training-01.jpg' |relative_url }}">
</div>

Depois, ajustar os pesos/parâmetros da penúltima camada para que, da próxima vez que a rede vir essa imagem, a última camada tenha informações melhores para fazer uma previsão melhor:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-training-02.jpg' |relative_url }}">
</div>

Depois... propagar esse tipo de ajuste até a camada de entrada:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-training-03.jpg' |relative_url }}">
</div>

E continuar mostrando à rede outra imagem de um número e repetindo o processo:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-training-04.jpg' |relative_url }}">
</div>

Continuar mostrando todas as imagens de treinamento para a rede até que ela tenha ajustado pesos suficientes para atingir um nível aceitável de acurácia nas suas previsões. Nenhuma previsão vai ter $$100\%$$ de certeza: alguns dos neurônios de saída _errados_ sempre vão ativar. O importante é que o neurônio correto seja o mais fortemente ativado:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-training-05.jpg' |relative_url }}">
</div>

Uma coisa importante a notar aqui é que esse é um processo lento. Os neurônios têm que aprender não só a detectar padrões sobre os $$10$$ dígitos, mas também padrões diferentes dentro de um mesmo dígito. Nem todos os zeros são iguais. Para fazer isso, a rede não pode simplesmente escolher os valores ótimos de parâmetros para uma imagem específica, e sim ajustar-se lentamente ao longo do tempo, e ao longo de centenas ou milhares de imagens, para chegar a um conjunto de parâmetros que funcione para uma grande variedade de imagens.

Mas é isso.

Resumindo: os modelos de IA fazem uma de duas coisas: classificam dados de entrada ou preveem valores contínuos. Hoje em dia, a maioria, se não todos, os nossos modelos de IA são construídos usando Redes Neurais, que são coleções muito grandes de unidades de soma muito simples que são ajustadas olhando milhares e milhares de exemplos de entrada.

## Mais Modelos

Esse processo geral de treinamento de um modelo é praticamente o mesmo, independente do tipo de rede.

Outros tipos de modelos de IA que encontramos com frequência podem não parecer relacionados aos modelos de classificação ou regressão que vimos, mas há uma boa chance de que sejam.

Por exemplo, um modelo de geração de texto, como o ChatGPT, é na realidade um modelo de classificação complexo. Durante o treinamento, o modelo aprende a classificar sequências de palavras de frases diferentes em classes. Às vezes são sequências de $$5$$ palavras, às vezes de $$1000$$ palavras, mas, se configurarmos a nossa rede para ter um neurônio de saída para cada palavra possível da nossa língua, o que o modelo realmente acaba aprendendo é a prever a próxima palavra de uma dada sequência de palavras:

<div class="scaled-images left w75">
  <img src="{{ 'assets/images/ai/nn-gen-text-00.jpg' |relative_url }}">
</div>

Modelos que detectam não só se há um objeto específico em uma imagem, mas também onde na imagem o objeto está, podem parecer modelos totalmente diferentes. Na realidade, esses modelos de detecção de objetos são uma combinação de um modelo de classificação com um modelo de regressão. O modelo de classificação classifica subseções da imagem em possíveis classes de objetos, e o modelo de regressão aprende os $$8$$ valores associados às caixas que são desenhadas em volta dos objetos:

<div class="scaled-images left">
  <img src="{{ 'assets/images/ai/nn-bike-dog-05.jpg' |relative_url }}">
</div>

O modelo de regressão não sabe o que são os objetos, e o modelo de classificação não sabe onde os objetos começam ou terminam, mas as previsões dos dois podem ser combinadas para nos dar localizações exatas de objetos específicos.

## Mas, Falando Sério

Treinar modelos pode ser divertido, mas leva muito tempo, dados e energia.

O que podemos fazer aqui é focar em como escolher o modelo pré-construído correto para uma tarefa específica e como usar diferentes bibliotecas e estratégias para rodar inferência com esses modelos pré-construídos.

Vamos começar.
