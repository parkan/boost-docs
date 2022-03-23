# Architecture

The `boostd` executable runs alongside a lotus full node and lotus miner as a daemon. This daemon replaces the current market subsystem in the lotus miner. The boost daemon exposes a libp2p interface for storage and retrieval deals. All the on-chain operations are performed by making API calls to a lotus node. The daemon also hands off downloaded data to the lotus miner for sealing via API calls to the lotus miner.

The `boostd` market subsystem also had a UI interface for easier fund management and deal monitoring. This UI is built as a react app for managing deals that consumes a graphql interface exposed by the daemon.

![](<.gitbook/assets/Boost Interfaces.png>)

### Storage Deal Flow

The typical flow for a Storage Deal is:

1. The Client puts funds in escrow with the Storage Market Actor on chain.
2. The Client uploads a CAR file to a web server.
3. The Client sends a storage deal proposal to Boost with the URL of the CAR file.
4. Boost checks that the client has enough funds in escrow to pay for storing the file.
5. Boost accepts the storage deal proposal.
6. Boost downloads the CAR file from the web server.
7. Boost publishes the deal on chain.
8. The client checks that the deal was successfully published on chain.

![](<.gitbook/assets/Boost Flow.png>)

Boost exposes a libp2p interface to listen for storage deal proposals from clients. This is similar to the libp2p interface exposed by the lotus market subsystem.&#x20;

Boost communicates with the lotus full node over its JSON-RPC API for on-chain operations like checking client funds and publishing the deal.

Once the deal has been published, Boost hands off the downloaded file to `lotus-miner` for sealing.