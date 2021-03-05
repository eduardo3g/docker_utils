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
