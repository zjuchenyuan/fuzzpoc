## heap-buffer-overflow, ReadMP3APETag, apetag.c:247, mp3gain 1.5.2

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/mp3gain_poc2

```
# ./3.mp3gain mp3gain_poc2
=================================================================
==24343==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60200000efb1 at pc 0x7ffff6ef6935 bp 0x7fffffff1b00 sp 0x7fffffff12a8
READ of size 3 at 0x60200000efb1 thread T0
    #0 0x7ffff6ef6934 in __asan_memcpy (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x8c934)
    #1 0x421f9a in ReadMP3APETag /d/prog/3mp3gain.afl/apetag.c:247
    #2 0x421f9a in ReadMP3GainAPETag /d/prog/3mp3gain.afl/apetag.c:371
    #3 0x40299a in main /d/prog/3mp3gain.afl/mp3gain.c:1778
    #4 0x7ffff67b782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #5 0x40f868 in _start (/d/p/aflasan/3.mp3gain+0x40f868)

0x60200000efb1 is located 0 bytes to the right of 1-byte region [0x60200000efb0,0x60200000efb1)
allocated by thread T0 here:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x420ae1 in ReadMP3APETag /d/prog/3mp3gain.afl/apetag.c:207
    #2 0x420ae1 in ReadMP3GainAPETag /d/prog/3mp3gain.afl/apetag.c:371

SUMMARY: AddressSanitizer: heap-buffer-overflow ??:0 __asan_memcpy
Shadow bytes around the buggy address:
  0x0c047fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9db0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9de0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c047fff9df0: fa fa fa fa fa fa[01]fa fa fa 00 07 fa fa 00 fa
  0x0c047fff9e00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e40: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==24343==ABORTING
Aborted
```

corresponding source code:

```
243             } else if (!_stricmp(name,"MP3GAIN_MINMAX")) {
244                                 /* value should be something like "001,153" */
245                 info->haveMinMaxGain = !0;
246                 vp = value;
247                                 memcpy(tmpString,vp,3);
248                                 tmpString[3] = '\0';
249                 info->minGain = atoi(tmpString);
250                 vp = vp + 4; /* skip the comma, too */
251                                 memcpy(tmpString,vp,3);
252                                 tmpString[3] = '\0';
253                 info->maxGain = atoi(tmpString);
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
