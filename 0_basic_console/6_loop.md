循环打印字符串中的数据，为之后逐位判断做铺垫

```asm
.386
.model flat, stdcall
option casemap:none
includelib msvcrt.lib
include msvcrt.inc

.DATA

    bufferA byte 256 dup(?)
    lenA dd 0
    
    inputMsgA byte "请输入第一个数字：", 0
    inputFmt byte "%s", 0
    outputMsg byte "数字是：%c", 0ah, 0
    lenMsg byte "字符串长度是：%d", 0ah, 0

.CODE

    main proc
        invoke crt_printf, addr inputMsgA
        invoke crt_scanf, addr inputFmt, addr bufferA

        invoke crt_strlen, addr bufferA
        mov lenA, eax

        invoke crt_printf, addr lenMsg, lenA

        ; 循环打印每一个字符
        xor esi, esi ; 计数器清零
        mov edi, offset bufferA ; edi指向bufferA

        print_loop:
            movzx eax, byte ptr [edi+esi] ; 从bufferA中读取一个字符
            invoke crt_printf, addr outputMsg, eax ; 打印字符
            inc esi ; 计数器加1
            cmp esi, lenA ; 比较计数器和字符串长度
            jl print_loop ; 如果计数器小于字符串长度，继续循环

        ret
    main endp
    end main
```

- 注意`invoke crt_printf, addr outputMsg, eax ; 打印字符`会改变`ecx`的值，
  - 所以我们不能像之前一样使用`ecx`先设置好循环次数，然后再`Loop`
  - 我们得注意循环中的函数可能改变寄存器的值