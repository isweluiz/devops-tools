Olá pessoal, espero que estejam bem! Este é o segundo post da série semana DevOps, hoje iremos conhecer o Prometheus e Grafana, com um breve resumo e logo em seguida partiremos para a prática(hands-on)... 

#### ***Acesse esse post no meu [GitHub](https://github.com/isweluiz)***

## Resumo
 ## Tópicos
 ## Prometheus
- O que é Prometheus?
- -  Recursos
- - Componentes
- - Arquitetura
- - Em que cenário devo utilizar?
- - Em que cenários **não** devo utilizar?
- Instalando e configurando o Prometheus
- - Configurando o Prometheus
- - Configurando um exporter
- Modelo de dados do Prometheus
- - O que são dados de série temporal?
- Tipos de métricas

## Grafana
- O que é Grafana?
- Instalação e configuração da Grafana
- Construindo um dashboard no Grafana com dados do Prometheus

![Prometheus e grafana](https://i.ibb.co/fYGMHq4/aptira-grafana-prometheus-training.png)

## Oque é Prometheus? 
O Prometheus é um kit de ferramentas de monitoramento e alerta de sistemas de código aberto originalmente criado no SoundCloud. Desde o seu início em 2012, muitas empresas e organizações adotaram o Prometheus, e o projeto tem uma comunidade de desenvolvedores e usuários muito ativa. Agora é um projeto autônomo de código aberto e mantido independentemente de qualquer empresa. Para enfatizar isso, e para esclarecer a estrutura de governança do projeto, a Prometheus se juntou à [Cloud Native Computing Foundation](io) em 2016 como o segundo projeto hospedado, depois do Kubernetes.

Para visões gerais mais elaboradas do Prometheus, consulte os recursos vinculados na seção de [mídia](https://www.prometheus.io/docs/introduction/media).

## Recursos
As principais características do Prometheus são:

- um modelo de dados multidimensional com dados de série temporal identificados por nome de métrica e pares chave/valor;
- PromQL, uma [linguagem de consulta flexível](https://www.prometheus.io/docs/prometheus/latest/querying/basics/) para aproveitar essa dimensionalidade;
- nenhuma dependência de armazenamento distribuído;
- nós de servidor único são autônomos;
- a coleta de série temporal acontece por meio de um modelo pull sobre HTTP;
- pushing time series é suportado por meio de um gateway intermediário;
- alvos são descobertos por meio de descoberta de serviço ou configuração estática;
- vários modos de suporte para gráficos e painéis.

## Componentes
O ecossistema Prometheus consiste em vários componentes, muitos dos quais são opcionais:

- o servidor principal do Prometheus, que coleta e armazena dados de séries temporais
- bibliotecas cliente para instrumentar o código do aplicativo
- um portal push para apoiar empregos de curta duração
- exportadores de fins especiais para serviços como HAProxy, StatsD, Graphite, etc.
- um alertmanager para lidar com alertas
- várias ferramentas de suporte

A maioria dos componentes do Prometheus são escritos em [Go](https://golang.org/), tornando-os fáceis de construir e implantar como binários estáticos.

## Arquitetura 

Este diagrama ilustra a arquitetura do Prometheus e alguns de seus componentes do ecossistema:

![Arquitetura Prometheus](https://www.prometheus.io/assets/architecture.png)

O Prometheus extrai métricas, diretamente ou por meio de um gateway intermediário para trabalhos de curta duração. Ele armazena todas as amostras coletadas localmente e executa regras sobre esses dados para agregar e registrar novas séries temporais de dados existentes ou gerar alertas. [Grafana](https://grafana.com/) ou outros consumidores de API podem ser usados ​​para visualizar os dados coletados.

## Em que cenário devo utilizar?
O Prometheus funciona bem para registrar qualquer série temporal puramente numérica. Ele se adapta tanto ao monitoramento centrado na máquina quanto ao monitoramento de arquiteturas orientadas a serviços altamente dinâmicas. Em um mundo de microsserviços, seu suporte para coleta e consulta de dados multidimensionais é um ponto forte particular.

O Prometheus foi projetado para ter confiabilidade,para ser o sistema que você acessa durante uma interrupção para permitir o diagnóstico rápido de problemas. Cada servidor Prometheus é autônomo, não dependendo do armazenamento de rede ou de outros serviços remotos. Você pode confiar nele quando outras partes de sua infraestrutura estão quebradas e não precisa configurar uma infraestrutura extensa para usá-la

## Em que cenários **não** devo utilizar?

A Prometheus valoriza a confiabilidade. Você sempre pode ver quais estatísticas estão disponíveis sobre o seu sistema, mesmo em condições de falha. Se você precisa de 100% de precisão, como para faturamento por solicitação, o Prometheus não é uma boa escolha, pois os dados coletados provavelmente não serão detalhados e completos o suficiente. Nesse caso, seria melhor usar algum outro sistema para coletar e analisar os dados para faturamento e o Prometheus para o restante do seu monitoramento.

## Instalação do Prometheus

Binários pré-compilados do Prometheus estão disponíveis para download em prometheus.io/download/.

O código-fonte do Prometheus pode ser encontrado em github.com/prometheus/prometheus.

### Documentação Relevante
- [Instalação do Prometheus](https://prometheus.io/docs/prometheus/latest/installation/)

## Referência de instalação 

**Distribuição:** CentOS Linux release 7.8.2003 (Core)

Plataforma: Google Cloud 

Tag: Prometheus

![VM GOOGLE CLOUD](https://i.imgur.com/MbxzQiv.png)

```bash
[root@srvcloud isweluiz]# curl ifconfig.me ; echo " "
34.66.133.7 
[root@srvcloud isweluiz]# cat /etc/redhat-release 
CentOS Linux release 7.8.2003 (Core)
[root@srvcloud isweluiz]# free -mh
              total        used        free      shared  buff/cache   available
Mem:           3,7G        1,6G        1,5G        8,6M        614M        1,9G
Swap:            0B          0B          0B
[root@srvcloud isweluiz]#
```

### Crie um usuário, grupo e diretórios para o Prometheus:

```bash
sudo useradd -M -r -s /bin/false prometheus
sudo mkdir /etc/prometheus /var/lib/prometheus
```

### Baixe e extraia os binários pré-compilados: 
#### [Releases](https://github.com/prometheus/prometheus/releases)

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.21.0/prometheus-2.21.0.linux-amd64.tar.gz

sudo tar -xvf prometheus-2.21.0.linux-amd64.tar.gz 
```
### Mova os arquivos do arquivo baixado para os locais apropriados e defina a propriedade:


```bash
sudo cp prometheus-2.16.0.linux-amd64/{prometheus,promtool} /usr/local/bin/

sudo chown prometheus:prometheus /usr/bin/{prometheus,promtool}

sudo cp -r prometheus-2.16.0.linux-amd64/{consoles,console_libraries} /etc/prometheus/

sudo cp prometheus-2.16.0.linux-amd64/prometheus.yml /etc/prometheus/prometheus.yml

sudo chown -R prometheus:prometheus /etc/prometheus

sudo chown prometheus:prometheus /var/lib/prometheus
```
### Teste brevemente sua configuração executando o Prometheus em primeiro plano:

```bash
prometheus --config.file=/etc/prometheus/prometheus.yml
```
![Testando a configuração](https://i.imgur.com/D9zAxjU.png)

### Crie um systemdarquivo de unidade para Prometheus:

```bash
sudo vi /etc/systemd/system/prometheus.service
```
### Defina o serviço Prometheus no arquivo de unidade:


```bash
[Unit]
Description=Prometheus Time Series Collection and Processing Server
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/bin/prometheus \
    --config.file /etc/prometheus/prometheus.yml \
    --storage.tsdb.path /var/lib/prometheus/ \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries

[Install]
WantedBy=multi-user.target
```

### Inicie e ative o serviço Prometheus:

```bash
sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus
```

## Faça uma solicitação HTTP ao Prometheus para verificar se ele é capaz de responder:

```bash
curl localhost:9090

```

### Você também pode acessar o Prometheus em um navegador usando o endereço IP público do servidor: 

http://<PROMETHEUS_SERVER_PUBLIC_IP>:9090.

![Prometheus home page](https://i.imgur.com/d4UQLXF.png)

## Configurando o Prometheus

O Prometheus possui uma ampla variedade de opções de configuração que podem permitir que você personalize seu comportamento para atender às suas necessidades. Embora existam muitas opções para cobrir todos eles em detalhes, é útil saber como você pode configurar o Prometheus. 

#### Documentação Relevante
- [Configuração Prometheus](https://prometheus.io/docs/prometheus/latest/configuration/configuration/)
- [Exemplo de arquivo de configuração do Prometheus](https://github.com/prometheus/prometheus/blob/release-2.15/config/testdata/conf.good.yml)


### Edite o arquivo de configuração do Prometheus:

```bash
sudo vi /etc/prometheus/prometheus.yml
Localize a global.scrape_intervalconfiguração e altere-a para 10s:

global:
  scrape_interval: 10s
```

### Recarregue a configuração do Prometheus:

```bash
sudo systemctl restart prometheus
```

### Consulte a API Prometheus para verificar se suas alterações entraram em vigor:

```bash
curl localhost:9090/api/v1/status/config
```

### Você deve ver global:\n scrape_interval: 10sna saída.

```bash
[root@srvcloud isweluiz]# curl localhost:9090/api/v1/status/config 
{"status":"success","data":{"yaml":"global:\n  scrape_interval: 10s\n  scrape_timeout: 10s\n  evaluation_interval: 15s\nalerting:\n  alertmanagers:\n  - scheme: http\n    timeout: 10s\n    api_version: v1\n    static_configs:\n    - targets: []\nscrape_configs:\n- job_name: prometheus\n  honor_timestamps: true\n  scrape_interval: 10s\n  scrape_timeout: 10s\n  metrics_path: /metrics\n  scheme: http\n  static_configs:\n  - targets:\n    - localhost:9090\n"}}[root@srvcloud isweluiz]#
```
![Configuração do prometheus](https://i.imgur.com/zoBQnkK.png)

## Configurando um exporter

Para utilizar totalmente o Prometheus, você precisará configurar os exportadores. Exportadores são fontes de dados métricos que o Prometheus coleta periodicamente. Vamos configurar o monitoramento de um servidor Linux CentOS. Vamos instalar o Node Exporter no servidor e configurar o Prometheus para extrair as métricas desse exportador, isso nos permitirá consultar o Prometheus para os dados métricos do novo servidor Linux.

### Crie um novo servidor:

**Referência do novo servidor**

**Distribuição:** Ubuntu 18.04 Bionic Beaver 

**Tamanho:** Small

**Tag:** OTRS6

Faça login no novo servidor. Vamos configurar este novo servidor para monitoramento do Prometheus usando o Node Exporter.

### Crie um usuário para o exportador de nós:

```bash
sudo useradd -M -r -s /bin/false node_exporter
```

### Baixe e extraia o binário Node Exporter:

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v0.18.1/node_exporter-0.18.1.linux-amd64.tar.gz

tar xvfz node_exporter-0.18.1.linux-amd64.tar.gz
```
![Node_exporter](https://i.imgur.com/8no2YzX.png)

### Copie o binário Node Exporter para o local apropriado e defina a propriedade:

```bash
sudo cp node_exporter-0.18.1.linux-amd64/node_exporter /usr/bin/

sudo chown node_exporter:node_exporter /usr/bin/node_exporter
```

### Crie um systemd do arquivo de unidade para Node Exporter:

```bash
sudo vi /etc/systemd/system/node_exporter.service
```

### Defina o serviço Node Exporter no arquivo de unidade:


```bash
[Unit]
Description=Prometheus Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/bin/node_exporter

[Install]
WantedBy=multi-user.target
Inicie e ative o node_exporterserviço:
```

### Inicie e ative o node_exporterserviço:

```bash
sudo systemctl daemon-reload
sudo systemctl start node_exporter
sudo systemctl enable node_exporter
```
![Node_exporter_](https://i.imgur.com/C5pVpWG.png)

### Você pode recuperar as métricas fornecidas pelo Node Exporter assim:

```bash
curl localhost:9100/metrics
```

![Node_exporter_metrics](https://i.imgur.com/t3Kp2mw.png)

### Configure o Prometheus Server para extrair métricas

Faça login em seu servidor Prometheus e configure o Prometheus para extrair métricas do novo servidor.

Edite o arquivo de configuração do Prometheus:

```bash
sudo vim /etc/prometheus/prometheus.yml
```
Localize a scrape_configs seção e adicione uma nova entrada nessa seção. Você precisará fornecer o endereço IP privado do seu novo servidor para targets.


```yml
...
- job_name: 'Linux Server'
  static_configs:
  - targets: ['<PRIVATE_IP_ADDRESS_OF_NEW_SERVER>:9100']
...
```
![node_exporter_server_prometheus](https://i.imgur.com/cJWb1mN.png)

### Recarregue a configuração do Prometheus:

```bash
sudo systemctl restart prometheus
```

Navegue até a página principal do Prometheus server em seu navegador usando o endereço IP público do seu servidor Prometheus: <PROMETHEUS_SERVER_PUBLIC_IP>:9090.

### Execute algumas consultas para recuperar dados métricos sobre seu novo servidor:

up
![UP](https://i.imgur.com/WyxWCgZ.png)

node_filesystem_avail_bytes
![node_filesystem_avail_bytes](https://i.imgur.com/ZnNnhs1.png)

![Graph](https://i.imgur.com/TsoKdlo.png)

## Modelo de dados do Prometheus
###  O que são dados de série temporal?

Todos os dados do Prometheus são fundamentalmente armazenados na forma de uma série temporal, por isso é importante termos uma compreensão básica de como os dados métricos funcionam.

Uma série temporal é uma série de pontos de dados indexados (ou listados ou gráficos) em ordem de tempo. Mais comumente, uma série temporal é uma sequência tomada em pontos sucessivos igualmente espaçados no tempo. Portanto, é uma sequência de dados em tempo discreto . Exemplos de séries temporais são alturas das marés do oceano , contagens de manchas solares e o valor de fechamento diário da Dow Jones Industrial Average .

As séries temporais são frequentemente traçadas por meio de gráficos de execução (um gráfico de linha temporal ). As séries temporais são usadas em estatística , processamento de sinais , reconhecimento de padrões , econometria , finanças matemáticas , previsão do tempo , previsão de terremotos , eletroencefalografia , engenharia de controle , astronomia , engenharia de comunicações e, em grande parte, em qualquer domínio da ciência aplicada e engenharia que envolva medições temporais .

[Leia mais sobre série temporal](https://en.wikipedia.org/wiki/Time_series)

[Modelo de dados do Prometheus](https://prometheus.io/docs/concepts/data_model/)

![Série temporal](https://upload.wikimedia.org/wikipedia/commons/7/77/Random-data-plus-trend-r2.png)




## Leitura recomendada
- [Blog Prometheus](https://www.prometheus.io/blog)
- [Configurando node Exporter](https://prometheus.io/docs/guides/node-exporter/)
- [Prometheus configuring](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config)
- [Prometheus releases](https://github.com/prometheus/prometheus/releases)
- [Linguagem GO](https://golang.org/doc/install)
- [Prometheus oficial download](https://prometheus.io/download/)


