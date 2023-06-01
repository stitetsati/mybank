# MyBank

## Requirements

### Smart Contract:

All costs associated with deploying smart contracts to the blockchain shall be paid for by the project owner.

1. Admin functions

    a. ```Setup(uint256[3] interestRates, uint256[6] durations, uint256[] referralRewards, uint256 aumCap)```
    - Each tier (Bronze, Silver, Gold) has its associated interest rate, regardless of the duration of the deposit
    - Duration options are: 3m, 6m, 9m, 1y, 2y, 3y
    - Referral reward rates for level 1 - 7 referrals are to be configured
    - AUM cap is to be configured

    a. ```Withdraw(tokenAddress, amount)```
    - Allows the admin to withdraw a specified amount of tokens from the contract


2. User Functions 

    a. ```ClaimPass(referenceCode)``` 

    - A Genesis Referral Code is 0
    - Allows a user to claim a NFT, which serves as a pass for depositing into MyBank. 
    - The reference code is the tokenId of the referrer
    - An internal storage shall be used to record the referrer associated with this newly minted pass.

    b. ```Deposit(tokenId, duration)```

    - Allows an NFT holder to set up a term deposit of a specified duration
    - If the deposit causes an upgrade of tier, it should trigger reward accumulation for deposits that are eligible. (i.e. for a tier A to tier B upgrade, rewards contributed by the 4th and 5th level referrals should start accumulating.)

    c. ```Claim(tokenId, rewardIds[])```
    - Allows an NFT holder to claim the referral rewards accumulated up to the current timestamp
    - Front-end will query the subgraph API and get all the metadata of any referral rewards associated with an NFT. The UI will then pass an array of rewardIds to the smart contract to claim 
    - Given the flexibility of this function, the front end can pass in all the reward Ids received from the subgraph api, as long as the length does not exceed a hundred (exact number might be 150 or 200), which thereby will hit the limit of a blokchain's block gas limit, it should be accepted by the EVM.


    d. ```Withdraw(tokenId, depositId)```
    - Allows an NFT holder to withdraw a term deposit, and collect the rewards associated with the deposit
    - If the withdrawal causes a downgrade of tier, it should collect all the accumulated rewards contributed by the deposits that fall out of the collectable range. (i.e. for a tier B to tier A downgrade, rewards accumulated by the 4th and 5th level referrals are automatically collected altogether)

3. Other Specifications

    a. Tiers:
    *  Tier Bronze: USDT 100 <= total deposit < USDT 1,000
    *  Tier Silver: USDT 1,000 <= total deposits < USDT 10,000
    *  Tier Gold: USDT 10,000 <= total deposits

    b. Collectable Rewards from Referrals:
    *  Tier Bronze: Collect from deposits made by level 1 - 3 referrals
    *  Tier Silver: Collect from deposits made by level 1 - 5 referrals
    *  Tier Gold: Collect from deposits made by level 1 - 7 referrals
 
    c. Interests from Deposits (configurable):
    *  Tier Bronze: ? % 
    *  Tier Silver: ? % 
    *  Tier Gold: ? % 
 
    d. Rewards from Referral Deposits (configurable):
    *  Level 1: ? %
    *  Level 2: ? %
    *  Level 3: ? %
    *  Level 4: ? %
    *  Level 5: ? %
    *  Level 6: ? %
    *  Level 7: ? %
    
    e. Deposits are not allowed when AUM cap is reached.


### Subgraph

#### Dashboard Data

1. List all deposits associated with the NFT, including startTime, endTime, principal, interest receivable

2. List all referral rewards associated with the NFT, grouped by level of referral (lv 1 - 7)



