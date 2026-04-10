# Projeto Final: Construindo o Neobank Vault 🏦

Agora é a hora de colocar a mão no código e aplicar Arquitetura, PDAs, Rent e CPIs na prática usando o Anchor Framework.

## A Arquitetura do Vault
Nosso modelo se baseia no core de grandes protocolos como Kamino (Lending), Meteora (Yield), Jupiter (DCA) e Realms (DAO Treasuries):
1. **Deposit:** O usuário envia seus tokens da sua conta pessoal (ATA) para o cofre do contrato (um PDA Vault).
2. **Withdraw:** O cofre (PDA Vault) assina a transação autonomamente e devolve os tokens para o usuário.

---

## 1. Scaffold Inicial (Para rodar no Terminal)

```bash
# Inicie o projeto Anchor
anchor init neobank_vault
cd neobank_vault
```

Arquivos importantes que foram gerados e com os quais vamos trabalhar juntos:
* `programs/neobank_vault/src/lib.rs` → Onde vive a nossa lógica (Smart Contract).
* `tests/neobank_vault.ts` → Onde testaremos as transações usando o cliente.
* `Anchor.toml` → Configurações e endereços da rede.

---

## 2. Escrevendo Lógica (`lib.rs`)

Abra o arquivo `lib.rs`, limpe o boilerplate e vamos escrever nossas instruções principais.

### A. Imports e Declaração de ID
```rust
use anchor_lang::prelude::*;
use anchor_spl::token_interface::{
    transfer_checked, Mint, TokenAccount,
    TokenInterface, TransferChecked,
};

// Aqui o Anchor colocará o ID real do seu programa
declare_id!("YOUR_PROGRAM_ID"); 
```

### B. O Módulo Principal (Onde as rotas vivem)
```rust
#[program]
pub mod neobank_vault {
    use super::*;

    // INSTRUÇÃO 1: DEPOSIT 
    pub fn deposit(ctx: Context<Deposit>, amount: u64) -> Result<()> {
        // Efetuamos um CPI pro Token Program transferir os fundos: user → vault
        transfer_checked(
            CpiContext::new(
                ctx.accounts.token_program.to_account_info(),
                TransferChecked {
                    from: ctx.accounts.user_token_account.to_account_info(),
                    to: ctx.accounts.vault_token_account.to_account_info(),
                    mint: ctx.accounts.mint.to_account_info(),
                    authority: ctx.accounts.user.to_account_info(), // Usuário assina autorizando
                },
            ),
            amount,
            ctx.accounts.mint.decimals,
        )?;

        // Atualizamos o estado do nosso "Banco de Dados"
        let vault_state = &mut ctx.accounts.vault_state;
        vault_state.total_deposited += amount;
        
        msg!("Depositado: {} tokens", amount);
        Ok(())
    }

    // INSTRUÇÃO 2: WITHDRAW
    pub fn withdraw(ctx: Context<Withdraw>, amount: u64) -> Result<()> {
        let vault_state = &mut ctx.accounts.vault_state;
        
        // Proteção contra saque indevido
        require!(vault_state.total_deposited >= amount, VaultError::InsufficientFunds);

        // O SEGREDO DO PDA: O programa precisa assinar para retirar os fundos do vault
        let authority_key = ctx.accounts.user.key();
        let seeds = &[
            b"vault",
            authority_key.as_ref(),
            &[vault_state.bump], // O bump garante que caiu no endereço correto fora da curva elíptica
        ];
        let signer_seeds = &[&seeds[..]];

        transfer_checked(
            // Utilizamos new_with_signer para embutir a "assinatura fake" do PDA
            CpiContext::new_with_signer(
                ctx.accounts.token_program.to_account_info(),
                TransferChecked {
                    from: ctx.accounts.vault_token_account.to_account_info(),
                    to: ctx.accounts.user_token_account.to_account_info(),
                    mint: ctx.accounts.mint.to_account_info(),
                    authority: ctx.accounts.vault_state.to_account_info(), // O VaultState PDA é o dono da grana
                },
                signer_seeds, 
            ),
            amount,
            ctx.accounts.mint.decimals,
        )?;

        vault_state.total_deposited -= amount;
        msg!("Sacado: {} tokens", amount);
        Ok(())
    }
}
```

---

## 3. Estruturação dos Dados e Contas

Todo aquele array de contas que aprendemos em "Instructions" é validado nas Structs abaixo. O macro `#[derive(Accounts)]` faz a mágica de segurança do Anchor.

### A. Contexto de Deposit
```rust
#[derive(Accounts)]
pub struct Deposit<'info> {
    #[account(mut)]
    pub user: Signer<'info>, // Quem deposita assina a transação e paga as taxas

    // Banco de Dados na Solana: PDA
    #[account(
        init_if_needed, 
        payer = user,
        space = 8 + VaultState::INIT_SPACE, // Rent dinâmico
        seeds = [b"vault", user.key().as_ref()], // Chave primária (Vault do Usuário)
        bump,
    )]
    pub vault_state: Account<'info, VaultState>,

    #[account(mut)]
    pub user_token_account: InterfaceAccount<'info, TokenAccount>,

    // A "Conta Corrente" do Vault para este token específico
    #[account(
        init_if_needed,
        payer = user,
        associated_token::mint = mint,
        associated_token::authority = vault_state, // O dono do dinheiro é o Estado do Vault (PDA)
    )]
    pub vault_token_account: InterfaceAccount<'info, TokenAccount>,

    pub mint: InterfaceAccount<'info, Mint>,
    
    // Programs essenciais chamados para a transação ocorrer (Nativos)
    pub token_program: Interface<'info, TokenInterface>,
    pub associated_token_program: Program<'info, AssociatedToken>,
    pub system_program: Program<'info, System>,
}
```

### B. O Estado Global (Tabelas) e Erros
```rust
#[account]
#[derive(InitSpace)] // Macro que calcula o espaço dos bytes automaticamente
pub struct VaultState {
    pub authority: Pubkey,    // 32 bytes — quem pode sacar
    pub total_deposited: u64, // 8 bytes  — saldo total no cofre
    pub bump: u8,             // 1 byte   — semente de derivação do PDA
}

// Matemática do Rent: 8 (discriminator padrão do Anchor) + 32 + 8 + 1 = 49 bytes.
// Custo de rent estimado: frações de centavo de Dólar.

// Tratamento amigável de erros da operação
#[error_code]
pub enum VaultError {
    #[msg("Fundos insuficientes no cofre para realizar esse saque.")]
    InsufficientFunds,
}
```
