# Introduction

This section describes what QVM is and how it works in general on a high level to get familiar with the architecture before you start building on it.

## What is QVM?

QVM is the world's first blockchain Virtual Machine which is capable of executing statically linked ELF Linux binaries in a deterministic way so that they can be used for smart contracts.
Such smart contract instances are launched inside a hardware separated sandbox through CPU level virtualization and are exposed to a synthetic Linux kernel which translate non-determininistic syscalls to patched variants which provide output cmopatible with a vanilla kernel.

### Example QVM runtime patches

- Requesting random bytes would return a byte sequence derived from the previous block's hash. While this is not random, it is deterministic and compatible with the getrandom() syscall.
- Getting the current time via the time() syscall would always return the previous block's timestamp. Again, while this does provide the exact time (as it will mostly be a couple seconds late), but it will be also deterministic all across the entire planet on each and every executing node.

### Key initiatives

The key is that developers:

- Do not need to deal with any such quirks and workarounds when they are writing contracts
- Feel like they are just writing regular CLI applications
- Can build contracts in any language which can be compiled to a statically linked binary which runs on Linux. This is pretty much any language by the way!

## Architecture

QVM is currently implemented during its testing phase as a Layer2 smart contract execution engine.
Currently the only supported network is Ethereum Testnet "Ropsten".

## Supported languages

What does it mean that a language is "supported" by QVM? Doesn't it support ***ALL*** languages?

The answer is yes, QVM supports ***ALL*** languages which can be compiled to a static ELF binary compatible with Linux.
However, there is a continously expanding list of languages officially supported by QANplatform development team.

This means that for these languages there will be official tutorials and optimized compilers made available to ensure that contracts written in these languages run as efficient as possible and consume as little fees as absolutely required.
