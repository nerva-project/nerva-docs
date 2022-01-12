# About
This page aims to provide information about the NERVA project and answer common questions. For help using the NERVA software, refer to User Guides section. You can also read more about the project on the [main website][nerva-website-link], and join our growing community on [Discord][nerva-discord-link] and [Reddit][nerva-reddit-link].

<hr>

# What is NERVA?
**"Proof-of-work is essentially one-CPU-one-vote" - Satoshi Nakamoto**

**NERVA** (XNV) is a pioneering proof-of-work cryptocurrency designed first and foremost to be truly GPU and FPGA/ASIC resistant. This means that NERVA can be mined competitively by standard computer processors - like the one in the device you're using to read this text. With the Cryptonight Adaptive PoW algorithm, NERVA aims to force miners to keep a local copy of the blockchain, incentivizing miners to run full network nodes. This structure encourages solo mining, and helps avoid the hashrate centralization that plagues pool-mined coins. By engineering the network to be as decentralized as possible, NERVA is naturally resistant to 51% attacks, and this resistance will only strengthen as the network scales. NERVA aims to take crypto back to its roots - away from corporations and huge mining farms, and back to the people - whilst incorporating the latest advancements in blockchain technology that allow for a fast, secure, private and untraceable financial network. 

Another unique attribute of NERVA is the accelerated emission schedule. NERVA launched in May 2018 and by late 2021 the initial supply of 18.4 million coins have already been mined. We believe NERVA is the first Monero clone to reach its tail emission. 

Tail emission means that each block has fixed miner reward of 0.3 XNV.  Currently this is 0.85% annual inflation rate.  That percentage will decrease slightly each year.  Tail emission was programmed to incentivize further mining and replace lost coins (hard drives sometimes crash and keys or seeds printed on a piece of paper sometimes end up at the bottom of the ocean during fishing accidents). 

Tail emission inflation translates to (approximately): 

- 432 new XNV per day
- 3024 new XNV per week
- 13150 new XNV per month
- 157800 new XNV per year

You're invited to join NERVA community of likeminded enthusiasts. In time, we hope that NERVA will gain further traction and visibility in the crowded field of cryptocurrencies. While many projects rely on hype and marketing, we're focused on providing the tools for individuals and businesses to integrate NERVA into their services and accept XNV as an alternative payment solution, leveraging the benefits of our fast and stable blockchain. In the end, we may succeed or fail, but NERVA is leading by example in demonstrating the viability of a purer vision of cryptocurrency: a currency of, by and for its users, where anybody with a CPU and an internet connection can actively contribute to and secure the network, with no centralized entities and no compromises.

<hr>

# Technical Specifications
- Name: NERVA
- Ticker: XNV
- Hash algorithm: Cryptonight-Adaptive (Proof-Of-Work)
- Difficulty algorithm: LWMA
- Total Supply: 18.44 million coins + tail emission (Tail emission started in late 2021)
- Initial Block Reward: 70 XNV
- Current Block Reward: 0.3 XNV
- Emission Speed: 18
- Block Time: 60 seconds
- Maturity: 20 blocks
- Premine: 1.05% (193,861 XNV)
- Ring size: 5 (fixed for all transactions)

NERVA forked from Monero and had its genesis block on May 1st, 2018. The official NERVA blockchain explorer can be found [here][nerva-explorer-link]. Additional network statistics are available on [Freeboard][nerva-freeboard-link] and the [Nodemap][nerva-website-link].

<hr>

# Premine
A premine of 180,000 coins was included in the genesis block. Additionally, each of the 3 seed nodes in place at launch were each mining on 1 CPU core as a temporary measure to establish the network. The first 200 blocks were mined, bringing the total funds in the dev wallet to 193,861 XNV. This results in a premine of slightly over 1% of the total supply (pre tail emission).

<hr>

# CryptoNight-Adaptive
The CryptoNight-Adaptive algorithm was developed for NERVA. The idea is quite simple: create a hash algorithm that changes automatically at regular intervals to break support for ASICS, mining pools, and GPU software, making solo-mining with a daemon and wallet the most efficient viable option. The initial release was based on the Monero v7 algorithm, with a few changes. First, the scratchpad was halved, like CryptoNight Lite. Second, we adapted the ASIC resistant changes from Alloy (XAO) which changes the number of mixing iterations every block. The number of mixing iterations is automatically incremented every block and reset every 1024 blocks.

<hr>

# Solo Mining
NERVA prioritizes decentralization, to which pools are antithetical. Many pool-mined coins are vulnerable to 51% attacks, and pool operators hold excessive influence on the wider network. To avoid these problems, NERVA has sought from day one to be solo mined only.

