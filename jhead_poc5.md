## heap-buffer-overflow, process_DQT, jpgqguess.c:112, jhead 3.00

> [jhead](http://www.sentex.net/~mwandel/jhead/) is a program for manipulating settings and thumbnails in Exif jpeg headers used by most Digital Cameras. v3.00 Matthias Wandel, Jan 30 2013.

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/jhead_poc5

```
# ./jhead jhead_poc5
Header missing JFIF marker

Nonfatal Error : '/d/cvesubmit/jhead_poc5' Extraneous 112 padding bytes before section DB

Nonfatal Error : '/d/cvesubmit/jhead_poc5' Extraneous 74 padding bytes before section DB
=================================================================
==40050==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60400000dffc at pc 0x00000041705c bp 0x7ffffffefdb0 sp 0x7ffffffefda0
READ of size 1 at 0x60400000dffc thread T0
    #0 0x41705b in process_DQT /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgqguess.c:112
    #1 0x40f2fe in ReadJpegSections /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgfile.c:223
    #2 0x41111d in ReadJpegSections /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgfile.c:126
    #3 0x41111d in ReadJpegFile /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgfile.c:375
    #4 0x408819 in ProcessFile /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jhead.c:896
    #5 0x402c19 in main /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jhead.c:1730
    #6 0x7ffff67b782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #7 0x4067d8 in _start (/d/benchmark/p/jhead+0x4067d8)

0x60400000dffc is located 0 bytes to the right of 44-byte region [0x60400000dfd0,0x60400000dffc)
allocated by thread T0 here:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x40e86b in ReadJpegSections /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgfile.c:173

SUMMARY: AddressSanitizer: heap-buffer-overflow /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgqguess.c:112 process_DQT
Shadow bytes around the buggy address:
  0x0c087fff9ba0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff9bb0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff9bc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff9bd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff9be0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c087fff9bf0: fa fa fa fa fa fa fa fa fa fa 00 00 00 00 00[04]
  0x0c087fff9c00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff9c10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff9c20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff9c30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c087fff9c40: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==40050==ABORTING
```

corresponding source code:

```
104         for (coefindex = 0; coefindex < 64; coefindex++) {
105             unsigned int val;
106             if (c>>4) {
107                 register unsigned int temp;
108                 temp=(unsigned int) (Data[a++]);
109                 temp *= 256;
110                 val=(unsigned int) Data[a++] + temp;
111             } else {
112                 val=(unsigned int) Data[a++];
113             }
114             table[coefindex] = val;
115             if (reftable) {
116                 double x;
117                 // scaling factor in percent
118                 x = 100.0 * (double)val / (double)reftable[coefindex];
119                 cumsf += x;
120                 cumsf2 += x * x;
121                 // separate check for all-ones table (Q 100)
122                 if (val != 1) allones = 0;
123             }
124         }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
