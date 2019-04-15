# vs19_pch_problem

This repo illustrates a problem with compiling single files within a project using precompiled headers.

## Building from inside VS

When building from inside Visual Studio the compilation fails due to mismatch of precompiled header and redefinitions of macros like `WIN32`.

1. Build project
2. Compile CMakeProject2.cpp



## Building from command line

When building from the command line (cmd.exe) everything works as expected.

```Batchfile
cd ..\build\x64-debug
ninja
del CMakeProject2\CMakeFiles\CMakeProject2.dir\CMakeProject2.cpp.obj
ninja C:\Users\pg\source\repos\CMakeProject2\CMakeProject2\CMakeProject2.cpp^^
```
