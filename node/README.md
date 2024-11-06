# Principais Comandos em Node.js

## Instalação do Node.js

Para instalar o Node.js, você pode usar o gerenciador de pacotes do seu sistema operacional. Aqui estão alguns exemplos:

### No Ubuntu/Debian

```sh
sudo apt update
sudo apt install nodejs
sudo apt install npm
```

### No macOS

```sh
brew install node
```

### No Windows

Baixe o instalador do site oficial do Node.js: [Node.js](https://nodejs.org/)

## Inicialização de um Projeto Node.js

Para iniciar um novo projeto Node.js, você pode usar o comando `npm init`:

```sh
npm init
```

## Instalação de Pacotes

Para instalar pacotes, você pode usar o comando `npm install` ou `npm i`:

```sh
npm install <nome-do-pacote>
npm install express
```

## Execução de Scripts

Para executar um script Node.js, você pode usar o comando `node`:

```sh
node <nome-do-arquivo>.js
node app.js
```

## Uso do Nodemon

Nodemon é uma ferramenta que ajuda a desenvolver aplicativos baseados em Node.js reiniciando automaticamente o aplicativo quando arquivos são alterados no diretório:

```sh
npm install -g nodemon
nodemon <nome-do-arquivo>.js
```

## Gerenciamento de Pacotes

### Instalar um pacote globalmente

```sh
npm install -g <nome-do-pacote>
```

### Instalar um pacote como dependência de desenvolvimento

```sh
npm install --save-dev <nome-do-pacote>
```

### Atualizar pacotes

```sh
npm update
```

### Remover um pacote

```sh
npm uninstall <nome-do-pacote>
```

## Verificação de Versões

### Verificar a versão do Node.js

```sh
node -v
```

### Verificar a versão do npm

```sh
npm -v
```

## Inicialização de um Servidor Básico com Express

Express é um framework web para Node.js:

```js
const express = require("express");
const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`);
});
```

## Debugging

Para depurar um aplicativo Node.js, você pode usar o comando `node inspect`:

```sh
node inspect <nome-do-arquivo>.js
```

## Uso do PM2

PM2 é um gerenciador de processos para aplicativos Node.js:

```sh
npm install pm2 -g
pm2 start <nome-do-arquivo>.js
pm2 stop <nome-do-arquivo>.js
pm2 restart <nome-do-arquivo>.js
pm2 list
pm2 logs
```

## Testes

Para executar testes, você pode usar frameworks como Mocha ou Jest:

```sh
npm install --save-dev mocha
npm install --save-dev jest
```

### Executar testes com Mocha

```sh
npx mocha
```

### Executar testes com Jest

```sh
npx jest
```

## Linting

Para verificar a qualidade do código, você pode usar ferramentas como ESLint:

```sh
npm install eslint --save-dev
npx eslint <nome-do-arquivo>.js
```

## Variáveis de Ambiente

Para gerenciar variáveis de ambiente, você pode usar o pacote `dotenv`:

```sh
npm install dotenv
```

### Uso do dotenv

```js
require("dotenv").config();
console.log(process.env.MY_VARIABLE);
```

## Conclusão

Esses são alguns dos principais comandos e práticas ao trabalhar com Node.js. Para mais informações, consulte a [documentação oficial do Node.js](https://nodejs.org/en/docs/).
