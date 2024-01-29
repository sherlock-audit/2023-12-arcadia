
# Arcadia contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Q&A

### Q: On what chains are the smart contracts going to be deployed?
- Now: Base
- Future: Optimism, Arbitrum, other L2s
___

### Q: Which ERC20 tokens do you expect will interact with the smart contracts? 
COMP, DAI, USDT, USDC, USDbC, rETH, wstETH, cbETH, wETH, SGT
___

### Q: Which ERC721 tokens do you expect will interact with the smart contracts? 
UNI-V3, Arcadia Stargate Positions (accounts-v2/src/asset-modules/Stargate-Finance/StakedStargateAM.sol)
___

### Q: Do you plan to support ERC1155?
Yes
___

### Q: Which ERC777 tokens do you expect will interact with the smart contracts? 
Any established ERC777, given it can be correctly priced.
___

### Q: Are there any FEE-ON-TRANSFER tokens interacting with the smart contracts?

Arcadia Accounts V1 generally do not support FOT tokens. FOT’s with ASSET_TYPE = 0 will not be supported. We could create a new asset type to accommodate this in future Account versions or handle it in a future Account version’s deposit flow.

However, USDC/USDT or other established tokens will be allowed even though the option exists that it becomes a FOT. Should any proxied token be switched to a FOT, the protocol will take steps through collateral & liquidation factors, removal of Account versions in the Creditor and creating a new Account version accommodating this behaviour.
___

### Q: Are there any REBASING tokens interacting with the smart contracts?

Not supported in AccountV1 (does not account for negative rebasing tokens at the moment).
Similar as with FOT tokens, a new Account version could support REBASING tokens (with a different ASSET_TYPE)
___

### Q: Are the admins of the protocols your contracts integrate with (if any) TRUSTED or RESTRICTED?
Chainlink and contracts of primary assets are TRUSTED, others are RESTRICTED
___

### Q: Is the admin/owner of the protocol/contracts TRUSTED or RESTRICTED?
- Owner is still TRUSTED, but we do everything we can to mitigate “godpowers”
    - Assets and pricing are append only, can’t be overwritten
    - Pausing functionality limited to max 30 days
    - Risk management can be outsourced
    - …
___

### Q: Are there any additional protocol roles? If yes, please explain in detail:
The following lists are non-exhaustive.

- Owner Protocol (core team):
    - Can add and block Account versions to the Factory.
    - Can set baseURIs.
    - Can append assets and oracles to the Protocol.
    - Can set the sequencerUptimeOracle.
    - Can set the interest rate curves.
    - Can set the liquidation parameters.
    - Can set the auction curve parameters.
    - Can set the minimumMargin.
    - Can activate/set fee shares (including origination fee, interest fees, liquidation fees).
    - Can reactivate paused functionalities.
    - Can change the pause Guardian.
    - Can change the Risk Manager.
    - Can initialise or activate contracts (other than deployed Accounts, which can only be initialised by the Factory).
    - Can add Tranches.
    - Can unlock Tranches.
- Owner of each Account
    - Can manage Account as if it was a smart contract wallet (withdraw, deposit, borrow, transfer…).
    - Can set/revoke an AssetManager.
    - Can upgrade the Account version to a version added by the protocol owners.
    - Can set a (approved) Creditor
    - **Cannot** set approvals from the Account to any token.
- AssetManager
    - Can withdraw, deposit, borrow, transfer… on behalf of the owner of the Account (the account owner should fully trust this role, the asset manager can act on behave of this user).
- Pause Guardian
    - Can pause contracts (for max 30 days).
- Risk Manager
    - Can set the risk variables (collateral and liquidation factors, maxExposures, min value of assets, fixed liquidation cost) for the Creditor to which its linked.
    - Can set the asset recipient for the Creditor to which it is linked.
- AssetRecipient
    - Receives remaining assets of failed auctions.
- Creditor or approvedCreditor of Account.
    - Can change the Creditor
    - Can perform similar actions as AssetManager, and should thus be fully trusted by the Account owner.
___

### Q: Is the code/contract expected to comply with any EIPs? Are there specific assumptions around adhering to those EIPs that Watsons should be aware of?
- Account proxy is simplified version of ERC1967.
- Debt (debtToken) is a non-standard ERC4626 (non transferrable, maxDeposit() and maxMint() are not implemented).
- Liquidity Positions of the Tranches are ERC4626s, should be 100% according to the standard.
- The Arcadia Accounts themselves are ERC721s (Factory maps id one-to-one to Account contract address).
- The Staking Module is an ERC721.
___

