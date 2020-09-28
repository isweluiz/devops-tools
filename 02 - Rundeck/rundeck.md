Olá pessoal, espero que estejam bem! Este é o segundo post da série semana DevOps, hoje iremos conhecer o rundeck, com um breve resumo e logo em seguida partiremos para a prática(hands-on)... 

#### ***Acesse esse post no meu [GitHub](https://github.com/isweluiz)***

## Resumo
 ## Tópicos
- O que é o Rundeck? 
- O Rundeck é gratuito?
- - Rundeck Enterpise (fka Rundeck Pro)
- Recursos destaques do Rundeck
- Como o Rundeck difere do Jenkins?
- O Jenkins pode integrar com o Rundeck? 
- Instalação do rundeck
- Criação de um projeto
- Adicionando um node 
- Executando scripts 
- Registro de logs

![Rundeck](https://devopsinstitute.com/wp-content/uploads/2020/03/rundeck-logo.png)

## O que é o Rundeck? 

Rundeck é uma ferramenta de código aberto que ajuda a definir, implantar e gerenciar automação. Ele fornece console da web, ferramentas CLI e uma API da web. Ele é escrito em Java e permite que você execute tarefas em um conjunto de nós. A política de controle de acesso baseado em função oferece mais flexibilidade para gerenciar diferentes permissões de acesso do usuário. As automações de processos são definidas como jobs. Você pode definir cada etapa do fluxo de trabalho, que pode ser um trabalho em si ou qualquer tarefa. O usuário pode fornecer opções de entrada na definição do trabalho. Essas opções podem ser usadas no fluxo de trabalho do trabalho como uma variável. Os valores das opções podem ser padrão, de múltipla escolha ou protegidos. O Rundeck é mais como uma ferramenta de orquestração que pode ser usada para gerenciar servidores ou ambientes em nuvem. Ele oferece suporte e se integra bem às ferramentas e práticas modernas de DevOps.

![Rundeck operation](https://i1.wp.com/foxutech.com/wp-content/uploads/2017/06/rundeckuse_zeroclickdeploy.png?resize=696%2C501&ssl=1)

A Rundeck foi projetada para aceitar a realidade de que infraestrutura e ferramentas heterogêneas são um fato da vida em qualquer organização de tamanho considerável. É por isso que a Rundeck não obriga a substituir os scripts, comandos ou ferramentas que você usa hoje. Você usa o Rundeck para executar fluxos de trabalho em sua automação existente (por exemplo, Ansible, Puppet, Chef, Jenkins, Docker, Kubernetes, ferramentas legadas e todos os seus scripts / APIs personalizados) ou automatizar rapidamente procedimentos manuais anteriores. Com o Rundeck, você pode reutilizar as habilidades de automação que já possui e adicionar novas conforme necessário.

## O Rundeck é gratuito?

Rundeck Open Source é um software de código aberto gratuito licenciado sob a [Apache Software License v2.0](http://www.apache.org/licenses/LICENSE-2.0.html) , e você pode participar do projeto no [GitHub](https://github.com/rundeck/rundeck). Para aqueles que escrevem e executam trabalhos Rundeck em pequena escala (por exemplo, uso limitado ou dentro de uma equipe), o Rundeck de código aberto oferece os recursos de que você precisa, gratuitamente.

## Rundeck Enterpise (fka Rundeck Pro)

O foco do [Rundeck Enterprise](https://www.rundeck.com/enterprise), é tornar o Rundeck pronto para produção em escala empresarial. O Rundeck Enterprise, desenvolvido com base no Rundeck de código aberto, é o pacote de software e serviços para executar o Rundeck como um serviço de nível empresarial.

Construído e testado para a empresa, o Rundeck Enterprise inclui recursos exclusivos (incluindo clustering / HA, fluxo de trabalho avançado, gerenciamento de ACL aprimorado, painéis / visualização aprimorados) e plug-ins exclusivos. Suporte profissional e serviços de integração também fazem parte do pacote de assinatura Rundeck Enterprise.


## Recursos destaques do Rundeck

- Execução de comando distribuído
- Fluxo de trabalho (incluindo passagem de opções, condicionais, tratamento de erros e várias estratégias de fluxo de trabalho)
- Sistema de execução plugável (SSH e WinRM por padrão; Powershell disponível)
- Modelo de recursos plugável (obtenha detalhes de sua infraestrutura de sistemas externos)
- On-demand (Web GUI, API ou CLI) ou execução de trabalho agendada
- Armazenamento de chaves seguro para senhas e chaves
- Política de controle de acesso baseado em função com suporte para LDAP / ActiveDirectory / SSO
- Ferramentas de edição / gerenciamento de políticas de controle de acesso
- Histórico e registros de auditoria
- Use qualquer linguagem de script


## Como o Rundeck difere do Jenkins?

Em palavras simples, Jenkins é empregado para desenvolvimento e Rundeck, para operações. Ambas as ferramentas compartilham certos recursos comuns, pois a interface de trabalho fornecida é para autoatendimento.

![Rundeck2](https://thinkpalm.com/wp-content/uploads/2018/04/Rundeck.jpg)

## O Jenkins pode se integrar ao Rundeck?

O Jenkins pode lidar com as compilações para o ciclo de integração contínua de desenvolvimento e o acionamento do Rundeck é necessário para controlar a orquestração distribuída na implantação. Plugins estão disponíveis para a integração do Jenkins com o Rundeck. Quando definimos um trabalho no Jenkins, podemos especificar a tarefa para acionar um trabalho após verificar o status de execução de determinado trabalho.

Da mesma forma, se os privilégios de administrador do Jenkins forem fornecidos ao Rundeck, ele poderá acessar os artefatos de trabalho. Assim, o Rundeck pode pegar os artefatos do trabalho do Jenkins e pode acionar a implantação. O mecanismo de reversão com Rundeck funciona bem quando uma implantação não funciona conforme o esperado. Podemos escolher quaisquer artefatos salvos do Jenkins como artefatos de implantação.

## Instalação do rundeck

Para este laborátorio irei utilizar o Centos 7.8, na GCP. Para instalações personalizadas e/ou instalações em derivações do debian, docker ou execução via jar consulte a documentação oficial [clicando aqui](https://docs.rundeck.com/docs/administration/install/).

### Requisitos de sistema

#### Sistemas operacionais suportados:

- Red Hat Enterprise Linux
- Oracle Linux
- CentOS
- Ubuntu
- Windows Server

####  Recursos mínimos:

- JAVA 8 or 11 Installed.
- 2 CPUs
- 2 CPUs per instance
- 4 GB RAM
- 20 GB hard disk

- Database

- Mysql version
- Mariadb version
- Postgres version
- Oracle version

#### Log store
- Sistema de arquivo
- Armazenamento de objetos compatível com S3

#### Amazon EC2
- Tamanho da instância de m3.medium ou maior
- Um tamanho de instância de m3.xlarge ou maior se houver mais de 100 hosts

Root (ou administrador no Windows) não é necessário ou recomendado. 

Se houver necessidade de acesso root, configure o usuário Rundeck para ter acesso via [sudo](https://en.wikipedia.org/wiki/Sudo).


## Instalando em distribuições CentOS ou Red Hat Linux

### Instale com yum

Você pode os comandos abaixo para adicionar o repo Rundeck yum e instalar o Rundeck:

```bash
rpm -Uvh http://repo.rundeck.org/latest.rpm
sudo yum install rundeck java
```

### Iniciando Rundeck

```bash
sudo service rundeckd start
```

#### Saída

```bash
[root@srvcloud /]# sudo service rundeckd start
Starting rundeckd (via systemctl):                         [  OK  ]
```

### Para verificar se o serviço foi iniciado corretamente, abra os registros de log: 

`tail -f /var/log/rundeck/service.log`

## Logando pela primeira vez

- Navegue para http: // localhost: 4440 / em um navegador 
- Faça login com o nome de usuário **admin** e senha **admin** Rundeck agora está instalado e funcionando!


![Login Rundeck](https://i.imgur.com/ieR6mt7.png)

![Página inicial](https://i.imgur.com/2F1vulf.png)


## Criando seu primeiro projeto

Como pode ser visualizado na imagem abaixo, existem diversas funcionalidades que podem ser definidas no escopo do projeto. Vale conferir, para este momento irei deixar todas essas opções como padrão.

![Criando seu primeiro projeto](https://i.imgur.com/r2bzFaP.png)

### Adicionando nós ao projeto

![Fonte dos nós](https://i.imgur.com/t53uc6G.png)

### Executando comandos aleatórios

![Execução de comandos](https://i.imgur.com/O3oLM4o.png)

### Opções de configurações dentro do projeto

![Configurações](https://i.imgur.com/PijbOjp.png)

### Criação de JOB

![Criando job](https://i.imgur.com/45SSFc2.png)

### Editando o workflow

Existem muitas opções nessa funcionalidade, vale a pena explorá-las uma a uma. Uma opção bem interessante é a de adicionar opções dentro do job e escolhe-las no momento da execução,  essas opções podem ser definidas dentros dos scripts de execução.

![workflow](https://i.imgur.com/zscyrqH.png)

![passo](https://i.imgur.com/zOfCUa6.png)

### Execução do JOB criado

![Execução do job](https://i.imgur.com/kpFOZy1.png)


### Registro de atividades

![Registro de logs](https://i.imgur.com/u1W5SLP.png)

### Lisa de tarefas

![Lista de tarefas](https://i.imgur.com/5jbcISr.png)


Por hoje é isso, espero que este post tenha sido útil para lhe dar um norte e conhecer esta ferramenta e quem sabe utilizá-la no ambiente em que trabalha. 

### Links oficiais para documentação

[Rundeck](https://docs.rundeck.com/docs/)

### Fontes de apoio

[Fonte1](https://docs.rundeck.com/docs/manual/01-introduction.html#who-makes-rundeck)

[Fonte2](https://foxutech.com/what-is-rundeck/)