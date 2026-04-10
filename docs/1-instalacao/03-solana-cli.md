# Passo 3: Solana CLI ☀️

A Solana CLI (Command Line Interface) fornece o kit de ferramentas essencial para se comunicar com a blockchain da Solana. É através dela que você vai gerar sua carteira de testes, criar keys, receber airdrops e fazer o deploy do seu programa para a rede (seja Devnet, Testnet ou Mainnet).

### Comandos de Instalação (Linux, Mac, WSL)

```bash
# 1. Instala a versão estável mais recente da ferramenta de linha de comando
sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"

# 2. Adiciona o binário da Solana ao seu PATH (caminho global do terminal)
# Se estiver usando Bash:
echo 'export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Se estiver usando Zsh (Mac/Padrão novo do Linux):
# echo 'export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.zshrc
# source ~/.zshrc
```

### Verificando a instalação

Verifique se a interface está pronta para uso:
```bash
solana --version
```
