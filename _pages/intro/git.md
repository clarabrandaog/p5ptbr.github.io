---
title: Git e GitHub
---
## Controle de Versão

Junto com uma boa IDE, outra ferramenta indispensável para trabalhar com código é um sistema de controle de versão.

Um sistema de controle de versão é fundamental quando começamos a trabalhar em projetos cada vez maiores com equipes cada vez maiores, mas eles também são muito úteis mesmo quando trabalhamos sozinhos em trabalhos e projetos.

Eles evitam que acabemos nesta situação:
<div class="scaled-images left w100">
  <img src="{{ '/assets/images/intro/git-00.jpg' | relative_url }}">
</div>

Eles fazem isso registrando todas as mudanças em nossos arquivos, e oferecendo mecanismos para voltarmos no tempo a qualquer versão do nosso código salva no histórico.

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/intro/git-01.jpg' | relative_url }}">
</div>

Isso nos dá outro nível de *salvamento* dos nossos arquivos. Um que não só nos permite ver depois o que foi adicionado, removido ou modificado em cada um dos nossos arquivos, mas também quando e por quem. Também tem formas de agrupar as mudanças feitas em arquivos diferentes, para que possamos acompanhar melhor como as diferentes partes de um projeto se relacionam.

Podemos, com certeza, usar um sistema de controle de versão sozinhos para acompanhar as mudanças em nossos arquivos, mas se estivermos trabalhando em um projeto com outra pessoa que também esteja usando um sistema de controle de versão em seu computador, podemos usar o controle de versão para sincronizar as mudanças que fazemos nos arquivos compartilhados.

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/intro/git-02.jpg' | relative_url }}">
</div>

Um sistema de controle de versão também nos ajuda a acompanhar diferentes versões do nosso código. Além de sempre podermos voltar a algum ponto do histórico do nosso código, também podemos *ramificar* (branch) nosso histórico e manter versões paralelas dos nossos arquivos.

Isso pode ser muito útil quando estamos experimentando e testando diferentes estratégias e métodos para implementar um algoritmo ou procedimento. Um sistema de controle de versão vai nos ajudar a alternar entre essas versões sem perder nenhuma informação.

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/intro/git-03.jpg' | relative_url }}">
</div>

## Git

