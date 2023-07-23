# Chapter 1 Wait Forever 笔记

> 一些额外的参考资料


## Makefile 分析

cargo rustc 和 rustc 是用于编译 Rust 代码的两个不同的命令。

cargo rustc：cargo rustc 是一个 Cargo 命令，用于通过 Cargo 构建和编译 Rust 项目。它是一个包装了 rustc 的高级命令，提供了更方便的构建和管理项目的功能。cargo rustc 命令会根据项目的配置和 Cargo.toml 文件中的设置，自动解决依赖关系、编译代码、生成可执行文件等。它还支持各种构建选项和功能，如编译模式（debug 或 release）、目标平台、依赖项管理等。

rustc：rustc 是 Rust 编程语言的官方编译器。它是一个独立的命令行工具，用于将 Rust 源代码编译为目标平台上的可执行文件或库。与 cargo rustc 不同，rustc 直接调用编译器，并需要手动管理项目的依赖关系、编译选项和构建过程。通过 rustc，你可以更加精细地控制编译过程，并进行一些高级的编译器配置。

[rustc文档](https://doc.rust-lang.org/rustc/codegen-options/index.html)里可以找到RUSTFLAGS里的参数


计算机系统存在如下两个概念。
- 体系结构（Architecture）：指对程序员可见的系统属性，如指令集、数据类型、输入/输出机制、内存寻址机制。
- 组织结构（Organization）：指实现结构规范的操作单元及其相互连接，对程序员透明（也就是从程序员的角度看不到这些），如控制信号、模块接口、存储器使用技术等。

嵌入式系统中常见的 ARM、PowerPC 等体系结构都采用这种统一的地址空间映射。也就是说，I/O设备的相应配置寄存器、内部缓冲区或 FIFO 都**统一映射到内存空间中的一段地址空间**。

当处理器产生读写操作时，目标地址经由系统的地址译码器（通常在互联结构上）进行译码，互联结构根据译码结果将读写命令转发到对应的设备上，设备响应命令或返回数据，完成传输。

**地址空间**不仅仅是**内存空间**！

对于 x86 体系架构处理器来说，存在着两种**地址空间**，分别是**内存空间**与 **I/O 空间**。

在现代设计中，内存空间包括存储器及 I/O 设备，处理器可以通过统一的访存指令（对于 ARM 架构而言就是 Load/Store 指令）经由 DRAM 控制器直接访问 DRAM 存储器对应的地址空间，也可以通过读写以太网控制器的硬件寄存器来收发数据包。

树莓派3上用户目前无法正常是使用GPIO中的UART串口(GPIO14&GPIO15),，原因是树莓派CPU内部有两个串口，一个是硬件串口(官方称为PL011 UART)，一个是迷你串口(官方成为mini-uart)。在树莓派2B/B+这些老版树莓派上，官方设计时都是将“硬件串口”分配给GPIO中的UART(GPIO14&GPIO15)，因此可以独立调整串口的速率和模式。而树莓派3的设计上，官方在设计时将硬件串口分配给了新增的蓝牙模块上，而将一个没有时钟源，必须由内核提供时钟参考源的“迷你串口”分配给了GPIO的串口，这样以来由于内核的频率本身是变化的，就会导致“迷你串口”的速率不稳定，这样就出现了无法正常使用的情况。目前解决方法就是，关闭蓝牙对硬件串口的使用，将硬件串口重新恢复给GPIO的串口使用，也就意味着树莓派3的板载蓝牙和串口，现在成了鱼和熊掌，两者无法兼得。

```c

gpio_pull(pin_number, Pull_None);

return gpio_call(pin_number, value, GPPUPPDN0, 2, GPIO_MAX_PIN);


unsigned int gpio_call(unsigned int pin_number, unsigned int value, unsigned int base, unsigned int field_size, unsigned int field_max) {
    unsigned int field_mask = (1 << field_size) - 1;
  
    if (pin_number > field_max) return 0;
    if (value > field_mask) return 0; 

    unsigned int num_fields = 32 / field_size;
    unsigned int reg = base + ((pin_number / num_fields) * 4);
    unsigned int shift = (pin_number % num_fields) * field_size;

    unsigned int curval = mmio_read(reg);
    curval &= ~(field_mask << shift);
    curval |= value << shift;
    mmio_write(reg, curval);

    return 1;
}
```

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
- [Raspberry Pi Bare Bones](https://wiki.osdev.org/Raspberry_Pi_Bare_Bones#Pi_3.2C_4)
- https://github.com/isometimes/rpi4-osdev
- https://zhuanlan.zhihu.com/p/140078824
- https://github.com/bztsrc/raspi3-tutorial/tree/master/01_bareminimum
- http://rcore-os.cn/rCore-Tutorial-Book-v3/chapter1/4first-instruction-in-kernel2.html

## ARM 汇编
- https://stackoverflow.com/questions/32341112/arm-assembly-local-labels
- https://stackoverflow.com/questions/53268118/whats-the-difference-between-mov-movz-movn-and-movk-in-armv8-assembly

手册
- 树莓派4B国产版评测：板载硬件资源全解析
 https://www.eefocus.com/article/1415671.html

## datasheet
- https://datasheets.raspberrypi.com/


## AXI 的 outstanding 模式
- https://blog.csdn.net/tbzj_2000/article/details/88042890
- https://zhuanlan.zhihu.com/p/376525373
- https://zhuanlan.zhihu.com/p/45122977
- https://zhuanlan.zhihu.com/p/157137488
- https://www.bilibili.com/video/BV1JT411s7GT
- https://www.lzrnote.cn/2021/10/08/axi%e6%80%bb%e7%ba%bf%e6%80%bb%e7%bb%93/
- https://forums.raspberrypi.com/viewtopic.php?t=181306

## UARTS PL011
- https://soclabs.org/technology/pl011-uart