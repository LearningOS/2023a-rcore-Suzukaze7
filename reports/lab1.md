### 1
报错内容
```
[kernel] PageFault in application, bad addr = 0x0, bad instruction = 0x804003c4, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
```

按顺序从上到下解释
- bad_address 访问了非法地址，所以报缺页异常错误
- bad_instructions 在 U 态使用了 S 态特权指令，所以报非法指令错误
- bad_instructions 同样在 U 态使用了 S 态特权指令，所以报非法指令错误

rustsbi version: 0.2.0-alpha.2

### 2
1. a0 代表着当前内核栈栈顶；开始运行用户程序时，处理trap返回用户程序时
2. 特殊处理了`sstatus, sepc, `