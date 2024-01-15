# devops
## Coleção de Comandos Úteis em Linux

O projeto "Comandos Úteis em Linux" é uma iniciativa voltada para reunir e armazenar uma ampla variedade de comandos essenciais e práticos do sistema operacional Linux. Seu objetivo principal é fornecer uma coleção abrangente de comandos que possam auxiliar usuários, desde iniciantes até experientes, a executar tarefas com maior facilidade e eficiência no ambiente Linux
# Linux

## Ajuda em linux  
Basta digitar o "man nome_comando_ajuda" 
```sh
man dir
```
## Remover diretorio 
```sh
rm -rf nome_diretorio
```
# Para excluir arquivos em lote
```sh
rm *.pdf
```

# Listar os arquivos no diretorio root 
```sh
ls /
```

# Listar arquivos no diretorio pai 
```sh
ls ..
```

# Listar arquivos no diretorio home do usuário ( /home/user)
```sh
ls ~
```
# listar somente diretorios
```sh
ls -d */
```

# listar arquivos com subdiretorios
```sh
ls *
```

# Listar arquivos recursivamente 
```sh
ls -R
```

# Listar arquivos e seus tamanhos
```sh
ls -s
```
# listar arquivos em formato longo
```sh
ls -l
```

# Listar arquivos em formato longo com tamanhos de arquivo legiveis
```sh
ls -lh
```


# Listar arquivos incluindo arquivos ocultos 
```sh
ls -a
```

# Listar arquivos em formato longo incluindo arquivos ocultos 
```sh
ls -l -a    
ls -a -l 
ls -la
ls -al
```

# Listar arquivos e ordena-los por data e hora
```sh
ls -t
ls -tr
```

# Listar arquivos e gear o resultado em um arquivo

```sh
ls > output.txt
cat output.txt
```



## lista todas as portas em aberto linux
```sh
netstat -tunl
ss -tunelp
```
## Matar todas as instâncias do node em aberto
```sh
killall node
```
## Verificar a hora atual da VPS pelo terminal 
```sh
date
sudo dpkg-reconfigure tzdata
```
## Comando touch
```sh
touch oi
echo oi
```

## caminho usado para as versões padrão
```sh
which npm
```

## Listar as versos do node instalada 
```sh
nvm ls
```
# wsl list - Exemplo de como exportar dados ubuntu windows para backup
```sh
wsl --export ubuntu  c:\backup\ubuntu05-08-23.tar 
```
#
```sh

```


# Para mais consulte 
```sh
 man ls 
```
