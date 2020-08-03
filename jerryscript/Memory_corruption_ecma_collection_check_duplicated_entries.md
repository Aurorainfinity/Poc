
Memory corruption in in ecma_collection_check_duplicated_entries (jerryscript/jerry-core/ecma/base/ecma-helpers-collection.c:204)

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

Memory_corruption_ecma_collection_check_duplicated_entries.js
```
function main() {
var v4 = Array("5PS2Lo7K4R",7,7,Array,..."5PS2Lo7K4R");
var v6 = "5PS2Lo7K4R" || "5PS2Lo7K4R";
var v9 = "FyENNPuYX*".match(0);
function v10(v11,v12) {
}
var v15 = ["trFYu6cu5w"];
async function v17(v18,v19,v20,v21,v22) {
    var v23 = v17(1,v20,v15,v19,13.37);
    var v27 = [-370590.12831863505,-370590.12831863505,-370590.12831863505,-370590.12831863505];
    var v29 = {b:v27,c:1337,constructor:v27,e:1337};
    var v31 = new Proxy(v29,Object);
    var v32 = new Proxy(v31,Boolean);
    var v34 = {__proto__:v32};
    var v36 = Object.seal(v31,"number",v34);
}
var v39 = v17(13.37,13.37,"EPSILON","EPSILON",v17);
var v41 = [13.37];
var v43 = [1337,1337];
var v44 = v41[-4096];
var v46 = 0;
var v47 = v46 + 1;
var v49 = 0;
var v50 = v49 + 1;
var v52 = undefined;
var v53 = [1337,1337,v41,1337,"5PS2Lo7K4R",v43,v41];
var v54 = {b:Int32Array,d:7,length:"5PS2Lo7K4R",toString:v41};
var v55 = {__proto__:1337,a:v53,b:v41,constructor:Int32Array,length:7,toString:Int32Array,valueOf:13.37};
var v56 = 7;
var v61 = [13.37,13.37,13.37];
var v62 = Float32Array | v61;
var v63 = 0;
var v64 = !13.37;
var v65 = v63 + 1;
v63 = v65;
var v67 = "trFYu6cu5w".__proto__;
var v70 = 0;
var v71 = v70 + 1;
v70 = v71;
try {
    var v72 = ~v67;
} catch(v73) {
    var v76 = [13.37,13.37,13.37];
    var v78 = 1337;
    var v80 = new Uint16Array(v78);
    var v81 = 0;
    var v82 = v80 & 13.37;
    var v84 = undefined;
    var v86 = [13.37,13.37,13.37];
    function v87(v88,v89) {
    }
}
var v92 = new Uint16Array(60947);
var v95 = 7;
var v96 = "trFYu6cu5w"[v95];
var v99 = ["EPSILON"];
var v100 = v99;
var v103 = String;
var v106 = Array(128);
var v107 = ["number"];
var v108 = v107;
var v110 = undefined;
var v112 = [v100];
var v113 = Math.round;
var v114 = Reflect.apply(v113,v96,v112);
var v117 = 0;
var v118 = v117 + 1;
v117 = v118;
var v122 = [13.37];
var v124 = [1337];
var v125 = [v122,13.37];
var v126 = {constructor:v125,d:Proxy,e:Proxy,length:"string",valueOf:v125};
var v127 = {};
var v128 = v122;
var v129 = {};
function* v132(v133,v134) {
    return v124;
}
var v135 = [-370590.12831863505,-370590.12831863505,-370590.12831863505,-370590.12831863505];
var v137 = {b:v135,c:64,constructor:v135,e:64};
var v138 = {call:v132,length:v129,getOwnPropertyDescriptor:v132,has:v132,isExtensible:v132,ownKeys:v132,preventExtensions:v132,set:v132};
var v140 = new Proxy(v137,v138);
var v141 = [1337,1337,1337,1337];
var v143 = JSON.stringify;
var v144 = v141.reduceRight(v143,v140);
// Stderr:
}
main();
```

###### Execution steps

