# About NERVA
*"Proof-of-work is essentially one-CPU-one-vote" - Satoshi Nakamoto*

**NERVA** (XNV) is a pioneering proof-of-work cryptocurrency designed first and foremost to be truly GPU and FPGA/ASIC resistant. This means that NERVA can be mined, and can only be mined, by standard computer processors - like the one in the device you're using to read this text. NERVA is also intended to be solo-mined only, thus avoiding the hashrate centralisation that plagues so many pool-mined coins. By engineering the network to be as decentralised as possible, NERVA is naturally resistant to 51% attacks and this resistance will only strengthen as the network scales. Through this combination, made possible by the unique Cryptonight Adaptive algoritm, NERVA aims to take crypto back to its roots - away from corporatisation and huge mining farms, and back to the people - whilst incorporating the latest advancements in blockchain technology that allow for a fast, secure, private and untraceable financial network.

Another unique attribute of NERVA is the accelerated emission schedule. NERVA launched in May 2018 and already the majority of the coin supply has been distributed to miners. All 18.4 million coins will be mined within 3 years. After this, like Monero and many other blockchains, a 1% annual tail emission (inflation) will exist to incentivise further mining. Unlike Monero, NERVA will be the first project to validate - or discredit - the reasoning behind this economic model, and by reaching tail emission first, NERVA will be both a test case and a prominent example of how to manage that vital transition.

Finally, NERVA is fostering a strong community of likeminded enthusiasts, and you're invited to join. In time, we hope that NERVA will gain further traction and visibility in the crowded field of cryptocurrencies. While many projects rely on hype and marketing, we're focused on providing the tools for individuals and businesses to integrate NERVA into their services and accept XNV as an alternative payment solution, leveraging the benefits of our fast and stable blockchain. In the end, we may succeed or fail, but NERVA is leading by example in demonstrating the viability of a purer vision of cryptocurrency: a currency of, by and for its users, where anybody with a CPU and an internet connection can actively contrbute to and secure the network, with no centralised entities and no compromises.

You can read more about the NERVA project on the [main website](https://getnerva.org), and you can also join us on [Discord](https://discord.gg/xBHxnGN), [Telegram](https://t.me/NervaXNV) and [Reddit](https://www.reddit.com/r/Nerva/).

### Technical Specifications
* Name: NERVA
* Ticker: XNV
* Hash algorithm: Cryptonight-Adaptive v3 (Proof-Of-Work)
* Difficulty algorithm: LWMA
* Total Supply: 18.44 million coins + tail emission
* Initial Block Reward: 70
* Emission Speed: 18
* Block Time: 60 seconds
* Maturity: 10 blocks
* Premine: ~1% (187,663+ XNV)
* Ring size: 5 (fixed for all transactions)

NERVA forked from Monero and Masari and had its genesis block on May 1st, 2018. The official NERVA blockchain exploer can be found [here](https://explorer.getnerva.org/). Additional network statistics are available on [Freeboard](https://freeboard.io/board/EV5-se) and the [Nodemap](https://nerva.syzygy.cc).

#### Premine
A premine of 180,000 coins was included in the genesis block. This is to finance the infrastructure to run the network for the years to come. Additionally, each of the 3 seed nodes in place at launch were each mining on 1 CPU core, as a temporary measure to establish the network. This results in an effective premine of slightly over 1% of the total supply (pre tail emission).

### CryptoNight-Adaptive
The CryptoNight-Adaptive algorithm was developed for NERVA. The idea is quite simple: create a hash algorithm that changes automatically at regular intervals to break support for ASICS, mining pools, and GPU software, making solo-mining with a daemon and wallet the most efficient viable option. The initial release was based on the Monero v7 algorithm present in the latest Masari hard fork, with a few changes. First, the scratchpad was halved, like CryptoNight Lite. Second, we adapted the ASIC resistant changes from Alloy (XAO) which changes the number of mixing functions at each hard fork to instead change every block. The number of mixing functions is automatically incremented every block and reset every 1024 blocks.

Many improvements have been made since NERVA launched, through several hard forks. A technical overview of CN-A v2 is available [here](https://bitbucket.org/snippets/nerva-project/keG5G8/the-cn-adaptive-v2-algorithm), and is also discussed in this [interview with angrywasp](https://blog.turtlecoin.lol/archives/cn-adaptive-nerva-and-the-quest-for-fair-mining/). These resources are not up to date with the latest improvements, so if you have questions the best place to ask is in the development channel in Discord.

### Solo Mining
NERVA prioritises decentralisation, to which pools are antithetical. Many pool-mined coins are vulnerable to 51% attacks, and pool operators hold excessive influence on the wider network. To avoid these problems, NERVA has sought from day one to be solo mined only.

With solo mining, miner rewards are not as consistent as they often are with pools - depending on hashrate and difficulty, a miner may go unrewarded for an extended period of time. However, as pool rewards are still based on the percentage of hash power contributed, in the long run miners actually earn more from solo mining, due to lack of pool fees. Another potential upside to solo mining is that it incentivises consistent, rather than on/off, mining. This can help to limit sudden swings in the overall network hashrate. Regardless, our primary motivation is to avoid centralization.

It should be noted that whilst it is difficult by design to develop a pool for NERVA, it is not impossible. Should a pool be developed in the future, the community and developer consensus is that a hardfork would then occur to make it ineffectual.

#### Pond Mining (proposed)
Pond Mining is a concept developed by the NERVA community for a peer-to-peer version of pool mining, which would avoid the centralization inherent in traditional mining pools. A [whitepaper](https://github.com/nithronium/pondmining/blob/master/pondmining.pdf) explaining the proposed system was published by community member Nithronium in November 2018.

Pond Mining remains an area of interest but is not currently under active development.

### Commerce
A WooCommerce integration has been developed which allows for online stores to accept payment in NERVA. There are two variants available, [woo-static](https://bitbucket.org/nerva-project/woo-static/) and [woo-dynamic](https://bitbucket.org/nerva-project/woo-dynamic). Documentation for both is available on BitBucket.

If you would like to integrate NERVA as a payment solution or have other suggestions, please join us in the commerce channel on Discord.
