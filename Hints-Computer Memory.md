- Memory stores data at numeric addresses. Think of addresses as houses on a street.
- You can directly load a value into a register:
```bash
mov rdi, 42  ; rdi now holds 42
```
- You can also load a value from memory using an address:
```bash
mov rdi, [133700]  ; rdi now holds the value stored at memory address 133700
```
- Registers can hold addresses (pointers), not just values. Example:
```bash
mov rax, 133700    ; rax now holds the address 133700
mov rdi, [rax]     ; rdi now holds the value at memory address 133700
```
This is called dereferencing a pointer.
- You can also dereference with offsets:
```bash
mov rax, [rdi+8]   ; get the value 8 bytes after the address in rdi
```
- Memory can store other addresses (pointers to pointers). This allows multiple levels of indirection:
```bash
; Example: triple dereference
rdi -> addr1
[addr1] -> addr2
[addr2] -> secret value
```
- In this challenge, your goal is to dereference memory 1, 2, or even 3 times to finally load the secret value into `rdi` and use it as the exit code.
____________________

### Hints / Key Points in Order
1. Direct memory access:
- You can load a value directly from memory using an address:
```bash
mov rdi, [133700]
```
2. Using a register as a pointer:
- Store the address in a register, then dereference it:
```bash
mov rax, 133700  ; rax holds address
mov rdi, [rax]   ; rdi gets value at that address
```
3. Dereferencing the same register:
- You can overwrite the pointer with the value it points to:
```bash
mov rax, 133700
mov rax, [rax]   ; rax now holds the value, not the address
```
4. Offset dereferencing:
- Access memory at a certain offset from a pointer:
```bash
mov rax, [rdi+8]  ; 8 bytes after the address in rdi
```
5. Pointer stored in memory:
- Memory can store addresses:
```bash
mov rdi, 123400  ; rdi points to memory that contains an address
mov rdi, [rdi]   ; dereference once to get the inner address
mov rax, [rdi]   ; dereference again to get the secret value
```
6. Multiple levels of indirection:
- You may need to dereference 2 or 3 times to get the secret value:
```bash
mov rdi, [rdi]  ; first dereference
mov rdi, [rdi]  ; second dereference
mov rdi, [rdi]  ; third dereference (if needed)
```
7. Registers are general-purpose:
- You can use `rax`, `rdi`, or any other register as a pointer.
- The CPU treats values and addresses the same; interpretation depends on the instruction

