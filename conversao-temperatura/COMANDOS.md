# Projetos DevOps - Docker



## Principais comandos



## IMAGES

- Instalando imagens em linha de comando

```
$ docker run hello-world
$ docker run ubuntu
$ docker run mysql
```

- Uso do Dockerfile

```
FROM ubuntu
RUN apt update
RUN apt install curl -y
```

- Lista imagens

```
$ docker images
ou
$ docker image ls
```

- Limpa imagens null

```
$ docker image prune
```

- Push para Docker Hub

```
# Renomeie a imagem para o padrão aceito pelo Docker Hub
$ docker tag ubuntu-curl robsontazinaffo/ubuntu-curl:v1

$ docker login

# Sobe a imagem para o Docker Hub
$ docker push robsontazinaffo/ubuntu-curl:v1

# Copia e cria nova tag latest
$ docker tag robsontazinaffo/ubuntu-curl:v1 robsontazinaffo/ubuntu-curl:latest

# Sobe a imagem para o Docker Hub
$ docker push robsontazinaffo/ubuntu-curl:latest
```



## Conversão de Temperatura



[Link Fontes Conversão de Temperatura](https://github.com/KubeDev/conversao-temperatura)



- Dockerfile

  ```
  FROM node
  WORKDIR /app
  COPY package*.json ./
  RUN npm install
  COPY . .
  EXPOSE 8085
  CMD [ "node", "server.js"]
  ```


- Build

  ```
  $ docker build -t robsontazinaffo/conversao-temperatura:v1 .
  ```

- Tag para nova imagem conversao-temperatura

```
$ docker tag robsontazinaffo/conversao-temperatura:v1 robsontazinaffo/conversao-temperatura:latest
```

- Subindo imagens criadas para o Docker Hub

  ```
  $ docker push robsontazinaffo/conversao-temperatura:v1
  $ docker push robsontazinaffo/conversao-temperatura:latest
  ```

  

- Montando imagens a partir do docker hub com a imagem conversao-temperatura criada

  ```
  # Antes eliminar todos os containers e imagens 
  $ docker container rm -f <id_container>
  $ docker image rm -f <id_imagem>
  
  # Gerar imagem e container novamente com a imagem criada em Docker Hub
  $ docker container run -d -p 8085:8080 robsontazinaffo/conversao-temperatura:v1
  
  # Listar para verificar se deu tudo certo
  $ docker images
  $ docker container ls
  ```

  
