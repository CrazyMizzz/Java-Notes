



添加到一级缓存：完成Bean的实例化，依赖注入，初始化等操作，生成完整的单例Bean后进行添加。

添加到二级缓存：通过getBean获取实例，在三级缓存中获取到时，添加到二级缓存，并清理三级缓存。

添加到三级缓存：通过createBeanInstance实例化Bean之后，调用populateBean进行依赖注入之前。