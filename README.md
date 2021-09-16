I have this working on an older AMI - but a work production one I can't just clone in the sandbox - all else fails, might have to work that out.  Or pack it.  Took a while to get going though and quite a few tweaks.  So the installs are relevant to a particular project, but still pretty generic.

Basic aim is to get an automated Spot Fleet setup so that you can train cheaply - and if it interrupts, it will start again.

# AWS-EC2-Deep-Learning-AMI-GDAL-Install-Problems
Problems encountered with trying to get this to work

## System Libraries
The default install has none.
If you:
- sudo apt-get install gdal-bin libgdal-dev
- gdal-config --version
- 
1.1.3

This is not supported with a pip install - too old.

### Next
- sudo add-apt-repository -y ppa:ubuntugis/ppa
- sudo apt-get update 
- gdal-config --version

2.2.4 

# tensorflow_p37 environment

- source activate tensorflow_p37
- pip3 install --global-option=build_ext --global-option="-I/usr/include/gdal" GDAL==`gdal-config --version`

```python
/home/ubuntu/anaconda3/envs/tensorflow_p37/lib/python3.7/site-packages/pip/_internal/commands/install.py:229: UserWarning: Disabling all use of wheels due to the use of --build-option / --global-option / --install-option.
  cmdoptions.check_install_build_global(options)
Looking in indexes: https://pypi.org/simple, https://pip.repos.neuron.amazonaws.com
Collecting GDAL==2.2.2
  Using cached GDAL-2.2.2.tar.gz (475 kB)
Skipping wheel build for GDAL, due to binaries being disabled for it.
Installing collected packages: GDAL
    Running setup.py install for GDAL ... error
    ERROR: Command errored out with exit status 1:
     command: /home/ubuntu/anaconda3/envs/tensorflow_p37/bin/python -u -c 'import io, os, sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-zg6etg37/gdal_4dc08e115ce941d0aa842f9b762bb6c2/setup.py'"'"'; __file__='"'"'/tmp/pip-install-zg6etg37/gdal_4dc08e115ce941d0aa842f9b762bb6c2/setup.py'"'"';f = getattr(tokenize, '"'"'open'"'"', open)(__file__) if os.path.exists(__file__) else io.StringIO('"'"'from setuptools import setup; setup()'"'"');code = f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' build_ext -I/usr/include/86_64-linux-gnu/gdal install --record /tmp/pip-record-akt7qgow/install-record.txt --single-version-externally-managed --compile --install-headers /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/GDAL
         cwd: /tmp/pip-install-zg6etg37/gdal_4dc08e115ce941d0aa842f9b762bb6c2/
    Complete output (14 lines):
    running build_ext
    building 'osgeo._gdal' extension
    creating build
    creating build/temp.linux-x86_64-3.7
    creating build/temp.linux-x86_64-3.7/extensions
    /home/ubuntu/anaconda3/envs/tensorflow_p37/bin/x86_64-conda-linux-gnu-cc -Wno-unused-result -Wsign-compare -DNDEBUG -fwrapv -O2 -Wall -Wstrict-prototypes -march=nocona -mtune=haswell -ftree-vectorize -fPIC -fstack-protector-strong -fno-plt -O2 -pipe -march=nocona -mtune=haswell -ftree-vectorize -fPIC -fstack-protector-strong -fno-plt -O2 -pipe -march=nocona -mtune=haswell -ftree-vectorize -fPIC -fstack-protector-strong -fno-plt -O2 -ffunction-sections -pipe -isystem /home/ubuntu/anaconda3/envs/tensorflow_p37/include -DNDEBUG -D_FORTIFY_SOURCE=2 -O2 -isystem /home/ubuntu/anaconda3/envs/tensorflow_p37/include -fPIC -I/usr/include/86_64-linux-gnu/gdal -I/home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m -I/home/ubuntu/anaconda3/envs/tensorflow_p37/lib/python3.7/site-packages/numpy/core/include -I/usr/include -c extensions/gdal_wrap.cpp -o build/temp.linux-x86_64-3.7/extensions/gdal_wrap.o
    cc1plus: warning: command line option '-Wstrict-prototypes' is valid for C/ObjC but not for C++
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/Python.h:34,
                     from extensions/gdal_wrap.cpp:173:
    /usr/include/stdlib.h:759:11: fatal error: bits/stdlib-bsearch.h: No such file or directory
      759 | # include <bits/stdlib-bsearch.h>
          |           ^~~~~~~~~~~~~~~~~~~~~~~
    compilation terminated.
    error: command '/home/ubuntu/anaconda3/envs/tensorflow_p37/bin/x86_64-conda-linux-gnu-cc' failed with exit status 1
    ----------------------------------------
ERROR: Command errored out with exit status 1: /home/ubuntu/anaconda3/envs/tensorflow_p37/bin/python -u -c 'import io, os, sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-zg6etg37/gdal_4dc08e115ce941d0aa842f9b762bb6c2/setup.py'"'"'; __file__='"'"'/tmp/pip-install-zg6etg37/gdal_4dc08e115ce941d0aa842f9b762bb6c2/setup.py'"'"';f = getattr(tokenize, '"'"'open'"'"', open)(__file__) if os.path.exists(__file__) else io.StringIO('"'"'from setuptools import setup; setup()'"'"');code = f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' build_ext -I/usr/include/86_64-linux-gnu/gdal install --record /tmp/pip-record-akt7qgow/install-record.txt --single-version-externally-managed --compile --install-headers /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/GDAL Check the logs for full command output.
```
## Fix
-sudo cp -r /usr/include/x86_64-linux-gnu/bits /usr/include/bits

