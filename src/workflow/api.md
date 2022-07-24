# Generic API

Any contract can interact with QVM by following the generic API described below:

## Reading from the database

QVM calls specify database slots to be accessed during contract execution.
These slots are made available under their specified keys in the execution context via environment variables.

Let's say the decimal precision and total supply information of a token is required for read and/or write in a contract interaction, then executing the following call would make it available under the environment variables DB_TOTAL_SUPPLY and DB_DECIMALS:

```
qvmctl call 0xCAFEBABE -r TOTAL_SUPPLY,DECIMALS print info
```

Then this contract would receive two command line arguments "print" and "info" and could print token information related to total supply and decimals which properties are available as environment variables as stated above.

## Writing to the database

If a contract wants to write to the database to insert and/or modify a record under a certain key, it needs to write the following instruction to the process' STDOUT:

```
DBW=$KEY=$VALUE
```

For example if a token's name is updatable and is stored under the key ```TOKEN_NAME```, it could be updated easily to something else like this in Golang:

```go
os.Stdout.WriteString("DBW=TOKEN_NAME=NewTokenName\n")
```

## Deleting (pruning) values from the database

To prune key(s) from the database, simply output a "DBP=" prefixed new-line (\n) terminated line to stdout:

```
DBP=removekey
```

Above line would delete the storage slot under key "removekey" from persistent storage.
Multiple keys can be removed either by specifying more lines prefixed with "DBP=", or simply concatenating keys to be removed with ',' like this:

```
DBP=removekey,removethistoo
```

Above line would delete the storage slot under key "removekey" and also "removethistoo" from persistent storage.

## Writing to virtual STDOUT

Since the "real" STDOUT is reserved for the specific opcodes above, writing to a "virtual" STDOUT is achieved as simply outputting a "OUT=" prefixed new-line (\n) terminated line to stdout:

```
OUT=Program ran all good
```

The string "Program ran all good" will be emitted on-chain as event data, but not persisted to the database. This is a lot cheaper in gas fees than writing to storage, but this data is only visible to clients and not usable in a next contract interaction as it is not saved to state.

## Reporting errors

If a contract execution fails for any reason, developers can utilize normal behavior and simply write the error to STDERR.
This will mark the contract call as failed due to STDERR not being empty, and emit STDERR content (trimmed to the first 32 bytes).

Note that this will cause all previous instructions written to STDOUT to be discarded and call state will be set as failed, and only the bytes written to STDERR will be emitted as an on-chain event similar to how STDOUT works above.



