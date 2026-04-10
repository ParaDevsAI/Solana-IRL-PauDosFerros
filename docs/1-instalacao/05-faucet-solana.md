# Como pegar fundos para testes (Faucet) 🚰

Para conseguirmos fazer o deploy dos nossos programas e pagar as taxas de rede (Rent e Transações), precisamos de SOL. Como estamos na **Devnet** (Rede de Desenvolvimento), não precisamos gastar dinheiro real — usaremos um Faucet!

Ocasionalmente o comando `solana airdrop 2` pode falhar diretamento do seu terminal devido a limites de requisição por IP. A forma oficial e à prova de falhas para contornar isso é utilizando o Web Faucet Oficial.

### Passo a passo para o Faucet Web:
1. Acesse o site oficial: [faucet.solana.com](https://faucet.solana.com/)
2. Clique no botão de autenticação e faça o login com a sua conta do **GitHub** (isso previne bots de drenarem a rede).
3. No seu terminal, pegue o endereço público da sua carteira (gerada na instalação da Solana CLI) rodando:
   ```bash
   solana address
   ```
4. Cole esse endereço gerado lá na caixinha de texto do site.
5. Selecione a quantidade desejada (ex: **2 ou 5 SOL**) e clique para enviar.
6. Confira no seu terminal se as moedas caíram executando o comando:
   ```bash
   solana balance
   ```

Tudo certo! Com esse saldo você conseguirá cobrir o rent de qualquer conta de programa (`Program PDA`) e dezenas de milhares de instruções nos Hackathons.
