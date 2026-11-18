I reproduced and investigated OpenImageIO issue #4613, where PNG writes caused uncontrolled RAM growth. Across Windows, Linux, and macOS, I tested releases 3.0.3.1, 3.0.5.0, 3.0.6.0, and 3.2.2.0 using a subprocess-isolated benchmarking harness that separated process RSS from macOS physical footprint.

The leak was present through 3.0.5.0 and absent from 3.0.6.0 onward. My reproduction also found a separate reproducible bug: an in-memory call could return None while a file-backed lazy read segfaulted in 10 of 10 fresh-process runs. I documented that as OpenImageIO #5698, and maintainer lgritz confirmed #4613 was fixed by #4685.

https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6974270940

#OpenSource #Python #SoftwareEngineering #Debugging #OpenImageIO