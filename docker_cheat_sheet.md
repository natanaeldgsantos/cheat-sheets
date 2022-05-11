# DOCKER CHEAT SHEET

### INSTALAÇÃO E CONFIGURAÇÃO DO AMBIENTE


**Como instalar Docker no Ubuntu**

https://docs.docker.com/engine/install/ubuntu/

**verifica a versão do sistema operacional**

`lsb_release -a`

**Dar permissões de admin para o usuário executar o docker**

`sudo usermod -aG docker $USER`


### Baixando e Executando Imagens

**Realiza o download de uma imagen direto do Docker Hub**

`docker pull nome_da_imagen #(exemplo docker pull ubuntu)`

**Executa um container**

Se existir localmente executa, senão realiza o download, valida e executa

`docker run nome_da_imagen`

dica: para executar o container em background sem travar o terminado use o comando -d, exemplo:

`docker run -d nome_da_imagen`

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

### CONFIGURANDO PORTAS

**Mostrar o mapeamento de portas do container**
	
`docker port id_do_container`

**Mapeando portas ao criar o container**

`docker run -d -P nome_da_imagen`

Detecta portas do container e vincula portas do host automaticamente

`docker run -d -p port_host:porta_container nome_da_imagen`

atribui uma porta especifica do host para uma porta do container


### IMAGENS
	
**Lista todas as imagens baixadas**

`docker images ls`

**Descreve a Imagen**

`docker inspect id_da_imagen`

**Detalha as camadas da Imagen**

`docker history id_da_imagen`

**Criando uma nova imagen**

	# DockerFile --> Imagen --> Container

		# Crie um arquivo Dockerfile
		# Dockerfile references: https://docs.docker.com/engine/reference/builder/

			FROM node:14
			WORKDIR /app-node
			EXPOSE 3000 #opcional indica que a aplicação estará exposta, rodando na porta 3000 ou qualquer outra
			COPY . . # copia todo contéudo do diretório atual para o node
			RUN npm install
			ENTRYPOINT npm start
		# Gerar uma imagens a partir de um Dockerfile
		docker build -t nome_da_imagen/app-node:1.0 .

**Constroi a nova Imagen**

`sudo docker build -t <nome_da_imagen> . # sinaliza que o arquivo Dockerfile esta no mesmo local`

Por padrão o docker vai procurar o arquivo chamado Dockerfile para realizar o build

Para utilizar algum outro arquivo com outro nome você precisa passar o parâmetro -f

`sudo docker build -t <nome_da_imagens> -f Dockerfiler-outro-nome`


### PERSISTINDO DADOS OU VOLUMES

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


### COMUNICAÇÃO E REDES

	# 3 opções de rede 
		--Bridge: 	- A rede bridge é usada para comunicar containers em um mesmo host;
					- Redes bridges criadas manualmente permitem comunicação via hostname;
		--Host: Conecta o container a mesma rede do host
		--None: de forma explicita declara que container não estará conectado a nenhuma rede

	# Inspecionando detalher de um container
	docker inspect id_do_container	

	#Criar própria rede
	docker network create --driver bridge nome_rede

	# Atribuindo um nome e nova rede ao container
	docker run -it --name nome_container --network nome_rede ubuntu bash

### DOCKER COMPOSE - Coordenação de Containers

	# Instalando docker compose no linux. 
	--referência: https://docs.docker.com/compose/install/


	# Crie o arquivo docker-compose.yml
	# para executar o arquivo va até o diretório do mesmo e execute:
	docker-compose up

	# lista servicos criados com docker-compose
	docker-compose ps 

	# encerrar servicos criados com docker-compose
	docker-compose down











