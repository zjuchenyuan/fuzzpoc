## heap-buffer-overflow, process_DHT, jpgqguess.c:188, jhead 3.00

> [jhead](http://www.sentex.net/~mwandel/jhead/) is a program for manipulating settings and thumbnails in Exif jpeg headers
used by most Digital Cameras.  v3.00 Matthias Wandel, Jan 30 2013.

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/jhead_poc4

```
# ./jhead jhead_poc4
=================================================================
==40047==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x610000007ff5 at pc 0x0000004184e1 bp 0x7ffffffefec0 sp 0x7ffffffefeb0
READ of size 1 at 0x610000007ff5 thread T0
    #0 0x4184e0 in process_DHT /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgqguess.c:188
    #1 0x40f5f2 in ReadJpegSections /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgfile.c:228
    #2 0x41111d in ReadJpegSections /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgfile.c:126
    #3 0x41111d in ReadJpegFile /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgfile.c:375
    #4 0x408819 in ProcessFile /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jhead.c:896
    #5 0x402c19 in main /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jhead.c:1730
    #6 0x7ffff67b782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #7 0x4067d8 in _start (/d/benchmark/p/jhead+0x4067d8)

0x610000007ff5 is located 0 bytes to the right of 181-byte region [0x610000007f40,0x610000007ff5)
allocated by thread T0 here:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x40e86b in ReadJpegSections /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgfile.c:173

SUMMARY: AddressSanitizer: heap-buffer-overflow /d/benchmark/2.jhead_AFL_jpg/jhead-3.00/jpgqguess.c:188 process_DHT
Shadow bytes around the buggy address:
  0x0c207fff8fa0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c207fff8fb0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c207fff8fc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c207fff8fd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c207fff8fe0: fa fa fa fa fa fa fa fa 00 00 00 00 00 00 00 00
=>0x0c207fff8ff0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00[05]fa
  0x0c207fff9000: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c207fff9010: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c207fff9020: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c207fff9030: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c207fff9040: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==40047==ABORTING
```

corresponding source code:

```
181     while (a<length)
182     {
183         c = Data[a++];
184         if (ShowTags>1){
185             printf("  table %d\n", c);
186         }
187         for (i=0; i<16; i++) {
188             huff[i]=(unsigned char) Data[a++];
189         }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
