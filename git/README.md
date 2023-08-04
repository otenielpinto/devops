# git 
```sh
## Clonando um repositorio remoto e  Atualizando localmente 
git clone git@github.com:otenielpinto/vendizap.git   || note que https: somente qdo. copiar direto pelo navegador
git status 
git add .  ( adiciona todos os arquivos )

## Atualizando repositorio remotamente 
git commit -m "Correção  "
git remote -v 
git remote add origin git@github.com:otenielpinto/vendizap
git push -u origin master   | subir o arquivo para github 

## Forma abreviada de atualizar repositorio
git add . && git commit -m "Commit"

## Atualizando arquivos localmente
git pull  (overwrite local files )

```
# Enviar arquivos 
```sh
git add . && \ 
git commit -m "Histórico atualização ..."
```

## referencias 
https://www.freecodecamp.org/portuguese/news/aprenda-o-basico-de-git-em-menos-de-10-minutos/