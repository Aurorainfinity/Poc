## 00-NULL-pointer-dereference-ObjectStream-getObject

	==112953==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x00000046165f bp 0x7ffc5b3b07e0 sp 0x7ffc5b3b07c0 T0)
	    #0 0x46165e in ObjectStream::getObject(int, int, Object*) /home/test/pdftohtml_tmp/xpdf/XRef.cc:183
	    #1 0x463a46 in XRef::fetch(int, int, Object*) /home/test/pdftohtml_tmp/xpdf/XRef.cc:833
	    #2 0x43c761 in Object::fetch(XRef*, Object*) /home/test/pdftohtml_tmp/xpdf/Object.cc:106
	    #3 0x41faa7 in Dict::lookup(char*, Object*) /home/test/pdftohtml_tmp/xpdf/Dict.cc:76
	    #4 0x41447f in Object::dictLookup(char*, Object*) (/home/test/pdftohtml_tmp/src/pdftohtml+0x41447f)
	    #5 0x41e2b2 in Catalog::Catalog(XRef*) /home/test/pdftohtml_tmp/xpdf/Catalog.cc:50
	    #6 0x43f340 in PDFDoc::setup(GString*, GString*) /home/test/pdftohtml_tmp/xpdf/PDFDoc.cc:200
	    #7 0x43f13e in PDFDoc::PDFDoc(GString*, GString*, GString*, void*) /home/test/pdftohtml_tmp/xpdf/PDFDoc.cc:102
	    #8 0x41c42a in main /home/test/pdftohtml_tmp/src/pdftohtml.cc:172
	    #9 0x7ff84751483f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)
	    #10 0x4028a8 in _start (/home/test/pdftohtml_tmp/src/pdftohtml+0x4028a8)

	AddressSanitizer can not provide additional info.
	SUMMARY: AddressSanitizer: SEGV /home/test/pdftohtml_tmp/xpdf/XRef.cc:183 ObjectStream::getObject(int, int, Object*)
	==112953==ABORTING

## 01-Stack-overflow-GString-resize

	==53770==ERROR: AddressSanitizer: stack-overflow on address 0x7fff8f17dff8 (pc 0x7f1188c9c2b4 bp 0x000006404890 sp 0x7fff8f17e000 T0)
	    #0 0x7f1188c9c2b3  (/usr/lib/x86_64-linux-gnu/libasan.so.2+0xb02b3)
	    #1 0x7f1188c9bd67  (/usr/lib/x86_64-linux-gnu/libasan.so.2+0xafd67)
	    #2 0x7f1188c0ef4f  (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x22f4f)
	    #3 0x7f1188c8567e in operator new[](unsigned long) (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x9967e)
	    #4 0x4a6d16 in GString::resize(int) (/home/test/pdftohtml_tmp/src/pdftohtml+0x4a6d16)
	    #5 0x4a614e in GString::GString(GString*) (/home/test/pdftohtml_tmp/src/pdftohtml+0x4a614e)
	    #6 0x41df3a in GString::copy() ../goo/GString.h:38
	    #7 0x43c699 in Object::copy(Object*) /home/test/pdftohtml_tmp/xpdf/Object.cc:78
	    #8 0x43c776 in Object::fetch(XRef*, Object*) /home/test/pdftohtml_tmp/xpdf/Object.cc:106
	    #9 0x41faa7 in Dict::lookup(char*, Object*) /home/test/pdftohtml_tmp/xpdf/Dict.cc:76
	    #10 0x41447f in Object::dictLookup(char*, Object*) (/home/test/pdftohtml_tmp/src/pdftohtml+0x41447f)
	    #11 0x410a9e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:1943
	    #12 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #13 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #14 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #15 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #16 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #17 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #18 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #19 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #20 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #21 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #22 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #23 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #24 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #25 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #26 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008
	    #27 0x410f8e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:2008

