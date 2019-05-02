## memory access violation, ReadMP3APETag, apetag.c:234, mp3gain 1.5.2

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/mp3gain_poc1

```
# ./3.mp3gain mp3gain_poc1
=================================================================
==20889==ERROR: AddressSanitizer: unknown-crash on address 0x60200000eeb5 at pc 0x00000042236e bp 0x7fffffff1b00 sp 0x7fffffff1af0
READ of size 4 at 0x60200000eeb5 thread T0
    #0 0x42236d in ReadMP3APETag /d/prog/3mp3gain.afl/apetag.c:234
    #1 0x42236d in ReadMP3GainAPETag /d/prog/3mp3gain.afl/apetag.c:371
    #2 0x40299a in main /d/prog/3mp3gain.afl/mp3gain.c:1778
    #3 0x7ffff67b782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #4 0x40f868 in _start (/d/p/aflasan/3.mp3gain+0x40f868)

0x60200000eeb8 is located 0 bytes to the right of 8-byte region [0x60200000eeb0,0x60200000eeb8)
allocated by thread T0 here:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x420ae1 in ReadMP3APETag /d/prog/3mp3gain.afl/apetag.c:207
    #2 0x420ae1 in ReadMP3GainAPETag /d/prog/3mp3gain.afl/apetag.c:371

SUMMARY: AddressSanitizer: unknown-crash /d/prog/3mp3gain.afl/apetag.c:234 ReadMP3APETag
Shadow bytes around the buggy address:
  0x0c047fff9d80: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9d90: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9db0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c047fff9dd0: fa fa fa fa fa fa[00]fa fa fa 00 05 fa fa fd fa
  0x0c047fff9de0: fa fa fd fa fa fa fd fa fa fa fd fa fa fa fd fa
  0x0c047fff9df0: fa fa fd fa fa fa fd fa fa fa fd fd fa fa 00 fa
  0x0c047fff9e00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==20889==ABORTING
Aborted
```

corresponding source code:

```
226             } else if (!_stricmp(name,"MP3GAIN_UNDO")) {
227                                 /* value should be something like "+003,+003,W" */
228                 info->haveUndo = !0;
229                 vp = value;
230                                 memcpy(tmpString,vp,4);
231                                 tmpString[4] = '\0';
232                 info->undoLeft = atoi(tmpString);
233                 vp = vp + 5; /* skip the comma, too */
234                                 memcpy(tmpString,vp,4);
235                                 tmpString[4] = '\0';
236                 info->undoRight = atoi(tmpString);
237                 vp = vp + 5; /* skip the comma, too */
238                 if ((*vp == 'w')||(*vp == 'W')) {
239                     info->undoWrap = !0;
240                 } else {
241                     info->undoWrap = 0;
242                 }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
