# Docker

## Índice

- [Conceitos](#conceitos)

- [Comandos Básicos](#comandos-básicos)

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