# Next error
```python
(tensorflow_p37) ubuntu@ip-172-31-21-97:~$ pip3 install --global-option=build_ext --global-option="-I/usr/include/gdal" GDAL==`gdal-config --version`
/home/ubuntu/anaconda3/envs/tensorflow_p37/lib/python3.7/site-packages/pip/_internal/commands/install.py:229: UserWarning: Disabling all use of wheels due to the use of --build-option / --global-option / --install-option.
  cmdoptions.check_install_build_global(options)
Looking in indexes: https://pypi.org/simple, https://pip.repos.neuron.amazonaws.com
Collecting GDAL==2.2.2
  Using cached GDAL-2.2.2.tar.gz (475 kB)
Skipping wheel build for GDAL, due to binaries being disabled for it.
Installing collected packages: GDAL
    Running setup.py install for GDAL ... error
    ERROR: Command errored out with exit status 1:
     command: /home/ubuntu/anaconda3/envs/tensorflow_p37/bin/python -u -c 'import io, os, sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-w_2ga_xs/gdal_8225fcb369174c5585286229e56d9881/setup.py'"'"'; __file__='"'"'/tmp/pip-install-w_2ga_xs/gdal_8225fcb369174c5585286229e56d9881/setup.py'"'"';f = getattr(tokenize, '"'"'open'"'"', open)(__file__) if os.path.exists(__file__) else io.StringIO('"'"'from setuptools import setup; setup()'"'"');code = f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' build_ext -I/usr/include/gdal install --record /tmp/pip-record-e37qte8n/install-record.txt --single-version-externally-managed --compile --install-headers /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/GDAL
         cwd: /tmp/pip-install-w_2ga_xs/gdal_8225fcb369174c5585286229e56d9881/
    Complete output (306 lines):
    running build_ext
    building 'osgeo._gdal' extension
    creating build
    creating build/temp.linux-x86_64-3.7
    creating build/temp.linux-x86_64-3.7/extensions
    /home/ubuntu/anaconda3/envs/tensorflow_p37/bin/x86_64-conda-linux-gnu-cc -Wno-unused-result -Wsign-compare -DNDEBUG -fwrapv -O2 -Wall -Wstrict-prototypes -march=nocona -mtune=haswell -ftree-vectorize -fPIC -fstack-protector-strong -fno-plt -O2 -pipe -march=nocona -mtune=haswell -ftree-vectorize -fPIC -fstack-protector-strong -fno-plt -O2 -pipe -march=nocona -mtune=haswell -ftree-vectorize -fPIC -fstack-protector-strong -fno-plt -O2 -ffunction-sections -pipe -isystem /home/ubuntu/anaconda3/envs/tensorflow_p37/include -DNDEBUG -D_FORTIFY_SOURCE=2 -O2 -isystem /home/ubuntu/anaconda3/envs/tensorflow_p37/include -fPIC -I/usr/include/gdal -I/home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m -I/home/ubuntu/anaconda3/envs/tensorflow_p37/lib/python3.7/site-packages/numpy/core/include -I/usr/include -c extensions/gdal_wrap.cpp -o build/temp.linux-x86_64-3.7/extensions/gdal_wrap.o
    cc1plus: warning: command line option '-Wstrict-prototypes' is valid for C/ObjC but not for C++
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/Python.h:25,
                     from extensions/gdal_wrap.cpp:173:
    /usr/include/stdio.h:365:45: error: expected initializer before '__THROWNL'
      365 |       const char *__restrict __format, ...) __THROWNL;
          |                                             ^~~~~~~~~
    /usr/include/stdio.h:380:26: error: expected initializer before '__THROWNL'
      380 |        _G_va_list __arg) __THROWNL;
          |                          ^~~~~~~~~
    /usr/include/stdio.h:388:6: error: expected initializer before '__THROWNL'
      388 |      __THROWNL __attribute__ ((__format__ (__printf__, 3, 4)));
          |      ^~~~~~~~~
    /usr/include/stdio.h:392:6: error: expected initializer before '__THROWNL'
      392 |      __THROWNL __attribute__ ((__format__ (__printf__, 3, 0)));
          |      ^~~~~~~~~
    /usr/include/stdio.h:401:6: error: expected initializer before '__THROWNL'
      401 |      __THROWNL __attribute__ ((__format__ (__printf__, 2, 0))) __wur;
          |      ^~~~~~~~~
    /usr/include/stdio.h:404:6: error: expected initializer before '__THROWNL'
      404 |      __THROWNL __attribute__ ((__format__ (__printf__, 2, 3))) __wur;
          |      ^~~~~~~~~
    /usr/include/stdio.h:407:6: error: expected initializer before '__THROWNL'
      407 |      __THROWNL __attribute__ ((__format__ (__printf__, 2, 3))) __wur;
          |      ^~~~~~~~~
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/Python.h:25,
                     from extensions/gdal_wrap.cpp:173:
    /usr/include/stdio.h:900:6: error: expected initializer before '__THROWNL'
      900 |      __THROWNL __attribute__ ((__format__ (__printf__, 2, 3)));
          |      ^~~~~~~~~
    /usr/include/stdio.h:904:6: error: expected initializer before '__THROWNL'
      904 |      __THROWNL __attribute__ ((__format__ (__printf__, 2, 0)));
          |      ^~~~~~~~~
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/Python.h:34,
                     from extensions/gdal_wrap.cpp:173:
    /usr/include/stdlib.h:510:35: error: expected initializer before '__attribute_alloc_size__'
      510 |      __THROW __attribute_malloc__ __attribute_alloc_size__ ((2)) __wur;
          |                                   ^~~~~~~~~~~~~~~~~~~~~~~~
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/Python.h:36,
                     from extensions/gdal_wrap.cpp:173:
    /usr/include/unistd.h:759:28: error: expected initializer before '__THROWNL'
      759 | extern __pid_t fork (void) __THROWNL;
          |                            ^~~~~~~~~
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/pythread.h:114,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/pystate.h:11,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/traceback.h:8,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/Python.h:119,
                     from extensions/gdal_wrap.cpp:173:
    /usr/include/pthread.h:236:31: error: expected initializer before '__THROWNL'
      236 |       void *__restrict __arg) __THROWNL __nonnull ((1, 3));
          |                               ^~~~~~~~~
    /usr/include/pthread.h:743:70: error: expected initializer before '__THROWNL'
      743 | extern int __sigsetjmp (struct __jmp_buf_tag *__env, int __savemask) __THROWNL;
          |                                                                      ^~~~~~~~~
    /usr/include/pthread.h:759:6: error: expected initializer before '__THROWNL'
      759 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:763:6: error: expected initializer before '__THROWNL'
      763 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:769:20: error: expected initializer before '__THROWNL'
      769 |         __abstime) __THROWNL __nonnull ((1, 2));
          |                    ^~~~~~~~~
    /usr/include/pthread.h:774:6: error: expected initializer before '__THROWNL'
      774 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:898:6: error: expected initializer before '__THROWNL'
      898 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:902:3: error: expected initializer before '__THROWNL'
      902 |   __THROWNL __nonnull ((1));
          |   ^~~~~~~~~
    /usr/include/pthread.h:908:23: error: expected initializer before '__THROWNL'
      908 |            __abstime) __THROWNL __nonnull ((1, 2));
          |                       ^~~~~~~~~
    /usr/include/pthread.h:913:6: error: expected initializer before '__THROWNL'
      913 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:917:6: error: expected initializer before '__THROWNL'
      917 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:923:23: error: expected initializer before '__THROWNL'
      923 |            __abstime) __THROWNL __nonnull ((1, 2));
          |                       ^~~~~~~~~
    /usr/include/pthread.h:928:6: error: expected initializer before '__THROWNL'
      928 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:978:6: error: expected initializer before '__THROWNL'
      978 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:982:6: error: expected initializer before '__THROWNL'
      982 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:1053:6: error: expected initializer before '__THROWNL'
     1053 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:1057:6: error: expected initializer before '__THROWNL'
     1057 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:1061:6: error: expected initializer before '__THROWNL'
     1061 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    /usr/include/pthread.h:1079:6: error: expected initializer before '__THROWNL'
     1079 |      __THROWNL __nonnull ((1));
          |      ^~~~~~~~~
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr.h:151,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/ext/atomicity.h:35,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:39,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/string:55,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/stdexcept:39,
                     from extensions/gdal_wrap.cpp:3092:
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:119:1: error: 'pthread_create' was not declared in this scope; did you mean 'pthread_key_create'?
      119 | __gthrw(pthread_create)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:119:1: error: 'pthread_create' was not declared in this scope; did you mean 'pthread_key_create'?
      119 | __gthrw(pthread_create)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:129:1: error: 'pthread_mutex_lock' was not declared in this scope; did you mean 'pthread_mutex_init'?
      129 | __gthrw(pthread_mutex_lock)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:129:1: error: 'pthread_mutex_lock' was not declared in this scope; did you mean 'pthread_mutex_init'?
      129 | __gthrw(pthread_mutex_lock)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:130:1: error: 'pthread_mutex_trylock' was not declared in this scope; did you mean 'pthread_mutex_t'?
      130 | __gthrw(pthread_mutex_trylock)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:130:1: error: 'pthread_mutex_trylock' was not declared in this scope; did you mean 'pthread_mutex_t'?
      130 | __gthrw(pthread_mutex_trylock)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:132:1: error: 'pthread_mutex_timedlock' was not declared in this scope; did you mean 'pthread_mutex_destroy'?
      132 | __gthrw(pthread_mutex_timedlock)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:132:1: error: 'pthread_mutex_timedlock' was not declared in this scope; did you mean 'pthread_mutex_destroy'?
      132 | __gthrw(pthread_mutex_timedlock)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:134:1: error: 'pthread_mutex_unlock' was not declared in this scope; did you mean 'pthread_mutex_init'?
      134 | __gthrw(pthread_mutex_unlock)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:134:1: error: 'pthread_mutex_unlock' was not declared in this scope; did you mean 'pthread_mutex_init'?
      134 | __gthrw(pthread_mutex_unlock)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:139:1: error: 'pthread_cond_broadcast' was not declared in this scope; did you mean 'pthread_cond_timedwait'?
      139 | __gthrw(pthread_cond_broadcast)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:139:1: error: 'pthread_cond_broadcast' was not declared in this scope; did you mean 'pthread_cond_timedwait'?
      139 | __gthrw(pthread_cond_broadcast)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:140:1: error: 'pthread_cond_signal' was not declared in this scope; did you mean 'pthread_cond_init'?
      140 | __gthrw(pthread_cond_signal)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:140:1: error: 'pthread_cond_signal' was not declared in this scope; did you mean 'pthread_cond_init'?
      140 | __gthrw(pthread_cond_signal)
          | ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h: In function 'int __gthread_create(__gthread_t*, void* (*)(void*), void*)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:676:68: error: '__gthrw_pthread_create' cannot be used as a function
      676 |   return __gthrw_(pthread_create) (__threadid, NULL, __func, __args);
          |                                                                    ^
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h: In function 'int __gthread_mutex_lock(__gthread_mutex_t*)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:762:49: error: '__gthrw_pthread_mutex_lock' cannot be used as a function
      762 |     return __gthrw_(pthread_mutex_lock) (__mutex);
          |                                                 ^
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h: In function 'int __gthread_mutex_trylock(__gthread_mutex_t*)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:771:52: error: '__gthrw_pthread_mutex_trylock' cannot be used as a function
      771 |     return __gthrw_(pthread_mutex_trylock) (__mutex);
          |                                                    ^
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h: In function 'int __gthread_mutex_timedlock(__gthread_mutex_t*, const __gthread_time_t*)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:782:69: error: '__gthrw_pthread_mutex_timedlock' cannot be used as a function
      782 |     return __gthrw_(pthread_mutex_timedlock) (__mutex, __abs_timeout);
          |                                                                     ^
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h: In function 'int __gthread_mutex_unlock(__gthread_mutex_t*)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:792:51: error: '__gthrw_pthread_mutex_unlock' cannot be used as a function
      792 |     return __gthrw_(pthread_mutex_unlock) (__mutex);
          |                                                   ^
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h: In function 'int __gthread_cond_broadcast(__gthread_cond_t*)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:866:50: error: '__gthrw_pthread_cond_broadcast' cannot be used as a function
      866 |   return __gthrw_(pthread_cond_broadcast) (__cond);
          |                                                  ^
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h: In function 'int __gthread_cond_signal(__gthread_cond_t*)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/x86_64-conda-linux-gnu/bits/gthr-default.h:872:47: error: '__gthrw_pthread_cond_signal' cannot be used as a function
      872 |   return __gthrw_(pthread_cond_signal) (__cond);
          |                                               ^
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/ext/string_conversions.h:43,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:6493,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/string:55,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/stdexcept:39,
                     from extensions/gdal_wrap.cpp:3092:
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/cstdio: At global scope:
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/cstdio:137:11: error: '::sprintf' has not been declared
      137 |   using ::sprintf;
          |           ^~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/cstdio:146:11: error: '::vsprintf' has not been declared
      146 |   using ::vsprintf;
          |           ^~~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/cstdio:175:11: error: '::snprintf' has not been declared
      175 |   using ::snprintf;
          |           ^~~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/cstdio:178:11: error: '::vsnprintf' has not been declared
      178 |   using ::vsnprintf;
          |           ^~~~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/cstdio:185:22: error: '__gnu_cxx::snprintf' has not been declared
      185 |   using ::__gnu_cxx::snprintf;
          |                      ^~~~~~~~
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/cstdio:188:22: error: '__gnu_cxx::vsnprintf' has not been declared
      188 |   using ::__gnu_cxx::vsnprintf;
          |                      ^~~~~~~~~
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/string:55,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/stdexcept:39,
                     from extensions/gdal_wrap.cpp:3092:
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h: In function 'std::string std::__cxx11::to_string(int)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:6547:50: error: 'vsnprintf' is not a member of 'std'; did you mean 'isprint'?
     6547 |   { return __gnu_cxx::__to_xstring<string>(&std::vsnprintf, 4 * sizeof(int),
          |                                                  ^~~~~~~~~
          |                                                  isprint
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h: In function 'std::string std::__cxx11::to_string(unsigned int)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:6552:50: error: 'vsnprintf' is not a member of 'std'; did you mean 'isprint'?
     6552 |   { return __gnu_cxx::__to_xstring<string>(&std::vsnprintf,
          |                                                  ^~~~~~~~~
          |                                                  isprint
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h: In function 'std::string std::__cxx11::to_string(long int)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:6558:50: error: 'vsnprintf' is not a member of 'std'; did you mean 'isprint'?
     6558 |   { return __gnu_cxx::__to_xstring<string>(&std::vsnprintf, 4 * sizeof(long),
          |                                                  ^~~~~~~~~
          |                                                  isprint
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h: In function 'std::string std::__cxx11::to_string(long unsigned int)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:6563:50: error: 'vsnprintf' is not a member of 'std'; did you mean 'isprint'?
     6563 |   { return __gnu_cxx::__to_xstring<string>(&std::vsnprintf,
          |                                                  ^~~~~~~~~
          |                                                  isprint
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h: In function 'std::string std::__cxx11::to_string(long long int)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:6569:50: error: 'vsnprintf' is not a member of 'std'; did you mean 'isprint'?
     6569 |   { return __gnu_cxx::__to_xstring<string>(&std::vsnprintf,
          |                                                  ^~~~~~~~~
          |                                                  isprint
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h: In function 'std::string std::__cxx11::to_string(long long unsigned int)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:6575:50: error: 'vsnprintf' is not a member of 'std'; did you mean 'isprint'?
     6575 |   { return __gnu_cxx::__to_xstring<string>(&std::vsnprintf,
          |                                                  ^~~~~~~~~
          |                                                  isprint
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h: In function 'std::string std::__cxx11::to_string(float)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:6584:50: error: 'vsnprintf' is not a member of 'std'; did you mean 'isprint'?
     6584 |     return __gnu_cxx::__to_xstring<string>(&std::vsnprintf, __n,
          |                                                  ^~~~~~~~~
          |                                                  isprint
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h: In function 'std::string std::__cxx11::to_string(double)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:6593:50: error: 'vsnprintf' is not a member of 'std'; did you mean 'isprint'?
     6593 |     return __gnu_cxx::__to_xstring<string>(&std::vsnprintf, __n,
          |                                                  ^~~~~~~~~
          |                                                  isprint
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h: In function 'std::string std::__cxx11::to_string(long double)':
    /home/ubuntu/anaconda3/envs/tensorflow_p37/x86_64-conda-linux-gnu/include/c++/9.3.0/bits/basic_string.h:6602:50: error: 'vsnprintf' is not a member of 'std'; did you mean 'isprint'?
     6602 |     return __gnu_cxx::__to_xstring<string>(&std::vsnprintf, __n,
          |                                                  ^~~~~~~~~
          |                                                  isprint
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/lib/gcc/x86_64-conda-linux-gnu/9.3.0/include/xmmintrin.h:34,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/lib/gcc/x86_64-conda-linux-gnu/9.3.0/include/immintrin.h:29,
                     from /home/ubuntu/anaconda3/envs/tensorflow_p37/lib/gcc/x86_64-conda-linux-gnu/9.3.0/include/x86intrin.h:32,
                     from /usr/include/gdal/cpl_port.h:762,
                     from extensions/gdal_wrap.cpp:3168:
    /home/ubuntu/anaconda3/envs/tensorflow_p37/lib/gcc/x86_64-conda-linux-gnu/9.3.0/include/mm_malloc.h: At global scope:
    /home/ubuntu/anaconda3/envs/tensorflow_p37/lib/gcc/x86_64-conda-linux-gnu/9.3.0/include/mm_malloc.h:34:16: error: declaration of 'int posix_memalign(void**, size_t, size_t)' has a different exception specifier
       34 | extern "C" int posix_memalign (void **, size_t, size_t);
          |                ^~~~~~~~~~~~~~
    In file included from /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/Python.h:34,
                     from extensions/gdal_wrap.cpp:173:
    /usr/include/stdlib.h:503:12: note: from previous declaration 'int posix_memalign(void**, size_t, size_t) throw ()'
      503 | extern int posix_memalign (void **__memptr, size_t __alignment, size_t __size)
          |            ^~~~~~~~~~~~~~
    extensions/gdal_wrap.cpp: In function 'PyObject* _wrap_StatBuf_size_get(PyObject*, PyObject*)':
    extensions/gdal_wrap.cpp:8408:5: error: 'sprintf' was not declared in this scope
     8408 |     sprintf(szTmp, CPL_FRMT_GIB, result);
          |     ^~~~~~~
    extensions/gdal_wrap.cpp:6116:1: note: 'sprintf' is defined in header '<cstdio>'; did you forget to '#include <cstdio>'?
     6115 | #include "gdal_utils.h"
      +++ |+#include <cstdio>
     6116 |
    extensions/gdal_wrap.cpp: In function 'PyObject* _wrap_StatBuf_mtime_get(PyObject*, PyObject*)':
    extensions/gdal_wrap.cpp:8443:5: error: 'sprintf' was not declared in this scope
     8443 |     sprintf(szTmp, CPL_FRMT_GIB, result);
          |     ^~~~~~~
    extensions/gdal_wrap.cpp:8443:5: note: 'sprintf' is defined in header '<cstdio>'; did you forget to '#include <cstdio>'?
    extensions/gdal_wrap.cpp: In function 'PyObject* _wrap_VSIFTellL(PyObject*, PyObject*)':
    extensions/gdal_wrap.cpp:8954:5: error: 'sprintf' was not declared in this scope
     8954 |     sprintf(szTmp, CPL_FRMT_GIB, result);
          |     ^~~~~~~
    extensions/gdal_wrap.cpp:8954:5: note: 'sprintf' is defined in header '<cstdio>'; did you forget to '#include <cstdio>'?
    extensions/gdal_wrap.cpp: In function 'PyObject* _wrap_Band_GetHistogram(PyObject*, PyObject*, PyObject*)':
    extensions/gdal_wrap.cpp:19958:9: error: 'sprintf' was not declared in this scope
    19958 |         sprintf(szTmp, CPL_FRMT_GUIB, integerarray[i]);
          |         ^~~~~~~
    extensions/gdal_wrap.cpp:19958:9: note: 'sprintf' is defined in header '<cstdio>'; did you forget to '#include <cstdio>'?
    extensions/gdal_wrap.cpp: In function 'PyObject* _wrap_GetCacheMax(PyObject*, PyObject*)':
    extensions/gdal_wrap.cpp:26992:5: error: 'sprintf' was not declared in this scope
    26992 |     sprintf(szTmp, CPL_FRMT_GIB, result);
          |     ^~~~~~~
    extensions/gdal_wrap.cpp:26992:5: note: 'sprintf' is defined in header '<cstdio>'; did you forget to '#include <cstdio>'?
    extensions/gdal_wrap.cpp: In function 'PyObject* _wrap_GetCacheUsed(PyObject*, PyObject*)':
    extensions/gdal_wrap.cpp:27031:5: error: 'sprintf' was not declared in this scope
    27031 |     sprintf(szTmp, CPL_FRMT_GIB, result);
          |     ^~~~~~~
    extensions/gdal_wrap.cpp:27031:5: note: 'sprintf' is defined in header '<cstdio>'; did you forget to '#include <cstdio>'?
    error: command '/home/ubuntu/anaconda3/envs/tensorflow_p37/bin/x86_64-conda-linux-gnu-cc' failed with exit status 1
    ----------------------------------------
ERROR: Command errored out with exit status 1: /home/ubuntu/anaconda3/envs/tensorflow_p37/bin/python -u -c 'import io, os, sys, setuptools, tokenize; sys.argv[0] = '"'"'/tmp/pip-install-w_2ga_xs/gdal_8225fcb369174c5585286229e56d9881/setup.py'"'"'; __file__='"'"'/tmp/pip-install-w_2ga_xs/gdal_8225fcb369174c5585286229e56d9881/setup.py'"'"';f = getattr(tokenize, '"'"'open'"'"', open)(__file__) if os.path.exists(__file__) else io.StringIO('"'"'from setuptools import setup; setup()'"'"');code = f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' build_ext -I/usr/include/gdal install --record /tmp/pip-record-e37qte8n/install-record.txt --single-version-externally-managed --compile --install-headers /home/ubuntu/anaconda3/envs/tensorflow_p37/include/python3.7m/GDAL Check the logs for full command output.
```


# CONDA VERSION
## Setup will be slower
- conda create --name gdaltest -y

- conda install tensorflow-gpu=1.15 keras=2.2.4 h5py-2.10.0 -y -y
- conda install gdal -y
- conda instal geopandas -y
- conda install rasterio -y 
- conda install tqdm matplotlib scikit-image opencv boto3 -y
- conda install jupyter ipywidgets -y
- pip install keras-tqdm
- conda install jupyter ipywidgets -y
#make sure uptodate jupyter etc - to work with keras tqdm

#keras error - h5py?
https://github.com/keras-team/keras/issues/14265

## Conda Pack
- Try packing the old working environment and copy from s3 and activate as a workaround
- Save on installs and C header issues

https://conda.github.io/conda-pack/

then can just have this on a volume you can attach via spot fleet and activate as necessary
- source exploracorn_env/bin/activate



