UMIP Title: Create KPI Options for SharedStake using SGT as Collateral; Using Price Identifiers for SGT, SGT/ETH UNI LP, and vETH2
Status: Draft
Authors: Warren Muffett and Chimera
Discourse: https://discourse.umaproject.org/t/add-utvl-kpi-sgt-as-a-price-identifier/860
Created: May 16th, 2021

##Summary 
Key Performance Indicator (KPI) options are synthetic tokens that redeem to protocol tokens with the redemption value determined by 
performance against that indicator. One example of a KPI is Total Value Locked (TVL).
This UMIP enables the DVM to support price requests based on the TVL of SharedStake.
A synthetic option is minted against a base collateral, in this case SGT, which expires at 00.00(UTC) December 31th 2021.
Options are redeemed on the basis of TVL/10^9, with a floor of 0.1 and a ceiling of 2.

##Motivation 
The primary motivation for the development of KPI options is to allow protocols to incentivize Defi users to assist them to reach the 
protocol's identified goals. By leveraging their community resources, SharedStake can increase TVL, strengthen their governance token, 
and share value with their community members.

##Markets and Data Sources
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

##How do Voters recieve the price vETH2 at the time of expiration?
It is assumed for this purpose that 1 vETH2 = 1 ETH. Refererence UMIP 6.

#How do Voters receive the price of SGT at the time of expiration?
SGT is a Native ERC-20, will use the deepest Dex Pool for the most accurate price. As of now that is Uniswap, with over 2million in Liquidity:
0x3d07f6e1627da96b8836190de64c1aed70e3fc55.

##How do Voters receive the price of LPs used in TVL calculations? 
Reference UMIP 59 and the contract addresses that were provided above. 

#What type of post processing is needed on the dolar value queried from each of the referenced pools?
Up to the SGT team to try to keep an accurate TVL measurement throughout the life of KPI Options?

##Options Details
Total Collateral Used?

How to value the options as TVL increases?

Will work on these with the team.

