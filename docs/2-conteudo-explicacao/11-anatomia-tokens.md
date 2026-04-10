# Anatomia dos Tokens na Solana 🪙

Diferente de outras redes onde o saldo de tokens fica armazenado numa variável `mapping(address => uint256)` dentro de um Smart Contract gigante, na Solana toda a arquitetura de tokens é incrivelmente modularizada.

Quando juntamos tudo o que aprendemos sobre PDAs e Programs Nativos, conseguimos desvendar a anatomia perfeita de um Token (Ex: o "SuperReal" que criamos no app Noah).

### 1. O Token Program (Motor)
É a engine central que dita as lógicas matemáticas de transferência.

### 2. A Mint Account (A Fábrica)
É uma conta base criada pelo Token Program que guarda os dados vitais da economia do token:
* **Supply Total** (A quantidade atual de moedas que existe).
* **Decimals** (Se o token pode ser fracionado ou não).
* **Mint Authority** (Qual carteira ou programa tem a "senha" permitida para imprimir e injetar novas moedas na rede).
* *Exemplo da aula:* "SuperReal", 9 decimals, Supply inicial infinito / 1M impresso.

### 3. Metadata Account (A Vitrine)
O Token Program puro só entende de números. Ele não sabe o nome do token e muito menos a imagem. Para isso, o *Metadata Program* varre a rede e cria uma conta PDA atrelada ao Mint.
* **Derivação:** Hash(`"metadata"` + `Endereço do Mint`)
* **Conteúdo:** Nome ("SuperReal"), Símbolo ("SREAL") e a URL da Imagem que vai aparecer na sua Solflare ou Phantom.

### 4. ATA - Associated Token Account (A Conta Corrente)
Você, como usuário portador de uma Wallet convencional, não guarda "tokens" dentro da sua Wallet. Sua Wallet guarda apenas a sua chave e o seu saldo na moeda nativa (SOL).

Para você receber o token SuperReal, o sistema precisou derivar um PDA (um cofre sem chave privada) chamado **ATA**.
* **Derivação:** Hash(`Sua Wallet` + `Endereço do Mint do SuperReal`)
* **Conteúdo:** O seu saldo atualizado (ex: 1.000.000 SREAL).

> A beleza dessa composição é a segurança atômica. Se um hacker tentar roubar tokens da sua Wallet, ele vai falhar, pois os tokens não estão lá, estão em compartimentos (ATAs) indexados que respondem exclusivamente à sua assinatura.
