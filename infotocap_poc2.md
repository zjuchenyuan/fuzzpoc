## global-buffer-overflow, _nc_find_entry, tinfo/comp_hash.c:66, ncurses 6.1

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/infotocap_poc2

```
# ./infotocap -o /dev/null infotocap_poc2
=================================================================
==34623==ERROR: AddressSanitizer: global-buffer-overflow on address 0x00000050f946 at pc 0x000000449f0b bp 0x7fffffffb450 sp 0x7fffffffb440
READ of size 2 at 0x00000050f946 thread T0
    #0 0x449f0a in _nc_find_entry ../ncurses/./tinfo/comp_hash.c:66
    #1 0x42c56d in nametrans ../progs/dump_entry.c:174
    #2 0x405c37 in put_translate ../progs/tic.c:338
    #3 0x405c37 in main ../progs/tic.c:1030
    #4 0x7ffff6ac082f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #5 0x408b98 in _start (/d/p/cveasan/infotocap+0x408b98)

0x00000050f946 is located 26 bytes to the left of global variable '_nc_info_hash_table' defined in '../ncurses/comp_captab.c:585:24' (0x50f960) of size 1990
0x00000050f946 is located 18 bytes to the right of global variable 'cap_names_text' defined in '../ncurses/comp_captab.c:1590:19' (0x50f360) of size 1492
SUMMARY: AddressSanitizer: global-buffer-overflow ../ncurses/./tinfo/comp_hash.c:66 _nc_find_entry
Shadow bytes around the buggy address:
  0x000080099ed0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x000080099ee0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x000080099ef0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x000080099f00: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x000080099f10: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x000080099f20: 00 00 00 00 00 00 04 f9[f9]f9 f9 f9 00 00 00 00
  0x000080099f30: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x000080099f40: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x000080099f50: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x000080099f60: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x000080099f70: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
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
==34623==ABORTING
```

corresponding source code:

```
 54 NCURSES_EXPORT(struct name_table_entry const *)
 55 _nc_find_entry(const char *string,
 56                const HashValue * hash_table)
 57 {
 58     bool termcap = (hash_table != _nc_get_hash_table(FALSE));
 59     const HashData *data = _nc_get_hash_info(termcap);
 60     int hashvalue;
 61     struct name_table_entry const *ptr = 0;
 62     struct name_table_entry const *real_table;
 63
 64     hashvalue = data->hash_of(string);
 65
 66     if (data->table_data[hashvalue] >= 0) {
 67
 68         real_table = _nc_get_table(termcap);
 69         ptr = real_table + data->table_data[hashvalue];
 70         while (!data->compare_names(ptr->nte_name, string)) {
 71             if (ptr->nte_link < 0) {
 72                 ptr = 0;
 73                 break;
 74             }
 75             ptr = real_table + (ptr->nte_link
 76                                 + data->table_data[data->table_size]);
 77         }
 78     }
 79
 80     return (ptr);
 81 }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
