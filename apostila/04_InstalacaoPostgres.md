# Instalação do PostgreSQL com uso de Container

## Introdução

Como tantas outras tecnologias, o PostgreSQL também pode ser em containers Docker. A tecnologia de containers possibilita que ambientes de desenvolvimento, testes e  produção executem de forma idêntica independente de características que existem somente em um ou outro ambiente. O uso de container impede que a situação do
software que executa sem erros na máquina do desenvolvedor, porém, ao rodar em produção, surgem erros não detectados antes.

Com os _containers_ podemos executar diversas versões do PostgreSQL lado a lado sem que uma atrapalhe a outra.

## Sítio Eletrônico do PostgreSQL no Docker

As imagens dos _containers Docker_ disponíveis do PostgreSQL podem ser acessadas no sítio eletrônico do [docker](https://hub.docker.com). Esse endereço é denominado de hub do Docker.

Neste sítio eletrônico além das imagens, há instruções de como instalar as imagens do _PostgreSQL_.

## Instalando o Docker
O _Docker_ é um gerenciador de _container_ que roda em diversos Sistemas Operacionais (SO). Sua instalação em um determinado SO depende das características do sistema e não será abordado nesta seção. Partiremos do princípio que o Docker já esteja instalado.

Para verificar se o Docker está instalado, dê o comando utilizando um usuário com acesso ao sudo:

```bash
sudo docker images
```

Caso receba uma mensagem de comando não localizado, então instale o _Docker_ em seu SO.

## Instalando Container do PostgreSQL  

Antes de instalarmos o *Postgres*, devemos verificar duas situações:  
  1. Se não temos um *Postgres* instalado localmente e  
  2. A versão do *psql*. Não eh recomendado instalar um Posgres em versão maior que a do *psql*.

### Verificando se temos Postgres instalado localmente

Para verificarmos isso, digite:
```bash
sudo service postgresql status
```

O retorno será:

```console
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/usr/lib/systemd/system/postgresql.service; enabled; preset: enabled)
     Active: active (exited) since Tue ...
    Process: 7808 ExecStart=/bin/true (code=exited, status=0/SUCCESS)
   Main PID: 7808 (code=exited, status=0/SUCCESS)
       CPU: 2ms
```

Caso retorno que há um Postgres ativo, devemos pará-lo com o comando:

```bash
sudo service postgresql stop
```

### Verificando a versão do *psql*

Para fazer essa verificação, digite no prompt:

```bash
psql --version
```

Vai aparecer algo como:

```console
psql (PostgreSQL) 16.13 (Ubuntu 16.13-0ubuntu0.24.04.1)
```


Para baixar uma versão do PostgreSQL, no hub do Docker, verifique qual a versão do Postgres que deseja instalar e digite:

1. Baixe a imagem para a máquina local com o coando docker pull:
```bash
sudo docker pull postgres:XX.X
Onde:
XX.X: é a versão do SGBD que você deseja instalar.
```

2. Verifique a imagem baixada:
```bash
docker images
```

3. Vamos iniciar o container e rodar o comando docker run uma única vez para configurar o PostgreSQL.
```bash

sudo docker run -p 5432:5432 --name NomeDoMeuContainer -d -e POSTGRES_PASSWORD=postdba postgres:XX.X

onde:
-p 5432(e):5432(i):
  A porta 5432(e) é a porta externa, a que será acessada por aplicações que desejam se conectar no PostgreSQL.
  A porta 5432(i) é a porta interna do Postgres dentro do container.

-d: sobre o container sem travar o prompt

--name NomeDoMeuContainer: é o nome de seu container.

postgres:XX.X: é o nome da imagem que baixamos.
```

Após instalar, verifique se o container está rodando com:

```bash
sudo docker ps
```

Verifique o container com o nome *NomeDoMeuContainer*.


##  Instalando o *PostgreSQL* com o banco de dados fora do *container*

A maneira que configuramos o PostgreSQL , os dados são armazenados internamente dentro do *container*. Se o *container* estiver parado, não temos mais acesso aos dados. Se removermos o *container*, perde-se todos os dados.

Para evitar estas situações, podemos configurar o *container* para gravar os dados localmente no computador, fora do container. A configuração deve ser feita quando configuramos o container pela primeira vez. Para isso, faça:

1. Vamos configurar o *container* indicando que os dados irão ficar guardados em */var/lib/database*. Primeiro, devemos criar o diretório e depois rodar o *docker run*:
```
sudo mkdir /var/lib/database

docker run -p 5432:5432 --name NomeDoMeuContainerBDLocal -d -v /var/lib/database:/var/lib/postgresql/data -e POSTGRES_PASSWORD=postdba postgres:XX.X
```

## Iniciando e parando um *container* Docker

Após o *Postgres* estar instalado, pode-se subir ou descer a imagem pelos comandos:

1. Para iniciar o container postgresXX.X
```bash
sudo docker start postgresXX.X
```

2. Para parar o container postgresXX.X
```bash
sudo docker stop postgresXX.X
```

## Finalizando
Vimos neste capítulo, como instalar o *PostgreSQL* em dentro de container. Este tipo de instalação oferece muitos benefícios e deve ser utilizado sempre que possível. Evite instalar o *PostgreSQL* diretamente no computador, prefira *containers*.

## Exercícios

1. Rode o *script* de criação do banco de dados da aula dentro do container criado com o banco de dados local. Você encontra o script no AVA Moodle ou no repostitório da disciplina no 8Github*.
