# CompletableFuture

### CompletableFuture在Future的基础上进行了优化。
### CompletableFuture类实现了CompletionStage和Future接口，因此你可以像Future那样使用它。


1. runAsync方法：它以Runnabel函数式接口类型为参数，所以CompletableFuture的计算结果为空。

    supplyAsync方法以Supplier函数式接口类型为参数，CompletableFuture的计算结果类型为U。
    
        public static CompletableFuture<Void> runAsync(Runnable runnable)
        public static CompletableFuture<Void> runAsync(Runnable runnable, Executor  executor)
        public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier)
        public static <U> CompletableFuture<U> supplyAsync(Supplier<U> supplier, Executor executor)

2. thenApply: 这些方法的输入是上一个阶段计算后的结果，返回值是经过转化后结果

        public <U> CompletionStage<U> thenApply(Function<? super T,? extends U> fn);
        public <U> CompletionStage<U> thenApplyAsync(Function<? super T,? extends U> fn);
        public <U> CompletionStage<U> thenApplyAsync(Function<? super T,? extends U> fn,Executor executor);

3. thenAccept: 这些方法针对结果进行消费，入参是Consumer，没有返回值

        public CompletionStage<Void> thenAccept(Consumer<? super T> action);
        public CompletionStage<Void> thenAcceptAsync(Consumer<? super T> action);
        public CompletionStage<Void> thenAcceptAsync(Consumer<? super T> action,Executor executor);

4. 结合两个CompletionStage的结果，进行转化后返回

        public <U,V> CompletionStage<V> thenCombine(CompletionStage<? extends U> other,BiFunction<? super T,? super U,? extends V> fn);
        public <U,V> CompletionStage<V> thenCombineAsync(CompletionStage<? extends U> other,BiFunction<? super T,? super U,? extends V> fn);
        public <U,V> CompletionStage<V> thenCombineAsync(CompletionStage<? extends U> other,BiFunction<? super T,? super U,? extends V> fn,Executor executor); 

5. 两个CompletionStage，谁计算的快，就用那个CompletionStage的结果进行下一步的处理

    两种渠道完成同一个事情，就可以调用这个方法，找一个最快的结果进行处理，最终有返回值。

        public <U> CompletionStage<U> applyToEither(CompletionStage<? extends T> other,Function<? super T, U> fn);
        public <U> CompletionStage<U> applyToEitherAsync(CompletionStage<? extends T> other,Function<? super T, U> fn);
        public <U> CompletionStage<U> applyToEitherAsync(CompletionStage<? extends T> other,Function<? super T, U> fn,Executor executor);

        /**
         * @program: callable
         * @description: test
         * @author: Mr.Wang
         * @create: 2018-08-12 12:36
         **/
        public class TestCompleteFuture {
            public static void main(String[] args){
            
               String result = CompletableFuture.supplyAsync(()->{
                   try {
                       Thread.sleep(100);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
                   return "Hi Boy";
               }).applyToEither(CompletableFuture.supplyAsync(()->{
                   try {
                       Thread.sleep(300);
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
                   return "Hi Girl";
               }),(s)->{return s;}).join();
               System.out.println(result);
            }
        }

6. 运行时出现了异常，可以通过exceptionally进行补偿

        public CompletionStage<T> exceptionally(Function<Throwable, ? extends T> fn);


