- 参考链接  
CMake入门实战：http://www.hahack.com/codes/cmake/  

- INCLUDE_DIRECTORIES(/usr/include/thrift)  
    INCLUDE_DIRECTORIES类似gcc中的编译参数“-I”，指定编译过程中编译器搜索头文件的路径。当项目需要的头文件不在系统默认的搜索路径时，需要指定该路径。在我们的项目中，log4cpp所需的头文件都存放在/usr/include下，不需要指定；但thrift的头文件没有存放在系统路径下，需要指定搜索其路径。
