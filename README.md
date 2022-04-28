# Smart Contract of HODL

Smart Contract of HODL-Token writtin in Solidity.

## Getting Started

### Solidity Version

* \>=0.8.7

### Dependencies

* SafeMath    >=0.6.8
* Utils       >=0.6.8

### Licence

* SPDX-License-Identifier: Unlicensed

### Installing

* How/where to download your program
* Any modifications needed to be made to files/folders

## Authors

Dev: ??
Upgrade to v7 by @Julek23

## Version History

* 3.0 - Smart Contract of HODL 3
    
* 3.7 - Upgrade to v7
    * Variables deleted:
        * claimgasfee
        * buyBackUpperLimit
        * buyBackEnabled
        * buyBackthresholdLimit
        * mintoken
        * maxtoken
        * sellAllocation
        * buyAllocation
        * transferAllocation
        
    * Variables added:
        * tokenomics (Struct: bnbReward, liquidity, marketing, reflection, reserve)
        * _Reflection (tokenomics.reflection)
        * _Tokenomics (tokenomics.bnbReward + tokenomics.liquidity + tokenomics.marketing + tokenomics.reserve)
        * triggerwallet
        * pairAddresses (mapping: address => bool)

    * Variables changed:
        * layer1tax, layer2tax, ... --> reinvestTax.layer1, reinvestTax.layer2, ...
        * tax1, tax2, ... --> bnbClaimTax.layer1, bnbClaimTax.layer2, ...

    * Variables now unused (but not deletable):
        * marketingshare
        * buybackshare
        * teamshare

    * Functions deleted:
        * Free gas fee
        * Buyback
        * blackList
        * removeFromBlacklist  

    * Public Functions added: 
        * changeTokenomics
        * updatePairAddress
        * initializeUpgradedContract
        * triggerSwapAndLiquify
         
    * Private Functions added:
        * updateTokenomics 

    * Upgrade details:
      * Blacklist functions are removed (this is not from me, actually I don't know why they are removed)
      * Redeem rewards: Calculation of bnbreward and reinvest tokens only if necessary. Free gas fee removed (should lower the gas fee to claim)
      * tokenomics updated: For my understanding in the current contract the tokenomics are not set properly. If a transaction is taxed, 1% is used as reflection. 
        Thats ok. The rest (9%) are send to the contract. When the sell bot sells tokens bnb is generated and split up as this:
          * 12.861% liquidity
          * 9.639% marketing
          * 3.213% team
          * 64.287% bnbpool  
          
         Our tokenomics instead says 10% Reflection, 10% Marketing, 20% Liquidity, 60% BNBpool. I changed the contract so we fullfill the tokenomics.
      * Currently the sell bot is only triggered if someone sells on pancakeswap. In the upgraded contract the sell bot is triggered on every sell regardless the DEX.
      * This also activates the 'Anti-dump' mechanism on every DEX.
      * A new function 'triggerSwapAndLiquify' can be called from the triggerwallet. The triggerwallet could be changed by the owner. This function calls the swap and liquify (the sell bot).
        It also checks if the triggerwallet has less than 0.1bnb and if so, it sends 0.1bnb to this wallet.
        I implemented this so we can set up a bot which has full control of the triggerwallet. This bot needs to check if a swap and liquify is possible (enough tokens available plus pool below rewardhardcap) and could trigger the sell bot continously for example every hour.
        This will ensure that the bnbpool is filled up properly.
        
* 3.7.1 - Upgrade to v7.1
    * Functions deleted:
        * Activate Contract
        * Initialize

    * Changed functions:
        * changerevinvesttax: Now it changes all layers at one transaction
        * changeclaimtax: Now it changes all layers at one transaction

    * Upgrade details:
        * Reinvest taxes removed completly
        * Reinvest procedure changed: Tokens are bought and directly send to the user. No use of the reinvestwallet 
        
        
