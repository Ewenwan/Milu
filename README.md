# Project Information 变异测试

　变异测试技术是一种对测试集的充分性进行评估的技术，以创建更有效的测试集。变异测试与路径或者数据流测试不同，没有测试数据的选取规则。变异测试应该与传统的测试技术结合，而不是取代它们。
 
 变异测试的基本思想：
 
　　给定一个程序P和一个测试数据集T，通过变异算子为P产生一组变异体Pn（P0、P1……Pn），对P和Pn都使用T进行测试运行，如果Pi在摸个测试输入t上与P产生不同的结果，则该Pi被杀死；若Pi在所有的测试数据及上都与P产生相同的结果，则称其为活的变异体。接下来对活的变异体进行分析，检查其是否等价于P；对于不等价与P的变异体Pi进行进一步的测试，直到充分性度量到满意的程度。

# 原理
    简单的说分为下面几部
    1.确定需要的mutation operator，
    2.构建AST，遍历每一个node，判断是否符合哪些mutation operator的pattern， 可以看成一种分类
    3.根据分类结果，采取每一种operator 对应的变异行为来变异，修改AST node
    4.通过新的AST 构建新的源文件

# MiLu：一个可定制、运行时优化、高阶的C语言变异测试工具； 

         1. MiLu：C语言变异测试工具 
                1、一阶 + 高阶变异测试 
                2、之前的工具是将所有可能的变异算子应用到测试中；而MiLu可以指定应用到测试中的变异算子； 
                3、为减少运行时开销，MiLu使用“测试治理(test harness)”技术将变异体和测试集嵌入到测试程序中。 
         2. background介绍了现有的一些变异测试工具。其中for C的有：Proteum（开源） + Plextest/Isure++（商业化）  
         3. Muli的基本功能：根据指定变异算子生成变异体 + 根据给定的测试集执行变异程序 + 报告变异scores（一个衡量测试集充分性的量


        变异测试：p + 变异算子 = p’；变异算子执行1次为一阶变异体(FOMs)；多次为高阶变异体(HOMs)；
        FOMs：p’杀死 + 存活 + 测试集T的充分等级
        HOMs：比一阶更难杀死；
        MiLu采用了77种变异算子 + 可定制变异算子

> MiLu体系结构

    1、包含三个部分（都提供了相应的接口供用户扩展相应组件的特点）： 
    ①、源码分析器(SCA)：是一个C语言解析器，以C代码为输入，将C代码解析成token列表；为了支持子序列变异算子的定制，不只是语法、上下文信息也会被保存。SCA也会根据这些语法构建成相应的抽象语法树(AST)。 
    ②、编译系统(MTS)：MTS以AST和变异算子为输入生成变异体模板； + 每一个变异体被表示成整形向量Mutation ID；变异模板用来生成Mutation ID。 
    ③、测试和评估系统(TES)：执行变异体程序；将MTS中的变异体传给GCC编译器，GCC编译器并不是将它编译成可执行的程序，而是编译成共享的类库 + 为了优化执行开销，使用“测试治理”，它能通过调用共享库的变异体动态调用测试用例。


    MiLu提供的两组变异测试模式——一阶变异测试模式： 
            1、：预定义的变异算子 + 自己定制的变异算子 
            2、MiLu做的工作：生成变异体 + 执行给定的测试机 + 报告变异分数；
    MiLu提供的两组变异测试模式——高阶变异测试模式 
            1、用户可以选择预定义的搜索优化算法，也可以指定他们自己的算法和合适的函数来查找分类的HOMs； 
            2、分类的HOMs也能应用到FOMs； 
            3、MiLu提供了良好的界面和API，研究人员可以通过这些API编写成圣和评估变异体的插件；

Flexibility

    MiLu允许用户使用一种脚本语言来定制变异算子来增强MiLu的灵活性。

    MiLu为用户提供了一个脚本语言——变异算子约束脚本语言(MOCS)来制定变异算子，MOCS为任何编译算子添加约束。约束类型包括两种：直接替代约束 + 环境条件约束；
    直接替代约束：允许用户选择一个指定的转换规则来执行替代，例如== 替换成 = ，而不用在被替换成><啊其它的了(简单但是灵活性高)
    环境条件约束：在特定的范围内指定变异算子操作而不用针对整个程序。例如只替代if语句中的操作算子：[if]ORRE，可以跟直接替代约束一起使用。




Efficiency

    使用其他变异测试工具的优化技术和引入“测试治理”技术来优化MiLu的执行、

    现有的优化技术有： 
    1、基于解释器技术：小规模的变异可行，大的就不适用； 
    2、基于编译器技术：效率比1高但是会引入额外开销； 
    3、基于突变模式技术：减少了2的额外开销，速度上比2高效。 
    (前三者都是对编译时到的优化)
    最新的优化技术十针对运行时而非编译时 
    1、MiLu中，一个测试治理包含了所有的测试用例和相应的设置，每一个变异体被编译成一个共享的类库，这些类库能被测试治理动态调用，只需执行测试治理，变异体程序被当做函数来调用。 
    2.这种方法比运行每一个变异体效率高很多。


## Introduction

Milu is an efficient and flexible C mutation testing tool designed for
both first order and highe order mutation testing. The name 'Milu' is
from a deer composed of four other animals. It has a horse's head, a
deer's antlers, a donkey's body and a cow's hooves.

If you've downloaded Milu, please let me know. If you are using Milu as
part of your research work (e.g. comparing your tool against it), then
please cite the following paper.

Yue Jia and Mark Harman. Milu: A Customizable, Runtime-Optimized Higher
Order Mutation Testing Tool for the Full C Language TAIC PART'08,
Windsor, UK, 29th-31st August 2008.

## Get Source Code 

git clone https://github.com/yuejia/Milu 

## Compilation in Ubuntu

- sudo apt-get install build-essential libglib2.0-dev llvm libclang-dev
- git clone https://github.com/yuejia/Milu
- cd Milu
- make

## Usage 
- ./bin/milu -f func.txt src.c 
NB: src.c need to be processed by gcc -E, func.txt contains a list of names of the functions under test

- ./bin/milu -? (to show options)

## Examples

Before to use the examples, you should compile the code. When you compile the code, a folder "bin" will be created. After that you can run the examples.

To run:

```sh
$ sh run.sh
```

## Acknowledgement

This project was supported by the EPSRC grant, EP/D050863(SEBASE) and
the ORSAS.
