## memcpy-param-overlap, set_pointer, mpglibDBL/common.c:328, mp3gain 1.5.2

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/mp3gain_poc3

```
# ./mp3gain mp3gain_poc3
mp3gain_poc3
fatal error.  MAXFRAMESIZE not large enough.
=================================================================
==25374==ERROR: AddressSanitizer: memcpy-param-overlap: memory ranges [0x7fffffff7314,0x7fffffff73cf) and [0x7fffffff7267, 0x7fffffff7322) overlap
    #0 0x7ffff6ef6662 in __asan_memcpy (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x8c662)
    #1 0x438d34 in set_pointer mpglibDBL/common.c:328
    #2 0x467b6f in do_layer3 mpglibDBL/layer3.c:1582
    #3 0x447a8c in decodeMP3 mpglibDBL/interface.c:643
    #4 0x4089d4 in main /d/prog/3mp3gain.afl/mp3gain.c:2262
    #5 0x7ffff67b782f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #6 0x40f868 in _start (/d/p/aflasan/3.mp3gain+0x40f868)

Address 0x7fffffff7314 is located in stack of thread T0 at offset 21572 in frame
    #0 0x401edf in main /d/prog/3mp3gain.afl/mp3gain.c:1411

  This frame has 7 object(s):
    [32, 33) 'maxgain'
    [96, 97) 'mingain'
    [160, 164) 'nprocsamp'
    [224, 232) 'maxsample'
    [288, 9504) 'lsamples'
    [9536, 18752) 'rsamples'
    [18784, 50704) 'mp' <== Memory access at offset 21572 is inside this variable
HINT: this may be a false positive if your program uses some custom stack unwind mechanism or swapcontext
      (longjmp and C++ exceptions *are* supported)
Address 0x7fffffff7267 is located in stack of thread T0 at offset 21399 in frame
    #0 0x401edf in main /d/prog/3mp3gain.afl/mp3gain.c:1411

  This frame has 7 object(s):
    [32, 33) 'maxgain'
    [96, 97) 'mingain'
    [160, 164) 'nprocsamp'
    [224, 232) 'maxsample'
    [288, 9504) 'lsamples'
    [9536, 18752) 'rsamples'
    [18784, 50704) 'mp' <== Memory access at offset 21399 is inside this variable
HINT: this may be a false positive if your program uses some custom stack unwind mechanism or swapcontext
      (longjmp and C++ exceptions *are* supported)
SUMMARY: AddressSanitizer: memcpy-param-overlap ??:0 __asan_memcpy
==25374==ABORTING
Aborted
```

corresponding source code:

```
317 int set_pointer( PMPSTR mp, long backstep)
318 {
319   unsigned char *bsbufold;
320
321   if(mp->fsizeold < 0 && backstep > 0) {
322 //    fprintf(stderr,"Can't step back %ld!\n",backstep);
323     return MP3_ERR;
324   }
325   bsbufold = mp->bsspace[1-mp->bsnum] + 512;
326   wordpointer -= backstep;
327   if (backstep)
328     memcpy(wordpointer,bsbufold+mp->fsizeold-backstep,(size_t)backstep);
329   bitindex = 0;
330   return MP3_OK;
331 }

1570 int do_layer3( PMPSTR mp,int *pcm_point)
1571 {
1572   int gr, ch, ss,clip=0;
1573   int scalefacs[2][39]; /* max 39 for short[13][3] mode, mixed: 38, long: 22 */
1574   //  struct III_sideinfo sideinfo;
1575   struct frame *fr=&(mp->fr);
1576   int stereo = fr->stereo;
1577   int single = fr->single;
1578   int ms_stereo,i_stereo;
1579   int sfreq = fr->sampling_frequency;
1580   int stereo1,granules;
1581
1582   if(set_pointer(mp, (int)sideinfo.main_data_begin) == MP3_ERR)
1583     return -32767;
1584
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
