#### mybatis-plus分页注意事项：

1.先在springboot中添加如下配置文件

```
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import com.baomidou.mybatisplus.plugins.PaginationInterceptor;
@Configuration
public class MybatisPlusConfig {
/**
*   mybatis-plus分页插件
*/
    @Bean
    public PaginationInterceptor paginationInterceptor() {
       		PaginationInterceptor page = new PaginationInterceptor();
     	page.setDialectType("mysql");
       return page;
    }
    }
```

2.将pom.xml中的pagehelper插件去掉

```
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.3</version>
        </dependency>
```