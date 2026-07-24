# About
This page aims to provide information about the NERVA project and answer common questions. For help using the NERVA software, refer to User Guides section. You can also read more about the project on the [main website][nerva-website-link], and join our growing community on [Discord][nerva-discord-link].

<hr>

# What is NERVA?
**"Proof-of-work is essentially one-CPU-one-vote" - Satoshi Nakamoto**

**NERVA** (XNV) is a privacy-focused proof-of-work cryptocurrency designed first and foremost to be truly GPU and FPGA/ASIC resistant. This means that NERVA can be mined competitively by standard computer processors - like the one in the device you're using to read this text. With the CryptoNight-Adaptive PoW algorithm, NERVA ties mining to a local copy of the blockchain, requiring miners to run full network nodes. This structure encourages solo mining, and helps avoid the hashrate centralization that plagues pool-mined coins. By engineering the network to be as decentralized as possible, NERVA is naturally resistant to 51% attacks, and this resistance will only strengthen as the network scales. NERVA aims to take crypto back to its roots - away from corporations and huge mining farms, and back to the people - whilst incorporating the latest advancements in blockchain technology that allow for a fast, secure, private and untraceable financial network.

NERVA remains under active development. Its most recent network upgrade, hard fork v13 in July 2026, introduced CryptoNight-Adaptive v6, a significant strengthening of the mining algorithm described further below.

Another unique attribute of NERVA is the accelerated emission schedule. NERVA launched in May 2018 and by early 2021 the initial supply of roughly 18.4 million coins had already been mined. We believe NERVA is the first Monero clone to reach its tail emission. 

Tail emission means that each block has fixed miner reward of 0.3 XNV.  Currently this is 0.82% annual inflation rate.  That percentage will decrease slightly each year.  Tail emission was programmed to incentivize further mining and replace lost coins (hard drives sometimes crash and keys or seeds printed on a piece of paper sometimes end up at the bottom of the ocean during fishing accidents). 

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
- Hash algorithm: CryptoNight-Adaptive v6 (Proof-of-Work)
- Difficulty algorithm: LWMA
- Total Supply: 18.44 million coins + tail emission (tail emission reached in early 2021)
- Initial Block Reward: 70 XNV
- Current Block Reward: 0.3 XNV
- Emission Speed: 18
- Block Time: 60 seconds
- Maturity: 20 blocks
- Premine: 1% (180,000 XNV)
- Ring size: 5 (fixed for all transactions)
- Latest hard fork: v13 at block 4,320,000 (July 2026)
- Current version: 0.3.0.0 "Legacy Remade"

NERVA forked from Monero and had its genesis block on May 1st, 2018. The official NERVA blockchain explorer can be found [here][nerva-explorer-link]. Additional network statistics are available on [Nodemap][nerva-nodemap-link].

<hr>

# Premine
A premine of 180,000 coins, roughly 1% of the total supply, was included in the genesis block and went to NERVA's original creator, who stepped away from the project in 2021. NERVA has been run entirely by its community ever since.

<hr>

# CryptoNight-Adaptive
The CryptoNight-Adaptive algorithm was developed for NERVA. The idea is quite simple: create a hash algorithm that changes automatically to break support for ASICs, mining pools, and GPU software, making solo-mining with a daemon and wallet the most efficient viable option. The initial release was based on the Monero v7 algorithm, with a few changes. First, the scratchpad was halved, like CryptoNight Lite. Second, we adapted the ASIC resistant changes from Alloy (XAO) which changes the number of mixing iterations every block. The number of mixing iterations was automatically incremented every block and reset every 1024 blocks.

### CryptoNight-Adaptive v6 (hard fork v13, July 2026)
Hard fork v13 replaced the mixing-iteration approach with a substantially stronger design, CryptoNight-Adaptive v6. Rather than tweaking a fixed hashing routine, each block now derives a random program that a small virtual machine executes against an 8 MB scratchpad. Three properties work together:

