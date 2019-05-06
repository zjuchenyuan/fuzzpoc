## heap-buffer-overflow, ReadJpegSections, jpgfile.c:251, jhead 3.00~3.03

> [jhead](http://www.sentex.net/~mwandel/jhead/) is a program for manipulating settings and thumbnails in Exif jpeg headers
> used by most Digital Cameras.  v3.03 Matthias Wandel, Dec 31 2018.

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/jhead_poc1

```
# ./jhead jhead_poc1
=================================================================
==40030==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60200000eff2 at pc 0x7ffff6ee1676 bp 0x7ffffffefea0 sp 0x7ffffffef648
READ of size 5 at 0x60200000eff2 thread T0
    #0 0x7ffff6ee1675 in memcmp (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x77675)
    #1 0x40efa9 in ReadJpegSections /d/benchmark/jhead-3.03/jpgfile.c:251
    #2 0x410f46 in ReadJpegSections /d/benchmark/jhead-3.03/jpgfile.c:126
    #3 0x410f46 in ReadJpegFile /d/benchmark/jhead-3.03/jpgfile.c:375
    #4 0x408463 in ProcessFile /d/benchmark/jhead-3.03/jhead.c:905
    #5 0x402ce8 in main /d/benchmark/jhead-3.03/jhead.c:1757
    #6 0x7ffff67b782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #7 0x406568 in _start (/d/benchmark/pl/jhead+0x406568)

0x60200000eff2 is located 0 bytes to the right of 2-byte region [0x60200000eff0,0x60200000eff2)
allocated by thread T0 here:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x40e95b in ReadJpegSections /d/benchmark/jhead-3.03/jpgfile.c:173

SUMMARY: AddressSanitizer: heap-buffer-overflow ??:0 memcmp
Shadow bytes around the buggy address:
  0x0c047fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9db0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9de0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c047fff9df0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa[02]fa
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
==40030==ABORTING
```

corresponding source code:

```
246             case M_JFIF:
247                 // Regular jpegs always have this tag, exif images have the exif
248                 // marker instead, althogh ACDsee will write images with both markers.
249                 // this program will re-create this marker on absence of exif marker.
250                 // hence no need to keep the copy from the file.
251                 if (memcmp(Data+2, "JFIF\0",5)){
252                     fprintf(stderr,"Header missing JFIF marker\n");
253                 }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
