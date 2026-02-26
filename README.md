# rrs-pico

A minimal fork of [rrs](https://github.com/GregAC/rrs) by
[Greg Chadwick](https://gregchadwick.co.uk/blog/building-rrs-pt1/), stripped
down to just the instruction decoding and processing core.

Supports the RV32IM instruction set plus CSR, environment call, and
privileged instructions (`ecall`, `ebreak`, `wfi`, `mret`).

## Usage

```toml
[dependencies]
rrs-lib = { git = "https://github.com/brevis-network/rrs-pico" }
```

The central abstraction is `InstructionProcessor`. Implement it to define what
happens for each RV32IM instruction — execute, disassemble, analyse, etc.
`process_instruction` decodes a raw 32-bit instruction word and dispatches to
the appropriate method.

```rust
use rrs_lib::InstructionProcessor;
use rrs_lib::instruction_formats::{RType, IType};
use rrs_lib::process_instruction;

struct InsnCounter { count: u32 }

impl InstructionProcessor for InsnCounter {
    type InstructionResult = ();
    fn process_add(&mut self, _: RType) { self.count += 1; }
    // ... one method per instruction
}
```

## License

Apache-2.0. See [LICENSE](LICENSE).
