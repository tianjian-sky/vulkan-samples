

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

### cmake -H 打印帮助
    Print usage information and exit.
    Usage describes the basic command line interface and its options.
### cmake -B 创建构建目录
    Path to directory which CMake will use as the root of build directory.

### cmake -D 创建构建目录
    当cmake首次运行在一个空的构建树中时，它会创建一个CMakeCache.txt文件并使用该项目的可自定义设置来填充它。 该选项可用于指定优先于项目默认值的设置。 该选项可以根据需要重复多次缓存条目
    -D <var>:<type>=<value>, -D <var>=<value>¶


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

### cmake -j
-j [<jobs>]

The maximum number of concurrent processes to use when building. If <jobs> is omitted the native build tool's default number is used.

