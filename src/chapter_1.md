# Chapter 1 Wait Forever 笔记

> 一些额外的参考资料


## Makefile 分析

cargo rustc 和 rustc 是用于编译 Rust 代码的两个不同的命令。

cargo rustc：cargo rustc 是一个 Cargo 命令，用于通过 Cargo 构建和编译 Rust 项目。它是一个包装了 rustc 的高级命令，提供了更方便的构建和管理项目的功能。cargo rustc 命令会根据项目的配置和 Cargo.toml 文件中的设置，自动解决依赖关系、编译代码、生成可执行文件等。它还支持各种构建选项和功能，如编译模式（debug 或 release）、目标平台、依赖项管理等。

rustc：rustc 是 Rust 编程语言的官方编译器。它是一个独立的命令行工具，用于将 Rust 源代码编译为目标平台上的可执行文件或库。与 cargo rustc 不同，rustc 直接调用编译器，并需要手动管理项目的依赖关系、编译选项和构建过程。通过 rustc，你可以更加精细地控制编译过程，并进行一些高级的编译器配置。

[rustc文档](https://doc.rust-lang.org/rustc/codegen-options/index.html)里可以找到RUSTFLAGS里的参数

# 参考资料

## 树莓派文档手册

- https://www.raspberrypi.com/products/raspberry-pi-4-model-b/specifications/

## 树莓派启动流程

- https://cloud.tencent.com/developer/article/1599592
- https://www.rpi4os.com/part1-bootstrapping/
- https://blog.csdn.net/qq_45172832/article/details/126040945
- https://thekandyancode.wordpress.com/2013/09/21/how-the-raspberry-pi-boots-up/

## FAT32 文件格式
- https://blog.csdn.net/csdn66_2016/article/details/88066637
- https://www.cnblogs.com/hwli/p/8633314.html
- https://github.com/HUST-OS/tornado-os/blob/main/doc/%E7%AC%AC%E5%85%AD%E7%AB%A0-%E5%BC%82%E6%AD%A5fat32%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F.md

## 树莓派OS教程

- https://github.com/isometimes/rpi4-osdev
- https://zhuanlan.zhihu.com/p/140078824
- https://github.com/bztsrc/raspi3-tutorial/tree/master/01_bareminimum
- http://rcore-os.cn/rCore-Tutorial-Book-v3/chapter1/4first-instruction-in-kernel2.html