
# Arcadia contest details

- Join [Sherlock Discord](https://discord.gg/MABEWyASkp)
- Submit findings using the issue page in your private contest repo (label issues as med or high)
- [Read for more details](https://docs.sherlock.xyz/audits/watsons)

# Q&A

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