With solo mining, miner rewards are not as consistent as they often are with pools - depending on hashrate and difficulty, a miner may go unrewarded for an extended period of time. However, as pool rewards are still based on the percentage of hash power contributed, in the long run miners actually earn more from solo mining, due to lack of pool fees. Another potential upside to solo mining is that it incentivizes consistent, rather than on/off, mining. This can help to limit sudden swings in the overall network hashrate. Regardless, our primary motivation is to avoid centralization.

It should be noted that whilst it is difficult by design to develop a pool for NERVA, it is not impossible. Should a pool be developed in the future, the community consensus is that a hardfork would then occur to make it ineffectual.

For more information about mining, refer to the [Mining FAQ](../guides/mining).

<hr>

# Hard Forks
NERVA is a project under constant development. Sometimes, improvements require changes to the core network consensus algorithms; such changes are known as "hard forks". Hard forks occur on older networks, such as Ethereum and Monero, as well as newer ones, like NERVA. By design, hard forks make older software incompatible with newer software. To prevent senseless interference, in addition to consensus-based node banning, NERVA uses optimistic version blocking: every new hard fork version blocks versions prior to the fork.

**Will I lose my coins after a hard fork?** No, there is no need for concern. A hard fork simply upgrades a part of the network software. As soon as you upgrade you will be able to transact again with your account.

**Does a hard fork mean that I will get extra coins, like "NERVA Classic" or "NERVA ABC"?** Technically, your coins still exist on the old chain for as long as (unupdated) nodes are mining it. But no, there is still only one main chain. Hard forks in NERVA do not create chain splits, there is just NERVA.

**What if I sent coins after a hard fork, but didn't upgrade?** Your transaction should be rejected in the upgraded network, returning the funds to your wallet. Once you upgrade to the latest software, you will need to rescan the blockchain from scratch, which will cause your wallet reflect the state of the upgraded network. To rescan the blockchain, simply restore your wallet from a seed phrase.
  
**Will the exchange support this hard fork?** Yes. The developers are in close communication with every exchange that currently lists NERVA. Scheduled forks are communicated well in advance to exchanges to ensure everything goes smoothly.

#### Previous Hard Forks
Here is a summarized history of NERVA network upgrades. More detailed release notes for each were published in Discord.

- **v6**, at block 45000 (1st June 2018). Changed the mined money unlock window from 10 to 20 blocks; introduced LWMA v2 DAA; halved base iteration count in CryptoNight-Adaptive. *Minimum version: 0.1.2.1*
- **v7**, at block 173500 (30th August 2018). Introduced the CryptoNight-Adaptive v2 algorithm. *Minimum version: 0.1.3.4*
- **v8**, at block 180000 (3rd September 2018). Introduced BulletProofs v1; resolved stability issues in CryptoNight-Adaptive v2. *Minimum version: 0.1.3.5*
- **v9**, at block 240500 (15th October 2018). Iterated CryptoNight-Adaptive to v3. *Minimum version: 0.1.4.0*
- **v10**, at block 341000 (24th December 2018). Iterated CryptoNight-Adaptive to v4. *Minimum version: 0.1.5.4*
- **v11**, at block 500000 (13th April 2019). Introduced BulletProofs v2; iterated CryptoNight-Adaptive to v5. *Minimum version: 0.1.6.4*
- **v12**, at block 930000

<hr>

# Commerce
A WooCommerce integration has been developed which allows for online stores to accept payment in XNV. There are two variants available, [woo-static][nerva-woo-static-link] and [woo-dynamic][nerva-woo-dynamic-link]. Documentation for both is available on GitHub.

We are actively working on integrating NERVA with other platforms and services to further its usage as a currency. If you would like to integrate NERVA as a payment solution or have other suggestions, please join us on [Discord][nerva-discord-link].

<hr>

# Exchanges
XNV is currently traded on [TradeOgre][tradeogre-link].

<hr>

# Motivations
*(Edited extracts from ["CN Adaptive, Nerva, and the Quest For Fair Mining"][turtlecoin-angry-interview-link], an interview with ex-NERVA lead developer angrywasp)*

#### Decentralization
Centralization is a major concern for all coins and the consequences are many. Giving too much hash power to an individual or pool increases the risk of a 51% attack, where a blockchain can be hijacked and manipulated for malicious purposes, and increases the fragility of the network. If a pool holding for example 65% of a network hashrate goes down, the network instantly loses 65% of it’s hash power, struggling to validate transactions and progress the blockchain. This results in increased transaction times and difficulty in making transactions, causing a loss of reputation to the coin and frustration to end users.

Centralization of hash power also causes the concentration of coin distribution. The vast majority of Cryptonight ASICs are operated by only a few companies in the business of making these devices. As a result, coins mined by ASICs are funneling the vast majority of their block rewards to only a small group of people, effectively centralizing price and distribution control and promoting cartel-like behavior among these individuals.

