## 总结
- 向任务控制块中添加两个字段，用来存储系统调用次数、与任务开始时间
- 在`run_first_task`与`run_next_task`中添加记录任务开始时间的代码
- 添加函数`increase_syscall_times`用来增加系统调用次数，并在`syscall`中调用
- 添加函数`get_task_info`用来获取`TaskInfo`所需信息，并在系统调用中调用

## 简答题
### 1
报错内容
```
[kernel] PageFault in application, bad addr = 0x0, bad instruction = 0x804003c4, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
```

按顺序从上到下解释
- `bad_address`访问了非法地址，所以报缺页异常错误
- `bad_instructions`在 U 态使用了 S 态特权指令，所以报非法指令错误
- `bad_instructions`同样在 U 态使用了 S 态特权指令，所以报非法指令错误

rustsbi version: 0.2.0-alpha.2

### 2
1. a0 代表着当前内核栈栈顶；开始运行用户程序时，处理 trap 返回用户程序时
2. 特殊处理了`sstatus, sepc, sscratch`
    - `sstatus`为了在`sret`后能正确切换到 U 态
    - `sepc`为了在`sret`后能返回到正确的地址
    - `sscratch`为了在下次 trap 后能正确获取内核栈地址
### 3
因为`x2`与`sscratch`交换的时候已经保存在`sscratch`中，而`x4`因为应用程序并不会使用

### 4
`sp`保存内核栈栈顶地址，而`sscratch`保存用户栈栈顶地址

### 5
`sret`，其将权限模式设置为`sstatus`的`SPP`域中的值

## 荣誉准则
1. 在完成本次实验的过程（含此前学习的过程）中，我曾分别与 以下各位 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容：

```
关于 src/task/mod.rs中的函数为何需要封装

交流对象：公开的API需要保证签名兼容性
我：意思是解耦
```

2. 此外，我也参考了 以下资料 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容：

```
RISC-V-Reader-Chinese-v2p1
大部分简答题都是从本书中找到答案的
```

3. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

4. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。 我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。 我清楚地知道，以上情况均为本课程纪律所禁止，若违反，对应的实验成绩将按“-100”分计。