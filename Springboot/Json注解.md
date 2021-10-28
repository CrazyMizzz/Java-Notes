# Json处理注解

1. 过滤 json 数据

    @JsonIgnoreProperties 作用在类上用于过滤掉特定字段不返回或者不解析。

    //生成json时将userRoles属性过滤

        @JsonIgnoreProperties({"userRoles"})
        public class User {
        
            private String userName;
            private String fullName;
            private String password;
            private List<UserRole> userRoles = new ArrayList<>();
        }
    @JsonIgnore一般用于类的属性上，作用和上面的@JsonIgnoreProperties 一样。

        public class User {

            private String userName;
            private String fullName;
            private String password;
           //生成json时将userRoles属性过滤
            @JsonIgnore
            private List<UserRole> userRoles = new ArrayList<>();
        }

2. 格式化 json 数据
    
    @JsonFormat一般用来格式化 json 数据。

    比如：

        @JsonFormat(shape=JsonFormat.Shape.STRING, pattern="yyyy-MM-dd'T'HH:mm:ss.  SSS'Z'",  timezone="GMT")
        private Date date;

3. 扁平化对象
    
        @Getter
        @Setter
        @ToString
        public class Account {
            private Location location;
            private PersonInfo personInfo;

        @Getter
        @Setter
        @ToString
        public static class Location {
             private String provinceName;
             private String countyName;
          }
        @Getter
        @Setter
        @ToString
        public static class PersonInfo {
            private String userName;
            private String fullName;
          }
        }
    未扁平化之前：

        {
            "location": {
                "provinceName":"湖北",
                "countyName":"武汉"
            },
            "personInfo": {
                "userName": "coder1234",
                "fullName": "shaungkou"
            }
        }
    使用@JsonUnwrapped 扁平对象之后：

        @Getter
        @Setter
        @ToString
        public class Account {
            @JsonUnwrapped
            private Location location;
            @JsonUnwrapped
            private PersonInfo personInfo;
            ......
        }
        {
          "provinceName":"湖北",
          "countyName":"武汉",
          "userName": "coder1234",
          "fullName": "shaungkou"
        }