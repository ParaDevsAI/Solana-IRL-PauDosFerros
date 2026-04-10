# Passo 2: Instalar Rust 🦀

Rust é a linguagem de programação nativa para o desenvolvimento de Smart Contracts (chamados de "Programs") na rede Solana. É uma linguagem de baixo nível focada em performance e segurança de memória.

### Comandos de Instalação (Linux, Mac, WSL)

O instalador oficial da linguagem chama-se `rustup`.

```bash
# 1. Baixa e instala a cadeia de ferramentas do Rust automaticamente
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

# 2. Atualiza as variáveis de ambiente do seu terminal atual
. "$HOME/.cargo/env"
```

### Verificando a instalação

Garanta que o compilador do Rust foi configurado corretamente:
```bash
rustc --version
cargo --version
```
*(O `cargo` é o gerenciador de pacotes e build system do Rust).*
