---
description: 2021-02-1
---

# day23

## 完成情况

{% hint style="success" %}
每一天都学习并记录
{% endhint %}

{% hint style="warning" %}
每天看至少3个leetcode的题目
{% endhint %}

## 学习内容

### 注解开发

```java
@Service("userService")
@Scope("singleton")  //单bean
//@Scope("prototype")  //多bean
public class UserServiceImpl implements UserService {


    @Value("doublez")
    private String driver;


//    @Autowired //按照数据类型从Spring容器中匹配
//    @Qualifier("userDao")  //如果有多个bean，就需要加 Qualifier，按照id从容器中匹配
    @Resource(name = "userDao")//相当于@Autowired+@Qualifier
    UserDao userDao = new UserDaoImpl();


    @Override
    public void save() {
        userDao.save();
    }
    @PostConstruct
    public void init(){
        System.out.println("init...");
    }
    @PreDestroy
    public void destroy(){
        System.out.println("destroy");
    }
}
```

新注解

```java
@Configuration  //核心配置类
@ComponentScan("pro.doublez")   //组件扫描
@PropertySource("classpath:jdbc.properties")  //外部配置
//@import
public class SpringConfiguration {

    @Bean("dataSource")  //会将当前方法的返回值以指定名称存储到Spring容器中
    public DataSource getDataSource(){
        return null;
    }
}
```

```java
ApplicationContext app=new AnnotationConfigApplicationContext(SpringConfiguration.class);
```

#### 整合MyBatis

```java
public class MyBatisConfig {
    @Bean
    public SqlSessionFactoryBean getSqlSessionFactoryBean(@Autowired DataSource dataSource) {
        SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
        ssfb.setTypeAliasesPackage("pro.doublez.domain");
        ssfb.setDataSource(dataSource);
        return ssfb;

    }

    @Bean
    public MapperScannerConfigurer getMapperScannerConfigurer() {
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("pro.doublez.dao");
        return msc;
    }
}
```

```java
public class JDBCConfig {

    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;

    @Bean("dataSource")
    public DataSource getDataSource() {
        DruidDataSource ds = new DruidDataSource();
        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(username);
        ds.setPassword(password);
        return ds;
    }
}
```

#### 整合Junit

```java
//设定Spring的类加载器
@RunWith(SpringJUnit4ClassRunner.class)
//设定加载的Spring上下文的位置
@ContextConfiguration(classes = SpringConfig.class)
public class UserServiceTest {

    @Autowired
    private AccountService accountService;

    @Test
    public void testFindById() {
        Account ac = accountService.findById(2);
        Assert.assertEquals("Jock1", ac.getName());
    }

    @Test
    public void testFindAll() {
        List<Account> list = accountService.findAll();
        Assert.assertEquals(2, list.size());
    }
}
```

IoC底层核心原理

* IoC核心接口
  * BeanFactory
* bean加载过程解析
* bean初始化过程解析

组件扫描器

* 开发过程中，需要根据需求加载必要的bean，排除指定的bean

```java
@ComponentScan(value = "pro.doublez",excludeFilters = @ComponentScan.Filter(
        type = FilterType.ANNOTATION,
        classes = Service.class

))
```

```java
@ComponentScan(value = "pro.doublez",excludeFilters = @ComponentScan.Filter(
        type = FilterType.CUSTOM,
        classes = MyTypeFilter.class

))
```

```java
public class MyTypeFilter implements TypeFilter {
    @Override
    public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory) throws IOException {

        ClassMetadata classMetadata = metadataReader.getClassMetadata();
        String className = classMetadata.getClassName();
        System.out.println(className);

        if(className.equals("pro.doublez.service.AccountService")){
            return true;
        }

        return false;
    }
}
```

自定义导入器

