 

# ----------目录----------

[TOC]



# 1、yml文件语法

```
SpringBoot提供了大量的默认配置，如果要修改默认配置，需要在配置文件中修改。

SpringBoot默认会加载resource下的配置文件：

- application*.yml
- application*.yaml
- application*.properties

这也是配置文件的加载顺序，如果某个key有多个配置，则后加载的会覆盖之前加载的配置。

yml、yaml是同一种文件，后缀写成yml、yaml都可以。

一般使用application.yml。

springboot在不同的环境下有默认的加载文件：

- application 开发、测试、生产都会加载，公共的
- application-dev 只在开发环境加载（调试src/main）
- application-test 只在测试环境加载（调试src/test）
- application-prod 只在生产环境加载（正式打包部署）
```

#### （1）普通字段：

```
name: zhangsan
```

值不加引号

#### （2）对象、Map

对象、Map的配置方式是一样的。

```
student: #对象名、Map名
  id: 1  #配置一个属性、一个键值对
  name: chy
  age: 20
  score: 100
```

值可以是对象：

```
server:
  port: 8080
  servlet:
    context-path: /springboot
```

servlet的值就是一个对象。不配置端口，默认为8080；不配置context-path，默认为/

#### （3）数组、List

```
city: [beijing,shanghai,guangzhou,shenzhen]
student: [{name: zhangsan,age: 20},{name: lisi,age: 20}]  #元素可以是对象
```

值，不管是key的值，还是数组元素，都不加引号。

key、value冒号分隔，冒号后面都要加一个空格，加了空格后key会变成橙色，才有效。

## 2、SpringBoot配置文件之Yml语法

1. ### Spring Boot使用一个全局的配置文件（配置文件名是固定的）

   a) application.properties
   b) application.yml
   配置文件放在src/main/resources目录或者类路径/config/下

2. .yml是YAML（YAML Ain’t Markup Language）语言的文件，以数据为中心，比json/xml等更适合做配置文件
   全局配置文件可以对一些默认配置值进行修改

3. .yml配置示例
   配置端口号：

```
server:
  port: 8081
```

1. yaml基本语法：
   a) k:(空格)v：表示一对键值对（空格必须有），以空格的缩进来控制层级关系；只要是左对齐的一列数据，则表示都是同一个层级的。例如如下代码:

```
server:
  port: 8081
```

注意：属性值大小写敏感

1. 值的写法
   a) 字面量（K:空格v）：普通的值（数字，字符串，布尔）
   i. 字符串默认不用加上单引号或者双引号
   ii. “”：双引号；不会转义字符串里面的特殊字符，特殊字符会作为本身想表示的意思，例如：
   name: “zhangsan \n lisi” 输出 zhangsan 换行 lisi
   iii. ‘’：单引号；会转移特殊字符，特殊字符最终只是一个普通的字符串数据
   name: ‘zhangsan \n lisi’ 输出 zhangsan \n lisi
   b) 对象/Map（属性和值）（键值对）（k: v）语法示例如下：

```
friends:
  lastName: zhangsan
  age: 20
123
```

ii. 行内写法:

```
friends: {lastName: zhangsan,age: 18}
1
```

c) 数组（List，Set）（用-值表示数组中的一个元素）语法示例如下：

```
pets:
  -	cat
  -	dog
  -	pig

12345
```

ii. 行内写法：

```
pets: [cat,dog,pig]
1
```

1. 配置文件值注入给JavaBean示例如下：
   注：我们可以利用Maven导入配置文件处理器，方便在配置文件中提示我们，要导入的Maven坐标如下:

```
<!-- 导入配置文件处理器,配置文件绑定后就会有提示 -->
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-configuration-processor</artifactId>
   <optional>true</optional>
</dependency>
123456
```

创建一个Person类，如下代码片段：（需要注意的是，需要自动注入到Spring容器中的类，要跟SpringBoot主配置类是同包下，或是主配置类所处包的子包下）

```
package test.spring.boot.demo;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;
import java.util.Date;
import java.util.*;
//将配置文件中的Person属性赋的值映射到该组件中
//@ConfigurationProperties：告诉SpringBoot将本类的所有属性和配置文件中相应的属性进行相互关联映射赋值
//该注解中的prefix属性告知绑定配置文件中的哪个属性
//当前的Person组件必须要加载到Spring容器中，才能使用@ConfigurationProperties提供的功能
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
	private Dog dog;
	//…省略getter/setter以及toString()方法
}
123456789101112131415161718192021
```

创建一个Dog类，示例如下：

```
package test.spring.boot.demo;

public class Dog {
    private String name;
    private Integer age;

    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    //...省略getter/setter方法的生成
}
12345678910111213141516
```

在src/main/resources/目录中建立application.yml，配置代码如下：

```
person:
  lastName: zhangsan
  age: 18
  boss: false
  birth: 2017/12/12
  maps: {k1: v1,k2: 12}
  lists:
    - lisi
    - zhaoliu
  dog:
  	name: luck
  	age: 3
123456789101112
```

最后进入在src/test/java/中的SpringBoot单元测试中，编写如下代码，进行测试，看在application.yml中配置的person属性是否绑定映射到Person对象中：

```
package test.spring.boot.demo;
import org.springframework.context.ApplicationContext;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

/**
 * SpringBoot单元测试
 * 可以在测试期间很方便的类似编码一样进行自动注入到容器
 */
@RunWith(SpringRunner.class)
@SpringBootTest
public class DemoApplicationTests {
	@Autowired
	Person person;

	@Test
	public void contextLoads() {
		System.out.print(person.toString());
	}
}
1234567891011121314151617181920212223
```

最后控制太输出如下效果：
![在这里插入图片描述](https://img-blog.csdn.net/20180920162034110?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2MzQ3NDYz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
说明yml值注入到JavaBean成功