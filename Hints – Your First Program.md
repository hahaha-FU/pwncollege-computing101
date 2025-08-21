#  Your First Program

1) Registers & MOV
- `mov dest, src` copies value (does not remove from src).
- Key regs: `rax` = syscall number, `rdi` = 1st arg, `rsi` = 2nd arg (often used here as source).

2) Exit cleanly
- Syscall `exit` number = `60` (put in `rax`).
- Exit code goes in `rdi`.
- Minimal flow: `mov rdi, <code>` → `mov rax, 60` → `syscall`.

3) First tiny tasks
- “Move 60 into rax”: `mov rax, 60`.
- “Exit now”: `mov rax, 60` → `syscall`.
- “Exit with code 42”: `mov rdi, 42` → `mov rax, 60` → `syscall`.

4) RSI → RDI (secret exit code)
- Checker preloads secret in `rsi`.
- Do: `mov rdi, rsi` → `mov rax, 60` → `syscall`.

5) Intel syntax + entry
- Put at top: `.intel_syntax noprefix`
- Silence linker warning (optional):
(Without `_start`, ld starts at file beginning — OK for these tasks.)

6) Build & run (bare-metal)
- Assemble: `as -o asm.o asm.s`
- Link:     `ld -o exe asm.o`
- Run:      `./exe`
- Check exit code: `echo $?`

7) Mental checklist (for most tasks)
- Set args (`rdi`, maybe move from `rsi`).
- Set syscall number in `rax`.
- `syscall`.
