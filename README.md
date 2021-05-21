# Curso Docker Alura

Índice
1. [Introdução a Docker](#introdução-a-docker)
    1. [Introdução](#intrudução)
    2. [O problema das máquinas virtuais](#o-problema-das-máquinas-virtuais)
        1. [A evolução do host de aplicações](#a-evolução-do-host-de-aplicações)
        2. [Capacidade pouco aproveitada](#capacidade-pouco-aproveitada)
        3. [Virtualização, uma solução?](#virtualização-uma-solução)
    3. [A era dos containers](#a-era-dos-containers)
        1. [Vantagens de um container](#vantagens-de-um-container)
        2. [Necessidade do uso de containers](#necessidade-do-uso-de-containers)
    4. [Vantagens de um container](#vantagens-de-um-container-teste)
    5. [O que é o Docker?](#o-que-é-o-docker)
        1. [Docker, Inc.](#docker-inc)
        2. [As tecnologias do Docker](#as-tecnologias-do-docker)
        3. [Open Source](#open-source)
    6. [Instale o Docker Engine no Ubuntu](#instale-o-docker-engine-no-ubuntu)
    7. [Hello World](#hello-world)
    8. [Uma alternativa online](#uma-alternativa-online)
        1. [Utilizando o Play With Docker](#utilizando-o-play-with-docker)
    9. [O que aprendemos?](#o-que-aprendemos)
    10. [Teste de conhecimento](#teste-de-conhecimento) 


## Introdução a Docker
### Intrudução

Seja bem-vindo ao curso de Docker aqui da Alura!

Aqui, estudaremos sobre essa famosa tecnologia de containers, que até já foi adotada por grandes empresas, como PayPal, Microsoft, Spotify, entre outras.

No começo desse curso, entenderemos o que é o Docker, como funciona essa tecnologia e as diferenças em relação às máquinas virtuais, precursoras do Docker. Veremos por que os containers são mais leves, como eles podem enxugar os gastos da nossa empresa com servidores, e como agilizam o deploy da nossa aplicação.

Veremos também sobre o ciclo de vida dos containers, quais os principais comandos envolvidos quando queremos criar, dimensionar ou remover um container, e claro, tudo o que precisamos saber para trabalhar com Docker.

Também vamos falar como baixar imagens do Docker Hub, o repositório de imagens do Docker, e como criar a nossa própria, além de disponibilizá-la para outros desenvolvedores utilizarem.

Ainda mais, veremos sobre volumes, um dos principais modos para armazenar e persistir dados em um container e como realizar a comunicação entre containers.

Ao final do curso, construiremos uma aplicação, que será dividida em pequenas partes, em que cada parte será um serviço da nossa aplicação, e vamos utilizar o Docker Compose para administrar os diversos containers envolvidos nisso, além de subir tudo isso para a nuvem.

Preparado? Então vamos começar!

---

### O problema das máquinas virtuais
Antes de vermos sobre o Docker, precisamos saber mais como hospedamos aplicações e como surgiram os _**containers**_.

#### A evolução do host de aplicações
Antigamente, quando queríamos montar o nosso sistema, com vários serviços (bancos de dados, proxy, etc) e aplicações, acabávamos tendo vários servidores físicos, cada um com um serviço ou aplicação do nosso sistema, por exemplo:

![Um serviço em cada servidor](https://s3.amazonaws.com/caelum-online-public/646-docker/01/imagens/servicos-servidores.png)

E claro, não conseguimos instalar os serviços diretamente no hardware do servidor, ou seja, precisamos de um intermediário entre as aplicações e o hardware, que é o sistema operacional. Ou seja, devemos instalar sistemas operacionais em cada servidor, e os sistemas poderiam ser diferentes:

![Um sistema operacional em cada servidor](https://s3.amazonaws.com/caelum-online-public/646-docker/01/imagens/sistema-operacional.png)

E se quisermos que uma aplicação se comunique com outra ou faça qualquer comunicação externa, devemos conectar os servidores à rede. Além disso, para eles funcionarem, precisamos ligá-los à eletricidade. Logo, havia diversos custos de eletricidade, rede e configuração dos servidores.

Além disso, o processo era lento, já que a cada nova aplicação, deveríamos comprar/montar o servidor físico, instalar o sistema operacional, configurá-lo e subir a aplicação.

#### Capacidade pouco aproveitada
O que foi falado anteriormente não era o único problema desse tipo de arquitetura. Era muito comum termos servidores parrudos, com uma única aplicação sendo executada, para normalmente ficarem funcionando abaixo da sua capacidade, para quando for necessário, o servidor aguentar uma grande quantidade de acessos. Isso resultava em muita capacidade ociosa nos servidores, com muitos recursos desperdiçados.

#### Virtualização, uma solução?
Para fugir desses problemas de servidores ociosos e alto tempo e custo de subir e manter aplicações em servidores físicos, surgiu como solução a **virtualização**, surgindo assim as **máquinas virtuais**.

As máquinas virtuais foram possíveis de ser criadas graças a uma tecnologia chamada _**Hypervisor**_, que funciona em cima do sistema operacional, permitindo a virtualização dos recursos físicos do nosso sistema. Assim, criamos uma máquina virtual que tem acesso a uma parte do nosso HD, memória RAM e CPU, criando um computador dentro de outro:

![Máquina virtual](https://s3.amazonaws.com/caelum-online-public/646-docker/01/imagens/maquina-virtual.png)

E se temos uma máquina virtual que está acessando uma parte do nosso hardware como um todo, conseguimos colocar dentro dela uma das nossas aplicações. E replicar isso, criando mais máquinas virtuais com outras aplicações:

![Máquina virtual com aplicações](https://s3.amazonaws.com/caelum-online-public/646-docker/01/imagens/aplicacoes-maquinas-virtuais.png)

Assim, reduzimos a quantidade de servidores e consequentemente os custos com luz e rede. Além disso, dividimos melhor o nosso hardware, aproveitando melhor os seus recursos e diminuindo a ociosidade.

#### Problemas das máquinas virtuais
Apesar de resolver os problemas do uso de vários servidores físicos, as máquinas virtuais também possuem problemas. Para podermos hospedar a nossa aplicação em uma máquina virtual, também precisamos instalar um sistema operacional nela:

![Máquina virtual com aplicações](https://s3.amazonaws.com/caelum-online-public/646-docker/01/imagens/sistema-operacional-vms.png)

Cada aplicação necessita de um sistema operacional para poder ser executada, e esses sistemas podem ser diferentes. E apesar dos sistemas operacionais serem um software, eles possuem um custo de memória RAM, disco e processamento. Ou seja, precisamos de uma capacidade mínima para manter as funcionalidades de um sistema operacional, aumentando o seu custo de manutenção a cada sistema que tivermos.

Além disso, há um custo de configuração, isto é, liberar portas, instalar alguma biblioteca específica, etc, toda uma configuração que um sistema operacional pede. Também devemos sempre estar atentos à sua segurança, mantendo o sistema de cada máquina virtual sempre atualizado.

Muitas vezes, o tempo voltado para a manutenção das máquinas virtuais era o mesmo tempo voltado para a nossa aplicação em si. Ou seja, acabávamos dividindo o valor da nossa empresa, ao invés de focar somente nas aplicações, dividíamos o trabalho com a manutenção dos sistemas operacionais.

Então, como melhorar essa situação? Daí surgiram containers, que serão vistos no próximo vídeo.

---

### A era dos containers 
Um container funcionará junto do nosso sistema operacional base, e conterá a nossa aplicação, ou seja, a aplicação será executada dentro dele. Criamos um _container_ para cada aplicação, e esses _containers_ vão **dividir** as funcionalidades do sistema operacional:

![Containers](https://s3.amazonaws.com/caelum-online-public/646-docker/01/imagens/container.png)

Não temos mais um sistema operacional para cada aplicação, já que agora as aplicações estão dividindo o mesmo sistema operacional, que está em cima do nosso hardware. Os próprios containers terão a lógica que se encarregará dessa divisão.

Assim, com somente um sistema operacional, reduzimos os custos de manutenção e de infraestrutura como um todo.

#### Vantagens de um container
Por não possuir um sistema operacional, o container é muito mais leve e não possui o custo de manter múltiplos sistemas operacionais, já que só teremos um sistema operacional, que será dividido entre os containers.

Além disso, por ser mais leve, o container é muito rápido de subir, subindo em questão de segundos. Logo, o container é uma solução para suprir o problema de múltiplas máquinas virtuais em um hardware físico, já que com o container, nós dividimos o sistema operacional entre as nossas aplicações.

#### Necessidade do uso de containers
Mas por que precisamos dos containers, não podemos simplesmente instalar as aplicações no nosso próprio sistema operacional? Até por que já fazemos isso, já que no nosso sistema operacional temos um editor de texto, terminal, navegador, etc.

No caso das nossas aplicações, essa abordagem pode ter alguns problemas. Por exemplo, se dois aplicativos precisarem utilizar a mesma porta de rede? Precisaremos de algo para isolar uma aplicação da outra. E se uma aplicação consumir toda a CPU, a ponto de prejudicar o funcionamento das outras aplicações? Isso acontece se não limitarmos a aplicação. Outro problema que pode ocorrer é cada aplicação precisar de uma versão específica de uma linguagem, por exemplo, uma aplicação precisa do Java 7 e outra do Java 8. Além disso, uma aplicação pode acabar congelando todo o sistema. Por isso é bom ter essa separação das aplicações, isolar uma da outra, e isso pode ser feito com os **containers**.

Com os containers, conseguimos limitar o consumo de CPU das aplicações, melhorando o controle sobre o uso de cada recurso do nosso sistema (CPU, rede, etc). Também temos uma facilidade maior em trabalhar com versões específicas de linguagens/bibliotecas, além de ter uma agilidade maior na hora de criar e subir containers, já que eles são mais leves que as máquinas virtuais.

---

### O que é o Docker?
Agora que já vimos a diferença entre máquinas virtuais e containers, chegou a hora de introduzirmos o Docker nesse contexto, que se divide entre duas coisas: a Docker, Inc., empresa que toma conta do Docker, e a tecnologia dos containers.

#### Docker, Inc.
Primeiramente, devemos falar sobre a Docker, Inc., que no início era chamada de dotCloud. A dotCloud era uma empresa de PaaS (Platform as a Service), sendo responsável pela hospedagem da nossa aplicação, levantando o servidor, configurando-o, liberando portas, etc, fazendo tudo o que é necessário para subir a nossa aplicação. Outros exemplos de empresas de PaaS são o Heroku, Microsoft Azure e Google Cloud Platform.

Inicialmente, para prover a parte de infraestrutura, a dotCloud utilizava o Amazon Web Services (AWS), serviço que nos disponibiliza máquinas virtuais e físicas para trabalharmos. E para hospedar uma aplicação, sabemos que precisamos do sistema operacional, mas a dotCloud introduziu o conceito de containers na hora de subir uma aplicação, dando origem ao Docker, tecnologia utilizada para baratear o custo de hospedar várias aplicações em uma mesma máquina.

Ou seja, quando a dotCloud criou o Docker, sua intenção era economizar os gastos da empresa, subindo várias aplicações em containers, em um mesmo hardware do AWS, e com o passar do tempo a empresa percebeu que tinham muitos desenvolvedores interessados na tecnologia que ela havia criado, a tecnologia que permite a criação de containers, que faz o intermédio entre eles e o sistema operacional, o Docker.

#### As tecnologias do Docker
O Docker nada mais é do que uma coleção de tecnologias para facilitar o deploy e a execução das nossas aplicações. A sua principal tecnologia é a Docker Engine, a plataforma que segura os containers, fazendo o intermédio entre o sistema operacional.

Outras tecnologias do Docker que facilitam a nossa vida e que veremos neste curso são o Docker Compose, um jeito fácil de definir e orquestrar múltiplos containers; o Docker Swarm, uma ferramenta para colocar múltiplos docker engines para trabalharem juntos em um cluster; o Docker Hub, um repositório com mais de 250 mil imagens diferentes para os nossos containers; e a Docker Machine, uma ferramenta que nos permite gerenciar o Docker em um host virtual.

#### Open Source
Quando a empresa dotCloud tornou-se a Docker, Inc., focada em manter o Docker, ela o abriu para o mundo open source, tudo disponibilizado no seu [GitHub](https://github.com/docker), inclusive com várias empresas contribuindo para o desenvolvimento dessa tecnologia.

Apesar de haver alguns serviços pagos, em sua grande parte a tecnologia do Docker é uma tecnologia open source, utilizada por várias empresas. Então, vamos colocar as mãos na massa e aprender a instalar o Docker nas próximas aulas.

---

### Instale o Docker Engine no Ubuntu
[Documentação Oficial](https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script)

---

### Hello World
Com o Docker instalado no nosso sistema operacional (seja ele qual for), já podemos testá-lo para ver o seu funcionamento.

Se o nosso Docker foi instalado pelo Docker for Mac ou Docker for Windows, conseguimos executar os seus comandos através do terminal nativo do Mac ou do Windows (Prompt de Comando). Mas se o nosso Docker foi instalado pelo Docker Toolbox, devemos executar os seus comandos através do Docker Quickstart Terminal, terminal que foi instalado pelo próprio Docker Toolbox.

Então, vamos abrir um terminal que consiga se comunicar com o nosso Docker, e executar o seguinte comando para verificar a sua versão:

```docker version```

Também podemos executar o clássico Hello World:

```docker run hello-world```

o Docker não conseguiu achar a imagem localmente, e ele foi em algum lugar e a baixou. Como assim? Quando executamos o comando docker run hello-world, estamos dizendo para o Docker criar um container com a imagem do hello-world. Como não possuímos essa imagem localmente, ele foi buscá-la no Docker Hub, repositório do próprio Docker com várias imagens para utilizarmos em nossos projetos.

O nosso Docker local entrou em contato com a Docker Engine, que por sua vez baixou a imagem hello-world do Docker Hub, criou um container com ela e a executou. Após isso, a saída é impressa para nós e a imagem é encerrada.

Esses passos descritos, imagem, container, seus ciclos de vida, tudo isso veremos ao longo dos próximos capítulos.

---

### Uma alternativa online
#### Utilizando o Play With Docker
Não se desespere caso você tenha tido algum problema em instalar o Docker em sua máquina, você também tem a opção de utilizar o [Play With Docker](https://labs.play-with-docker.com/). Nele você poderá utilizar os comandos vistos daqui em diante e testar as diversas funcionalidades que o Docker proporciona. Ao acessar o site basta clicar em +Add New Instance e começar a utilizá-lo como estivesse usando sua máquina normalmente.

---

### O que aprendemos?
Nessa aula você:
- conheceu a ideia de virtualização,
- entendeu o conceito de containers,
- descobriu o que é o Docker e suas principais tecnologias,
- instalou o Docker no sistema operacional,
- e o testou através de uma imagem Hello, World, que foi baixada do Docker Hub

Segue também uma breve lista dos comandos utilizados:

- ```docker version``` - exibe a versão do docker.
- ```docker run NOME_DA_IMAGEM ```- cria um container com a respectiva imagem passada como parâmetro.

---

### Teste de conhecimento
