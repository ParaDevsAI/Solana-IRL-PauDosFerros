# Verificando no Explorer 🔍

A melhor forma de atestar o sucesso da sua arquitetura é buscar no grande livro-razão aberto (o Explorer).

Acesse: [explorer.solana.com/?cluster=devnet](https://explorer.solana.com/?cluster=devnet)

Faça uma gincana rápida em sala:

1. **Cole seu Program ID:** Verifique que ele aparece como `Executable: Yes` e o owner é o `BPF Loader` (o sistema de deployer da Solana).
2. **Cole a assinatura da Transação (tx) de Deposit:** Expanda para ver as *Inner Instructions*. Você verá exatamente o CPI acontecendo: O seu programa ordenando o Token Program a transferir o saldo. Nos *Logs*, verá a mensagem customizada: `"Depositado: 1000000 tokens"`.
3. **Cole o PDA do Vault State:** O Explorer (devido ao IDL pareado pelo Anchor) vai mostrar pra você os dados mastigados e decodificados do banco: A authority (quem pode sacar), o total depositado, e o bump code.
4. **Cole o Vault Token Account (ATA do Cofre):** Balance: X tokens depositados. Qualquer um pode auditar esse saldo.

---

# 🎉 Parabéns, você acabou de construir DeFi!

Você superou a barreira de entrada da Solana. O programa que vocês acabaram de deployar (O Vault PDA associado à autoridade matemática) é o **BUILDING BLOCK (a fundação primordial)** do Decentralized Finance inteiro.

Os protocolos trilionários não fazem feitiçaria, eles fazem exatamente o que vocês acabaram de programar. Tudo se baseia nesse modelo que vocês construíram na mão hoje:
* **Kamino Finance** usa Vaults PDAs.
* **MarginFi (Project Zero)** usa Vaults PDAs.
* **Jupiter (Módulo de DCA e Limit Orders)** usa Vaults PDAs.
* **Realms (Tesourarias de DAOs)** usam Vaults PDAs.
