## Headers
UMIP Title: Create KPI Options for SharedStake using SGT as Collateral; Using Price Identifiers for SGT, SGT/ETH UNI LP, and vETH2

Status: Draft

Authors: Warren Muffett and Chimera

Discourse: https://discourse.umaproject.org/t/add-utvl-kpi-sgt-as-a-price-identifier/860

Created: May 16th, 2021

## Summary 
This UMIP enables the DVM to support price requests based on the TVL of SharedStake. 
The DVM should support TVL requests for total vETH2 in existance and the price of SGT. 
A synthetic option is minted against a base collateral, in this case SGT, which expires at 00.00(UTC) December 31th 2021.

## Motivation 
The primary motivation for the development of KPI options is to allow protocols to incentivize Defi users to assist them to reach the 
protocol's identified goals. By leveraging their community resources, SharedStake can increase TVL, strengthen their governance token, 
and share value with their community members.

### SharedStake vETH2 Tracker

Adding this identifier will allow the creation of KPI options that will hopefully have a positive effective on the marketing of the SGT 
token as well as encouraging growth within the SharedStake ecosystem.

Note: It is anticipated that these Price Identifiers will be used to create KPI Options, however it is acknowledged that it may be used for a variety of purposes.

## Rationale
The UMA Protocol would be improved by this proposal, as this can increase the number of KPI Options being created on the UMA protocol and thus increasing the value of assets supported by the DVM. There will also be collaborative benefits between the UMA and SharedStake communities.

## Data Specifications 
### SharedstakeTVLUSD

Price Identifier Name: vETH2Growth

#### Sources and Metricts: 
1. Etherscan: etherscan.io
 	vETH2: https://etherscan.io/token/0x898bad2774eb97cf6b94605677f43b41871410b1?a=0xa1feaf41d843d53d0f6bed86a8cf592ce21c409e
	
2. Uniswap: https://v2.info.uniswap.org/pair/0x3d07f6e1627da96b8836190de64c1aed70e3fc55
	SGT Uniswap Contract: 0x3d07f6e1627da96b8836190de64c1aed70e3fc55

#### Cost To Use: Free
#### Real Time Data Update Frequency: Yes


## Technical Specifications

Price Identifier Name: SharedStakeTVLUSD

Base Currency: SGT, ETH, vETH2 

Quote currency: USD

Intended Collateral Currency: 

Scaling Decimals: 18 (1e18)

Rounding: Round to nearest 8 decimal places (seventh decimal place digit >= 5 rounds up and < 5 rounds down)

vETH2 Minining SGT Rewards can be found here: https://etherscan.io/token/0x898bad2774eb97cf6b94605677f43b41871410b1?a=0xA919D7a5fb7ad4ab6F2aae82b6F39d181A027d35.
 mentioned above


## Implementation

1. Query USD Value of SharedStake vETH2Growth 
2. Round to the nearest 8 decimal places

It should be noted that this identifier is potentially prone to attempted manipulation because of its reliance on few pricing sources. As always, voters should ensure that their results do not differ from broad market consensus. This is meant to be vague as the tokenholders are responsible for defining broad market consensus.



## Security Considerations

Adding these new identifiers by themselves pose little security risk to the DVM or priceless financial contract users. However, anyone deploying a new priceless token contract referencing this identifier should take care to parameterize the contract appropriately to the reference asset’s volatility and liquidity characteristics to avoid the loss of funds for synthetic token holders. Additionally, the contract deployer should ensure that there is a network of liquidators and disputers ready to perform the services necessary to keep the contract solvent.

$UMA-holders should evaluate the ongoing cost and benefit of supporting price requests for these identifiers and also contemplate de-registering these identifiers if security holes are identified. As noted above, $UMA-holders should also consider re-defining these identifiers as liquidity in the underlying asset changes, or if added robustness (e.g. via TWAPs) are necessary to prevent market manipulation.

