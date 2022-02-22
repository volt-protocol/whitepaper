# ⚡VOLT⚡

Inflation erodes the purchasing power of savers over time unless they seek out yield. Volatile inflation makes it impossible to plan ahead and undermines those dependent on fixed incomes. It also makes it difficult to trust "fixed rate" terms, when the value of the money itself can change. Volt offers a solution -- a stablecoin whose price tracks inflation, rather than a depreciating fiat currency.

VOLT will start at a price of $1 and adjust over time according to consumer price index (CPI) data. This will allow VOLT holders to preserve their wealth over time without the need to actively manage their savings or taking on excess risk.

## Volt and the CPI
The CPI-U or Consumer Price Index for Urban Consumers is composed of data collected by the Bureau of Labor Statistics (BLS) on consumer prices in urban areas. It tracks the prices of goods and services in relation to the starting index price for those same items in 1982. The U.S. Government targets an inflation rate of 1-2% annually which is designed to erode purchasing power over time and incentivize those who hold wealth to either spend it, which stimulates the economy or invest it which provides jobs. Recently, the inflation rate has been higher and more volatile, putting more pressure on savers or those on fixed incomes.

### Methodology
The CPI-U was chosen because it most accurately reflects the rising costs consumers pay due to inflation in the real economy. The BLS collects new prices and reports new CPI data monthly. During certain times of the year, prices of certain goods and services predictably rise due to seasonal changes in consumer demand. An example of this is propane gas whose price increases in the winter due to increased demand. Because certain goods and services increase in prices based on seasonal demand, the BLS has two data sets on inflation. One which is adjusted for seasonal changes in supply and demand, and one which is not adjusted for seasonal factors. Because all changes are linearly interpolated over one year, the VOLT unit of account will use non seasonally adjusted numbers because they most accurately track prices consumers pay.

### Chainlink Oracles
In order to get this data into the VOLT system, a Chainlink oracle will be used to feed the newest monthly CPI data into the VOLT price oracle. Each new piece of data from Chainlink will be retained for 12 months to calculate the Trailing Twelve Month (TTM) inflation rate.

### VOLT Price Oracle
The VOLT system will expose a public smart contract API that any project can use to get the system price of VOLT, in other words the current CPI index. This allows many projects to coordinate around this Schelling point for a unit of account that preserves purchasing power through time. Other projects including FPI plan to use this API in their token design. This will create network effects around the VOLT unit of account and encourage deep liquidity on decentralized exchanges.

## Mechanism Design

### Peg Stability Module
VOLT will be issued through a Peg Stability Module (PSM) which will allow users to either mint or redeem VOLT at the current target price in exchange for accepted stablecoins. These will be immediately deployed into whitelisted yield venues that offer returns greater than or equal to the prevailing inflation rate. Likewise, when a user wishes to exchange their VOLT for stablecoins, they can always swap at the PSM at VOLT's target price. Stablecoins are withdrawn from the yield venues and given to the user in exchange for VOLT.

At launch, each stablecoin will have a single yield strategy. In the future, VCON holders will be able to direct protocol controlled value among a wide divrsity of yield venues.

### Supported stablecoins and yield venues

At launch the protocol will support the following stablecoins and yield venues:
* Liquity LUSD, deposited into the Yearn LUSD vault to be staked in the LUSD stability pool
* Fei Protocol FEI, deposited into a Fuse pool owned by the VCON DAO which will earn VCON incentives to guarantee a return matching inflation while we bootstrap lending operations

### Debt Ceiling and Price Behavior
There will be a debt ceiling, in which case the protocol will not issue any new VOLT. There will also be a limit on the rate of new VOLT issuance even when below the debt ceilign. The protocol will **always** buy back VOLT for stables at the current target price. This means that it's possible for VOLT to go above target price when it reaches the supply cap, but users can always get at least the target price (unless the protocol becomes undercollateralized).

The debt ceiling at launch will be 20 million VOLT.

### Allocating PCV

At first, the DAO will maintain a fixed ratio between the PCV venues that is adjustable when we increase the debt ceiling. Based on the need to generate yield greater than inflation, we will move up or down the risk curve. Based on market conditions at time of writing, we expect most PCV to be deposited into the Yearn LUSD vault at launch.

### Fees
A fee will be charged on VOLT issuance, with no fee on redemption. At launch the fee will be larger and compensated with VCON incentives. Later, the fee can be reduced. The assessment of a fee provides a buffer backing VOLT price.

## Monetary Policy
The VCON DAO is responsible for earning yield equal to or greater than inflation on protocol assets, with the goal of increasing the buffer of VOLT's backing over time. Our goal is to preserve purchasing power over the long term, so the protocol will take a conservative approach in whitelisitng yield venues.

### Collateralization
Like Fei Protocol, Volt will always be **overcollateralized**, with the desired collateralization set by governance. At launch, the system will be only slightly overcollateralized due to the fee on VOLT issuance. Provided the protocol earns yield greater than inflation on protocol assets, collateralization will increase over time. New VOLT issuance lowers the collateralization ratio toward 1:1+issuance fee. As a result, the DAO will limit the rate of VOLT issuance when the buffer is too small, or increase issuance if the buffer grows sufficiently.

In the future, the DAO will seek to standardize the VOLT supply expansion rate according to a consistent schedule. Early on, we must regulate supply based on the protocol's ability to source yield at an appropriate risk level.

## Tokenomics & Governance

The role of the VCON token is to direct the protocol's assets among yield venues to outperform inflation without taking on excess risk. The full scope of the VCON token will not be enabled at launch, but is described at the end of this document in the section "Decentralizing VOLT".

### VCON Distribution

There will be a total of 1 billion VCON tokens created at launch. Of this supply:

