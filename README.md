# Docker Livro de Mão

## Comandos básicos Docker

O comando abaixo lista todos os containers rodando no momento na nossa máquina. Aqui podemos obter informações como o identificador do container, o nome da imagem, o comando executado contra o container, a quanto tempo ele foi criado, as portas que estão expostas e se estão mapeadas, e o nome atribuido ao container.
```shell
docker ps
```

O comando abaixo tem um output igual ao anterior, porem este lista todos os containers, tanto os que estão rodando no momento, quanto os que já foram parados.
```shell
docker ps -a
```

O comando abaixo, dá um pull na imagem chamada hello-world, substitua pelo nome da imagem desejada, e logo em seguida executa o container.
```shell
docker run hello-world
```

O comando abaixo, faz o mesmo que o comando anterior, baixa e executa a imagem do ubuntu, entretanto aqui, veja que podemos sobrescrever o CMD da imagem, informando que queremos entrar no bash, assim que executarmos o container. A flag -it, é uma junção de duas flags:
* i: Modo interativo, no caso, vao manter o bash atachado ao nosso terminal.
* t: Conhecido por TTY, uma vez o bash atachado ao nosso terminal, esta flag nos permitirá digitar comandos do nosso terminal para o terminal do container.
```shell
docker run -it ubuntu bash
```

Para entrar em um container já rodando, podemos faze-lo por meio do comando exec.
```shell
docker exec -it nginx bash
```

Podemos renomear um container com a flag --name
```shell
docker run --name nginx nginx
```

Podemos deixar o container rodando em background com a flag -d.
```shell
docker run -d nginx
```

Quando damos um docker ps -a, temos uma lista com os containers que estão ativos e parados, uma vez que um container está parado, podemos atraves do comando start, levantar o container, informando o id do container ou o nome do mesmo.
```shell
docker start container_name
docker start container_id
```

Se precisamos parar nosso container, podemos faze-lo com o comando stop.
```shell
docker stop container_name
docker stop container_id
```

Se precisamos remover nosso container, o fazemos por meio do comando rm. Se o container estiver rodando, podemos forçar a remoção com a flag -f.
```shell
docker rm container_name
docker rm container_id
docker rm container_id -f
```

Entretanto temos também uma opção que é a flag -rm, que nos permitirá remover o container da lista de containers tão logo o proceso do container for finalizado. Veja que rodamos o container no modo interativo para acessar o bash, e por conta da flag -rm, o container será removido após abandonarmos o terminal.
```shell
docker run -it -rm ubuntu bash
```

Uma vez que um container possua uma porta exposta, podemos mapear uma porta do nosso computador para ela, por meio da flag -p. No caso do comando abaixo, estamos apontando nossa porta 8080 para a porta 80 exposta no container do nginx.
```shell
docker run -d -p 8080:80 nginx
```

### Volumes

Para usar volumes no docker, podemos usar a flag -v ou a flag --mount, ambas terão o mesmo efeito. Com estes comandos, como no exemplo abaixo, o diretório html do meu computador, será mapeada para o diretório html do nginx, assim nossas alterações neste diretório irão persistir.
```shell
docker run -d --name nginx -p 8080:80 -v /Users/jubileu/html:/usr/share/nginx/html
docker run -d --name nginx -p 8080:80 --mount type=bind,source=/Users/jubileu/html,target=/usr/share/nginx/html
```

Podemos listar nossos volumes com o comando volume.
```shell
docker volume ls
```

Podemos criar volumes.
```shell
docker volume create nome_do_volume
```

Podemos também, obter informações acerca do nosso volume.
```shell
docker volume inspect nome_do_volume
```

Podemos mapear pelo nome do volume criado.
```shell
docker run -d --name nginx -p 8080:80 --mount type=volume,source=nome_do_volume,target=/app nginx
```

### Imagens

Podemos baixar uma imagem com o comando pull.
```shell
docker pull ubuntu
```

O comando abaixo lista todas as imagens que voce possui na máquina.
```shell
docker images
```

O comando a seguir, remove uma imagem baseado no seu nome.
```shell
docker rmi nome_da_imagem
```

### Dockerfile