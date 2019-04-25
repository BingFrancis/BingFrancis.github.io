# Spring的XML文件配置

在src文件夹下直接创建配置文件bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   <!--我们可以将这个bean理解为我们的javaBean，其中两个property标签即表示给IntrduceDemo类中的name和age属性赋值！-->
    <bean id="IntrduceDemo" class="com.test1.IntrduceDemo">
       <property name="name" value="李佳奇"/>
       <property name="age" value="2"/>
    </bean>
</beans>
```

### 加载配置文件

```java
ApplicationContext acx= new ClassPathXmlApplicationContext("bean.xml");
```

### 获取类实例

```java
IntrduceDemo idNew=acx.getBean("IntrduceDemo",IntrduceDemo.class);
```

在bean.xml中bean标签中的scope属性设置为singleton,在赋值以后,后面每次实例化的对象都是相同的

与singleton对应的是prototype，如果将scope属性设置为prototype，再运行程序，我们获取到的对象值就为null.

### init-method和destory-method

这两个属性分别定义bean初始化和销毁时自动调用的方法，比如我们在IntrduceDemo  在底部添加:

```java
    //bean初始化时调用的方法
public void init(){
    System.out.println("Bean初始化.....");
}
//bean销毁时调用的方法
public void destroy(){
    System.out.println("Bean销毁.....");
   }
```

可以在xml里面进行如下的配置

```xml
    <bean id="IntrduceDemo" class="com.test1.IntrduceDemo" scope="singleton" init-method="init" destroy-method="destroy">
    </bean>

```

值得注意的是,当我们将bean的scope属性设置为singleton或者默认时，当容器销毁时才会调用destroy-method的方法

## Spring自动装配Bean

在Spring中，可以使用 @Autowired 注解通过setter方法，构造函数或字段自动装配Bean。此外，它可以在一个特定的bean属性自动装配。

#### 注 @Autowired注解是通过匹配数据类型自动装配Bean。

```java
public class Customer {

    @Autowired
    private Person person;
//    public Person getPerson() {
//        return person;
//    }
//
//    public void setPerson(Person person) {
//        this.person = person;
//    }
    @Autowired
    public Customer(Person person){
        this.person = person;
    }
    其他属性的get  setter省略
}

```

可以用@Autowired注解 和 setter   或者是@Autowired注解构造方法的方式来自动装配

**但是要启用@Autowird,就必须注册AutowiredAnnotationBeanPostProcesso**r

方法如下:

```
<bean 
class="org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor"/>
```

#### **依赖检查**

```java
@Autowired(required=false)
```

如果不能找到一个匹配的Bean,则属性值不设定

**Spring可以使用@Qualifier设定其自动装配一个特定的bean,它的意思是Bean有资格自动装配到一个字段**

