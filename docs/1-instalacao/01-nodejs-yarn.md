# Passo 1: Node.js e Yarn

O Anchor (framework para construção de programas na Solana) depende do Node.js e do gerenciador de pacotes Yarn, principalmente para criar e rodar os testes TypeScript da sua aplicação.

Se você ainda não possui o Node.js instalado, a forma mais segura é via NVM (Node Version Manager).

### Comandos de Instalação (Linux, Mac, WSL)

```bash
# 1. Instala o gerenciador de versões do Node (NVM)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash

# 2. Recarrega as configurações do seu terminal
source ~/.bashrc  # Utilize ~/.zshrc se estiver no Mac ou usando Zsh

# 3. Instala a versão mais estável do Node.js
nvm install --lts

# 4. Ativa o Corepack integrado para habilitar o Yarn
corepack enable
```

### Verificando a instalação

Certifique-se de que os comandos retornam as versões instaladas:
```bash
node --version
yarn --version
```
