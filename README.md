> Notas de estudos lendo o livro: Descomplicando o docker 2

Fique a vontade para contribuir com anotações também 🚀

- [DOCKER](#docker)
  - [`Para realizar a instalação do docker, consultar o guia da documentação oficial`](#para-realizar-a-instalação-do-docker-consultar-o-guia-da-documentação-oficial)
- [Estudos utilizando linux](#estudos-utilizando-linux)
  - [Adicionar usuário ao grupo do docker](#adicionar-usuário-ao-grupo-do-docker)
  - [Iniciar o docker](#iniciar-o-docker)
  - [Listar imagens](#listar-imagens)
- [CONTAINER](#container)
  - [Listar containers](#listar-containers)
  - [Iniciando um container](#iniciando-um-container)
  - [**Rodar um container**](#rodar-um-container)
  - [Criar um container mas não roda-lo instantaneamente](#criar-um-container-mas-não-roda-lo-instantaneamente)
  - [Entrar em um container](#entrar-em-um-container)
  - [Subindo container](#subindo-container)
  - [Parando o container](#parando-o-container)
  - [Reiniciando o container](#reiniciando-o-container)
  - [Pausando um container](#pausando-um-container)
  - [Despausando um container](#despausando-um-container)
  - [Visualizar consumo de recursos](#visualizar-consumo-de-recursos)
  - [Mostrar os processos dentro de um container](#mostrar-os-processos-dentro-de-um-container)
  - [Logs](#logs)
  - [Ficar mostrando os logs ao vivo](#ficar-mostrando-os-logs-ao-vivo)
  - [Remover o container](#remover-o-container)
  - [Inspecionar um container](#inspecionar-um-container)
  - [Especificando recursos do container](#especificando-recursos-do-container)
  - [Alterar os recursos do container em execução](#alterar-os-recursos-do-container-em-execução)
- [DOCKERFILE](#dockerfile)
  - [primeiro docker file](#primeiro-docker-file)
  - [Criar uma imagem a partir do Dockerfile](#criar-uma-imagem-a-partir-do-dockerfile)
- [Volumes](#volumes)
  - [Criando e montando um DATA-ONLY CONTAINER](#criando-e-montando-um-data-only-container)
  - [Backups](#backups)
- [GERENCIANDO IMAGENS](#gerenciando-imagens)
  - [Dockerfile](#dockerfile-1)
  - [Algumas opções do dockerfile](#algumas-opções-do-dockerfile)
  - [Multi-Stage (Método bem utilizado)](#multi-stage-método-bem-utilizado)
  - [Customizando uma imagem base](#customizando-uma-imagem-base)
- [COMPARTILHANDO IMAGENS](#compartilhando-imagens)
  - [Docker hub](#docker-hub)
  - [Criando um registry local](#criando-um-registry-local)
- [GERENCIANDO A REDE DOS CONTAINERS](#gerenciando-a-rede-dos-containers)
  - [Daemon](#daemon)
  - [Sockets](#sockets)
- [OPÇÕES DE STORAGE](#opções-de-storage)
- [OPÇÕES DE REDE](#opções-de-rede)
- [OPÇÕES DIVERSAS](#opções-diversas)
- [Docker Machine (Deprecated) - Não consegui fazer funcionar](#docker-machine-deprecated---não-consegui-fazer-funcionar)
- [DOCKER SWARM](#docker-swarm)
- [**SERVICES**](#services)
- [SECRETS (Usada com docker swarm (Serviços))](#secrets-usada-com-docker-swarm-serviços)
- [DOCKER COMPOSE](#docker-compose)
- [Exemplos](#exemplos)
  - [`Primeiro exemplo`](#primeiro-exemplo)
  - [`Segundo exemplo`](#segundo-exemplo)
  - [`Terceiro exemplo`](#terceiro-exemplo)
  - [`Quarto exemplo`](#quarto-exemplo)
  - [`Quinto exemplo`](#quinto-exemplo)
- [LINKS UTEIS](#links-uteis)

# DOCKER

## `Para realizar a instalação do docker, consultar o guia da documentação oficial`

# Estudos utilizando linux

## Adicionar usuário ao grupo do docker

```bash
sudo usermod -a -G docker <name>
```

## Iniciar o docker

```bash
sudo /etc/init.d/docker
# ou
service docker start
```

## Listar imagens

```bash
docker image ls
```

# CONTAINER

## Listar containers

```bash
#Listar containers ativos
docker container ls
#Listar todos os containers
docker container ls -a
```

## Iniciando um container

**Parâmetros**

`t` – Disponibiliza um TTY (console) para o nosso container .

`i` – Mantém o STDIN aberto mesmo que você não esteja conectado no container.

`d` – Faz com que o container rode como um daemon , ou seja, sem a interatividade que os outros dois parâmetros nos fornecem. Com isso temos dois modos de execução de nossos containers : modo interativo ou daemonizando o container .

## **Rodar um container**

```bash
#rodar em modo iterativo
docker container run -ti centos:7
#rodar no background - O Daemon faz o trabalho
docker container run -d centos:7
#Para dar nome ao container se utiliza a tag --name
docker container run -ti --name teste centos:7
```

**Para sair do modo iterativo pressione** `CTRL+P+Q`

## Criar um container mas não roda-lo instantaneamente

```bash
docker container create -ti ubuntu
```

## Entrar em um container

```docker
docker container attach container_id
```

## Subindo container

```bash
docker container start container_id
```

## Parando o container

```bash
coder container stop container_id
```

## Reiniciando o container

```bash
docker container restart donctainer_id
```

## Pausando um container

```bash
docker container pause container_id
```

## Despausando um container

```bash
docker container unpause container_id
```

## Visualizar consumo de recursos

```bash
docker container stats container_id
```

## Mostrar os processos dentro de um container

```bash
docker container top container_id
```

## Logs

```bash
docker container logs container_id
```

## Ficar mostrando os logs ao vivo

```bash
docker container logs -f container_id
```

## Remover o container

```bash
docker container rm container_id
```

## Inspecionar um container

```bash
docker container inspect name_container
#Para pegar somente a memoria
docker container inspect name_container | grep -i mem
#Para pegar somente o cpus
docker container inspect name_container | grep  -i cpu
```

## Especificando recursos do container

```bash
docker container run -ti -m 512M --name container_name debian
docker container run -ti -cpus=0.5 --name container_name nginx
```

## Alterar os recursos do container em execução

```bash
docker container update --cpus=4 -m 512M container_name
```

# DOCKERFILE

## primeiro docker file

```bash
vim Dockerfile
```

```docker
FROM debian
RUN /bin/echo "Hello World!"
```

## Criar uma imagem a partir do Dockerfile

```bash
docker build -t name:1.0 [path do arquivo docker]
#exemplo
docker build -t exemplo:1.0 .
# . para indicar a pasta atual
```

# Volumes

Adicionando um volume a um container

Modelo antigo (Adicionando um volume quando subimos o container)

```bash
docker container run -ti --mount type=bind,src=<absolute_path>,dst=<absolute_path> <dist> 

#Example
docker container run -ti --mount type=bind,src=/home/primeiro_docker,dst=/volume ubuntu

# utilize do comando [ro] para deixar como read only se necessário
docker container run -ti --mount type=bind,src=/home/primeiro_docker,dst=/volume,ro ubuntu

#É possível também passar um arquivo para o container
docker contianer run -ti --mount type=bind,srv=/home/primeiro_docker/arquivo.sh,dst=/volume ubuntu
```

Modelo atual, e segundo o autor, o modelo mais elegante

Desta maneira os volumes sempre são criados em: `/var/lib/docker/volumes/`

Então os volumes serão adicionados a esse diretório automaticamente

```bash
#criar volume
docker volume create <name>
#Remover volume
docker volume rm <name>
#Ver detalhes do volume
docker volume inspect <name>
#Para remover os volumes que não estão sendo utilizados
docker volume prune
#Para montar o volume no contianer, utilize da seguinte forma
docker container run <option> --mount type=volume,source=<name>,destination=<path> <dist>
#example
docker container run -d --mount type=volume,source=teste,destination=/volume ubuntu
```

Para localizar o volume 

```bash
docker volume inspect --format '{{.Mountpoint}}' <name>
```

## Criando e montando um DATA-ONLY CONTAINER

A principal função desse container é prover volumes para outros containers

```bash
#criar container com o volume de dados
docker container create -v/data --name dbdados centos
#identificando onde esta o volume de dados
docker inspect -f {{.Mounts}} dbdados
```

 `—volumes-from` É utilizado quando vamos montar um volume disponibilizado por outro container

`-e` É utilizado para informar as variáveis de ambiente do container

```bash
docker run -d -p 5432:5432 --name pgsql1 --volumes-from dbdados \
-e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker \
-e POSTGRESQL_DB=docker kamui/postgresql

docker run -d -p 5433:5432 --name pgsql2 --volumes-from dbdados \
-e POSTGRESQL_USER=docker -e POSTGRESQL_PASS=docker \
-e POSTGRESQL_DB=docker kamui/postgresql
```

## Backups

```bash
cd backup/
docker run -ti --volumes-from dbdados -v $(pwd):/backup debian tar -cvf /backup/backup.tar /data
#Criamos um novo container utilizando volumes
#Utilizamos o comando tar para empacotar a pasta /data para dentro de /backup/
```

# GERENCIANDO IMAGENS

Se pode utilizar o docker hub para buscar imagens

## Dockerfile

Exemplo de docker file

```bash
FROM debian
RUN apt-get update && apt-get install -y apache2 && apt-get clean
ENV APACHE_LOCK_DIR="/var/lock"
ENV APACHE_PID_FILE="/var/run/apache2.pid"
ENV APACHE_RUN_USER="www-data"
ENV APACHE_RUN_GROUP="www-data"
ENV APACHE_RUN_DIR="/var/run/apache2"
ENV APACHE_LOG_DIR="/var/log/apache2"
LABEL description="Webserver"
VOLUME /var/www/html
EXPOSE 80
ENTRYPOINT ["/usr/sbin/apachectl"]
CMD ["-D","FOREGROUND"]
```

Para criar a imagem, basta rodar o comando `docker build -t name:<version> .` no diretório do Dockerfile

```bash
docker build -t apache-teste:1.0 .
docker run -ti apache-teste:1.0
#Dentro do container
#Ativar o apache
service apache2 start
#Verificar se a porta 80 está ativa
ps -ef
#Pegar o ip no Debian
/sbin/ip route|awk '/default/ { print $3 }'
```

## Algumas opções do dockerfile

```docker
ADD Permite adcionar alguma pasta/arquivo do host para o container
CMD Depois que o container estiver ativo, ele executara os comandos listados no CMD
LABEL Adiciona metadados a imagem, como descrição, versão e fabricante
COPY Copia arquivos ou pastas e adiciona ao filesystem do container
ENTRYPOINT Configura o container para rodar um executavél. Assim que o executavel é finalizado o container também será
ENV Informa variavel de ambiente
EXPOSE Informa que porta o container está ouvindo
FROM Informa qual image será utilizada como base (Precisa ser a primeira linha do dockerfile)
MAINTAINER Autor da imagem
RUN Executa qualquer comando da camada do topo da imagem e comita as alterações no container
VOLUME Permite a criação de um ponto de montagem no container
WORKDIR Muda o diretório raiz, especificando um diretório
```

> Quando estamos utilizando o `ENTRYPOINT` e `CMD` no mesmo dockerfile, o `CMD` somente aceita parâmetros do `ENTRYPOINT`
> 

```docker
#Comando
ENTRYPOINT [”/usr/sbin/apachectl”] 
#Parametro
CMD ["-D","FOREGROUND"]
```

## Multi-Stage (Método bem utilizado)

Utilizado como ponte entre imagens, utilizando o multi-stage é possível referenciar outras imagens  para utilizar de sua infraestrutura. Assim as imagens não precisaram sempre importar toda a infraestrutura de uma linguagem, por exemplo, Java, C, Go e etc...

**Exemplo de criação de aplicativo de escrita em Golang**

`vim goapp.go`

```go
// Dentro do arquivo goapp.gp
package main 
import "fmt"
func main() {
	fmt.Println("GIROPOPS STRIGUS GIRUS - LINUXTIPS")
}
```

**Bora criar o dockerfile, porém ele tera duas versões**

*Imagem que conterá a importação do go*

`vim Dockerfile`

```docker
FROM golang
WORKDIR /app
ADD . /app
RUN go env -w GO111MODULE=off
RUN go build -o goapp
ENTRYPOINT ./goapp
```

`docker build -t goapp:1.0 .`

*Imagem que vai referenciar o container anterior, utilizando sua infraestrutura*

`vim Dockerfile2`

```docker
FROM golang AS buildando
ADD . /src
WORKDIR /src
RUN go env -w GO111MODULE=off
RUN go build -o goapp

FROM alpine:3.1
WORKDIR /app
COPY --from=buildando /src/goapp /app
ENTRYPOINT ./goapp
```

`docker build -t buildando:1.0 -f Dockerfile2 .`

![Buildando]('images/buildando.png')

Veja que o container `buildando` utiliza a infraestrutura do container `goapp`, assim o container `buildando` não precisa importar toda a infraestrutura para a linguagem do `go`, ficando dessa forma, bem mais leve.

## Customizando uma imagem base

Demonstração de modificação de imagem

```bash
#Primeiro criar o container
docker container run -ti debian:8 /bin/bash
#Fazer alguma alteração, nesse exemplo instalar o apache
apt-get update && apt-get install -y apache2 && apt-get clean
#Saia do contaier CTRL+p+q
#Salve as mudanças que foram feitas dentro de uma nova imagem e mantenha essas mudanças no container atual
docker commit -m "Mensagem" container_id
#Adicione uma tag para a nova imagem, pois ela está sem
docker tag image_id name/version
#Vamos criar um container com a imagem que acabamos de criar
docker container run -ti image_id /bin/bash
#Inicie o apache
/etc/init.d/apache2 start
#Capture o ip do container
ip addr show eth0
#Saia do container CTRL+p+q
curl ip_container
```

# COMPARTILHANDO IMAGENS

Sabendo mais de uma imagem

```bash
docker image inspect image_id
docker history image_id
```

Existem sites para inspecionar as camadas de imagens

## Docker hub

[Docker Hub Container Image Library | App Containerization](https://hub.docker.com/)

**Para authenticar no docker:** 

`docker login`

Para se autenticar em algum site de hospedagem diferente, basta passar o site da seguinte forma

`docker login registry.seilaqual.com`

Padrão para fazer o upload da imagem, a tag da imagem deve seguir o padrão abaixo:

`seuusuario/nomedaimagem:versão`

**Fazendo push da imagem**

`docker push seuusuario/nomedaimagem:versão`

Fazendo busca pela imagem

`docker search seu_usuário`

**Fazendo Pull** da imagem

`docker pull seuusuario/nomedaimagem:versão`

## Criando um registry local

<aside>
💡 Utilizado em locais que não desejam utilizar alguma registry web, por exemplo o docker hub. Utilizamos nesse caso o Docker Distribuition

</aside>

Rodando o registry localmente em um container

`docker container run -d -p 5000:5000 --restart=always --name registry registry:2`

Mapeando a tag da imagem, para fazer push no registry local

`docker tag image_id ip:porta/nome_imagem`

Exemplo: `docker tag 7897498465415 localhost:5000/debian_apache`

[https://github.com/distribution/distribution](https://github.com/distribution/distribution)

(Tem que pesquisar em que do container ele salva isso)

# GERENCIANDO A REDE DOS CONTAINERS

**Range ip**

Por padrão vem 172.16.x.x, para mudar o docker precisa ser iniciado com outro ip. Para trocar o ip se usa o comando `--bip` em conjunto com o daemon do docker o `dockerd`.

`dockerd —bip 192.168.0.1/24`

Assim a rede bridge ***docker0*** ficará com esse ip e consequentemente seus containers também seguiram esse range.

Para restringir o range que o docker vai utilizar na rede bridge se utiliza o comando `—fixed-cdir`

`dockerd —fixed-cdir 192.168.0.0/24`

**portas**

Da para especificar o direcionamento de requisições entre host e container.

Rodando o comando da seguinte forma, utilizando a opção `-p`

`docker container run -p porta_host:porta_contianer imagem`

exemplo: `docker container run -ti -p 8080:80 apache`

## Daemon

Daemon é um software que roda no background do docker como um processo pai. Ele controla tudo, desde containers a imagens e etc...

Para se alterar configurações do daemon, é necessário utilizar o `dockerd`

[dockerd](https://docs.docker.com/engine/reference/commandline/dockerd/)

## Sockets

Podem ser utilizadas três opções de sockets (UNIX, TCP, FD), por padrão se utiliza o UNIX. Como o daemon só pode ser acessado localmente, para resolver isso se utiliza o TCP.

O docker pode escutar diferentes tipos de sockets através do parâmetro `-h` passado ao `dockerd`

*unix*

`dockerd -H unix:///var/run/docker.sock`

*tcp*

`dockerd -H tcp://0.0.0.0:2375`

# OPÇÕES DE STORAGE

O docker aceita somente alguns drivers de storage, todos são baseados em esquemas de layers.

Da para alterar a forma com o qual os itens se relacionam com o device mapper, através do parâmetro `—storage-opt` . Que também recebem o parametro de `dm` ou `zfs`. Todos os comandos são transmitidos pelo daemon.

Para alterar o thin-pool que ele usa para criar os snapshots, containers e imagens.

`dockerd —storage-opt dm.thinpooldev=/<path_thin_poll>`

**Definido o tamanho máximo dos containers**

<aside>
💡 Alerta! É preciso deletar imagens, containers e reiniciar o serviço do docker.
</aside>

Através do parâmetro `dm.basesize` é possível definir o tamanho máximo do container

`dockerd —storage-opt dm.basesize=15G`

**Especificar o file system do docker**

O comando é do `dm.fs` e seus parâmetros são **EXT4** ou **XFS**

# OPÇÕES DE REDE

Para controlar como o daemon se controlara mediante a rede:

```bash
-- default-gateway
-- dns 
#Usado para especificar o dominio a ser procurado, assim não precisa utilizar o fqdn para encontrar uma máquina
-- dns search
#Habilita o roteamento entre containers, por padrão ele vem como true
-- ip-foward
```

# OPÇÕES DIVERSAS

```bash
#Valor padrão para o ulimit(Vai saber oque é isso)
-- default-ulimit 
#Inter container comunication, por padrão vem true
-- icc
#Mudar a forma como o docker trabalha com os logs do container. Mais 'verboso' ou menos.
-- log-level
```

# Docker Machine (Deprecated) - Não consegui fazer funcionar

É tipo uma vm de docker, ele é suportado por diversos aplicativos, como VMware, VirtualBox, HyperV e etc... Sendo suportado pela AWS, Azure e etc...

<aside>
💡 Como está ultrapassado, decidi pular esta parte, mas se necessário voltarei e anotarei

</aside>

Tive que voltar, não teve jeito

[https://github.com/docker/machine](https://github.com/docker/machine)

**Instalação no Linux**

```bash
curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine
chmod +x /tmp/docker-machine
sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
```

**Primeiro projeto**

```bash
#Necessário para emular
apt-get install virtualbox
#Criando um novo host
docker-machine create --driver virtualbox linuxtips
```

`docker-machine create – Cria um novo Docker Host.`

`-- driver virtualbox – Irá criá-lo utilizando o VirtualBox.`

`linuxtips – Nome da VM que será criada.`

**Comandos do docker-machine**

```bash
#Listar hosts
docker-machine ls
#Listar variaveis de ambiente de um host
docker-machine env machine_name
#Para acessar o ambiente criado
eval "$(docker-machine env machine_name)"
```

# DOCKER SWARM

Utilizado para criar cluster de containers, tem suporte a filover e faz balanceamento de carga

Um manager e diversos workers.

Docker swarm já é nativo do docker, então quando se instala o docker automaticamente o docker swarm já é instalado

**Criando um cluster**

**MANAGER**

**Definir host como Master**

```bash
docker swarm init --advertise-addr ip_do_host
```

**Listar workers**

```bash
docker node ls
```

**Buscar token de manager**

```bash
docker swarm join-token manager
```

**Buscar token de worker**

```bash
docker swarm join-token worker
```

**Promover um nó para manager**

```bash
docker node promote node_numero
```

**Inspecionar nó**

```bash
docker node inspect node_numero
```

**Tornar um manager worker**

```bash
docker node demote node_numero
```

Para um host sair

```bash
docker swarm leave
#Se for manager
docker swarm leave --force

docker node rm node_numero
```

**WORKER**

Para criar os workers, é necessário rodar um comando apontando para o Master

**Criar workers**

`docker swarm join —token token_master` 

```bash
#Comando na máquina worker host
docker swarm join --token SWMTKN-1-5xt6yp7nxfzau137y93xwo45cdlc9t1q74cdjbtvbq7uv2rwz0-e8qnnijl72b6exz1w61voagf4 192.168.0.13:2377
```

# **SERVICES**

Balanceamento de requisições entre os containers

**Exemplo prático, criando um service com 5 containers, para rodar o nginx**

- Nome do service que desejo criar – webserver .
- Quantidade de containers que desejo debaixo do service – 5 .
- Portas que iremos “bindar”, entre o service e o node – 8080:80 .
- Imagem dos containers que irei utilizar – Nginx .

```bash
#master01
docker service create --name webserver --replicas 5 -p 8080:80 nginx
curl QUALQUER_IP_NODES_CLUSTER:8080
```

**Visualizar o service criado**

```bash
#Executar na máquina manager
docker service ls
```

**Para ver os services rodando nos contianers**

```bash
docker service ps nome_servico
```

**Inspecionar o servico**

**VIP (VirtualIPs)**

Ip balanceador, sempre que alguém acessar via esse ip, a requisição será encaminhada para o melhor container. 

```bash
docker service inspect nome_servico
```

**Escalar containers**

```bash
docker service scale nome_servico=numero_container
```

**Logs do serviço**

```bash
docker service logs -f nome_servico
```

**Remover serviço**

```bash
docker service rm nome_serviço
```

**Criar service com um volume conectado**

```bash
docker service create --name name_service --replicas 5 -p 8080:80 --mount type=volume,src=source_aqui,dst=path_directory nginx
```

# SECRETS (Usada com docker swarm (Serviços))

O Docker Secrets é a solução do Docker para trabalhar com toda a parte de secrets em um ambiente multi-node e multi-container.

Secrets são consumidas por serviços

É tipo variável de ambiente, porém secreta :p

A Secret pode pesar no máximo 500kb

**Criar a secret**

```bash
#Criar a partir do STDIN
echo 'minha secret' | docker secret create minha_secret -
#Criar a partir de um arquivo de texto
docker secret create minha_secret minha_secret.txt
```

**Listar**

```bash
docker secret ls
```

**Inspecionar**

```bash
docker secret inspect nome_ou_id_secret
```

**Deletar**

```bash
docker secret rm nome_ou_id_secret
```

Dentro dos containers, a secret fica em `/run/secrets`

**Parâmetros adicionais passados na hora de informar a secret**

Mudando o uid, gid e mode

```bash
docker service create --detach=false --name app --secret source=secret_nome,target=password,uid=2000,gid=3000,mode=0400 meu_app:1.0
```

**Atualizando a secret de um serviço**

Secrets são imutáveis, porém é possível fazer a troca entre elas

Trocando uma secret

```bash
docker service update --secret-rm secret_antiga --detach=false --secret-add source=secret_nova,target=target_desejado app
```

# DOCKER COMPOSE

Escrever em um único arquivo todos os detalhes do ambiente da aplicação

YML

para checar se o `docker-compose.yml` é um arquivo valido rode o seguinte comando:

```yaml
docker-compose -f docker-compose.yml config
```

# Exemplos

## `Primeiro exemplo`

```bash
docker swarm init --advertise-addr ip
mkdir /root/composes
cd /root/composes
mkdir 1
cd 1
vim docker-compose.yml
```

Utilize esse documento (Se atente a indentação que é muito importante)

```yaml
version: "3"
services:
  web:
    image: nginx
    deploy: 
      replicas: 5
      resources: 
        limits: 
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "8080:80"
    networks: 
      - webserver

networks:
  webserver:
```

**Fazer deploy do service**

```yaml
docker stack deploy -c docker-compose.yml primeiro
```

**Checar se o serviço realmente está de pé**

```yaml
#obter uma sáida do nginxs
curl ip:8000
#Checar serviços
docker service ls
#Checar containers rodando o service
docker service ps nome_service
#Listar os stacks
docker stack ls
```

**Para checar determinados services dentro de uma stack**

```yaml
docker stack services stack_name
```

**Checar detalhes de uma Stack**

```yaml
docker stack ps primeiro
```

Seguindo com o exemplo, remova o stack que foi criada, visando criar uma nova

**Remover uma stack**

```yaml
docker stack rm service_name
```

## `Segundo exemplo`

Crie uma pasta diferente, visando manter o versionamento e vamos criar um novo arquivo `docker-compose.yml` 

```yaml
version: "3"
services: 
  db:
    image: mysql:5.7
    volumes: 
    - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: 123
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on:
    - db
    image: wordpress:latest
    ports:
    - "8000:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_USER: wordpress
      
volumes:
  db_data:
```

Checando se tudo ocorreu da maneira correta

```yaml
docker stack ls
docker stack services stack_name
docker service ls
docker service ps service_name
#Para checar logs
docker service logs service_name
```

## `Terceiro exemplo`

Criar uma pasta diferente novamente 

Criar novo arquivo `docker-compose.yml`

```yaml
version: '3'
services:
  web:
    image: nginx
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: '0.1'
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
    - "8080:80"
    networks:
    - webserver

  visualizer:
    image: dockersamples/visualizer:stable
    ports:
    - "8888:8080"
    volumes:
    - '/var/run/docker.sock:/var/run/docker.sock'
    deploy:
      placement:
        constraints: [node.role == manager]
    networks:
    - webserver

networks: 
  webserver:
```

Para rodar o update no serviço na stack existente, utiliza-se do mesmo comando de deploy inicial

```yaml
docker stack deploy -c docker-compose.yml mesmo_nome
```

## `Quarto exemplo`

Utilizar um projeto já do próprio docker

`docker-compose.yml`

```yaml
version: "3.7"
services:

  redis:
    image: redis:alpine
    networks:
      - frontend
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
        delay: 10s
        order: start-first
      rollback_config:
        parallelism: 1
        delay: 10s
        failure_action: continue
        monitor: 60s
        order: stop-first
      restart_policy:
        condition: on-failure
  db:
    image: postgres:9.4
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      placement:
        constraints: [node.role == manager]
  vote:
    image: dockersamples/examplevotingapp_vote:before
    ports:
      - 5000:80
    networks:
      - frontend
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure
  result:
    image: dockersamples/examplevotingapp_result:before
    ports:
      - 5001:80
    networks:
      - backend
    depends_on:
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure

  worker:
    image: dockersamples/examplevotingapp_worker
    networks:
      - frontend
      - backend
    depends_on:
      - db
      - redis
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints: [node.role == manager]

  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    stop_grace_period: 1m30s
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints: [node.role == manager]

networks:
  frontend:
  backend:

volumes: 
  db-data:
```

## `Quinto exemplo`

```yaml
git clone https://github.com/badtuxx/giropops-monitoring.git
cd giropops-monitoring
docker stack deploy -c docker-compose.yml nome_stack
```

[https://github.com/badtuxx/giropops-monitoring](https://github.com/badtuxx/giropops-monitoring)

[Giropops-Monitoring](https://www.youtube.com/playlist?list=PLf-O3X2-mxDls9uH8gyCQTnyXNMe10iml)

# LINKS UTEIS

[How to detach from docker container from integrated terminal (send [CTRL + P CTRL + Q]](https://stackoverflow.com/questions/55484562/how-to-detach-from-docker-container-from-integrated-terminal-send-ctrl-p-ctr)

[Docker DCA 00 - Preparação da Máquina (Windows / Linux)](https://www.youtube.com/watch?v=U-GGoWq26C4&list=PL4ESbIHXST_TJ4TvoXezA0UssP1hYbP9_&ab_channel=CaioDelgado)

[https://github.com/caiodelgadonew/docker](https://github.com/caiodelgadonew/docker)