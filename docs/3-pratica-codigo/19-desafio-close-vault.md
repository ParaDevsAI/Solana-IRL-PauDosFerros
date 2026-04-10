# Desafio: Adicione a instrução `close_vault` 🎯

Já entendemos como cobrar **Rent** (Taxa de Alocação de Espaço) quando abrimos um PDA `vault_state` e criamos a `vault_token_account`. Mas e quando o protocolo acaba ou o usuário decide encerrar aquele cofre? Como somos bons cidadãos na blockchain e limpamos o estado para receber o depósito do Rent de volta?

**O Desafio:** Crie uma instrução que:
1. Transfere **TODOS** os tokens de volta para a carteira do usuário.
2. Fecha a `vault_token_account` para devolver o Rent ao usuário.
3. Fecha a `vault_state` account para devolver o Rent ao usuário.

* 💡 **Dica 1:** Use `close = user` na constraint do macro `#[account()]` do `vault_state`.
* 💡 **Dica 2:** Você não consegue fechar contas de Token que ainda possuem saldo dentro, então a transferência de saldo deve ocorrer primeiro.
* 💡 **Dica 3:** Use Inteligência Artificial como copiloto! `(Tempo estimado: 10 minutos) GO!`

---

## Solução Oficial (Gabarito)

Se você seguiu os passos corretamente e aproveitou a elegância poderosa do Anchor, seu resultado (boilerplate) ao adicionar o `close_vault` no seu `lib.rs` deve ser algo muito parecido com isso:

```rust
pub fn close_vault(ctx: Context<CloseVault>) -> Result<()> {
    let vault_state = &ctx.accounts.vault_state;
    let amount = vault_state.total_deposited;
    
    // Transfere tudo o que sobrou (se sobrou) de volta.
    if amount > 0 {
        let authority_key = ctx.accounts.user.key();
        let seeds = &[b"vault", authority_key.as_ref(), &[vault_state.bump]];
        let signer_seeds = &[&seeds[..]];
        
        transfer_checked(
            CpiContext::new_with_signer(
                ctx.accounts.token_program.to_account_info(),
                TransferChecked {
                    from: ctx.accounts.vault_token_account.to_account_info(),
                    to: ctx.accounts.user_token_account.to_account_info(),
                    mint: ctx.accounts.mint.to_account_info(),
                    authority: ctx.accounts.vault_state.to_account_info(),
                },
                signer_seeds,
            ),
            amount,
            ctx.accounts.mint.decimals,
        )?;
    }
    
    // E depois disso? Nada! O Anchor faz a limpeza automática devido às constraints Struct abaixo.
    msg!("Vault completamente fechado e Rent devolvido!");
    Ok(())
}

#[derive(Accounts)]
pub struct CloseVault<'info> {
    #[account(mut)] 
    pub user: Signer<'info>,
    
    #[account(
        mut,
        seeds = [b"vault", user.key().as_ref()],
        bump = vault_state.bump,
        close = user, // <-- A MÁGICA: Fecha a account e devolve as lamports pro user!
    )]
    pub vault_state: Account<'info, VaultState>,
    
    // (As contas de token e os Programs seguiriam declarados aqui também...  )
}
```

> **Por que isso é absurdo de bom?** O `close = user` abstrai a matemática de manipulação de *lamports* base da Solana e simplesmente apaga o estado da memória da blockchain quando a transação termina, ejetando as solanas de volta para seu dono original. Elegante, ultra seguro e limpo.
