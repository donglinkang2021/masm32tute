相当于添加了自动换行的`crt_printf`

```asm
.386
.model flat,stdcall
option casemap:none
includelib msvcrt.lib
include msvcrt.inc

.DATA
    someMsg byte "测试打印", 0	

.CODE
    main:
        invoke crt_puts, addr someMsg
        ret
    end main
```