```
$ ls 
Memory_corruption_ecma_collection_check_duplicated_entries.js
$ ../jerryscript/build/bin/jerry Memory_corruption_ecma_collection_check_duplicated_entries.js
ASAN:SIGSEGV
=================================================================
==4583==ERROR: AddressSanitizer: SEGV on unknown address 0x00000075b000 (pc 0x00000040d100 bp 0x00000001ff06 sp 0x7ffe8579ba90 T0)
    #0 0x40d0ff in ecma_collection_check_duplicated_entries /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-helpers-collection.c:204
    #1 0x431e55 in ecma_proxy_object_own_property_keys /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-proxy-object.c:1574
    #2 0x42ccf8 in ecma_op_object_own_property_keys /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-objects.c:2213
    #3 0x42d604 in ecma_op_object_get_enumerable_property_names /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-objects.c:1904
    #4 0x418291 in ecma_builtin_json_serialize_object /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtin-json.c:988
    #5 0x418291 in ecma_builtin_json_serialize_property /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtin-json.c:1380
    #6 0x41858b in ecma_builtin_json_str_helper /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtin-json.c:1419
    #7 0x419c2b in ecma_builtin_json_stringify /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtin-json.c:1694
    #8 0x419c2b in ecma_builtin_json_dispatch_routine /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtin-json.inc.h:34
    #9 0x41b3e1 in ecma_builtin_dispatch_routine /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtins.c:1181
    #10 0x41b3e1 in ecma_builtin_dispatch_call /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtins.c:1205
    #11 0x4260e1 in ecma_op_function_call /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-function-object.c:1142
    #12 0x465eed in ecma_builtin_array_reduce_from /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtin-array-prototype.c:2239
    #13 0x465eed in ecma_builtin_array_prototype_dispatch_routine /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtin-array-prototype.c:2855
    #14 0x41b3e1 in ecma_builtin_dispatch_routine /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtins.c:1181
    #15 0x41b3e1 in ecma_builtin_dispatch_call /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtins.c:1205
    #16 0x4260e1 in ecma_op_function_call /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-function-object.c:1142
    #17 0x45e6f1 in opfunc_call /home/test/AST/jerryscript/jerry-core/vm/vm.c:778
    #18 0x45e6f1 in vm_execute /home/test/AST/jerryscript/jerry-core/vm/vm.c:4681
    #19 0x45f78b in vm_run /home/test/AST/jerryscript/jerry-core/vm/vm.c:4783
    #20 0x424c34 in ecma_op_function_call_simple /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-function-object.c:943
    #21 0x4260e1 in ecma_op_function_call /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-function-object.c:1142
    #22 0x45e6f1 in opfunc_call /home/test/AST/jerryscript/jerry-core/vm/vm.c:778
    #23 0x45e6f1 in vm_execute /home/test/AST/jerryscript/jerry-core/vm/vm.c:4681
    #24 0x45f78b in vm_run /home/test/AST/jerryscript/jerry-core/vm/vm.c:4783
    #25 0x406a59 in jerry_run /home/test/AST/jerryscript/jerry-core/api/jerry.c:579
    #26 0x4026db in main /home/test/AST/jerryscript/jerry-main/main-unix.c:123
    #27 0x7f767c5e983f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)
    #28 0x404238 in _start (/home/test/AST/jerryscript/build/bin/jerry+0x404238)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-helpers-collection.c:204 ecma_collection_check_duplicated_entries
==4583==ABORTING
```

```
$ ls 
Memory_corruption_ecma_collection_check_duplicated_entries.js
$ ../jerryscript-noasan/build/bin/jerry Memory_corruption_ecma_collection_check_duplicated_entries.js
Segmentation fault (core dumped)
```

###### Backtrace

```
$ gdb -args ../../../../AST/jerryscript-noasan/build/bin/jerry Memory_corruption_ecma_collection_check_duplicated_entries.js
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
Starting program: /home/test/AST/jerryscript-noasan/build/bin/jerry Memory_corruption_ecma_collection_check_duplicated_entries.js

Program received signal SIGSEGV, Segmentation fault.
ecma_collection_check_duplicated_entries (collection_p=collection_p@entry=0x668840 <jerry_global_heap+5392>) at /home/test/AST/jerryscript-noasan/jerry-core/ecma/base/ecma-helpers-collection.c:204
204	    ecma_string_t *current_name_p = ecma_get_prop_name_from_value (buffer_p[i]);
(gdb) x/3i $rip
=> 0x405d8d <ecma_collection_check_duplicated_entries+31>:	mov    0x0(%r13,%rax,4),%edi
   0x405d92 <ecma_collection_check_duplicated_entries+36>:	mov    %ebx,%ebp
   0x405d94 <ecma_collection_check_duplicated_entries+38>:	callq  0x4088d3 <ecma_get_prop_name_from_value>
(gdb) p/x $rax
$1 = 0x281ec
(gdb) x/gx $rax
0x281ec:	Cannot access memory at address 0x281ec
```