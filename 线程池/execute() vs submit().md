# execute() vs submit()
1. execute()方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；

2. submit()方法用于提交需要返回值的任务。线程池会返回一个 Future 类型的对象，通过这个 Future 对象可以判断任务是否执行成功，并且可以通过 Future 的 get()方法来获取返回值，get()方法会阻塞当前线程直到任务完成，而使用 get（long timeout，TimeUnit unit）方法则会阻塞当前线程一段时间后立即返回，这时候有可能任务没有执行完。

我们以AbstractExecutorService接口中的一个 submit 方法为例子来看看源代码：
```
public Future<?> submit(Runnable task) {
        if (task == null) thrownew NullPointerException();
        RunnableFuture<Void> ftask = newTaskFor(task, null);
        execute(ftask);
        return ftask;
    }
```
上面方法调用的 newTaskFor 方法返回了一个 FutureTask 对象。
```
protected <T> RunnableFuture<T> newTaskFor(Runnable runnable, T value) {
        returnnew FutureTask<T>(runnable, value);
    }
```
我们再来看看execute()方法：
```
public void execute(Runnable command) {
      ...
    }
```