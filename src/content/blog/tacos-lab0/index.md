---
title: 'TacOS 踩坑记 | Lab 0: Appetizer'
publishDate: 2026-09-15 15:26:23
description: 'TacOS 实现过程个人纪录'
tags:
  - '操作系统'
  - 'lab'
  - 'coding'
heroImage: { src: './daimao1.jpg', color: '#B4C6DA' }
language: '中文'
---

## Environment preparation

没啥好说的，按照文档来做即可。装完 Docker 之后 `git clone` 一下，然后在 vscode 中选择 `Dev Containers: Open Folder in Container...`。rust-analyzer 插件可能需要一些额外的配置才能正常工作，都交给 AI 做就好了（）

## Booting Tacos

直接 `cargo run` 即可，终端输出如下：

```
OpenSBI v1.4-15-g9c8b18e
   ____                    _____ ____ _____
  / __ \                  / ____|  _ \_   _|
 | |  | |_ __   ___ _ __ | (___ | |_) || |
 | |  | | '_ \ / _ \ '_ \ \___ \|  _ < | |
 | |__| | |_) |  __/ | | |____) | |_) || |_
  \____/| .__/ \___|_| |_|_____/|____/_____|
        | |
        |_|

Platform Name             : riscv-virtio,qemu
Platform Features         : medeleg
Platform HART Count       : 1
Platform IPI Device       : aclint-mswi
Platform Timer Device     : aclint-mtimer @ 10000000Hz
Platform Console Device   : semihosting
Platform HSM Device       : ---
Platform PMU Device       : ---
Platform Reboot Device    : syscon-reboot
Platform Shutdown Device  : syscon-poweroff
Platform Suspend Device   : ---
Platform CPPC Device      : ---
Firmware Base             : 0x80000000
Firmware Size             : 323 KB
Firmware RW Offset        : 0x40000
Firmware RW Size          : 67 KB
Firmware Heap Offset      : 0x48000
Firmware Heap Size        : 35 KB (total), 2 KB (reserved), 9 KB (used), 23 KB (free)
Firmware Scratch Size     : 4096 B (total), 328 B (used), 3768 B (free)
Runtime SBI Version       : 2.0

Domain0 Name              : root
Domain0 Boot HART         : 0
Domain0 HARTs             : 0*
Domain0 Region00          : 0x0000000000100000-0x0000000000100fff M: (I,R,W) S/U: (R,W)
Domain0 Region01          : 0x0000000002000000-0x000000000200ffff M: (I,R,W) S/U: ()
Domain0 Region02          : 0x0000000080040000-0x000000008005ffff M: (R,W) S/U: ()
Domain0 Region03          : 0x0000000080000000-0x000000008003ffff M: (R,X) S/U: ()
Domain0 Region04          : 0x000000000c400000-0x000000000c5fffff M: (I,R,W) S/U: (R,W)
Domain0 Region05          : 0x000000000c000000-0x000000000c3fffff M: (I,R,W) S/U: (R,W)
Domain0 Region06          : 0x0000000000000000-0xffffffffffffffff M: () S/U: (R,W,X)
Domain0 Next Address      : 0x0000000080200000
Domain0 Next Arg1         : 0x0000000082200000
Domain0 Next Mode         : S-mode
Domain0 SysReset          : yes
Domain0 SysSuspend        : yes

Boot HART ID              : 0
Boot HART Domain          : root
Boot HART Priv Version    : v1.10
Boot HART Base ISA        : rv64imafdch
Boot HART ISA Extensions  : zicntr
Boot HART PMP Count       : 16
Boot HART PMP Granularity : 2 bits
Boot HART PMP Address Bits: 54
Boot HART MHPM Info       : 0 (0x00000000)
Boot HART Debug Triggers  : 0 triggers
Boot HART MIDELEG         : 0x0000000000001666
Boot HART MEDELEG         : 0x0000000000f0b509
[36 ms] Hello, World!
[123 ms] Goodbye, World!
```

## Debugging

这一部分我们需要使用曾经在 ICS 中用过的 gdb 来追踪 Tacos 的启动过程。按照附录中 Debugging 部分的说明，首先以如下方式启动 Tacos，它会使 Tacos 在启动后将 CPU 停在初始状态，并开启一个端口等待 gdb 连接：

```
cargo run -- -s -S
```

然后打开新的终端来启动 gdb：

```
gdb-multiarch
(gdb) file-debug
(gdb) debug-qemu
```

发现显示

```
0x0000000000001000 in ?? ()
```

说明 gdb 已经连接到了 Tacos。可以看出程序启动时的初始地址在 `0x1000`，使用 `x/10i 0x1000` 反汇编该处的 10 条指令，得到如下输出：

```
=> 0x1000:      auipc   t0,0x0
   0x1004:      addi    a2,t0,40
   0x1008:      csrr    a0,mhartid
   0x100c:      ld      a1,32(t0)
   0x1010:      ld      t0,24(t0)
   0x1014:      jr      t0
   0x1018:      unimp
   0x101a:      .2byte  0x8000
   0x101c:      unimp
   0x101e:      unimp
```

因此第一条指令就是 `auipc  t0, 0x0`。结合文档，我们知道前面的部分就是所谓的 Zero Stage Bootloader，它进行了若干准备工作，最后在 `0x1014` 处通过 `jr t0` 跳转到 SBI。从汇编中容易看出跳转到的地址是 `0x80000000`，这也可以通过五次 `si` 后，用 `p/x $t0` 来验证：

