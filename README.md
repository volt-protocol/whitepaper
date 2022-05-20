# ⚡VOLT⚡

VOLT is a [stablecoin](#stablecoin) that tracks the price of $1 in December 2021.

Persistent [inflation](#inflation) erodes savings and makes it difficult to plan ahead financially. By holding VOLT, users protect their purchasing power over time without taking on exposure to volatile assets.

Volt Protocol is a system of [smart contracts](#smart-contracts) that allows users to [mint VOLT](#minting) using stablecoins like FEI or USDC, or [redeem VOLT](#redemption) for its face value. There is a fee on minting, and no fee on redemption. There are caps on VOLT minting to protect the system, no limitations on redemption.

VOLT is a reserves backed stablecoin, meaning that each $1 of VOLT is backed by more than $1 in [protocol controlled value](#protocol-controlled-value). The reserves target is a 20% buffer above the circulating VOLT supply, allowing the system to endure losses in a yield venue or periods in which yields are lower than the inflation rate.

![US dollar purchasing power over time](dollar.jpg)

There are many ways to measure inflation. Volt Protocol uses the CPI-U or [Consumer Price Index for Urban Consumers](#cpi-u) as the source of truth for inflation, one of the most widely known and used inflation metricts.

Volt Protocol uses a [Chainlink oracle](#chainlink) to bring CPI-U data onto Ethereum in a reliable and trust-minimized way. A Chainlink node reports the new inflation data each month to the Volt system.

Volt Protocol was incubated by the TRIBE DAO and enjoys an ongoing deep partnership. There is a planned [token swap](https://snapshot.org/#/fei.eth/proposal/0x33c689b49e000f7380fc16b7c366d37f9088db5427d544e073f245e4b48bb243) and [active cooperation](https://snapshot.org/#/fei.eth/proposal/0x05054792882cf1f7fe9972bc9125df9f0359a277ad15f7f0c8ce585c553b0535) on PCV strategy.

The future governance token of Volt Protocol is known as VCON, or the Volt Controller. The role of the VCON token is to direct the protocol's assets among yield venues to outperform inflation without taking on excess risk. Plans for VCON launch are tentative as the Volt [Market Governance system](#market-governance) is under active design.

Volt Protocol takes a rigorous approach to security. Throughout the development process, the Volt codebase received extensive review within the Tribe DAO. Prior to launch there was an [audit by Zellic](https://github.com/volt-protocol/volt-protocol-core/blob/develop/audits/Volt%20Protocol%20-%20Zellic%20Audit%20Report.pdf) and a [Code4rena public audit](https://code4rena.com/contests/2022-03-volt-protocol-contest). Post launch, the team engages in regular joint security reviews with partners like Porter Finance. There is a bug bounty program [on Immunefi](https://immunefi.com/bounty/voltprotocol/) offering up to 250k in rewards.

During the launch period, the Volt system is governed by a multisig consisting of Volt cofounders Kirk Hutchison and Elliot Friedman, as well as Joey Santoro and Tom Waite from Fei Protocol. Volt Protocol will follow [progressive decentralization](#progressive-decentralization), adding checks and balances over time.

VOLT is currently available only on Ethereum. Soon, the system will be deployed to [rollups](#rollups) like Optimism and Arbitrum.

# Disclaimers
VOLT is an experimental open system, all participants should be aware that there are substantial risks involved when interacting with any new smart contract product or economic system. The developers of VOLT, whether individuals or entities, do not make any guarantees about returns on the VCON token. VOLT borrowers are responsible for their own risk and should be careful with leverage. Any desirable properties of VOLT are expressed in the smart contracts and are not a guarantee offered by any individual or corporation.

All code used in the VOLT system will be audited and open source, and the system’s state will be observable on chain. Please do your own research and enjoy responsibly.

# Glossary and Definitions
### Stablecoin
A stablecoin is a cryptocurrency whose value is *pegged* to that of another asset, in other words a synthetic asset or derivative. The value of stablecoins is backed either by assets in the traditional finance system (as in the case of USDC or USDT), by cryptoassets as in the case of LUSD, or a combination of both like DAI.

### Smart Contracts
Smart contracts refer to code deployed on a blockchain network. In the case of Volt Protocol, the system contracts are written in Solidity and deployed on Ethereum mainnet. In the future, they will also exist on rollups like Arbitrum. The source code for the Volt Protocol contracts is open source and can be found [on Github](https://github.com/volt-protocol/volt-protocol-core).

### Inflation
Inflation refers to the loss in purchasing power of a currency over time. While in the short term prices can fluctuate for many reasons, such as shortages of a given good or service, the long term decline in purchasing power is due to expansion of the currency supply. Central banks print money to help governments accomplish their goals, a stealth tax on savers.

### Protocol Controlled Calue
Commonly abbreviated as PCV, refers to cryptoassets deposited into the protocol and redeemable for VOLT (or whichever system is being referred to, FEI also has protocol controlled value). Distinct from the overcollateralized user deposited ETH that backs DAI or LUSD, which is not directly redeemable.

### CPI-U
The CPI-U is published monthly by the United States Bureau of Labor Statistics (BLS). It tracks the prices of goods and services in relation to the starting index price for those same items in 1982. VOLT uses the CPI-U as its inflation benchmark because it most accurately reflects the rising costs consumers pay due to inflation in the real economy and is one of the most widely used and understood measurements of inflation. One common critique of the CPI-U is that it isn’t weighted heavily enough towards housing, which represents a large and growing portion of expenses for most people in the United States. It's important to remember that the price of a single good can increase faster than others for reasons other than inflation.

### Chainlink
Chainlink is a network of node operators that reports real world data onto blockchains like Ethereum. Chainlink was chosen because it is the most well established and widely adopted oracle provider. There are safeguards in place such that if an oracle made a malicious report, like 10000x inflation in a single month, it will be rejected by the Volt smart contracts. If an oracle fails to report, the old inflation rate will remain in place. Safe failure modes are a key part of Volt design.

### Minting
VOLT can be minted using one of the supported assets like FEI or USDC. There is a limit on the total amount of VOLT, once this limit is reached no more VOLT can be minted through the PSM until redemptions occur. There are also limits on how much can be minted against a given asset like FEI. These limits can be raised by governance. There is a fee of 0.5% to mint VOLT which may be adjusted by governance.

### Redemption
VOLT can be redeemed through the PSM at any time with zero slippage, zero fees. Some PSMs are mint only, such as the USDC PSM, so redemption and minting may not always be available in the same asset. There is more than $2 of FEI in the PSM for every $1 of circulating user VOLT, so the entire supply could be redeemed at any time safely leaving room to spare.

### Market Governance
The goal of the VCON governance system is to allow for fully liquid, block-by-block allocation of PCV among yield venues and adjustment of debt ceiling between issuance pools. This means that allocation of Volt's PCV must be a **market outcome**, not a governance decision. It is essential to let tokenholders directly allocate protocol assets while having the proper skin in the game. They must benefit or suffer most from their individual governance decisions, not the result of majority consensus or the efforts of others.

While this system is under active design, here is an illustration of how it might work and the principles involved:

Imagine a special borrowing pool like Aave, Compound, or Fuse, in which VCON can be used as collateral and the protocol's PCV is deposited. VCON holders will be able to borrow VOLT or other protocol assets subject to a set of special terms and constraints:

* use a system price for VCON instead of market price so holders can borrow their pro rata share of assets
* the pool will have only forced liquidations (see ["gib"](https://github.com/fei-protocol/tribe-turbo)), as the deposited VCON is used for governance rather than to "back" the assets in the pool
* if borrowing VOLT, pay small premium as "reserves fee"
* if borrowing other stablecoins, pay inflation rate plus "reserves fee"
* borrowers can only deposit assets into authorized yield venues, not just walk away with them
* offer incentives on borrowing in this pool so active VCON holders dilute passive holders, and to ensure utilization remains high

Note that in this model VCON holders directly access a cash flow based on their independent risk management. What they can earn allocating VOLT or PCV stablecoins in excess of inflation and the reserves fee, they keep. The long term goal of the VCON DAO is to remove as many arbitrary, vote-based actions from the system as possible. To build the most scalable and powerful system possible, we must rely on markets rather than governance.

### Progressive Decentralization
Given the high complexity of building a decentralized financial application, many small changes and course corrections are required. In the early period the presence of Fei Protocol engineers on the multisig ensures that the Volt team does not have unilateral control over the system. In the future, all changes will be subjected to a timelock and vetoable by Guardian roles. Once the VCON governance token is live, holders will also have veto power over system changes.

### Rollups
A rollup or "layer two" is a scaling solution allowing users of Ethereum to send transactions at lower cost. On Ethereum mainnet, it costs several dollars each time a user transfers tokens, and more complex operations cost more. Rollups allow this cost to be reduced dramatically while maintaining the security benefits of Ethereum itself. Examples of rollups include Arbitrum and Optimism. [Read here to learn more about the technical details of rollups and Ethereum scaling](https://ethereum.org/en/developers/docs/scaling/).
