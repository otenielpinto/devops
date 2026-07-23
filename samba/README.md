# Como usar Samba no Linux para compartilhar arquivos com Windows e Linux

O Samba implementa o protocolo SMB/CIFS, nativo do Windows, permitindo
compartilhamento de arquivos entre Linux e Windows na mesma rede.

## 1. Instalar o Samba

**Debian/Ubuntu:**

```bash
sudo apt update
sudo apt install samba
```

**Fedora/RHEL:**

```bash
sudo dnf install samba
```

**Arch:**

```bash
sudo pacman -S samba
```

## 2. Criar a pasta a ser compartilhada

```bash
sudo mkdir -p /srv/samba/compartilhado
sudo chmod -R 0775 /srv/samba/compartilhado
sudo chown -R nobody:nogroup /srv/samba/compartilhado
```

> Em algumas distros o grupo é `nogroup`, em outras `nobody`. Ajuste conforme
> necessário.

## 3. Configurar o Samba

Edite o arquivo de configuração:

```bash
sudo nano /etc/samba/smb.conf
```

Adicione no final do arquivo:

```ini
[Compartilhado]
   path = /srv/samba/compartilhado
   browsable = yes
   writable = yes
   guest ok = no
   valid users = seu_usuario
   create mask = 0664
   directory mask = 0775
```

**Explicação rápida:**

- `path` → caminho da pasta compartilhada
- `browseable = yes` → aparece na rede
- `read only = no` → permite escrita
- `guest ok = yes` → permite acesso sem senha (use `no` para exigir login)

### Alternativa com autenticação (mais segura)

```ini
[Compartilhado]
   path = /srv/samba/compartilhado
   browseable = yes
   read only = no
   guest ok = no
   valid users = seu_usuario
```

## 4. Criar usuário Samba (se usar autenticação)

```bash
sudo smbpasswd -a seu_usuario
```

## 5. Reiniciar o serviço

```bash
sudo systemctl restart smbd nmbd
sudo systemctl enable smbd nmbd
```

## 6. Liberar no firewall (se ativado)

```bash
sudo ufw allow samba
```

## 7. Acessando do Windows

No Windows, abra o Explorador de Arquivos e digite na barra de endereço:

```text
\\IP_DO_LINUX\Compartilhado
```

Exemplo: `\\192.168.1.100\Compartilhado`

Vai pedir usuário e senha — use as credenciais criadas com `smbpasswd`.

## 8. Acessando de outro Linux

```bash
smbclient //IP_DO_LINUX/Compartilhado -U seu_usuario
```

Ou monte via `mount.cifs`:

```bash
sudo mount -t cifs //IP_DO_LINUX/Compartilhado /mnt/ponto_montagem -o username=seu_usuario
```

## Dicas extras

- Descubra o IP do Linux com `ip a` ou `hostname -I`.
- Teste a configuração antes de reiniciar com `testparm`.
- Se o Windows não encontrar o compartilhamento, verifique se ambos estão na mesma rede/sub-rede e se o firewall não está bloqueando.

Quer que eu detalhe algum cenário específico (ex: compartilhamento público sem senha, compartilhar impressora, ou integrar com Active Directory)?
