# Building GLSL-Debugger on Windows with MinGW

Downloaded GLSL-Debugger GLSL-Debugger/GLSL-Debugger@8f7a66cf32deecb

Downloaded mhook SirAnthony/mhook@6474de01ee5058

Launched cmake-gui

 - Setup source path to "C:/users/Me/GLSL-Debugger"
 - Setup build path to "C:/users/Me/GLSL-Debugger-Build"

On clicking "configure", choose "MinGW makefiles" and "native default compilers"

Auto-detected:

 - GCC   v 4.7.2
 - BISON v 2.7
 - FLEX  v 2.5.37 
 - Perl  v 5.16.3

Manually setup


 - GLUT lib and include paths
 - QT_MAKE qmake.exe v 4.8.0
 - GLEW 			  v 1.9.0

Launched MinGW shell `C:\MinGW\msys\1.0\msys.bat`

## Error 1

	cd "C:/users/Me/GLSL-Debugger-Build"
	mingw32-make.exe

	[  1%] Building CXX object GLSLCompiler/compiler/CMakeFiles/InitializeDll.dir/InitializeDll.cpp.obj
	g++.exe: erreur: /W3: No such file or directory
	g++.exe: erreur: /wd4005: No such file or directory
	g++.exe: erreur: /wd4996: No such file or directory
	g++.exe: erreur: /nologo: No such file or directory
	GLSLCompiler\compiler\CMakeFiles\InitializeDll.dir\build.make:57: recipe for target 'GLSLCompiler/compiler/CMakeFiles/InitializeDll.dir/InitializeDll.cpp.obj' failed
	mingw32-make[2]: *** [GLSLCompiler/compiler/CMakeFiles/InitializeDll.dir/InitializeDll.cpp.obj] Error 1
	CMakeFiles\Makefile2:95: recipe for target 'GLSLCompiler/compiler/CMakeFiles/InitializeDll.dir/all' failed
	mingw32-make[1]: *** [GLSLCompiler/compiler/CMakeFiles/InitializeDll.dir/all] Error 2
	Makefile:115: recipe for target 'all' failed
	mingw32-make: *** [all] Error 2

#### Problem:
	
/W3 /wd4005 /wd4996 /nologo are msvc flags that g++ doesn't understands

#### Solution:
	
Remove MSVC compiler flags incompatible with GCC
commit: @de2716c880

## Error 2

	[  1%] Building CXX object GLSLCompiler/compiler/CMakeFiles/InitializeDll.dir/InitializeDll.cpp.obj
	Linking CXX static library ..\..\bin\libInitializeDll.a
	[  1%] Built target InitializeDll
	Scanning dependencies of target DebugJump
	[  1%] Building CXX object GLSLCompiler/glslang/DebugCodeGen/CMakeFiles/DebugJump.dir/DebugJump.cpp.obj
	In file included from c:\mingw\bin\../lib/gcc/mingw32/4.7.2/include/c++/unordered_map:35:0,
	                 from C:/Users/Thibault/GLSL-Debugger/GLSLCompiler/glslang/DebugCodeGen/../Include/Common.h:75,
	                 from C:\Users\Thibault\GLSL-Debugger\GLSLCompiler\glslang\DebugCodeGen\DebugJump.cpp:36:
	c:\mingw\bin\../lib/gcc/mingw32/4.7.2/include/c++/bits/c++0x_warning.h:32:2: erreur: #error This file requires compiler and library support for the ISO C++ 2011 standard. This support is currently experimental, and must be enabled with the -std=c++11 or -std=gnu++11 compiler options.
	[...] Lot of verbose errors [...]
	GLSLCompiler\glslang\DebugCodeGen\CMakeFiles\DebugJump.dir\build.make:57: recipe for target 'GLSLCompiler/glslang/DebugCodeGen/CMakeFiles/DebugJump.dir/DebugJump.cpp.obj' failed
	mingw32-make[2]: *** [GLSLCompiler/glslang/DebugCodeGen/CMakeFiles/DebugJump.dir/DebugJump.cpp.obj] Error 1
	CMakeFiles\Makefile2:171: recipe for target 'GLSLCompiler/glslang/DebugCodeGen/CMakeFiles/DebugJump.dir/all' failed
	mingw32-make[1]: *** [GLSLCompiler/glslang/DebugCodeGen/CMakeFiles/DebugJump.dir/all] Error 2
	Makefile:115: recipe for target 'all' failed
	mingw32-make: *** [all] Error 2

#### Problem:
	
C++ 2011 standard flag not set

#### Solution:

Add the -std=gnu++11 compiler flag (gnu because the cause uses non-standard _itoa, _strdup, ...)
commit: @f0ad7575aa7f

