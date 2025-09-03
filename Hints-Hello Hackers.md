
### Level 1: Write a single character to stdout
```bash
section .text
global _start

_start:
    mov rdi, 1          ; file descriptor: stdout
    mov rsi, 1337000    ; memory address of secret char
    mov rdx, 1          ; number of bytes
    mov rax, 1          ; syscall number for write
    syscall
```
__________

### Level 2: Write a single character, then exit cleanly
```bash
section .text
global _start

_start:
    ; write(1, 1337000, 1)
    mov rdi, 1
    mov rsi, 1337000
    mov rdx, 1
    mov rax, 1
    syscall

    ; exit(42)
    mov rdi, 42
    mov rax, 60
    syscall

```
 ________________


### Level 3: Write multiple characters (14 bytes)
```bash
section .text
global _start

_start:
    ; write(1, 1337000, 14)
    mov rdi, 1
    mov rsi, 1337000
    mov rdx, 14
    mov rax, 1
    syscall

    ; exit(42)
    mov rdi, 42
    mov rax, 60
    syscall
```
______________


### Level 4: Read 8 bytes from stdin, write them to stdout, then exit
```bash
section .text
global _start

_start:
    ; read(0, 1337000, 8)
    mov rdi, 0          ; stdin
    mov rsi, 1337000    ; buffer
    mov rdx, 8          ; bytes to read
    mov rax, 0          ; syscall number for read
    syscall

    ; write(1, 1337000, 8)
    mov rdi, 1          ; stdout
    mov rsi, 1337000    ; same buffer
    mov rdx, 8          ; bytes to write
    mov rax, 1          ; syscall number for write
    syscall

    ; exit(42)
    mov rdi, 42
    mov rax, 60
    syscall
```