- **Random per-block program**: every block generates a fresh sequence of instructions, so there is no fixed circuit for an ASIC or FPGA to bake into silicon.
- **8 MB scratchpad with dependent memory access**: memory reads chain off previous results (pointer chasing), making the work memory-latency-bound. This kills the parallelism GPUs rely on and keeps per-core hashrate even across ordinary CPUs.
- **Blockchain-seed dependency**: the program seed is tied to chain data, so a miner must hold a local copy of the blockchain to mine. This is what makes pool mining architecturally impractical.

The net effect is that a typical CPU is competitive, and specialized or centralized mining gains little advantage. Because the v6 hash is heavier than the previous algorithm, network hashrate figures after the fork are far lower in raw H/s while security is unchanged; difficulty simply readjusts to the new algorithm.

You can read more about our algo on [CryptUnit website][nerva-cryptunit-algo-link].

<hr>

# Solo Mining
NERVA prioritizes decentralization, to which pools are antithetical. Many pool-mined coins are vulnerable to 51% attacks, and pool operators hold excessive influence on the wider network. To avoid these problems, NERVA has sought from day one to be solo mined only.

With solo mining, miner rewards are not as consistent as they often are with pools - depending on hashrate and difficulty, a miner may go unrewarded for an extended period of time. However, as pool rewards are still based on the percentage of hash power contributed, in the long run miners actually earn more from solo mining, due to lack of pool fees. Another potential upside to solo mining is that it incentivizes consistent, rather than on/off, mining. This can help to limit sudden swings in the overall network hashrate. Regardless, our primary motivation is to avoid centralization.

More importantly, pool resistance in NERVA is enforced at the protocol level, not just by policy. The PoW algorithm pulls parameters directly from blockchain data, so a miner cannot participate without a full node. A pool server cannot generate those parameters without running a full node itself, and even if it did, each "pool member" would still need their own chain access to verify and submit work, functionally collapsing back to solo mining. In short, it is not just a rule against pools: the algorithm physically requires blockchain data that only a full node has, making pool-style work distribution impossible without every participant running a full node, at which point they are simply solo mining anyway.

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
- **v12**, at block 930000 (6th February 2020). Added blockchain pruning and long-term block weighting to prevent rapid block size fluctuations from spam attacks, plus full DNS support for dynamic seed node changes and update notifications. *Minimum version: 0.1.7.1 (Kakapo)*
- **v13**, at block 4320000 (21st July 2026). Introduced CryptoNight-Adaptive v6: each block runs a random program over an 8 MB scratchpad with dependent memory access and a blockchain-seed dependency, hardening the network against ASIC, FPGA, GPU and pool mining. *Minimum version: 0.3.0.0 "Legacy Remade"*

<hr>

# Commerce
A WooCommerce integration has been developed which allows for online stores to accept payment in XNV. There are two variants available, [woo-static][nerva-woo-static-link] and [woo-dynamic][nerva-woo-dynamic-link]. Documentation for both is available on GitHub.

We are actively working on integrating NERVA with other platforms and services to further its usage as a currency. If you would like to integrate NERVA as a payment solution or have other suggestions, please join us on [Discord][nerva-discord-link].

<hr>

# Exchanges
XNV is traded on several exchanges. For the current list, see the [Exchanges section][nerva-exchanges-link] on the NERVA website.

<!--Reference links -->
[nerva-website-link]: https://nerva.one/
[nerva-explorer-link]: https://explorer.nerva.one/
[nerva-nodemap-link]: https://map.nerva.one/

[nerva-discord-link]: https://discord.gg/ufysfvcFwe

[nerva-woo-static-link]: https://github.com/nerva-project/woo-static
[nerva-woo-dynamic-link]: https://github.com/nerva-project/woo-dynamic

[nerva-exchanges-link]: https://nerva.one/#exchanges

[nerva-cryptunit-algo-link]: https://www.cryptunit.com/algo/CNAdaptive