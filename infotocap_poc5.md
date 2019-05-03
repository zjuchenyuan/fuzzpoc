## heap-buffer-overflow, postprocess_terminfo, tinfo/parse_entry.c:997, ncurses 6.1

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/infotocap_poc5

```
# ./infotocap -o /dev/null infotocap_poc5
=================================================================
==34629==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x62100001b500 at pc 0x0000004b44d9 bp 0x7fffffffaa30 sp 0x7fffffffaa20
READ of size 1 at 0x62100001b500 thread T0
    #0 0x4b44d8 in postprocess_terminfo ../ncurses/./tinfo/parse_entry.c:997
    #1 0x4bda40 in _nc_parse_entry ../ncurses/./tinfo/parse_entry.c:553
    #2 0x4a8f4c in _nc_read_entry_source ../ncurses/./tinfo/comp_parse.c:225
    #3 0x404658 in main ../progs/tic.c:961
    #4 0x7ffff6ac082f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #5 0x408b98 in _start (/d/p/cveasan/infotocap+0x408b98)

0x62100001b500 is located 0 bytes to the right of 4096-byte region [0x62100001a500,0x62100001b500)
allocated by thread T0 here:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x4f38c1 in _nc_init_entry ../ncurses/./tinfo/alloc_entry.c:74
    #2 0x3  (<unknown module>)

SUMMARY: AddressSanitizer: heap-buffer-overflow ../ncurses/./tinfo/parse_entry.c:997 postprocess_terminfo
Shadow bytes around the buggy address:
  0x0c427fffb650: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c427fffb660: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c427fffb670: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c427fffb680: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c427fffb690: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c427fffb6a0:[fa]fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c427fffb6b0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c427fffb6c0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c427fffb6d0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c427fffb6e0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c427fffb6f0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==34629==ABORTING
```

corresponding source code:

```
 977 static void
 978 postprocess_terminfo(TERMTYPE2 *tp)
 979 {
 980     /*
 981      * TERMINFO-TO-TERMINFO MAPPINGS FOR SOURCE TRANSLATION
 982      * ----------------------------------------------------------------------
 983      */
 984
 985     /*
 986      * Translate AIX forms characters.
 987      */
 988     if (PRESENT(box_chars_1)) {
 989         char buf2[MAX_TERMCAP_LENGTH];
 990         string_desc result;
 991
 992         _nc_str_init(&result, buf2, sizeof(buf2));
 993         _nc_safe_strcat(&result, acs_chars);
 994
 995         append_acs0(&result, 'l', box_chars_1[0]);      /* ACS_ULCORNER */
 996         append_acs0(&result, 'q', box_chars_1[1]);      /* ACS_HLINE */
 997         append_acs0(&result, 'k', box_chars_1[2]);      /* ACS_URCORNER */
 998         append_acs0(&result, 'x', box_chars_1[3]);      /* ACS_VLINE */
 999         append_acs0(&result, 'j', box_chars_1[4]);      /* ACS_LRCORNER */
1000         append_acs0(&result, 'm', box_chars_1[5]);      /* ACS_LLCORNER */
1001         append_acs0(&result, 'w', box_chars_1[6]);      /* ACS_TTEE */
1002         append_acs0(&result, 'u', box_chars_1[7]);      /* ACS_RTEE */
1003         append_acs0(&result, 'v', box_chars_1[8]);      /* ACS_BTEE */
1004         append_acs0(&result, 't', box_chars_1[9]);      /* ACS_LTEE */
1005         append_acs0(&result, 'n', box_chars_1[10]);     /* ACS_PLUS */
1006
1007         if (buf2[0]) {
1008             acs_chars = _nc_save_str(buf2);
1009             _nc_warning("acsc string synthesized from AIX capabilities");
1010             box_chars_1 = ABSENT_STRING;
1011         }
1012     }
1013     /*
1014      * ----------------------------------------------------------------------
1015      */
1016 }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
