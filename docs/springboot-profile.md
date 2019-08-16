# springboot获取active profile

!!! question "如何通过代码获取当前激活的profile"

    不同的profile环境下运行不同的code

## 方式一

```java

WebApplicationContext wac = WebApplicationContextUtils.getWebApplicationContext(request.getServletContext());
String profile = wac.getEnvironment().getActiveProfiles()[0];

```


## 方式二

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Configuration;

/**
 * @author wangjiuzhou (835540436@qq.com)
 * @date 2018/11/07
 *
 * 获取当前项目环境：local、dev、test、pro
 */
@Configuration
public class ProfileConfig {
    public static final String LOCAL_PROFILE = "local";
    public static final String DEV_PROFILE = "dev";
    public static final String TEST_PROFILE = "test";
    public static final String PRO_PROFILE = "pro";

    @Autowired
    private ApplicationContext context;

    public String getActiveProfile() {
        return context.getEnvironment().getActiveProfiles()[0];
    }
}

```


## 方式三

```java


@Component
public class SpringContextUtil implements ApplicationContextAware {

    


    private static ApplicationContext context = null;



    @Override
    public void setApplicationContext(ApplicationContext applicationContext)
            throws BeansException {
        context = applicationContext;
    }

    //获取applicationContext
    public static ApplicationContext getApplicationContext() {
        return applicationContext;
    }
 

	//通过name获取 Bean.
    public static Object getBean(String name){
        return getApplicationContext().getBean(name);
    }

    //通过class获取Bean.
    public static <T> T getBean(Class<T> clazz){
        return getApplicationContext().getBean(clazz);
    }

    //通过name,以及Clazz返回指定的Bean
    public static <T> T getBean(String name,Class<T> clazz){
        return getApplicationContext().getBean(name, clazz);
    }

 
    // 国际化使用
    public static String getMessage(String key) {
        return context.getMessage(key, null, Locale.getDefault());
    }
 
    // 获取当前环境
    public static String getActiveProfile() {
        return context.getEnvironment().getActiveProfiles()[0];
    }
}
```


























