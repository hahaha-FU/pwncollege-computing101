## Registers Basics
- Set `rdi = 0x1337`  
  Use a `mov` instruction to put the constant directly into the register.

- Set multiple registers (`rax`, `r12`, `rsp`)  
  Same as above, just load each register with the required constant.  
  Watch out: `rsp` changes the stack pointer → don’t touch the stack after unless required.

- Add `0x331337` to `rdi`  
  You don’t need to recalc from scratch, just use `add`.

- Compute `f(x) = m*x + b` (m=rdi, x=rsi, b=rdx)  
  Multiply `m * x` → use `imul`, then add `b`. Put result in `rax`.

- Division (`speed = distance / time`)  
  Put dividend in `rax`, zero out `rdx`, then `div`. Result → `rax`.

- Modulo (`rdi % rsi`)  
  Same as division, but remainder is left in `rdx`. Move it into `rax`.

- Access upper 8 bits of `ax` (set to 0x42)  
  Use the `ah` register. (`al`=low 8, `ah`=upper 8 of `ax`).

- Fast modulo with powers of 2 (`% 256`, `% 65536`)  
  `% 256` = lowest 8 bits (`al`). `% 65536` = lowest 16 bits (`ax`).

## Bit Logic & Shifts
- Get the 5th least significant byte of `rdi`  
  Shift right until the target byte is in the lowest position, then move.

- Bitwise AND (`rdi AND rsi` → `rax`)  
  Just apply `and`. You don’t need mov here.

- Even/odd check with bitwise  
  Lowest bit determines parity. Use `and` with 1.

## Memory
- Move value at 0x404000 into `rax`  
  Use dereferencing with brackets: `[address]`.

- Store value of `rax` into 0x404000  
  Reverse: `[address] = rax`.

- Load value at 0x404000, put in `rax`, then increment memory by 0x1337  
  First `mov` into `rax` to keep original, then add constant to memory directly.

## Stack
- Pop value, subtract rdi, push result  
  Remember: `pop` into a register, operate, then `push` back.

- Swap rdi and rsi with only push/pop  
  Classic 3-push trick.

- Average of 4 qwords on stack (no pop)  
  Access via `[rsp]`, `[rsp+8]`, etc. Don’t change `rsp`.

## Control Flow
- Jump to absolute 0x403000  
  Put target in a register then `jmp`.

- Relative jmp +0x51 → set rax=1  
  Use padding (like `nop`s) to land exactly at the second instruction.

- Two jumps (jmp +0x51, then absolute jmp)  
  First jump skips forward, second jump goes to the absolute.

- Conditional: ELF vs MZ vs other  
  Compare header bytes and branch accordingly.  
  ELF magic = `0x7f454c46`, MZ = `0x5A4D`.

- Jump table (≤1 cmp, ≤3 jmps)  
  Idea: compare `rdi` to bound, use index*8 offset in a table. Default case at +0x20.

## Loops
- Average of n qwords  
  Use counter (rcx) in a loop. Accumulate sum, divide at end.

- Count non-zero bytes until 0  
  Like strlen: loop byte-by-byte, increment counter until zero found.

## Functions
- Lowercase transform (`str_lower`)  
  Walk string until null. If byte ≤ 0x5A, call helper at 0x403000. Count changes.

- Most common byte (`most_common_byte`)  
  Need a histogram (256 slots). Use stack space as an array of counters.  
  First pass: count. Second pass: find max. Return the byte with highest freq.
