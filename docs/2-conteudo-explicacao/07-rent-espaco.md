# Rent e Alocação de Espaço 💰

Em blockchains, armazenar dados no estado global é o recurso mais caro e escasso que existe. Para evitar que desenvolvedores enviem spam ou inchem a rede com dados inúteis (State Bloat), a Solana introduziu o conceito de **Rent (Aluguel)**.

Basicamente: toda account criada ocupa um espaço em bytes, e você precisa pagar para manter esse espaço lá.

### Rent-Exempt (Isento de Aluguel)
O mercado adotou quase que exclusivamente este modelo. Em vez de pagar "aluguel mensal", você faz um **depósito mínimo que garante a conta para sempre**. 

* Se você alocar na account uma quantidade de SOL suficiente para cobrir **2 anos de Rent**, o sistema declara sua conta como `rent-exempt` (isenta de pagar aluguel contínuo).
* Se a conta for encerrada/deletada, esse depósito mínimo é **reembolsado** ao criador!
* O custo médio é na casa de **~0.00089 SOL por byte/ano**. Muito barato.

### Quem Paga Essa Conta?
O **Payer** da transação (A carteira de quem assina a transação e inicia a instrução de criação). Na prática, significa que o seu usuário final vai gastar algumas frações de centavo de Dólar na primeira interação dele com o seu programa para "iniciar o perfil" ou o banco de dados pessoal dele.

---

## Cálculo Prático no Anchor Framework

Quando construímos uma struct (tabela) usando o Anchor, precisamos dizer pro compilador o tamanho exato de espaço que essa conta vai ocupar.

* **O Discriminador:** Toda account criada pelo Anchor possui nativamente 8 bytes iniciais adicionados automaticamente. Usamos eles para identificar que tipo de struct de dados a account representa.

```rust
#[account]
pub struct Counter {
    pub count: u64,          // Inteiro ocupa 8 bytes
    pub authority: Pubkey,   // Uma carteira (PublicKey) sempre ocupa 32 bytes
}
```

**Matemática Final:**
* `8 bytes` (Discriminador do Anchor)
* `+ 8 bytes` (variável count)
* `+ 32 bytes` (variável authority)
* **Total = 48 bytes**.

Sua conta custará aproximadamente `~0.001 SOL` para ser criada na blockchain, imutável para a eternidade.
