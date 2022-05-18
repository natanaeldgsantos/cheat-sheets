# DOCKER CHEAT SHEET

## INSTALAÇÃO E CONFIGURAÇÃO DO AMBIENTE


**Como instalar Docker no Ubuntu**

https://docs.docker.com/engine/install/ubuntu/

**verifica a versão do sistema operacional**

`lsb_release -a`

**Dar permissões de admin para o usuário executar o docker**

`sudo usermod -aG docker $USER`


## IMAGENS

**Realiza o download de uma imagen direto do Docker Hub**

`docker pull nome_da_imagen #(exemplo docker pull ubuntu)`

**Cria sua própria imagem com base em um Dockerfile**

`sudo docker build -t <nome da imagen> .` # o ponto significa que o build deve considar um Dockerfile que está na mesma pasta.

`sudo docker build -t <nome da imagen> -f <caminho até Dockerfile desejado>` # executa qualquer outro Dockerfile específico

**Lista todas as imagens locais**

`sudo docker images`

**Descreve a Imagen**

`docker inspect <nome ou id da imagem>`

**Usando comandos bash para filtrar o detalhamento da imagen**

`sudo docker inspect <nome ou id da imagen> | grep "ENV" # termo de busca, por exemplo variáveis de ambiente Env`

**Detalha as camadas da Imagen**

`docker history id_da_imagen`

**Deletando uma imagen**

`sudo docker rmi <nome ou id da imagem>`

**Deletando todas as imagens locais de uma vez**

`sudo docker rmi -f $(sudo docker images -aq)`

## CONTAINERS

**Executa um container**

Se existir localmente executa, senão realiza o download, valida e executa

`docker run --name <nome do container> <nome da imagem>`

dica: para executar o container em background sem travar o terminado use o comando -d, exemplo:

`docker run -d <nome da imagens>`

**Lista todos os containers**

	sudo docker ps
	sudo docker ps -a # lista inclusive containeres que não estão em execução

	sudo container ls 
	sudo container ls -a # lista inclusive containeres que não estão em execução

**Docker Cheat Sheet Download**
	https://www.docker.com/sites/default/files/d8/2019-09/docker-cheat-sheet.pdf

**Como para a execução do container**
	
`docker stop id_do_container`

**Parando todos os containers em execução**
	
`docker stop $(docker container ls -q)`

**Inicializar um container já criado**
	
`docker start id_do_container`

**Pausando a execução do container**
	
`docker pause id_do_container`

**Reativando o container pausado**	

`docker unpause id_do_container`

**Removendo o container**
	
`docker rm id_do_container`

**Parando e removendo o container ao mesmo tempo**

`docker rm id_do_container --force`

	docker container rm $(docker container ls -aq) --force # remove todos os container inclusive os que não estão em execução

### INTERAGINDO COM O CONTAINER

**Executando um comando no modo interativo**

`docker exec -it id_do_container comando`

por exemplo, execute o comando:

`docker exec -it id_do_container bash` em um container ubuntu para acessar o terminal de forma interativa

outro exemplo: 

`doccker exec -it <NOME ou ID> /bin/bash`

**Exibindo logs do Container**
	
`docker logs <Nome ou ID>`

## NETWORK

**Mostrar o mapeamento de portas do container**
	
`docker port id_do_container`

**Mapeando portas ao criar o container**

`docker run -d -P nome_da_imagen`

Detecta portas do container e vincula portas do host automaticamente

`docker run -d -p port_host:porta_container nome_da_imagen`

atribui uma porta especifica do host para uma porta do container

### Configurando uma Network

	# 3 opções de rede 
		--Bridge: 	- A rede bridge é usada para comunicar containers em um mesmo host;
					- Redes bridges criadas manualmente permitem comunicação via hostname;
		--Host: Conecta o container a mesma rede do host
		--None: de forma explicita declara que container não estará conectado a nenhuma rede

	# Inspecionando detalher de um container
	docker inspect id_do_container	
	
	**Consultando o endereço IP do container**
	``sudo docker inspect <nome do container> | grep IPAddress`

	#Criar própria rede
	docker network create --driver bridge nome_rede

	# Atribuindo um nome e nova rede ao container
	docker run -it --name nome_container --network nome_rede ubuntu bash

## VOLUMES

**Bind Mounts - ao rodar um docker escreve em alguma pasta do Host**
**Volume - criar espaço reservado para armazenamento**

		# Bind Mounts
		docker run -it -v  /path_to_host_directory:/app ubuntu bash # tudo que for armazenado no diretório app do container será persistido na pasta do host
		docker run -it --mount type=bind, source =/path_to_host_directory:/path_to_container_directory, target=/app ubuntu bash

		# Volumes

			# Lista todos os volumes existentes
			docker volume ls

			#Cria um novo volume
			docker volume create nome_volume

			#Associando um novo volume
			docker run -it -v nome_volume:/app ubuntu bash
			docker run --mount source=nome_volume, target =/app ubuntu bash


**Subindo um novo servidor MySQL de Exemplo**

Subindo um container com MySQL

`sudo docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql:5.7`

Subindo um container com servidor MySQL e atribuindo um Volume para persistência de dados

`sudo docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -v ${PWD}/volume/mysql:/var/lib/mysql mysql:5.7`

Ao mapear a pasta de configurações do MySQL como acima, mesmo que o container seja deletado, não serão perdidas as tabelas e bancos.

**Deletando Volumes**

Quando criamos uma imagem através de um Dockerfille é possível criar um volume a ser utilizado dentro do container 

	FROM postgres:13
	ENV POSTGRES_USER=root POSTGRES_PASSWORD=root POSTGRESS_DB=ny_taxi
	VOLUME /my_data

Entretando ao se especificar o volume dentro do Dockerfile não é possível indicar o local do respositorio na máquina host. por padrão o repositorio host será sempre gravado em: ´/var/lib/docker/volumes´

Para deletar também os volumes associados ao deletar o container use:

`sudo docker rm -v <nome ou id do container>`

**Lista Volumes**

`sudo docker volume ls`

**Remove um ou mais volumes**

`sudo docker volume rm <nome ou id do volume>`

**Remove todos os volumes sem utilização (Atenção)**

´sudo docker volume prune´

