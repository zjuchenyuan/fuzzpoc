## buffer overflow (memcpy-param-overlap), set_pointer, mpglibDBL/common.c:328, mp3gain 1.5.2

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

```
./mp3gain mp3gain_poc3
mp3gain_poc3
fatal error.  MAXFRAMESIZE not large enough.
fatal error.  MAXFRAMESIZE not large enough.
Recommended "Track" dB change: -4.700000
Recommended "Track" mp3 gain change: -3
WARNING: some clipping may occur with this gain change!
Max PCM sample at current gain: 12792886124736674426985638455937444411271456856027240145723853411857493627294669318713917964030732817587451039710264115093054312142410882793487886211546846160119687396883441600078624727086370680264618294922894153210150564315510797704577217423263658826384134729830583015206567373729003468777769799904133120.000000
Max mp3 global gain field: 174
Min mp3 global gain field: 16


Recommended "Album" dB change for all files: -4.700000
Recommended "Album" mp3 gain change for all files: -3
WARNING: with this global gain change, some clipping may occur in file mp3gain_poc3
*** buffer overflow detected ***: /d/p/justafl/3.mp3gain terminated
======= Backtrace: =========
/lib/x86_64-linux-gnu/libc.so.6(+0x777e5)[0x7ffff777b7e5]
/lib/x86_64-linux-gnu/libc.so.6(__fortify_fail+0x5c)[0x7ffff781d15c]
/lib/x86_64-linux-gnu/libc.so.6(+0x117160)[0x7ffff781b160]
/lib/x86_64-linux-gnu/libc.so.6(+0x1166c9)[0x7ffff781a6c9]
/lib/x86_64-linux-gnu/libc.so.6(_IO_default_xsputn+0x80)[0x7ffff777f6b0]
/lib/x86_64-linux-gnu/libc.so.6(+0x5225a)[0x7ffff775625a]
/lib/x86_64-linux-gnu/libc.so.6(_IO_vfprintf+0x1f49)[0x7ffff77530b9]
/lib/x86_64-linux-gnu/libc.so.6(__vsprintf_chk+0x84)[0x7ffff781a754]
/lib/x86_64-linux-gnu/libc.so.6(__sprintf_chk+0x7d)[0x7ffff781a6ad]
/d/p/justafl/3.mp3gain[0x41e4a6]
/d/p/justafl/3.mp3gain[0x406afd]
/lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0xf0)[0x7ffff7724830]
/d/p/justafl/3.mp3gain[0x410929]
======= Memory map: ========
00400000-0045d000 r-xp 00000000 08:12 181141537                          /d/p/justafl/3.mp3gain
0065d000-0065e000 r--p 0005d000 08:12 181141537                          /d/p/justafl/3.mp3gain
0065e000-0065f000 rw-p 0005e000 08:12 181141537                          /d/p/justafl/3.mp3gain
0065f000-00b3c000 rw-p 00000000 00:00 0                                  [heap]
7ffff74ee000-7ffff7504000 r-xp 00000000 00:49 89                         /lib/x86_64-linux-gnu/libgcc_s.so.1
7ffff7504000-7ffff7703000 ---p 00016000 00:49 89                         /lib/x86_64-linux-gnu/libgcc_s.so.1
7ffff7703000-7ffff7704000 rw-p 00015000 00:49 89                         /lib/x86_64-linux-gnu/libgcc_s.so.1
7ffff7704000-7ffff78c4000 r-xp 00000000 00:49 39                         /lib/x86_64-linux-gnu/libc-2.23.so
7ffff78c4000-7ffff7ac4000 ---p 001c0000 00:49 39                         /lib/x86_64-linux-gnu/libc-2.23.so
7ffff7ac4000-7ffff7ac8000 r--p 001c0000 00:49 39                         /lib/x86_64-linux-gnu/libc-2.23.so
7ffff7ac8000-7ffff7aca000 rw-p 001c4000 00:49 39                         /lib/x86_64-linux-gnu/libc-2.23.so
7ffff7aca000-7ffff7ace000 rw-p 00000000 00:00 0
7ffff7ace000-7ffff7bd6000 r-xp 00000000 00:49 93                         /lib/x86_64-linux-gnu/libm-2.23.so
7ffff7bd6000-7ffff7dd5000 ---p 00108000 00:49 93                         /lib/x86_64-linux-gnu/libm-2.23.so
7ffff7dd5000-7ffff7dd6000 r--p 00107000 00:49 93                         /lib/x86_64-linux-gnu/libm-2.23.so
7ffff7dd6000-7ffff7dd7000 rw-p 00108000 00:49 93                         /lib/x86_64-linux-gnu/libm-2.23.so
7ffff7dd7000-7ffff7dfd000 r-xp 00000000 00:49 32                         /lib/x86_64-linux-gnu/ld-2.23.so
7ffff7fed000-7ffff7ff1000 rw-p 00000000 00:00 0
7ffff7ff7000-7ffff7ff8000 rw-p 00000000 00:00 0
7ffff7ff8000-7ffff7ffa000 r--p 00000000 00:00 0                          [vvar]
7ffff7ffa000-7ffff7ffc000 r-xp 00000000 00:00 0                          [vdso]
7ffff7ffc000-7ffff7ffd000 r--p 00025000 00:49 32                         /lib/x86_64-linux-gnu/ld-2.23.so
7ffff7ffd000-7ffff7ffe000 rw-p 00026000 00:49 32                         /lib/x86_64-linux-gnu/ld-2.23.so
7ffff7ffe000-7ffff7fff000 rw-p 00000000 00:00 0
7ffffffde000-7ffffffff000 rw-p 00000000 00:00 0                          [stack]
ffffffffff600000-ffffffffff601000 r-xp 00000000 00:00 0                  [vsyscall]

Program received signal SIGABRT, Aborted.
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
