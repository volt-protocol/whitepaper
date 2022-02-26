# ⚡VOLT⚡

High inflation erodes the purchasing power of savers over time unless they seek out yield and undermines those dependent on fixed income. Volatile inflation makes it impossible to plan ahead financially when the value of the money itself can change unpredictably. Volt offers a solution -- a stablecoin whose price tracks inflation, rather than a depreciating fiat currency.

VOLT will start at a price of $1 and adjust over time according to consumer price index (CPI) data. This will allow VOLT holders to preserve their wealth over time without the need to actively manage their savings or taking on excessive risk.

# Volt and the CPI
The CPI-U or Consumer Price Index for Urban Consumers is composed of data collected by the Bureau of Labor Statistics (BLS) on consumer prices in urban areas. It tracks the prices of goods and services in relation to the starting index price for those same items in 1982. The U.S. Government targets an inflation rate of 1-2% annually which is designed to erode purchasing power over time and incentivize those who hold wealth to either spend it, which stimulates the economy or invest it which provides jobs. Recently, the inflation rate has been higher and more volatile, putting more pressure on savers or those on fixed incomes.

## Methodology
The CPI-U was chosen because it most accurately reflects the rising costs consumers pay due to inflation in the real economy. The BLS collects new prices and reports new CPI data monthly. During certain times of the year, prices of certain goods and services predictably rise due to seasonal changes in consumer demand. An example of this is propane gas whose price increases in the winter due to increased demand. Because certain goods and services increase in prices based on seasonal demand, the BLS has two data sets on inflation. One which is adjusted for seasonal changes in supply and demand, and one which is not adjusted for seasonal factors. Because all changes are linearly interpolated over one year, the VOLT unit of account will use non seasonally adjusted numbers because they most accurately track prices consumers pay.

## Chainlink Oracles
In order to get this data into the VOLT system, a Chainlink oracle will be used to feed the newest monthly CPI data into the VOLT price oracle. Each new piece of data from Chainlink will be retained for 12 months to calculate the Trailing Twelve Month (TTM) inflation rate.

## VOLT Price Oracle
The VOLT system will expose a public smart contract API that any project can use to get the system price of VOLT, in other words the current CPI index. This allows many projects to coordinate around this Schelling point for a unit of account that preserves purchasing power through time. Other projects including FPI plan to use this API in their token design. This will create network effects around the VOLT unit of account and encourage deep liquidity on decentralized exchanges.

# VOLT Issuance
All VOLT will be backed by yield sources offering returns greater than inflation at an acceptable risk. The role of the VCON DAO, like in MakerDAO or other lending platforms, is to manage this risk. There are two main mechanisms for VOLT issuance, the peg stability module (PSM) and debt issuance.

## Peg Stability Module
VOLT will be issued through a Peg Stability Module (PSM) which will allow users to either mint or redeem VOLT at the current target price reported by the Volt Price Oracle in exchange for accepted stablecoins. These will be immediately deployed into approved yield venues that offer returns greater than or equal to the prevailing inflation rate. Likewise, when a user wishes to exchange their VOLT for stablecoins, they can always swap at the PSM at VOLT's target price. Stablecoins are withdrawn from the yield venues and given to the user in exchange for VOLT. At launch, there will only be a FEI PSM, with all PCV FEI deposited in a single yield venue. Our default choice is Fuse pool 8 (FeiRari DAO Pool), though if Fuse pool 156 (Tribe Convex Pool) is live before our launch we may prefer that option. In the future, VCON holders will allocate PCV assets among numerous venues.

The PSM will charge a fee on VOLT issuance, with no fee on redemption. At launch the fee will be set to 2% to encourage building up a larger system surplus. After the launch of VCON incentives, the fee will be reduced to 1%.

## VOLT Lending
In addition to the PSM, we will issue VOLT directly through supported Fuse pools. VOLT issuance pools will use the VOLT Price Oracle rather than market price data for the purposes of lending and liquidations. This means that borrowers are safe from being liquidated due to excess VOLT demand, and VOLT is not overvalued in these pools as collateral during times of high demand.

