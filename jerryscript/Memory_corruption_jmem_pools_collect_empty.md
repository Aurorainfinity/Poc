
Memory corruption in in jmem_pools_collect_empty (jerryscript/jerry-core/jmem/jmem-poolman.c:165)

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

Memory_corruption_jmem_pools_collect_empty.js
```
function main() {
var v4 = [-545238.9091190018,-545238.9091190018,-545238.9091190018,-545238.9091190018];
var v6 = [2805259460];
var v7 = [Infinity,2805259460,"b",-545238.9091190018,2805259460,v4];
var v8 = {c:Infinity,d:v4,e:v4,length:-545238.9091190018,valueOf:"b"};
var v9 = {__proto__:v4,a:-545238.9091190018,d:v6,length:v8,toString:v6};
var v10 = v8;
var v15 = [13.37,13.37,13.37,13.37];
var v17 = [1337];
var v18 = ["8*1/1r3kUV",v15,13.37,"8*1/1r3kUV",v17,13.37];
var v19 = {b:1337,c:isFinite,e:v18,length:1337,valueOf:v15};
var v23 = [13.37,13.37,13.37];
var v27 = [2.220446049250313e-16,2.220446049250313e-16,"NEGATIVE_INFINITY"];
var v28 = async (v29,v30,v31) => {
    var v32 = v27.copyWithin();
    return v31;
};
var v34 = v23.reverse();
var v36 = String;
var v37 = v34.reduceRight(v36);
var v38 = String.fromCharCode(String,1337,String,v37);
var v41 = [895176.4098468735,895176.4098468735];
async function v43(v44,v45,v46,v47,v48) {
    var v50 = RegExp();
    return v50;
}
var v51 = [895176.4098468735,895176.4098468735,"uQXGfNINNe",JSON,895176.4098468735,1337,-65535,"uQXGfNINNe",v41];
var v52 = {};
var v53 = {b:v51,c:-65535,constructor:1337,d:v51,e:895176.4098468735,length:JSON};
var v54 = JSON;
async function v56(v57,v58,v59,v60,v61) {
    return v58;
}
var v65 = new Uint16Array(Function);
var v69 = new Map();
async function v77(v78,v79,v80,v81,v82) {
    function* v85(v86,v87,v88,v89,v90) {
        yield* v89;
        v17[-997023272] = v81;
        var v92 = 0;
        var v93 = !v92;
        return 1024;
    }
    var v94 = v85("MIN_VALUE",-1165603609,..."MIN_VALUE",-1165603609);
    var v96 = new WeakMap(v94);
    return v85;
}
var v97 = 257;
var v104 = v77(13.37,13.37,"EPSILON","EPSILON",v77);
var v107 = [1337,1337,13.37];
v107.constructor = 13.37;
var v108 = v69.keys();
var v109 = v15;
var v111 = ["trFYu6cu5w"];
var v114 = Promise.bind(1);
var v116 = {};
var v118 = new Promise(Symbol,v116);
v118.__proto__ = v111;
// Stderr:
}
main();
```

###### Execution steps

```
$ ls 
Memory_corruption_jmem_pools_collect_empty.js
$ ../jerryscript/build/bin/jerry Memory_corruption_jmem_pools_collect_empty.js
ASAN:SIGSEGV
=================================================================
==4759==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x000000439c29 bp 0x7ffeef1d6370 sp 0x7ffeef1d6060 T0)
    #0 0x439c28 in jmem_pools_collect_empty /home/test/AST/jerryscript/jerry-core/jmem/jmem-poolman.c:165
    #1 0x4397da in jmem_finalize /home/test/AST/jerryscript/jerry-core/jmem/jmem-allocator.c:161
    #2 0x402b56 in main /home/test/AST/jerryscript/jerry-main/main-unix.c:324
    #3 0x7f0b20b3d83f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)
    #4 0x404238 in _start (/home/test/AST/jerryscript/build/bin/jerry+0x404238)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /home/test/AST/jerryscript/jerry-core/jmem/jmem-poolman.c:165 jmem_pools_collect_empty
==4759==ABORTING
```

```
$ ls 
Memory_corruption_jmem_pools_collect_empty.js
$ ../jerryscript-noasan/build/bin/jerry Memory_corruption_jmem_pools_collect_empty.js
Segmentation fault (core dumped)
```

###### Backtrace

```
$ gdb -args ../../../../AST/jerryscript-noasan/build/bin/jerry Memory_corruption_jmem_pools_collect_empty.js
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
Reading symbols from ./AST/jerryscript-noasan/build/bin/jerry...done.
(gdb) r
Starting program: /home/test/AST/jerryscript-noasan/build/bin/jerry ./Memory_corruption_jmem_pools_collect_empty.js

Program received signal SIGSEGV, Segmentation fault.
jmem_pools_collect_empty () at /home/test/AST/jerryscript-noasan/jerry-core/jmem/jmem-poolman.c:165
165	    jmem_pools_chunk_t *const next_p = chunk_p->next_p;
(gdb) x/3i $rip
=> 0x41bef5 <jmem_pools_collect_empty+24>:	mov    (%rdi),%rbx
   0x41bef8 <jmem_pools_collect_empty+27>:	mov    $0x8,%esi
   0x41befd <jmem_pools_collect_empty+32>:	callq  0x401555 <jmem_heap_free_block_internal>
(gdb) p/x $rdi
$1 = 0x4280000667840
(gdb) x/gx 0x4280000667840
0x4280000667840:	Cannot access memory at address 0x4280000667840
```