```java
public class CustomerImportSelector implements ImportSelector {

    private String expression;

    public CustomerImportSelector(){
        try {
            //初始化时指定加载的properties文件名
            Properties loadAllProperties = PropertiesLoaderUtils.loadAllProperties("import.properties");
            //设定加载的属性名
            expression = loadAllProperties.getProperty("selectorPath");
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }

    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        //1.定义扫描包的名称
        String[] basePackages = null;
        //2.判断有@Import注解的类上是否有@ComponentScan注解
        if (importingClassMetadata.hasAnnotation(ComponentScan.class.getName())) {
            //3.取出@ComponentScan注解的属性
            Map<String, Object> annotationAttributes = importingClassMetadata.getAnnotationAttributes(ComponentScan.class.getName());
            //4.取出属性名称为basePackages属性的值
            basePackages = (String[]) annotationAttributes.get("basePackages");
        }
        //5.判断是否有此属性（如果没有ComponentScan注解则属性值为null，如果有ComponentScan注解，则basePackages默认为空数组）
        if (basePackages == null || basePackages.length == 0) {
            String basePackage = null;
            try {
                //6.取出包含@Import注解类的包名
                basePackage = Class.forName(importingClassMetadata.getClassName()).getPackage().getName();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
            //7.存入数组中
            basePackages = new String[] {basePackage};
        }
        //8.创建类路径扫描器
        ClassPathScanningCandidateComponentProvider scanner = new ClassPathScanningCandidateComponentProvider(false);
        //9.创建类型过滤器(此处使用切入点表达式类型过滤器)
        TypeFilter typeFilter = new AspectJTypeFilter(expression,this.getClass().getClassLoader());
        //10.给扫描器加入类型过滤器
        scanner.addIncludeFilter(typeFilter);
        //11.创建存放全限定类名的集合
        Set<String> classes = new HashSet<>();
        //12.填充集合数据
        for (String basePackage : basePackages) {
            scanner.findCandidateComponents(basePackage).forEach(beanDefinition -> classes.add(beanDefinition.getBeanClassName()));
        }
        //13.按照规则返回
        return classes.toArray(new String[classes.size()]);
    }
}
```

自定义注册器

```java
public class CustomerImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {

    private String expression;

    public CustomerImportBeanDefinitionRegistrar(){
        try {
            //初始化时指定加载的properties文件名
            Properties loadAllProperties = PropertiesLoaderUtils.loadAllProperties("import.properties");
            //设定加载的属性名
            expression = loadAllProperties.getProperty("path");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        //1.定义扫描包的名称
        String[] basePackages = null;
        //2.判断有@Import注解的类上是否有@ComponentScan注解
        if (importingClassMetadata.hasAnnotation(ComponentScan.class.getName())) {
            //3.取出@ComponentScan注解的属性
            Map<String, Object> annotationAttributes = importingClassMetadata.getAnnotationAttributes(ComponentScan.class.getName());
            //4.取出属性名称为basePackages属性的值
            if (annotationAttributes != null) {
                basePackages = (String[]) annotationAttributes.get("basePackages");
            }
        }
        //5.判断是否有此属性（如果没有ComponentScan注解则属性值为null，如果有ComponentScan注解，则basePackages默认为空数组）
        if (basePackages == null || basePackages.length == 0) {
            String basePackage = null;
            try {
                //6.取出包含@Import注解类的包名
                basePackage = Class.forName(importingClassMetadata.getClassName()).getPackage().getName();
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
            //7.存入数组中
            basePackages = new String[] {basePackage};
        }
        //8.创建类路径扫描器
        ClassPathBeanDefinitionScanner scanner = new ClassPathBeanDefinitionScanner(registry, false);
        //9.创建类型过滤器(此处使用切入点表达式类型过滤器)
        TypeFilter typeFilter = new AspectJTypeFilter(expression,this.getClass().getClassLoader());
        //10.给扫描器加入类型过滤器
        scanner.addIncludeFilter(typeFilter);
        //11.扫描指定包
        scanner.scan(basePackages);
    }
}
```

bean的初始化过程

```java
public class MyBeanFactory implements BeanFactoryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {
        System.out.println("bean工厂制作之后balabala");
    }
}
```

```java
public class MyBean implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("在bean:" + beanName + "制作之前balabala");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("在bean:" + beanName + "制作之后balabala");
        return null;
    }
}
```

```java
@Service("accountService")
public class AccountServiceImpl implements AccountService, InitializingBean {

    @Resource
    private AccountDao accountDao;

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("accountServiceImpl...bean...init...");
    }
```

* FactoryBean：封装单个bean的创建过程，一般是框架才用
* BeanFactory：Spring容器的顶层接口，定义了bean相关的操作

### AOP

Aspect Oriented Programming 面向切面编程

通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术

* 在程序运行期间，在不修改源码的情况下对方法进行增强
* 减少重复代码，提高开发效率，并且便于维护

