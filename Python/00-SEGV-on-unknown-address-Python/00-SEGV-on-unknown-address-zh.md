
### 漏洞简介

    在对Python最新版Python 3.8.5进行分析中发现了一个空指针引用漏洞,漏洞发生在python对.pyc文件进行处理时.

    下面是奔溃信息.

    $ Python-3.8.5/python 00-SEGV-on-unknown-address-Python-3.8.5.pyc 

        Could not find platform dependent libraries <exec_prefix>
        Consider setting $PYTHONHOME to <prefix>[:<exec_prefix>]
        AddressSanitizer:DEADLYSIGNAL
        =================================================================
        ==8079==ERROR: AddressSanitizer: SEGV on unknown address 0x000000000018 (pc 0x0000008aa86b bp 0x000000000000 sp 0x7ffe2a3f5bf0 T0)
        ==8079==The signal is caused by a READ memory access.
        ==8079==Hint: address points to the zero page.
            #0 0x8aa86a in _PyEval_EvalCodeWithName /home/test/Python-3.8.5/Python/ceval.c:4266:23
            #1 0x866d0f in PyEval_EvalCodeEx /home/test/Python-3.8.5/Python/ceval.c:4327:12
            #2 0x866d0f in PyEval_EvalCode /home/test/Python-3.8.5/Python/ceval.c:718:12
            #3 0x9f7355 in run_eval_code_obj /home/test/Python-3.8.5/Python/pythonrun.c:1125:9
            #4 0x9e682d in run_pyc_file /home/test/Python-3.8.5/Python/pythonrun.c:1184:9
            #5 0x9e682d in PyRun_SimpleFileExFlags /home/test/Python-3.8.5/Python/pythonrun.c:419:13
            #6 0x9e4ca5 in PyRun_AnyFileExFlags /home/test/Python-3.8.5/Python/pythonrun.c:86:16
            #7 0x5108db in pymain_run_file /home/test/Python-3.8.5/Modules/main.c:381:15
            #8 0x5108db in pymain_run_python /home/test/Python-3.8.5/Modules/main.c:606:21
            #9 0x5108db in Py_RunMain /home/test/Python-3.8.5/Modules/main.c:685:5
            #10 0x5129d6 in pymain_main /home/test/Python-3.8.5/Modules/main.c:715:12
            #11 0x512dd7 in Py_BytesMain /home/test/Python-3.8.5/Modules/main.c:739:12
            #12 0x7f8316d4b82f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291
            #13 0x438888 in _start (/home/test/Python-3.8.5-Fuzz/python+0x438888)

        AddressSanitizer can not provide additional info.
        SUMMARY: AddressSanitizer: SEGV /home/test/Python-3.8.5/Python/ceval.c:4266:23 in _PyEval_EvalCodeWithName
        ==8079==ABORTING

### 漏洞分析

    在函数PyEval_EvalCode(Python-3.8.5/Python/ceval.c)中调用PyEval_EvalCodeEx函数.

        PyObject *
        PyEval_EvalCode(PyObject *co, PyObject *globals, PyObject *locals)
        {
            return PyEval_EvalCodeEx(co,
                            globals, locals,
                            (PyObject **)NULL, 0,
                            (PyObject **)NULL, 0,
                            (PyObject **)NULL, 0,
                            NULL, NULL);
        }

    传递给PyEval_EvalCodeEx函数的参数中closure设置为NULL.

        PyObject *
        PyEval_EvalCodeEx(PyObject *_co, PyObject *globals, PyObject *locals,
                        PyObject *const *args, int argcount,
                        PyObject *const *kws, int kwcount,
                        PyObject *const *defs, int defcount,
                        PyObject *kwdefs, PyObject *closure)
        {
            return _PyEval_EvalCodeWithName(_co, globals, locals,
                                            args, argcount,
                                            kws, kws != NULL ? kws + 1 : NULL,
                                            kwcount, 2,
                                            defs, defcount,
                                            kwdefs, closure,
                                            NULL, NULL);
        }

    PyEval_EvalCodeEx函数继续调用_PyEval_EvalCodeWithName函数,closure值不变依旧为NULL.

        PyObject *
        _PyEval_EvalCodeWithName(PyObject *_co, PyObject *globals, PyObject *locals,
                PyObject *const *args, Py_ssize_t argcount,
                PyObject *const *kwnames, PyObject *const *kwargs,
                Py_ssize_t kwcount, int kwstep,
                PyObject *const *defs, Py_ssize_t defcount,
                PyObject *kwdefs, PyObject *closure,
                PyObject *name, PyObject *qualname)
        {
            ******
            /* Copy closure variables to free variables */
            for (i = 0; i < PyTuple_GET_SIZE(co->co_freevars); ++i) {
                PyObject *o = PyTuple_GET_ITEM(closure, i);   <----------------------------- crash
                Py_INCREF(o);
                freevars[PyTuple_GET_SIZE(co->co_cellvars) + i] = o;
            }
            ******
        }

    在_PyEval_EvalCodeWithName函数中对closure进行解引用,此时closure值为NULL,触发空指针引用漏洞.

### 修复建议

    在 _PyEval_EvalCodeWithName函数中引用closure前，对closure的值进行判断.