## Error 3

	[  1%] Building CXX object GLSLCompiler/compiler/CMakeFiles/InitializeDll.dir/InitializeDll.cpp.obj
	Linking CXX static library ..\..\bin\libInitializeDll.a
	[  1%] Built target InitializeDll
	[  1%] Building CXX object GLSLCompiler/glslang/DebugCodeGen/CMakeFiles/DebugJump.dir/DebugJump.cpp.obj
	[  2%] Building CXX object GLSLCompiler/glslang/DebugCodeGen/CMakeFiles/DebugJump.dir/IntermStack.cpp.obj
	Linking CXX static library ..\..\..\bin\libDebugJump.a
	[  2%] Built target DebugJump
	Scanning dependencies of target DebugVar
	[  2%] Building CXX object GLSLCompiler/glslang/DebugCodeGen/CMakeFiles/DebugVar.dir/DebugVar.cpp.obj
	Linking CXX static library ..\..\..\bin\libDebugVar.a
	[  2%] Built target DebugVar
	Scanning dependencies of target CodeGen
	[  2%] Building CXX object GLSLCompiler/glslang/GenericCodeGen/CMakeFiles/CodeGen.dir/CodeGen.cpp.obj
	[  3%] Building CXX object GLSLCompiler/glslang/GenericCodeGen/CMakeFiles/CodeGen.dir/Link.cpp.obj
	[  3%] Building CXX object GLSLCompiler/glslang/GenericCodeGen/CMakeFiles/CodeGen.dir/CodeInsertion.cpp.obj
	[  4%] Building CXX object GLSLCompiler/glslang/GenericCodeGen/CMakeFiles/CodeGen.dir/CodeTools.cpp.obj
	Linking CXX static library ..\..\..\bin\libCodeGen.a
	[  4%] Built target CodeGen
	Scanning dependencies of target utils
	[  5%] Building C object glsldb/utils/CMakeFiles/utils.dir/hash.c.obj
	cc1.exe: attention : command line option '-std=gnu++11' is valid for C++/ObjC++ but not for C [enabled by default]
	[  5%] Building C object glsldb/utils/CMakeFiles/utils.dir/dlutils.c.obj
	cc1.exe: attention : command line option '-std=gnu++11' is valid for C++/ObjC++ but not for C [enabled by default]
	C:\Users\Thibault\GLSL-Debugger\glsldb\utils\dlutils.c: In function 'openLibrary':
	C:\Users\Thibault\GLSL-Debugger\glsldb\utils\dlutils.c:50:48: erreur: 'LOAD_IGNORE_CODE_AUTHZ_LEVEL' undeclared (first use in this function)
	C:\Users\Thibault\GLSL-Debugger\glsldb\utils\dlutils.c:50:48: note: each undeclared identifier is reported only once for each function it appears in
	glsldb\utils\CMakeFiles\utils.dir\build.make:79: recipe for target 'glsldb/utils/CMakeFiles/utils.dir/dlutils.c.obj' failed
	mingw32-make[2]: *** [glsldb/utils/CMakeFiles/utils.dir/dlutils.c.obj] Error 1
	CMakeFiles\Makefile2:765: recipe for target 'glsldb/utils/CMakeFiles/utils.dir/all' failed
	mingw32-make[1]: *** [glsldb/utils/CMakeFiles/utils.dir/all] Error 2
	Makefile:115: recipe for target 'all' failed
	mingw32-make: *** [all] Error 2

#### Problem:

Some Windows dll loading functions

#### Solution:

I didn't really understood this one, from IRC, might be related to the the Windows SDK not being present.

Following kudoi advice, I added the requirement for DL in cmake config and DL_INCLUDE DL_LIBRARY showed up in cmake gui.

I downloaded this replacement library [dlfcn-win32](https://code.google.com/p/dlfcn-win32/) and setup the paths to it.

Not sure if 100% fixed because the error had previously appeared and disappeared randomly...

commit: @cf5ecf2d048

## Error 4

	[  1%] Built target InitializeDll
	[  2%] Built target DebugJump
	[  2%] Built target DebugVar
	[  4%] Built target CodeGen
	[  5%] Building CXX object GLSLCompiler/glslang/OSDependent/Windows/CMakeFiles/Ossource.dir/main.cpp.obj
	In file included from C:\Users\Thibault\GLSL-Debugger\GLSLCompiler\glslang\OSDependent\Windows\main.cpp:35:0:
	C:/Users/Thibault/GLSL-Debugger/GLSLCompiler/glslang/OSDependent/Windows/../../../compiler/InitializeDll.h:37:23: erreur fatale: osinclude.h : No such file or directory
	compilation terminée.
	GLSLCompiler\glslang\OSDependent\Windows\CMakeFiles\Ossource.dir\build.make:81: recipe for target 'GLSLCompiler/glslang/OSDependent/Windows/CMakeFiles/Ossource.dir/main.cpp.obj' failed
	mingw32-make[2]: *** [GLSLCompiler/glslang/OSDependent/Windows/CMakeFiles/Ossource.dir/main.cpp.obj] Error 1
	CMakeFiles\Makefile2:429: recipe for target 'GLSLCompiler/glslang/OSDependent/Windows/CMakeFiles/Ossource.dir/all' failed
	mingw32-make[1]: *** [GLSLCompiler/glslang/OSDependent/Windows/CMakeFiles/Ossource.dir/all] Error 2
	Makefile:115: recipe for target 'all' failed
	mingw32-make: *** [all] Error 2

#### Problem:

Some include paths from CMake config are wrong i guess...

#### Solution:

None yet