# Compiling a contract

When compiling a Go contract one wants to reduce the resulting binary size to the bare minimum possible to save on deployment fees, since that metric is tied only to the size of the deployed binary.

As a good rule of thumb is to always strip DWARF, symbol table and debug info. To achieve that build with the following flags like the below example:

```go build -ldflags '-s -w'```

We will provide an optimized compiler for Go which will shrink the binary size (and as a result the deployment fees!) by an order of magnitude.
The sample provided compiles to a ~200k sized binary instead of ~1.3M one with information stripped as recommended above or ~1.8M without removing those!
