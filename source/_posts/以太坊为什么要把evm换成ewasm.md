---
title: 以太坊为什么要把evm换成WebAssembly
categories:
  - 区块链
  - 以太坊
tags:
  - evm
  - ewasam
comments: true
abbrlink: 634bfd05
date: 2020-03-17 16:35:32
img:
---
## Evm简介
EVM 由基于堆栈的架构组成。为了部署智能合约，必须首先将所有高级以太坊智能合约代码编译为机器可读代码（称为字节码）。然后，EVM 通过后进先出堆栈安排来处理此字节码代码（一系列单字节操作码和可选参数）。此操作类似于 Java 虚拟机（JVM），其中每条指令都以单字节操作码和参数开始，如果有参数的话，则占用后面的未对齐的字节，值按大端排序给出。
<!--more-->
## WebAssemble(简称Wasm)
WebAssembly(Wasm)是一种可以在现代 Web 浏览器中运行新型代码。它带来了新功能并在性能上有重大进展。Wasm 旨在成为 C，C++和 Rust 等低级源语言的有效编译目标。
## 以太坊WebAssmble(简称Ewasm)
以太坊 Web Assembly （Ewasm）是基于现代标准 WebAssembly 虚拟机构建的确定性智能合约执行引擎。Ewasm 是替代EVM 的主要候选方案，是Ethereum 2.0 “Serenity”路线图的一部分。甚至，还有人建议在以太坊主网上采用 Ewasm。
## 为什么选择Ewasm
以太坊需要性能更好的虚拟机, wasm有着接近原声的运行速度,而且wasm有着谷歌微软等公司推进, 并且拥有成熟的社区。  
EVM 的当前架构是释放其原始（raw）性能的最大障碍之一（GitHub EIP48，2019）。例如，虽然 256 位字长可促进本机哈希和椭圆曲线运算（Antonopoulos and Wood，2018），但也使从 EVM 操作码转换成硬件指令的过程难度过大，远超所需。提供更接近硬件映射的架构将大大提高以太坊的性能（GitHub EIP48，2019）。  
除了性能增强方面，Ewasm 项目的设计目标之一是还支持跨多种语言和工具进行智能合约开发，即把 LLVM，C，C++，Rust，JavaScript 纳入开发周期。  
从理论上讲，可以编译为 Wasm 的任何语言都可以用来编写智能合约。只要实现 Ewasm 合约接口（ECI）和以太坊环境接口（EEI）（ewasm，2019）。