At launch, we will support the following Fuse pools:
* pool 8 (FeiRari DAO pool)

## Buffer Caps and Gradual Issuance
Both supported Fuse pools and PSM venues will have buffer caps and daily issuance limits, meaning VOLT supply increase will be constrained to a predictable and gradual rate. At first, the buffer cap will be 30k VOLT and daily issuance of 25k VOLT in the PSM. For debt issuance, the buffer cap will be 200k VOLT and daily issuance of 165k VOLT. At launch these rates will target a maximum issuance of 5 million VOLT in debt issuance and 1 million in the PSM. In the future, the rate will be set to a level judged appropriate based on available liquidity and demand for VOLT.

## External PSM and Tribe Launch
VOLT will seek the support of FEI protocol with our launch with a FEI loan allocated to a PSM controlled by TRIBE governance. This PSM will work in a similar way to the main VOLT PSM, dripping FEI based on a buffer cap of XXk FEI. This will ensure that VOLT price is stable in the early stage before the VCON token and liquidity incentives are established. We will request a loan of $5m FEI in the PSM, equal to the total debt issuance of VOLT that will be allowed prior to the launch of VCON token incentives.

# Monetary Policy
The VCON DAO is responsible for earning yield equal to or greater than inflation on protocol assets, with the goal of increasing the buffer of VOLT's backing over time. Our goal is to preserve purchasing power over the long term, so the protocol will take a conservative approach in authorized yield venues.

## Collateralization
Like Fei Protocol, Volt's PCV will always be **overcollateralized**, with the desired collateralization set by governance. At launch, the system will be only slightly overcollateralized due to the fee on VOLT issuance. Provided the protocol earns yield greater than inflation on protocol assets, collateralization will increase over time. New VOLT issuance through the PSM lowers the collateralization ratio toward 1:1+issuance fee. As a result, the DAO will limit the rate of VOLT issuance when the buffer is too small, or increase issuance if the buffer grows sufficiently. In the future, the DAO will seek to standardize the VOLT supply expansion rate according to a consistent schedule. Early on, we must regulate supply based on the protocol's ability to source yield at an appropriate risk level.

# VCON Tokenomics & Governance

The role of the VCON token is to direct the protocol's assets among yield venues to outperform inflation without taking on excess risk. The full scope of the VCON token will not be enabled at launch, but is described in the section "Liquid Governance".

## VCON Distribution
There will be a total of 1 billion VCON tokens created at launch. Of this supply:

* 10% was allocated for a seed round raising $2m to support VOLT development, on a three year linear vest with one year cliff.
* 20% for VOLT contributors, on a four year linear vest with one year cliff
* 42% for DAO partners
    *  30% given to the TRIBE DAO which has incubated and is jointly building VOLT. This will be offered as 6% given directly to the Tribe DAO in exchange for incubating VOLT and assistance with launch, and an offer of 24% of the supply for a swap with TRIBE at seed round valuation to establish long term alignment. TRIBE obtained in this way will be held as xTRIBE and used to encourage VOLT adoption on Fuse
    *  6% given to OlympusDAO in exchange for participation in the Olympus incubator and onboarding VOLT as a treasury asset
    *  6% offered to Frax protocol as a token swap for FXS in recognition of our joint efforts coordinating on a CPI index. FXS obtained will be locked as veFXS and used to enhance VOLT liquidity through gauge votes
* 18.1% reserved in protocol treasury for token incentives, bonds, or future treasury swaps
    *  0.25% used for VOLT liquidity incentives over the first month after LBP, reassess based on results of this program
* 9.9% issued in LBP to efficiently bootstrap VCON market

Those VCON tokens offered for DAO swaps (not including the 6% each in direct grants to Olympus and TRIBE) will be mutually subject to on chain vesting with clawback.

