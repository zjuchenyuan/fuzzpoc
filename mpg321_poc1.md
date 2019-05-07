## memory leak, new_playlist, playlist.c:51, mpg321 

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/mpg321_poc1

```
# ./mpg321 mpg321_poc1
High Performance MPEG 1.0/2.0/2.5 Audio Player for Layer 1, 2, and 3.
Version 0.3.2-1 (2012/03/25). Written and copyrights by Joe Drew,
now maintained by Nanakos Chrysostomos and others.
Uses code from various people. See 'README' for more!
THIS SOFTWARE COMES WITH ABSOLUTELY NO WARRANTY! USE AT YOUR OWN RISK!

Directory: /d/cvesubmit
Playing MPEG stream from mpg321_poc1 ...

[0:00] Decoding of mpg321_poc1 finished.

=================================================================
==29241==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 4120 byte(s) in 1 object(s) allocated from:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x414dc2 in new_playlist /d/benchmark/16.mpg321_AFL_mp3/mpg321-0.3.2-orig/playlist.c:51

Indirect leak of 16384 byte(s) in 1 object(s) allocated from:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x414ddd in new_playlist /d/benchmark/16.mpg321_AFL_mp3/mpg321-0.3.2-orig/playlist.c:55

Indirect leak of 25 byte(s) in 1 object(s) allocated from:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x7ffff5fc3489 in __strdup (/lib/x86_64-linux-gnu/libc.so.6+0x8b489)

SUMMARY: AddressSanitizer: 20529 byte(s) leaked in 3 allocation(s).
```

corresponding source code:

```
 49 playlist * new_playlist()
 50 {
 51     playlist *pl = (playlist *) malloc(sizeof(playlist));
 52
 53     srand(time(NULL));
 54
 55     pl->files = (char **) malloc(DEFAULT_PLAYLIST_SIZE * sizeof(char *));
 56     pl->files_size = DEFAULT_PLAYLIST_SIZE;
 57     pl->numfiles = 0;
 58     pl->random_play = 0;
 59     strcpy(pl->remote_file, "");
 60
 61     return pl;
 62 }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
