# Wire Ecosystem 

## Immutable Communities

### Wire Blockchain

### Consensus

Wire utilizes an Appointed Proof of Stake Model (APoS) where T1 Node Operators must be appointed and recognized by existing T1 Node Operators to be recognized. Under this algorithm, T1s may select block producers through a continuous approval appointment system. Each block is produced at 500 m/s and exactly one producer is authorized to produce a block at any given point in time. If the block is not produced at the scheduled time, then the block for that time slot is skipped. When one or more blocks are skipped, there is a 0.5 or more second gap in the blockchain: Wire blocks are produced in rounds of 126 (6 blocks each, times 21 producers.) 


There are two main consensus models on which blockchains are formed. Proof of Work(PoW) and Proof of Stake(PoS). In a Proof of Work Model, miner nodes compete to find a nonce added to the header of a block that causes it to have the desired property. It is computationally expensive to find such nonces that make the blocks valid, therefore; it becomes difficult for attacks to be done using an alternative fork of the blockchain that would be accepted by the rest of the network as the best chain. The drawbacks of utilizing a Proof of Work include slow transaction speeds with expensive fees as well as high energy usage.

The other model that is widely used and accepted is known as the Proof of Stake model. In this model, nodes that own the largest stake/ percentage of some asset have equivalent decision power. This results in voting power being proportionate to the stakeholders. A variant of this model is known as a Delegated Proof of Stake model in which a large number of participants or stakeholders elect a smaller number of delegates who in turn make decisions for them. Although energy-efficient, validators with large holdings can potentially have excessive influence on transaction verification. Proof of Stake is also not as proven in terms of security as Proof of Work and is often criticized for its vulnerability to centralization. Blockchains such as EOSIO have attempted to mend the Proof of Stake Model through a Delegated Proof of State Model where a large number of Nodes can use their stake as a method of voting. These votes are used to select a small group to make decisions on behalf of the community. Even in a DPoS Model, the blockchain’s governance structure is subject to manipulation if enough votes are cast from a centralized majority. Wire utilizes an Appointed Proof of Stake combined with Asynchronous Byzantine Fault Tolerance. 

Asynchronous Byzantine Fault Tolerance, commonly known as aBFT, is a complicated but well-liked and trusted method for layer one consensus. Essentially, the layer utilizes a two-stage block confirmation mechanism. This mechanism entails the use of a ⅔ plus 1 protocol in which the supermajority of node operators from the currently scheduled set confirm each block twice. This layer will also be utilized to signal producer schedule changes at the beginning of each schedule round.

Wire’s Appointed Proof of Stake Model has improved upon its predecessors Proof of Authority and Delegated Proof of Stake. In a Proof of Authority Model, the consensus mechanism is based on identity and is a practical and efficient solution for private blockchain networks. In essence, this model stakes your “reputation” rather than coins. The drawbacks of this model are more easily centralized leaving it vulnerable to censorship and manipulation. This is a very similar problem to Delegated Proof of Stake as well since a large number of votes cast by a single centralized authority could potentially manipulate the decision-making process of the Node Operators.

This is where Appointed Proof of Stake has been able to make improvements to the previous model’s designs. In an Appointed Proof of Stake Model, Node Seats are appointed among Operators seen to have great insight and value of the Governance of the Wire Blockchain. The only way for new T1 and T2 Node Seats to be added would be through the election of a supermajority of current Node Seat votes in a ⅔ plus one mechanism. This prevents exterior influence on the Node Operators and allows for them to be viewed in public. 

### Resources

Wire introduces the concept of resources in the form of $PWR and $INV. $PWR is a regenerative representation of computation and bandwidth; while, $INV is a representation of state storage. All Node Operators are provided with a present amount of resources that are tied to their Node Seat Tier. Higher T1 Node Seats are given a larger allocation of resources than the T2 Node Seats. 

### Roll Up Technology 

Wire Blockchain introduces the concept of “likely irreversible transactions” that will help games requiring faster frames per second (fps) to take place. The roll-up moves batches of transactions to Layer two and creates a validity proof of every batch. The proofs are submitted to Layer one to serve as a proxy for their corresponding bundles. This technology allows for reduced data resulting in lower time and gas costs for validating a block. The drawback of such technology is that they are not suited for general purpose applications. This is not a problem with minor transactions of data, such as player movement and other non-core aspects of fast competitive gameplay; however, if a faulty play from a participant is likely, then actions can be taken immediately resulting in their loss.
