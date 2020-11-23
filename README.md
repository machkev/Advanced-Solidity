# Advanced Solidity - Crowdsale of PupperCoin Token

A new start-up has decided to crowdsale their PupperCoin token in order to help fund the network development.
This network will be used to track the dog breeding activity across the globe in a decentralized way, and allow humans to track the genetic trail of their pets. You have already worked with the necessary legal bodies and have the green light on creating a crowdsale open to the public. However, the start-up is required to enable refunds if the crowdsale is successful and the goal is met, and it is only allowed to raise a maximum of 300 Ether. The crowdsale will run for 24 weeks.

ERC20 token should be created and will be minted through a `Crowdsale` contract that can leverage the OpenZeppelin Solidity library.

This crowdsale contract will manage the entire process, allowing users to send ETH and get back PUP (PupperCoin).
This contract will mint the tokens automatically and distribute them to buyers in one transaction.

It will need to inherit `Crowdsale`, `CappedCrowdsale`, `TimedCrowdsale`, `RefundableCrowdsale`, and `MintedCrowdsale`.

The crowdsale will be conducted on the Kovan testnet in order to get a real-world pre-production testing.

---

### Instructions
---

Crowd-selling PupperCoin coded on Solidity to fund generic trailing project on PUPS. Two contracts will be written written and deployed on Remix.

<details>
    <summary>Creating the project</summary>

Using Remix, create a file called `PupperCoin.sol` and create a standard `ERC20Mintable` token.

Create a new contract named `PupperCoinCrowdSale.sol`, and prepare it like a standard crowdsale.
    
</details>

<details>
    <summary>Designing the contracts</summary>
    
1. ERC20 PupperCoin

Use a standard `ERC20Mintable` and `ERC20Detailed` contract, hardcoding `18` as the `decimals` parameter, and leaving the `initial_supply` parameter alone.

2. PupperCoincrowdsale

Leverage the [Crowdsale](../Starter-Code/Crowdsale.sol) starter code, saving the file in Remix as `Crowdsale.sol`.

Bootstrap the contract by inheriting the following OpenZeppelin contracts:

* `Crowdsale`

* `MintedCrowdsale`

* `CappedCrowdsale`

* `TimedCrowdsale`

* `RefundablePostDeliveryCrowdsale`

Provide parameters for all of the features of crowdsale, such as the `name`, `symbol`, `wallet` for fundraising and `goal`

