## stack-buffer-overflow, fmt_entry, progs/dump_entry.c:1116, ncurses 6.1

Note: This is different from [CVE-2017-10684](https://bugzilla.redhat.com/show_bug.cgi?id=1464687), which crashes on line 1131 `len += (int) strlen(capability) + 1; `

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/infotocap_poc4

```
# ./infotocap -o /dev/null infotocap_poc4
=================================================================
==34627==ERROR: AddressSanitizer: stack-buffer-overflow on address 0x7ffffffeb194 at pc 0x000000436da5 bp 0x7ffffffea070 sp 0x7ffffffea060
WRITE of size 1 at 0x7ffffffeb194 thread T0
    #0 0x436da4 in fmt_entry ../progs/dump_entry.c:1116
    #1 0x438ac8 in dump_entry ../progs/dump_entry.c:1514
    #2 0x405006 in main ../progs/tic.c:1035
    #3 0x7ffff6ac082f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #4 0x408b98 in _start (/d/p/cveasan/infotocap+0x408b98)

Address 0x7ffffffeb194 is located in stack of thread T0 at offset 4212 in frame
    #0 0x42dc3f in fmt_entry ../progs/dump_entry.c:879

  This frame has 2 object(s):
    [32, 43) 'boxchars'
    [96, 4212) 'buffer' <== Memory access at offset 4212 overflows this variable
HINT: this may be a false positive if your program uses some custom stack unwind mechanism or swapcontext
      (longjmp and C++ exceptions *are* supported)
SUMMARY: AddressSanitizer: stack-buffer-overflow ../progs/dump_entry.c:1116 fmt_entry
Shadow bytes around the buggy address:
  0x10007fff55e0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff55f0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff5600: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff5610: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff5620: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x10007fff5630: 00 00[04]f4 f3 f3 f3 f3 00 00 00 00 00 00 00 00
  0x10007fff5640: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff5650: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x10007fff5660: 00 00 00 00 00 00 f1 f1 f1 f1 04 f4 f4 f4 f2 f2
  0x10007fff5670: f2 f2 00 02 f4 f4 f2 f2 f2 f2 00 00 00 00 00 00
  0x10007fff5680: 00 00 00 00 f4 f4 f2 f2 f2 f2 00 00 00 00 00 00
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
==34627==ABORTING
```

corresponding source code:

```
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
