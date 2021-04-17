# Docker Survival Guide
Useful Docker CLI commands 👷🏽

## Listar containers ativos
```
docker ps
```

## Listar todos os containers (incluindo inativos)
```
docker ps -a
```

## Criar um container hello-world
```
docker run hello-world
```

## Criar container ubuntu e iniciar o bash
- -it: são parâmetros que poderiam ser passados separadamente. (-i -t), sendo eles:
  - -i: modo interativo, ou seja, irá manter o stdin ativo em seu terminal. Basicamente mantém seu terminal local conectado com o bash do cotainer.
  - -t: significa TTY, o que permite executar comandos no terminal.
- ubuntu: nome da imagem que será baixada do Image Registry.
- bash: é o comando que será executado dentro do container, que neste caso serve para abrir iniciar o bash.
```
docker run -it ubuntu bash
```

## Criar um container e removê-lo assim que o processo for encerrado
- --rm: remove o container quando o processo é encerrado.
```
docker run -it --rm ubuntu bash
```

## Iniciar um container existente
```
docker start [container_id ou container_name]
```
## Parar um container
```
docker stop [container_id ou container_name]
```
## Redirecionar portas
Suponha que você queira subir um container com NGINX, que por default expõe a porta 80. Para acessar a porta 80 do NGINX, é necessário fazer um redirecionamento da porta do Docker Host (no caso pode ser sua máquina) para a porta do container.
- -p: indica que será realizado um redirecionamento de portas.
- 8080: indica qual porta do <b>Docker Host</b> será redirecionada.
- 80: indica para qual porta será feito o redirecionamento, quando for realizado um acesso na porta do Docker Host (8080).
- nginx: nome da imagem.
```
docker run -p 8080:80 nginx
```

## Iniciar containers em modo detached
- -d: executar o container em modo <i>Detached</i>, para não prender o terminal e manter a execução do container em background.
```
docker run -d -p 8080:80 nginx
```

## Remover container
```
docker rm [container_id ou container_name]
```

## Forçar remoção de container
```
docker rm [container_id ou container_name] -f
```

## Remover todos os containers
- -a lista todos os containers ativos e inativos
- -q retorna os IDs dos containers selecionados
- -f força a remoção caso algum dos containers estejam ativos
```
docker rm $(docker ps -a -q) -f
```

## Executar comando no container
No exemplo abaixo, acessamos o container chamado "nginx" e executamos o comando "ls".
```
docker exec nginx ls
```
Para executar o bash e manter o terminal "alive", é preciso adicionar as flags abaixo:
```
docker exec -it nginx bash
```

## Bind Mounts - Mapear um volume local para dentro do Docker
- Criará um container Nginx, mapeando a porta 8080 do Docker Host para a porta 80 do Nginx.
- Irá realizar um bind mount da pasta "[diretorio-local]/html" para o diretório /usr/share/nginx/html dentro do container Nginx. Todos os arquivos criados localmente serão refletidos para dentro do container.
- $(pwd) - é o path que o usuário está localizado.
```
docker run -d --name nginx -p 8080:80 --mount type=bind,source="$(pwd)"/html,target=/usr/share/nginx/html nginx
```
Também é possível utilizar uma sintaxe mais antiga, anterior ao --mount, que é a flag -v. Veja o exemplo abaixo:
```
docker run -d --rm -p 8080:80 -v $(pwd)/html:/usr/share/nginx/html nginx
```

## Volumes
### Listar volumes
```
docker volume ls
```

### Criar volume
```
docker volume create my-volume
```

### Inspecionar volume
Mountpoint: local onde ficam gravados os arquivos dentro de sua máquina (ex: <b>/var/lib/docker/volumes/my-volume/_data</b>).
```
docker volume inspect my-volume
```

### Mapear volume para dentro do container
Dessa maneira, será possível criar diversos containers (nginx1, nginx2, nginx3) compartilhando um único volume (source). Ou seja, arquivos criados em qualquer um dos containers estarão compartilhados entre eles. Os arquivos do volume serão criados dentro da pasta <b>app</b> no container.
```
docker run --name nginx -d --mount type=volume,source=my-volume,target=/app nginx
```
É possível criar um container utilizando a flag -v ao invés de --mount. Veja abaixo:
```
docker run --name nginx3 -d -v my-volume:/app nginx
```

### Limpar os volumes
É possível que sua máquina fique com o armazenamento cheio por conta de volumes do Docker. Para limpar estes arquivos, execute o comando abaixo:
```
docker volume prune
```

## Images
### Listar imagens instaladas localmente
```
docker images
```

### Baixar imagem do Docker Hub
Baixará a imagem mais recente do Ubuntu.
```
docker pull ubuntu
```

### Baixar imagem com uma tag específica
Baixará a imagem do PHP com a tag <b>rc-alpine</b>
```
docker pull php:rc-alpine
```

### Baixar imagem com uma tag específica
```
docker rmi php:rc-alpine
```

### Buildar imagem a partir do Dockerfile
- O parâmetro <b>eduardo3g</b> da flag -t (tag) é o usuário do Docker Hub (Registry). Esta é uma boa prática para criar imagems.
- O "." no final do comando é o diretório em que o Dockerfile está presente. Caso o comando seja executado no mesmo diretório, utiliza-se um ".".
```
docker build -t eduardo3/nginx-with-vim:latest .
```

Para acessar o container o qual o build foi feito a partir da imagem recém-criada, basta utilizar o comando:
```
docker run -it eduardo3/nginx-with-vim bash
```

## Network
O principal uso de network é permitir que um container se comunique com o outro. Imagine que você tenha dois containers, um executando Node.js e outro Laravel (PHP). Para que um comunique-se com o outro, é necessária uma network.

### Tipos de Network
- Bridge (tipo de rede default): utulizada para um container comunicar-se com o outro.
- * Host: mescla a rede do Dcker com a rede do Docker Host (sua máquina por exemplo). Imagine que você tenha uma aplicação PHP rodando no Docker Host através da porta 80. <br /> Caso você suba um container Docker na mesma rede (tipo "host") do Docker Host, será possível realizar a comunicação entre a aplicação PHP no Docker Host com a aplicação no container do Docker. Essa comunicação será feita diretamente, sem ser necessário realizar o mapeamento de portas (-p).
- Overlay: é utilizado para realizar comunicação de Docker Hosts diferentes.
- None: quando não há nenhuma rede e o container irá ser exetuado de forma isolada.
