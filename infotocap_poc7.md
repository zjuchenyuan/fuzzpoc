## SEGV, _nc_find_entry, tinfo/comp_hash.c:70, ncurses 6.1

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/infotocap_poc7

```
# ./infotocap -o /dev/null infotocap_poc7
=================================================================
==34650==ERROR: AddressSanitizer: SEGV on unknown address 0x62500004f900 (pc 0x000000449e93 bp 0x0000000a19ef sp 0x7fffffffb460 T0)
    #0 0x449e92 in _nc_find_entry ../ncurses/./tinfo/comp_hash.c:70
    #1 0x42c56d in nametrans ../progs/dump_entry.c:174
    #2 0x405c37 in put_translate ../progs/tic.c:338
    #3 0x405c37 in main ../progs/tic.c:1030
    #4 0x7ffff6ac082f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #5 0x408b98 in _start (/d/p/cveasan/infotocap+0x408b98)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV ../ncurses/./tinfo/comp_hash.c:70 _nc_find_entry
==34650==ABORTING

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
