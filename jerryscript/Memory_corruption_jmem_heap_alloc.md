
Memory corruption in in jmem_heap_alloc (jerryscript/jerry-core/jmem/jmem-heap.c:189)

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

xxxxxxxxxxxxx.js
```
function main() {
var v4 = [13.37,13.37,13.37];
var v6 = [1337,1337,1337];
var v7 = [];
var v8 = {b:13.37,d:565692808};
var v9 = {__proto__:v6,b:DataView,d:"match",e:v8};
var v10 = v7;
var v15 = [13.37,13.37,13.37];
var v17 = v15.reverse();
var v19 = String;
var v20 = v17.reduceRight(v19);
var v21 = String.fromCharCode(String,1337);
var v22 = RegExp(v21);
var v25 = [13.37,13.37];
async function v27(v28,v29,v30,v31,v32) {
    var v34 = RegExp();
    return String;
}
var v35 = [13.37,13.37,"uQXGfNINNe",JSON,13.37,1337,-65535,"uQXGfNINNe",v25];
var v36 = {};
var v38 = Symbol.search;
function v39(v40,v41,v42,v43,v44) {
    return v7;
}
var v45 = {b:v35,c:-65535,constructor:1337,d:v35,e:13.37,length:JSON};
async function v46(v47,v48,v49,v50,v51) {
}
var v54 = Math.cos();
var v57 = new Map(arguments);
var v62 = [13.37,13.37,13.37,13.37];
var v63 = [v62,Promise,"caller","caller"];
var v64 = {a:-2510604095,c:-2510604095,d:"caller",e:v63,valueOf:-2510604095};
var v67 = new Int8Array(34809);
for (var v69 in "mgGiYOWJuj") {
    var v70 = v69.match();
}
async function v71(v72,v73,v74,v75,v76) {
    function* v79(v80,v81,v82,v83,v84) {
        yield* v83;
        return -2546168978;
    }
    var v85 = v79("MIN_VALUE",-1165603609,..."MIN_VALUE",-1165603609);
    var v87 = new WeakMap(v85);
}
var v92 = [1337,13.37,13.37,13.37];
var v93 = ["EPSILON"];
var v94 = v71(13.37,13.37);
var v97 = [1337,1337,13.37];
var v98 = v67[-3176153122];
var v99 = RegExp();
var v101 = undefined;
var v105 = ["trFYu6cu5w"];
async function v107(v108,v109,v110,v111,v112) {
    var v113 = v107(1,v110,v105);
    var v115 = Symbol.match;
    v110[v115] = v64;
}
var v116 = RegExp(..."function");
var v117 = v107();
// Stderr:
}
main();
```

###### Execution steps

1. Username must contains 4 letters (example: xxxx. x can be any letters.).
2. js file must in /home/xxxx/xxx/xxxxxxx/xxxxxx/xxxxxxxx/ (x can be any letters).
3. js filename must contains 13 letters (example: xxxxxxxxxxxxx.js. x can be any letters).
4. run with jsfile's full path (example: /home/xxxx/xxx/xxxxxxx/xxxxxx/xxxxxxxx/xxxxxxxxxxxxx.js. x can be any letters).

```
$ ls 
xxxxxxxxxxxxx.js
$ pwd
/home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx
$ ../jerryscript/build/bin/jerry /home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx/xxxxxxxxxxxxx.js
ASAN:SIGSEGV
=================================================================
==4910==ERROR: AddressSanitizer: SEGV on unknown address 0x00001446a628 (pc 0x000000403b24 bp 0x000000000000 sp 0x7ffe258fc030 T0)
    #0 0x403b23 in jmem_heap_alloc /home/test/AST/jerryscript/jerry-core/jmem/jmem-heap.c:189
    #1 0x439ab7 in jmem_heap_gc_and_alloc_block /home/test/AST/jerryscript/jerry-core/jmem/jmem-heap.c:296
    #2 0x44d562 in re_initialize_regexp_bytecode /home/test/AST/jerryscript/jerry-core/parser/regexp/re-bytecode.c:37
    #3 0x44df4e in re_compile_bytecode /home/test/AST/jerryscript/jerry-core/parser/regexp/re-compiler.c:121
    #4 0x435576 in ecma_op_create_regexp_from_pattern /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-regexp-object.c:336
    #5 0x473798 in ecma_builtin_regexp_dispatch_helper /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtin-regexp.c:174
    #6 0x41b42b in ecma_builtin_dispatch_call /home/test/AST/jerryscript/jerry-core/ecma/builtin-objects/ecma-builtins.c:1213
    #7 0x4260e1 in ecma_op_function_call /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-function-object.c:1142
    #8 0x45f09c in vm_spread_operation /home/test/AST/jerryscript/jerry-core/vm/vm.c:694
    #9 0x45f09c in vm_execute /home/test/AST/jerryscript/jerry-core/vm/vm.c:4692
    #10 0x45f78b in vm_run /home/test/AST/jerryscript/jerry-core/vm/vm.c:4783
    #11 0x424c34 in ecma_op_function_call_simple /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-function-object.c:943
    #12 0x4260e1 in ecma_op_function_call /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-function-object.c:1142
    #13 0x45e6f1 in opfunc_call /home/test/AST/jerryscript/jerry-core/vm/vm.c:778
    #14 0x45e6f1 in vm_execute /home/test/AST/jerryscript/jerry-core/vm/vm.c:4681
    #15 0x45f78b in vm_run /home/test/AST/jerryscript/jerry-core/vm/vm.c:4783
    #16 0x406a59 in jerry_run /home/test/AST/jerryscript/jerry-core/api/jerry.c:579
    #17 0x4026db in main /home/test/AST/jerryscript/jerry-main/main-unix.c:123
    #18 0x7f821826083f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)
    #19 0x404238 in _start (/home/test/AST/jerryscript/build/bin/jerry+0x404238)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /home/test/AST/jerryscript/jerry-core/jmem/jmem-heap.c:189 jmem_heap_alloc
==4910==ABORTING

```

```
$ ls 
xxxxxxxxxxxxx.js
$ pwd
/home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx
$ ../jerryscript-noasan/build/bin/jerry /home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx/xxxxxxxxxxxxx.js
Segmentation fault (core dumped)
```

###### Backtrace

```
$ gdb -args ../../../../AST/jerryscript-noasan/build/bin/jerry /home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx/xxxxxxxxxxxxx.js
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
Starting program: /home/test/AST/jerryscript-noasan/build/bin/jerry /home/test/xxx/xxxxxxx/xxxxxx/xxxxxxxx/xxxxxxxxxxxxx.js

Program received signal SIGSEGV, Segmentation fault.
jmem_heap_alloc (size=size@entry=20) at /home/test/AST/jerryscript-noasan/jerry-core/jmem/jmem-heap.c:185
185	      const uint32_t next_offset = current_p->next_offset;
(gdb) x/3i $rip
=> 0x4014c8 <jmem_heap_alloc+182>:	mov    0x667338(%rdx),%edx
   0x4014ce <jmem_heap_alloc+188>:	mov    0x4(%rax),%ecx
   0x4014d1 <jmem_heap_alloc+191>:	cmp    %rcx,%rdi
(gdb) p/x $rdx
$1 = 0x13d90070
(gdb) x/gx 0x13d90070
0x13d90070:	Cannot access memory at address 0x13d90070
```