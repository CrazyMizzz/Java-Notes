## 用法

1. 让主线程await，业务线程进行业务处理，处理完成时调用countdownLatch.countDown().

2. 让业务线程await，主线程处理完数据以后进行countDown(),此时业务线程被唤醒，然后去主线程拿数据进行操作

## 原理

AQS共享锁+state变量