## 寄存器

* x86-64 架构中有许多不同类型的寄存器，通用寄存器总共有 16 个。每个寄存器宽 64 位，且每个寄存器的低字节、字和双字都可以单独寻址。
* 字 = 2 字节，双字 = 4 字节

| **Register** | **Lower byte** | **Lower word** | **Lower dword** |
| ------------ | -------------- | -------------- | --------------- |
| rax          | al             | ax             | eax             |
| rbx          | bl             | bx             | ebx             |
| rcx          | cl             | cx             | ecx             |
| rdx          | dl             | dx             | edx             |
| rsp          | spl            | sp             | esp             |
| rsi          | sil            | si             | esi             |
| rdi          | dil            | di             | edi             |
| rbp          | bpl            | bp             | ebp             |
| r8           | r8b            | r8w            | r8d             |
| r9           | r9b            | r9w            | r9d             |
| r10          | r10b           | r10w           | r10d            |
| r11          | r11b           | r11w           | r11d            |
| r12          | r12b           | r12w           | r12d            |
| r13          | r13b           | r13w           | r13d            |
| r14          | r14b           | r14w           | r14d            |
| r15          | r15b           | r15w           | r15d            |

> [!attention] 以下内容来自 Grok 老师
> ```text
> RAX (64 位):
+---------------------------------------------------------------+
| 高 32 位                          | 低 32 位 (EAX)            |
|                                   +---------------------------+
|                                   | 高 16 位 | 低 16 位 (AX)  |
|                                   |          +---------------+
|                                   |          | AH | AL       |
+---------------------------------------------------------------+
> ```

## 调用系统函数

### 导入 DLL

手动构造导入符号表

```nasm
section '.idata' import readable writeable
idt: ; import directory table starts here
     ; entry for KERNEL32.DLL
     dd rva kernel32_iat
     dd 0
     dd 0
     dd rva kernel32_name
     dd rva kernel32_iat
     ; NULL entry - end of IDT
     dd 5 dup(0)
name_table: ; hint/name table
        _ExitProcess_Name dw 0
                          db "ExitProcess", 0, 0

kernel32_name: db "KERNEL32.DLL", 0
kernel32_iat: ; import address table for KERNEL32.DLL
        ExitProcess dq rva _ExitProcess_Name
        dq 0 ; end of KERNEL32's IAT
```

### 函数调用约定（Windows）

* **参数通过栈还是寄存器传递？** 前四个整型或指针参数通过寄存器 rcx、rdx、r8 和 r9 传递；前四个浮点参数通过寄存器 xmm0 到 xmm3 传递。任何额外的参数都通过栈传递。
* 无论前四个参数是否使用栈传递，或者无论是否有四个参数，调用者都必须在栈上为它们分配空间。
* **谁负责清理堆栈？** 调用者。
* **栈指针必须对齐 16 字节**。因此在为前四个参数分配栈空间时，由于在压入参数前已经压入了 8 字节的返回地址，因此必须多压入 8 字节保证对齐。最终需要 `sub rsp, 8 * 5`。

## References

* [Let's Learn x86-64 Assembly! Part 0 - Setup and First Steps](https://gpfault.net/posts/asm-tut-0.txt.html)