[category]: <> (General,Blockchains)
[date]: <> (2024/03/28)
[title]: <> (Ethereum has blobs. Where do we go from here?)
[pandoc]: <> ()

* | March 13,
  * Dencun hard fork activated,
    * enable 
      * ⭐️proto-danksharding⭐️or blobs
        * == Ethereum's feature -- EIP-4844 -- 
        * ->
          * | INITIALLY,
            * 👀reduced rollups' transaction fees -100x👀
              * == blobs == nearly free
          * | LAST day,
            * blobs spiked up in volume -> fee market activate == `blobscriptions` protocol started to use them
            * == blobs 
              * NOT free
              * vs `calldata`
                * much cheaper 

![](../images/blobs/blobscriptions.png)

* Left
  * | final
    * blob usage spiking up -- , thanks to Blobscriptions, to -- 3-per-block target

* Right
  * | final
    * blob fees enters "price discovery mode"
  * [Source](https://dune.com/0xRob/blobs)

* blobs
  * -> ⚠️Ethereum scaling "zero-to-one" problem -- become to -- "one-to-N" problem ⭐️
    * "zero-to-one"
      * how to scale INITIALLY Ethereum?
    * "one-to-N"
      * scaling ALREADY in place
      * how far away can it scale?
  * ❌NOT gives us data availability sampling❌
    * Reason: 🧠ONLY  set up the basic scaffolding / 
      * | FUTURE, WITHOUT involving users or applications,
        * data availability sampling can be introduced
        * blob count can be increased 🧠

* FUTURE 
  * scaling research
    * how to increase blob count (== capacity)?
      * _Example:_ FULL vision of data availability sampling : 16 MB / slot of data space
    * improve rollups' ability
      * == improve use of data space
  * critical points
    * problems -- closer to the -- application layer

* goal of this post
  * 👀where does Ethereum go from here?👀

## The future of Ethereum scaling

* | LAST FEW years,
  * 👀Ethereum == [L2-centric ecosystem](https://ethereum-magicians.org/t/a-rollup-centric-ethereum-roadmap/4698)👀
    * MAJOR applications have moved over from L1 -- to -> L2,
    * payments L2-based by default,
    * wallets start to build their UI | NEW MULTI-L2 environment

* rollup-centric roadmap
  * 's goal
    * 💡special section of space | block💡 / 
      * ❌EVM would NOT have access to❌ -> it can be
        * broadcasted separately -- from a -- block 
        * verified separately -- , via [data availability sampling](https://www.paradigm.xyz/2022/08/das), from a -- block
      * hold data -- for -- L-2 projects (_Example:_ rollups)
  
* [data availability sampling](https://www.paradigm.xyz/2022/08/das)
  * node verifies -- , 💡by ONLY RANDOMLY checking a FEW SMALL samples💡, -- the data was correctly published 
    * == ONLY download FEW SMALL samples
    * -> 👀blob space could be GREATELY expanded👀
      * goal == 16 MB / slot (~1.33 MB / second)

  ![](../images/blobs/das.png)

### Bringing DAS to life

* [PeerDAS](https://ethresear.ch/t/peerdas-a-simpler-das-approach-using-battle-tested-p2p-components/16541)
  * 💡== simplified version of DAS💡
  * 's node
    * 👀downloads & stores ALL blob data's significant fraction (_Example:_ 1/8)👀   
      * -> ⭐️scale blobs⭐️
        * _Example:_ if nodes ONLY store 1/8 -> scale by 4x 
          * 8x / 2 = 4x
            * 2 -- due to -- redundancy of erasure coding
    * maintain connections -- to -- MANY peers | p2p network
    * 👀if it needs to sample / particular piece of data -> asks one of the peers / is responsible for storing that piece👀
  * 👀| time, can be rolled out👀
    * == professional stakers / download FULL blobs + solo stakers / download 1/8 of the data

<center><br>

![](../images/blobs/peerdas.png)

</center><br>

* [EIP-7623](https://eips.ethereum.org/EIPS/eip-7623) or [2D pricing](https://ethresear.ch/t/multidimensional-eip-1559/11651) (ALTERNATIVE)
  * put stricter bounds | execution block's maximum size (== "regular transactions" | block)
    * -> safer to increase blob target & L1 gas limit

* [2D DAS protocols](https://ethresear.ch/t/from-4844-to-danksharding-a-path-to-scaling-ethereum-da/18046)
  * MORE complicated
  * | longer term,
    * enable increase blob space further

### Improving L2s

There are four key places in which layer 2 protocols today can improve.

**1. Using bytes more efficiently with data compression**

<center><br>

![](../../../../images/blobs/compression.png)

</center><br>

My outline-in-a-picture of data compression [continues to be available here](https://twitter.com/VitalikButerin/status/1554983955182809088);

Naively, a transaction takes up around 180 bytes of data. However, there are a series of compression techniques [that can be used](https://twitter.com/VitalikButerin/status/1554983955182809088) to bring this size down over several stages; with optimal compression we could potentially go all the way down to under 25 bytes per transaction.

**2. Optimistic data techniques that secure L2s by only using the L1 in exceptional situations**

<center><br>

![](../../../../images/blobs/plasmacash.png)

</center><br>

[Plasma](https://vitalik.eth.limo/general/2023/11/14/neoplasma.html) is a category of techniques that allows you to get rollup-equivalent security for some applications while keeping data on L2 in the normal case. For EVMs, plasma can't protect _all_ coins. But Plasma-inspired constructions can protect _most_ coins. And constructions much simpler than Plasma can improve greatly on the [validiums](https://docs.zksync.io/zk-stack/concepts/validiums.html) of today. L2s that are not willing to put all of their data on-chain should explore such techniques.

**3. Continue improving on execution-related constraints**

Once the Dencun hard fork activated, making rollups set up to use the blobs that it introduced 100x cheaper. usage on the [Base](https://www.base.org/) rollup spiked up immediately:

<center><br>

![](../../../../images/blobs/base.png)

</center><br>

This in turn led to Base hitting its own internal gas limit, causing [fees to unexpectedly surge](https://cryptorank.io/news/feed/408a0-coinbases-base-network-struggles-with-surge-in-traffic-and-rising-fees). This has led to a more widespread realization that Ethereum data space is not the only thing that needs to be scaled: rollups need to be scaled internally as well.

Part of this is parallelization; rollups could implement [something like EIP-648](https://github.com/ethereum/EIPs/issues/648). But just as important is [storage](https://vitalik.eth.limo/general/2021/05/23/scaling.html#storage), and [interaction effects](https://vitalik.eth.limo/general/2021/05/23/scaling.html#interaction-effects) between compute and storage. This is an important engineering challenge for rollups.

**4. Continue improving security**

We are still far from a world where rollups are truly protected by code. In fact, according to [l2beat](https://l2beat.com/scaling/summary?sort-by=stage&sort-order=desc#active) only these five, of which only Arbitrum is full-EVM, have even reached [what I have called "stage 1"](https://ethereum-magicians.org/t/proposed-milestones-for-rollups-taking-off-training-wheels/11571).

<center><br>

![](../../../../images/blobs/l2beat.png)

</center><br>

This needs to be tackled head-on. While we are not currently at the point where we can be confident enough in the complex code of an optimistic or SNARK-based EVM verifier, we are absolutely at the point where we can go _halfway there_, and have security councils that can revert the behavior of the code only with a high threshold (eg. I proposed 6-of-8; Arbitrum is doing 9-of-12).

**The ecosystem's standards need to become stricter: so far, we have been lenient and accepted any project as long as it claims to be "on a path to decentralization". By the end of the year, I think our standards should increase and we should only treat a project as a rollup if it has actually reached at least stage 1**.

After this, we can cautiously move toward stage 2: a world where rollups truly are backed by code, and a security council can only intervene if the code "provably disagrees with itself" (eg. accepts two incompatible state roots, or two different implementations give different answers). One path toward doing this safely is to [use multiple prover implementations](https://www.youtube.com/watch?v=6hfVzCWT6YI).

## What does this mean for Ethereum development more broadly?

In [a presentation at ETHCC in summer 2022](https://www.youtube.com/watch?v=kGjFTzRTH3Q), I made a presentation describing the current state of Ethereum development as an S-curve: we are entering a period of very rapid transition, and after that rapid transition, development will once again slow down as the L1 solidifies and development re-focuses on the user and application layer.

<center><br>

![](../../../../images/blobs/scurve.png)

</center><br>

**Today, I would argue that we are decidedly on the decelerating, right side of this S-curve**. As of two weeks ago, the two largest changes to the Ethereum blockchain - the switch to proof of stake, and the re-architecting to blobs - are behind us. Further changes are still significant (eg. [Verkle trees](https://verkle.info/), [single-slot finality](https://ethereum.org/fil/roadmap/single-slot-finality/), [in-protocol account abstraction](https://www.erc4337.io/)), but they are not _drastic to the same extent_ that proof of stake and sharding are. In 2022, Ethereum was like a plane replacing its engines mid-flight. In 2023, it was replacing its wings. The Verkle tree transition is the main remaining truly significant one (and we already have testnets for that); the others are more like replacing a tail fin.

The goal of EIP-4844 was to make a single large one-time change, in order to set rollups up for long-term stability. Now that blobs are out, a future upgrade to full danksharding with 16 MB blobs, and even switching the cryptography over to [STARKs over a 64-bit goldilocks field](https://blog.icme.io/small-fields-for-zero-knowledge/), can happen without requiring any further action from rollups and users. It also reinforces an important precedent: that the Ethereum development process executes according to a long-existing well-understood roadmap, and applications (including L2s) that are built with "the new Ethereum" in mind get an environment that is stable for the long term.

## What does this mean for applications and users?

The first ten years of Ethereum have largely been a training stage: the goal has been to get the Ethereum L1 off the ground, and applications have largely been happening within a small cohort of enthusiasts. Many have argued that the lack of large-scale applications for the past ten years proves that crypto is useless. I have always argued against this: pretty much every crypto application that is not financial speculation depends on low fees - and so while we have high fees, we should not be surprised that we mainly see financial speculation!

Now that we have blobs, this key constraint that has been holding us back all this time is starting to melt away. Fees are finally much lower; my statement from seven years ago that [the internet of money should not cost more than five cents per transaction](https://www.forbesindia.com/article/crypto-made-easy/vitalik-buterin-says-transaction-fees-need-to-reduce-to-5-cents-to-remain-acceptable/75941/1) is finally [coming true](https://l2fees.info/). We are not entirely out of the woods: fees may still increase if usage grows too quickly, and we need to continue working hard to scale blobs (and separately scale rollups) further over the next few years. But we are seeing the light at the end of the... err..... dark forest.

<center><br>

![](../../../../images/blobs/l2fees.png)

</center><br>

What this means to developers is simple: **we no longer have any excuse**. Up until a couple of years ago, we were setting ourselves a low standard, building applications that were clearly not usable at scale, as long as they worked as prototypes and were reasonably decentralized. **Today, we have all the tools we'll need, and indeed most of the tools we'll ever have, to build applications that are simultaneously [cypherpunk](https://vitalik.eth.limo/general/2023/12/28/cypherpunk.html) and user-friendly**. And so we should go out and do it.

Many are rising to the challenge. The Daimo wallet is explicitly describing itself as [Venmo on Ethereum](https://www.youtube.com/watch?v=7O4Zp_nuRyg), aiming to combine Venmo's convenience with Ethereum's decentralization. In the decentralized social sphere, Farcaster is doing a good job of combining genuine decentralization (eg. see [this guide](https://blog.thirdweb.com/guides/build-a-farcaster-client/) on how to build your own alternative client) with excellent user experience. Unlike the previous hype waves of "social fi", the average Farcaster user is not there to gamble - passing the key test for a crypto application to truly be sustainable.

<center><br>

![](../../../../images/blobs/farcaster.png)

<small><i>This post was sent on the main Farcaster client, [Warpcast](https://warpcast.com/), and this screenshot was taken from the alternative Farcaster + [Lens](https://www.lens.xyz/) client [Firefly](https://firefly.mask.social/).</i></small>

</center><br>

These are successes that we need to build on, and expand to other application spheres, including identity, reputation and governance.

## Applications built or maintained today should be designed with 2020s Ethereum in mind

The Ethereum ecosystem still has a large number of applications that operate around a fundamentally "2010s Ethereum" workflow. Most ENS activity is still on layer 1. Most token issuance happens on layer 1, without serious consideration to making sure that bridged tokens on layer 2s are available (eg. see [this fan of the ZELENSKYY memecoin]((https://warpcast.com/alik.eth/0xcbacdd43)) appreciating the coin's ongoing donations to Ukraine but complaining that L1 fees make it too expensive). In addition to scalability, we are also behind on privacy: [POAPs](https://poap.xyz/) are all publicly on-chain, probably the right choice for some use cases but very suboptimal for others. Most DAOs, and [Gitcoin Grants](https://grants.gitcoin.co/), still use fully transparent on-chain voting, making them [highly vulnerable to bribery](https://vitalik.eth.limo/general/2019/04/03/collusion.html) (including retroactive airdrops), and this has been shown to heavily distort contribution patterns. Today, ZK-SNARKs have existed for years, and yet many applications still have not even started to [properly use them](https://vitalik.eth.limo/general/2022/06/15/using_snarks.html).

These are all hard-working teams that have to handle large existing user bases, and so I do not fault them for not simultaneously upgrading to the latest wave of technology. But soon, this upgrading needs to happen. Here are some key differences between "a fundamentally 2010s Ethereum workflow" and "a fundamentally 2020s Ethereum workflow":

<br>

|  | 2010s Ethereum | 2020s Ethereum |
| - | - | - |
| Architecture | Build everything on L1 | Build on a specific L2, or architect the application so that it supports every L2 that follows some standards |
| Privacy | Everything public | A user's data is private by default, users merkle-prove or ZK-prove specific claims as needed to establish trust |
| Anti-sybil | You must have 0.01 ETH | Application can require an ETH deposit, but clients should offer wrappers for non-crypto users that provide "centralized anti-sybil" (eg. SMS) |
| Wallets | EOAs | Account abstraction wallets: [key recovery](https://vitalik.eth.limo/general/2021/01/11/recovery.html), different access control for different  security levels, sponsored txs... |
| Proof of community membership (for voting, airdrops...) | Based on how much ETH you have | ETH + proof of personhood + POAPs + ZuStamps + [EAS](https://attest.sh/) + third party curated lists (eg. [Starknet's solo staker list](https://github.com/starknet-io/provisions-data/tree/main/eth)) |

<br>

Basically, **Ethereum is no longer just a financial ecosystem**. It's a full-stack replacement for large parts of "centralized tech", and even provides some things that centralized tech does not (eg. governance-related applications). And we need to build with this broader ecosystem in mind.

## Conclusions

* Ethereum is in the process of a decisive shift from a "very rapid L1 progress" era to an era where L1 progress will be still very significant, but somewhat more mellow, and less disruptive to applications.
* We still need to finish scaling. This work will be more in-the-background, but it remains important.
* Application developers are no longer building prototypes; we are building tools for many millions of people to use. Across the ecosystem, we need to fully readjust mindsets accordingly.
* Ethereum has upgraded from being "just" a financial ecosystem into a much more thorough independent decentralized tech stack. Across the ecosystem, we need to fully readjust mindsets accordingly to this too.
