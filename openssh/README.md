# Configuração de SFTP Seguro no Windows com OpenSSH

Para configurar um servidor SFTP seguro e gratuito no Windows, a melhor opção atual é utilizar o OpenSSH Server nativo (da própria Microsoft) ou o Bitvise SSH Server (gratuito para uso pessoal).

Abaixo está o passo a passo para ativar e configurar o OpenSSH Nativo, que dispensa a instalação de programas de terceiros.

---

## 🛠️ Como Configurar o SFTP Nativo no Windows

### 1. Instalar o Servidor OpenSSH

1. Abra as Configurações do Windows (`Win + I`).
2. Vá em Aplicativos > Recursos Opcionais (ou _Recursos Adicionais_).
3. Clique em Adicionar um recurso.
4. Digite `OpenSSH` na barra de pesquisa.
5. Marque Servidor OpenSSH e clique em Instalar. [3, 4]

### 2. Iniciar o Serviço SFTP

1. Abra o menu iniciar, digite `Serviços` e abra o aplicativo.
2. Procure por OpenSSH SSH Server na lista.
3. Clique com o botão direito nele e selecione Propriedades.
4. Mude o _Tipo de inicialização_ para Automático.
5. Clique no botão Iniciar e depois em OK. [5, 6]

### 3. Liberar o SFTP no Firewall

Se o Windows Firewall não liberar o acesso automaticamente, execute o comando abaixo no PowerShell (como Administrador):

```powershell
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -LocalPort 22 -Action Allow
```

### 4. Como se Conectar ao Servidor

Para enviar ou receber arquivos de outra máquina para este servidor:

