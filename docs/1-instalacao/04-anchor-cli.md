# Passo 4: Instalar Anchor ⚓

O Anchor é um framework para o desenvolvimento de programas Solana. Ele abstrai grande parte do "código chato/repetitivo" (boilerplate) necessário no desenvolvimento nativo em Rust puro, tornando a jornada do Desenvolvedor muito mais parecida com a de criar um backend convencional, focado em regras de negócio.

A recomendação oficial é utilizar o AVM (Anchor Version Manager), assim você pode transitar em diferentes versões do Anchor tranquilamente.

### Comandos de Instalação (Linux, Mac, WSL)

> **⏳ IMPORTANTE:** O primeiro passo (`cargo install`) vai baixar, focar e compilar código Rust pesado na sua máquina local. O processo de compilação pode demorar **vários minutos** dependendo da potência do seu computador. Tenha paciência!

```bash
# 1. Usa o cargo (gerenciador do Rust) para instalar o controlador de versões do Anchor
cargo install --git https://github.com/solana-foundation/anchor avm --force

# 2. Pede para o AVM baixar a última versão do Anchor (isso é mais rápido)
avm install latest

# 3. Define a versão 'latest' como o padrão da sua máquina
avm use latest
```

### Problema de Linker (Opcional - Apenas algumas máquinas Linux)
Se na hora de instalar o AVM você esbarrar em um erro do tipo: `error: could not exec the linker cc...`, você precisa instalar os pacotes globais básicos C++. No Ubuntu/Debian, rode:
`sudo apt-get update && sudo apt-get install -y pkg-config build-essential libudev-dev`

### Verificando a instalação

Confirme se o Anchor Framework está pronto para ser utilizado:
```bash
anchor --version
```
