# 总

aop是ioc的一个扩展功能，现有ioc，再有的aop

只是在ioc的整个流程中新增的一个扩展点而已：BeanPostProcessor的PostProcessorAfterInitializion

动态代理

# 分

1. 代理对象的创建过程（advice，切面，在哪些方法上进行动态代理）
2. 通过jdk或者cglib的方式生成代理对象
3. 再执行方法调用的时候，会调用到生成的字节码文件中，直接会找到DynamicAdvisoredInterceptor类中的intercept方法，从此方法开始执行
4. 根据之前定义好的通知来生成拦截器链
5. 从拦截器链中依次获取每个通知开始执行，在执行过程中为了方便找到下一个通知是哪一个，会有CglibMethodInvocation的对象