### Q: Please list any known issues/acceptable risks that should not result in a valid finding.
- If a contract is owner of an Account and has the exact executeAction function implemented (or an unprotected withdrawal in their fallback), and Transfers the Account to a new owner, it may be vulnerable to old approves.
- If you give a credit allowance to someone, transfer ownership of your account, and receive ownership back X time later, the old allowances become valid again.
`uint256 allowed = creditAllowance[account][accountOwner][msg.sender]`
- The risk manager of a creditor can change liquidation factors, collateral factors and minimum margin at any time. This may lead to early liquidation of Accounts, although their assets will be sold for fair market value and any surplus will be credited towards the original owner of the Account.
- Negative rebasing tokens cannot be used in current implementation of Accounts, as it might result in failed withdrawals, wrong valuation and non-ending auctions (can be mitigated by manually transferring tokens to the Account to make up for the difference).
- The price calculation of the Dutch auction may revert after a long time, but not before the set base and cutOff time (max 18 hours). After this time, the auction can be ended without passing through the price calculation.
- Some tokens can blacklist users or pause operations.
- If the bad debt of a liquidation exceeds the liquidity contained in the most junior tranche, the senior tranche will also be affected. Since this senior tranche is not locked during auctions, this liquidation can be frontrun by withdrawing liquidity from the senior tranche.
- No validation is performed on the storage slot of `IMPLEMENTATION_SLOT` on the proxies (as this is stored as a constant).
- Contracts are compiled with Solidity 0.8.22 and evm Shangai, which might cause issues for deployments on chains not supporting PUSH0.
- Withdrawals by LPs may leave dust liquidity behind in the lending pools (the difference in interests/assets between transaction creation and confirmation).
- Any finding which has been acknowledged or mentioned in a previous audit report.
- Id’s of ERC721 and ERC1155 should not exceed uint96 (→ if they can exceed uint96 they should be wrapped in a new contract before they can be deposited).
- Balances of ERC20 or ERC1155 or the USD value of derived assets (with 18 decimals) are assumed to never exceed a uint112.
- `liquidityOfAndSync()` syncs interests but does not recalculate a new interest rate. This function is originally implemented for the Tranche to avoid a double calculation of pending interests in a single transaction. The function can be also be called by non-Tranches but the result is identical as if the function would not be called at all (interests are compounding continuously with a fixed interest rate).
- If the protocol is paused, some interest may keep accruing until a permissionless function is called to set the interest at 0.
___

### Q: Please provide links to previous audits (if any).
https://github.com/arcadia-finance/arcadia-finance-audits/tree/main/audits-v2
___

### Q: Are there any off-chain mechanisms or off-chain procedures for the protocol (keeper bots, input validation expectations, etc)?
- Liquidations (Dutch auctions) rely on arbitrageurs, MEV bots.
Both to start liquidations, to bid on collateral and to end auctions.
- If the protocol is paused for 30 days, any external party can unpause it partly during a 2 day window (to prevent protocol owners locking up withdrawals forever).
- While the protocol is paused everyone can set the interest rate to 0.
___

### Q: In case of external protocol integrations, are the risks of external contracts pausing or executing an emergency withdrawal acceptable? If not, Watsons will submit issues related to these situations that can harm your protocol's functionality.
Yes, they are acceptable.

- Has to be determined on a case by case basis.
- If possible the protocol handles these edge cases (eg. the protocol can handle malfunctioning Chainlink oracles)
- Worst case this has to be mitigated by the appropriate risk factors.
___

### Q: Do you expect to use any of the following tokens with non-standard behaviour with the smart contracts?
- **Streaming Token**
    - Not supported in AccountV1.
- **Revert on Zero Value**
    - Supported, transfers of assets are done with Solmate’s SafeTransferLib
- **Revert on Non-Zero to Non-Zero Approval**
    - Supported, transfers of assets are done with Solmate’s SafeTransferLib
- **Doesn’t Revert on Failure**
    - Supported, transfers of assets are done with Solmate’s SafeTransferLib
    - If this shouldn’t be caught, this may be a big issue (since the value is stored on the Account but funds are not actually transferred)
