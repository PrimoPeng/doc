---
title: java web 的注解 
tags: annotation
grammar_cjkRuby: true
---

### java 内置 注解
@Override：用于修饰此方法覆盖了父类的方法;
@Deprecated：用于修饰已经过时的方法;
@SuppressWarnnings:用于通知java编译器禁止特定的编译警告。
　　　　
### springmvc annotation
--old
@Controller
--new
@RestController

--old
@Component
--new
@ComponentScan

--old
@RequestMapping
--new
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping

--new
@PathVariable
@RequestBody
@ResponseBody
@MatrixVariable
@RequestParam
@RequestHeader
@CookieValue
@RequestPart
@ModelAttribute
@SessionAttributes
@RequestAttribute

### spring core annotation
@Autowired
@Resource
@Bean
@Qualifier("main")
@Configuration
@ComponentScan
@Component
@PostConstruct
@PreDestroy

 1. Classpath scanning and managed components
@Configuration
@ComponentScan
@Qualifier("public")
@Repository
@Service
 2.  Using JSR 330 Standard Annotations
@Autowired
@Component
@Inject
@Scope("singleton")
@Qualifier / @Named
@Value
@Required
@Lazy
 3. Java-based container configuration
@Configuration
@Bean
@Import({ServiceConfig.class, RepositoryConfig.class})
@ImportResource("classpath:/com/acme/properties-config.xml")
```
@Configuration
@ImportResource("classpath:/com/acme/properties-config.xml")
public class AppConfig {

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource() {
        return new DriverManagerDataSource(url, username, password);
    }
}
```
@Profile("development") #加载不同的配置文件
@PropertySource("classpath:/com/myco/app.properties") 
```
@Configuration
@PropertySource("classpath:/com/${my.placeholder:default/path}/app.properties")
public class AppConfig {

    @Autowired
    Environment env;

    @Bean
    public TestBean testBean() {
        TestBean testBean = new TestBean();
        testBean.setName(env.getProperty("testbean.name"));
        return testBean;
    }
}
```

 4. Annotation-based event listeners
@EventListener
@Async

### spring 事务
@Transactional

 - Spring中的scope配置和@scope注解
singleton：单例模式，在整个Spring IoC容器中，使用singleton定义的Bean将只有一个实例

prototype：原型模式，每次通过容器的getBean方法获取prototype定义的Bean时，都将产生一个新的Bean实例

request：对于每次HTTP请求，使用request定义的Bean都将产生一个新实例，即每次HTTP请求将会产生不同的Bean实例。只有在Web应用中使用Spring时，该作用域才有效

session：对于每次HTTP Session，使用session定义的Bean豆浆产生一个新实例。同样只有在Web应用中使用Spring时，该作用域才有效

globalsession：每个全局的HTTP Session，使用session定义的Bean都将产生一个新实例。典型情况下，仅在使用portlet context的时候有效。同样只有在Web应用中使用Spring时，该作用域才有效
