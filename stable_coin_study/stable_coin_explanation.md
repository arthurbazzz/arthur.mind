
![Stablecoin](../images/stablecoin_image.png)

Stablecoin é uma criptomoeda cujo valor normalmente é atrelado a um ativo do mundo real, por exemplo: BRL, USD, EUR.

Nesse caso a volatilidade que acontece nas criptomoedas é amenizada, pegando o exemplo da empresa BRD no qual ``1 BRD ~= 1 BRL``.

Diferente das criptomoedas mais voláteis, como o Bitcoin, a stablecoin foi criada para oferecer maior previsibilidade de preço e mais estabilidade no uso diário. Isso acontece porque o token é respaldado por um lastro, ou seja, por uma garantia que confirma o valor do ativo.

Além disso, as stablecoins mantêm diversas vantagens das criptomoedas, como:
- Transações 24/7
- Liquidações em segundos

****
## Exemplos de stablecoins

| Stablecoin | Ticker | Blockchain principal | Market Cap | Lastro |
| --- | --- | --- | --- | --- |
| Tether | USDT | Ethereum, Tron | ~$144B | Títulos EUA, caixa |
| USD Coin | USDC | Ethereum, Solana | ~$62B | Títulos EUA, caixa |
| DAI | DAI | Ethereum | ~$5B | Cripto sobrecolateralizado |
| First Digital USD | FDUSD | Ethereum, BNB | ~$1.8B | Títulos EUA, caixa |
| BRD | BRD | Solana | ~R$ 730K | Tesouro Nacional BR |
| BRZ (Transfero) | BRZ | Ethereum, Solana, Algorand | ~R$ 100M+ | Títulos públicos, caixa |
| BRL1 (Mercado Bitcoin) | BRL1 | Ethereum, Polygon | ~R$ 60M+ | Caixa custodiado |
| DREX (Banco Central) | DREX | Plataforma própria (piloto) | N/A (em teste) | Real Digital — CBDC |
| cBRZ (CloudWalk) | cBRZ | Rede própria | N/D | Lastro em recebíveis |

****

### O que seria Lastro?
Lastro é a garantia real que prova que algo tem valor não apenas para stablecoins, mas também para o mercado financeiro e outras aplicações.

Mas no nosso caso de estudo específico, podemos encontrar 4 tipos diferentes (baseado nas minhas pesquisas):

| | Definição | Depósito | Saque |
| --- | --- | --- | --- |
| Lastro em Moeda Fiduciária | Mantém reservas físicas ou financeiras da moeda de referência (ex: Dólar). Para cada token emitido, a empresa guarda US$ 1 em caixa. |  A empresa emissora mantém ``BRL 1.000`` depositados em uma conta bancária tradicional ou aplicados em títulos públicos de liquidez imediata, criando um Token Digital de acordo com a quantidade depositada. Por exemplo: ``50 BRL ~= 50 Tokens Digitais``. | No momento do saque/resgate, você pode resgatar todo o valor depositado. Sendo assim, seus Tokens Digitais vão ser deletados.

| | Definição | Depósito | Segurança |
| --- | --- | --- | --- |
| Lastro em Criptomoedas | Neste modelo descentralizado, o lastro não está em um banco, mas sim guardado dentro de contratos inteligentes (smart contracts) na blockchain | Para emitir ``BRL 1.000`` em stablecoins, o sistema exige que alguém deposite cerca de ``BRL 1.500`` ou ``BRL 2.000`` em outras criptomoedas (como Ethereum ou Bitcoin) como garantia | Se o valor do Bitcoin cair drasticamente, o sistema vende a garantia automaticamente para garantir que os seus ``BRL 1.000`` continuem valendo ``BRL 1.000``

| | Definição | Como funciona? |
| --- | --- | --- |
| Lastro em Algorítmicas | Este modelo não utiliza dinheiro guardado, mas sim matemática e incentivos financeiros | O sistema usa um algoritmo computacional para controlar a quantidade de moedas em circulação. Se o preço da stablecoin cai abaixo de ``BRL 1,00``, o algoritmo queima moedas para reduzir a oferta e fazer o preço subir. Se passa de ``BRL 1,00``, ele cria novas moedas para o preço cair.

| | Definição | Depósito | Saque |
| --- | --- | --- | --- |
| Lastro em Commodities | Têm o seu valor atrelado a matérias-primas ou metais preciosos, como o ouro (ex: PAX Gold). | Cada unidade financeira emitida equivale a uma fração exata da mercadoria. Por exemplo: 1 token = 1 grama de ouro; ou 1 contrato = 1 saca de soja. | O sistema busca a cotação exata da commodity naquele momento. Se você tem um token lastreado em 10 gramas de ouro e o grama vale R$ 400, o sistema converte seu ativo em R$ 4.000. |
****

## Blockchain no contexto de Stablecoins
Blockchain é um livro-razão (ledger) digital distribuído e imutável, onde transações são registradas em blocos encadeados criptograficamente.
- **Na prática:** Imagine um Excel compartilhado por milhares de computadores no mundo. Cada linha nova precisa ser validada por consenso da rede antes de entrar. Uma vez registrada, ninguém consegue alterar ou apagar — porque cada bloco contém um "hash" do bloco anterior, formando uma corrente (daí o nome block + chain).
- **Por que importa pra stablecoins:** A blockchain é o "trilho" onde os tokens circulam. Em vez de depender de um banco central para validar transferências, a própria rede descentralizada garante que a transação é legítima. A BRD usa a Solana (~400ms por transação); USDT usa Ethereum, Tron etc.

****
## Token no contexto de Stablecoins
Token é a representação digital de um ativo na blockchain. No caso de stablecoins: você deposita ``R$ 1`` em custódia na ``BRD → a BRD emite 1 token BRD na Solana`` a seu favor. Esse token vive na blockchain, pode ser transferido, guardado em wallet, usado em DeFi — mas sempre representa aquele R$ 1 real custodiado.
Ou seja, o token não é o dinheiro em si, é um voucher digital que prova que você tem direito a sacar aquele valor de volta. Quando você resgata, o token é queimado (burned) e o real é liberado.
****

## DeFi - Decentralized Finance (Finanças Descentralizadas)
São serviços financeiros — empréstimos, rendimento, trocas, seguros — que rodam direto na blockchain via smart contracts, sem intermédio de banco, corretora ou qualquer instituição central.

****
## Material complementar
[Coinbase: O que é uma stablecoin?](https://www.coinbase.com/pt-br/learn/crypto-basics/what-is-a-stablecoin)
[Nubank: Stablecoin e suas vantagens no mercado de criptomoedas](https://blog.nubank.com.br/o-que-e-stablecoin-como-funciona/)
[Stripe: Criação de uma stablecoin: do conceito ao lançamento](https://stripe.com/br/resources/more/creating-a-stablecoin-from-concept-to-launch)