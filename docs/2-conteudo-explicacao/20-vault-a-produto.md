# De Vault a Produto 📈

A trajetória na Web3 (e nos projetos matadores de Hackathons) é estritamente **incremental**. Ninguém escreve o Serum ou a Jito em uma tarde. Tudo começa daquilo que acabamos de fazer.

### Nível 1: Vault (Onde estamos hoje)
O que você sabe fazer: `Deposit` e `Withdraw` usando **PDAs**, efetuando **CPIs** para o Token Program assinar autonomamente.

### Nível 2: Lending Protocol
Evolução direta do Vault. Você pega o cofre atual, mas agora adiciona o recebimento de Colateral, bloqueios (borrow) condicionados aos preços do mundo real via **Oracles** (como Pyth ou Switchboard), e adiciona lógica matemática para juros e liquidação.

### Nível 3: Yield Aggregator
Você cria um programa mestre que engloba dezenas de instâncias de Nível 2 (vários lending protocols). Seu Vault agora tem **Auto-Compound** e **Estratégia Automática** roteando liquidez pra onde paga mais.

### Nível 4: Produto Completo
Sua engenharia no Rust atingiu o auge. Agora você envelopa tudo com um Frontend estonteante, indexadores em background filtrando eventos na rede, rodando em nós RPC dedicados com código auditado antes de empurrar o projeto para a Mainnet.

> Cada grande protocolo de DeFi é apenas um agrupamento lógico de pequenos Vaults PDAs.