- **Blacklisting**
    - Supported. This may cause liquidations to not end, but in that case the protocol takes ownership of the Account to manually resolve the situation after cutOffTime.
- **Token upgradeability**
    - Supported. Although unpredictable and has to be evaluated case-by-case.
- **Over 18 decimals**
    - We check internally in the primary asset modules so that no primary asset can be added as collateral which has more than 18 decimals. This has to be checked in any future asset modules as well if relevant.
    - For derived assets (e.g. ERC4626 Vaults) this should not be a problem, as long as the underlying asset has 18 or less decimals.
- **Pausability**
    - Supported. If a token is ‘paused’ (ie not transferable), it may lead to similar issues as being blacklisted.
- **Flash-mintable tokens**
    - Supported. We may do unsafe casts on some exposures to uint112, however they should only be done after a check of that exposure variable as a uint256 against maxExposure (which is a 112).
- **Callback tokens (ERC777)**
    - Supported.
- **non-bool returning tokens**
    - Supported, Should be OK with SafeTransferLib
___

### Q: Add links to relevant protocol resources
- Invariants:
    - LendingPool:
        - The sum of the the total debt of the pool and the underlying asset remaining in the pool should be bigger than or equal to the total Liquidity LPs have a claim to. In the contract this is expressed as:
        asset.balanceOf(address(this)) + realisedDebt ≥ totalRealisedLiquidity
        - Only Accounts can have a non-zero debt balance.
        - realisedDebt equals sum of all accounts debt positions (balanceOf).
        - totalRealisedLiquidity equals sum of all claims of individual LPs.
        - totalInterestWeight is sum of all individual weights.
        - No more than 256 tranches to a single lending pool.
        - The LendingPool cannot operate after Feb 07th 2106.
    - Account:
        - Balance of assets in Account should be equal to or bigger than the amount stored.
        - An amount stored is always strictly bigger than 0.
        - Per DerivedAssetModule: Sum of all lastExposureAsset equals lastUsdExposureProtocol.
        - Per DerivedAssetModule: The lastExposureAssetToUnderlyingAsset is smaller than or equal to the exposure of the underlyingAsset in its own AssetModule.
        - The numeraire of the Account should always be identical to the numeraire of its Creditor (if set).
        - If an account has an open position (liabilities), it’s numeraire and creditor can’t change.
        - During Auctions, an Account cannot: withdraw, deposit, change numeraire, change creditor.
        - The ownership of an Account should map one-to-one with the owner of the id of the ERC721 in the factory.
- Potential attack vectors:
    - AbstractDerivedAssetModule has complex logic to calculate and update the price and/or exposure of both the asset and its underlying assets.
    - flashAction bundles multiple actions together (borrow, withdraw, deposit, transferFrom), and calls user defined, untrusted external logic.
    - The Account logic can be upgraded (on an opt-in basis by both the AccountOwner and Creditor, not forced by the Protocol).
    - In some flows, untrusted or ‘non-real' 'Account’ or ‘Creditor’ addresses can be given, which shouldn’t impede in any of the flows of actual Account and our own Creditor(s).
    - Changing/setting creditor may not cause at any time to have a situation where the numeraire is different than the numeraire of the creditor that is set (for example, all value in the account is already expressed in ETH (18 decimals) but the creditor is still a USDC-based creditor (6 decimals) and any external call can be made which may lead to the user taking on additional debt → this would lead the USDC creditor to think the Account has much more value than it actually has).
    - There should be no flow in which an auction cannot end in any manner, as this will permanently lock the junior tranche of the liquidity pool.
    - We do a few authorization checks in the LendingPool that the msg.sender is an Account, by checking if msg.sender has a non-zero debt balance. If we can somehow have non-zero balances for non-Accounts they might circumvent some of these checks.
    - All rounding should be in the disadvantage of the user (eg. rounding down for assets, rounding up for liabilities).
    - Unrealised debt calculation in LendingPool.calcUnrealisedDebt is unchecked, but shouldn’t overflow for ‘reasonable’ values.
    - Stored balance of a token in the Account can never be 0, as this would block liquidating Accounts due to a divide by 0.

