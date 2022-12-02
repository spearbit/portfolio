![](https://i.imgur.com/Wao4sYK.png)

## Table of Contents

- [What is a Bridge?](#what-is-a-bridge)
- [The significance of bridges](#the-significance-of-bridges)
- [How does a bridge operate?](#how-does-a-bridge-operate)
- [Common vulnerabilities of bridges](#common-vulnerabilities-of-bridges)
- [Spearbit's Bridge Clients](#spearbits-bridge-clients)
- [How are we helping secure these bridges?](#how-are-we-helping-secure-these-bridges)

<br>

## What is a Bridge?

A blockchain bridge is a protocol that lets users send messages (data) between chains. These messages could be simple (sending information) or complex (transferring assets). Bridges solve one of the crypto spaces' main pain points - a lack of interoperability. The vast majority of blockchain assets exist natively only on a single blockchain, with a few exceptions (such as native USDC on both Ethereum and Solana). Bridges can therefore facilitate the usage of a synthetic bridged representation of a native asset on blockchain A on another blockchain B.

Bridges can be categorized as either trusted (custodial) or trust minimized (non-custodial). The differences between these two categories can be defined as:

- **Trusted:** Trusted bridges depend upon a central entity or system for their operations. They have trust assumptions with respect to the custody of funds and the security of the bridge. Users of custodial bridges rely on the bridge operator’s reputation to inform their trust assumptions.
- **Trust Minimized:** Trust minimized bridges operate using smart contracts and economically incentivized participants. In theory, these bridges are 'trustless'. However, in actuality, each decentralized bridge makes tradeoffs with regards to trust and speed.

Bridges can be further categorized by the methodology used to verify transactions on the origin and destination chains:

- **Externally Verified:** Externally verified bridges rely on a 3rd party (an address or multisig governed by a counterparty or counterparties) to validate transactions on both the destination and origin chains. For externally verified bridges, a third party is trusted.
- **Natively Verified:** Natively verified bridges run a validator (or a light client) for the origin chain on the destination chain to verify that a transaction has occurred. Natively verified bridges inherit the trust assumptions of the origin chain. An example would be a Bitcoin light client implemented as a smart contract on the Ethereum blockchain. 
- **Optimistically Verified:** Optimistically verified bridges assume that transactions are valid and allow anyone to challenge a transaction within a certain window of time before finalizing the transaction. These bridges rely heavily on economic trust assumptions. Examples: optimistic rollups, and the NEAR<>Ethereum rainbow bridge.

Lastly, trust minimized bridges can be classified by the type of data that they are transferring:

- **Arbitrary Message Bridge (AMB):** AMBs serve as simple cross-chain messaging protocols. These messages can range in specificity and might include oracle data, hashes, addresses.
- **Liquidity Bridges:** Liquidity bridges typically have cross-chain liquidity pools, whereby a user deposits an asset into the liquidity pool on the origin chain and a confirmation message passed through the bridge allows them to withdraw from a liquidity pool on destination chain.
- **Token Bridges:** Token bridges allow users to either lock or burn an asset on the origin chain and then mint the asset on the destination chain.

Note that both liquidity bridges and token bridges incorporate an AMB within their design.

<br>

## The significance of bridges

The open, collaborative, and composable nature of web3 is one of the most appealing attributes of blockchain for developers. Before the existence of bridge protocols, each blockchain was inhibited by the liquidity and applications within its own domain. The introduction of bridges has provided an avenue for enabling data exchange, smart contracts, token transfer, and instructions between two independent protocols.

1. **Capital Efficiency:** Users can obtain the same utility by using a dApp on a cheaper blockchain. Assets are briged from an expensive to a cheap blockchain and transacted upon there at lower network fees. This opens the door applications like gaming and mass market applications. A simple example of this efficiency is a user can bridge their token from Ethereum (Layer 1) to Polygon (Layer 2) to enjoy much lower transaction fees.
2. **Cross-chain bonding:** Bridges enable users to transfer digital assets from a blockchain that holds significant value, but few dapps of its own, such as Bitcoin, to one that has a developed DeFi ecosystem, like Ethereum, and is in need for additional liquidity.
3. **Interoperability:** Bridges give users the ability to have freedom when it comes to the DeFi applications they can interact, opening up possibilities beyond the Native chain. For example, let's say there is ABC project on Arbitrum that offers 15% APY on DAI in a liquidity pool, but the user has assets on the Ethereum chain. In this scenario, the user can bridge their Ethereum tokens to Arbitrum and access this yield. The user can then bridge the LP tokens back to Ethereum and borrow against these on a lending platform, like Aave.
4. **Composability**: atomic multi-chain transactions have not evolved yet, but it is expected that they will via the emergence of super builder entities to validate multiple chains to capture inter-chain MEV. This means users can submit a transaction batch that touches multiple chain. Example: a rollup sequencer accepts a batch of 3 transations from a user: (1) a withdraw tx from an Ethereum-tethered rollup a chain (2) a deposit to Polygon's deposit contract on Ethereum (3) a swap tx on Polygon. 
5. **Scalability:** Bridges are designed to withstand high transaction volumes, which enables greater scalability without forcing developers and users to give up the liquidity and network effect of the original chains.
   <br>

## How does a bridge operate?

**Sending assets across chains:**
Here is an example where Stefan wants to transfer 1000 USDC from Ethereum Network to Polygon Network.

![](https://i.imgur.com/3IBF1Is.png)

1. Stefan deposits 1000 USDC to the bridge contract on Ethereum.
2. The USDC token is then locked in this bridge contract.
3. An external party, such as an incentivised relayer or a validator, posts a proof of said deposit to the bridge contract on Polygon.
4. The bridge contract on Polygon verifies that the proof (which is typically a Merkle path) is valid and is included in the latest finalized block of the Ethereum blockchain (note: if Ethereum were still running a PoW consensus, the validation would involve checking that the deposit tx on Ethereum is buried under enough number of blocks, similar to how CEXes wait a number of blocks before considering a deposit into a PoW chain confirmed).
5. The bridge contract on Polygon Network mints 1000 USDC on Polygon and transfers the tokens to Stefan's Polygon address.

**Withdrawing assets back to the Original chain:**
Now suddenly, Stefan wakes up one day and would like to withdraw the 1000 USDC back to Ethereum Network.

![](https://i.imgur.com/G4ugxef.jpg)

1. Stefan sends 1000 USDC to the bridge contract in Polygon Network.
2. The bridge burns the tokens.
3. An external party, such as an incentivised relayer or a validator, posts a proof of said deposit to the bridge contract on Ethereum.
4. The the contract validates the proof and unlocks the USDC tokens, which were at first deposited by Stefan, and finally transfers the tokens back to Stefan’s address on Ethereum.

<br>

## Common vulnerabilities of bridges

The following are examples from published Spearbit Audit reports which can be found [here](https://github.com/spearbit/portfolio).

### Message Verification

Cross-chain bridges perform validation of a deposit or withdrawal before actually performing any transfers. There have been many instances in the past where lack of proper validation of signature or dispatching leads to millions of dollars in loses.

Here is an example of an identified vulnerability regarding verification when it comes to dispatching of messages:

In this instance, upon calling _xcall()_, a message is dispatched. A hash of this message is inserted into the merkle tree and the new root will be added at the end of the queue. Whenever the updater of the contract commits to a new root, _improperUpdate()_ will check that the new update is not fraudulent. In doing so, it must iterate through the queue of merkle roots to find the correct committed root. Because anyone can dispatch a message and insert a new root into the queue it is possible to impact the availability of the protocol by preventing honest messages from being included in the updated root.

```solidity
function improperUpdate (..., bytes32 _newRoot, ...) public notFailed returns (bool) {
   // if the _newRoot is not currently contained in the queue, 
   // slash the Updater and set the contract to FAILED state 
   if (!queue.contains(_newRoot)) {
      _fail();
      ...
   }
   ...
}

function contains (Queue storage _q, bytes32 _item) internal view returns (bool) {
   for (uint256 i = _q.first; i <= _q.last; i++) {
      if (_q.queue[i] == _item) {
         return true;
      }
   }
   return false;
}
```

### Price Oracle Manipulation

Bridges typically receive asset pricing data from third-party oracles. Oracles act as a data source that can be fed into a smart contract, one that enables them to access real-time data that isn’t on-chain yet, which is most often the accurate price of assets. This presents the risk that such price feed can be manipulated and the wrong price is input into a smart contract, falsely reducing or increasing the price of an asset, creating an arbitrage opportunity for the manipulator. The effect of such a manipulation gets further amplified when attackers implement flash loans to exploit the arbitrage opportunity created by the price manipulation.

Here is an example of an identified vulnerability regarding the importance of price checks being actively updated when smart contracts interact with oracles:

The function directly interacting with a series of oracles within the smart contract, at each step, it has a few validations to avoid incorrect price. If such validations succeed, the function returns the non-zero oracle price. For the Chainlink oracle, _getTokenPrice()_ ultimately calls _getPriceFromChainlink()_ which has the following validation — _updateAt_ refers to the timestamp of the round. This value isn’t checked to make sure it is recent.

Additionally, it is important to be aware of the _minAnswer_ and _maxAnswer_ of the Chainlink oracle, these values are not allowed to be reached or surpassed. See [Chainlink API](https://docs.chain.link/data-feeds/price-feeds/api-reference/) reference for documentation on minAnswer and _maxAnswer_ as well as this piece of code: [OffchainAggregator.sol](https://github.com/smartcontractkit/libocr/blob/master/contract/OffchainAggregator.sol#L57)

```solidity
if (answer == 0 || answeredInRound < roundId || updateAt == 0) { 
   // answeredInRound > roundId ===> ChainLink Error: Stale price 
   // updatedAt = 0 ===> ChainLink Error: Round not complete
   return 0;
}
```

### Access Controls

It is important to have access control validations on critical functions that execute actions like modifying the owner, transfer of funds and tokens, pausing and unpausing the contracts, etc.

Here is an example of an identified vulnerability regarding the importance of access controls, even slight modifications to mechanisms can have unprecedented impact:

There are several access control mechanisms. If they somehow would allow _address(self)_ then risks would increase as there are several ways to call arbitrary functions.

```solidity
library LibAccess {
   function addAccess (bytes4 selector, address executor) internal {
      ...
      accStor.execAccess[selector][executor] = true;
   }
}

contract AccessManagerFacet {
   function setCanExecute(...) ... {
   ) external {
      ...
      _canExecute? LibAccess.addAccess(_selector, _executor) : LibAccess.removeAccess(_selector, _executor);
}

contract DexManagerFacet {
   function addDex (address _dex) external {
      ...
      dexAllowlist [_dex] = true;
      ...
   }

   function batchAddDex (address [] calldata _dexs) external {
      ...
         dexAllowlist [_dexs[i]] = true;
      ...
   }
}
```

### Other Notable Vulnerabilities

- Fake Events
- Validator Takeover
- Admin Private Key Leak

<br>

## Spearbit's Bridge Clients

### Connext

Connext is a crosschain liquidity network that enables fast, fully-noncustodial transfers between EVM-compatible chains and L2 systems. It leverages the Ethereum blockchain along with groundbreaking distributed systems tech to enable instant, near-free transfers anywhere in the world.

- [June 2022: Link to Audit Report](https://github.com/spearbit/portfolio/blob/master/pdfs/Connext-Spearbit-Security-Review.pdf)
- [Watch our conversation with Arjun Bhuptani (CEO & Founder) Understanding Bridge Security](https:/https://www.youtube.com/watch?v=gQzRpU6GYhc/)

### Li . Fi

LI.FI is a cross-chain bridge aggregation protocol that supports any-2-any swaps by aggregating bridges and
connecting them to DEX aggregators.

- [August 2022: Link to Audit Report](https://github.com/spearbit/portfolio/blob/master/pdfs/LIFI-Spearbit-Security-Review.pdf)

### Other Notable Bridges

- Hop
- LayerZero
- Across

<br>

## How are we helping secure these bridges?

Spearbit has assembled a community of Security Researchers with the specific domain expertise needed to understand the technical complexity of bridge protocols. Security reviews are only a partial solution when it comes to securing bridges. By fostering the conversation through seminars, twitter spaces, and community discussion, Spearbit aims to serve as a backbone for developers in this complex ecosystem.

If you are a developer or security researcher looking to dive deeper into bridging protocols we invite you to checkout and help us improve our [Bridge Security Checklist](https://github.com/spearbit/portfolio/blob/master/content/bridges/BridgeSecurityChecklist.md).

---

<br>
