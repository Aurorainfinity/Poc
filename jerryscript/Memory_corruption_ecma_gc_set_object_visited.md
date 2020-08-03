
Memory corruption in in ecma_gc_set_object_visited (jerryscript/jerry-core/ecma/base/ecma-gc.c:85)

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

Memory_corruption_ecma_gc_set_object_visited.js
```
function main() {
var v4 = [13.37,13.37];
var v6 = [1337,1337,1337,1337,1337];
var v7 = [13.37,Uint8ClampedArray,1337,-65535,-65535,"OaT5sFZLFO"];
var v8 = {__proto__:1337,a:Uint8ClampedArray,b:1337,constructor:-65535,valueOf:"OaT5sFZLFO"};
var v9 = {__proto__:v7,b:Uint8ClampedArray,c:-65535,constructor:v7,toString:"OaT5sFZLFO",valueOf:1337};
var v10 = [-65535,...v7];
var v12 = [13.37];
var v15 = [-370590.12831863505,-370590.12831863505,-370590.12831863505,-370590.12831863505];
var v17 = {b:v15,c:1337,constructor:v15,e:1337};
var v19 = new Proxy(v17,Object);
v19.__proto__ = v12;
var v24 = "trFYu6cu5w".slice(1);
var v27 = WeakMap;
var v29 = [13.37,3205311021,13.37,13.37];
try {
    var v30 = v27();
} catch(v31) {
}
var v32 = [13.37,13.37,13.37];
var v34 = [1337,1337,1337,1337,1337];
var v35 = ["trFYu6cu5w"];
var v36 = {__proto__:1,b:Float32Array,c:1,d:"trFYu6cu5w",length:1,toString:1337};
var v37 = Float32Array instanceof Float32Array;
var v39 = -1024;
var v40 = v39 + 1;
var v41 = {__proto__:v32,b:1,constructor:13.37,valueOf:v35};
var v42 = "trFYu6cu5w";
var v45 = -1226825305 + -1226825305;
var v46 = ["trFYu6cu5w"];
async function v48(v49,v50,v51,v52,v53) {
    var v54 = v48(1,v51,v46,v50);
    var v57 = [-65536];
    var v58 = async (v59,v60,v61) => {
        return v59;
    };
    var v62 = v58(v58);
    var v63 = v58(v62,v57);
    var v65 = ["NEGATIVE_INFINITY"];
    var v66 = async (v67,v68,v69) => {
        return v63;
    };
    var v70 = v66(v66,v66);
    var v71 = v66(v70,v65);
    var v74 = ["EPSILON"];
    var v76 = "pgy/i*bidI".match();
    var v77 = {apply:v54,call:v48,construct:v52,deleteProperty:v51,get:v48,getOwnPropertyDescriptor:v51,isExtensible:v52,set:v48,setPrototypeOf:isFinite};
    var v79 = Proxy();
    var v80 = isFinite();
    var v82 = 13.37;
    var v83 = {call:isFinite,construct:isFinite,defineProperty:isFinite,deleteProperty:isFinite,getOwnPropertyDescriptor:isFinite,has:isFinite,ownKeys:isFinite,preventExtensions:isFinite,e:isFinite,setPrototypeOf:isFinite};
    var v86 = {get:v48};
    var v88 = Object.defineProperty(v42,"__proto__");
    var v89 = Int32Array(v74);
    var v91 = [1.7976931348623157e+308];
    var v93 = (-329557.7589866845).constructor;
    var v94 = v91;
    var v95 = v94 + 1;
    var v97 = v89.subarray(3);
    var v100 = [1337,1337,1337,1337,1337];
    var v101 = [v100];
    var v102 = [DataView];
    var v103 = "symbol".lastIndexOf;
    var v108 = "EPSILON".replace(0,"valueOf");
    var v111 = ["function"];
    var v112 = Int8Array();
    var v114 = ["EPSILON",13.37,13.37,13.37];
    var v115 = v114.lastIndexOf(v108,13.37);
    var v116 = {};
    var v117 = [v116,v116,v116,v116,v116];
    for (var v121 = 0; v121 < 5; v121 = v121 + 1) {
    }
}
var v124 = v48(13.37,13.37,"EPSILON");
var v126 = [-2.0,-2.0,-2.0,-2.0];
var v127 = v48(v48,v35,"trFYu6cu5w",v124,v41);
// Stderr:
}
main();
```