* 10% was allocated for a seed round raising $2m to support VOLT development, on a three year linear vest with one year cliff.
* 20% for VOLT contributors, on a four year linear vest with one year cliff
* 42% for DAO partners
    *  30% given to the TRIBE DAO which has incubated and is jointly building VOLT
    *  6% given to OlympusDAO in exchange for participation in the Olympus incubator and onboarding VOLT as a treasury asset
    *  6% offered to Frax protocol as a token swap in recognition of our joint efforts coordinating on a CPI index
* 18% reserved in protocol treasury for token incentives or bonds
* 10% issued in LBP to efficiently bootstrap VCON market

### Direct Decentralization Through DAO Partners

Volt Protocol has been developed in partnership with the Fei / Rari Tribe. Along with the support of our other partners including OlympusDAO and Frax, we're decentralizing VOLT by distributing VCON to existing token communities. At launch, the protocol will be governed by the Control Multisig currently consisting of Volt Protocol founder Kirk Hutchison, Fei Protocol founder Joey Santoro, Frax Finance founder Sam Kazemiam, OlympusDAO representative Glueeater, and Gabagool.eth, a well regarded investor and community member. This multisig will perform protocol governance at launch, remaining answerable to the partner DAOs who control the largest stakes in Volt.

which is $1 at launch adjusted over time for inflation. There will be a limit on the total issued VOLT, which can be increased over time as the protocol’s ability to deploy capital grows and trust is established in the system. This means it is possible for VOLT to trade above the index price on secondary markets, especially if we’ve reached the debt ceiling. However, VOLT can always be redeemed at target price directly from the protocol.
The PCV deposited to back VOLT will be deployed by the protocol to earn yield greater than inflation. The mandate of the VCON DAO that governs VOLT is to 
In order to create a system that allows holders deep liquidity at current VOLT prices, Peg Stability Modules (PSM’s) will be leveraged that concentrate all liquidity at the current VOLT price as reported by the VOLT Price Oracle.
PSM
PCVDeposits
Buffer Caps
Supply Caps
VCON DAO Monetary Policy
Target inflation + a 5% buffer
VCON DAO is always overcollaterlized
Compare to TRIBE DAO and FRAX
Less overcollateralized than TRIBE, more overcollaterlized than FRAX
Never allow under collaterlization
Grow PCV over time

Governance
Direct decentralization through DAO partners.
Start off with a multisig (phrase this diplomatically)
VCON Governance
Directly control terms of how FEI is lent and get some of the yield

Protocol Controlled Value
The protocol will maintain and invest assets it receives from sale of VOLT. The VOLT protocol will need to be able to maintain or beat inflation. In order to achieve this goal, protocol assets will be invested into lending markets such as Fuse, AAVE and Compound, or yield aggregators such as [INSERT RARI YIELD AGGREGATOR NAME] or yearn. In order to hedge against interest rate fluctuations, the VOLT protocol can acquire interest rate swaps through partner DeFi protocols, trading the protocol’s floating rates for fixed rates.
Protocol Income
Yield from PCV
Peg Stability Module Fees
Incentivizing
Tokenomics
We have a target time surplus in the system based on the current TTM inflation rate. As the time surplus shrinks to within a certain band, VCON will reopen a continuous crowdsale that allows the system to get back a larger time surplus. (manual activation at first, then fully automate things).
This mechanism collectively rewards VCON holders when they make decisions that beat inflation, and punishes them collectively when they make decisions that underperform versus inflation. When the continuous crowdsale opens, this allows new entrants to the system and punishes the existing holders for not beating inflation.

VCON holders will be able to capture a certain amount of profits that exceed the inflation rate. Conversely, VCON holders will be punished if the investments they staked on under perform relative to inflation.
Reward VCON individual holders for their good decisions by giving them profits from the stablecoin staking + liquid VCON so that others that aren’t beating inflation are getting actively diluted, which incentivizes them to at least match inflation in their performance
Reward VCON holders for their collective good decisions
Punish VCON holders for their bad decisions as they get diluted down 7% per year.

VCON Token Distribution
Break down partner DAO’s
FXS
Get 6%
Token swap
OHM
Get 6%
Offer up to 8% for a swap
TRIBE
Get 30%
Full incentive alignment
5 mil Tribe Grant
10% of all VCON into the LBP
Put in 95 parts VCON and 5 parts FEI
Go over continuous dutch auction of VCON during times of under collateralization 
Continuous emissions of VCON to grow PCV.
Use turbo to split revenue between the protocol and VCON holders that stake on certain pools.
Token Sale
2 tranches for sale
1 tranche in an LBP and that will be immediately liquid. Call it the bulk liquidity tranche. This will be used to bootstrap liquidity for VCON.
Hopefully it gets 5 million worth of FEI
Possibility: That FEI then could get sold in exchange for VOLT which then creates a VOLT/VCON pool
1 tranche is a bond which entitles you to face value in VOLT at time of purchase + some VCON on top that vests over time so you can’t just instadump. (We’ll need to figure out what this is). Give out 1 or 2 cents in VCON top.
Two models for this:
Fixed amount of VCON used, no cap on amount of FEI in tranche. The effective rate users earn is decided by how many choose to deposit. VOLT and VCON vest linearly over one year. Potential problem – do users need to commit before they know the rate? Must allow users to set a minimum rate when they lock, and unlock them if their bid isn’t hit.
Fixed amount of FEI targeted, gradually increase the offered rate in VCON until deposits are full.

Algorithmic Risk Controls
The first version of the VOLT protocol will not algorithmically manage risk using onchain data. However, future versions of the protocol will allow lending of VOLT and or PCV into approved venues whose interest rate falls within a certain threshold. This rate is used as a proxy for market risk. This assumes that within our whitelisted pools the markets are efficient in regards to risk.
