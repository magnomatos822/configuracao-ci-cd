# Pipeline CI/CD

## Atualizando máquina
```console
sudo apt update -y && sudo apt upgrade -y
```

##  Instalar openjkd
```console
sudo apt install openjdk-11-jdk -y
```

## Instalar Jenkins em container
```console
docker run -u 0 -d --name jenkins -p 8081:8080 -p 50000:50000 -v /VOLUMES/jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock jenkins/jenkins:lts
```
## Criar Docker Registry
```console
docker run -d -p 5000:5000 -v /VOLUMES/registry:/var/lib/registry  --restart=always --name local-registry registry:2
```
## Fazendo Push para registry local
Devemos tagear apontando para o registry local conforme abaixo se a imagem já houver sido construída.
```console
docker tag <id-da-imagem> localhost:5000/nome-da-imagem:tag
```
## Caso contrário devemos efetuar build, a partir de seu dockerfile, conforme abaixo:
```console
docker build -t localhost:5000/nome-da-imagem:tag -f Dockerfile .

```
## Efetuar push para registry local
```console
docker push localhost:5000/nome-imagem:tag

```
## Efetuar pull de imagem registry local
```console
docker pull localhost:5000/nome-imagem:tag
```

## Implementar interface para registry local
```console
sudo docker run \
  -d \
  --name registry-frontend \
  -p 8083:80 \
  --link local-registry \
  -e ENV_DOCKER_REGISTRY_HOST=local-registry \
  -e ENV_DOCKER_REGISTRY_PORT=5000 \
  konradkleine/docker-registry-frontend:v2
```

## Login e senha Registry
Para logar no registry local execute:
```console
docker login ip-do-registy:porta
```
Usuário e senha padrão é:

Usuário: testuser
Senha: testpassword