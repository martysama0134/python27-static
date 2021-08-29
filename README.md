# python27-static
fork ready-to-compile static v2.7.17 .lib project for windows vs2019

To make it static, I've enabled Py_NO_ENABLE_SHARED MS_NO_COREDLL in every project. It's currently in MT MTd.

To make sure the library is linked properly in the program, you have to edit pyconfig.h.

At the beginning of pyconfig.h add:

```cpp
#ifndef Py_NO_ENABLE_SHARED
#define Py_NO_ENABLE_SHARED
#endif

#ifndef MS_NO_COREDLL
#define MS_NO_COREDLL
#endif
```

For the linking you need:
```cpp

/* For an MSVC DLL, we can nominate the .lib files used by extensions */
#ifdef MS_COREDLL
#	ifndef Py_BUILD_CORE /* not building the core - must be an ext */
#		if defined(_MSC_VER)
			/* So MSVC users need not specify the .lib file in
			their Makefile (other compilers are generally
			taken care of by distutils.) */
#			ifdef _DEBUG
#				pragma comment(lib,"python27_d.lib")
#			else
#				pragma comment(lib,"python27.lib")
#			endif /* _DEBUG */
#		endif /* _MSC_VER */
#	endif /* Py_BUILD_CORE */
#else
# ifdef _DEBUG
#  pragma comment(lib,"python27_d-static.lib")
# else
#  pragma comment(lib,"python27-static.lib")
# endif /* _DEBUG */
#endif /* MS_COREDLL */
```
