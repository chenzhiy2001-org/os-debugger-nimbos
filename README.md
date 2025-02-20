# os-debugger-nimbos

- commit id: 545ab808b462cf606bd5073c6dad9b86297b761c
- link: <https://github.com/equation314/nimbos>

## steps
1. add
```
[profile.release]
debug = true
```
in kernel/Cargo.toml, add 
```
[profile.release]
debug = true
debuginfo-level = 2
```
in user/rust/Cargo.toml

2. run nimbos in riscv64 once
3. load code-debug extension, put `launch.json` in .vscode/

## known issues
- `debug = true
debuginfo-level = 2
- C not supported (supporting C is not hard, I just don't have time)
- only riscv supported (because I don't know arch64/x86 context switching details)
- `exit` syscall is not used in user code so we didn't write hookpoints for it (you need hookpoints for all syscalls that changes next user program symbol file such as `exec` but not `fork`)
- "gdbpath" attribute need full path like `/opt/riscv/bin/riscv64-unknown-elf-gdb`
- `make clean` in `kernel/` not completely working
  1. delete user/build manually
  2. `make clean` in `kernel`
  3. `make run ARCH=riscv64` in `kernel`
  4. `make debug ARCH=riscv64 GDB=riscv64-unknown-elf-gdb` note: you must mention `GDB`, the default `gdb-multiarch` can't single stepping into userland
- hookbreakpoint is given but not tested, you should change OS source code to test this that if you don't want to press "continue" 114514 times