###### Execution steps

```
$ ls 
Memory_corruption_ecma_gc_set_object_visited.js
$ ../jerryscript/build/bin/jerry Memory_corruption_ecma_gc_set_object_visited.js
ASAN:SIGSEGV
=================================================================
==4826==ERROR: AddressSanitizer: SEGV on unknown address 0x00000540a590 (pc 0x00000040af94 bp 0x0000006dceb4 sp 0x7fff81ac4240 T0)
    #0 0x40af93 in ecma_gc_set_object_visited /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-gc.c:85
    #1 0x40a0ce in ecma_gc_mark_promise_object /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-gc.c:284
    #2 0x40a0ce in ecma_gc_mark /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-gc.c:588
    #3 0x40b856 in ecma_gc_run /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-gc.c:1520
    #4 0x403e80 in jmem_heap_realloc_block /home/test/AST/jerryscript/jerry-core/jmem/jmem-heap.c:539
    #5 0x40ce6a in ecma_collection_push_back /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-helpers-collection.c:141
    #6 0x42f9e0 in ecma_promise_do_then /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-promise-object.c:876
    #7 0x42f9e0 in ecma_promise_then /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-promise-object.c:941
    #8 0x471981 in ecma_builtin_promise_prototype_then /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtin-promise-prototype.c:51
    #9 0x471981 in ecma_builtin_promise_prototype_dispatch_routine /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtin-promise-prototype.inc.h:32
    #10 0x41b3e1 in ecma_builtin_dispatch_routine /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtins.c:1181
    #11 0x41b3e1 in ecma_builtin_dispatch_call /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtins.c:1205
    #12 0x4260e1 in ecma_op_function_call /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-function-object.c:1142
    #13 0x428785 in ecma_process_promise_resolve_thenable_job /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-jobqueue.c:381
    #14 0x428785 in ecma_process_all_enqueued_jobs /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-jobqueue.c:541
    #15 0x402a94 in main /home/test/AST/jerryscript/jerry-main/main-unix.c:286
    #16 0x7f03f4d0683f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)
    #17 0x404238 in _start (/home/test/AST/jerryscript/build/bin/jerry+0x404238)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-gc.c:85 ecma_gc_set_object_visited
==4826==ABORTING

```

```
$ ls 
Memory_corruption_ecma_gc_set_object_visited.js
$ ../jerryscript-noasan/build/bin/jerry Memory_corruption_ecma_gc_set_object_visited.js 
Segmentation fault (core dumped)
```

###### Backtrace

```
$ gdb -args ../../../../AST/jerryscript-noasan/build/bin/jerry Memory_corruption_ecma_gc_set_object_visited.js
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
Reading symbols from ../../jerryscript-noasan/build/bin/jerry...done.
(gdb) r
Starting program: /home/test/AST/jerryscript-noasan/build/bin/jerry Memory_corruption_ecma_gc_set_object_visited.js

Program received signal SIGSEGV, Segmentation fault.
ecma_gc_set_object_visited (object_p=0x8ea7310) at /home/test/AST/jerryscript-noasan/jerry-core/ecma/base/ecma-gc.c:85
85	  if (object_p->type_flags_refs >= ECMA_OBJECT_NON_VISITED)
(gdb) x/3i $rip
=> 0x4051f4 <ecma_gc_set_object_visited>:	mov    (%rdi),%ax
   0x4051f7 <ecma_gc_set_object_visited+3>:	cmp    $0xffbf,%ax
   0x4051fb <ecma_gc_set_object_visited+7>:	jbe    0x40522b <ecma_gc_set_object_visited+55>
(gdb) p/x $rdi
$1 = 0x8ea7310
(gdb) x/gx 0x8ea7310
0x8ea7310:	Cannot access memory at address 0x8ea7310

```