O sistema de controle de versão mais popular, robusto, social e cheio de recursos hoje se chama [`git`](https://git-scm.com/):

<div class="scaled-images left w75">
  <img src="https://cdn.stackoverflow.co/images/jo7n4k8s/production/c52f86a87dfbcaa1bf9674c6e9ca55f7bb446afe-890x188.png">
</div>

É o que a maioria das empresas, equipes e indivíduos usa, não só para código, mas cada vez mais para vários tipos de arquivos baseados em texto. Sua popularidade provavelmente não é por acaso, estando ligada à popularidade do GitHub, a plataforma web de hospedagem/rede social baseada em nuvem para projetos `git`, mas vamos chegar ao GitHub em seguida.

Vamos começar olhando apenas para o `git`, o software de controle de versão, rodando em nossas máquinas locais.

A maioria dos Macs e computadores Linux já vem com uma versão do `git` instalada. Para computadores que não têm o `git`, ele pode ser baixado e instalado a partir do [site](https://git-scm.com/downloads) oficial do `git`. Esse é o método clássico de instalar o `git`, que nos permite usá-lo por meio de comandos de texto em um terminal, sem uma interface gráfica.

A forma mais fácil de instalar e começar a usar o `git` é usando o [GitHub Desktop App](https://desktop.github.com/). E já que, eventualmente, vamos querer publicar nossos projetos na web, essa também é a forma mais fácil de conectar os arquivos do nosso projeto local ao serviço do GitHub.

Este vídeo mostra rapidamente como baixar e instalar o App em um Mac:

{% include video.html url="intro/git-video-00.webm" %}

## Fluxo de Trabalho Básico

Agora que temos o `git` instalado, vamos ver como configurá-lo e usá-lo em um projeto.

Primeiro, vamos usar o gerenciador de arquivos do nosso computador para criar um diretório vazio para o nosso projeto, e depois vamos adicionar esse diretório à nossa *IDE*:

{% include video.html url="intro/git-video-01.webm" %}

Em seguida, vamos adicionar dois arquivos ao nosso projeto, um chamado `index.html` e outro chamado `sketch.js`, e começar a trabalhar no nosso projeto:

{% include video.html url="intro/git-video-02.webm" %}

O conteúdo exato desses arquivos não é muito importante agora. Só queremos algo que possamos adicionar ao controle de versão. Poderíamos muito bem ter começado com algum projeto que já tivéssemos no computador.

Agora, vamos mudar para o app do GitHub para *inicializar* nosso *repositório*. *Repositório* é só uma palavra chique para nossa pasta/diretório/projeto depois que ele é adicionado ao controle de versão. Uma coisa a observar: ao selecionar o `Local Path` do nosso repositório, queremos selecionar o diretório pai do nosso projeto existente, e NÃO o diretório do projeto em si. No nosso exemplo, selecionamos a pasta `Creative-Coding`, e não a pasta `HW01`.

Assim que o app inicializa nosso repositório, ele também adiciona e faz o *commit* automaticamente dos nossos dois arquivos (`index.html` e `sketch.js`) no histórico de controle de versão. Um *commit* é um conjunto de mudanças que fica registrado permanentemente no histórico do nosso repositório.

Se selecionarmos o *commit* e olharmos o conteúdo dos arquivos, todas as linhas vão aparecer sombreadas em verde, indicando que são linhas novas adicionadas ao histórico do nosso repositório:

{% include video.html url="intro/git-video-03.webm" %}

Se agora fizermos mudanças nos nossos arquivos, vamos ver que tanto a nossa IDE quanto o app do GitHub percebem essas mudanças. A IDE vai adicionar marcas verticais ao lado das linhas de código modificadas, e o app do GitHub vai listar nosso arquivo e destacar suas modificações na aba `Changes`.

Ele destaca novamente em verde as linhas que foram adicionadas, e agora também destaca em vermelho a linha vazia que foi "removida" quando adicionamos o novo código.

Podemos adicionar essas mudanças ao histórico permanente do nosso repositório escrevendo uma pequena nota descrevendo as mudanças na caixa de texto na parte inferior esquerda do app, e clicando no botão `Commit to main`.

Se agora conferirmos o histórico, vamos ver que temos um novo commit, que mostra exatamente o que mudamos:

{% include video.html url="intro/git-video-04.webm" %}

Se cometermos um erro e criarmos um commit antes do previsto, ou se acidentalmente adicionarmos código errado a um commit, sempre podemos *desfazer* o último commit. Isso vai restaurar nosso código para como ele estava imediatamente antes do commit, nos permitindo voltar à nossa IDE para corrigir o que for necessário antes de atualizar a mensagem do commit (se necessário) e fazer o commit novamente:

{% include video.html url="intro/git-video-05.webm" %}

## Branches (Ramificações)

Vamos ver como criar *branches* (ramificações) paralelas do nosso projeto para experimentar e testar diferentes versões do nosso código.

Vamos começar com um commit *base* comum, onde estamos usando fundo `black` (preto) e preenchimentos `white` (branco) no nosso projeto:

{% include video.html url="intro/git-video-06.webm" %}

Agora vamos criar uma *branch* do nosso projeto onde podemos experimentar com cores diferentes.

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/intro/git-03.jpg' | relative_url }}">
</div>

Vamos chamar a nova branch de `bright-colors`. Assim que ela for criada, todo o histórico da branch `main` até esse ponto será copiado para a nova branch, mas, a partir daí, elas provavelmente terão históricos de commits diferentes. Depois de estarmos na nova branch, qualquer mudança que fizermos commit só será adicionada ao histórico dessa branch:

{% include video.html url="intro/git-video-07.webm" %}

Podemos ter várias branches paralelas, começando a partir do mesmo commit *base*, ou podemos ramificar a partir de outras branches.

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/intro/git-04.jpg' | relative_url }}">
</div>

No nosso exemplo, vamos voltar para a branch `main` e criar uma terceira branch chamada `pastel-colors` para experimentar outra paleta de cores. O fluxo de trabalho é o mesmo de antes: criar a branch, mudar o código, fazer commit no novo histórico:

{% include video.html url="intro/git-video-08.webm" %}

Agora temos acesso às três versões do nosso código. Nada se perdeu, não precisamos dar nomes engraçados aos nossos arquivos, e todas as mudanças estão documentadas nos históricos de commits.

Criar branches também pode ser útil ao adicionar funcionalidades ou fazer mudanças extensas e complexas em uma base de código grande, porque elas criam um espaço separado onde podemos acompanhar facilmente nosso progresso e garantir que não estamos mudando partes do código que não têm relação com nossa tarefa.

De qualquer forma, depois de implementarmos novas funcionalidades ou experimentarmos várias opções para o nosso projeto, podemos *mesclar* (merge) uma branch em outra, basicamente combinando seus históricos em um histórico único.

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/intro/git-05.jpg' | relative_url }}">
</div>

No nosso exemplo, depois de explorar e testar nossas três paletas de cores, digamos que decidimos que a paleta `bright-colors` é a melhor e queremos que ela se torne a versão `main` do nosso código. Só precisamos usar a opção de merge para trazer as mudanças de `bright-colors` para `main`:

{% include video.html url="intro/git-video-09.webm" %}

## Conflitos

Quando os históricos de duas branches divergem demais, ou quando exatamente as mesmas linhas de código são alteradas em branches diferentes, podem ocorrer *conflitos* que impedem um merge, porque o `git` não vai saber como combinar essas mudanças automaticamente.

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/intro/git-06.jpg' | relative_url }}">
</div>

Conflitos são mais comuns quando trabalhamos com outras pessoas, mas também podem acontecer entre branches quando trabalhamos sozinhos.

No nosso exemplo, se tentarmos mesclar a branch `pastel-colors` na `main` depois de já termos mesclado `bright-colors`, vamos ter um conflito, porque essas duas branches alteraram exatamente as mesmas linhas de código. Nessa situação, o `git` vai interromper o merge e pedir para *resolvermos* o conflito antes de continuar. Nossa IDE tem uma interface integrada bem prática para selecionar qual conjunto de mudanças queremos manter. Depois disso, voltamos ao app do GitHub para finalizar o merge:

{% include video.html url="intro/git-video-10.webm" %}

## Revisão do Git

- `Repositório`: nosso projeto. O conjunto de arquivos sob controle de versão e seu histórico de mudanças.
- `Commit`: um único ponto do nosso histórico, composto por mudanças relacionadas e uma mensagem descritiva.
- `Histórico`: coleção de commits.
- `Branch`: uma versão separada do repositório, com seu próprio histórico. Útil para acompanhar e testar diferentes versões do nosso código, ou para implementar mudanças grandes e complexas separadamente da versão principal do código.
- `Merge`: combinar os históricos de duas branches.

## GitHub

Estivemos vendo como usar o `git` localmente no nosso computador, sozinhos. Outro benefício do software de controle de versão é que ele nos permite colaborar facilmente com outras pessoas compartilhando os arquivos do nosso projeto.

Já podemos imaginar como um sistema que mantém várias versões dos nossos arquivos e acompanha todo o seu histórico de mudanças pode ser útil ao trabalhar com outras pessoas. Só precisamos de uma forma de conectar nosso repositório local aos repositórios de outras pessoas.

E é exatamente isso que o [GitHub](https://github.com/) faz.

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/intro/github-00.jpg' | relative_url }}">
</div>

O GitHub é um dos poucos serviços de hospedagem de repositórios online. Outros serviços incluem o [GitLab.com](https://gitlab.com/), o [Bitbucket](https://bitbucket.org/) e o [CodeCommit](https://aws.amazon.com/codecommit/), mas o GitHub é, de longe, o mais completo, o mais usado e o mais fácil de começar a usar. E, como veremos, o `git` facilita usarmos vários serviços remotos ao mesmo tempo, se quisermos:

<div class="scaled-images left w75">
  <img src="{{ '/assets/images/intro/github-01.jpg' | relative_url }}">
</div>

Antes de podermos hospedar nosso repositório no GitHub, precisamos criar uma conta no GitHub.

<div style="width:100%; position:relative; overflow-y:hidden; padding-bottom:51.5%;">
  <div style="position:absolute; top:-5%">
    {% include video.html url="intro/github-video-00.webm" %}
  </div>
</div>

Agora podemos pedir ao app do GitHub para publicar nosso repositório no GitHub. Ele vai pedir para permitirmos que o app do GitHub acesse nossa conta, e depois de alguns cliques vamos ver nosso repositório online:

{% include video.html url="intro/github-video-01.webm" %}

## Fluxo de Trabalho Básico no GitHub

O fluxo de trabalho para trabalhar em um repositório compartilhado é bem parecido.

A principal diferença é que, toda vez que sentamos para trabalhar no nosso projeto, antes de escrever qualquer código novo, devemos sempre fazer *fetch* e *pull* da versão atual do nosso repositório *remoto*. Isso traz quaisquer mudanças feitas por outras pessoas, que podemos ver ao olhar o histórico de commits do projeto. Na verdade, essas são duas ações separadas: primeiro, o *fetch* verifica o repositório remoto e nos avisa se nossa cópia local está diferente da cópia remota, e como. Se houver novos commits no repositório remoto, podemos optar por fazer o *pull* e incorporá-los ao nosso histórico local.

{% include video.html url="intro/github-video-02.webm" %}

Por outro lado, uma vez que estamos no fluxo, escrevendo código, alterando arquivos e fazendo commit no nosso repositório local, também devemos sempre lembrar de fazer *push* das nossas mudanças locais para o repositório *remoto*. Assim, outras pessoas têm acesso à versão mais recente do projeto. Só precisamos lembrar que primeiro fazemos commit localmente, e depois, após alguns commits locais, enviamos todas as novas mudanças para o repositório remoto com um único *push*.

{% include video.html url="intro/github-video-03.webm" %}

## Fluxo de Trabalho Inteligente no GitHub

Mesmo com o `git` mantendo o registro do nosso histórico compartilhado e nos permitindo voltar a qualquer commit do nosso passado, agora que potencialmente colaboramos com centenas de pessoas em nossos projetos, as chances de conflitos de merge e outros tipos de confusão são bem altas.

Uma estratégia para evitar quebrar o projeto inteiro com um commit ruim, ou até mesmo sobrescrever sem querer o código de outras pessoas, é manter um conjunto de branches para diferentes estágios do nosso projeto.

Podemos ter uma branch `production`, com código que sempre funciona. Esse é o código que pessoas que não trabalham no nosso projeto podem baixar e usar. Ou pode ser o código de um site que está no ar.

Também podemos ter uma branch `dev`, onde todo mundo mescla seu código conforme trabalha em diferentes funcionalidades e adições ao projeto. Esse código nem sempre funciona. Pode ter funcionalidades parcialmente implementadas, coisas que não foram testadas, placeholders etc., mas essa branch é importante porque é onde os grandes conflitos são resolvidos.

E entre as branches `production` e `dev`, podemos ter uma branch `test` ou `staging`, que contém código quase pronto para ir para `production`, mas que ainda precisa ser testado ou integrado com outros serviços antes.

E agora sempre trabalhamos na branch `dev`. Código, salvar, commit, código, salvar, commit, push, código, commit, push... etc, etc, etc.

De vez em quando, a `dev` é mesclada na `test`, o código é testado, os erros são corrigidos, e, eventualmente, a `test` é mesclada na `prod`, e quaisquer mudanças feitas diretamente na `test` são ramificadas de volta para a `dev`.

<div class="scaled-images left w100">
  <img src="{{ '/assets/images/intro/github-02.jpg' | relative_url }}">
</div>

## Pull Requests

Além dessas branches compartilhadas predefinidas, também é uma boa ideia usar uma branch `feature` separada e pessoal para implementar qualquer mudança grande no projeto. Assim, sabemos exatamente como estavam o código e o histórico compartilhados quando começamos nossas mudanças, e não precisamos nos preocupar em resolver conflitos enquanto trabalhamos nessas mudanças grandes.

Assim que terminarmos de implementar nossa incrível funcionalidade, vamos fazer *pull* de quaisquer mudanças da branch `dev` remota para a nossa branch `feature`, resolver o merge e corrigir eventuais conflitos por lá, e então estaremos prontos para mesclar nossas mudanças na `dev` e fazer o *push* de volta para o repositório *remoto*.

Mas vamos fazer algo diferente. Em vez de mesclar nossas mudanças diretamente na `dev`, vamos fazer push da nossa branch `feature` para o repositório remoto e abrir um *pull request*.

Podemos pensar em um pull request como um merge colaborativo. Escolhemos quais branches queremos mesclar, e o GitHub nos dá uma interface onde toda a equipe pode verificar possíveis problemas, discutir questões sobre o código, dar feedback etc.

Assim que as pessoas estiverem satisfeitas com as mudanças propostas, alguém mescla o pull request da nossa branch `feature` na branch `dev` compartilhada.

{% include video.html url="intro/github-video-04.webm" %}

Isso pode parecer muito trabalho de clicar aqui e ali, mas, para projetos grandes com equipes grandes, é importante ter estratégias assim para manter todas as versões do nosso código organizadas e nosso projeto funcionando.

## Revisão do GitHub

- `Remote`: a cópia compartilhada do nosso repositório, hospedada online.
- `Fetch`: buscar uma lista de possíveis mudanças do repositório remoto.
- `Pull`: mesclar mudanças do repositório remoto no nosso repositório local.
- `Push`: mesclar mudanças do nosso repositório local no repositório remoto compartilhado.
- `Pull Request`: uma proposta de merge.

## Recursos Adicionais
- [W3Schools](https://www.w3schools.com/git/git_intro.asp?remote=github)
- [GitHub](https://docs.github.com/en/get-started/start-your-journey/about-github-and-git)
- [Interactive Git training materials](https://githubtraining.github.io/training-manual/#/01_getting_ready_for_class)
- [GitHub's Learning Lab](https://lab.github.com/)
