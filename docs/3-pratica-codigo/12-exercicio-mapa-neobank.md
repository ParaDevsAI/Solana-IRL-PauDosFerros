# Exercício: Mapa do Neobank 🗺️

Este é o resumo de ouro. Vamos fazer o de-para (mapeamento) de cada ação abstrata que fizemos lá no início usando o Vibecoding e a Solflare, e traduzir exatamente para a arquitetura de baixo nível da Solana que aprendemos hoje.

| O que fizemos no Noah / Solflare 💻 | O que realmente aconteceu na Solana (Baixo Nível) ⚙️ |
| :--- | :--- |
| **Criar uma carteira (Solflare)** | O *System Program* foi acionado para iniciar uma **Wallet Account** vazia atrelada a um Keypair e uma Chave Privada. |
| **Apertar o botão "Create Token"** | Chamamos a instrução de Initialize no **Token Program**, pedindo para ele criar e nos dar a posse de uma **Mint Account** (definindo lá dentro os decimals e a authority). |
| **Olhar quanto dinheiro tínhamos** | A carteira leu não o nosso perfil base, mas sim uma **Associated Token Account (ATA)**, uma conta **PDA** derivada usando as seeds (Sua Wallet + Endereço do Token). |
| **Clicar em "Enviar" tokens para um amigo** | O nosso frontend executou uma **CPI (Cross-Program Invocation)**. Nós pedimos educadamente ao *Token Program* para checar nossa assinatura e debitar nosso PDA e creditar o PDA do amigo. |
| **Ver o status verdinho no Explorer** | Significa apenas que a blockchain persistiu o estado. O Explorer atuou como leitor e formatou bonitinho para nós os dados contidos nas **Accounts** e **Historico de Transações**. |

---

> **Lição Final:** Esses são os pilares imutáveis. Não importa se você vai criar um Neobank, uma Exchange Descentralizada Complexa (DEX), Mercado de Previsão ou Jogo. Se você entender essa base arquitetural, desenvolver o resto é só combinar esses blocos mágicos (Composabilidade Lego).
