Make/options

用来指定OpenFOAM需要调用的外挂库的**路径**以及**名称**；

  ```c++
EXE_INC = -I路径           // (指定的是编译成库之后自动生成的lnInclude文件夹)
EXE_LIBS = _L$(FOAM_USER_LIBBIN) \  //（这个是编译库的时候将库存放的位置，通常库文件放在                                     //  了$FOAM_USER_LIBBIN路径下，可执行文件放在                                         //   $FOAM_USER_APPBIN路径下）
          -l库的名称                 // （这个是编译库的时候将库存放在指定位置时取的字）
  ```

Make/files

 用来指定OpenFOAM顺序进行编译的文件**名称**以及**路径**；

```c++
C文件名称                        //与Make同目录下的C文件
EXE = $(FOAM_USER_APPBIN)/名称   //如果要编译成可执行文件就是EXE,如果要编译成库就是LIB,                                 //路径分别是$FOAM_USER_APPBIN和$FOAM_USER_LIBBIN，后                                 //面的名称就是终端执行时候需要输入的或者其他文件需要引用                                 //这个库的时候在他的Make/options文件里面指定的库名称
```

​        

