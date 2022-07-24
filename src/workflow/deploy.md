# Deploying a contract

Deploying a contract is done as follows:

## Create your workspace

A workspace is nothing else but a folder which will be mounted into the ```qvmctl``` docker container.

It will contain:
- A private key used for deployment and contract calls
- The smart contract binary to be deployed

To create the workspace create the ```ws``` directory in your current directory like this:

```mkdir ws```

## Generate a random private key

First you need a random secp256k1 private key, which is (almost) any random 32 byte sequence in a hexadecimal format with ```0x``` prepended to it.
If you have OpenSSL installed (most likely you have it if you are running Linux or macOS) then you can generate one easily with the following command:

```echo "0x"$(openssl rand -hex 32) > ws/privkey.txt```

## Get some rETH for your wallet

As stated previously, QVM is tested as a Layer2 smart contract execution engine on the Ropsten Ethereum testnet.
First you will need at least 1 rETH so you can play around qith QVM, because fees related to the operations must be paid on the testnet as well.

You can get some rETH for your wallet on this website for example: [https://faucet.egorfine.com](https://faucet.egorfine.com)

## Copy the compiled contract binary to your workspace

This simply means copying the binary file you have compiled with static linking in the step which is [specific for your language](./compile.md).
After you have successfully executed the compilation, just copy the resulting binary to your workspace under the name ```contract``` like this:

```cp /path/to/my/compiled/binary ws/contract```

## Run the deploy command of ```qvmctl``` using docker


```
docker run --rm -v $(pwd)/ws:/ws qanplatform/qvmctl deploy --privkey privkey.txt contract
```

## What happens under the hood?

### Upload

- Deployment fees are estimated based on your contract size
- Balance check is performed whether your wallet address has enough funds to cover the fee
- Your contract binary is pinned to IPFS
- A checksum of the binary's integrity is generated
- The above checksum is signed with your private key
- A call is made to the QVM Repository contract with your IPFS CID + the signature
- Fees are either paid during this call or the whole call fails

### Download

- The QVM Repository contract emits a Deployment event upon successful deployment
- QVM Executor nodes will download the binary based on the IPFS CID
- They will cryptographically prove to the QVM Repository that the contract has been downloaded indeed
- The QVM Repository contract verifies the submitted cryptographic proof to ensure Executors have the binary
- If above verification verification was successful then the contract is Registered and is now callable
