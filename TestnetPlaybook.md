### How to setup for the testnet

Prerequisites:
Either:
- cardano-node running with ability to query testnet via cardano-cli
or:
- Daedalus Testnet installed and synced at 100%

## If you are choosing to use Daedalus Testnet as your cardano-node
If you are using Daedalus in order to interact with the Cardano Blockchain using it's socket for cardano-cli then you will need to install Daedalus Testnet from https://testnets.cardano.org/en/testnets/cardano/get-started/wallet/

After you have installed Daedalus Testnet please run the program and allow it to sync with the blockchain, this will take a while but hopefully not as long as it would on mainnet.

After your Daedalus cardano-node has synced, (this can be verified in the stake delegation part of the wallet interface), you will need to make sure that your CARDANO_NODE_SOCKET_PATH points to socket Daedalus has open.
This process can be for Windows here: https://www.youtube.com/watch?v=gfHMpDW6W7w

If anyone is having trouble accessing the testnet ***OR*** mainnet please feel free to reach out to community members on discord.

***PLEASE*** test to make sure that you actually can use the testnet by running `cardano-cli query tip --testnet-magic 8`, if this command does not work, you are not set up to move to the next step.

## After you have the cardano-cli working with a cardano-node
After you have installed and synced your node, you will need to generate keys and an address to use during the testnet.
If you already have a set of key files that are being used for mainnet interaction these can be used, but I recommend setting up separate keys for the testnet.

To generate key files use: `cardano-cli address key-gen --normal-key --verification-key-file testnet.vkey --signing-key-file testnet.skey`, the directory that this is executed in will be the location that the key filess are stored.

After you have your key files you can generate an address with: `cardano-cli address build --payment-verification-key-file testnet.vkey --testnet-magic 8`, please provide this address to @rekd on the discord if you would like testnet Governance Tokens.

### Interacting with the contract via the CLI
I will be filling out more here shortly, I have yet to deploy the system on the testnet and I am waiting for my Daedalus Testnet to sync.
