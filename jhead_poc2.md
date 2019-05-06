## heap-buffer-overflow, ReadJpegSections, jpgfile.c:273, jhead 3.00~3.03

> [jhead](http://www.sentex.net/~mwandel/jhead/) is a program for manipulating settings and thumbnails in Exif jpeg headers
> used by most Digital Cameras.  v3.03 Matthias Wandel, Dec 31 2018.

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/jhead_poc2

```
# ./jhead jhead_poc2
=================================================================
==40036==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60200000efd3 at pc 0x000000410069 bp 0x7ffffffefee0 sp 0x7ffffffefed0
READ of size 2 at 0x60200000efd3 thread T0
    #0 0x410068 in ReadJpegSections /d/benchmark/jhead-3.03/jpgfile.c:273
    #1 0x410f46 in ReadJpegSections /d/benchmark/jhead-3.03/jpgfile.c:126
    #2 0x410f46 in ReadJpegFile /d/benchmark/jhead-3.03/jpgfile.c:375
    #3 0x408463 in ProcessFile /d/benchmark/jhead-3.03/jhead.c:905
    #4 0x402ce8 in main /d/benchmark/jhead-3.03/jhead.c:1757
    #5 0x7ffff67b782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #6 0x406568 in _start (/d/benchmark/pl/jhead+0x406568)

0x60200000efd4 is located 0 bytes to the right of 4-byte region [0x60200000efd0,0x60200000efd4)
allocated by thread T0 here:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x40e95b in ReadJpegSections /d/benchmark/jhead-3.03/jpgfile.c:173

SUMMARY: AddressSanitizer: heap-buffer-overflow /d/benchmark/jhead-3.03/jpgfile.c:273 ReadJpegSections
Shadow bytes around the buggy address:
  0x0c047fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9db0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9de0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c047fff9df0: fa fa fa fa fa fa fa fa fa fa[04]fa fa fa fd fd
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
==40036==ABORTING
```

corresponding source code:

```
263                 if (ShowTags){
264                     printf("JFIF SOI marker: Units: %d ",ImageInfo.JfifHeader.ResolutionUnits);
265                     switch(ImageInfo.JfifHeader.ResolutionUnits){
266                         case 0: printf("(aspect ratio)"); break;
267                         case 1: printf("(dots per inch)"); break;
268                         case 2: printf("(dots per cm)"); break;
269                         default: printf("(unknown)"); break;
270                     }
271                     printf("  X-density=%d Y-density=%d\n",ImageInfo.JfifHeader.XDensity, ImageInfo.JfifHeader.YDensity);
272
273                     if (Data[14] || Data[15]){
274                         fprintf(stderr,"Ignoring jfif header thumbnail\n");
275                     }
276                 }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