## Direct Decentralization Through DAO Partners
Volt Protocol has been developed in partnership with the Fei / Rari Tribe. Along with the support of our other partners including OlympusDAO and Frax, we're decentralizing VOLT by distributing VCON to existing token communities. At launch, the protocol will be governed by the Control Multisig currently consisting of Volt Protocol founder Kirk Hutchison, Fei Protocol founder Joey Santoro, Frax Finance founder Sam Kazemian, OlympusDAO representative Glueeater, and Gabagool.eth, a well regarded investor and community member. This multisig will perform protocol governance at launch, remaining answerable to the partner DAOs who control the largest stakes in Volt.

## Liquid Governance
The goal of the VCON governance system is to let tokenholders *directly* allocate protocol assets and make other governance decisions, while having the proper skin in the game. That means that they benefit or suffer most from their *individual* governance decisions, not the result of majority consensus. We accomplish this with a special lending pool for VCON holders, based on the Tribe TURBO system.

The VCON Pool will be the same as the VOLT issuance pool described in the "VOLT Lending" section above, and will also be the recipient of all assets deposited into the PSM. VCON holders will be able to borrow VOLT or other protocol assets subject to a set of special terms and constraints:
* use a system price for VCON instead of market price so holders can borrow their pro rata share of assets
* the pool will have only forced liquidations, as the deposited VCON is used for governance rather than to "back" the assets in the pool
* charge a fixed rate of 1% for VOLT borrowing, charge the inflation rate reported by VOLT Price Oracle plus 1% for stable asset borrowing
* borrowers can only deposit assets into authorized yield venues, like TURBO, not just walk away with them
* we will have incentives on borrowing in this pool so active VCON holders dilute passive holders

This system will allow a market process to govern how VOLT and other PCV assets are allocated among yield venues, while ensuring the protocol is always earning at least the inflation rate on PCV stablecoins. Note that in this model VCON holders directly access a **cash flow**, as whatever they can earn allocating VOLT or PCV stablecoins in excess of inflation and the DAO's 1% cut which accumulates in reserves, they keep.

# Launch Plans

## Guarded Launch
The VOLT issuance system will be launched prior to the existence of the VCON governance token. In this early stage, it will be governed by the Volt DAO multisig, subject to a timelock and with veto power assigned to a Guardian (either the TRIBE Nope DAO or the Fei Labs Guardian). Users will be able to borrow VOLT on Fuse, and the PSM will start with 0 VOLT available for issuance and increase continuously based on the rate set by governance. The system will be live for one month before the LBP, giving users a chance to try it out. During this period, there will be a limit of 5m VOLT issued through Fuse, and 1m issued through our FEI PSM, with a backstop of 5m FEI provided by the TRIBE DAO.

## Liquidity Bootstrapping Event
The VCON governance token will be launched in a liquidity bootstrapping pool (LBP) on Balancer via Copper Launch. An LBP is a mechanism for on chain price discovery of a new token. The LBP will be seeded with VCON governance tokens and protocol controlled FEI. The LBP determines what the starting price of the VCON token is at launch. This VCON/FEI liquidity will remain permanently as PCV, although in the future may be migrated to VCON/VOLT liquidity. The LBP will start at a market cap of $250m for VCON or $0.25 per VCON token and last for a week. The pool will be seeded with 9.9% of the VCON supply, 99m VCON tokens and 250k FEI at a 99/1 weighting.

After the LBP, there will be VCON incentives available for the G-UNI VOLT/FEI LP pair as well as the VOLT/OHM Curve LP pair. Effectively, incentives on liquidity are a continuous token sale, and contribute to increasing the size of the system surplus. As a matter of policy, the VCON DAO is willing to engage in joint incentives with reputable stablecoin issuers for VOLT pairs, especially when it involves DAO treasury partnerships.

## Olympus Pro Bonding
After the LBP, we will offer VCON in exchange for VOLT bonds on Olympus Pro. 