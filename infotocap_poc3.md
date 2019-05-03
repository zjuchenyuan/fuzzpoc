## heap-buffer-overflow, fmt_entry, progs/dump_entry.c:1100, ncurses 6.1

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/infotocap_poc3

```
# ./infotocap -o /dev/null infotocap_poc3
=================================================================
==34625==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60300000eff4 at pc 0x000000436d9d bp 0x7ffffffea070 sp 0x7ffffffea060
READ of size 1 at 0x60300000eff4 thread T0
    #0 0x436d9c in fmt_entry ../progs/dump_entry.c:1100
    #1 0x438ac8 in dump_entry ../progs/dump_entry.c:1514
    #2 0x405006 in main ../progs/tic.c:1035
    #3 0x7ffff6ac082f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #4 0x408b98 in _start (/d/p/cveasan/infotocap+0x408b98)

0x60300000eff4 is located 0 bytes to the right of 20-byte region [0x60300000efe0,0x60300000eff4)
allocated by thread T0 here:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x4a2520 in _nc_tic_expand ../ncurses/./tinfo/comp_expand.c:85

SUMMARY: AddressSanitizer: heap-buffer-overflow ../progs/dump_entry.c:1100 fmt_entry
Shadow bytes around the buggy address:
  0x0c067fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9db0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9dc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9dd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9de0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c067fff9df0: fa fa fa fa fa fa fa fa fa fa fa fa 00 00[04]fa
  0x0c067fff9e00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9e10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9e20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9e30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9e40: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==34625==ABORTING
```

corresponding source code:

```
1089                 if (cv == 0) {
1090                     if (outform == F_TCONVERR) {
1091                         _nc_SPRINTF(buffer, _nc_SLIMIT(sizeof(buffer))
1092                                     "%s=!!! %s WILL NOT CONVERT !!!",
1093                                     name, srccap);
1094                         WRAP_CONCAT;
1095                     } else if (suppress_untranslatable) {
1096                         continue;
1097                     } else {
1098                         char *s = srccap, *d = buffer;
1099                         int need = 3 + (int) strlen(name);
1100                         while ((*d = *s++) != 0) {
1101                             if ((d - buffer + 1) >= (int) sizeof(buffer)) {
1102                                 fprintf(stderr,
1103                                         "%s: value for %s is too long\n",
1104                                         _nc_progname,
1105                                         name);
1106                                 *d = '\0';
1107                                 break;
1108                             }
1109                             if (*d == ':') {
1110                                 *d++ = '\\';
1111                                 *d = ':';
1112                             } else if (*d == '\\') {
1113                                 *++d = *s++;
1114                             }
1115                             d++;
1116                             *d = '\0';
1117                         }
1118                         need += (int) (d - buffer);
1119                         wrap_concat("..", need, w1ST | wERR);
1120                         need -= 2;
1121                         wrap_concat(name, need, wOFF | wERR);
1122                         need -= (int) strlen(name);
1123                         wrap_concat("=", need, w2ND | wERR);
1124                         need -= 1;
1125                         wrap_concat(buffer, need, wEND | wERR);
1126                         outcount = TRUE;
1127                     }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
