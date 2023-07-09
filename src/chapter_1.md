# Chapter 1 Wait Forever 笔记

> 一些额外的参考资料


## Makefile 分析

cargo rustc 和 rustc 是用于编译 Rust 代码的两个不同的命令。

cargo rustc：cargo rustc 是一个 Cargo 命令，用于通过 Cargo 构建和编译 Rust 项目。它是一个包装了 rustc 的高级命令，提供了更方便的构建和管理项目的功能。cargo rustc 命令会根据项目的配置和 Cargo.toml 文件中的设置，自动解决依赖关系、编译代码、生成可执行文件等。它还支持各种构建选项和功能，如编译模式（debug 或 release）、目标平台、依赖项管理等。

rustc：rustc 是 Rust 编程语言的官方编译器。它是一个独立的命令行工具，用于将 Rust 源代码编译为目标平台上的可执行文件或库。与 cargo rustc 不同，rustc 直接调用编译器，并需要手动管理项目的依赖关系、编译选项和构建过程。通过 rustc，你可以更加精细地控制编译过程，并进行一些高级的编译器配置。

[rustc文档](https://doc.rust-lang.org/rustc/codegen-options/index.html)里可以找到RUSTFLAGS里的参数