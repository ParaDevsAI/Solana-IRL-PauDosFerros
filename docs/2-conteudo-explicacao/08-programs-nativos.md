# Programs Nativos da Solana 🏢

Uma das maiores vantagens da arquitetura da Solana é que **você não precisa reimplementar a roda**. A rede já possui um conjunto de smart contracts chamados de **Programs Nativos** (ou Cânonicos) que rodam hardcoded nos validadores. 

Eles são altamente otimizados e seguros. Seus programas customizados vão passar o tempo todo conversando com eles via CPI (Cross-Program Invocations).

### 1. System Program (O Gerente do Prédio)
É o programa fundamental da Solana. Suas funções incluem:
* Criar novas *Accounts*.
* Transferir a moeda nativa **SOL** entre carteiras.
* Alocar memória e designar *ownership* (dono) de uma account para outro programa.

### 2. Token Program (O Banco Central)
Na grande maioria das outras blockchains, você sobe um contrato gigantesco toda vez que quer criar uma nova moeda. Na Solana, existe apenas um programa global para isso. Suas funções:
* Criar (Initialize) um novo token (*Mint*).
* Transferir, Mintar e Queimar (*Burn*) tokens convencionais.
* *Nota: Existe também a versão nova, o Token-2022, que adiciona recursos nativos como taxas por transferência ou hooks customizados.*

### 3. Associated Token Program (O Criador de Database)
Tokens na Solana não ficam "soltos" na sua carteira principal. Eles vivem em "gavetas" específicas chamadas Token Accounts. O *Associated Token Program* serve exclusivamente para:
* Criar contas determinísticas para os usuários guardarem moedas.
* Ele calcula as seeds com um Hash contendo: `Sua Wallet + o Mint do Token`.
* Isso garante que sempre saberemos onde está o saldo do usuário, sem a necessidade de um banco de dados centralizado.

> **Resumo da Composabilidade Atômica:** Quando você constrói um App (como um Neobank), o seu código principal orquestra a lógica, chama o *System Program* para criar contas, e o *Token Program* para movimentar o dinheiro. Tudo atrelado e seguro.
