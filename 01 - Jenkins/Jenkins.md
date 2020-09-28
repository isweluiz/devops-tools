Olá pessoal, espero que estejam bem! Este é o primeiro post da série semana DevOps, hoje iremos conhecer o jenkins, com um breve resumo e logo em seguida partiremos para a prática(hands-on)... 

#### ***Acesse esse post no meu [GitHub](https://github.com/isweluiz)***

## Resumo
- ## Tópicos
- - Oque é o Jenkins? 
- - Hudson e Jenkins
- - Automação Jenkins
- - Como funciona o Jenkins
- - Oque é uma Pipelines Jenkins
- - - Por que Pipeline?
- - - Conceitos de pipeline
- - Instalação do Jenkins
- - Jenkins para CI / CD


# Oque é o Jenkins? 
> Jenkins oferece uma maneira simples de configurar uma integração contínua e ambiente de entrega contínua para quase qualquer combinação de linguagens e repositórios de código-fonte

Jenkins oferece uma maneira simples de configurar um ambiente de integração contínua ou entrega contínua  ( CI / CD ) para quase qualquer combinação de linguagens e repositórios de código-fonte usando pipelines, além de automatizar outras tarefas de desenvolvimento de rotina. Embora o Jenkins não elimine a necessidade de criar scripts para etapas individuais, ele oferece uma maneira mais rápida e robusta de integrar toda a sua cadeia de ferramentas de construção, teste e implantação do que você pode construir facilmente.

