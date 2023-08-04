# Aqui estão alguns exemplos de como usar os comandos do Docker:
```sh
docker run - Inicia um contêiner a partir de uma imagem. Por exemplo, para iniciar um contêiner com o servidor web Nginx, você pode usar o seguinte comando:
docker run -p 8080:80 nginx
Este comando irá iniciar um contêiner com o servidor web Nginx e mapear a porta 80 do contêiner para a porta 8080 do seu host.

docker pull - Baixa uma imagem do Docker Hub. Por exemplo, para baixar a imagem do servidor web Nginx do Docker Hub, você pode usar o seguinte comando:
docker pull nginx
docker push - Envia uma imagem para o Docker Hub. Por exemplo, para enviar a imagem do servidor web Nginx que você criou para o Docker Hub, você pode usar o seguinte comando:
docker push <seu-nome-de-usuário>/nginx
docker images - Lista as imagens do Docker. Por exemplo, para listar todas as imagens do Docker no seu sistema, você pode usar o seguinte comando:
docker images
docker ps - Lista os contêineres em execução. Por exemplo, para listar todos os contêineres em execução no seu sistema, você pode usar o seguinte comando:
docker ps
docker stop - Para um contêiner. Por exemplo, para parar um contêiner chamado my-container, você pode usar o seguinte comando:
docker stop my-container
docker start - Inicia um contêiner. Por exemplo, para iniciar um contêiner chamado my-container, você pode usar o seguinte comando:
docker start my-container
docker restart - Reinicia um contêiner. Por exemplo, para reiniciar um contêiner chamado my-container, você pode usar o seguinte comando:
docker restart my-container
docker rm - Remove um contêiner. Por exemplo, para remover um contêiner chamado my-container, você pode usar o seguinte comando:
docker rm my-container
docker rmi - Remove uma imagem. Por exemplo, para remover uma imagem chamada my-image, você pode usar o seguinte comando:
docker rmi my-image
docker build - Cria uma imagem a partir de um Dockerfile. Por exemplo, para criar uma imagem a partir de um Dockerfile chamado Dockerfile, você pode usar o seguinte comando:
docker build -t my-image .
docker tag - Adiciona uma tag a uma imagem. Por exemplo, para adicionar uma tag latest à imagem my-image, você pode usar o seguinte comando:
docker tag my-image:latest
docker inspect - Mostra informações sobre uma imagem ou contêiner. Por exemplo, para mostrar informações sobre um contêiner chamado my-container, você pode usar o seguinte comando:
docker inspect my-container
docker logs - Mostra os logs de um contêiner. Por exemplo, para mostrar os logs de um contêiner chamado my-container, você pode usar o seguinte comando:
docker logs my-container
docker exec - Executa um comando em um contêiner. Por exemplo, para executar o comando ls em um contêiner chamado my-container, você pode usar o seguinte comando:
docker exec my-container ls
docker network - Cria ou gerencia uma rede. Por exemplo, para criar uma rede chamada my-network, você pode usar o seguinte comando:
docker network create my-network
docker volume - Cria ou gerencia um volume. Por exemplo, para criar um volume chamado my-volume, você pode usar o seguinte comando:
docker volume create my-volume
Estes são apenas alguns dos muitos comandos do Docker. Para obter mais informações, consulte a documentação do Docker.
```
