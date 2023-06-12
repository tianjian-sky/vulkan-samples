

# 安装步骤
## 安装third_party第三方库,

```
git submodule init
git submodule update
```
如果遇到下载不成功的库，去仓库地址下载源代码后拷贝到/third_party相应的子文件夹

[third_party](https://github.com/KhronosGroup/Vulkan-Samples/tree/main/third_party)

## 安装vulkan sdk

## 构建
[说明](https://github.com/KhronosGroup/Vulkan-Samples/blob/main/docs/build.md#macos)

### 创建makefile

```
cmake -H. -Bbuild/mac -DCMAKE_BUILD_TYPE=Release
```

### 构建

```
cmake --build build/mac --config Release --target vulkan_samples -- -j4

```

### 运行

#### 查看文档

``` 
./build/mac/app/bin/Release/${platform}/vulkan_samples --help

```

#### 查看所有可执行例子

``` 
./build/mac/app/bin/Release/${platform}/vulkan_samples samples

```

#### 运行某个例子

``` 
./build/mac/app/bin/Release/${platform}/vulkan_samples sample hpp_hello_triangle

```

## 关于 cmake 

[https://cmake.org/cmake/help/latest/manual/cmake.1.html#options] (cmake cli 参数文档)
[https://zhuanlan.zhihu.com/p/179246411?utm_source=wechat_session] (Cmake 学习)


CMake有两个主要的阶段。首先是"配置(configure)"，在此阶段CMake处理所有的输入然后创建软件构建过程的内部表达。第二个阶段是"生成(generate)"，负责创建出实际的构建文件。

环境变量与缓存

对1999年甚至是今天的许多构建系统来说，生成工程时都要用到底层(shell级别)的环境变量。典型的情况是，用PROJECT_ROOT环境变量来指向源码树的根目录。环境变量还被用于指定可选软件包和外部软件包。但是使用环境变量的方法也有弊端，它需要每次构建时都重新设置环境变量。为解决这个问题，CMake使用缓存文件来存储生成过程中用到的所有变量。这些变量不再是环境变量，而是CMake变量。CMake针对某个特定构建树第一次运行时，会创建一个CMakeCache.txt文件，存储当前构建过程中需要用到的CMake变量。这个缓存文件属于构建树的一部分，所以在之后的每次针对该构建树的重新配置时, 这些变量都是可重用的。


环境变量与缓存

对1999年甚至是今天的许多构建系统来说，生成工程时都要用到底层(shell级别)的环境变量。典型的情况是，用PROJECT_ROOT环境变量来指向源码树的根目录。环境变量还被用于指定可选软件包和外部软件包。但是使用环境变量的方法也有弊端，它需要每次构建时都重新设置环境变量。为解决这个问题，CMake使用缓存文件来存储生成过程中用到的所有变量。这些变量不再是环境变量，而是CMake变量。CMake针对某个特定构建树第一次运行时，会创建一个CMakeCache.txt文件，存储当前构建过程中需要用到的CMake变量。这个缓存文件属于构建树的一部分，所以在之后的每次针对该构建树的重新配置时, 这些变量都是可重用的。

配置阶段

在配置阶段，CMake首先尝试读取CMakeCache.txt文件，该文件在第一次运行时生成。然后，读取源码树根目录下的CMakeLists.txt文件，并使用CMake词法分析器处理。CMakeLists.txt中的每条命令都由一个命令模式对象执行。通过include和add_subdirectory命令，更多的CMakeLists.txt得到执行。对于每条命令，CMake都有一个C++对象来处理，比如add_library,if, add_executable, add_subdirectory,include等。实际上，整个CMake语言就是以命令调用的方式实现的。词法分析器只不过将输入文件内容转化为命令和命令参数而已。

配置阶段主要是运行用户定义的CMake代码。等到执行完之后，以及所有缓存变量计算完成之后，CMake在内存中得到一个项目构建的内部表达。这个内存中的内部表达包括了所有的库文件，可执行文件，定制的命令，以及生成指定generator(指特定的编译环境)所需的其他必要信息。这时，CMakeCache.txt会被存储到磁盘上，供以后重新运行CMake时使用。

项目在内存中的表达实际上是一些待生成的目标的集合，包括基本的库文件和可执行文件。CMake还支持目标的定制，即用户可以定义输入和输出，并提供定制的可在构建过程中运行的可执行文件或脚本。CMake将每个目标存储在一个cmTarget对象中，然后多个cmTarget存储在一个cmMakefile对象中，cmMakefile对象实际上用来存储源码树中某个目录中的所有目标。最后得到的结果是一棵cmMakefile对象的树，树结点中存储cmTarget对象的映射。

生成阶段

一旦配置(configure)阶段完成，生成(generator)阶段就可以开始了。生成阶段将生成用户指定类型(如Visual Studio或GNU/Linux GCC)的构建文件。这时，目标的内部表达(库，可执行文件，定制目标)转化为本地构建工具的输入文件，如Visual Studio或Makefile文件。CMake由配置阶段获得的内部表达要尽可能地抽象和通用，这样的数据结构才能被不同的本地构建工具所共享。


### 作用域

Each new directory or function creates a new scope

```
set(<variable> <value>... [PARENT_SCOPE])
```
If the PARENT_SCOPE option is given the variable will be set in the scope above the current scope. Each new directory or function() command creates a new scope. A scope can also be created with the block() command. This command will set the value of a variable into the parent directory, calling function or encompassing scope (whichever is applicable to the case at hand). The previous state of the variable's value stays the same in the current sco

The block(PROPAGATE) and return(PROPAGATE) commands can be used as an alternate method to the set(PARENT_SCOPE) and unset(PARENT_SCOPE) commands to update the parent scope.

### function
CMake中的function类似编程语言的函数，允许我们将一系列复杂的命令封装起来，方便调用与重复使用。
其基本的语法如下：
```
function(<name> [<arg1> ...])
  <commands>
endfunction([<name>])
```

```
function(my_function)
    message(STATUS "print my function")
endfunction()

my_function()


```

### cmake include

```
include(<file|module> [OPTIONAL] [RESULT_VARIABLE <var>]
                      [NO_POLICY_SCOPE])
```

Loads and runs CMake code from the file given.

if a module is specified instead of a file, the file with name <modulename>.cmake is searched first in CMAKE_MODULE_PATH, then in the CMake module directory. There is one exception to this: if the file which calls include() is located itself in the CMake builtin module directory, then first the CMake builtin module directory is searched and CMAKE_MODULE_PATH afterwards. See also policy CMP0017.

### cmake add_subdirectory
```
add_subdirectory(source_dir [binary_dir] [EXCLUDE_FROM_ALL] [SYSTEM])

```

Adds a subdirectory to the build. The source_dir specifies the directory in which the source CMakeLists.txt and code files are located. If it is a relative path, it will be evaluated with respect to the current directory (the typical usage), but it may also be an absolute path. The binary_dir specifies the directory in which to place the output files. If it is a relative path, it will be evaluated with respect to the current output directory, but it may also be an absolute path. If binary_dir is not specified, the value of source_dir, before expanding any relative path, will be used (the typical usage). The CMakeLists.txt file in the specified source directory will be processed immediately by CMake before processing in the current input file continues beyond this command.

### cmake set
[CMAKE——set()函数及常用变量名](https://blog.csdn.net/Zhanganliu/article/details/99851352?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-2)
```
set(<variable> <value>... [PARENT_SCOPE])
set(<variable> <value>... CACHE <type> <docstring> [FORCE]) // Set Cache Entry
set(ENV{<variable>} [<value>]) // 
```
### cmake list
[【CMake学习】list使用](https://blog.csdn.net/m0_47448477/article/details/124971222)
The list subcommands APPEND, INSERT, FILTER, PREPEND, POP_BACK, POP_FRONT, REMOVE_AT, REMOVE_ITEM, REMOVE_DUPLICATES, REVERSE and SORT may create new values for the list within the current CMake variable scope. Similar to the set() command, the LIST command creates new variable values in the current scope, even if the list itself is actually defined in a parent scope. To propagate the results of these operations upwards, use set() with PARENT_SCOPE, set() with CACHE INTERNAL, or some other means of value propagation.

### set_target_properties and get_target_property()

### source_group
[CMake 为visual studio 工程创建筛选器](https://blog.csdn.net/technologyleader/article/details/125260395)

```
source_group(<name> [FILES <src>...] [REGULAR_EXPRESSION <regex>])
source_group(TREE <root> [PREFIX <prefix>] [FILES <src>...])
```

Define a grouping for source files in IDE project generation. There are two different signatures to create source groups.

Defines a group into which sources will be placed in project files. This is intended to set up file tabs in Visual Studio. The group is scoped in the directory where the command is called, and applies to sources in targets created in that directory.


### cmake target_compile_definitions
[cmake：target_compile_definitions()](https://zhuanlan.zhihu.com/p/629613698)
[target_compile_definitions](https://cmake.org/cmake/help/latest/command/target_compile_definitions.html?highlight=target_compile_definitions)
```
target_compile_definitions(<target>
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
```
添加宏定义
Add compile definitions to a target.

Specifies compile definitions to use when compiling a given <target>. The named <target> must have been created by a command such as add_executable() or add_library() and must not be an ALIAS target.

Specifies compile definitions to use when compiling a given <target>. The named <target> must have been created by a command such as add_executable() or add_library() and must not be an ALIAS target.

The INTERFACE, PUBLIC and PRIVATE keywords are required to specify the scope of the following arguments. PRIVATE and PUBLIC items will populate the COMPILE_DEFINITIONS property of <target>. PUBLIC and INTERFACE items will populate the INTERFACE_COMPILE_DEFINITIONS property of <target>. The following arguments specify compile definitions. Repeated calls for the same <target> append items in the order called.

New in version 3.11: Allow setting INTERFACE items on IMPORTED targets.

Arguments to target_compile_definitions may use generator expressions with the syntax $<...>. See the cmake-generator-expressions(7) manual for available expressions. See the cmake-buildsystem(7) manual for more on defining buildsystem properties.

Any leading -D on an item will be removed. Empty items are ignored. For example, the following are all equivalent:


### 生成器表达式

[ cmake-generator-expressions](https://cmake.org/cmake/help/latest/manual/cmake-generator-expressions.7.html)

Generator expressions are evaluated during build system generation to produce information specific to each build configuration. They have the form $<...>. For example:

```
target_include_directories(tgt PRIVATE /opt/include/$<CXX_COMPILER_ID>)

```
This would expand to /opt/include/GNU, /opt/include/Clang, etc. depending on the C++ compiler used.

Generator expressions are allowed in the context of many target properties, such as LINK_LIBRARIES, INCLUDE_DIRECTORIES, COMPILE_DEFINITIONS and others. They may also be used when using commands to populate those properties, such as target_link_libraries(), target_include_directories(), target_compile_definitions() and others. They enable conditional linking, conditional definitions used when compiling, conditional include directories, and more. The conditions may be based on the build configuration, target properties, platform information, or any other queryable information.

* Conditional Expressions
* Logical Operators
* Primary Comparison Expressions
* ...

#### $<TARGET_PROPERTY:tgt,prop>
Value of the property prop on the target tgt.

```
include_directories(include)
add_library(add STATIC source/add.cpp)
add_library(subtraction SHARED source/subtraction.cpp)
add_library(multipy OBJECT source/multipy.cpp)
 
set_target_properties(add subtraction multipy PROPERTIES _ADD_ static _SUBTRACTION_ shared _MULTIPY_ object)
 
get_target_property(var add _ADD_)
message("var: ${var}") # var: static
get_target_property(var subtraction _SUBTRACTION_)
message("var: ${var}") # var: shared
get_target_property(var multipy _MULTIPY_)
message("var: ${var}") # var: object
 
get_target_property(var add XXXX)

————————————————
版权声明：本文为CSDN博主「fengbingchun」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/fengbingchun/article/details/128258041
```
### add_executable()
Adds an executable target called <name> to be built from the source files listed in the command invocation. The <name> corresponds to the logical target name and must be globally unique within a project. The actual file name of the executable built is constructed based on conventions of the native platform (such as <name>.exe or just <name>).


### cmake add_library
#### Normal Libraries
```
add_library(<name> [STATIC | SHARED | MODULE]
            [EXCLUDE_FROM_ALL]
            [<source>...])
```

Adds a library target called <name> to be built from the source files listed in the command invocation. The <name> corresponds to the logical target name and must be globally unique within a project. The actual file name of the library built is constructed based on conventions of the native platform (such as lib<name>.a or <name>.lib).


STATIC, SHARED, or MODULE may be given to specify the type of library to be created. STATIC libraries are archives of object files for use when linking other targets. SHARED libraries are linked dynamically and loaded at runtime. MODULE libraries are plugins that are not linked into other targets but may be loaded dynamically at runtime using dlopen-like functionality. If no type is given explicitly the type is STATIC or SHARED based on whether the current value of the variable BUILD_SHARED_LIBS is ON. For SHARED and MODULE libraries the POSITION_INDEPENDENT_CODE target property is set to ON automatically. A SHARED library may be marked with the FRAMEWORK target property to create an macOS Framework.

#### Object Libraries
```
add_library(<name> OBJECT [<source>...])
```
Creates an Object Library. An object library compiles source files but does not archive or link their object files into a library. Instead other targets created by add_library or add_executable() may reference the objects using an expression of the form $<TARGET_OBJECTS:objlib> as a source, where objlib is the object library name. 

这种形式类型固定为OBJECT，以这种方式，只编译source列表的文件，但不将生成的目标文件打包或者链接为库，而是在其他add_library()或者add_executable()生成目标的时候，可以使用形如$<TARGET_OBJECTS:objlib>的表达式将对象库作为源引入。
————————————————
版权声明：本文为CSDN博主「在下马农」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/mataojie/article/details/121466125

```
add_library(... $<TARGET_OBJECTS:objlib> ...)
add_executable(... $<TARGET_OBJECTS:objlib> ...)
```

#### Interface Libraries

```
add_library(<name> INTERFACE)
```
Creates an Interface Library. An INTERFACE library target does not compile sources and does not produce a library artifact on disk. However, it may have properties set on it and it may be installed and exported. Typically, INTERFACE_* properties are populated on an interface target using the commands:

* set_property(),
* target_link_libraries(INTERFACE),
* target_link_options(INTERFACE),
* target_include_directories(INTERFACE),
* target_compile_options(INTERFACE),
* target_compile_definitions(INTERFACE), and
* target_sources(INTERFACE),

and then it is used as an argument to target_link_libraries() like any other target.

生成一个接口库，这类库不编译任何文件，也不在磁盘上产生库文件。它有一些属性被设置，并且能够被安装和导出。通常，使用以下命令在接口目标上填充属性。

#### Imported Libraries

```
add_library(<name> <type> IMPORTED [GLOBAL])

```
Creates an IMPORTED library target called <name>. No rules are generated to build it, and the IMPORTED target property is True. The target name has scope in the directory in which it is created and below, but the GLOBAL option extends visibility. It may be referenced like any target built within the project. IMPORTED libraries are useful for convenient reference from commands like target_link_libraries(). Details about the imported library are specified by setting properties whose names begin in IMPORTED_ and INTERFACE_.

The <type> must be one of:

STATIC, SHARED, MODULE, UNKNOWN

References a library file located outside the project. The IMPORTED_LOCATION target property (or its per-configuration variant IMPORTED_LOCATION_<CONFIG>) specifies the location of the main library file on disk:

An UNKNOWN library type is typically only used in the implementation of Find Modules. It allows the path to an imported library (often found using the find_library() command) to be used without having to know what type of library it is. This is especially useful on Windows where a static library and a DLL's import library both have the same file extension.

OBJECT

References a set of object files located outside the project. The IMPORTED_OBJECTS target property (or its per-configuration variant IMPORTED_OBJECTS_<CONFIG>) specifies the locations of object files on disk. Additional usage requirements may be specified in INTERFACE_* properties.

INTERFACE

Does not reference any library or object files on disk, but may specify usage requirements in INTERFACE_* properties.

用来导入已经存在的库，CMake也不会添加任何编译规则给它。
此类库的标志就是有IMPORT属性，导入的库的作用域为创建它的目录及更下级目录。但是如果有GLOBE属性，则作用域被拓展到全工程。

#### Alias Libraries

```
add_library(<name> ALIAS <target>)

```
Creates an Alias Target, such that <name> can be used to refer to <target> in subsequent commands. The <name> does not appear in the generated buildsystem as a make target. The <target> may not be an ALIAS.

为给定library添加一个别名，后续可使用<name>来替代<target>

### target_sources

```
target_sources(<target>
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])
  ```
  Specifies sources to use when building a target and/or its dependents. The named <target> must have been created by a command such as add_executable() or add_library() or add_custom_target() and must not be an ALIAS target. The <items> may use generator expressions.

[使用 target_sources() 提高源文件处理能力](https://blog.csdn.net/guaaaaaaa/article/details/125601766)

在CMake项目中，通常存在从大量源文件 (source files) 构建 (build) 的目标 (targets)。这些文件可以分布在不同的子目录中，这些子目录本身可以嵌套在多个层次上。在此类项目中，传统方法通常要么在最顶层目录列出所有源文件，要么将源文件列表储存于一个变量，并将其传递给add_library(), add_executable() 等。在CMake 3.13中，引入了一个新的命令 target_sources()，该命令提供了各种 target_xxx() 命令中缺失的部分功能。虽然CMake文档简洁地描述了 target_sources() 的功能，但没有强调新命令的有用性以及它为何能更好地支持 CMake 项目：

CMakeLists.txt 文件的 add_executable() 或 add_library() 语句中。这种使用变量存储源文件列表的方法不是特别可靠。例如，如果在整个目录层次结构中构建了许多目标，变量的数量和命名可能会失控。这可以通过坚持某种命名约定来解决，但这取决于所有开发人员都知道并遵守该约定。此外，如果开发人员不小心在更深层目录中重复使用变量名，则源可能最终会被添加到意外的目标中。CMake 通常对此不会发出任何类型的诊断消息，因为它不知道您不打算这样做。

但是，使用变量的更大缺点可能是，它阻止了在深入到子目录时的 CMake 目标定义。这反过来意味着在子目录中也不能直接调用 target_compile_definitions(), target_compile_options(), target_include_directories() 或 target_link_libraries()。为了将编译器标识 (flags)、选项 (options)、头搜索路径 (header search paths) 和其他要链接的库 (libraries) 关联起来，必须定义更多的变量来将这些信息传递回顶层。在正确处理引用 (quoting) 时也必须格外小心。如果您想充分利用 PUBLIC, PRIVATE, INTERFACE 这些目标命令，仅仅一个目标所需的变量数量已经开始变得有点愚蠢。如果在项目的目录结构中定义了许多目标，那么可以想象变量数量将爆炸。


### target_include_directories
指定目标包含的头文件路径
```
target_include_directories(<target> [SYSTEM] [AFTER|BEFORE]
  <INTERFACE|PUBLIC|PRIVATE> [items1...]
  [<INTERFACE|PUBLIC|PRIVATE> [items2...] ...])

```
Specifies include directories to use when compiling a given target. The named <target> must have been created by a command such as add_executable() or add_library() and must not be an ALIAS target.

The INTERFACE, PUBLIC and PRIVATE keywords are required to specify the scope of the following arguments. PRIVATE and PUBLIC items will populate the INCLUDE_DIRECTORIES property of <target>. PUBLIC and INTERFACE items will populate the INTERFACE_INCLUDE_DIRECTORIES property of <target>. The following arguments specify include directories.

[cmake：target_** 中的 PUBLIC，PRIVATE，INTERFACE](https://zhuanlan.zhihu.com/p/82244559)

#### target_** 中的 PUBLIC，PRIVATE，INTERFACE
.-----------.------------------.----------------.
|           | Linked by target | Link interface |
:-----------+------------------+----------------:
| PUBLIC    |        X         |        X       |
:-----------+------------------+----------------:
| PRIVATE   |        X         |                |
:-----------+------------------+----------------:
| INTERFACE |                  |        X       |
'-----------'------------------'----------------'
* Linked by target: libraries included in target sources (not a dependency for projects linking the library).
* Link interface: libraries included in target public headers (dependencies for projects linking the library).

[cmake：target_** 中的 PUBLIC，PRIVATE，INTERFACE](https://zhuanlan.zhihu.com/p/82244559)


### target_link_libraries

```
target_link_libraries(<target> ... <item>... ...)

```

specify libraries or flags to use when linking a given target and/or its dependents. Usage requirements from linked library targets will be propagated. Usage requirements of a target's dependencies affect compilation of its own sources.

The named <target> must have been created by a command such as add_executable() or add_library() and must not be an ALIAS target. If policy CMP0079 is not set to NEW then the target must have been created in the current directory. Repeated calls for the same <target> append items in the order called.

指定目标链接的库。

### configure_file
```
configure_file(<input> <output>
               [NO_SOURCE_PERMISSIONS | USE_SOURCE_PERMISSIONS |
                FILE_PERMISSIONS <permissions>...]
               [COPYONLY] [ESCAPE_QUOTES] [@ONLY]
               [NEWLINE_STYLE [UNIX|DOS|WIN32|LF|CRLF] ])
```
Copies an <input> file to an <output> file and substitutes variable values referenced as @VAR@, ${VAR}, $CACHE{VAR} or $ENV{VAR} in the input file content. Each variable reference will be replaced with the current value of the variable, or the empty string if the variable is not defined. Furthermore, input lines of the form

[Cmake命令之configure_file介绍](https://www.jianshu.com/p/2946eeec2489)

configure_file命令一般用于自定义编译选项或者自定义宏的场景。configure_file命令会根据options指定的规则，自动对input文件中cmakedefine关键字及其内容进行转换。
  具体来说，会做如下几个替换：
  1. 将input文件中的@var@或者${var}替换成cmake中指定的值。
  2. 将input文件中的#cmakedefine var关键字替换成#define var或者#undef var，取决于cmake是否定义了var。

作者：Domibaba
链接：https://www.jianshu.com/p/2946eeec2489
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

### INTERFACE_INCLUDE_DIRECTORIES
Targets may populate this property to publish the include directories required to compile against the headers for the target. The target_include_directories() command populates this property with values given to the PUBLIC and INTERFACE keywords. Projects may also get and set the property directly.

```
target_include_directories(plugins PRIVATE ${CMAKE_CURRENT_SOURCE_DIR} $<TARGET_PROPERTY:apps,INTERFACE_INCLUDE_DIRECTORIES> $<TARGET_PROPERTY:framework,INTERFACE_INCLUDE_DIRECTORIES>)

```

### INTERFACE_COMPILE_FEATURES

Targets may populate this property to publish the compile features required to compile against the headers for the target. The target_compile_features() command populates this property with values given to the PUBLIC and INTERFACE keywords. Projects may also get and set the property directly.

### target_compile_features()
```
target_compile_features(<target> <PRIVATE|PUBLIC|INTERFACE> <feature> [...])

```
Specifies compiler features required when compiling a given target. If the feature is not listed in the CMAKE_C_COMPILE_FEATURES, CMAKE_CUDA_COMPILE_FEATURES, or CMAKE_CXX_COMPILE_FEATURES variables, then an error will be reported by CMake. If the use of the feature requires an additional compiler flag, such as -std=gnu++11, the flag will be added automatically.

### cmake_parse_arguments
```
cmake_parse_arguments(<prefix> <options> <one_value_keywords>
                      <multi_value_keywords> <args>...)

cmake_parse_arguments(PARSE_ARGV <N> <prefix> <options>
                      <one_value_keywords> <multi_value_keywords>)
```
This command is for use in macros or functions. It processes the arguments given to that macro or function, and defines a set of variables which hold the values of the respective options.

```
function(MY_INSTALL)
  set(options OPTIONAL FAST)
  set(oneValueArgs DESTINATION RENAME)
  set(multiValueArgs TARGETS CONFIGURATIONS)
  cmake_parse_arguments(MY_INSTALL "${options}" "${oneValueArgs}"
                        "${multiValueArgs}" ${ARGN} )
  ...

```

```
MY_INSTALL_OPTIONAL = TRUE
MY_INSTALL_FAST = FALSE (this option was not used when calling my_install()
MY_INSTALL_DESTINATION = "bin"
MY_INSTALL_RENAME = "" (was not used)
MY_INSTALL_TARGETS = "foo;bar"
MY_INSTALL_CONFIGURATIONS = "" (was not used)
MY_INSTALL_UNPARSED_ARGUMENTS = "blub" (no value expected after "OPTIONAL"
```


## cmake cli

[build a project](https://cmake.org/cmake/help/latest/manual/cmake.1.html)

### cmake --build <dir>

--build <dir>
Project binary directory to be built. This is required (unless a preset is specified) and must be first.

>
cmake --build .

该命令的含义是：执行当前目录下的构建系统，生成构建目标。

cmake项目构建过程简述:

1. 首先，使用命令行:‘cmake <source tree>’,比如：cmake .. ,在你的构建目录(外部构建方式)下生成了项目文件project files, 官方文档中又叫build tree/binary tree，这其中就包括，比如:Makefile，还有一些其他相关文件/目录/子目录;

2. 其次，自然是对生成好的项目(project files)进行编译构建，使用到的就是你说的'cmake --build .'

3. 最后，--build后面的那个‘.’,指的是生成好的build tree的路径. 一般来说，如果你明确知道，你的系统中使用的是哪种构建器(build generator), 比如：Unix Makefiles, 你完全可以直接使用make进行项目构建.

对于这种--build的形式，多用于自动化脚本之中，或者IDE环境下.

注： <source tree>指的是源文件+顶层CMakeLists所在的路径，cmake ..假设了路径在上一层.

通过cmake ./cmake .. 命令创建Makefile文件后，一般使用make命令编译文件。这里的cmake --build .就与make一样的效果

为什么不直接 make，而是使用 cmake --build 形式的命令，主要是为了跨平台，使用这种形式后，不管你是使用的什么生成器，CMake 都能正确构建，否则如果使用的是 Ninja 或者其他生成器，那 make 就不生效了
————————————————
版权声明：本文为CSDN博主「ASS-ASH」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_38563206/article/details/126486183

>


### cmake --config
--config <cfg>¶
For multi-configuration tools, choose configuration <cfg>.

### cmake --target
-t <tgt>..., --target <tgt>...
Build <tgt> instead of the default target. Multiple targets may be given, separated by spaces.

[–target选项在CMake中意味着什么？](https://www.codenong.com/25896657/)

``` cmake
add_executable(hello hello.cpp)

add_executable(goodbye goodbye.cpp)
```
然后CMake为每个可执行文件创建"构建目标"。 它还会创建其他构建目标，例如" all"构建目标，它将构建所有内容。

默认情况下，如果未指定目标，则将执行"全部"目标，这意味着将同时建立问候和告别。

如果仅要构建目标之一，则可以指定要构建的目标。


### cmake -H 打印帮助
    Print usage information and exit.
    Usage describes the basic command line interface and its options.
### cmake -B 创建构建目录
    Path to directory which CMake will use as the root of build directory.

### cmake -D 设置变量
    当cmake首次运行在一个空的构建树中时，它会创建一个CMakeCache.txt文件并使用该项目的可自定义设置来填充它。 该选项可用于指定优先于项目默认值的设置。 该选项可以根据需要重复多次缓存条目
    -D <var>:<type>=<value>, -D <var>=<value>¶

如：
```
cmake -H. -Bbuild/mac -DCMAKE_BUILD_TYPE=Release -D VKB_VULKAN_DEBUG=ON
```

打印：
```
message("   *** var VKB_VULKAN_DEBUG:   ${VKB_VULKAN_DEBUG}") # ON
```

### cmake -j
-j [<jobs>]

The maximum number of concurrent processes to use when building. If <jobs> is omitted the native build tool's default number is used.