![Integração contínua](https://i.ibb.co/StNQvnG/wigait-100720449-large.jpg)

"Don't break the night building!"  é uma regra fundamental nas lojas de desenvolvimento de software que publicam uma versão diária do produto recém-construída para seus testadores. Antes do Jenkins, o melhor que um desenvolvedor poderia fazer para evitar quebrar a compilação noturna era compilar e testar cuidadosamente e com sucesso em uma máquina local antes de confirmar o código. Mas isso significava testar as mudanças de uma pessoa isoladamente, sem os compromissos diários de todos os outros. Não havia nenhuma garantia firme de que a construção noturna sobreviveria ao commit.

## Hudson e Jenkins

Em 2004, Kohsuke Kawaguchi foi desenvolvedor Java na Sun. Kawaguchi se cansou de quebrar compilações em seu trabalho de desenvolvimento e queria encontrar uma maneira de saber, antes de enviar o código para o repositório, se o código iria funcionar. Então Kawaguchi construiu um servidor de automação em e para Java para tornar isso possível, chamado Hudson. Hudson se tornou popular na Sun e se espalhou para outras empresas como código aberto.

Em 2011, uma disputa entre a Oracle (que havia adquirido a Sun) e a comunidade independente de código aberto Hudson levou a uma bifurcação com a mudança de nome, Jenkins . Em 2014, Kawaguchi se tornou CTO da CloudBees, que oferece produtos de entrega contínua baseados em Jenkins.

Ambos os garfos continuaram existindo, embora Jenkins fosse muito mais ativo. Hoje, o projeto Jenkins ainda está ativo. O site da Hudson foi fechado em 31 de janeiro de 2020.

Em março de 2019, a Linux Foundation, juntamente com a CloudBees, o Google e várias outras empresas, lançou uma nova fundação de software de código aberto chamada Continuous Delivery Foundation ( CDF ). Os colaboradores do Jenkins decidiram que seu projeto deveria se juntar a esta nova fundação. Kawaguchi escreveu na época que nada de significativo mudaria para os usuários.

Em janeiro de 2020, Kawaguchi anunciou que estava mudando para sua nova startup, a Launchable . Ele também disse que estaria oficialmente se afastando de Jenkins, embora permanecesse no Comitê de Supervisão Técnica da Fundação de Entrega Contínua, e mudaria sua função na CloudBees para um consultor.

##  **Vídeo relacionado:** Como fornecer código mais rápido com CI / CD

[![Como fornecer código mais rápido com CI / CD](http://img.youtube.com/vi/Rq5TQlPyr8g/0.jpg)](http://www.youtube.com/watch?v=Rq5TQlPyr8g "Como fornecer código mais rápido com CI / CD")

## Automação Jenkins

Hoje, Jenkins é o servidor de automação de código aberto líder com cerca de 1.600 plug-ins para oferecer suporte à automação de todos os tipos de tarefas de desenvolvimento. O problema que Kawaguchi estava tentando resolver originalmente, integração contínua e entrega contínua de código Java (ou seja, construção de projetos, execução de testes, análise de código estático e implantação) é apenas um dos muitos processos que as pessoas automatizam com o Jenkins. Esses 1.600 plug-ins abrangem cinco áreas: plataformas, UI, administração, gerenciamento de código-fonte e, mais frequentemente, gerenciamento de construção.

## Como funciona o Jenkins

Jenkins é distribuído como um arquivo WAR e como pacotes de instalação para os principais sistemas operacionais, como um pacote Homebrew, como uma imagem Docker e como código-fonte . O código-fonte é principalmente Java, com alguns arquivos Groovy, Ruby e Antlr.

Você pode executar o WAR do Jenkins autônomo ou como um servlet em um servidor de aplicativos Java, como o Tomcat. Em ambos os casos, ele produz uma interface de usuário da web e aceita chamadas para sua API REST.

Quando você executa o Jenkins pela primeira vez, ele cria um usuário administrativo com uma senha longa e aleatória, que você pode colar em sua página inicial da Web para desbloquear a instalação.

## Oque é uma Pipelines Jenkins?
***Jenkins Pipeline (ou simplesmente "Pipeline" com "P" maiúsculo) é um conjunto de plug-ins que suporta a implementação e integração de pipelines de entrega contínua no Jenkins.***

***Um pipeline é um modelo definido pelo usuário de um pipeline de CD. O código de um Pipeline define todo o seu processo de construção, que normalmente inclui estágios para construir um aplicativo, testá-lo e, em seguida, entregá-lo.***

Um pipeline de entrega contínua (CD) é uma expressão automatizada do seu processo para obter o software do controle de versão direto para seus usuários e clientes. Cada mudança em seu software  (committed in source control) passa por um processo complexo para ser liberada. Esse processo envolve a construção do software de maneira confiável e repetível, bem como o progresso do software construído (chamado de "construção") por meio de vários estágios de teste e implantação.

O Pipeline fornece um conjunto extensível de ferramentas para modelar pipelines de entrega simples a complexos "como código" por meio da sintaxe de linguagem específica de domínio [DSL](https://www.jenkins.io/doc/book/pipeline/syntax) do Pipeline .

A definição de um Jenkins Pipeline é escrita em um arquivo de texto (denominado a Jenkinsfile) que, por sua vez, pode ser confirmado em um repositório de controle de origem do projeto. Esta é a base de "Pipeline-as-code"; tratar o pipeline do CD como uma parte do aplicativo a ser versionada e revisada como qualquer outro código.

Criar um Jenkinsfilee comprometê-lo com o controle de origem fornece uma série de benefícios imediatos:

- Cria automaticamente um processo de construção de Pipeline para todas as ramificações e solicitações pull.

- Revisão / iteração de código no pipeline (junto com o código-fonte restante).

- Trilha de auditoria para o pipeline.

- Fonte única de verdade [ 3 ] para o Pipeline, que pode ser visualizada e editada por vários membros do projeto.

Embora a sintaxe para definir um Pipeline, na IU da web ou com um, Jenkinsfile seja a mesma, geralmente é considerada a prática recomendada definir o Pipeline em a Jenkinsfile e fazer o check-in no controle de origem.

## Por que Pipeline?
Jenkins é, fundamentalmente, um mecanismo de automação que suporta vários padrões de automação. O Pipeline adiciona um conjunto poderoso de ferramentas de automação ao Jenkins, dando suporte a casos de uso que vão desde a integração contínua simples até pipelines de CD abrangentes. Ao modelar uma série de tarefas relacionadas, os usuários podem tirar proveito dos muitos recursos do Pipeline:

- Código : os pipelines são implementados no código e normalmente verificados no controle de origem, dando às equipes a capacidade de editar, revisar e iterar em seu pipeline de entrega.

- Durável : os pipelines podem sobreviver a reinicializações planejadas e não planejadas do mestre Jenkins.

- Pausável : os pipelines podem, opcionalmente, parar e aguardar a entrada ou aprovação humana antes de continuar a execução do pipeline.

- Versátil : os pipelines oferecem suporte a requisitos complexos de CD do mundo real, incluindo a capacidade de fazer fork / join, loop e realizar trabalho em paralelo.

- Extensível : O plugin Pipeline suporta extensões personalizadas para seu DSL e múltiplas opções para integração com outros plugins.

Embora Jenkins sempre tenha permitido formas rudimentares de encadear Jobs de Freestyle para executar tarefas sequenciais, Pipeline torna esse conceito um cidadão de primeira classe em Jenkins.

Com base no valor central de extensibilidade do Jenkins, o Pipeline também é extensível tanto por usuários com [Bibliotecas compartilhadas](https://www.jenkins.io/doc/book/pipeline/shared-libraries) Pipeline quanto por desenvolvedores de plug-ins.

O fluxograma abaixo é um exemplo de um cenário de CD facilmente modelado no Jenkins Pipeline:

![Fluxograma](https://www.jenkins.io/doc/book/resources/pipeline/realworld-pipeline-flow.png)

## Conceitos de pipeline

Os conceitos a seguir são aspectos-chave do Jenkins Pipeline, que estão intimamente ligados à sintaxe do Pipeline.

### Pipeline

Um pipeline é um modelo definido pelo usuário de um pipeline de CD. O código de um Pipeline define todo o seu processo de construção, que normalmente inclui estágios para construir um aplicativo, testá-lo e, em seguida, entregá-lo.
Além disso, um pipeline stage é uma parte fundamental da [sintaxe declarativa do pipeline](https://www.jenkins.io/doc/book/pipeline/#declarative-pipeline-fundamentals).

### Node
Um nó é uma máquina que faz parte do ambiente Jenkins e é capaz de executar um Pipeline.
Além disso, um node bloco é uma [parte fundamental da sintaxe do Pipeline com script](https://www.jenkins.io/doc/book/pipeline/#scripted-pipeline-fundamentals).

### Stage
Um stage bloco define um subconjunto conceitualmente distinto de tarefas realizadas por todo o Pipeline (por exemplo, estágios "Build", "Test" e "Deploy"), que é usado por muitos plug-ins para visualizar ou apresentar o status /progresso do Jenkins Pipeline.

### Step
Uma única tarefa. Fundamentalmente, uma etapa diz a Jenkins o que fazer em um determinado momento (ou "etapa" do processo). Por exemplo, para executar o comando shell make uso da sh etapa: sh 'make'. Quando um plugin estende o Pipeline DSL, normalmente significa que o plugin implementou uma nova etapa

Leia mais [clicando aqui](https://www.jenkins.io/doc/book/pipeline/)


## SO utilizado no guia CentOS 7.8 na AWS

```bash
[isweluiz@jenkins ~]$ cat /etc/redhat-release 
CentOS Linux release 7.8.2003 (Core)
```

## Instalação do Jenkins: 

### Script de instalação resumido

```bash
#!/bin/bash
sudo yum install -y java-1.8.0-openjdk-devel
sudo yum install -y wget
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
sudo yum install -y jenkins
systemctl enable jenkins ; systemctl start jenkins
sudo yum install nginx -y
sudo setsebool -P httpd_can_network_connect 1
sudo setsebool -P httpd_can_network_relay 1
```

### Install java-1.8.0-openjdk-devel
> sudo yum install -y java-1.8.0-openjdk-devel

### Install wget:
> sudo yum install -y wget

### Download the repo:
> sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo

### Import the required key:
> sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key

### Install Jenkins:
> sudo yum install -y jenkins

### Enable eand start jenkins 
> systemctl enable jenkins ; systemctl start jenkins

### Acesse o IP do host na porta 8080

http://54.163.0.252:8080/login?from=%2F

### Acesse o diretório mencionado na página de login para desbloquear o jenkins
> cat /var/lib/jenkins/secrets/initialAdminPassword
    43fcfd0931ac49e4b1cf39713170a581

![Unlock Jenkins](https://i.imgur.com/bhDRypE.png)

### Crie seu usuário na próxima tela

![Create user](https://i.imgur.com/iYuXOy1.png)



## Plugins Jenkins

![Plugins install](https://i.imgur.com/VTCphW5.png)

## Jenkins Dashboard 

![Jenkins Dashboard](https://i.imgur.com/vDe0G75.png)


## Instalando nginx para fazer um proxy_pass 

> sudo yum install nginx -y 

## Adicione a linha abaixo no arquivo de configuração do nginx dentro do bloco server {}

> proxy_pass http://localhost:8080; 

location / {

        proxy_pass http://127.0.0.1:8080;
}

## Habilitando o roteamento de porta no SElinux
> sudo setsebool -P httpd_can_network_connect 1

> sudo setsebool -P httpd_can_network_relay 1

##  Reinicie o nginx 
> systemctl restart nginx 


### Jenkins para CI / CD
No geral, o Jenkins oferece uma maneira simples de configurar um ambiente de CI / CD para praticamente qualquer combinação de linguagens e repositórios de código-fonte usando pipelines, bem como automatizar uma série de outras tarefas de desenvolvimento de rotina. Embora o Jenkins não elimine a necessidade de criar scripts para etapas individuais, ele oferece uma maneira mais rápida e robusta de integrar sua cadeia inteira de ferramentas de construção, teste e implantação do que você poderia construir facilmente.




### Fontes do resumo teórico

[infoworld](https://www.infoworld.com/article/3239666/what-is-jenkins-the-ci-server-explained.html)

[jenkins.io](https://www.jenkins.io/doc/book/pipeline/)

[linuxacademy](https://linuxacademy.com/cp/courses/lesson/course/4255/lesson/3/module/348/)