## stack-overflow in Parser::getObj, pdftotext, xpdf-4.01

POC: https://github.com/zjuchenyuan/fuzzpoc/raw/master/pdftotext_poc1

```
# ./pdftotext pdftotext_poc1

ASAN:SIGSEGV
=================================================================
==4458==ERROR: AddressSanitizer: stack-overflow on address 0x7fffff7fefc0 (pc 0x7ffff624eea3 bp 0x7fffff801660 sp 0x7fffff7fefa0 T0)
    #0 0x7ffff624eea2  (/lib/x86_64-linux-gnu/libc.so.6+0x4fea2)
    #1 0x7ffff624c32c in vfprintf (/lib/x86_64-linux-gnu/libc.so.6+0x4d32c)
    #2 0x7ffff6ecaba1 in vfprintf (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x60ba1)
    #3 0x7ffff6ecacf9 in fprintf (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x60cf9)
    #4 0x5599d8 in error(ErrorCategory, long, char const*, ...) /d/prog/5pdftotext.afl/xpdf/Error.cc:82
    #5 0x78d731 in Lexer::getObj(Object*) /d/prog/5pdftotext.afl/xpdf/Lexer.cc:480
    #6 0x7dc95d in Parser::shift() /d/prog/5pdftotext.afl/xpdf/Parser.cc:268
    #7 0x7dc95d in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:146
    #8 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #9 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #10 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #11 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #12 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #13 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #14 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #15 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #16 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #17 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #18 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #19 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #20 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #21 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #22 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #23 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #24 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #25 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #26 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #27 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #28 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #29 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #30 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #31 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #32 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #33 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #34 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #35 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #36 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #37 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #38 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #39 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #40 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #41 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #42 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #43 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #44 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #45 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #46 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #47 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #48 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #49 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #50 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #51 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #52 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #53 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #54 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #55 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #56 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #57 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #58 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #59 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #60 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #61 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #62 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #63 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #64 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #65 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #66 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #67 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #68 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #69 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #70 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #71 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #72 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #73 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #74 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #75 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #76 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #77 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #78 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #79 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #80 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #81 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #82 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #83 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #84 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #85 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #86 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #87 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #88 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #89 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #90 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #91 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #92 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #93 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #94 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #95 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #96 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #97 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #98 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #99 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #100 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #101 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #102 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #103 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #104 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #105 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #106 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #107 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #108 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #109 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #110 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #111 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #112 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #113 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #114 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #115 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #116 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #117 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #118 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #119 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #120 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #121 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #122 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #123 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #124 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #125 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #126 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #127 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #128 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #129 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #130 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #131 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #132 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #133 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #134 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #135 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #136 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #137 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #138 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #139 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #140 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #141 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #142 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #143 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #144 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #145 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #146 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #147 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #148 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #149 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #150 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #151 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #152 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #153 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #154 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #155 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #156 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #157 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #158 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #159 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #160 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #161 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #162 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #163 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #164 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #165 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #166 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #167 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #168 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #169 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #170 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #171 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #172 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #173 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #174 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #175 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #176 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #177 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #178 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #179 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #180 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #181 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #182 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #183 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #184 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #185 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #186 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #187 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #188 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #189 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #190 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #191 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #192 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #193 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #194 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #195 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #196 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #197 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #198 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #199 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #200 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #201 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #202 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #203 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #204 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #205 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #206 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #207 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #208 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #209 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #210 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #211 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #212 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #213 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #214 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #215 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #216 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #217 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #218 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #219 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #220 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #221 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #222 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #223 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #224 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #225 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #226 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #227 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #228 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #229 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #230 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #231 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #232 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #233 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #234 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #235 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #236 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #237 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #238 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #239 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #240 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #241 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #242 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #243 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #244 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #245 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #246 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #247 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #248 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #249 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #250 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #251 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #252 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #253 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #254 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #255 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #256 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #257 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #258 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #259 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #260 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #261 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #262 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #263 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #264 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #265 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #266 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #267 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #268 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #269 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #270 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #271 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #272 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #273 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #274 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #275 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #276 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #277 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #278 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #279 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #280 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71
    #281 0x7dc1cc in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:93
    #282 0x8b5301 in XRef::fetch(int, int, Object*, int) /d/prog/5pdftotext.afl/xpdf/XRef.cc:1087
    #283 0x8557bc in Object::dictLookup(char const*, Object*, int) /d/prog/5pdftotext.afl/xpdf/Object.h:267
    #284 0x8557bc in Stream::addFilters(Object*, int) /d/prog/5pdftotext.afl/xpdf/Stream.cc:138
    #285 0x7d97cf in Parser::makeStream(Object*, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:245
    #286 0x7dcd52 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:103
    #287 0x7dc6e1 in Parser::getObj(Object*, int, unsigned char*, CryptAlgorithm, int, int, int, int) /d/prog/5pdftotext.afl/xpdf/Parser.cc:71

SUMMARY: AddressSanitizer: stack-overflow ??:0 ??
==4458==ABORTING
```

## gdb:

```
Program received signal SIGSEGV, Segmentation fault.
0x00007ffff71b918e in _IO_vfprintf_internal (s=0x7fffff7ff1f0, format=0x4f2e40 "%s (%d): %s\n", ap=0x7fffff8018a8) at vfprintf.c:1267
1267    vfprintf.c: No such file or directory.
(gdb) exploitable
__main__:99: UserWarning: GDB v7.11 may not support required Python API
Description: Possible stack corruption
Short description: PossibleStackCorruption (7/22)
Hash: da3718c73390ccdcfa058e437910e709.3f3ab3064b10d35392382a4bbe0c0102
Exploitability Classification: EXPLOITABLE
Explanation: GDB generated an error while unwinding the stack and/or the stack contained return addresses that were not mapped in the inferior's process address space and/or the stack pointer is pointing to a location outside the default stack region. These conditions likely indicate stack corruption, which is generally considered exploitable.
Other tags: DestAv (8/22), AccessViolation (21/22)
```

This bug was found by [NESA Lab](https://nesa.zju.edu.cn).
