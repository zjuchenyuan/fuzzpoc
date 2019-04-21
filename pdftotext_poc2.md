POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/pdftotext_poc2


```
# ./pdftotext pdftotext_poc2
=================================================================
==4462==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x00000044c3d0 bp 0x6030000019c0 sp 0x7fffffffdc90 T0)
    #0 0x44c3cf in TextLine::TextLine(GList*, double, double, double, double, double) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:804
    #1 0x487185 in TextPage::buildLine(TextBlock*) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:3725
    #2 0x48784b in TextPage::buildLines(TextBlock*, GList*) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:3641
    #3 0x48784b in TextPage::buildLines(TextBlock*, GList*) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:3652
    #4 0x48a532 in TextPage::buildColumn(TextBlock*) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:3466
    #5 0x48fc97 in TextPage::buildColumns2(TextBlock*, GList*, int) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:3436
    #6 0x48fc97 in TextPage::buildColumns2(TextBlock*, GList*, int) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:3448
    #7 0x49c2a4 in TextPage::buildColumns2(TextBlock*, GList*, int) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:1477
    #8 0x49c2a4 in TextPage::buildColumns(TextBlock*, int) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:3424
    #9 0x49c2a4 in TextPage::writeReadingOrder(void*, void (*)(void*, char const*, int), UnicodeMap*, char*, int, char*, int) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:1474
    #10 0x49f2d4 in TextPage::write(void*, void (*)(void*, char const*, int)) /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:1420
    #11 0x5a09af in Gfx::~Gfx() /d/prog/5pdftotext.afl/xpdf/Gfx.cc:590
    #12 0x7d48bc in Page::displaySlice(OutputDev*, double, double, int, int, int, int, int, int, int, int, int (*)(void*), void*) /d/prog/5pdftotext.afl/xpdf/Page.cc:406
    #13 0x7d48bc in Page::display(OutputDev*, double, double, int, int, int, int, int (*)(void*), void*) /d/prog/5pdftotext.afl/xpdf/Page.cc:323
    #14 0x7e3e80 in PDFDoc::displayPage(OutputDev*, int, double, double, int, int, int, int, int (*)(void*), void*) /d/prog/5pdftotext.afl/xpdf/PDFDoc.cc:388
    #15 0x7e3e80 in PDFDoc::displayPages(OutputDev*, int, int, double, double, int, int, int, int, int (*)(void*), void*) /d/prog/5pdftotext.afl/xpdf/PDFDoc.cc:400
    #16 0x43c66a in main /d/prog/5pdftotext.afl/xpdf/pdftotext.cc:256
    #17 0x7ffff621f82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #18 0x43e1e8 in _start (/d/p/aflasan/5.pdftotext+0x43e1e8)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /d/prog/5pdftotext.afl/xpdf/TextOutputDev.cc:804 TextLine::TextLine(GList*, double, double, double, double, double)
==4462==ABORTING
```

### gdb

```
Program received signal SIGSEGV, Segmentation fault.
0x000000000043cbea in TextLine::TextLine(GList*, double, double, double, double, double) ()
(gdb) exploitable
__main__:99: UserWarning: GDB v7.11 may not support required Python API
Description: Access violation on destination operand
Short description: DestAv (8/22)
Hash: 81b6da538816c37502a8f4fa79fedd19.4d5357df3e3a53a5babe462c1c730bcf
Exploitability Classification: EXPLOITABLE
Explanation: The target crashed on an access violation at an address matching the destination operand of the instruction. This likely indicates a write access violation, which means the attacker may control the write address and/or value.
Other tags: AccessViolation (21/22)
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
