## stack-buffer-overflow, GetVbrTag, mpglibDBL/interface.c:286, mp3gain 1.5.2

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/mp3gain_poc4

```
# ./mp3gain mp3gain_poc4
mp3gain_poc4
=================================================================
==35029==ERROR: AddressSanitizer: stack-buffer-overflow on address 0x7fffffff1ef0 at pc 0x000000442bbc bp 0x7fffffff1d60 sp 0x7fffffff1d50
READ of size 1 at 0x7fffffff1ef0 thread T0
    #0 0x442bbb in GetVbrTag mpglibDBL/interface.c:286
    #1 0x449839 in check_vbr_header mpglibDBL/interface.c:364
    #2 0x449839 in decodeMP3 mpglibDBL/interface.c:482
    #3 0x4089d4 in main /d/prog/3mp3gain.afl/mp3gain.c:2262
    #4 0x7ffff67b782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #5 0x40f868 in _start (/d/p/aflasan/3.mp3gain+0x40f868)

Address 0x7fffffff1ef0 is located in stack of thread T0 at offset 240 in frame
    #0 0x443caf in decodeMP3 mpglibDBL/interface.c:458

  This frame has 2 object(s):
    [32, 160) 'pTagData'
    [192, 240) 'xing' <== Memory access at offset 240 overflows this variable
HINT: this may be a false positive if your program uses some custom stack unwind mechanism or swapcontext
      (longjmp and C++ exceptions *are* supported)
SUMMARY: AddressSanitizer: stack-buffer-overflow mpglibDBL/interface.c:286 GetVbrTag
Shadow bytes around the buggy address:
  0x10007fff6380: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff6390: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff63a0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff63b0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff63c0: f1 f1 f1 f1 00 00 00 00 00 00 00 00 00 00 00 00
=>0x10007fff63d0: 00 00 00 00 f2 f2 f2 f2 00 00 00 00 00 00[f4]f4
  0x10007fff63e0: f3 f3 f3 f3 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff63f0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff6400: 00 00 00 00 00 00 00 00 f1 f1 f1 f1 01 f4 f4 f4
  0x10007fff6410: f2 f2 f2 f2 01 f4 f4 f4 f2 f2 f2 f2 04 f4 f4 f4
  0x10007fff6420: f2 f2 f2 f2 00 f4 f4 f4 f2 f2 f2 f2 00 00 00 00
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07
  Heap left redzone:       fa
  Heap right redzone:      fb
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack partial redzone:   f4
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
==35029==ABORTING
```

corresponding source code:

```
281         if( head_flags & TOC_FLAG )
282         {
283                 if( pTagData->toc != NULL )
284                 {
285                         for(i=0;i<NUMTOCENTRIES;i++)
286                                 pTagData->toc[i] = buf[i];
287                 }
288                 buf+=NUMTOCENTRIES;
289         }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
