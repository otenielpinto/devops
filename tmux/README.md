# tmux

O tmux é um terminal multiplexer, ou seja, uma ferramenta que permite criar sessões de terminal que continuam rodando no servidor mesmo depois que você desconecta da SSH.

## Como funciona

O tmux roda no servidor e cria sessões virtuais de terminal. Você pode:

- abrir várias janelas e painéis em uma única conexão SSH;
- desconectar e deixar os programas rodando em segundo plano;
- reconectar depois e voltar exatamente para onde parou.

## Comandos básicos

```bash
# Instalar (se ainda não estiver instalado)
sudo apt install tmux    # Debian/Ubuntu
sudo dnf install tmux    # Fedora/RHEL

# Criar uma nova sessão com nome
tmux new -s claude

# Dentro do tmux, rode os comandos normalmente
claude

# Desanexar da sessão (deixar rodando em background)
# Pressione: Ctrl+b, depois solte e pressione d

# Listar sessões ativas
tmux ls

# Reanexar a uma sessão existente
tmux attach -t claude

# Encerrar uma sessão
tmux kill-session -t claude
```

## Atalhos úteis dentro do tmux

Todos os atalhos começam com Ctrl+b (o prefixo), depois solte e pressione a tecla desejada:

- Ctrl+b, d: desanexar a sessão
- Ctrl+b, c: criar nova janela
- Ctrl+b, n: ir para a próxima janela
- Ctrl+b, p: voltar para a janela anterior
- Ctrl+b, %: dividir painel verticalmente
- Ctrl+b, ": dividir painel horizontalmente
- Ctrl+b, x: fechar o painel atual

## Por que é útil no homelab

No seu cenário, o tmux resolve exatamente o problema de interrupções de conexão: você abre o tmux, roda o Claude Code dentro dele e, se a SSH cair, o processo continua rodando no servidor. Quando você reconectar, basta fazer:

```bash
ssh usuario@servidor
tmux attach -t claude
```

Assim, você consegue voltar exatamente para a tela onde estava, sem perder a execução em andamento.
