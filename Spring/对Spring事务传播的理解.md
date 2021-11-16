# 总

事务的传播特性指的是不同方法的嵌套调用过程中，事务应该如何进行处理，是用同一个事务还是不同事务，出现异常的时候是提交还是回滚，在日常生活中使用比较多的是required,requeries_new,nested

# 分

1. 事务主要分为三类： 支持当前事务，不支持当前事务，嵌套事务
2. 如果外层方法是 required，内层方式是 required，requires_new,nested
3. 如果外层方法是 requires_new,内层方法是 required,requires_new,nested
4. 如果外层方法是 nested, 内层方法是 required,requires_new,nested  

核心处理逻辑非常简单

判断内外方法是否是同一个事务
    
    是： 异常统一在外层方法处理
    
    不是： 内层方法有可能影响到外层方法，但是外层方法不会影响到内层方法