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
