## 总结
- 将之前实验的功能移植到本次实验，主要需要将获取当前任务改为调用`current_task()`
- 仿照`fork(), exec()`实现`spawn()`
- 在`TCB`中添加字段`stride, priority`，实现`set_priority`；同时修改`TaskManager`将 `TCB`用`Vec`存储，并修改`fetch()`逻辑实现按照优先级调度

## 问答作业
- 不是的，因为上一次会选择`p2`，使得`p2.stride += 10`导致上溢变成 4，而这一次调度还是选择`stride`较小的`p2`
- 证明：
  - 因为`priority >= 2`
  - 则`pass = BigStride / pririty <= BigStride / 2`
  - 假设当前满足`STRIDE_MAX – STRIDE_MIN <= BigStride / 2`，调度之后`STRIDE_MIN += pass`，易知仍然满足要求
  - 而初始状态所有`stride`都为 0，满足要求，所以如果严格按照算法执行，会一直满足
- 补全
    ```rust
    impl PartialOrd for Stride {
        fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
            let x = self.0;
            let y = other.0;
            let abs = if x > y { x - y } else { y - x };

            Some(if abs > 255 / 2 {
                Ordering::Greater
            } else {
                Ordering::Less
            })
        }
    }
    ```

1. 在完成本次实验的过程（含此前学习的过程）中，我曾分别与 以下各位 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容：

```
无
```

2. 此外，我也参考了 以下资料 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容：

```
无
```

3. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

4. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。 我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。 我清楚地知道，以上情况均为本课程纪律所禁止，若违反，对应的实验成绩将按“-100”分计。