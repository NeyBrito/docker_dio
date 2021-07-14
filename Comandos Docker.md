##Instalar explorador de imagem
#https://github.com/wagoodman/dive
wget https://github.com/wagoodman/dive/releases/download/v0.9.2/dive_0.9.2_linux_amd64.deb
sudo apt install ./dive_0.9.2_linux_amd64.deb

# Docker RUN
# Comando utilizado para criar um container
docker run --name newcontainer hello-world
docker run --name hello -d busybox sleep 3600
docker run --name site -d -p 80:80 nginx

#Docker PS
#Lista os container em execução, para listar os que não estão precisamos colocar o parâmetro -a
docker ps -a

#Docker INFO
#Exibe um sumuário dos nossos container, como também especificações do nosso docker
docker info

#Docker EXEC //Acesso ao container
#Adiciona um processo a mais no container
#Vamos criar uma pasta dentro do container
docker exec hello mkdir teste 

docker exec -it hello sh

# Acessamos o container com o servico SH
docker exec -it hello sh

#Docker STOP, START
#Paramos nosso container
docker stop hello
#Iniciamos nosso container
docker start hello

#Docker LOGS
#coletamos o output do nosso container, ótimo para debugar uma aplicação
docker logs site

#Docker PULL
docker pull hello-world

#Docker COMMIT
docker commit --author="Luis Miguel" --message="Imagem com commit" hello hello

#Docker TAG
#Preparando para docker hub
docker tag hello luistkd4/hello:1.0
#Trocando um nome de um repositorio
docker tag hello-world ola-mundo

#Docker LOGIN,LOGOUT
#Logar no repositório local, ou público. Por padrão logamos no Docker HUB
docker login <usuário>

#Docker PUSH
#Vamos versionar nosso repositório/imagem ao docker hub
docker push luistkd4/hello:1.0

#Docker SEARCH
#Procura por uma imagem no repositório
docker search <consulta>

#Docker RM
#Remove um container previamente parado
docker rm newcontainer
#para remover um container em execução é nesessário o parâmetro -f
docker rm -f site

#Docker RMI
#Remove um repositório/imagem local, se algum container estiver parado que utiliza essa imagem deverá passar o parâmetro -f
docker rmi hello-world

#Docker EXPORT,IMPORT
#Exporta o container mergeando as suas camadas
docker export hello > export.tar
#Importa o arquivo gerado, criando uma imagem do container a partir dela
cat export.tar | docker import - hello-export:latest





curl localhost // Conteudo da página



## Comandos Rede



- Bridge 
  - docker network  create -d bridge <nome da rede> = criar uma rede
  - docker network ls = listar as redes 
    - docker run -d --net petsBridge --name db consul = criando um container na rede
    - docker run -d --env "DB=db" --net petsBridge --name web -p 8000:5000 chrch/docker-pets:1.0 = criando a imagem



- Hotst

  - docker run -d --net host --name db consul = criando um container na rede host

  - docker run -d --env "DB=db" --net petsBridge --name web -p 8000:5000 chrch/docker-pets:1.0 = criando a imagem 

    

### Docker Swarm 

- docker swarm init --advertise-addr 192.168.0.28 = inicializando o swarm no servidor
- docker swarm join --token <token gerado> 192.168.0.28:2377 = se juntando como worker no swarm

- Overlay

  - docker network create -d overlay petsOverlay = criando uma rede overlay

  - docker service create --net petsOverlay --name db consul = criando backend

  - docker service create --netwok petsOverlay -p 8000:5000 -e "DB=db" --name web chrch/docker-pets:1.0 = frontend

- docker sevice scale web=3 = escalonar serviços



## Armazenamento 

- __Tipo Volume__

  - docker volume create<nome volume>

  - docker volume inspect <nome do volume>

- Criando container no volume
  - docker run -d -p 80:80 --name <nome do container> --mount source=<nome do volume>,target=/usr/share/nginx nginx
- __Tipo  Bind__
  - docker run -d --name container-bind -p 80:80 -v /html:/usr/share/nginx/html nginx
- __Tipo TMPFS__ 
  - docker run -d --name container-tmpfs --mount type=tmpfs,destination=/cache,tmpfs-size=1000000 
    - dd if=/dev/zero of=1mb.file bs=1024 count=1024 = criando um arquivo dentro da pasta cache



## Limites

- CPU
  - docker run -d --rm progrium/stress -c 8 -t 30s
  - docker stats
- Memória
  - docker run -d --memory 10m busybox sleep 3600
    - free -m = ver memoria disponivel