NERVA's model of solo CPU mining maximizes hashrate distribution, making it much more difficult for any one entity to obtain the 51% threshold to stage an attack against the coin, while minimizing the network disruption caused by nodes going off the network. Distributed hashrate also has the added advantage of more equitable coin distribution creating a fairer system for all participants.

#### Environmental Sustainability
E-waste has in recent times become a major global concern. From the mining of heavy metals such as copper, to PCB and component manufacturing processes, to device assembly, pollution is present. Furthermore, the incorrect disposal of electronics results in heavy metal contamination to local environments and adverse health effects to the wildlife in these environments, as well as adverse health effects on the people responsible for the production and disposal of electronics.

Through NERVA's ability to be mined on older hardware and by removing the ability of dedicated hardware to mine the coin, NERVA mitigates the issue of E-waste. Furthermore, scaling up hashing power in a CPU mined coin is more expensive, creating a financial disincentive for users to buy hardware specifically to mine NERVA, further mitigating the impact of E-waste on the environment.

Anyone looking at the evolution of crypto mining over the last few years will have noticed an explosion in the amount of hardware being used for mining. GPUs have been sold out around the world and ASICs are being rolled out in increasing batches. This is creating an endless cycle of buying more hashpower to compete with other miners for coins. Besides the E-waste considerations previously discussed, we have witnessed an explosion in the amount of electricity being used to power these networks. It is estimated that Bitcoin alone uses more electricity than the entire Republic of Ireland. That is far from sustainable. NERVA on the other hand can be mined on the spare cores of your computer, with only a minimal increase in the amount of electricity being used by a device that would otherwise likely be running anyway.

#### The Spirit of Satoshi
It is worth remembering why crypto was developed in the first place. The driving force was to create a currency that could be used by anyone, anywhere, anytime. Across borders, free from taxation, unrestricted by government regulation and monetary policy. What people have been thinking all along, that banks are deceiving and stealing from the public, has been proven to be true. We need something else as a society. We need a way to transact with each other that bypasses the greed and corruption of the banking sector, and a way to manage our own money that no government or corporation can lay their hands on.

Somewhere along the way, that vision was lost. Again, corrupted by greed and held at the mercy of an elite few. Only this time it isn’t governments and bankers, it’s wealthy ASIC manufacturers and rented hash services. Hijacking blockchains, stealing coins and controlling their price and distribution through excessive hash rates, well beyond what the average person is capable of. Once again, we have a select few corporations making massive profits acting in unethical ways to deceive and steal from the public.

With NERVA, our aim is to take back control and show that a cryptocurrency is successful if it is owned by everyone. And by making it CPU mineable, we give everyone who owns a computer a chance to collect their own coins. NERVA is dedicated to existing in the same spirit in which cryptocurrency began.

#### Challenging Conventional Wisdom
Finally, there is some conventional wisdom we aim to challenge and some lingering questions that need to be answered if crypto is to have any future at all. It is said that a coin needs a large hashrate to be stable. NERVA challenges this wisdom: we believe that a stable coin comes from a highly distributed network and mitigation of network hashrate variance, by means of a robust difficulty adjustment algorithm and resistance to dedicated mining hardware. Monero, and by extension all Monero clones, also have a limited lifespan for mining. Every Cryptonote coin is on a ticking clock. One day, the Proof of Work will be done and the entire coin supply will be emitted. At which point the coin enters its ‘tail emission’ where only a very small proportion of the coin's original block reward is released to provide some inflation and replace lost coins.

So if conventional wisdom dictates that block rewards keep miners incentivized to mine, and a blockchain relies on mining, then what happens when those block rewards are gone? Nobody exactly knows, because no Cryptonote coin has yet exhausted its emission. From a market and coin value perspective, what happens to the price once the selling pressure applied by miners is gone? Again, we just don’t know. But we need to know.

NERVA's unique approach to mining is important, but mining is only part of a coin’s life. We need to be able to peek through that looking glass and see what’s on the other side. For this reason, NERVA will hit its tail emission just 3 years from the first block, around 1 May 2021. This will make NERVA a prominent first example of how cryptocurrencies make that transition and what is to be expected in the life after mining.




<!--Reference links -->
[nerva-website-link]: https://nerva.one/
[nerva-explorer-link]: https://explorer.nerva.one/

[nerva-discord-link]: https://discord.gg/ufysfvcFwe
[nerva-reddit-link]: https://www.reddit.com/r/Nerva/
[nerva-freeboard-link]: https://freeboard.io/board/EV5-se

[nerva-woo-static-link]: https://github.com/nerva-project/woo-static
[nerva-woo-dynamic-link]: https://github.com/nerva-project/woo-dynamic

[tradeogre-link]: https://tradeogre.com/exchange/BTC-XNV

[turtlecoin-angry-interview-link]: https://blog.turtlecoin.lol/archives/cn-adaptive-nerva-and-the-quest-for-fair-mining/