## 02-Heap-buffer-overflow-GString-getLength

	==86439==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x602000016e30 at pc 0x000000413f33 bp 0x7fff777ada30 sp 0x7fff777ada20
	READ of size 4 at 0x602000016e30 thread T0
	    #0 0x413f32 in GString::getLength() ../goo/GString.h:50
	    #1 0x4a6132 in GString::GString(GString*) (/home/test/pdftohtml_tmp/src/pdftohtml+0x4a6132)
	    #2 0x410ae7 in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:1947
	    #3 0x410808 in HtmlOutputDev::dumpDocOutline(Catalog*) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:1916
	    #4 0x41ce3c in main /home/test/pdftohtml_tmp/src/pdftohtml.cc:303
	    #5 0x7f2f38afc83f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)
	    #6 0x4028a8 in _start (/home/test/pdftohtml_tmp/src/pdftohtml+0x4028a8)

	0x602000016e32 is located 0 bytes to the right of 2-byte region [0x602000016e30,0x602000016e32)
	allocated by thread T0 here:
	    #0 0x7f2f397df602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
	    #1 0x4a7bb6 in gmalloc (/home/test/pdftohtml_tmp/src/pdftohtml+0x4a7bb6)
	    #2 0x4a7dce in copyString (/home/test/pdftohtml_tmp/src/pdftohtml+0x4a7dce)
	    #3 0x43c709 in Object::copy(Object*) /home/test/pdftohtml_tmp/xpdf/Object.cc:93
	    #4 0x43c776 in Object::fetch(XRef*, Object*) /home/test/pdftohtml_tmp/xpdf/Object.cc:106
	    #5 0x41faa7 in Dict::lookup(char*, Object*) /home/test/pdftohtml_tmp/xpdf/Dict.cc:76
	    #6 0x41447f in Object::dictLookup(char*, Object*) (/home/test/pdftohtml_tmp/src/pdftohtml+0x41447f)
	    #7 0x410a9e in HtmlOutputDev::newOutlineLevel(_IO_FILE*, Object*, Catalog*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:1943
	    #8 0x410808 in HtmlOutputDev::dumpDocOutline(Catalog*) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:1916
	    #9 0x41ce3c in main /home/test/pdftohtml_tmp/src/pdftohtml.cc:303
	    #10 0x7f2f38afc83f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)

	SUMMARY: AddressSanitizer: heap-buffer-overflow ../goo/GString.h:50 GString::getLength()
	Shadow bytes around the buggy address:
	  0x0c047fffad70: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c047fffad80: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c047fffad90: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c047fffada0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	  0x0c047fffadb0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
	=>0x0c047fffadc0: fa fa 00 00 fa fa[02]fa fa fa fd fd fa fa fd fd
	  0x0c047fffadd0: fa fa fd fa fa fa fd fa fa fa fd fa fa fa fd fa
	  0x0c047fffade0: fa fa fd fa fa fa fd fa fa fa fd fd fa fa fd fa
	  0x0c047fffadf0: fa fa fd fd fa fa fd fa fa fa fd fd fa fa fd fa
	  0x0c047fffae00: fa fa fd fa fa fa fd fd fa fa fd fa fa fa fd fa
	  0x0c047fffae10: fa fa fd fa fa fa fd fa fa fa fd fa fa fa fd fa
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
	==86439==ABORTING

