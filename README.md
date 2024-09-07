# FUGAZI: The First Fully On-chain Dark Pool on Fhenix

This repository contains the final result of Orakle Dark Pool Team's hard work and is submitted to ETHOnline 2024. Fugazi is the first fully on-chain dark pool built on Fhenix. Unlike many other dark pools, Fugazi does not require any external or centralized entity to work, thanks to the fully homomorphic encryption (FHE) supported by Fhenix Helium Testnet.

## What is Dark Pool?

Dark pools are equity trading systems that do not publicly display orders. They enable large trades to be executed in private manner so that the traders get better price for their orders.

## Why Do We Need Decentralized Dark Pool?

Although the order is not public, the exchange operators can still see user orders and exploit them. This is not a mere conspirancy theory; [See](https://www.sec.gov/newsroom/press-releases/2014-114) [these](https://www.sec.gov/newsroom/press-releases/2016-16) [examples](https://www.sec.gov/newsroom/press-releases/2018-193). This happens because of the trust assumption that the trusted & centralized entity, in this case, exchange operators, will act honestly. Through the trustless and decentralized dark pool, users can benefit from the better execution without risk of being exploited.

## How Does It Work?

Fugazi usese following 3 tools to hide order information:

- FHE
- Batch Execution
- Noise Order

### FHE

Fully Homomorphic Encryption (FHE) is a emerging cryptographic scheme that can process data blindly (i.e., without decryption) without compromising transparency and usability. By leveraging FHE supported by Fhenix's Helium testnet. We store and process user balances, pool reserves, and order information such as size and direction.

### Batch Execution

AMM with encrypted reserves is not enough for privacy. See [following](https://arxiv.org/abs/2103.01193) [studies](https://eprint.iacr.org/2021/1101). TL;DR of these studies are: 1. The attacker can discover user order through sandwiching it, and 2. Batching is inevitable to mitigate such attack vector. Instead of authors original proposal (randomize order priorities then execute them one by one), we clear them in uniform price. We adopted specialized AMM curve for batching, called FM-AMM. FM-AMM itself has another positive effect, too. The more arbitrageurs are trading against our pool, the total cost of pool paying to arbitrageurs decay by 1/N. For more details check [this article](https://ethresear.ch/t/notes-on-the-lvr-of-fm-amm/20151).

### Noise Order

There is one more way of hiding user order from attacker. As introduced in [here](https://arxiv.org/abs/2309.14652). By forcing protocol-owned account to submit randomized order and pay protocol an additional fee for providing privacy, users can hide their order without batching. The additional fee is expected profit of arbitrageur from exploiting the post-trade state. We adopted this method too. Users can set the degree of noise up to 200%. This will be especially useful whenever there are not many orders in given batch.

## Conclusion

We built a dark pool that does not require any external or centralized entity to run. User orders' information is hidden through either batching or adding noise (and pay fee). Our dark pool supports UX that is as same as most of existing DEXs such as Uniswap, Curve, Balancer, etc etc, except users have to wait for a while after submitting order.

## References

### Dark pool in General

[SoK: Privacy-Enhancing Technologies in Finance](https://ia.cr/2023/122)\
[MPC Joins The Dark Side](https://ia.cr/2018/1045)\
[An Efficient Data-Independent Priority Queue and its Application to Dark Pools](https://ia.cr/2023/1014)\
[Optimal Trade Execution in Illiquid Markets](https://doi.org/10.48550/arXiv.0902.2516)\
[Optimal liquidation in dark pools](https://ssrn.com/abstract=2698419)\
[Liquidation in the Face of Adversity: Stealth vs. Sunshine Trading](https://dx.doi.org/10.2139/ssrn.1007014)\
[Do Dark Pools Harm Price Discovery?](https://doi.org/10.1093/rfs/hht078)\
[Diving into dark pools](https://doi.org/10.1111/fima.12395)\
[Regulations of EU Financial Markets](https://global.oup.com/academic/product/regulation-of-the-eu-financial-markets-9780198767671?cc=us&lang=en&)

### Privacy in AMMs

[A Note on Privacy in Constant Function Market Makers](https://doi.org/10.48550/arXiv.2103.01193)\
[Differential Privacy in Constant Function Market Makers](https://eprint.iacr.org/2021/1101)\
[Pricing Personalized Preferences for Privacy Protection in Constant Function Market Makers](https://doi.org/10.48550/arXiv.2309.14652)

### LVR

[Automated Market Making and Loss-Versus-Rebalancing](https://doi.org/10.48550/arXiv.2208.06046)\
[Automated Market Making and Arbitrage Profits in the Presence of Fees](https://doi.org/10.48550/arXiv.2305.14604)\
[The Cost of Permissionless Liquidity Provision in Automated Market Makers](https://doi.org/10.48550/arXiv.2402.18256)
[Notes on the LVR of FM-AMM](https://ethresear.ch/t/notes-on-the-lvr-of-fm-amm/20151)

### Batch Execution

[The High-Frequency Trading Arms Race: Frequent Batch Auctions as a Market Design Response](https://doi.org/10.1093/qje/qjv027)\
[Frequent Batch Auctions and Informed Trading](https://dx.doi.org/10.2139/ssrn.4065547)\
[The Market Quality Effects of Sub-Second Frequent Batch Auctions: Evidence from Dark Trading Restrictions](https://ssrn.com/abstract=4191970)\
[Augmenting Batch Exchanges with Constant Function Market Makers](https://doi.org/10.48550/arXiv.2210.04929)\
[Arbitrageurs' profits, LVR, and sandwich attacks: batch trading as an AMM design response](https://doi.org/10.48550/arXiv.2307.02074)

### Singleton DEXs

[CrocSwap-protocol](https://github.com/CrocSwap/CrocSwap-protocol.git)\
[Muffin](https://github.com/muffinfi/muffin.git)\
[Balancer V2 Monorepo](https://github.com/balancer/balancer-v2-monorepo.git)
