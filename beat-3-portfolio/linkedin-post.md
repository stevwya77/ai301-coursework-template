I recently investigated and reproduced a memory leak in OpenImageIO issue #4613 where writing PNG files caused uncontrolled RAM growth. By building a subprocess-isolated benchmarking harness tracking physical memory footprints across multiple versions, I isolated the issue to a precise 5 bytes per pixel value per frame and traced its disappearance to release 3.0.6.0.

Although the maintainers had already addressed the underlying handle reference leak upstream in commit #4685, my reproduction empirically verified the fix, reconciled memory measurement discrepancies between RSS and macOS physical footprints, and even uncovered a secondary segmentation fault edge case during type probing. This investigation highlighted the importance of rigorous isolation testing and empirical validation in open-source systems.

https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6788821588

#OpenSource #Python #SoftwareEngineering #Debugging #OpenImageIO
