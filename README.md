# Project Information 变异测试

# 原理
    简单的说分为下面几部
    1.确定需要的mutation operator，
    2.构建AST，遍历每一个node，判断是否符合哪些mutation operator的pattern， 可以看成一种分类
    3.根据分类结果，采取每一种operator 对应的变异行为来变异，修改AST node
    4.通过新的AST 构建新的源文件

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
