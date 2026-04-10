# Bônus Parte 1: Integração Frontend 🖥️

O backend está On-Chain, como ligar o botão do site nisso? Você vai usar a biblioteca de Wallet Adapter. No hackathon, a forma mais rápida de iniciar é rodando `npx create-solana-dapp`, que já traz tudo (wallet adapter, provider) pré-configurado.

A carcaça básica num frontend React ficaria assim:

```typescript
import { useAnchorWallet } from "@solana/wallet-adapter-react";
import { Program, AnchorProvider } from "@coral-xyz/anchor";
import idl from "./idl/neobank_vault.json"; // O arquivo .json gerado no anchor build

// Conectar Padrão
const wallet = useAnchorWallet();
const provider = new AnchorProvider(connection, wallet, {});
const program = new Program(idl, provider);

// Função do Botão de DEPOSIT
await program.methods
    .deposit(new BN(amount))
    .accounts({ 
        user: wallet.publicKey, 
        mint, 
        userTokenAccount 
    })
    .rpc();

// Função de LER o Bando de Dados (Ler quanto tem depositado)
const [vaultPDA] = PublicKey.findProgramAddressSync(
    [Buffer.from("vault"), wallet.publicKey.toBuffer()],
    program.programId
);

const vaultState = await program.account.vaultState.fetch(vaultPDA);
console.log("Total Depositado:", vaultState.totalDeposited.toString());
```

---

# Bônus Parte 2: O Solana Vault Standard (SVS)

Agora que você construiu na unha para entender os conceitos, que tal ter isso padronizado em 5 minutos? Acabamos de rodar a roda, mas não precisamos sempre inventá-la.

Conheça o [Solana Vault Standard](https://github.com/solanabr/solana-vault-standard) da SuperteamBR. Um SDK e CLI Open-Source que padroniza todos esses vaults de forma modular e altamente segura.

```bash
# Instalar a CLI globalmente
npm install -g @stbr/solana-vault

# Iniciar uma configuração do vault (Versão padrão svs-1)
solana-vault config init
solana-vault config add-vault my-vault <ADDRESS> --variant svs-1

# Interagir diretamente do seu terminal (Depositar 1M de tokens)
solana-vault deposit my-vault -a 1000000

# Sacar e ver saldos
solana-vault withdraw my-vault -a 500000
solana-vault balance my-vault
solana-vault info my-vault
```

Com o SVS, você tem 6 variantes robustas prontas (public, private, streaming yield) e módulos opcionais (fees, timelocks, whitelist). Tudo modular, tudo open-source. Mais uma arma poderosa pro Hackathon.