## 03-alloc-dealloc-mismatch-HtmlString-~HtmlString

	==126277==ERROR: AddressSanitizer: alloc-dealloc-mismatch (malloc vs operator delete) on 0x606000008420
	    #0 0x7f795792cb2a in operator delete(void*) (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x99b2a)
	    #1 0x40380b in HtmlString::~HtmlString() /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:152
	    #2 0x408206 in HtmlPage::coalesce() /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:672
	    #3 0x40d943 in HtmlOutputDev::endPage() /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:1458
	    #4 0x471ab5 in Gfx::~Gfx() /home/test/pdftohtml_tmp/xpdf/Gfx.cc:509
	    #5 0x43ebe5 in Page::displaySlice(OutputDev*, double, double, int, int, int, int, int, int, int, Links*, Catalog*, int (*)(void*), void*) /home/test/pdftohtml_tmp/xpdf/Page.cc:347
	    #6 0x43e1ad in Page::display(OutputDev*, double, double, int, int, int, Links*, Catalog*, int (*)(void*), void*) /home/test/pdftohtml_tmp/xpdf/Page.cc:223
	    #7 0x43fa41 in PDFDoc::displayPage(OutputDev*, int, double, double, int, int, int, int, int (*)(void*), void*) /home/test/pdftohtml_tmp/xpdf/PDFDoc.cc:327
	    #8 0x43fb1c in PDFDoc::displayPages(OutputDev*, int, int, double, double, int, int, int, int, int (*)(void*), void*) /home/test/pdftohtml_tmp/xpdf/PDFDoc.cc:343
	    #9 0x41ce0a in main /home/test/pdftohtml_tmp/src/pdftohtml.cc:300
	    #10 0x7f7956c4883f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)
	    #11 0x4028a8 in _start (/home/test/pdftohtml_tmp/src/pdftohtml+0x4028a8)

	0x606000008420 is located 0 bytes inside of 64-byte region [0x606000008420,0x606000008460)
	allocated by thread T0 here:
	    #0 0x7f795792b602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
	    #1 0x4a7c4d in grealloc (/home/test/pdftohtml_tmp/src/pdftohtml+0x4a7c4d)
	    #2 0x403a6b in HtmlString::addChar(GfxState*, double, double, double, double, unsigned int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:167
	    #3 0x405646 in HtmlPage::addChar(GfxState*, double, double, double, double, double, double, unsigned int*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:361
	    #4 0x40df16 in HtmlOutputDev::drawChar(GfxState*, double, double, double, double, double, double, unsigned int, int, unsigned int*, int) /home/test/pdftohtml_tmp/src/HtmlOutputDev.cc:1513
	    #5 0x47ef30 in Gfx::doShowText(GString*) /home/test/pdftohtml_tmp/xpdf/Gfx.cc:2823
	    #6 0x47df96 in Gfx::opShowSpaceText(Object*, int) /home/test/pdftohtml_tmp/xpdf/Gfx.cc:2681
	    #7 0x47244c in Gfx::execOp(Object*, Object*, int) /home/test/pdftohtml_tmp/xpdf/Gfx.cc:676
	    #8 0x471e7a in Gfx::go(int) /home/test/pdftohtml_tmp/xpdf/Gfx.cc:566
	    #9 0x471c5e in Gfx::display(Object*, int) /home/test/pdftohtml_tmp/xpdf/Gfx.cc:538
	    #10 0x43e9e1 in Page::displaySlice(OutputDev*, double, double, int, int, int, int, int, int, int, Links*, Catalog*, int (*)(void*), void*) /home/test/pdftohtml_tmp/xpdf/Page.cc:317
	    #11 0x43e1ad in Page::display(OutputDev*, double, double, int, int, int, Links*, Catalog*, int (*)(void*), void*) /home/test/pdftohtml_tmp/xpdf/Page.cc:223
	    #12 0x43fa41 in PDFDoc::displayPage(OutputDev*, int, double, double, int, int, int, int, int (*)(void*), void*) /home/test/pdftohtml_tmp/xpdf/PDFDoc.cc:327
	    #13 0x43fb1c in PDFDoc::displayPages(OutputDev*, int, int, double, double, int, int, int, int, int (*)(void*), void*) /home/test/pdftohtml_tmp/xpdf/PDFDoc.cc:343
	    #14 0x41ce0a in main /home/test/pdftohtml_tmp/src/pdftohtml.cc:300
	    #15 0x7f7956c4883f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)

	SUMMARY: AddressSanitizer: alloc-dealloc-mismatch ??:0 operator delete(void*)
	==126277==HINT: if you don't care about these warnings you may set ASAN_OPTIONS=alloc_dealloc_mismatch=0
	==126277==ABORTING