```
0x0000000000001014 in ?? ()
(gdb) p/x $t0
$1 = 0x80000000
```

接下来，按照要求我们需要追踪 `main` 函数的执行，首先来看 `main` 函数的定义：

```
pub extern "C" fn main(hart_id: usize, dtb: usize) -> ! {
    ...
}
```

因此问题中提到的 `hart_id` 和 `dtb` 是该函数的两个参数，它们应当存放在 `a0` 和 `a1` 两个寄存器中。为此，我们先通过 `b *main` 在 `main` 函数处打断点，然后 `c` 让程序执行过去：

```
(gdb) b *main
Breakpoint 1 at 0xffffffc08021338c: file src/main.rs, line 50.
(gdb) c
Continuing.

[省略若干内容]

Breakpoint 1, tacos::main (hart_id=2147773992, dtb=16) at src/main.rs:50
50      pub extern "C" fn main(hart_id: usize, dtb: usize) -> ! {
```

然后查看这两个寄存器的值（其实上文也能直接看）：

```
(gdb) p/x $a0
$2 = 0x0
(gdb) p/x $a1
$3 = 0x82200000
```

与启动信息进行比较：

```
Domain0 Next Address      : 0x0000000080200000
Domain0 Next Arg1         : 0x0000000082200000
Domain0 Next Mode         : S-mode
Boot HART ID              : 0
```

可以看到 `Boot HART ID` 正是 `hart_id`，而 `Domain0 Next Arg1` 正是 `dtb`，另外两个则不是参数。可以问问 AI 这些数值的含义：`Domain0 Next Address` 指的是 SBI 接下来要跳转到的地址，也就是 Tacos 内核的起始地址；而 `Domain0 Next Arg1`，是 Device Tree Blob（`dtb`）的地址，它存有一些供内核使用的硬件信息；`Domain0 Next Mode` 表示我们的内核将运行在 Supervisor-mode 之下，这是一个权限等级，SBI 本身是运行在 Machine-mode 之下；`Boot HART ID` 中的 `HART` 全名 Hardware Thread，每一个 HART 可以理解为一个逻辑 CPU，带有独立的一套寄存器等。

最后，我们需要追踪 `console_putchar` 函数，但这里打断点有点小问题：

```
(gdb) b *console_putchar
No symbol 'console_putchar' in current context
```

直接反汇编也不行：

```
(gdb) disassemble console_putchar
No symbol 'console_putchar' in current context
```

说明函数名是被修饰过的，`console_putchar` 并不在符号表里面。先使用 `info` 来查找一下：

```
(gdb) info functions console_putchar
All functions matching regular expression "console_putchar":

File src/sbi.rs:
54:     static fn tacos::sbi::legacy::console_putchar(usize);
```

然后就 OK 了：

```
(gdb) disassemble tacos::sbi::legacy::console_putchar
Dump of assembler code for function _ZN5tacos3sbi6legacy15console_putchar17h7d2ca8ba15011994E:
   0xffffffc08021319c <+0>:     addi    sp,sp,-48
   0xffffffc08021319e <+2>:     sd      ra,40(sp)
   0xffffffc0802131a0 <+4>:     sd      s0,32(sp)
   0xffffffc0802131a2 <+6>:     addi    s0,sp,48
   0xffffffc0802131a4 <+8>:     sd      a0,-24(s0)
   0xffffffc0802131a8 <+12>:    li      a7,1
   0xffffffc0802131aa <+14>:    li      a6,0
   0xffffffc0802131ac <+16>:    ecall
   0xffffffc0802131b0 <+20>:    sd      a0,-40(s0)
   0xffffffc0802131b4 <+24>:    sd      a1,-32(s0)
   0xffffffc0802131b8 <+28>:    ld      ra,40(sp)
   0xffffffc0802131ba <+30>:    ld      s0,32(sp)
   0xffffffc0802131bc <+32>:    addi    sp,sp,48
   0xffffffc0802131be <+34>:    ret
End of assembler dump.
```

从汇编中可以看出，在 `ecall` 之前，`a6` 是 `0`，`a7` 是 `1`，打断点验证：

```
(gdb) b *0xffffffc0802131ac
Breakpoint 2 at 0xffffffc0802131ac: file src/sbi.rs, line 24.
(gdb) c
Continuing.

Breakpoint 2, tacos::sbi::legacy::console_putchar (char=91) at src/sbi.rs:24
24                      core::arch::asm!("ecall",
(gdb) p/x $a6
$4 = 0x0
(gdb) p/x $a7
$5 = 0x1
```

询问 ai 可知，`a7` 是所谓的 `EID`，即扩展 ID，决定调用哪一类 SBI 服务；`a6` 则是 FID，即功能 ID，决定调用该类服务中的哪一个具体功能，有点类似于系统调用号。

## Kernel Monitor

最后我们需要扩展 `main` 函数做一些非常简单的功能。这里主要就是处理输入输出，`sbi.rs` 中已经提供了 `console_getchar` 和 `console_putchar`，直接使用即可，另外我们在 `main` 函数已有的代码中会发现还有 `kprintln!` 这个封装好的宏，它定义在 `console.rs` 中，会调用 `kprint!`，后者进一步调用 `write!`，直接用这个输出更简单。最后效果是：

```
[12 ms] Hello, World!
PKUOS> whoami
[16709 ms] 114514
PKUOS> other input
[21993 ms] invalid command
PKUOS> exit
[24998 ms] Goodbye, World!
```