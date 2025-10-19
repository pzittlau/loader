# ELF Loader

A minimal user-space ELF loader for Linux.

This project is an experiment in implementing a user-space `execve`. It can load and execute Linux
ELF binaries by mapping their segments into memory, setting up the stack, and trampolining to their
entry point.

## Features

It supports statically linked PIE(`ET_DYN`) and non-PIE(`ET_ECEC`) executables directly.

For dynamically linked executables it loads the in `PT_INTERP` specified interpreter and transfers
control to it, such that it handles the full dynamic linkign process.

It also sanitizes the stack by removing the loader's arguments and updates `auxv` with the client's
information.

## Building and Running 

This creates the `loader` executable and a set of test binaries in `zig-out/bin/` :
```sh
zig build
```

Alternatively use something like this to run directly:
```sh
zig build run -- /bin/ls
```

This runs the tests:
```sh
zig build test
```

You can run an executable by passing it as an argument to the `loader`. Any subsequent arguments are
passed through to the target executable.

```sh
# Run a test executable through the loader
./zig-out/bin/loader ./zig-out/bin/test_nolibc_pie_helloWorld
# Output: Hello World!

# Run a test executable that prints its arguments
./zig-out/bin/loader ./zig-out/bin/test_nolibc_pie_printArgs foo bar baz
# Output: ./zig-out/bin/test_nolibc_pie_printArgs foo bar baz
```

## License

Apache 2.0
