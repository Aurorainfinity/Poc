
Memory corruption in in ecma_lcache_invalidate (jerryscript/jerry-core/ecma/base/ecma-lcache.c:209)

###### JerryScript revision

commit : https://github.com/jerryscript-project/jerryscript/commit/26a299adf0318354caebf3294c46a35ddb9bae50
version : v2.3.0

###### Build platform

Ubuntu 16.04.1 LTS (4.15.0-45-generic x86_64)

###### Build steps

```
python tools/build.py --profile=es2015-subset --lto=off --compile-flag=-g \
--strip=off --logging=on \
--compile-flag=-fsanitize=address --stack-limit=15
```

###### Test case

xxxxxxxxxxxx.js
```
function main() {
var v3 = "AhzGEZ4UBQ".__proto__;
var v6 = [13.37,13.37];
var v8 = [13.37,13.37,13.37];
var v10 = [255];
async function v12(v13,v14,v15,v16,v17) {
    var v18 = v12(127,v17,v10,v8,v16);
    var v26 = [939162.7217443376,Float32Array,939162.7217443376,939162.7217443376];
    var v28 = {b:v26,c:1337,constructor:v26,e:1337};
    var v30 = new ArrayBuffer(v28);
    var v31 = v30.slice(9007199254740992,536870912);
    var v33 = [1.0,1.0,1.0,1.0];
    var v35 = {call:1.0,construct:1.0,defineProperty:"EPSILON",deleteProperty:1337,getOwnPropertyDescriptor:1.0,getPrototypeOf:1337,has:644405221,set:"EPSILON",setPrototypeOf:v33};
    var v37 = new Proxy(Float32Array,v35);
    var v38 = v26.flatMap(v16,v28);
    var v41 = 1337 == null;
    v16.__proto__ = v37;
}
var v43 = [1337,1337];
var v44 = [];
var v45 = {c:66232812,constructor:66232812};
var v48 = 0;
while (v48 < 9) {
    var v49 = v48 + 1;
    v48 = v49;
}
var v50 = {b:13.37,a:1337,d:isFinite,e:13.37,length:v44,toString:v44};
async function v55(v56,v57,v58,v59,v60) {
    function* v63(v64,v65,v66,v67,v68) {
        yield* v67;
    }
    var v69 = v63("MIN_VALUE",-1165603609,..."MIN_VALUE",-1165603609);
    var v71 = new WeakMap(v69);
}
var v74 = v55(13.37,13.37,"EPSILON","EPSILON",v55);
var v75 = [13.37,13.37,13.37];
var v76 = ["trFYu6cu5w"];
var v77 = {__proto__:v75,b:1,constructor:13.37,valueOf:v76};
var v80 = [2995767872,2995767872,2995767872,2995767872,2995767872];
var v81 = [...v80,..."EPSILON"];
var v84 = undefined;
var v86 = v81.filter("EPSILON");
var v87 = 66232812;
var v90 = 0;
var v91 = v90 + 1;
v90 = v91;
var v93 = Math.acos(v90,0);
var v95 = [-284091.18646696594,-284091.18646696594,-284091.18646696594];
var v97 = Number[5];
var v99 = JSON.parse(v95,v97);
// Stderr:
}
main();
```

###### Execution steps

1. Username must contains 4 letters (example: xxxx. x can be any letters.).
2. js file must in /home/xxxx/xxx/xxxxxxx/xxxxxx/xxxxxxxx/ (x can be any letters).
3. js filename must contains 12 letters (example: xxxxxxxxxxxx.js. x can be any letters).
4. run with jsfile's full path (example: /home/xxxx/xxx/xxxxxxx/xxxxxx/xxxxxxxx/xxxxxxxxxxxx.js. x can be any letters).

```
$ ls 
xxxxxxxxxxxx.js
$ pwd
/home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx
$ ../jerryscript/build/bin/jerry /home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx/xxxxxxxxxxxx.js
TypeError
ASAN:SIGSEGV
=================================================================
==2769==ERROR: AddressSanitizer: SEGV on unknown address 0x00000075b008 (pc 0x00000041630e bp 0x0000006da5c0 sp 0x7ffcb766ab30 T0)
    #0 0x41630d in ecma_lcache_invalidate /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-lcache.c:209
    #1 0x414cb9 in ecma_free_property /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-helpers.c:795
    #2 0x40b390 in ecma_gc_free_properties /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-gc.c:1088
    #3 0x40c6b6 in ecma_gc_free_object /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-gc.c:1460
    #4 0x40c6b6 in ecma_gc_run /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-gc.c:1589
    #5 0x41601c in ecma_finalize /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-init-finalize.c:85
    #6 0x4065b7 in jerry_cleanup /home/test/AST/jerryscript/jerry-core/api/jerry.c:238
    #7 0x402b56 in main /home/test/AST/jerryscript/jerry-main/main-unix.c:324
    #8 0x7f3c8e34383f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)
    #9 0x404238 in _start (/home/test/AST/jerryscript/build/bin/jerry+0x404238)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-lcache.c:209 ecma_lcache_invalidate
==2769==ABORTING
```

```
$ ls 
xxxxxxxxxxxx.js
$ pwd
/home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx
$ ../jerryscript-noasan/build/bin/jerry /home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx/xxxxxxxxxxxx.js 
TypeError
Segmentation fault (core dumped)
```

###### Backtrace

```
$ gdb -args ../../../../AST/jerryscript-noasan/build/bin/jerry /home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx/xxxxxxxxxxxx.js 
GNU gdb (Ubuntu 7.11.1-0ubuntu1~16.5) 7.11.1
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
<http://www.gnu.org/software/gdb/documentation/>.
For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from ../../../../AST/jerryscript-noasan/build/bin/jerry...done.
(gdb) r
Starting program: /home/test/AST/jerryscript-noasan/build/bin/jerry /home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx/xxxxxxxxxxxx.js
TypeError

Program received signal SIGSEGV, Segmentation fault.
ecma_lcache_invalidate (object_p=object_p@entry=0x667388 <jerry_global_heap+88>, name_cp=name_cp@entry=0, prop_p=prop_p@entry=0x667340 <jerry_global_heap+16> "(")
    at /home/test/AST/jerryscript-noasan/jerry-core/ecma/base/ecma-lcache.c:209
209	    if (entry_p->id != 0 && entry_p->prop_p == prop_p)
(gdb) x/3i $rip
=> 0x4098d0 <ecma_lcache_invalidate+29>:	cmpl   $0x0,0x8(%rax)
   0x4098d4 <ecma_lcache_invalidate+33>:	je     0x4098ef <ecma_lcache_invalidate+60>
   0x4098d6 <ecma_lcache_invalidate+35>:	cmp    %rbp,(%rax)
(gdb) p/x $rax
$1 = 0x709000
(gdb) x/gx $rax
0x709000:	Cannot access memory at address 0x709000
```