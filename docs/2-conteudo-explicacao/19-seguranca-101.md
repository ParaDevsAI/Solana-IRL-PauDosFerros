# Segurança 101 ⚠️

Mesmo com um ferramental poderoso como o Anchor blindando boa parte das falhas convencionais, o desenvolvedor ainda é o responsável pela lógica de negócios. Um erro na Solana custa caro. 

Aqui estão as regras de ouro:

1. **Valide o *owner* das accounts:** O Anchor já faz isso quase que automaticamente caso você utilize os macros como `#[account(owner = ...)]`, garantindo que ninguém injete contas maliciosas de outros programas.
2. **Verifique os signers:** A constraint `Signer<'info>` assegura na marra que a transação possui a assinatura criptográfica daquela conta. Nunca, em hipótese alguma, confie na instrução sem verificar quem está mandando.
3. **Seeds corretos para PDAs:** As sementes (seeds) determinam o cofre. Seeds errados = account errada manipulada = *exploit* completo dos seus fundos. Revise sua derivação sempre.
4. **Checked math (Sempre!):** Ao invés de somar saldos com `+`, use `.checked_add()`, `.checked_sub()`, `.checked_mul()`. Ataques de Overflow / Underflow enchem os bolsos de hackers diariamente.
5. **Nunca confie em dados que vêm do cliente (Frontend):** O cliente é sempre um adversário em potencial na blockchain. Tudo que vier de input via SDK deve ser re-validado na sua pipeline do Anchor on-chain.

> O Anchor cuida sozinho de uns 80% das falhas de segurança mais comuns (ex: verificar as sysvars). Os outros 20% estão puramente na sua lógica. Usar config com skills da **Trail of Bits** (empresa especialista em auditoria) no `solana-claude-config` corta esse risco quase a zero para hackathons.
