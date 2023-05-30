# pwnable.tw-deaslr
1. Move the stack to a fixed read and write area (possibly bss area), the purpose is to leave dynamic library addresses.
2. Continue moving stack 1 again to another place in this area, the purpose is so that the next execution does not affect the old area in step 1.
3. Use the gets@plt function (note that rdi serves as a parameter) to insert ROPs into the area around the libc address left behind (by performing step 1) in a logical way, the purpose is to give dynamic library address into RBX.
4. Use ROP add ebx, esi to assign a value that matches one_gadget system("/bin/sh") into ebx (ignoring the low 4 bytes).
5. Use ROP push rbx, note that before it was designed to rbp = r12, the purpose is to make the program error-free.
6. Use the gets@plt function to complete the last 4 bytes of the one_gadget that was pushed onto the stack in step 5. Format is 0x7f?? (success rate is 1/256, I chose "c4" to include ??).
7. Move the stack into ret to the part of the one_gadget address left out earlier to get the shell.
8. The miner program I added connects to the server multiple times until the shell is retrieved (on average, about 70 runs succeeds once).
