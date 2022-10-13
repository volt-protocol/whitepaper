# ⚡VOLT⚡
Volt Protocol is a decentralized savings and credit system built on Ethereum. VOLT is the USD-denominated yield bearing stablecoin.

The central premise of VOLT is that system parameters should be governed by market forces rather than votes wherever possible, and protected by checks and balances otherwise.

VOLT holders are lenders who earn the "VOLT rate", VCON holders are allocators who provide first loss capital and obtain leveraged exposure to yields.

# Contents
<details>
<summary></summary>

[Motivation](#motivation)

[Yield Bearing Digital Cash](#yield-bearing-digital-cash)

[Current Implementations and their Problems](#current-implementations-and-their-problems)

[Market Governance Overview](#market-governance-overview)

[Accounting](#pcv-accounting)
* [Reserves](#reserves)
* [Surplus](#system-surplus)
* [Oracles](#accounting-oracles)
    * [TWAPCV](#twapcv)
    * [TWASB](#twasb)

* [Supply](#volt-supply)
    * [Maximum Supply](#maximum-supply)
    * [Total Supply](#total-supply)
    * [Supply Changes](#supply-changes)
  
[Minting and Redemption](#minting-and-redemptions)
* [Rate Limits](#rate-limits)

[System Rates](#system-rates)
* [The VOLT Rate](#the-volt-rate)
* [VCON Revenue Split](#vcon-revenue-split)
  
[Market Governance](#market-governance)
* [VCON Voting](#vcon-voting-power)
* [VCON Delegation](#vcon-delegation)
* [VCON Liquidation](#vcon-holder-liquidation)

[Yield Venues](#yield-venues)
* [Venue Types](#venue-types)
  * [Frictionless](#frictionless-venues)
  * [Liquid](#liquid-venues)
  * [Illiquid](#illiquid-venues)
* [Venue Onboarding](#venue-onboarding)
* [Venue Risk Management](#venue-risk-management)
  * [PCV Guard](#pcv-guard)
  * [Bad Debt Sentinel](#bad-debt-sentinel)
* [Base Venue](#base-yield-venue)
* [Internal Venue](#internal-yield-venues)

[Market Module](#market-module)

[Checks and Balances](#checks-and-balances)
* [VOLT Veto Module](#volt-veto-module)
* [VCON Veto Module](#vcon-veto-module)
* [Timelocks](#timelocks)

[Consensus Parameters](#consensus-parameters)
* [Target Surplus Ratio](#target-surplus-buffer-ratio)
* [Target VCON Participation](#target-vcon-participation)
* [Surplus Buffer Deviation Sensitivity](#surplus-buffer-deviation-sensitivity)
* [VCON Participation Deviation Sensitivity](#vcon-utilization-deviation-sensitivity)
* [VCON Auction Parameters](#auction-parameters)
* [PSM Allocations](#psm-allocations)

[VCON Economics](#vcon-economics)
* [VCON Auction](#vcon-auction)
* [Ragequit](#ragequit)

[vETH](#veth)

[MVP Constraints](#MVP-constraints)

[Cross Chain](#cross-chain)
* [Liquidity Management](#liquidity-management)
* [Rate and Price Uniformity](#rate-and-price-uniformity)

</details>

---------------

# Motivation
Banks accept deposits of currency and use them to make loans of varied durations and kinds. Banks offer yield on deposits which is lower than that they earn lending, and profit by the spread. A bank's business is to ensure a good match between the time and yield preferences of its depositors and the composition of its loan book. When they fail to do so, a bank run is the natural result.

There are three main problems with banks and banklike systems today --
* they require substantial trust in the operators of the bank (we saw the problems of this in the collapse of Celsius)
* crippling overregulation degrades the user experience, effectively taxing all savers
* they are vulnerable to censorship or seizure of funds

These problems are closely related. The solution is a trust-minimized system implemented in smart contracts, which can simultaneously obviate the need for trusted human operators, eliminate central points of failure that are vulnerable to censorship, and be trusted by the public thanks to rules encoded more reliably than any legal proction could be.

# Yield Bearing Digital Cash
The related concepts of cash, banknotes, checkable deposits, and money market accounts are distinct by a combination of real past technical limitations, legal whim, and historical accident. There is no deep logical reason why notes that are transferrable peer to peer freely cannot also earn interest. There were very real limitations on peer to peer checking in the past, as both parties needed their own banking intermediary prior to the advent of cryptocurrency. There is no good reason that a separation should be enforced between checking and savings accounts today.

Smart contracts make it possible to combine all these functions into one whole -- an electronic note that is redeemable on demand, seizure resistant, interest bearing, and freely transferrable between individuals across any jurisdictions. And what's more, composable into any kind of other financial contract or instrument without requiring permission from the issuer.

# Current Implementations and their Problems
We must recongize that stablecoin issuers are banklike. This is not any kind of regulatory definition but a practical one. If he could be revived and made to understand the nuances of the blockchain and electronic money transfers, [Amschel Rothschild](https://en.wikipedia.org/wiki/Mayer_Amschel_Rothschild) would surely recognize MakerDAO as being in his own line of work. The point of smart contract systems built on a neutral consensus and execution layer is to remove the elements of human trust that were so crucial to private bankers in the past and that make today's banking system vulnerable to censorship and corruption. However, the economics of good banking remain the same, as do the general needs of users.

## Centralized stablecoins
Equivalent to a checking account that requires only an Ethereum address to use, centralized stablecoins are an excellent step forward but suffer many of the same issues as the traditional financial system. It is difficult for the average user to know the full details of their risk management, and impossible to prevent unwanted changes or outright censorship.

Centralized stablecoins may offer yield products to KYC'd holders but not to the majority, effectively taxing all users and rewarding institutional or more sophisticated clients who onboard to the yield product.

## Decentralized stablecoins
True decentralized stablecoins such as DAI ensure users do not have to fear being individually targeted for censorship. Unexpected changes in the system risk are more difficult to perform than within a corporate issuer of a centralized stablecoin, and impossible to accomplish in secret. Stablecoin holders cannot prevent unwanted system changes, only exit.

Decentralized stablecoins like DAI offer very little yield to users. Those that do generally rely on unsustainable token emissions schemes.

## Lending markets
To earn yield, centralized and decentralized stablecoins alike are deposited into lending markets like Compound. This allows users to access yield but forces them to take on liquidity risks. Also, the rates on these markets are volatile, so given a set of a few acceptable markets a user is not assured of getting the best rates without frequent rebalancing. This favors large users or collective management of funds. The same is true of emissions-based models whether they incentivize LP pairs or direct staking.

## Yield vaults
Yield vaults or aggregators seek to solve the problems small accounts face when using lending markets or other yield venues directly. However, current implementations have no mechanism to be credibly neutral at scale in terms of which venues are onboarded, and how much of the system capital is deposited into a given market at a given time. The incentive structure of yield aggregators encourages them to seek out the maximum possible yield irrespective of risk. Furthermore, users are still forced to choose a specific stablecoin denomination to deposit, when really all they want is a consistent risk-return on their dollars.

Both lending markets and yield vaults today are highly vulnerable to bank runs in the event of losses, since there is no mechanism to fairly socialize losses among vault depositors in the event of bad debt occuring in the market. Sophisticated and responsive actors are the least likely to use, passive savers the most likely to take losses.

# Market Governance Overview
Market governance seeks to create a crypto economic incentive aligned system balancing supply and demand between capital suppliers and capital allocators. Users supply capital in the form of VOLT, and the protocol adjusts the VOLT rate to manage liquidity and reach an efficient market equilibrium.

VCON holders make risk decisions through liquid allocation of capital and are exposed to the associated risk and return. Allocation into venues that suffer losses or underperform versus the VOLT rate for a sustained period of time will be subject to liquidation. Profits are split between the system surplus and the VCON allocators based on utilization.

VOLT supply expansion and contraction is regulated by interest rate curves (similar to a Compound style market with a utilization curve) and a controller that adjusts this curve to find a market rate. The system will algorithmically determine the yield paid to VOLT holders to guide the system toward a target surplus buffer ratio decided by governance.

Market governance is a control system, a set of feedback loops that connect the VOLT holders who are “lenders” to the system, and the VCON holders who are the “borrowers”. The following sections describe in detail how the system calculates interest rates, accounts for yield, and handles liquidations.

In addition to market governance, checks and balances will be put in place throughout the Volt Protocol system to make sure all VOLT holders have the same protections even in the event of losses, and ensure they can defend their rights against unwanted system changes.

# PCV Accounting
The entirety of capital within the VOLT system is referred to as “protocol controlled value” or PCV. The PCV is broken down into two buckets.

## Reserves
Reserves are used either to provide liquidity for VOLT holders seeking to exit (funds inside a custodial Peg Stability Module), or simply held idle for future use along these lines.

The target value for the reserves will likely be determined by governance in an early version of the system, and later dynamically based on the surplus buffer ratio, the size of the surplus buffer vs the size of the PCV as a whole. When the surplus buffer is too small, more PCV should be held idle to prevent unsafe amounts of leverage being granted to VCON holders, which causes incentive alignment to break down.

The remainder of protocol assets outside the reserves can be controlled by VCON market governance to earn yield. VOLT holders can also redeem directly against liquid non-reserve assets so long as the [daily redemption limit](#rate-limits) has not been exceeded. When the capital in the PSMs exceeds the target reserves level, it becomes eligible for allocation by VCON holders. Likewise, when the reserves are depleted, a rebalance can be triggered to move capital out of the market governance venues.

## System Surplus
Sometimes called the “surplus buffer”, this is defined as the total PCV minus the value of all circulating VOLT. The system surplus is not a separate bucket or specifically distributed in either the reserves or the capital allocated by market governance. It is the “equity” of the protocol, originating from VCON holders who add capital to the system in exchange for VCON tokens, or accrued from system fees.

The system surplus is significant in determining certain rates within the system. When the surplus is large vs the total PCV, a higher rate is paid to VOLT holders, and vice versa. Likewise, when the surplus is large vs the total PCV, the size of the reserves can shrink, and vice versa.

## Accounting Oracles
There are two significant values tracked in system memory relating to the PCV, a "Time Weighted Average PCV" and a "Time Weighted Average Surplus Buffer". These values are used in calculations to decide the VOLT rate, which are described under [Interest Rates](#interest-rates).

### TWAPCV

TWAPCV is calculated as

![](TWAPCV.png)

where:
* `t` equals a snapshot of the total PCV at a point in time using manipulation resistant balances
* `n` equals the total number of periods to measure across
* `i` is the period’s index, which starts at 1

### TWASB
Trailing Weighted Average Surplus Buffer (TWASB) can be calculated with the same formula formula,

![](TWASB.png)

where:

* `t` equals a snapshot of the total surplus buffer at a point in time using manipulation resistant balances
* `n` equals the total number of periods to measure across
* `i` is the period’s index, which starts at 1.

## VOLT Supply

### Maximum Supply
The maximum VOLT supply is based on the size of the surplus. The minimum surplus is defined by governance, the starting value will be 5%. With a surplus of 1m VOLT, there might be at most 20m VOLT in circulation. According to the VOLT rate calculation, more of the yield earned will be retained in the system when the surplus buffer is close to the minimum, and more given to VOLT holders when it is high.

### Total Supply
The VOLT Rate is based in part on the [time weighted average of the protocol controlled value](#twapcv), which depends on snapshots at multiple points in time. It is difficult to account for the amount of circulating VOLT on L2 networks such as Arbitrum without regular oracle reporting from L2 to L1. To resolve this difficulty, pessimistic accounting is used, where all the VOLT potentially mintable on L2 in peg stability modules is considered to be circulating. As a result, the only VOLT tokens that are excluded from the VOLT rate calculation are those uncirculating VOLT held in mainnet PSMs.

### Supply Changes
When a user mints VOLT, the idle stablecoin balance in the system will go above the target reserve level, and vice versa for redemption. Any stablecoins in the PSM in excess of the reserves target are eligible to be converted into MGV and deployed via market governance. Rebalance need not occur on every mint or redeem, and instead can be incentivized and automated to occur only when the reserves exceed or drop below their target value by a certain threshold. The early system will likely use a static reserve target. A dynamic fractional reserve calculation will improve efficiency.

# Minting and Redemptions
A VOLT holder can mint VOLT by depositing stablecoins or redeem for the same in the liquid buffer for no fee. There is a daily limit on how much VOLT can be withdrawn through the PSM. So long as this limit hasn't been reached, any user can refill the PSM by withdrawing funds from a yield venue. As in any interaction with a PCV Deposit, the venue's accounting is updated. Refilling the PSM will be done automatically by a Defender autotask, and can also be triggered permissionlessly by users.

## Rate Limits

A limit is necessary on redemption through the PSM to ensure that losses in yield venues can be properly accounted for. If Volt Protocol took a loss in one of the PCV yield venues that exceeded the size of the surplus buffer, which would not be instantly knowable on chain (sentinels must report existence of bad debt positions to take action), sophisticated users could exit at the old, full target price. This occurs at the expense of less sophisticated users and is undesirable. A reasonable global rate limit on VOLT redemptions would allow all but the largest institutional users (ie, an exchange or major fund) to exit atomically, and keep the delay for these larger users down to a reasonable time for the loss liquidation auction or other mechanism to mark down the value of bad debt. This limit should never be set so low as to inconvenience users during healthy operation and can be adjusted based on market data. We can envision a market dynamic approach where when redemptions occur, the rate limit increases, and then decays when there is low redemption demand.

# System Rates
The following sections will describe the different interest rates and curves within the VOLT system, how they interact with each other, and how they are derived. Under the final system, rates should be purely market based, including system memory and a controller. In the MVP, the system will resemble Compound with utilization based curves and an equilibrium value determined by governance.

## The VOLT Rate

The baserate is defined as the VOLT rate when all system ratios are at target (surplus buffer to PCV ratio and liquid reserves ratios). Governance must determine these target values, as well as the baseline rate. For example, governance might determine that the optimal surplus buffer ratio is 10%, the optimal liquid reserves 50%, and a baserate of 2.5%.

If the surplus buffer and liquid reserves are at target, the VOLT rate will be equal to the baserate, 2.5%.

If more VOLT were minted such that the surplus buffer ratio reduced to 5%, the VOLT rate might reduce to 1.25%. If redemptions occurred such that the surplus buffer ratio was 20%, the VOLT rate might increase to 5%.

The baserate should be set based on the actual yield available in the integrated venues and observed market behavior. If the system remains too long above or below target surplus and reserves ratios, the baserate can be adjusted. At first this process will be manual, in a future version it can be turned over to a controller.

# Market Governance Fee Split

Interest is accounted for whenever protocol assets enter or exit a yield venue, such as when a VCON holder takes profits, or a VOLT holder redeems or mints. All yield is initially split into two – a portion that accrues to the VCON holders allocating to that particular venue, and a portion that goes to the VOLT holders by being directed to the surplus buffer.

## VCON Revenue Split

The profit split between VCON holders and the surplus buffer (setting aside the portion going directly to VOLT holders through the current rate of price appreciation) is adjusted based on the utilization of market governance. If most VCON is participating in market governance, the VCON holders will receive a low share of the returns, as this likely means they are obtaining high leverage and a healthy profit. If little VCON is participating, that which does will get to keep most of the returns it generates above the base venue rate. During longer periods of high utilization, the base profit split will adjust down, and vice versa.

The profit split for VCON holders is calculated according to a similar equation as the VOLT rate, whose terms are described below.

`p = y * n * (u / t) ^ k`
`y` is a VCON holder’s profit
`y` is the actual yield generated in the venue in excess of the VOLT rate
`n`  is the base fee split ratio when VCON is at target utilization, adjusted up or down by the controller. `0=<n<= 1` as VCON holders must earn between 0% and 100% of profits.
`u` is the time weighted average VCON utilization
`t` is the target VCON utilization, a system constant set by governance
`k` is the sensitivity of the system to short term deviations from the target market governance participation rate. A `k` of zero means that VCON holders receive a constant fee share, a `k` of 1 means that the yield VCON holders receive is 1:1 directly proportional to the level of deviation from the target participation rate, and a `k` of 1000 means the system is extremely sensitive to changes in utilization. 

# Market Governance

## VCON Voting Power
VCON holders can direct the protocol assets excluding the [reserves](#reserves), with the right to do so divided evenly among the portion of VCON staked in market governance. VCON holders who do not participate in market governance implicitly delegate their voting power to those who do. Therefore, there is no explicit restriction on the share of the MGV a given VCON holder can borrow, so long as that MGV is currently idle. If only one VCON holder participates in market governance, they can direct the entirety of the MGV.

VCON holders can always rebalance among themselves to an equal pro rata share of MGV. If a VCON holder is currently borrowing more than their pro rata share, any other VCON holder with less than their pro rata share can trigger a rebalance to their own benefit. If one holder was directing the entire MGV, another holder who stakes an equal VCON share could trigger a rebalance to gain control over half of the MGV. There may be a nominal fee on the rebalance to prevent griefing.

Protocol assets can only be deposited into whitelisted [yield venues](#yield-venues). A VCON holder can direct MGV to a whitelisted yield venue, subject to its own liquidation rules. 

A VCON holder can modify their position by withdrawing funds from their chosen venue and moving them into a new whitelisted venue. During this withdrawal, checks are performed to ensure that the yield venue did not suffer a loss, and fail if not enough PCV was withdrawn from the underlying venue. When a market governance participant withdraws from or deposits into a venue, a rebalance is triggered if either the origin or target venue has an incorrect PCV share due to redemptions (see section below).

## VCON Delegation
VCON holders can delegate their market governance power to other accounts. To be eligible as a delegate, an account must opt in. As part of this, the new delegate can set their desired fee split, and choose whether or not to enforce a delegator whitelist. A VCON delegator can choose to remove their delegation at any time, converting back into a standard VCON position which can be repaid and closed out if the underlying funds were in a liquid venue.

## VCON Holder Liquidation
If PCV is illiquid (or in the case that losses were taken in a yield venue), either a Guardian or, in the future VOLT/VCON bonder can trigger a “margin call” or “gib” on VCON holders. If they do not return the loan within a predetermined period of time, their VCON is burnt and the system surplus absorbs any losses. Since it may not be possible to immediately and trustlessly tabulate losses on chain, the loss can initially be marked down to zero and liquidation can proceed through another mechanism.

There is no partial liquidation for VCON holders – if the loss in the underlying venue is less than the value of their VCON, it is their responsibility to restore the PCV.

After the “gib”, seized collateral is transferred to a venue where governance can decide how to value or dispose of it, generally either by marking down and reuniting with the main PCV if liquid, holding to maturity if needed, or in some cases perhaps auctioning off. Accounting problems in the event of venue losses are the most likely situation that would need intervention.

# Yield Venues
Each venue for market governance has its own accounting and liquidation rules. Whenever a VCON holder allocates into or out of a venue, the cumulative yield must be calculated. A per venue basis instead of a per VCON holder basis keeps things organized. Liquid venues like Aave will allow for direct redemption by VOLT holders as described above. Some venues may be less liquid than others and have longer to meet “margin calls”. For example, a real world asset loan might have thirty days to repay when called.

If one venue has more than its share of PCV based on market governance allocation due to VOLT holders redeeming, anyone can trigger a rebalance between this and another venue or venue(s), “paying off the debt” of VCON holders in the oversaturated venue. In the case of an illiquid venue, another VCON holder can assume ownership of the debt even if the underlying collateral asset cannot be liquidated.

All Yield venues will have the following set of parameters set on instantiation and changeable by governance. The margin call period is the length of time VCON holders are given to repay their debts if a liquidation is triggered. The liquidity profile of a venue will be attached to the venue so that the system can be aware of the liquidity profile of its PCV through a Collateralization Oracle. Yield oracles will be instantiated for all yield venues and will track the performance of the venue over a period of time.

## Venue Types

Yield venues can be broken down into three types based on their liquidity characteristics.

### Frictionless venues
This includes any venue that is usually liquid with no cost to enter or exit, such as Compound v2 or other lending markets that accept deposits of PCV stablecoins like DAI or USDC.

### Liquid venues
This includes any venue that is usually liquid, but cannot be entered or exited on demand without slippage, or otherwise face some possibility of principle losses. This includes any stablecoin with a volatile price, like LUSD, that can still be reliably liquidated on demand even if there is some fee or variance in the return.

### Illiquid venues
In the future Volt Protocol will be extended to support assets or strategies with a range of liquidity profiles, including real world assets and fixed-duration lending. These venues may have more complicated rules for liquidation or pricing of collateral, and may have additional restrictions such as deposit caps. This may include assets that can only be effectively liquidated with some delay.

## Venue Onboarding
When a new yield venue is onboarded by either the core contributor team in the early period, or later by VCON vote, it will have a deposit ceiling that starts at 0 and increments toward some limit, depending on venue type, of the PCV over a significant period of time. This allows for VOLT or VCON holders to act to blacklist a malicious or dangerous venue even after it has been onboarded.

Integration with external yield venues is the greatest source of risk for the protocol. The Volt Protocol community must develop a healthy immune system that rejects dangerous changes and unwise PCV deployments. Allowing multiple opportunities for rejecting new venues and deprecating those revealed to be dangerous will help to keep VOLT safe for the long term.

For the early venues, the core contributor team and paid external auditors will conduct reviews of all integrated venues, as well as any new governance changes to these venues. Over time, the DAO will be able to engage with the security community in a more open manner.

[Read the Volt Protocol security review of Compound v2 here.](https://github.com/volt-protocol/volt-protocol-core/blob/develop/audits/venue-audits/compound.md)

## Venue Risk Management
Each venue may have distinct risk and liquidity profiles, but types of venues often share risks in common that can be mitigated. For example, in Compound v2 or other lending pool system, there is a "race condition" in the event of failed liquidations. The market does not automatically detect bad debt, so those lenders who withdraw first take no loss, while those who are too slow are stuck with the loss. A timely reaction can protect the VOLT holders compared to the lending market as a whole.

### PCV Guard
The simplest defense is a permissioned role to withdraw PCV from a venue in an emergency. Currently assigned to a set of core team members, the PCV Guard role can remove protocol assets from a venue like Compound and return them to the Governor.

### Bad Debt Sentinel
The full market governance system should not rely on permissioned roles, though these can be valuable for the early system. Permissionless safeguards are generally referred to as Sentinels. A Bad Debt Sentinel will be able to detect and provide evidence on chain of underwater borrowing positions in integrated markets like Compound, and trigger a withdrawal of Volt Protocol PCV, receiving a small reward in the process.

## Base Yield Venue
The Base Yield Venue is not necessarily a single venue but a default strategy when no active market governance occurs. The rate generated by the base yield venue is the minimum rate VOLT holders will receive when the surplus buffer ratio is at target. The initial base yield venue may be a strategy that allows rebalancing between a few high quality venues and assets.

When there is no participation in market governance, all PCV defaults to the Base Yield Venue. PCV restricted from use in market governance and held in the reserve will also be included in the Base Yield Venue if not held in a custodial PSM.

The base venue is liquid and should have no fee for VOLT holders who wish to redeem directly, vs market governance which may have a nominal fee to prevent griefing.

## Internal Yield Venues
Besides external yield venues, Volt Protocol is likely to develop its own lending markets and yield strategies to allow for ever more customizable risk and return strategies for VCON market governance. An example would be a native market for lending against a single asset, ETH. This market gives users access to the best borrowing rate among Volt Protocol and its supported venues. Having only a single super liquid collateral asset greatly reduces risk, and no long tail assets will ever be added as collateral.

# Market Module
Besides deposits of DAI or USDC into frictionless venues like Compound, most strategies for market governance will face slippage when funds are reallocated between venues. This either introduces griefing risk or imposes costs on VCON holders. The market module is a (partial) solution, allowing swaps between stablecoins or dollar-denominated yield products at low, no, or even negative swap costs to the Volt system.

The market module is a type of Peg Stability Module that allows a one way swap from asset A to asset B at a fixed exchange rate. For example, the USDC-BUSD-1 module would offer USDC in exchange for BUSD at a 1:1.0001 ratio. By undercutting normal stableswap fees, we empirically observe that a PSM can be fully used on a reasonable timespan.

While funds are deposited in the Market Module, they are not idle, but instead are still deposited in the underlying venues to earn yield. The Market Module simply has permission to pull a certain amount of funds from a venue on behalf of those who have "deposited" there. In this way a switch from Compound USDC to Euler BUSD might be accomplished seamlessly with no fees, earning yield the whole time.

# Consensus Parameters
Despite our efforts, there are certain system parameters that may need human intervention to tune, or judgements to be made such as marking down the value of collateral after losses in a yield venue due to an exploit. These require token voting outside a strict context of economic incentives, with changes subject to the Veto Module.

## Target surplus buffer ratio
The target ratio between the system’s reserves in excess of the backing of the VOLT supply, and the total circulation of VOLT. This important system constant should be determined by governance based on the overall risks of the whitelisted yield venues. Exceeding this threshold for prolonged periods will cause the VOLT system to adjust baseline rates down, and vice versa. This is a key limit to excessive risk-taking.

## Target VCON Participation
The target portion of the VCON supply engaged in market governance. This important system constant should be determined by governance based on the performance gains of market governance vs more static yield deployments. Exceeding the target threshold for prolonged periods will cause the share of system profits distributed to market governance participants to reduce, and vice versa.

## Surplus buffer deviation sensitivity
Referred to as “k” in the relevant equations, this is the system’s sensitivity to immediate term deviations from the target surplus buffer ratio. A higher k means a larger change in the VOLT rate in response to deviations from the target surplus buffer size. This system constant should be determined by governance through models and simulations to determine an optimal sensitivity to meet the system’s liquidity needs at a given time, based on the composition of the PCV and the velocity of the VOLT supply.

## VCON utilization deviation sensitivity
Another k, this constant is similar to the above and determines how sensitive the VCON profit share is to deviations from the target participation rate in market governance. This constant should be determined by governance to allow VCON holders reasonably consistent fee share while still adjusting to supply and demand.

## Auction parameters
What portion of the VCON supply can be sold in a given auction to grow the surplus buffer, and the maximum frequency of these auctions, are important questions for the growth of Volt Protocol. It is difficult to determine how decision making about VCON emission can be performed purely through market governance at this stage. A later implementation may attempt to allow market governance to control the frequency and size of VCON auction.

## PSM Allocations
How much VOLT is mint/redeemable on which layer must be decided by governance. In general, no more than 10% of the VOLT supply should be minted through a single L2 PSM.

# VCON Economics

## VCON Auction
Merely accumulating the surplus buffer from system fees constrains expansion. VCON auctions can be used to grow the surplus buffer and allow more rapid expansion than reliance on fee accumulation while remaining within safe bounds. At regular intervals, a VCON auction can be triggered with a minimum price equal to the value of the surplus buffer divided by the VCON supply, plus a mint fee. The maximum size and frequency of VCON auctions can be determined by governance.

## Ragequit
VCON holders can “ragequit” in exchange for a pro rata share of the surplus buffer less a redemption fee. This allows capital to exit the system when the surplus buffer is too large relative to the PCV, if the system were to become defunct, or if they judge their fellow VCON holders to be taking unacceptable risks in governance. The process is rate limited so VOLT holders can react to fluctuations in the size of the surplus buffer.

VCON holders can only ragequit so long as the surplus buffer is above a target ratio determined by governance, which signfies low demand to hold VOLT vs the amount of capital in the system. When the surplus buffer is low, ragequit will be blocked to ensure VOLT holders remain protected.

# vETH
Market governance can be applied to alternative denominations of capital besides USD. The same infrastructure that will govern VOLT can be extended to support vETH, a market governed ETH-denominated currency. Other “fiat” denominations are also possible, if there is on chain holder demand to support tokenization. Supporting multiple capital types will enable efficiency gains in lending, borrowing, and exchange. There are several advantages to including an ETH derivative in the VOLT system. The same market governance codebase can support allocation of ETH or its derivatives into yield venues, or even conduct Rocket Pool-esque decentralized staking. Volt Protocol can internalize borrow aggregation and peer to peer matching functions in the future, such as allowing vETH holders to borrow in stablecoins at whichever is lowest among the VOLT rate or the whitelisted yield venues.

# Checks and Balances
Profit motive for VCON holders alone is insufficient to ensure certain protocol functions are performed correctly from the perspective of VOLT holders. To prevent abuses, checks and balances will be added outside of market governance. Most prominent among these are the two Veto Modules, mechanisms by which VOLT or VCON holders can veto system changes. This applies to any governance action, such as parameter adjustments or onboarding of new yield venues. Different quorum thresholds may be used for different types of changes.

## Optimistic Governance
While the Volt Protocol market governance system is in its early stages, it's not realistic to expect VOLT and VCON holders to take an active role in its ongoing development. However, it is essential that they have the right to dissent against harmful changes by the core team. We will accomplish this with optimistic timelocks where the core contributors can propose system changes, but VOLT and VCON holders are both (separately) capable of veto. VCON holders will also receive proposer rights.

## VOLT Veto Module
An appropriate quorum of the VOLT supply will be able to veto any governance proposal, with the veto threshold determinable by governance. Delegation will be important for this system, as the majority of stablecoin holders cannot be expected to pay active attention to governance changes. The goal of the Veto Module is that an active minority defending their own interests will benefit the collective.

## VCON Veto Module
Similar to the VOLT Veto Module, the implication of the VCON Veto Module is that it is easier to block a governance change than to pass one. A small minority may be able to pass a change if no one dissents, while it might take an absolute majority to push a change against the wishes of a dissenting minority.

## Timelocks
The principle risk of the Veto Module is that deadlock is produced. To mitigate this, Volt Protocol governance will use timelocks with different proposal durations, quorum thresholds, and veto requirements. A three day timelock might need only 1% of the VOLT supply to veto, but 10% of the VCON supply to submit a proposal, while a one month timelock needs only 1% of the VCON supply to submit a proposal, but requires 10% of the VOLT supply to veto.

# MVP Constraints
This document outlines a complicated system that will be a substantial engineering effort to complete safely. A simpler intermediate model is being developed. This model has the following restraints that make it more feasible to implement over a two to three month timeline:

* the VCON governance token will not be transferable, and thus not subject to liquidations in market governance
* market governance participants will receive VCON rewards, not cash (all excess yield to the surplus buffer)
* there will be neither VCON auctions nor VCON redemption (ragequit)
* the system accounting and yield oracles cannot directly account for losses -- in the event of a loss in a yield venue, the PCV Guard will withdraw protocol assets to the Governor, and Governance must make any accounting changes needed in the venues

# Cross Chain
The Volt Protocol cross-layer mechanisms will emphasize simplicity.

VOLT is currently available on both Arbitrum Nitro and Ethereum mainnet. In principle VOLT minting and redemption can be performed on any chain; in practice it will be restricted to trustless Layer Two networks on Ethereum (or those well on their way to trustlessness). Volt Protocol will NOT deploy PCV into any non-L1 yield venues in the initial version, as this adds unreasonable complexity to PCV and yield accounting.

## Liquidity Management
When a Peg Stability Module on a given L2 is above its target reserves by a certain threshold, a permissionless autotask will remove idle funds to L1 for market governance to allocate. This does not require any cross chain state observations, simply a transfer of stablecoins from L2 to L1. The total amount of funds moved from L2 to L1 is tracked in an accumulator.

When a Peg Stability Module on a given L2 is below its target reserves by a certain threshold, a permissionless autotask will send funds from L1 to refill the L2 PSM. This does not require any cross chain state observations, as the maximum amount of funds to send will be limited to what was previously sent. There will also be rate limits for movement of funds in either direction to further reduce risk.

This minimal system that does not involve complex cross chain state observations or communication will allow for robust usage of VOLT on multiple layers.

## Rate and Price Uniformity
When the VOLT rate is updated on L2, it will read in the L1 VOLT rate and normalize the VOLT price. This means that although it's possible for a small deviation to occur in L2 and L1 VOLT price if the L1 rate is updated before the L2 price, it will shortly be normalized. In the worst case scenario of Arbitrum downtime that coincides with the VOLT rate update, it is highly unlikely that the system would experience more than a 0.006% deviation before a correction occurred (assuming a VOLT rate update of +-2%, and a full day of Arbitrum downtime).
