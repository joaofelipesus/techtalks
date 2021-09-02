# Docker

## Índice

- [Conceitos](#conceitos)

- [Comandos Básicos](#comandos-básicos)

- [Docker Hub](#docker-hub)

- [Volumes](#volumes)

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
   docker volume create meu_volume;
   docker run --name some-nginx -v meu_volume:/usr/share/nginx/html:ro -d nginx;
   ```
   
Nos dois exemplos está sendo usado o modificador de acessibilidade `:ro` que indica que esse mapeamento será read-only, ou seja, o container não terá permissão para modificar o conteúdo do mapeamento, somente o host tem o controle. Para que seja possível essa escrita deve-se remover o modificador `-v meu_volume:/usr/share/nginx/html`, com isso, tanto o container quanto o host tem acesso total ao conteúdo do volume

**Atenção**: Ao mapear um volume no container, o Docker irá sobrescrever o conteúdo original da pasta ou arquivo mapeado do container com o mapeado no host, portanto, deve-se tomar cuidado com a sobrescrita de arquivos de configuração e de mapeamentos dentro do container o que pode causar o seu mau funcionamento.
