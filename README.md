# ESTUDOS DOCKER

Esse é um repositório que explica como Docker funciona e como utilizalo ele.

## O QUE É DOCKER?

Docker é uma ferramenta que permite criar, organizar e rodar aplicações dentro de containers.

Ele ajuda a garantir que um programa funcione da mesma forma em qualquer computador, sem problemas de configuração.

Com o Docker, você não precisa instalar várias coisas manualmente no seu sistema, pois tudo que a aplicação precisa já vem dentro do container.

De forma simples:
- você cria uma imagem (modelo)
- roda um container (aplicação funcionando)

O principal objetivo do Docker é facilitar o desenvolvimento, testes e execução de aplicações de forma rápida e consistente.

## O QUE É IMAGENS?

Uma imagem é como um "modelo pronto" de um sistema ou aplicação.  
Ela contém tudo o que é necessário para rodar algo: código, bibliotecas e configurações.

Pense como uma receita de bolo: ela não é o bolo pronto, mas contém tudo para fazer um.

## O QUE É UM CONTAINER?

Um container é a execução de uma imagem.  

Ou seja, é a aplicação rodando de verdade.

Se a imagem é a receita, o container é o bolo pronto.

Containers são leves, rápidos e isolados, ou seja, cada um roda separado dos outros.

## O QUE É DOCKERFILE?

O Dockerfile é um arquivo onde você descreve como criar uma imagem.

Nele você define:
- qual sistema base usar
- quais programas instalar
- o que sua aplicação precisa para funcionar

É basicamente um passo a passo automático para montar uma imagem.

## O QUE É DOCKER-COMPOSE?

O docker-compose é uma ferramenta que ajuda a rodar vários containers juntos.

Por exemplo:
- um container para o backend
- outro para o banco de dados
- outro para o frontend

Em vez de iniciar tudo manualmente, você define tudo em um arquivo e executa com um comando só.

## O QUE É VOLUMES?

Volumes são usados para guardar dados de forma persistente.

Isso significa que, mesmo que o container seja apagado, os dados continuam salvos.

Exemplo:
- banco de dados
- arquivos enviados por usuários

Sem volumes, você perderia tudo ao remover o container.

## O QUE É REDES NO DOCKER?

Redes permitem que os containers conversem entre si.

Por exemplo:
- um container do backend acessa o banco de dados em outro container

O Docker cria redes para organizar essa comunicação de forma segura e simples.

## DOCKER É UMA VM?

Não, Docker não é uma máquina virtual (VM).

Diferença simples:
- VM: simula um computador inteiro (mais pesado)
- Docker: compartilha o sistema e roda só o necessário (mais leve e rápido)

Ou seja, Docker é mais eficiente e consome menos recursos que uma VM.

## COMANDOS:

```bash
# Comandos Básicos (uso no dia a dia):

docker pull <nome-da-imagem>    # Baixa uma imagem do Docker Hub

docker images    # Lista imagens disponíveis

docker run <nome-da-imagem>    # Cria e roda um container

docker run -d --name <nome-do-container> <nome-da-imagem>    # Roda em segundo plano com nome

docker ps -a    # Lista todos os containers

docker stop <id-ou-nome-do-container>    # Para um container

docker start <id-ou-nome-do-container>    # Inicia um container parado

docker rm <id-ou-nome-do-container>    # Remove um container

# Comandos Intermediários (controle maior dos containers):

docker pull <nome-da-imagem>:<tag>    # Baixa uma versão específica

docker run <nome-da-imagem>:<tag>    # Roda versão específica da imagem

docker run -it <nome-da-imagem> <comando>    # Modo interativo (terminal dentro do container)

docker run -d --name <nome-do-container> -p <porta-host>:<porta-container> <nome-da-imagem>    # Mapeia portas

docker exec -it <id-ou-nome-do-container> <comando>    # Executa comando dentro de container rodando

docker stats    # Mostra uso de recursos em tempo real

# Comandos Avançados (manutenção e criação de imagens):

docker rmi <nome-da-imagem>:<tag>    # Remove imagem por nome

docker rmi <id-da-imagem>    # Remove imagem por ID

docker build .    # Cria imagem a partir do Dockerfile

docker build -t <nome-da-imagem>:<tag> .    # Cria imagem com nome e versão

docker system prune -a    # Limpa tudo que não está sendo usado (cuidado!)
```

## EXEMPLO DE USO:

### CRIE UMA PASTA:

```bash
mkdir app
cd app
```

### INICIALIZE O PROJETO:

```bash
npm init -y
```

Isso cria o `package.json`.

## INSTALE AS DEPENDÊNCIAS

Vamos criar uma API simples usando Express:

```bash
npm install express
```

## CRIANDO O CÓDIGO DA APLICAÇÃO

Crie um arquivo chamado:

```bash
app.js
```

Conteúdo:

```javascript
const express = require('express');
const app = express();

const PORT = 3000;

app.get('/', (req, res) => {
  res.send('Minha app Node rodando no Docker!');
});

app.listen(PORT, () => {
  console.log(`Servidor rodando na porta ${PORT}`);
});
```

## AJUSTANDO O PACKAGE.JSON

Edite o `package.json` e adicione:

```json
"scripts": {
  "start": "node app.js"
}
```

## TESTANDO LOCALMENTE (ANTES DO DOCKER)

```bash
npm start
```

Acesse:

```
http://localhost:3000
```

Se isso funcionar, o Docker também vai funcionar.

## CRIANDO O DOCKERFILE

Agora crie o arquivo:

```bash
Dockerfile
```

Conteúdo:

```Dockerfile
FROM node:20

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

## CRIANDO O .DOCKERIGNORE

Crie:

```bash
.dockerignore
```

Conteúdo:

```txt
node_modules
.git
.gitignore
```

## BUILDANDO A IMAGEM

```bash
docker build -t api-express .
```

## RODANDO O CONTAINER

```bash
docker run -p 3000:3000 api-express
```

Agora abra:

```
http://localhost:3000
```

## O QUE ACONTECEU

Você:

1. Criou um projeto Node.js
2. Instalou dependências
3. Criou um servidor HTTP
4. Empacotou tudo com Docker
5. Rodou a aplicação dentro de um container isolado
