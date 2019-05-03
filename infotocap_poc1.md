## heap-buffer-overflow, one_one_mapping, progs/dump_entry.c:1373, ncurses 6.1

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/infotocap_poc1

```
# ./infotocap -o /dev/null infotocap_poc1
==34599==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x61c00000ff4e at pc 0x000000440933 bp 0x7ffffffeb1f0 sp 0x7ffffffeb1e0
READ of size 1 at 0x61c00000ff4e thread T0
    #0 0x440932 in one_one_mapping ../progs/dump_entry.c:1373
    #1 0x440932 in purged_acs ../progs/dump_entry.c:1399
    #2 0x440932 in dump_entry ../progs/dump_entry.c:1561
    #3 0x405006 in main ../progs/tic.c:1035
    #4 0x7ffff6ac082f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #5 0x408b98 in _start (/d/p/cveasan/infotocap+0x408b98)

0x61c00000ff4e is located 0 bytes to the right of 1742-byte region [0x61c00000f880,0x61c00000ff4e)
allocated by thread T0 here:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x4f492c in _nc_wrap_entry ../ncurses/./tinfo/alloc_entry.c:177
    #2 0x62100001a4ff  (<unknown module>)

SUMMARY: AddressSanitizer: heap-buffer-overflow ../progs/dump_entry.c:1373 one_one_mapping
Shadow bytes around the buggy address:
  0x0c387fff9f90: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c387fff9fa0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c387fff9fb0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c387fff9fc0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c387fff9fd0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c387fff9fe0: 00 00 00 00 00 00 00 00 00[06]fa fa fa fa fa fa
  0x0c387fff9ff0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c387fffa000: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c387fffa010: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c387fffa020: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c387fffa030: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==34599==ABORTING
```

corresponding source code:

```
1366 static bool
1367 one_one_mapping(const char *mapping)
1368 {
1369     bool result = TRUE;
1370
1371     if (VALID_STRING(mapping)) {
1372         int n = 0;
1373         while (mapping[n] != '\0') {
1374             if (isLine(mapping[n]) &&
1375                 mapping[n] != mapping[n + 1]) {
1376                 result = FALSE;
1377                 break;
1378             }
1379             n += 2;
1380         }
1381     }
1382     return result;
1383 }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
