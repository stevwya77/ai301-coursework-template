I reproduced and investigated OpenImageIO issue #4613. OpenImageIO is an open-source image library used in film and visual-effects pipelines; the issue involved uncontrolled RAM growth during PNG writes. Across Windows, Linux, and macOS, I tested four versions.

The leak was present through 3.0.5.0 and absent from 3.0.6.0 onward. My reproduction also uncovered another bug: one way of reading image data could silently return nothing or crash the program, which I reported to the project as a new issue.

My report let the maintainer confirm the fix and close an issue that had been open for over a year. When my first check didn’t reproduce the crash (0 out of 10), I worked out why before reporting it—a useful reminder to investigate unexpected results before drawing conclusions.

https://github.com/AcademySoftwareFoundation/OpenImageIO/issues/4613#issuecomment-6974270940

#OpenSource #Python #SoftwareEngineering #Debugging #OpenImageIO
