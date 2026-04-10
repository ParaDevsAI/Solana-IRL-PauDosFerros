# Build, Testes e Deploy 🚀

Depois de escrever nosso Smart Contract (Program), precisamos compilar o código Rust, testar a lógica com TypeScript e, finalmente, mandar para a rede.

## 1. Build (Compilação)
O primeiro passo é garantir que nosso código Rust não tem erros crasso de sintaxe e compilar os binários.
```bash
anchor build
```
*Se rodar sem erros vermelhinhos, sucesso! O compilador do Rust é seu maior amigo: se compila, tem 90% de chance de rodar perfeitamente on-chain.*

---

## 2. Testes TypeScript (`tests/neobank_vault.ts`)

Nunca mandamos um código para produção sem antes simular a integração no nosso próprio computador usando uma Localnet (Surfpool/Test Validator).

```typescript
import * as anchor from "@coral-xyz/anchor";
import { Program } from "@coral-xyz/anchor";

describe("neobank_vault", () => {
  // Configura a conexão e a carteira local (provider)
  const provider = anchor.AnchorProvider.env();
  anchor.setProvider(provider);
  const program = anchor.workspace.NeobankVault as Program<any>;

  it("Deposita tokens no vault", async () => {
    // Simulando o FrontEnd chamando o Smart Contract
    const tx = await program.methods
      .deposit(new anchor.BN(1_000_000))
      .accounts({
        user: provider.wallet.publicKey,
        mint: mintAddress, // (Endereço do token previamente criado no teste)
        userTokenAccount: userATA,
      })
      .rpc(); // .rpc() envia e assina a transação pra rede
      
    console.log("Deposit tx:", tx);
    // TODO: Adicionar assertions para verificar o saldo do vault usando 'program.account.vaultState.fetch'
  });

  it("Saca tokens do vault", async () => {
    const tx = await program.methods
      .withdraw(new anchor.BN(500_000))
      .accounts({
        user: provider.wallet.publicKey,
        mint: mintAddress,
        userTokenAccount: userATA,
      })
      .rpc();
      
    console.log("Withdraw tx:", tx);
    // TODO: Verificar que os 500k voltaram pra carteira do User
  });
});
```

*Nota: O Anchor é tão mágico que ele resolve automaticamente os endereços dos PDAs e do SystemProgram por baixo dos panos sem precisarmos passar todos eles explicitamente no TypeScript.*

### Rodando a Suíte de Testes
```bash
anchor test
```
*Output esperado:* Uma lista verdinha marcando os blocos (it) do neobank_vault com sucesso.

---

## 3. Deploy na Devnet! 🌐

Tudo testado localmente? É hora de ir para o mundo real (versão de testes). Vamos mandar nosso programa pra Solana Devnet.

```bash
# Aponta a CLI para a Devnet
solana config set --url devnet

# Pede dinheiro de mentira (2 SOL) para pagar a taxa de deploy.
solana airdrop 2
# Nota: Se o comando de airdrop acima falhar por rate-limit do IP, faça o processo pelo Faucet Web oficial:
# Acesse https://faucet.solana.com/ logando com seu GitHub. Preencha com o endereço gerado pelo `solana address`.

# Faz o deploy utilizando as chaves configuradas
anchor deploy --provider.cluster devnet
```

Pronto! Seu programa agora vive nativamente na Blockchain. Qualquer pessoa no mundo pode integrá-lo.
