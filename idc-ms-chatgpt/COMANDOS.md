![](..\img\Logo-Imersao.png)



- [x] Primeiro criar o cluster Kubernetes com o K3D fazendo o port bind das 2 portas para a aplicação
- [x] Construir a imagem docker da aplicação chatservice
- [x] Aplicar o manifesto do chatservice
- [x] Aplicar o manifesto do Keycloak
- [ ] Alterar o arquivo host da máquina para adicionar o host keycloak 
- [ ] Construir a imagem docker da aplicação webapp
- [ ] Aplicar o manifesto da aplicação webapp



[TOC]

## **Passos para subir a aplicação:**



**Links úteis**

Link para o DBeaver:

https://dbeaver.io/download/

Link para a plataforma da OpenAI:

https://platform.openai.com/

Link para o postman:

https://www.postman.com/

Caminho do arquivo host nos sistemas operacionais:

Windows - C:\Windows\System32\drivers\etc\hosts

Linux - /etc/hosts

Mac - /etc/hosts

Adicionar a linha abaixo:

127.0.0.1       keycloak



### **1-Primeiro criar o cluster Kubernetes com o K3D fazendo o port bind das 2 portas para a aplicação**.

``` bash
k3d cluster create meucluster -p "3000:30000@loadbalancer" -p "8080:30001@loadbalancer"
```



### **2-Construir a imagem docker da aplicação chatservice**

```
# Pasta /idc-ms-chatgpt/chatservice$

$ docker build -t robsontazinaffo/imersao-chatservice:v1 .
$ docker image ls
$ docker push robsontazinaffo/imersao-chatservice:v1

$ docker tag robsontazinaffo/imersao-chatservice:v1 robsontazinaffo/imersao-chatservice:latest
$ docker image ls
$ docker push robsontazinaffo/imersao-chatservice:latest
```



### **3-Aplicar o manifesto do chatservice**

```
# Voltar uma pasta
$ cd ..

$ kubectl apply -f k8s/deploy-chatservice.yaml
$ kubectl get po
$ watch 'kubectl get po'
```



### **4-Aplicar o manifesto do Keycloak**

```
$ kubectl apply -f k8s/deploy-keycloak.yaml
$ kubectl get po

http://keycloak:8080/
```



### **5-Alterar o arquivo host da máquina para adicionar o host keycloak** 

```
127.0.0.1 keycloak

http://keycloak:8080/
```



### **6-Construir a imagem docker da aplicação webapp**

```
$ docker build -t robsontazinaffo/imersao-gpt-webapp:v1 .

# Fazendo um push para o Docker Hub
$ docker push robsontazinaffo/imersao-gpt-webapp:v1

# Criando a imagem latest
$ docker tag robsontazinaffo/imersao-gpt-webapp:v1 robsontazinaffo/imersao-gpt-webapp:latest

# Fazendo um push para o Docker Hub
$ docker push robsontazinaffo/imersao-gpt-webapp:latest

# Deleta o pod e é recriado automaticamente
$ kubectl logs <seu-pod-chatservice>
$ kubectl delete pod <seu-pod-chatservice>
```



### **7-Aplicar o manifesto da aplicação webapp**

```
$ cd ..
$ kubectl apply -f k8s/deploy-webapp.yaml
$ watch 'kubectl get po'

# Ver o log para ver se está dando algum erro, e se tiver, deleta que ele cria novamente
$ kubectl logs <seu-pod-chatservice>
$ kubectl delete pod <seu-pod-chatservice>

$ kubectl get po
$ kubectl get svc
```

