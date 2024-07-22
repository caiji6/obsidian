### 常见属性
1. **id**
   - Bean的唯一标识符。
```
<bean id="myBean" class="com.example.MyBean"/>
```

1. **name**
   - Bean的别名，可以为Bean定义一个或多个别名。
```
<bean id="myBean" name="alias1,alias2" class="com.example.MyBean"/>
```

1. **class**
   - Bean的全限定类名。
```
<bean id="myBean" class="com.example.MyBean"/>
```

1. **scope**
   - Bean的作用域，常见值包括singleton（默认）、prototype、request、session、globalSession、application。
```
<bean id="myBean" class="com.example.MyBean" scope="prototype"/>
```

1. **init-method**
   - Bean初始化时调用的方法。
```
<bean id="myBean" class="com.example.MyBean" init-method="init"/>
```

1. **destroy-method**
   - Bean销毁时调用的方法。
```
<bean id="myBean" class="com.example.MyBean" destroy-method="cleanup"/>
```

1. **factory-method**
   - 用于创建Bean实例的静态工厂方法。
```
<bean id="myBean" class="com.example.MyBeanFactory" factory-method="createInstance"/>
```

1. **factory-bean**
   - 用于创建Bean实例的工厂Bean的名称。
```
<bean id="myFactory" class="com.example.MyFactory"/>
<bean id="myBean" factory-bean="myFactory" factory-method="createInstance"/>
```
### 依赖注入相关属性

1. **constructor-arg**
   - 用于构造函数注入。
```
<bean id="myBean" class="com.example.MyBean">
    <constructor-arg value="someValue"/>
    <constructor-arg ref="anotherBean"/>
</bean>
```

1. **property**
   - 用于Setter方法注入。
```
<bean id="myBean" class="com.example.MyBean">
    <property name="propertyName" value="someValue"/>
    <property name="anotherProperty" ref="anotherBean"/>
</bean>
```
### 其他常用属性

1. **autowire**
   - 自动装配模式，常见值包括no（默认）、byName、byType、constructor、autodetect。
```
<bean id="myBean" class="com.example.MyBean" autowire="byName"/>
```

1. **depends-on**
   - 指定Bean的依赖关系，即在初始化当前Bean之前需要先初始化的Bean。
```
<bean id="myBean" class="com.example.MyBean" depends-on="anotherBean"/>
```

1. **lazy-init**
   - 是否延迟初始化，默认值为false。
```
<bean id="myBean" class="com.example.MyBean" lazy-init="true"/>
```

1. **primary**
   - 当自动装配时，如果有多个候选Bean，可以将某个Bean标记为主要候选者。
```
<bean id="myBean" class="com.example.MyBean" primary="true"/>
```
### 示例配置
```
<bean id="myBean" class="com.example.MyBean" scope="singleton" init-method="init" destroy-method="cleanup" lazy-init="true" primary="true" autowire="byType">
    <constructor-arg value="someValue"/>
    <constructor-arg ref="anotherBean"/>
    <property name="propertyName" value="someValue"/>
    <property name="anotherProperty" ref="anotherBean"/>
</bean>
```
# 
