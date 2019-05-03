## memory leak, TIFFClientOpen, libtiff/tif_open.c:117, libtiff v4.0.10 

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/tiffsplit_poc1

```
# ./tools/tiffsplit tiffsplit_poc1
TIFFFetchStripThing: Warning, Incorrect count for "StripOffsets"; tag ignored.
TIFFFetchStripThing: Warning, Incorrect count for "StripByteCounts"; tag ignored.
_TIFFVSetField: xaaa.tif: Bad value 1 for "TileWidth" tag.
_TIFFVSetField: xaaa.tif: Bad value 34 for "TileLength" tag.
TIFFReadRawTile: Read error at row 4294967295, col 4294967295, tile 0; got 0 bytes, expected 432.

=================================================================
==42090==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 2263 byte(s) in 2 object(s) allocated from:
    #0 0x7ffff6f02602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x47abdf in TIFFClientOpen /d/prog/libtiff/libtiff/tif_open.c:117

Indirect leak of 3728 byte(s) in 3 object(s) allocated from:
    #0 0x7ffff6f02961 in realloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98961)
    #1 0x4b03ed in _TIFFCheckRealloc /d/prog/libtiff/libtiff/tif_aux.c:71
    #2 0x4b03ed in _TIFFCheckMalloc /d/prog/libtiff/libtiff/tif_aux.c:86

Indirect leak of 16 byte(s) in 1 object(s) allocated from:
    #0 0x7ffff6f02961 in realloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98961)
    #1 0x4b0213 in _TIFFCheckRealloc /d/prog/libtiff/libtiff/tif_aux.c:71

SUMMARY: AddressSanitizer: 6007 byte(s) leaked in 6 allocation(s).
```

corresponding source code:

```
114         m = _TIFFgetMode(mode, module);
115         if (m == -1)
116                 goto bad2;
117         tif = (TIFF *)_TIFFmalloc((tmsize_t)(sizeof (TIFF) + strlen(name) + 1));
118         if (tif == NULL) {
119                 TIFFErrorExt(clientdata, module, "%s: Out of memory (TIFF structure)", name);
120                 goto bad2;
121         }
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
