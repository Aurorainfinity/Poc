
Memory corruption in in ecma_property_hashmap_find (jerryscript/jerry-core/ecma/base/ecma-property-hashmap.c:480)

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

Memory_corruption_ecma_property_hashmap_find.js
```
function main() {
var v4 = [13.37];
var v6 = [1337,1337,1337,1337];
var v7 = [1337,Reflect,v4,"function",Reflect,1337,1283242462,v4,1337,v4];
var v8 = {b:v7,c:v7,d:1283242462,e:1337};
var v9 = {__proto__:v6};
var v10 = 1283242462;
var v15 = [1000.0,1000.0,1000.0];
var v17 = [1337,1337];
var v18 = [v17,1337,v17,v17,1337,"U5XfzZnjMi",1000.0,785945262,Int32Array,"U5XfzZnjMi"];
var v19 = {__proto__:Int32Array,d:v15,e:v15};
var v20 = {__proto__:v15,a:Int32Array,b:"U5XfzZnjMi",constructor:1337,d:1000.0,e:v18};
var v21 = 1337;
var v26 = [13.37,13.37,13.37,13.37];
var v28 = [1337,1337,1337,1337,1337];
var v29 = [v26,Promise,"caller","caller"];
var v30 = {a:-2510604095,c:-2510604095,d:"caller",e:v29,valueOf:-2510604095};
var v31 = {__proto__:Promise,a:"caller",c:13.37,constructor:"caller",toString:v28,valueOf:1337};
var v32 = v30;
var v37 = [13.37,13.37,13.37,13.37];
var v38 = [v37,Promise,"caller",v26];
var v41 = 0;
var v43 = [13.37,13.37,13.37];
for (var v44 in v43) {
    var v46 = Function();
    var v47 = v43.pop();
}
var v51 = [v41,13.37,v20];
var v52 = ["trFYu6cu5w"];
var v53 = {__proto__:v51,b:1,constructor:13.37,valueOf:v52};
async function v55(v56,v57,v58,v59,v60) {
    for (var v61 of v59) {
        var v62 = await v60;
    }
    v32 = 13.37;
    var v64 = v56(v56,v59,Infinity,13.37,"4eHA6Qb60z");
    var v65 = v59[1337];
    for (var v66 of v59) {
    }
    var v68 = parseFloat("4eHA6Qb60z");
    return v38;
}
var v71 = v55(13.37,13.37,"EPSILON","EPSILON");
var v73 = [2995767872,2995767872,2995767872,2995767872,2995767872];
var v74 = [...v73,..."QRtZ09jMcv"];
var v77 = ["trFYu6cu5w"];
var v78 = "EPSILON".concat(v73,v38,"5PS2Lo7K4R");
var v80 = [13.37,13.37,13.37,13.37];
async function v81(v82,v83,v84,v85,v86) {
}
var v88 = v81(13.37,13.37,v80);
async function v90(v91,v92,v93,v94,v95) {
    var v96 = v90(1,v93,v77,v92,-1.7976931348623157e+308);
    var v100 = [13.37,13.37,"EPSILON",13.37];
    var v103 = [-822000.4262047571,-822000.4262047571,-822000.4262047571,-822000.4262047571];
    var v104 = new Float32Array(v103);
    var v106 = {b:v100,c:1337,constructor:v100,e:1337};
    var v107 = v104 + v106;
    var v108 = v107(...v107);
    var v110 = RegExp(2);
    return 1;
}
var v111 = v90(1,1,v52,v53);
// Stderr:
}
main();
```

###### Execution steps

```
$ ls 
Memory_corruption_ecma_property_hashmap_find.js
$ ../jerryscript/build/bin/jerry Memory_corruption_ecma_property_hashmap_find.js
ASAN:SIGSEGV
=================================================================
==4638==ERROR: AddressSanitizer: SEGV on unknown address 0x000094c9c590 (pc 0x0000004177a3 bp 0x7ffff80755e0 sp 0x7ffff8075540 T0)
    #0 0x4177a2 in ecma_property_hashmap_find /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-property-hashmap.c:480
    #1 0x414869 in ecma_find_named_property /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-helpers.c:608
    #2 0x42ae2c in ecma_op_object_get_own_property /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-objects.c:240
    #3 0x42b0d2 in ecma_op_ordinary_object_has_own_property /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-objects.c:3019
    #4 0x42b0d2 in ecma_op_object_has_property /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-objects.c:373
    #5 0x47799e in ecma_op_put_value_lex_env_base /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-get-put-value.c:264
    #6 0x45d66c in vm_loop /home/test/AST/jerryscript/jerry-core/vm/vm.c:4322
    #7 0x45e526 in vm_execute /home/test/AST/jerryscript/jerry-core/vm/vm.c:4675
    #8 0x45352e in opfunc_resume_executable_object /home/test/AST/jerryscript/jerry-core/vm/opcodes.c:825
    #9 0x4285a8 in ecma_process_promise_async_reaction_job /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-jobqueue.c:319
    #10 0x4285a8 in ecma_process_all_enqueued_jobs /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-jobqueue.c:529
    #11 0x402a94 in main /home/test/AST/jerryscript/jerry-main/main-unix.c:286
    #12 0x7fee06b9183f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)
    #13 0x404238 in _start (/home/test/AST/jerryscript/build/bin/jerry+0x404238)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-property-hashmap.c:480 ecma_property_hashmap_find
==4638==ABORTING
```

```
$ ls 
Memory_corruption_ecma_property_hashmap_find.js
$ ../jerryscript-noasan/build/bin/jerry Memory_corruption_ecma_property_hashmap_find.js
Segmentation fault (core dumped)
```

###### Backtrace

```
$ gdb -args ../../../../AST/jerryscript-noasan/build/bin/jerry Memory_corruption_ecma_property_hashmap_find.js
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
Starting program: /home/test/AST/jerryscript-noasan/build/bin/jerry Memory_corruption_ecma_property_hashmap_find.js

Program received signal SIGSEGV, Segmentation fault.
0x000000000040a05e in ecma_property_hashmap_find (hashmap_p=0x66d408 <jerry_global_heap+24792>, name_p=name_p@entry=0x667910 <jerry_global_heap+1504>, 
    property_real_name_cp=property_real_name_cp@entry=0x7fffffffe04e) at /home/test/AST/jerryscript-noasan/jerry-core/ecma/base/ecma-property-hashmap.c:480
480	    if (pair_list_p[entry_index] != ECMA_NULL_POINTER)
(gdb) x/3i $rip
=> 0x40a05e <ecma_property_hashmap_find+288>:	movzwl 0x0(%rbp,%rax,2),%edi
   0x40a063 <ecma_property_hashmap_find+293>:	mov    %r14d,%eax
   0x40a066 <ecma_property_hashmap_find+296>:	shr    $0x3,%eax
(gdb) p/x $rax
$1 = 0x52f212f4
(gdb) x/gx $rax
0x52f212f4:	Cannot access memory at address 0x52f212f4
```