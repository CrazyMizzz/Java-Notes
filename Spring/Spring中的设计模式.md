# Spring 框架中用到了哪些设计模式

- 工厂设计模式 : Spring 使用工厂模式通过 BeanFactory、ApplicationContext 创建 bean 对象。
- 原型模式： 指定作用域为prototype
- 装饰者模式： IO类，BeanWrapper
- 代理模式 : Spring AOP 功能的实现，动态代理
- 单例模式 : Spring 中的 Bean 默认都是单例的。
- 模板模式 : postProcessBeanFactory 和 jdbcTemplate、hibernateTemplate 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- 观察者模式: listener， event， multicast
- 适配器模式 : Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配Controller，Adapter