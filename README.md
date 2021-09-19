# we sure can attract a crowd!

![crowd](Images/crowd.png)

## Background

Our company has decided to crowdsale their PupperCoin token in order to help fund the network development.
This network will be used to track the dog breeding activity across the globe in a decentralized way, and allow humans to track the genetic trail of their pets. we have already worked with the necessary legal bodies and have the green light on creating a crowdsale open to the public. However, we are required to enable refunds if the crowdsale is successful and the goal is met, and we are only allowed to raise a maximum of 300 Ether. The crowdsale will run for 24 weeks.

We will need to create an ERC20 token that will be minted through a `Crowdsale` contract that we can leverage from the OpenZeppelin Solidity library.

This crowdsale contract will manage the entire process, allowing users to send ETH and get back PUP (PupperCoin).
This contract will mint the tokens automatically and distribute them to buyers in one transaction.

It will need to inherit `Crowdsale`, `CappedCrowdsale`, `TimedCrowdsale`, `RefundableCrowdsale`, and `MintedCrowdsale`.

we will conduct the crowdsale on the Kovan or Ropsten testnet in order to get a real-world pre-production test in.

### Creating our project

Using Remix, create a file called `PupperCoin.sol` and create a standard `ERC20Mintable` token. 

Create a new contract named `PupperCoinCrowdsale.sol`, and prepare it like a standard crowdsale.

### Designing the contracts

#### ERC20 PupperCoin

we will need to simply use a standard `ERC20Mintable` and `ERC20Detailed` contract, hardcoding `18` as the `decimals` parameter, and leaving the `initial_supply` parameter alone.

we don't need to hardcode the decimals, however since most use-cases match Ethereum's default, we may do so.

Simply fill in the `PupperCoin.sol` file with this [starter code](../Starter-Code/PupperCoin.sol), which contains the complete contract we'll need to work with in the Crowdsale.

#### PupperCoinCrowdsale

Leverage the [Crowdsale](Code/Crowdsale.sol) starter code, saving the file in Remix as `Crowdsale.sol`.

We will need to bootstrap the contract by inheriting the following OpenZeppelin contracts:

* `Crowdsale`

* `MintedCrowdsale`

* `CappedCrowdsale`

* `TimedCrowdsale`

* `RefundablePostDeliveryCrowdsale`

We will need to provide parameters for all of the features of our crowdsale, such as the `name`, `symbol`, `wallet` for fundraising, `goal`, etc. 

We can hardcode a `rate` of 1, to maintain parity with Ether units (1 TKN per Ether, or 1 TKNbit per wei). If we'd like to customize our crowdsale rate, follow the [Crowdsale Rate](https://docs.openzeppelin.com/contracts/2.x/crowdsales#crowdsale-rate) calculator on OpenZeppelin's documentation. Essentially, a token (TKN) can be divided into TKNbits just like Ether can be divided into wei. When using a `rate` of 1, just like 1000000000000000000 wei is equal to 1 Ether, 1000000000000000000 TKNbits is equal to 1 TKN.

Since `RefundablePostDeliveryCrowdsale` inherits the `RefundableCrowdsale` contract, which requires a `goal` parameter, we must call the `RefundableCrowdsale` constructor from our `PupperCoinCrowdsale` constructor as well as the others. `RefundablePostDeliveryCrowdsale` does not have its own constructor, so just use the `RefundableCrowdsale` constructor that it inherits.

If we forget to call the `RefundableCrowdsale` constructor, the `RefundablePostDeliveryCrowdsale` will fail since it relies on it (it inherits from `RefundableCrowdsale`), and does not have its own constructor.

When passing the `open` and `close` times, use `now` and `now + 24 weeks` to set the times properly from our `PupperCoinCrowdsaleDeployer` contract.

### Deploying the Crowdsale

Deploy the crowdsale to the Kovan or Ropsten testnet, and store the deployed address for later. Switch MetaMask to our desired network, and use the `Deploy` tab in Remix to deploy our contracts. Take note of the total gas cost, and compare it to how costly it would be in reality. Since we are deploying to a network that we don't have control over, faucets will not likely give out 300 test Ether. we can simply reduce the goal when deploying to a testnet to an amount much smaller, like 10,000 wei.
