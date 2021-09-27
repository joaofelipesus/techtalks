# Docker

## Índice

- [Conceitos](#conceitos)

- [Comandos Básicos](#comandos-básicos)

- [Docker Hub](#docker-hub)

- [Volumes](#volumes)

- [Variáveis de Ambiente](#variáveis-de-ambiente)

- [Network](#network)

- [Dockerfile](#dockerfile)



## Conceitos

- **Container**: é uma metodologia utilizada para empacotar aplicações para que possam ser executadas/disponibilizadas de maneira isolada e eficiente no 
intuito de segregar e facilitar a portabilidade.

- **Imagem**: é uma "receita de bolo" utilizada pelas engines para criar os containers. Dentro de cada imagem possui uma sequência de comandos e 
procedimentos necessários para a criação do container.

- **Docker**: é um container engine que reúne várias funcionalidades que facilitam a criação de imagens e containers, além do gerenciamento dos mesmos.

Apesar do conceito de containers já existir há muito tempo, principalmente em ambientes Linux, o Docker popularizou esse conceito, facilitando a criação 
e o gerenciamento de containers, proporcionando o acesso a esse tipo de tecnologia para qualquer pessoa ou empresa.

Antes do conceito de containers, em uma infraestrutura padrão de virtualização, era necessário segregar os recursos de uma máquina (memória, disco, rede, cpu, etc) 
em várias VMs (Virtual Machines), instalar um sistema operacional em cada uma dessa VMs afim de gerenciar esses recursos segregados, para só então 
instalar uma aplicação ou serviço a ser utilizado.

Dessa forma o gerenciamento de recursos da máquina host (normalmente uma máquina física) era bem mais difícil e os recursos se esgotavam rapidamente, pois cada 
VM "mordia" uma parte de cada recurso. Além disso, a máquina host precisava executar um gerenciador de VMs, ou VM engine, que gerenciava toda essa segregação e manutenção
das VMs, o que "sugava" mais uma parte dos recursos.

A diferença do conceito de containers para o tradicional com VMs, é que os containers isolam recursos da máquina host, mas não os segregam da própria máquina, ou seja, 
a máquina host continua tendo acesso e controle desses recursos, que serão compartilhados com cada container.
Portanto, cada container executa um processo separado na máquina host, usufruindo de todo o potencial dos recursos disponibilizados pelo host.

O Docker reuniu várias ferramentas Linux que já eram utilizadas anteriormente para isolar alguns recursos, como filesystem, process, user e groups, etc, em uma única plataforma
de gerenciamento e execução de containers.

A imagem abaixo ilustra melhor a diferença entre as duas infraestruturas:

![image](https://user-images.githubusercontent.com/8127577/131718760-89d20221-865a-4d82-bad1-07c93b49314b.png)


## Comandos Básicos

### Containers

- **Listar containers**: Lista todos os container em execução
 
    `docker container ls` 
    
    É possível usar o parâmetro `-a` para listar todos os containers existentes independente de seu status (running, stopped, exited, etc)
    
- **Criar container**: Cria e executa um container usando uma imagem (receita de bolo) como referência
    
    `docker container run nginx`
    
    Recomendado sempre utilizar os seguintes parâmetros do comando `docker container run` ao criar containers:
    
    - `-d`: Irá executar o container em modo daemon, ou seja, em um processo em background, não bloqueando o terminal
    - `--name`: Adiciona um nome ao container para facilitar sua identificação, em vez de deixar o Docker gerar um nome aleatório

    É possível também utilizar o parâmetro `--rm` para indicar que deseja que o container seja removido ao final da execução do mesmo, ou seja, ele vai existir enquanto o seu processo principal esteja sendo executado, assim que esse processo finalizar o container também será removido.
    
    `docker container run --rm hello-world`
    
- **Parar container**: Para a execução de um container

    `docker container stop meu_nginx`
    
- **Iniciar container**: Inicia um container parado

    `docker container start meu_nginx`
    
- **Remover container**: Remove um container existente

    `docker container rm meu_nginx`
    
    **Atenção**: Caso o container esteja em execução é possível usar o parâmetro `-f` (force) para forçar a parada do container antes de removê-lo, caso contrário
    será necessário parar o container antes.
    
    
### Imagens

- **Listar imagens**: Lista todas as imagens baixadas e armazenadas no repositório local

    `docker image ls`
    
- **Baixar imagem**: Baixa uma imagem do repositório remoto para o local

  `docker image pull nginx`
  
- **Remover imagem**: Remove uma imagem do repositório local

    `docker image rm nginx`
    
    **Atenção**: Caso existam containers ou outras imagens relacionados a essa imagem é possível usar o parâmetro `-f` (force) para forçar a remoção dos containers e imagens
    relacionados antes de removê-la, caso contrário deverá executar todos os passos de remoção dos containers e imagens relacionadas antes.
    
    
### Volumes

- **Listar volumes**: Lista todos os volumes

    `docker volume ls`
    
- **Criar volume**: Cria um volume a ser mapeado em containers

    `docker volume create meu_volume`
    
- **Remover volume**: Remove um volume existente

    `docker volume rm meu_volume`
    
    
### Network

- **Lista networks**: Lista todas as networks

    `docker network ls`
    
- **Criar network**: Cria uma nova network

    `docker network create minha_network`
    
    É possível usar o parâmetro `-d` para indicar o driver que será usado nessa network (mais informações na sessão [Network](#network))
    
- **Remover network**: Remove uma network

    `docker network rm minha_network`
    
    **Atenção**: Caso existam containers atrelados a essa network não será possível removê-la até que os container sejam movidos para outra network ou removidos
    

### Execute

- **Executar comando no container**: Executa um comando específico dentro de um container em execução

   `docker container exec meu_nginx echo "TESTE"`
   
   É possível executar um terminal dentro do container permitindo que acesse o contexto interno do container. 
   Para tal é necessário usar os parâmetros `-it` para executar o comando em modo iterativo dentro do container e passar como parâmetro o comando de execução do terminal, 
   que normalmente são `bash` ou `sh` dependendo da distribuição linux a qual o container é baseado
   
   `docker container exec -it meu_nginx bash`
    
    
## Docker Hub
 
Docker Hub é o repositório central e público de imagens do Docker. Toda vez que algum comando do Docker necessitar de uma imagem, o Docker CLI irá inicialmente 
procurar a imagem no repositório local, caso não encontre, irá fazer um pull (baixar) a imagem do Docker Hub para o repositório local, para só então usar a imagem

É possível também criar suas próprias imagens personalizadas e publica-las no Docker Hub. Para isso é necessário seguir os passos:
 1. Criar uma conta grátis no Docker Hub
 2. Logar no Docker Hub pelo CLI local usando o comando `docker login`, passar as credenciais da sua conta
 3. Taguear sua imagem com seu nome de usuário com o comando `docker image tag minha_imagem:1.0 meu_username/minha_imagem:1.0` 
 4. Executar o push (upload) da sua imagem personalizada na sua conta usando o comando `docker image push meu_username/minha_imagem:1.0`


## Volumes

Os volumes são utilizados quando se quer mapear uma pasta ou arquivo do container para o host com intuito de manter os dados salvos mesmo que o container seja removido.
Como informado nas sessões anteriores, um dos recursos que os containers isolam do host é o filesystem, ou seja, as estruturas de pastas e arquivos do container existem somente dentro dele, o host não tem acesso. Para que seja possível o host ter acesso a esses arquivos, ou que dados de um container seja compartilhado com outros containers, ou ainda que os dados de um container sejam salvos no disco do host para não serem perdidos após o container ser removido, existem duas opções de mapeamento no momento da criação do container usando o parâmetro `-v`:
 
 1. Efetuar o mapeamento de uma pasta do host com uma pasta do container diretamente usando o comando:
  
   `docker container run --name some-nginx -v /alguma/pasta/do/host:/usr/share/nginx/html:ro -d nginx`
   
 2. Criar um volume nomeado e utiliza-lo para efetuar o mapeamento usando os comandos:

   ```
   docker volume create meu_volume
   docker run --name some-nginx -v meu_volume:/usr/share/nginx/html:ro -d nginx
   ```
   
Nos dois exemplos está sendo usado o modificador de acessibilidade `:ro` que indica que esse mapeamento será read-only, ou seja, o container não terá permissão para modificar o conteúdo do mapeamento, somente o host tem o controle. Para que seja possível essa escrita deve-se remover o modificador `-v meu_volume:/usr/share/nginx/html`, com isso, tanto o container quanto o host tem acesso total ao conteúdo do volume

**Atenção**: Ao mapear um volume no container, o Docker irá sobrescrever o conteúdo original da pasta ou arquivo mapeado do container com o mapeado no host, portanto, deve-se tomar cuidado com a sobrescrita de arquivos de configuração e de mapeamentos dentro do container o que pode causar o seu mau funcionamento.


## Variáveis de Ambiente

É possível criar variáveis de ambiente dentro dos container no momento da criação dos mesmos usando o parâmetro `-e`

  ```
  docker container run -d -e MINHA_VARIAVEL=teste --name meu_nginx nginx
  docker container exec meu_nginx bash -c "echo $MINHA_VARIAVEL"
  ```
  
Também é possível usar um arquivo no formato chave-valor contendo todas as variáveis de ambiente que serão criadas no container e seus valores

  *variaveis.txt*
  ```
  VAR1=teste1
  VAR2=teste2
  ```
  
  ```
  docker container run -d --env-file ./variaveis.txt --name meu_nginx nginx
  docker container exec meu_nginx bash -c "echo $VAR1; echo $VAR2"
  ```
  
Pode-se também combinar as duas opções extendendo a criação de variáveis de ambiente de um arquivo com variáveis adicionais em tempo de criação do container.
Usando variáveis de ambiente deixa-se a imagem do container parametrizável e independente para qualquer ambiente que seja executado, aderindo assim ao conceito "code once, run anywhere"


## Network

Docker faz o isolamento da rede interna dos containers com a rede externa do host, ou seja, ele cria um rede virtual com faixas de IPs específicas.

Ao instalar o Docker são criadas automaticamente 3 tipos de networks: bridge, host, none.

- Bridge: esse tipo de interface de rede isola a rede virtual dos containers da rede do host, no entanto, é possível cria um "ponte" entre os processos rodando nos containers e a rede do host usando as configurações de bind de portas. Essa é a interface padrão que o Docker usa para criar os containers.
- Host: nesse tipo de iterface de rede os containers usam a rede do host para rodar seus processos e fazer bind das portas, ou seja, não há isolamento entre a rede do container e a rede do host
- None: esse tipo de interface é usado quando não se deseja usar rede nos containers

É possível criar novas rede virtuais baseadas nesses 3 tipos de interfaces, criando assim uma faixa de IP para cada grupo de containers, podendo, portanto, isolar ambientes por networks, como dev, qa, uat, stage, prod, etc

Para criar uma newtork é possível usar o seguinte comando:
   
   `docker network create minha_network`
   
Para especificar uma network ao criar um container é possível usar o seguinte parâmetro:

   `docker container run -d --network minha_network --name meu_nginx nginx`
   

## Dockerfile

Dockerfile é um arquivo usado pelo Docker como uma receita para criar uma imagem que posteriormente irá gerar containers.
Nesse arquivo é possível organizar várias configurações e comandos que serão executados toda vez que criar um container.
O arquivo sempre iniciar com o comando FROM que indica qual á a imagem base que será usada para criação da sua imagem.

Os camandos mais comuns utilizados são os seguintes:

- FROM: Indica qual a imagem base que será utilizada

- RUN: Usado para executar um comando na imagem antes de subir o container

- CMD: Usado para executar processos dentro do container

- EXPOSE: Portas de processos que rodam dentro do container que podem ser expostas para o host

- ENV: Usado para configurar variáveis de ambiente dentro do container

- COPY: Usado para copiar arquivos e diretórios do host para a imagem

- ADD: Também é usado para copiar arquivos e diretórios, porém, é possível usar URLs na origem, bem como descompactar arquivos

- ENTRYPOINT: Comando executado no container que será considerado o processo principal do mesmo, ou seja, caso esse processo seja interrompido o container é automaticamente terminado.

- VOLUME: Usado para criar volumes dentro do container que podem ser mapeados para o host

- WORKDIR: Usado para indicar a pasta principal que os comandos serão executados dentro da imagem

- ARG: Usado para indicar argumentos que podem ser passados no momento da criação da imagem

Exemplo de um Dockerfile:

   ```bash
   FROM node:12-alpine
   RUN apk add --no-cache python g++ make
   WORKDIR /app
   COPY . .
   RUN yarn install --production
   CMD ["node", "src/index.js"]
   ```
   
Para se criar uma imagem baseado nesse Dockerfile usa-se o seguinte comando:

`docker image build -t minha_imagem:v1 .`

Referência sobre boas práticas de criação do Dockerfile: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
