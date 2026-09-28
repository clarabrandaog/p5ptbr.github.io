---
title: Programando Computadores
---
## Computação

Vamos considerar dois momentos da história da computação para termos um contexto sobre como projetamos, construímos e usamos computadores hoje.

Essa é apenas uma narrativa da história da computação. Siga os links abaixo para mais informações sobre outras histórias e figuras importantes para o desenvolvimento dos computadores e da computação.

Por enquanto, vamos começar nos anos $$1930$$ com o desenvolvimento paralelo de duas ideias matemáticas abstratas: as propriedades binárias dos interruptores elétricos e a Máquina de Turing.

### A Máquina de Turing

O matemático inglês Alan Turing criou o conceito da [Máquina de Turing](https://en.wikipedia.org/wiki/Turing_machine) em $$1936$$ durante seu doutorado na Universidade de Princeton.

A Máquina de Turing é um modelo conceitual de computação que descreve uma máquina simples que usa poucas regras para realizar computações arbitrárias e complexas.

Em sua forma mais simples, a máquina consiste em uma fita infinita que armazena dados e instruções. Durante sua operação, a máquina lê um valor da fita e, dependendo do histórico de valores lidos até então, ela vai sobrescrever o valor na fita, mover uma posição e ler um novo valor, ou parar. Só isso.

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/turing-machine.jpg' | relative_url }}">
</div>
*Representação simplificada de uma Máquina de Turing com o conjunto de instruções $$\{$$ $$X$$, $$Y$$, $$\varnothing$$, $$\forall$$ $$\}$$*

Esse modelo simples de ler e escrever instruções e dados no mesmo lugar ainda é usado hoje, e é o que permite que os computadores realizem um número quase infinito de tarefas usando um número finito de instruções.

### Interruptores Elétricos

Em $$1937$$ Claude Shannon escreveu sua dissertação de mestrado "[A Symbolic Analysis of Relay and Switching Circuits](https://en.wikipedia.org/wiki/A_Symbolic_Analysis_of_Relay_and_Switching_Circuits)" no MIT. Em sua dissertação, Shannon mostrou como otimizar circuitos de relés telefônicos usando uma forma de álgebra que usa apenas dois números: $$1$$s e $$0$$s.

Ele então provou que esse tipo de matemática, chamada [Álgebra Booleana](https://en.wikipedia.org/wiki/Boolean_algebra), podia ser implementado usando interruptores elétricos que estavam ligados ($$on$$) ou desligados ($$\mathit{off}$$).

As propriedades dessa álgebra binária tornam fácil construir circuitos complexos a partir de blocos de construção bem básicos e repetíveis. Isso significa que muitos tipos de cálculos e problemas lógicos passaram a poder ser resolvidos usando circuitos físicos fáceis de conceitualizar, projetar e escalar.

<div class="scaled-images">
  <img src="{{ '/assets/images/intro/shannon-switches.jpg' | relative_url }}">
</div>
*Diferentes representações das operações lógicas implementadas por Shannon usando interruptores elétricos*

Isso possibilitou a construção física de Máquinas de Turing que usam $$0$$s e $$1$$s para descrever instruções, dados e estado, e ainda é usado hoje para construir máquinas de computação mais complexas.

## Programação

Nessas duas histórias podemos ver não só o início do que depois se transformou em sistemas mais refinados de computação, mas também o surgimento de certos conceitos que continuam importantes hoje quando queremos dizer a um computador o que fazer.

Os tipos de coisas que um computador pode fazer evoluíram constantemente desde os anos $$1930$$, mas *como* dizemos aos computadores o que fazer ainda é fortemente influenciado por conceitos como memória, instruções, estado interno, loops, lógica booleana e circuitos binários que remontam a uma época em que o que entendemos hoje por *computador* nem sequer era fisicamente possível.

Programar, ou codificar, é a arte e a ciência de dizer a um computador para fazer *algo*, dando a ele alguns dados junto com sequências de instruções que especificam exatamente o que ele deve fazer com esses dados.

Alguma forma de programação de computadores já acontecia desde pelo menos os anos $$1830$$, quando [Ada Lovelace](https://en.wikipedia.org/wiki/Ada_Lovelace) escreveu um programa para calcular uma sequência de números de Bernoulli em um computador mecânico. Computadores mecânicos eram programados usando cartões perfurados, pedaços físicos de papelão com furos, que eram alimentados na máquina em momentos específicos. Os furos, ou a ausência deles, em posições específicas dos cartões, determinavam se certas conexões mecânicas eram feitas, e isso é o que fazia o computador se comportar de uma maneira específica.

Só nos anos $$1940$$, com o avanço dos computadores eletrônicos, é que escrever e executar um programa de computador passou a poder ser feito na mesma máquina. Os comandos dados a esses computadores, porém, ainda eram basicamente escritos especificando sequências de sinais ligado e desligado para interruptores eletrônicos e outros componentes.

## Linguagens

Podemos rastrear a origem das linguagens de programação que usamos hoje até o final dos anos $$1950$$, com o trabalho da cientista da computação e almirante da Marinha americana [Grace Hopper](https://en.wikipedia.org/wiki/Grace_Hopper). Hopper mostrou que termos em inglês podiam ser usados para descrever instruções de um programa de forma genérica, que depois seriam traduzidas em sequências de sinais ligado e desligado para computadores específicos.

Isso levou ao surgimento de linguagens de programação "de alto nível", independentes de máquina, como FORTRAN, ALGOL e COBOL, que podiam ser usadas para escrever programas em uma linguagem legível para humanos. O termo "alto nível" aqui é um tanto relativo e fluido. É usado para descrever o quão próxima uma linguagem de programação está de uma linguagem natural, e para indicar o nível de abstração que a linguagem oferece em relação ao hardware específico em que os programas resultantes vão rodar.

O que era considerado "alto nível" nos anos $$1950$$ e $$1960$$ certamente não é o que consideramos "alto nível" hoje.

A maioria das linguagens de programação populares usadas hoje tem sintaxes bastante expressivas, com comandos de uma única palavra que se transformam em sequências muito longas de instruções para o computador executar. Algumas dessas linguagens nem sequer exigem uma etapa separada de tradução para transformar o código legível por humanos em instruções para o computador.

## JavaScript e p5.js

Uma dessas linguagens é o JavaScript.

JavaScript é uma linguagem interpretada, o que significa que o código que escrevemos não é compilado, ou seja, traduzido, em instruções para o computador, mas sim *interpretado* linha por linha por um programa separado responsável por executar nosso código.

Uma das principais razões para a popularidade do JavaScript é que, na maioria dos casos, o programa que interpreta e executa nosso código JavaScript é um navegador. A maioria dos computadores, celulares, relógios etc. têm um navegador. Isso significa que não só não precisamos nos preocupar com as especificidades do hardware em que ele vai rodar, mas, na maioria dos casos, nem precisamos nos preocupar com o sistema operacional ou outras questões de compatibilidade.

Por ser uma linguagem de programação de alto nível e multiparadigma, o JavaScript pode ser usado para escrever diferentes tipos de programas usando diferentes técnicas ou estilos de programação. E, como a maioria das outras linguagens de programação genéricas, ele depende de *bibliotecas* para estender suas funcionalidades principais e oferecer formas mais fáceis de realizar tarefas específicas.

A biblioteca [p5.js](https://p5js.org/) é uma biblioteca JavaScript para creative coding, com foco em tornar a programação acessível e inclusiva. Ela estende a funcionalidade principal da linguagem JavaScript e facilita para os programadores a criação de experiências audiovisuais usando imagens, desenhos, vídeos, som etc., diretamente em uma página web no navegador.

## Referências

Novamente, essa é uma narrativa bem resumida e particular de alguns momentos da história da computação. Outros momentos, pessoas e histórias importantes podem ser encontrados nos seguintes links:

- [A sketch for an alternate history of computing](https://phoenixperry.medium.com/an-alternate-history-of-computing-a-sketch-1811197814ff)
- [The Story of NASA’s *Hidden Figures*](https://www.scientificamerican.com/article/the-story-of-nasas-real-ldquo-hidden-figures-rdquo/)
- [Rhizome's Queer History of Computing](https://rhizome.org/editorial/2013/feb/19/queer-computing-1/)
- [The Wild West of Computing](https://cutpathways.podbean.com/e/a-byte-size-history-of-computing/)
