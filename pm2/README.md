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
pm2 start npm --name server_backend -- run start -- -p 3000
pm2 save --force
```

## comando para chamar o PM2 Cluster

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

## comando DELETE

pm2 delete numero_do_server

## mostra informações sobre a aplicação

pm2 show NomeDoApp

## inicia um ou mais processos via arquivo JSON.

pm2 start process_prod.json

## libera todos os dados de log, disponibilizando espaço em disco

pm2 flush

## recarrega a configuração do aplicativo, como as variáveis de ambiente.

pm2 reload all

## reinicia todos os aplicativos em execução.

pm2 restart all

## mata todos os aplicativos em execução.

pm2 kill all

## Diretorio logs do PM2

```sh
Exemplo :
 /home/jelastic/.pm2/logs
```

### referencias

https://danieldcs.com/como-usar-pm2-com-node-js-em-producao/
