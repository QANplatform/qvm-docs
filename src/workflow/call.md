# Calling a contract function

Contracts can be called by their address.
There are various pieces of data one can/should pass to a contract call in order to make it succeed.

The general syntax of a contract call is executed as follows:

```sh
docker run --rm qanplatform/qvmctl call \
    -m $RAM_BYTES \
    -t $TIMEOUT_MILLISEC \
    -r $DB_READ_SLOTS \
    -w $DB_WRITE_SLOTS_COUNT \
    --privkey $PRIVKEY_PATH \
    $CONTRACT_ADDR
    $ARG1 $ARG2 $ARGN
```

The arguments prepended with a **$** sign mean the following:

- $RAM_BYTES: How much RAM the contract needs to execute the requested operation successfully
- $TIMEOUT_MILLISEC: How much time the contract needs to execute the requested operation successfully
- $DB_READ_SLOTS: The Database fields to be read and passed to the execution environment, separated by commas
- $DB_WRITE_SLOTS_COUNT: How many database fields this operation will write to after successful execution
- $PRIVKEY_PATH: Path to a file which contains the private key for a wallet which has enough funds to execute the call
- $CONTRACT_ADDR: Address of the smart contract to be called
- $ARG{N}: Arguments passed to the smart contract binary during execution
