# Protocol

  1. [Consensus Protocol](https://developer.alacritys.net/docs/protocol/1._Consensus_Protocol.md): An ALAIO blockchain is a highly efficient, deterministic, distributed state machine that can operate in a decentralized fashion. The blockchain keeps track of transactions within a sequence of interchanged blocks. Each block cryptographically commits to the previous blocks along the same chain. It is therefore intractable to modify a transaction recorded on a given block without breaking the cryptographic checks of successive blocks. This simple fact makes blockchain transactions immutable and secure.
  2. [Transactions Protocol](https://developer.alacritys.net/docs/protocol/2._Transactions_Protocol.md): Actions define atomic behaviors within a smart contract. At a higher level, transactions define groups of actions that execute atomically within a decentralized application. Analogously to a database transaction, the group of actions that form a blockchain transaction must all succeed, one by one, in a predefined order, or else the transaction will fail. To maintain transaction atomicity and integrity in case of a failed transaction, the blockchain state is restored to a state consistent with the state prior to processing the transaction. This guarantees that no side effects arise from any actions executed prior to the point of failure.
  3. [Network Peer Protocol](https://developer.alacritys.net/docs/protocol/3._Network_Peer_Protocol.md): Nodes on an active ALAIO blockchain must be able to communicate with each other for relaying transactions, pushing blocks, and syncing state between peers. The peer-to-peer (p2p) protocol, part of the alanode service that runs on every node, serves this purpose. The ability to sync state is crucial for each block to eventually reach finality within the global state of the blockchain and allow each node to advance the last irreversible block (LIB). In this regard, the fundamental goal of the p2p protocol is to sync blocks and propagate transactions between nodes to reach consensus and advance the blockchain state.
  4. [Accounts And Permissions](https://developer.alacritys.net/docs/protocol/4._Accounts_and_Permissions.md): An account identifies a participant in an ALAIO blockchain. A participant can be an individual or a group depending on the assigned permissions within the account. Accounts also represent the smart contract actors that push and receive actions to and from other accounts in the blockchain. Actions are always contained within transactions. A transaction can be one or more atomic actions.
  5. [Alaio Gaia Storage](https://developer.alacritys.net/docs/protocol/5._Alaio_Gaia_Storage.md): In short, Gaia Hub is a decentralized data storage system. For a completely decentralized application, transactional data is stored on the blockchain and user application data and/or media is stored in Gaia storage. Storing data off of the blockchain ensures that your application can provide users with high performance and high availability for data reads and writes without introducing central trust parties.
  6. [Alaio Oracle ](https://developer.alacritys.net/docs/protocol/6._Alaio_Oracle.md): An oracle, in the context of blockchains and smart contracts, is an agent that finds and verifies real-world occurrences and submits this information to a blockchain to be used by smart contracts.