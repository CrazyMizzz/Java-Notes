# JPA注解

1. 创建表

    @Entity声明一个类对应一个数据库实体。

    @Table 设置表名

    @Entity
    @Table(name = "role")
    public class Role {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        private String name;
        private String description;
        省略getter/setter......
    }
2. 创建主键

    @Id ：声明一个字段为主键。

    使用@Id声明之后，我们还需要定义主键的生成策略。我们可以使用 @GeneratedValue 指定主键生成策略。

    1.通过 @GeneratedValue直接使用 JPA 内置提供的四种主键生成策略来指定主键生成策略。

        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    
    JPA 使用枚举定义了 4 种常见的主键生成策略，如下：

        public enum GenerationType {
        
            /**
             * 使用一个特定的数据库表格来保存主键
             * 持久化引擎通过关系数据库的一张特定的表格来生成主键,
             */
            TABLE,

            /**
             *在某些数据库中,不支持主键自增长,比如Oracle、PostgreSQL其提供了一种叫做"序列(sequence)"        的机制生成主键
             */
            SEQUENCE,

            /**
             * 主键自增长
             */
            IDENTITY,

            /**
             *把主键生成策略交给持久化引擎(persistence engine),
             *持久化引擎会根据数据库在以上三种主键生成 策略中选择其中一种
             */
            AUTO
        }
    @GeneratedValue注解默认使用的策略是GenerationType.AUTO

        public @interface GeneratedValue {
        
            GenerationType strategy() default AUTO;
            String generator() default "";
        }

    通过 @GenericGenerator声明一个主键策略，然后 @GeneratedValue使用这个策略

        @Id
        @GeneratedValue(generator = "IdentityIdGenerator")
        @GenericGenerator(name = "IdentityIdGenerator", strategy = "identity")
        private Long id;
    等价于：

        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
    jpa 提供的主键生成策略有如下几种：

        public class DefaultIdentifierGeneratorFactory
        		implements MutableIdentifierGeneratorFactory, Serializable,         ServiceRegistryAwareService {
                
        	@SuppressWarnings("deprecation")
        	public DefaultIdentifierGeneratorFactory() {
        		register( "uuid2", UUIDGenerator.class );
        		register( "guid", GUIDGenerator.class );			// can be done with         UUIDGenerator + strategy
        		register( "uuid", UUIDHexGenerator.class );			// "deprecated" for new use
        		register( "uuid.hex", UUIDHexGenerator.class ); 	// uuid.hex is deprecated
        		register( "assigned", Assigned.class );
        		register( "identity", IdentityGenerator.class );
        		register( "select", SelectGenerator.class );
        		register( "sequence", SequenceStyleGenerator.class );
        		register( "seqhilo", SequenceHiLoGenerator.class );
        		register( "increment", IncrementGenerator.class );
        		register( "foreign", ForeignGenerator.class );
        		register( "sequence-identity", SequenceIdentityGenerator.class );
        		register( "enhanced-sequence", SequenceStyleGenerator.class );
        		register( "enhanced-table", TableGenerator.class );
        	}

        	public void register(String strategy, Class generatorClass) {
        		LOG.debugf( "Registering IdentifierGenerator strategy [%s] -> [%s]", strategy,      generatorClass.getName() );
        		final Class previous = generatorStrategyToClassNameMap.put( strategy,       generatorClass );
        		if ( previous != null ) {
        			LOG.debugf( "    - overriding [%s]", previous.getName() );
        		}
        	}

        }
3. 设置字段类型
    
    @Column 声明字段。

    示例：

    设置属性 userName 对应的数据库字段名为 user_name，长度为 32，非空

        @Column(name = "user_name", nullable = false, length=32)
        private String userName;
    设置字段类型并且加默认值，这个还是挺常用的。

        @Column(columnDefinition = "tinyint(1) default 1")
        private Boolean enabled;

4. 指定不持久化特定字段

    @Transient ：声明不需要与数据库映射的字段，在保存的时候不需要保存进数据库 。

    如果我们想让secrect 这个字段不被持久化，可以使用 @Transient关键字声明。

        @Entity(name="USER")
        public class User {

            ......
            @Transient
            private String secrect; // not persistent because of @Transient

        }
    除了 @Transient关键字声明， 还可以采用下面几种方法：

        static String secrect; // not persistent because of static
        final String secrect = "Satish"; // not persistent because of final
        transient String secrect; // not persistent because of transient
        一般使用注解的方式比较多。

5. 声明大字段

    @Lob:声明某个字段为大字段。

        @Lob
        private String content;
    更详细的声明：

    @Lob
    //指定 Lob 类型数据的获取策略， FetchType.EAGER 表示非延迟加载，而 FetchType.LAZY 表示延迟  加载 ；
        @Basic(fetch = FetchType.EAGER)
        //columnDefinition 属性指定数据表对应的 Lob 字段类型
        @Column(name = "content", columnDefinition = "LONGTEXT NOT NULL")
        private String content;
6. 创建枚举类型的字段

    可以使用枚举类型的字段，不过枚举字段要用@Enumerated注解修饰。

        public enum Gender {
            MALE("男性"),
            FEMALE("女性");

            private String value;
            Gender(String str){
                value=str;
            }
        }
        @Entity
        @Table(name = "role")
        public class Role {
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            private Long id;
            private String name;
            private String description;
            @Enumerated(EnumType.STRING)
            private Gender gender;
            省略getter/setter......
        }
    数据库里面对应存储的是 MALE/FEMALE。

 7. 增加审计功能

    只要继承了 AbstractAuditBase的类都会默认加上下面四个字段。

        @Data
        @AllArgsConstructor
        @NoArgsConstructor
        @MappedSuperclass
        @EntityListeners(value = AuditingEntityListener.class)
        public abstract class AbstractAuditBase {

            @CreatedDate
            @Column(updatable = false)
            @JsonIgnore
            private Instant createdAt;

            @LastModifiedDate
            @JsonIgnore
            private Instant updatedAt;

            @CreatedBy
            @Column(updatable = false)
            @JsonIgnore
            private String createdBy;

            @LastModifiedBy
            @JsonIgnore
            private String updatedBy;
        }
    我们对应的审计功能对应地配置类可能是下面这样的（Spring Security 项目）:

        @Configuration
        @EnableJpaAuditing
        public class AuditSecurityConfiguration {
            @Bean
            AuditorAware<String> auditorAware() {
                return () -> Optional.ofNullable(SecurityContextHolder.getContext())
                        .map(SecurityContext::getAuthentication)
                        .filter(Authentication::isAuthenticated)
                        .map(Authentication::getName);
            }
        }

    简单介绍一下上面涉及到的一些注解：

    @CreatedDate: 表示该字段为创建时间字段，在这个实体被 insert 的时候，会设置值

    @CreatedBy :表示该字段为创建人，在这个实体被 insert 的时候，会设置值

    @LastModifiedDate、@LastModifiedBy同理。

    @EnableJpaAuditing：开启 JPA 审计功能。

8. 删除/修改数据

    @Modifying 注解提示 JPA 该操作是修改操作,注意还要配合@Transactional注解使用。

        @Repository
        public interface UserRepository extends JpaRepository<User, Integer> {

            @Modifying
            @Transactional(rollbackFor = Exception.class)
            void deleteByUserName(String userName);
        }

9. 关联关系
    - @OneToOne 声明一对一关系
    - @OneToMany 声明一对多关系
    - @ManyToOne 声明多对一关系
    - @MangToMang 声明多对多关系