To install a local repo:
- install Accounts V2 (https://github.com/arcadia-finance/accounts-v2.git) and Lending V2 (https://github.com/arcadia-finance/lending-v2.git)
- add `RPC_URL=<rpc url to a base mainnet (or fork)>` in your .env

gh repo clone arcadia-finance/accounts-v2
cd accounts-v2
git checkout audit/sherlock
git pull
forge install
forge build
forge test

gh repo clone arcadia-finance/lending-v2
cd lending-v2
git checkout audit/sherlock
git pull
forge install
forge build
forge test
___



# Audit scope


[accounts-v2 @ 9b24083cb832a41fce609a94c9146e03a77330b4](https://github.com/arcadia-finance/accounts-v2/tree/9b24083cb832a41fce609a94c9146e03a77330b4)
- [accounts-v2/src/Factory.sol](accounts-v2/src/Factory.sol)
- [accounts-v2/src/Proxy.sol](accounts-v2/src/Proxy.sol)
- [accounts-v2/src/Registry.sol](accounts-v2/src/Registry.sol)
- [accounts-v2/src/abstracts/Creditor.sol](accounts-v2/src/abstracts/Creditor.sol)
- [accounts-v2/src/accounts/AccountStorageV1.sol](accounts-v2/src/accounts/AccountStorageV1.sol)
- [accounts-v2/src/accounts/AccountV1.sol](accounts-v2/src/accounts/AccountV1.sol)
- [accounts-v2/src/asset-modules/ERC20-Primaries/ERC20PrimaryAM.sol](accounts-v2/src/asset-modules/ERC20-Primaries/ERC20PrimaryAM.sol)
- [accounts-v2/src/asset-modules/Stargate-Finance/StakedStargateAM.sol](accounts-v2/src/asset-modules/Stargate-Finance/StakedStargateAM.sol)
- [accounts-v2/src/asset-modules/Stargate-Finance/StargateAM.sol](accounts-v2/src/asset-modules/Stargate-Finance/StargateAM.sol)
- [accounts-v2/src/asset-modules/UniswapV3/UniswapV3AM.sol](accounts-v2/src/asset-modules/UniswapV3/UniswapV3AM.sol)
- [accounts-v2/src/asset-modules/abstracts/AbstractAM.sol](accounts-v2/src/asset-modules/abstracts/AbstractAM.sol)
- [accounts-v2/src/asset-modules/abstracts/AbstractDerivedAM.sol](accounts-v2/src/asset-modules/abstracts/AbstractDerivedAM.sol)
- [accounts-v2/src/asset-modules/abstracts/AbstractPrimaryAM.sol](accounts-v2/src/asset-modules/abstracts/AbstractPrimaryAM.sol)
- [accounts-v2/src/asset-modules/abstracts/AbstractStakingAM.sol](accounts-v2/src/asset-modules/abstracts/AbstractStakingAM.sol)
- [accounts-v2/src/guardians/BaseGuardian.sol](accounts-v2/src/guardians/BaseGuardian.sol)
- [accounts-v2/src/guardians/FactoryGuardian.sol](accounts-v2/src/guardians/FactoryGuardian.sol)
- [accounts-v2/src/guardians/RegistryGuardian.sol](accounts-v2/src/guardians/RegistryGuardian.sol)
- [accounts-v2/src/libraries/AssetValuationLib.sol](accounts-v2/src/libraries/AssetValuationLib.sol)
- [accounts-v2/src/libraries/BitPackingLib.sol](accounts-v2/src/libraries/BitPackingLib.sol)
- [accounts-v2/src/oracle-modules/ChainlinkOM.sol](accounts-v2/src/oracle-modules/ChainlinkOM.sol)
- [accounts-v2/src/oracle-modules/abstracts/AbstractOM.sol](accounts-v2/src/oracle-modules/abstracts/AbstractOM.sol)

[lending-v2 @ dcc682742949d56928e7e8e281839d2229bd9737](https://github.com/arcadia-finance/lending-v2/tree/dcc682742949d56928e7e8e281839d2229bd9737)
- [lending-v2/src/DebtToken.sol](lending-v2/src/DebtToken.sol)
- [lending-v2/src/LendingPool.sol](lending-v2/src/LendingPool.sol)
- [lending-v2/src/Liquidator.sol](lending-v2/src/Liquidator.sol)
- [lending-v2/src/Tranche.sol](lending-v2/src/Tranche.sol)
- [lending-v2/src/guardians/LendingPoolGuardian.sol](lending-v2/src/guardians/LendingPoolGuardian.sol)


