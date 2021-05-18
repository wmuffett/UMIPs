UMIP Title: Create KPI Options for SharedStake using SGT as Collateral; Using Price Identifiers for SGT, SGT/ETH UNI LP, and vETH2
Status: Draft
Authors: Warren Muffett and Chimera
Discourse: https://discourse.umaproject.org/t/add-utvl-kpi-sgt-as-a-price-identifier/860
Created: May 16th, 2021

## Summary 
Key Performance Indicator (KPI) options are synthetic tokens that redeem to protocol tokens with the redemption value determined by 
performance against that indicator. One example of a KPI is Total Value Locked (TVL).
This UMIP enables the DVM to support price requests based on the TVL of SharedStake.
A synthetic option is minted against a base collateral, in this case SGT, which expires at 00.00(UTC) December 31th 2021.
Options are redeemed on the basis of TVL/10^9, with a floor of 0.1 and a ceiling of 2.

## Motivation 
The primary motivation for the development of KPI options is to allow protocols to incentivize Defi users to assist them to reach the 
protocol's identified goals. By leveraging their community resources, SharedStake can increase TVL, strengthen their governance token, 
and share value with their community members.

##Benefits
## Benefits FAQ

1. 	What are the financial positions enabled by creating this synthetic that do not already exist?
	
	a. 	This synthetic token will allow the creation of tokens which expire to a set rate of the collateral asset tokens based on a pre-identified bounded ratio as determined by the TVL of the protocol at the time of expiry.
	
2. 	Provide an example of a person interacting with a contract that uses this price identifier.
	
	a. 	SharedStake may leverage its community and/or its reputation by minting TVL Options for its token which can be redeemed to a token amount as determined by the TVL of the protocol at the expiry point.
	
	b. 	A protocol community member, token holder, voter, or proximal DeFi protocol participant may be gifted a TVL option by a protocol as an incentive to build the TVL of the protocol within the option timeframe and redeem at expiry.
	
	c. 	Any user may purchase a TVL Option for a protocol that they believe has the potential for growth in TVL prior to expiry.
	
3. 	What is SharedStake’s current TVL?

	a. 	The current TVL of SharedStake is approximately $37m as at 4:30 UTC 19th April 2021.

## Markets and Data Sources
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

## Rationale

This synthetic is designed as an incentivization mechanism to leverage the SGT community, our partners and the wider DeFi userbase to grow our protocol as measured by our identified Key Performance Indicator of Total Value Locked.

This price identifier offers a guarantee that these options will be of value, even if this key metric is poor through the floor price mechanism, however the nature of SGT is such that the amount of value that can be locked in the protocol is potentially limitless and consequently a ceiling price is required to limit provide a cap.

## How do Voters recieve the price vETH2 at the time of expiration?
It is assumed for this purpose that 1 vETH2 = 1 ETH. Refererence UMIP 6.

## How do Voters receive the price of SGT at the time of expiration?
SGT is a Native ERC-20, will use the deepest Dex Pool for the most accurate price. As of now that is Uniswap, with over 2million in Liquidity:
0x3d07f6e1627da96b8836190de64c1aed70e3fc55.

## How do Voters receive the price of LPs used in TVL calculations? 
Reference UMIP 59 and the contract addresses that were provided above. 

## What type of post processing is needed on the dolar value queried from each of the referenced pools?
Up to the SGT team to try to keep an accurate TVL measurement throughout the life of KPI Options?

## Options Details
Total Collateral Used?

## Security Considerations

1. 	Where could manipulation occur?

	a) 	Negligible opportunities for manipulation.

2. 	How could this price ID be exploited?

	a) 	It is possible that as expiry approaches, a user may be able to purchase a large number of TVL option on the open market, should the TVL be significantly below the level required to achieve the ceiling level, then add large amounts of collateral to an SharedStake contract slightly before expiry to temporarily drive up the TVL, redeem the synthetic tokens, then withdraw the collateral immediately afterwards.

	b) 	It is possible that a user may purchase uTVL_KPI_SGT at a low price, lock substantial amounts of collateral in SharedStake contracts causing the uTVL_KPI_SGT price to rise, then sell these tokens at a profit and immediately withdraw the collateral from the contracts.

3. 	Do the instructions for determining the price provide people with enough certainty?
	
	a) 	YES

4. 	What are current or future concern possibilities with the way the price identifier is defined?

	a) 	It is possible that price identifiers for collateral types may be altered prior to expiry

	b) 	It is possible that collateral types may be removed. This would not impact on this PI as they would not feature in any relevant contract.

