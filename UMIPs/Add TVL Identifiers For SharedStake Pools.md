## Headers
UMIP Title: Create KPI Options for SharedStake using SGT as Collateral; Using Price Identifiers for SGT, SGT/ETH UNI LP, and vETH2
Status: Draft
Authors: Warren Muffett and Chimera
Discourse: https://discourse.umaproject.org/t/add-utvl-kpi-sgt-as-a-price-identifier/860
Created: May 16th, 2021

## Summary 
This UMIP enables the DVM to support price requests based on the TVL of SharedStake. 
The DVM should support TVL requests for several SharedStake Pools, including: SGT Pool, SGT/ETH Uni LP, SGT/vETH2 Uni LP, SGT/wETH Saddle Pool, and the single asset vETH2 Pool. 
A synthetic option is minted against a base collateral, in this case SGT, which expires at 00.00(UTC) December 31th 2021.
Options are redeemed on the basis of TVL/10^9, with a floor of 0.1 and a ceiling of 2.

## Motivation 
The primary motivation for the development of KPI options is to allow protocols to incentivize Defi users to assist them to reach the 
protocol's identified goals. By leveraging their community resources, SharedStake can increase TVL, strengthen their governance token, 
and share value with their community members.

### SharedStake TVLUSD

Adding this identifier will allow the creation of KPI options that will hopefully have a positive effective on the marketing of the SGT 
token as well as encouraging growth within the SharedStake ecosystem.

Note: It is anticipated that these Price Identifiers will be used to create KPI Options, however it is acknowledged that it may be used for a variety of purposes.

## Rationale
The UMA Protocol would be improved by this proposal, as this can increase the number of KPI Options being created on the UMA protocol and thus increasing the value of assets supported by the DVM. There will also be collaborative benefits between the UMA and SharedStake communities.

## Data Specifications 
### SharedstakeTVLUSD

Price Identifier Name: SharedStakeTVLUSD

#### Sources and Metricts: 
1. Etherscan: etherscan.io
 	1. SGT: 0xc637dB981e417869814B2Ea2F1bD115d2D993597
	2. SGT/ETH UNIv2 LP: 0x64A1DB33f68695df773924682D2EFb1161B329e8
	3. vETH2: 0xA919D7a5fb7ad4ab6F2aae82b6F39d181A027d35
	4. SGT/vETH2: 0x53dc9d5deb3b7f5cd9a3e4d19a2becda559d57aa
	5. vETH2/wETH: 0xCF91812631e37C01c443a4fa02DfB59ee2DDbA7c
2. Sharedstake.org/earn

#### Cost To Use: Free
#### Real Time Data Update Frequency: Yes


## Technical Specifications

Price Identifier Name: SharedStakeTVLUSD

Base Currency: SGT, ETH, wETH, vETH2 

Quote currency: USD

Intended Collateral Currency: 

Scaling Decimals: 18 (1e18)

Rounding: Round to nearest 8 decimal places (seventh decimal place digit >= 5 rounds up and < 5 rounds down)

There are now five assets approved as collateral within SharedStake’s staking contracts. These pools have been summarized previously. To maintain consistency with existing price identifier UMIPs, it is suggested that TVL is calculated using the deepest DEX pools on Uniswap and Sushiswap. Additionally, there is a liquidity token that is accepted as collateral at Sharedstake.org, which the TVL can be calculated using the Uniswap liquidity / total circulating supply.

The contract addresses for the five staking pools are as shown below:

	1. SGT: 0xc637dB981e417869814B2Ea2F1bD115d2D993597
	2. SGT/ETH UNIv2 LP: 0x64A1DB33f68695df773924682D2EFb1161B329e8
	3. vETH2: 0xA919D7a5fb7ad4ab6F2aae82b6F39d181A027d35
	4. SGT/vETH2: 0x53dc9d5deb3b7f5cd9a3e4d19a2becda559d57aa
	5. vETH2/wETH: 0xCF91812631e37C01c443a4fa02DfB59ee2DDbA7c

These pools consist of total tokens staked and the rewards that are to be distributed to stakers. This is easy to navigate with LP tokens by using Etherscan to understand the pools’ composition and split LPs from SGT rewards.

It is proposed that these are treated in the above groups for the purposes of determining markets and data sources. Discussion is detailed on this in Rationale.
Since the staking pool addresses change at the end of every rewards epoch (currently 3 mos, 1mo for saddle) its best to use something else. 
We propose using ETH reserves in the UNI rewards pools: https://etherscan.io/address/0x3d07f6e1627da96b8836190de64c1aed70e3fc55#readContract.
We can track the total ETH in the pool as getReserves()[ _reserve1] and multiply by the price of ETH x 2 to get TVL.
e.g. currently this is 329 ETH, reported as 329010667784417474724. Therefore 329010667784417474724 / 1e18 * 3,000 * 2 returns the TVL in SGT 
liquidity as roughly ~$2M

vETH2 Minining SGT Rewards can be found here: https://etherscan.io/token/0x898bad2774eb97cf6b94605677f43b41871410b1?a=0xA919D7a5fb7ad4ab6F2aae82b6F39d181A027d35.
 mentioned above

Saddle LP: following a similar mechanism to uni LP, but only counting ETH so as not to double count vETH2 using the Saddle pool contract here: 
https://etherscan.io/address/0xdec2157831d6abc3ec328291119cc91b337272b5

These three addresses should be effective in tracking SharedStakes TVL.

## Implementation

1. Query USD Value of SharedStakeTVL
2. Round to the nearest 8 decimal places

It should be noted that this identifier is potentially prone to attempted manipulation because of its reliance on few pricing sources. As always, voters should ensure that their results do not differ from broad market consensus. This is meant to be vague as the tokenholders are responsible for defining broad market consensus.



## Security Considerations

Adding these new identifiers by themselves pose little security risk to the DVM or priceless financial contract users. However, anyone deploying a new priceless token contract referencing this identifier should take care to parameterize the contract appropriately to the reference asset’s volatility and liquidity characteristics to avoid the loss of funds for synthetic token holders. Additionally, the contract deployer should ensure that there is a network of liquidators and disputers ready to perform the services necessary to keep the contract solvent.

$UMA-holders should evaluate the ongoing cost and benefit of supporting price requests for these identifiers and also contemplate de-registering these identifiers if security holes are identified. As noted above, $UMA-holders should also consider re-defining these identifiers as liquidity in the underlying asset changes, or if added robustness (e.g. via TWAPs) are necessary to prevent market manipulation.

