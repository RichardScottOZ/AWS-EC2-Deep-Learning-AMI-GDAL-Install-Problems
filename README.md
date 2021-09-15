I have this working on an older AMI - but a work production one I can't just clone in the sandbox - all else fails, might have to work that out.  Or pack it.  Took a while to get going though and quite a few tweaks.

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