- **IP do Servidor**: O endereço IP local (ex: `192.168.1.50`).
- **Porta**: `22` (padrão do SSH/SFTP).
- **Usuário e Senha**: As mesmas credenciais da conta de usuário do Windows (se usar conta Microsoft, use o e-mail e a senha/PIN).
- **Cliente recomendado**: Use o [FileZilla](https://filezilla-project.org/) ou [WinSCP](https://winscp.net/) na máquina cliente para gerenciar os arquivos visualmente. [7]

---

## 📂 Como Restringir o Usuário a uma Pasta Específica (Opcional)

Por padrão, o usuário terá acesso a quase todo o sistema. Se quiser que ele veja apenas uma pasta isolada, siga estes passos:

1. Abra o arquivo `C:\ProgramData\ssh\sshd_config` com o Bloco de Notas (como Administrador).
2. Vá até o final do arquivo e adicione as seguintes linhas:

```conf
Match User nome_do_usuario
    ChrootDirectory C:\Caminho\Da\Pasta
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no
```

3. Reinicie o serviço _OpenSSH SSH Server_ no gerenciador de Serviços.

---

## 👤 Como Criar e Configurar o Usuário "oporto" com Acesso SFTP

Para adicionar o usuário oporto e garantir que ele tenha acesso ao SFTP, você precisa criar a conta no Windows e configurar as permissões corretas.

Siga o passo a passo abaixo utilizando o Prompt de Comando (CMD) como Administrador.

### 1. Criar o usuário "oporto" no Windows

1. Clique no menu Iniciar, digite CMD, clique com o botão direito nele e selecione Executar como Administrador.
2. Digite o comando abaixo para criar o usuário (substitua `SuaSenhaAqui` pela senha desejada):

```cmd
net user oporto SuaSenhaAqui /add
```

3. _(Opcional)_ Para evitar que a senha expire e mude sozinha, execute:

```cmd
wmic useraccount where name='oporto' set PasswordExpires=FALSE
```

### 2. Configurar a Pasta Exclusiva do SFTP

Para garantir a segurança, o Windows exige que a pasta raiz do SFTP (Chroot) pertença estritamente ao grupo de Administradores e que o usuário comum não tenha permissão de escrita direta nela. O padrão recomendado é criar uma pasta principal (raiz) e, dentro dela, uma subpasta onde o usuário oporto poderá enviar os arquivos.

1. No CMD, crie a estrutura de pastas (exemplo no disco C:):

```cmd
mkdir C:\sftp_oporto\arquivos
```

2. Dê permissão de escrita para o usuário oporto apenas na subpasta `arquivos`:

```cmd
icacls C:\sftp_oporto\arquivos /grant oporto:(OI)(CI)M
```

### 3. Isolar o usuário "oporto" no OpenSSH

Agora, vamos configurar o OpenSSH para prender o usuário nessa pasta, impedindo-o de navegar pelo restante do seu HD.

1. Abra o arquivo `C:\ProgramData\ssh\sshd_config` no Bloco de Notas (como Administrador).
2. Vá até o final do arquivo e cole o seguinte bloco exatamente como está:

```conf
Match User oporto
    ChrootDirectory C:\sftp_oporto
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no
```

3. Salve e feche o arquivo. [1, 2]

### 4. Reiniciar o Servidor SFTP

Para aplicar as novas configurações do usuário e do isolamento de pasta, reinicie o serviço executando o comando abaixo no CMD:

```bash
net stop sshd && net start sshd
```

### 📡 Como Testar a Conexão

No programa cliente (como FileZilla ou WinSCP), conecte-se usando:

- **Host**: IP do seu computador
- **Usuário**: `oporto`
- **Senha**: A senha que você definiu no passo 1
- **Porta**: `22`

Ao conectar, o usuário verá apenas a pasta `arquivos` e poderá arrastar os documentos para dentro dela.

---

## 🔑 Como Alterar a Senha do Usuário "oporto"

Para alterar a senha do usuário oporto, você pode fazer isso rapidamente pelo Prompt de Comando (CMD) ou pelas Configurações visuais do Windows. Escolha o método mais fácil para você abaixo:

### Método 1: Pelo Prompt de Comando (Mais Rápido)

1. Clique no menu Iniciar, digite CMD.
2. Clique com o botão direito sobre o "Prompt de Comando" e selecione **Executar como Administrador**.
3. Digite o comando abaixo, substituindo `NovaSenhaAqui` pela nova senha que você deseja definir:

```cmd
net user oporto NovaSenhaAqui
```

4. Pressione Enter. A mensagem _"Comando concluído com êxito"_ confirmará a alteração. [8, 9, 10]

### Método 2: Pela Interface Gráfica (Gerenciamento do Computador)

Se preferir não usar linhas de comando, siga estes passos:

1. Pressione as teclas `Win + X` no teclado e selecione **Gerenciamento do Computador** (ou digite `compmgmt.msc` no menu Iniciar).
2. No menu esquerdo, vá em **Ferramentas do Sistema > Usuários e Grupos Locais > Usuários**.
3. Na lista do centro, clique com o botão direito em cima do usuário **oporto** e selecione **Definir Senha...**.
4. Clique em **Prosseguir** no aviso que aparecerá.
5. Digite a nova senha, confirme-a e clique em **OK**. [11, 12, 13]

### ⚠️ Importante após a alteração

- **Não é necessário reiniciar o serviço SSH/SFTP**: O OpenSSH valida a senha diretamente com o Windows a cada nova tentativa de login. A mudança é instantânea.
- **Atualize a senha no cliente**: Lembre-se de atualizar a senha salva no seu gerenciador de arquivos (FileZilla, WinSCP, etc.) antes de tentar se conectar novamente.

---

## 🚨 Resolvendo Erro "Authentication failed" (Falha de Autenticação)

O erro "Authentication failed" acompanhado de "Requesting keyboard-interactive authentication" significa que o servidor OpenSSH recusou as credenciais do usuário oporto. Isso geralmente acontece por três motivos: a senha foi digitada errada, o Windows está bloqueando o login de usuários sem privilégios via SSH, ou o OpenSSH não está configurado para aceitar senhas comuns.

Siga estes passos na máquina onde o servidor está instalado para corrigir o problema:

### Passo 1: Permitir autenticação por senha no OpenSSH

Por padrão, algumas instalações do OpenSSH vêm com a autenticação por senha desativada, exigindo chaves. Vamos garantir que ela esteja ativa.

1. Abra o arquivo `C:\ProgramData\ssh\sshd_config` com o Bloco de Notas (como Administrador).
2. Procure pela linha `PasswordAuthentication`.
3. Certifique-se de que ela está configurada como `yes` e que não tem um símbolo de `#` no início. Deve ficar assim:

```conf
PasswordAuthentication yes
```

4. Procure também por `PubkeyAuthentication` e mude para `no` se quiser forçar apenas senha (ou deixe `yes` se usar chaves).
5. Salve o arquivo e reinicie o SSH no CMD (como Administrador):

```bash
net stop sshd && net start sshd
```

### Passo 2: Verificar o formato do Usuário no cliente (FileZilla / WinSCP)

Se a sua máquina está conectada a uma conta Microsoft ou a um Domínio, o OpenSSH pode exigir o escopo do usuário.

- No campo Usuário do FileZilla/WinSCP, tente digitar: `.\oporto` (o ponto e a barra invertida forçam o Windows a buscar o usuário localmente).
- Certifique-se de que a Porta está definida como `22` e o protocolo como `SFTP` (e não FTP).

### Passo 3: Adicionar o usuário ao grupo de Usuários de Área de Trabalho Remota (se necessário)

O OpenSSH no Windows às vezes restringe o login se o usuário não tiver permissão de logon remoto nas diretivas do sistema.

1. Abra o CMD como Administrador.
2. Execute o comando abaixo para dar a permissão necessária ao usuário:

```cmd
net localgroup "Remote Desktop Users" oporto /add
```

_(Nota: Se o seu Windows estiver em português, use `net localgroup "Usuários do Emulador de Terminal" oporto /add` ou `"Usuários da Área de Trabalho Remota"`)_

### Passo 4: Testar a senha localmente

Para garantir que a senha não foi digitada com algum caractere errado (como o Caps Lock ativado) durante a alteração:

1. Bloqueie o seu Windows (`Win + L`) ou faça Logoff.
2. Tente fazer login na tela inicial do Windows selecionando o usuário oporto e digitando a nova senha. Se o Windows recusar, refaça o procedimento de alteração de senha (usando o método do CMD ensinado anteriormente) para garantir que a senha é exatamente o que você deseja.

---

## 🔐 Resolvendo Erro "Too many authentication failures" (Acesso via DDNS/Internet)

O log mostra que o cliente agora consegue alcançar o seu servidor através do endereço externo (ex: `empresarial.ddns.com.br`), mas o OpenSSH do Windows continua rejeitando a senha do usuário `oporto`. Isso acontece porque o OpenSSH é extremamente rígido com as permissões de segurança quando acesso vem de fora (via internet/DDNS).

Siga o passo a passo abaixo na máquina onde o servidor está instalado para resolver isso de vez:

### Passo 1: Permitir explicitamente o login do usuário no OpenSSH

Por padrão, o OpenSSH pode bloquear novos usuários locais se eles não estiverem explicitamente liberados na lista de acessos do arquivo de configuração.

1. Abra o arquivo `C:\ProgramData\ssh\sshd_config` com o Bloco de Notas (como Administrador).
2. Vá até o final do arquivo.
3. Logo antes da linha `Match User oporto` que criamos antes, adicione a seguinte diretiva:

```conf
AllowUsers oporto
```

4. Certifique-se de que o bloco completo do final do arquivo ficou exatamente assim:

```conf
AllowUsers oporto

Match User oporto
    ChrootDirectory C:\sftp_oporto
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no
```

5. Salve e feche o arquivo.

### Passo 2: Corrigir a permissão da pasta raiz (Muito Comum)

O OpenSSH possui um recurso de segurança estrito: se você usa a diretiva `ChrootDirectory` (para prender o usuário na pasta), a pasta pai (`C:\sftp_oporto`) **NÃO pode ter permissão de escrita** para o usuário comum, e o dono dela deve ser o sistema/administrador. Se o usuário tiver permissão nela, o OpenSSH recusa o login por segurança.

Vamos resetar as permissões dessa pasta para o padrão correto usando o CMD:

1. Abra o Prompt de Comando (CMD) como Administrador.
2. Cole os comandos abaixo, um por um, pressionando Enter após cada um:

```cmd
icacls C:\sftp_oporto /reset
icacls C:\sftp_oporto /grant:r Administrators:(OI)(CI)F /grant:r SYSTEM:(OI)(CI)F
icacls C:\sftp_oporto /inheritance:r
```

3. Agora, garanta que o usuário `oporto` tenha acesso total apenas na subpasta de arquivos (onde ele vai jogar os dados):

```cmd
icacls C:\sftp_oporto\arquivos /grant oporto:(OI)(CI)M
```

### Passo 3: Reiniciar o serviço para aplicar as mudanças

No mesmo CMD como Administrador, reinicie o servidor OpenSSH:

```bash
net stop sshd && net start sshd
```

### Passo 4: Como preencher os campos no FileZilla

Ao tentar conectar novamente no FileZilla, configure os campos exatamente assim para evitar conflitos de escopo de nome do Windows:

- **Host**: `sftp://empresarial.ddns.com.br` _(adicionar o `sftp://` na frente ajuda o FileZilla a forçar o protocolo correto)_
- **Nome de usuário**: `.\oporto` _(o ponto e a barra invertida forçam o Windows a autenticar como usuário local, ignorando contas de e-mail ou domínios)_
- **Senha**: A nova senha que você definiu anteriormente.
- **Porta**: `22`

### 🔍 Teste de Isolamento de Problema

Se após seguir esses passos o erro persistir, faça um teste rápido para isolar o problema:

1. No FileZilla, mude o **Nome de usuário** para o seu usuário principal de Administrador do Windows (com a sua senha de Admin) e tente conectar.
2. Se o usuário Administrador conseguir conectar com sucesso e o `oporto` não, saberemos que o problema é estritamente uma permissão pendente na conta do `oporto`.

---

## ❓ Próximos Passos e Suporte

Para avançarmos na configuração, considere os seguintes pontos:

- Os usuários que vão se conectar são contas locais do Windows ou você precisa criar um usuário exclusivo para isso?
- O teste de conexão funcionou com o usuário oporto?
- Se recebeu erro de autenticação:
  - Qual programa cliente você está usando (FileZilla, WinSCP, etc.)?
  - O usuário oporto consegue fazer login físico na máquina se você tentar trocar de usuário no Windows?
- Esse acesso será feito apenas por dispositivos na mesma rede Wi-Fi/cabeada ou você precisará que alguém acesse pela internet (fora da sua casa/empresa)?
- Você precisa de ajuda para descobrir o IP da máquina na rede ou para abrir as portas do roteador para acesso externo?

---

## 📚 Referências

[1] [https://www.wpbeginner.com](https://www.wpbeginner.com/pt/wp-tutorials/how-to-move-wordpress-to-a-new-host-or-server-with-no-downtime/)  
[2] [https://www.digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04-pt)  
[3] [https://www.manageengine.com](https://www.manageengine.com/br/ad-manager/remote-server-administration-tools.html)  
[4] [https://netwrix.com](https://netwrix.com/pt/resources/blog/openssh-to-move-files/)  
[5] [https://learn.microsoft.com](https://learn.microsoft.com/pt-br/fslogix/how-to-configure-storage-permissions)  
[6] [https://experienceleague.adobe.com](https://experienceleague.adobe.com/pt-br/docs/experience-cloud-kcs/kbarticles/ka-14765)  
[7] [https://www.hostgator.com.br](https://www.hostgator.com.br/blog/o-que-e-protocolo-sftp/)  
[8] [https://kb.adentrocloud.com.br](https://kb.adentrocloud.com.br/knowledgebase/alterar-senha-de-usuario-com-acesso-ao-servidor/)  
[9] [https://learn.microsoft.com](https://learn.microsoft.com/pt-br/answers/questions/4157655/como-alterar-a-pol-tica-de-senha-no-windows-11-hom)  
[10] [https://www2.strategy.com](https://www2.strategy.com/producthelp/current/Cloud/pt-br/Content/change_default_password.htm)  
[11] [https://learn.microsoft.com](https://learn.microsoft.com/pt-br/answers/questions/2775573/como-alterar-senha-de-de-uma-conta-local)  
[12] [https://docs.oracle.com](https://docs.oracle.com/pt-br/iaas/Content/Compute/Tasks/howto-reset-windows-opc-password.htm)  
[13] [https://help.sap.com](https://help.sap.com/docs/buying-invoicing/managing-your-user-information/how-to-change-your-password?locale=pt-BR)
