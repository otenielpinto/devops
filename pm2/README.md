# PM2
```sh
 PM2 é um gerenciador de processos para aplicacoes em node.js em producao . 
```
 # Instalando o PM2 .
 ```sh
npm install pm2@latest -g
```
## 
```sh
pm2 --version 
```
## Iniciar processo com PM2 na porta 3000
```sh
npm run build
pm2 start npm --name vendiza-frontend -- run start -- -p 3000
```
## comando para chamar o PM2  Cluster
pm2 start server.js --watch --name nome_do_seu_projeto -i max

## comando para parar o PM2
pm2 stop nome_do_seu_projeto

## comando para mostrar log 
pm2 logs nome_do_seu_projeto 
pm2 logs nome_do_seu_projeto --lines 100 

## comando STOP 
pm2 stop nome_do_seu_projeto

## comando MONIT 
pm2 monit

### referencias 
https://danieldcs.com/como-usar-pm2-com-node-js-em-producao/
