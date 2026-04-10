# Revisão: Core Concepts da Solana 🧠

Antes de colocar a mão na massa com o Anchor, é fundamental entender os pilares arquiteturais da Solana. Se você dominar esses 5 conceitos, você entende como a Solana funciona por baixo dos panos.

---

## 1. Accounts & Ownership (Contas e Posse)

Na Solana, **TUDO é uma Account**. Pense nelas como arquivos em um sistema operacional.
Toda Account possui: um endereço (Public Key), saldo (Lamports), Dados (Data) e um Dono (Owner).

Existem basicamente 3 tipos:
1. **Wallet (Carteira):** Pertence a um usuário comum (System Program).
2. **Data Account:** Guarda estado e informações.
3. **Program Account:** Guarda o código executável de um Smart Contract.

> **Regra de Ouro do Ownership:** Qualquer um pode ler os dados de uma Account. Mas **apenas o Owner** (o Programa dono daquela conta) pode escrever ou modificar seus dados.

---

## 2. Programs Stateless (Programas sem Estado)

Diferente de redes como o Ethereum, na Solana o **Código é separado dos Dados**. 
- Os Smart Contracts (Programs) são **Stateless** (não guardam variáveis locais entre execuções).
- Quando você chama um Program, você precisa passar para ele **todas as Accounts** que ele vai precisar ler ou modificar durante aquela execução.

As ações são chamadas de **Instructions**. Um pacote de Instructions forma uma **Transaction**, que é atômica (ou roda tudo com sucesso, ou falha tudo e nada muda).

---

## 3. PDAs (Program Derived Addresses)

PDAs são os "bancos de dados" relacionais da Solana. São contas que **não possuem Chave Privada**, o que significa que nenhum humano pode assiná-las diretamente. 

Como funcionam:
`Endereço = hash(seeds + program_id + bump)`

*   **Account Normal:** Endereço totalmente aleatório gerado a partir de um Keypair.
*   **PDA:** Endereço **Determinístico** gerado a partir de *seeds* (como a união de uma string "vault" + a chave de um usuário).

Isso significa que o frontend consegue adivinhar e derivar o endereço exato dos dados de um usuário sem precisar consultar uma API. E quem assina as transações em nome dessas Accounts é o próprio Smart Contract!

---

## 4. CPIs (Cross-Program Invocations)

Na Web2 temos chamadas de API (um microsserviço chamando outro). Na Solana temos os CPIs: **Programas chamando outros Programas**. A diferença é que a chamada é *on-chain* e atômica.

**Exemplo Prático (Deposit em um Vault):**
1. O usuário assina uma transação chamando o SEU programa.
2. Seu programa faz um CPI chamando o **Token Program** oficial da rede para transferir tokens (Ex: `token_program.transfer_checked(...)`).
3. O Token Program verifica as permissões e move o saldo.

> Seu programa orquestra. Os programas oficiais executam a operação base. **Se o CPI falhar, a transação inteira falha e é revertida.** Isso garante segurança total e composabilidade.

---

## 5. O Ciclo de Vida de uma Transação (150ms)

O fluxo completo desde o clique até a blockchain:

1. **App cria e assina:** Seu frontend monta a instrução e a carteira do usuário assina.
2. **RPC Roteia:** O nó recebe a transação e manda direto pro Líder atual da rede.
3. **Validator Ordena (PoH):** O validador usa o *Proof of History* para colocar a transação no tempo.
4. **Program Lê e Modifica:** O Smart Contract é executado, processando a lógica.
5. **Estado Atualizado:** O saldo das contas/dados mudam e ficam persistidos de forma imutável na blockchain.

Tudo isso acontece em **milissegundos**. Esse é o superpoder da arquitetura da Solana!
