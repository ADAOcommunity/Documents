***NOTICE: The Testnet contract address will change and the contract is likely to have a few bugs, one is currently identified and remdiation plans will be exercised***

## How to setup for the testnet

Prerequisites:
Either:
- cardano-node running with ability to query testnet via cardano-cli
or:
- Daedalus Testnet installed and synced at 100%

### If you are choosing to use Daedalus Testnet as your cardano-node
If you are using Daedalus in order to interact with the Cardano Blockchain using it's socket for cardano-cli then you will need to install Daedalus Testnet from https://testnets.cardano.org/en/testnets/cardano/get-started/wallet/

After you have installed Daedalus Testnet please run the program and allow it to sync with the blockchain, this will take a while but hopefully not as long as it would on mainnet.

After your Daedalus cardano-node has synced, (this can be verified in the stake delegation part of the wallet interface), you will need to make sure that your CARDANO_NODE_SOCKET_PATH points to socket Daedalus has open.
This process can be for Windows here: https://www.youtube.com/watch?v=gfHMpDW6W7w

If anyone is having trouble accessing the testnet ***OR*** mainnet please feel free to reach out to community members on discord.

***PLEASE*** test to make sure that you actually can use the testnet by running `cardano-cli query tip --testnet-magic 1097911063`, if this command does not work, you are not set up to move to the next step.

### After you have the cardano-cli working with a cardano-node
After you have installed and synced your node, you will need to generate keys and an address to use during the testnet.
If you already have a set of key files that are being used for mainnet interaction these can be used, but I recommend setting up separate keys for the testnet.

To generate key files use: `cardano-cli address key-gen --normal-key --verification-key-file testnet.vkey --signing-key-file testnet.skey`, the directory that this is executed in will be the location that the key filess are stored.

After you have your key files you can generate an address with: `cardano-cli address build --payment-verification-key-file testnet.vkey --testnet-magic 8`, please provide this address to @rekd on the discord if you would like testnet Governance Tokens.

## Interacting with the contract via the CLI

As a first step, please create a file named owner.json and placed the following contents inside:
```
{"constructor":0,"fields":[{"bytes":"<YOUR_PUBLIC_KEY_HASH_HERE>"},{"int":0},{"int":0}]}
```

*In order to get your public key hash you can run the following command: `cardano-cli address key-hash --payment-verification-key-file testnet.vkey`*

Modify the `owner.json` so that the second 'int' field contains a value corresponding to less than 120 seconds after the transaction to be submitted in POSIXTIME.

After you have this you may use it as the embedded datum file in your transaction to the script.

Create a transaction that sends your governance tokens to the script with your new `owner.json` file as the embedded datum file in the tx.

```
cardano-cli transaction build --alonzo-era --testnet-magic 1097911063 --protocol-params-file pparams.json \
--tx-in <UTXO Containing Your GOV Tokens> \
--tx-out addr_test1wzs92hw4ltkws0vrkh5fvhaw9nwekk98s9xtz9lcp8nvpncmk55fk+20000000+"xxx 4848df818216fe84c2054acca26c6892afff29246be43c7d19ccb743.476f76546f6b656e" \
--tx-out-datum-embed-file owner.json \
--change-address <YOUR_ADDRESS> \
--out-file putO.raw
```

This command can be used as a general template for building the transaction to lock your governance tokens, it must then be signed by your signing key and submitted to the chain.

After you have submitted this transaction, you can:

### 1) Send your owned UTxOs to any other address:
In order to do this, you will need to build a transaction that consists of the UTxO(s) that you want to move. In addition to this, you will need to make sure that the output is valid, (if it it sent to a script you need to apply a valid datum).
For example, if you'd like to send the contents of the UTxO back to your address you can use this command to build the transaction:
*Please make sure ***NOT*** *to modify your owner.json to be valid prior to running this command, this is due to it being used as the "datum" value in the UTxO being redeemed. To submit back to the script, a new modified datum file will be needed, as well as the original*

```
cardano-cli transaction build --alonzo-era --testnet-magic 1097911063 --protocol-params-file pparams.json \
--tx-in <UTxO in your wallet with funds for the fee> \
--tx-in <UTxO You own in the script> \
--tx-in-script-file validator.plutus \
--tx-in-datum-file owner.json \
--tx-in-redeemer-value 42 \
--tx-in-collateral <UTxO in your wallet with the funds for collateral> \
--tx-out <YOUR_ADDRESS>+2000000+"xxx 4848df818216fe84c2054acca26c6892afff29246be43c7d19ccb743.476f76546f6b656e" \
--change-address <YOUR_ADDRESS> \
--out-file redeem.raw
```

Once you sign and submit the transaction that you just built you should receive the GovTokens that you had locked 
1) Create a proposal.
2) Apply your voting power to a proposal.

***You can download the validator.plutus file here: <Coming shortly> ***

I need to write the transaction templates for these and continue testing the properties of our script. The script address may change over the coming days as bugs are identified and rectified.
