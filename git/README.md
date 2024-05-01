# git

```sh
## Clonando um repositorio remoto e  Atualizando localmente
git clone git@github.com:otenielpinto/nome_rep.git   || note que https: somente qdo. copiar direto pelo navegador

## Obtem a branch que esta sendo trabalhada
git branch

## Obtem o status da branch
git status

## Adiciona todos os arquivos
git add .

## Atualizando repositorio remotamente
git commit -m "Correção  "
git remote -v
git remote add origin git@github.com:otenielpinto/nome_rep
git push -u origin master   | subir o arquivo para github
```

## Forma abreviada de atualizar repositorio

```sh
git add . && git commit -m "Historico da alteração"
```

## Atualizando arquivos localmente

```sh
git pull
```

## O forçar uma atualização em todos os arquivos de um diretório Git é o seguinte

```sh
git pull --force origin master
```

## Discard the local changes

```sh
git reset --hard
```

## Discard local changes for a specific file

```sh
git checkout filename
```

# Enviar arquivos

```sh
git add . && \
git commit -m "Histórico atualização ..."
```

# Comando para mudar o nome do diretorio no github

```sh
git remote set-url origin git@github.com:otenielpinto/embalamix-frontend.git
```

# Observacao importante para clonar repositorios com chave SSH

```sh
git clone git@github.com:otenielpinto/embalamix-frontend.git
```

observe que precisa ser SSH para que seja usadas as chaves ssh autorizadas na conta do github

## referencias

https://www.freecodecamp.org/portuguese/news/aprenda-o-basico-de-git-em-menos-de-10-minutos/
