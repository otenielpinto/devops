# Windows Server 2019 - Verificação de Licença

Existem várias formas de verificar a data de expiração/status de licença do Windows Server 2019. Aqui estão os principais métodos:

## 1. Via Prompt de Comando (CMD)

### Verificar status geral de ativação

```bash
slmgr /xpr
```

Mostra se a licença é permanente ou, se for uma licença por assinatura/trial, mostra a data exata de expiração.

### Ver informações detalhadas da licença

```bash
slmgr /dlv
```

Exibe detalhes completos, incluindo tipo de licença, canal (Retail, Volume/KMS, OEM) e data de expiração (se aplicável).

## 2. Interpretando os resultados

- **Licença Retail/OEM (permanente)**: aparece como "The machine is permanently activated" — não há data de vencimento, pois é uma licença perpétua.
- **Licença via KMS (Volume Licensing)**: mostra uma data de expiração da ativação (normalmente renovada automaticamente a cada 180 dias enquanto o servidor KMS estiver acessível). O comando `slmgr /dlv` mostra "License expiration" com a contagem de dias restantes.
- **Licença Trial/Avaliação**: mostra claramente quantos dias restam antes de expirar.

## 3. Verificar via PowerShell

```powershell
Get-WmiObject SoftwareLicensingProduct -Filter "Name like 'Windows%'" | Where-Object { $_.PartialProductKey } | Select-Object Description, LicenseStatus, GracePeriodRemaining
```

## 4. Verificar contrato de licenciamento (Volume Licensing / CSP)

Se o servidor foi adquirido via **Volume Licensing Center (VLSC)**, **CSP** ou contrato com Microsoft/parceiro, a "validade" pode não estar no próprio SO, mas sim no contrato comercial (ex: licenças de assinatura como parte do Microsoft 365, Azure Hybrid Benefit, ou licenciamento por assinatura anual). Nesse caso, verifique:

- **Volume Licensing Service Center (VLSC)** — portal da Microsoft
- Com o revendedor/parceiro que vendeu a licença

## 5. Comando slmgr /rearm

O comando `slmgr /rearm` **reinicia o contador do período de carência (grace period)** de ativação do Windows.

### Executar o comando

```bash
slmgr /rearm
```

### O que ele faz especificamente:

1. **Reseta o timer de ativação**: Restaura a contagem do período de avaliação/carência para o valor inicial (geralmente 180 dias para trials, ou o período padrão de ativação).
2. **Limpa dados de ativação**: Remove o status de ativação atual, fazendo o sistema "esquecer" que estava perto de expirar.
3. **Reinicialização necessária**: Após executar o comando, é preciso **reiniciar o servidor** para que as mudanças tenham efeito.

### Limitações importantes

⚠️ **Número limitado de usos**: O rearm só pode ser executado um número limitado de vezes:

- No Windows Server, geralmente **3 a 5 vezes** (varia por versão)
- Depois de esgotar o limite, o comando não funciona mais

⚠️ **Não é uma solução permanente**: É útil para:

- Testar prazos de avaliação (trial) em ambientes de laboratório
- Adiar temporariamente uma expiração de KMS enquanto se resolve um problema de ativação
- Cenários de troubleshooting (sysprep, clonagem de VMs, etc.)

⚠️ **Não ativa a licença permanentemente**: Ele só "compra tempo" — não substitui uma ativação válida. Depois do rearm, ainda é necessário ativar o Windows com uma chave válida ou servidor KMS.

### Verificar quantos rearms restam

```bash
slmgr /dlv
```

Procure pelo campo **"Remaining Windows rearm count"** na saída.
