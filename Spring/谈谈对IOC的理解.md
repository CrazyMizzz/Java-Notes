# 总：
1. 控制反转：理论思想，原来的对象是由使用者来控制，有了spring以后，可以把整个对象交给spring来帮我们进行管理
2. DI：依赖注入，把对应的属性的值注入到具体的对象中，@Autowired，@Inject，populateBean完成属性值的注入
3. 容器：存放具体对象，使用map结构来存储。在spring中一般存在3级缓存。singletonObject存放完整的bean对象。

整个bean的生命周期，从创建到使用到销毁的过程全部都是由容器来管理。

---

# 分
1. 容器的创建过程，通过createBeanFacotry创建出一个Bean工厂（DefaultListableBeanFactory），向bean工厂中设置一些参数（BeanPostProcessor，Aware接口的子类）等属性
2. 加载解析bean对象，准备要创建的bean对象的定义对象beanDefinition（xml或者注解的解析过程）
3. beanFactoryPostProcessor的处理，PlaceHolderConfigurSupport，ConfigurationClassPostProcessor
4. BeanPostProcessor的注册功能，方便后续对bean对象完成具体的扩展功能
5. 通过反射的方式将BeanDefinition对象实例化成具体的bean对象
6. bean对象的初始化过程通过createBean,doCreateBean方法，以反射的方法创建对象，一般情况下是无参的构造方法（getDeclaredConstructor，newInstance），
7. 调用aware接口相关方法，invokeAwareMethod，完成BeanName，BeanFactory，BeanClassLoader对象的属性设置，
8. 调用beanPostProcessor的PostProcessorBeforeInitialization方法（使用比较多的有ApplicationContextPostProcessor，设置ApplicationContext，Enviroment，ResourceLoader）
9. 调用init-method方法，调用BeanPostProcessorAfterInitialLization方法
10. 生成完整的bean对象，通过getBean方法可以直接获取
11. 销毁过程    