什么是happens-before？
先行发生原则（Happens-Before）是判断数据是否存在竞争，线程是否安全的主要依据
## 总结： 如果两个操作之间具有happens-before关系，那么前一个关系的结果就会对后面的一个操作可见。

常见的happens-before规则：
- 程序顺序规则
- 锁规则
- volatile变量规则
- 传递性
- 线程启动规则 
- 线程终止规则
- 线程中断规则
