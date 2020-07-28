###### JerryScript revision

commit : https://github.com/jerryscript-project/jerryscript/commit/227007eda75d86db1bb32ea380cf9650122d6920
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

main.js
```
print('esm');
import * as Foo from './foo.js';
print(Foo.x);
print(Foo.y());
print(Foo.z);
print(Foo.shortVar);
print(Foo.a);
print(Foo.b);
import { y, z as zAliased } from './foo.js';
print(y(), zAliased);
print(FooDefault());
import FooDefault from './foo.js';
```

foo.js
```
export var x = 42;
export function y() {
  return 182;
}
export default function() {
  return 352;
}
var myLongVariableName = 472;
var shortVar = 157;
export {myLongVariableName as z, shortVar};
export * from './bar.js';
```

bar.js
```
export var a = 24;
export var b = 1845;
export default 843;
```

###### Execution steps

```
$ ls 
bar.js  foo.js  main.js
$ ../jerryscript-asan/build/bin/jerry main.js 
ASAN:SIGSEGV
=================================================================
==73016==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000000 (pc 0x000000413b68 bp 0x000000000000 sp 0x7ffc47d006f8 T0)
    #0 0x413b67 in ecma_get_lex_env_type /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-helpers.c:276
    #1 0x4753a9 in ecma_op_get_value_lex_env_base /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-get-put-value.c:56
    #2 0x462bbf in ecma_module_namespace_object_add_export_if_needed /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-module.c:507
    #3 0x4637eb in ecma_module_namespace_object_add_export_if_needed /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-module.c:486
    #4 0x4637eb in ecma_module_create_namespace_object /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-module.c:573
    #5 0x4637eb in ecma_module_connect_imports /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-module.c:735
    #6 0x4637eb in ecma_module_initialize_current /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-module.c:821
    #7 0x46068a in vm_run_global /home/test/AST/jerryscript/jerry-core/vm/vm.c:328
    #8 0x40653a in jerry_run /home/test/AST/jerryscript/jerry-core/api/jerry.c:579
    #9 0x402c6f in main /home/test/AST/jerryscript/jerry-main/main-unix.c:759
    #10 0x7f0fe6b9083f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2083f)
    #11 0x404788 in _start (/home/test/AST/jerryscript-asan/build/bin/jerry+0x404788)

AddressSanitizer can not provide additional info.
SUMMARY: AddressSanitizer: SEGV /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-helpers.c:276 ecma_get_lex_env_type
==73016==ABORTING
```

```
$ ls 
bar.js  foo.js  main.js
$ ../jerryscript/build/bin/jerry main.js 
Segmentation fault (core dumped)
```

###### Backtrace

```
$ gdb -arg ../jerryscript/build/bin/jerry main.js 
(gdb) r
Starting program: /home/test/AST/jerryscript/build/bin/jerry main.js

Program received signal SIGSEGV, Segmentation fault.
ecma_get_lex_env_type (object_p=object_p@entry=0x0) at /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-helpers.c:276
276	  return (ecma_lexical_environment_type_t) (object_p->type_flags_refs & ECMA_OBJECT_TYPE_MASK);
(gdb) bt
#0  ecma_get_lex_env_type (object_p=object_p@entry=0x0) at /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-helpers.c:276
#1  0x00000000004382e0 in ecma_op_get_value_lex_env_base (lex_env_p=0x0, ref_base_lex_env_p=0x7fffffffdae0, name_p=0x764ad0 <jerry_global_heap+1928>)
    at /home/test/AST/jerryscript/jerry-core/ecma/operations/ecma-get-put-value.c:56
#2  0x000000000042c0ae in ecma_module_namespace_object_add_export_if_needed (module_p=module_p@entry=0x764658 <jerry_global_heap+784>, export_name_p=export_name_p@entry=0x764ad0 <jerry_global_heap+1928>)
    at /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-module.c:507
#3  0x000000000042c47f in ecma_module_namespace_object_add_export_if_needed (export_name_p=0x764ad0 <jerry_global_heap+1928>, module_p=0x764658 <jerry_global_heap+784>)
    at /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-module.c:486
#4  ecma_module_create_namespace_object (module_p=0x764658 <jerry_global_heap+784>) at /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-module.c:573
#5  ecma_module_connect_imports () at /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-module.c:735
#6  ecma_module_initialize_current () at /home/test/AST/jerryscript/jerry-core/ecma/base/ecma-module.c:821
#7  0x000000000042aa72 in vm_run_global (bytecode_p=0x764870 <jerry_global_heap+1320>) at /home/test/AST/jerryscript/jerry-core/vm/vm.c:328
#8  0x0000000000402b90 in jerry_run (func_val=<optimized out>) at /home/test/AST/jerryscript/jerry-core/api/jerry.c:579
#9  0x0000000000401479 in main (argc=<optimized out>, argv=<optimized out>) at /home/test/AST/jerryscript/jerry-main/main-unix.c:759
(gdb) x/5i $rip
=> 0x408dc4 <ecma_get_lex_env_type>:	mov    (%rdi),%ax
   0x408dc7 <ecma_get_lex_env_type+3>:	and    $0xf,%eax
   0x408dca <ecma_get_lex_env_type+6>:	retq   
   0x408dcb <ecma_get_lex_env_binding_object>:	movzwl 0x4(%rdi),%edi
   0x408dcf <ecma_get_lex_env_binding_object+4>:	jmpq   0x41bc29 <jmem_decompress_pointer>
(gdb) p/x $rdi
$1 = 0x0
```