Hardcode a `rate` of 1, to maintain parity with Ether units (1 TKN per Ether, or 1 TKNbit per wei). To customize crowdsale rate, follow the [Crowdsale Rate](https://docs.openzeppelin.com/contracts/2.x/crowdsales#crowdsale-rate) calculator on OpenZeppelin's documentation. Essentially, a token (TKN) can be divided into TKNbits just like Ether can be divided into wei. When using a `rate` of 1, just like 1000000000000000000 wei is equal to 1 Ether, 1000000000000000000 TKNbits is equal to 1 TKN.

Since `RefundablePostDeliveryCrowdsale` inherits the `RefundableCrowdsale` contract, which requires a `goal` parameter, the `RefundableCrowdsale` constructor must be called from `PupperCoinCrowdsale` constructor as well as the others. `RefundablePostDeliveryCrowdsale` does not have its own constructor, so just use the `RefundableCrowdsale` constructor that it inherits.

If one forgets to call the `RefundableCrowdsale` constructor, the `RefundablePostDeliveryCrowdsale` will fail since it relies on it (it inherits from `RefundableCrowdsale`), and does not have its own constructor.

When passing the `open` and `close` times, use `now` and `now + 24 weeks` to set the times properly from `PupperCoinCrowdsaleDeployer` contract.

3. PupperCoinSaleDeployer Contract
    
Leverage the [OpenZeppelin Crowdsale Documentation](https://docs.openzeppelin.com/contracts/2.x/crowdsales) for an example of a contract deploying another, as well as the starter code provided.
    
</details>

<details>
    <summary>Testing the Crowdsale</summary>

Test the crowdsale by sending Ether to the crowdsale from a different account (**not** the same account that is raising funds), then crowdsale works as expected is confirmed, try to add the token to MyCrypto and test a transaction. In order to test the time functionality, replace `now` with `fakenow`, and create a setter function to modify `fakenow` to whatever time want to simulate. Set the `close` time to be `now + 5 minutes`, or whatever timeline (eg. 5 minutes) to test for a shorter crowdsale.

When sending Ether to the contract, make sure to hit your `goal` that is set, and `finalize` the sale using the `Crowdsale`'s `finalize` function. In order to finalize, `isOpen` must return false (`isOpen` comes from `TimedCrowdsale` which checks to see if the `close` time has passed yet). Since the `goal` is 300 Ether, it needs  to be sent from multiple accounts. Create new workspace in Ganache, if prefunded accounts are exhausted. 

Remember, the refund feature of `RefundablePostDeliveryCrowdsale` only allows for refunds once the crowdsale is closed **and** the goal is met. See the [OpenZeppelin RefundableCrowdsale](https://docs.openzeppelin.com/contracts/2.x/api/crowdsale#RefundableCrowdsale) documentation for details as to why this is logic is used to prevent potential attacks on your token's value.

</details>

<details>
    <summary>Deploying the Crowdsale</summary>

Deploy the crowdsale to the Kovan or Ropsten testnet, and store the deployed address for later. Switch MetaMask to your desired network, and use the `Deploy` tab in Remix to deploy your contracts. Take note of the total gas cost, and compare it to how costly it would be in reality.
    
</details>

<details>
    <summary>Guide for Contract Deployment Process</summary>

* Deployment on Kovan testnet

In order for crowdsale contracts to function accurately, smart contracts should be executed in the following order.

1. Open Ganache and Metamask, change the network to Kovan. Pre-fund the address to ensure successful deployment of the contract as it would require some Gas. 

2. Deployment of the first contract `puppercoin` (Solidity codes written in this contract should be imported to `Crowdsale.sol`). Paramaters required for deployment: `name`, `symbol`, `initial_supply`.

3. Deploy `PupperCoinSaleDeployer` Contract. Parameters required: `name`, `symbol`, `wallet` (Same as your Wallet Address) and `goal`

    * Note: `Token_Address` & `Token_Sale_Address`
    
![Crowdsale_deployer](Images/Crowdsale_deployer.png)

4. Deploy `PupperCoinSale` Contract with `Token_sale_address` in the `At_Address` section

5. Deploy `PupperCoin` Contract with `Token_Address` in the `At_Address` section

6. Contract Deployed - Check the `getter` functions to see whether contract has been deployed properly. 

* Features

1. Buy Tokens under PupperCoinSale on Remix

   * Validate the transaction on [Etherscan](https://etherscan.io/)
   
   
2. Add Custom Tokens (`PUPCOIN`) to Metamask<br />
    (a) Under `Assets`, click on `Add Tokens`<br />
    (b) Under `Add Tokens`, click on `Custom Tokens`<br />
    (c) Parameters:<br />
    * `Token Address` - **`Token_Sale_Address`**<br />
    * `Name` - **`Token_Name`**<br />
    * `Decimal` - **`18`**<br />
                    
3. Metamask wallet gives an overview of tokens`

![Metamask_balances](Images/Metamask_balances.png)
    
4. View Tokens on MyCrypto Wallet
    
Access wallet on MyCrypto via the `private key` in Ganache. Add `Custom Token` in MyCrypto Wallet. Balance will be be updated. 
    
</details>

### Resources
---

* [Remix](https://remix.ethereum.org/)
* [Solidity_Docs](https://solidity.readthedocs.io/en/v0.7.0/)
* [Solidity_Example](https://solidity.readthedocs.io/en/v0.5.3/solidity-by-example.html#simple-open-auction)
* [OpenZeppelin Crowdsale Documentation](https://docs.openzeppelin.com/contracts/2.x/crowdsales)
* [Kovan_Faucet](https://faucet.kovan.network/)


