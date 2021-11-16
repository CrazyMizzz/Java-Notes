# Spring事务管理如何实现？

## 总

spring的事务是由aop来实现的，首先要生成具体的代理对象，然后按照aop的整套流程来执行具体操作逻辑，一般情况下要通过通知来完成核心功能，但是事务不是通过通知来实现的，而是通过一个TransactionInterceptor来实现的，然后调用invoke来实现具体逻辑

## 分

1. 首先解析各个方法上事务的属性，根据具体的属性来判断是否开启新事务
2. 当需要开启的时候，获取数据库连接，关闭自动提交功能，开启事务
3. 执行具体的sql逻辑操作
4. 在操作过程总如果执行失败了，会通过completeTransacionAfterThrowing来完成事务的回滚，回滚是通过doRollBack方法来实现的，实现的时候也是先获取Connection对象，通过连接对象来rollback
5. 如果执行过程中没有意外，通过commitTransactionAfterReturning来完成事务的提交，提交的具体逻辑是通过doCommit方法来实现的。
6. 当事务执行完毕之后，清楚相关事务信息，cleanupTransactionInfo

TransactionInfo，TransactionStatus