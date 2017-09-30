Java 在 jdk 1.5 增加了注解，为我们的开发带来了更多的可能性。如 spring 中的注解，极大的简便了我们的开发，并提高了代码的可维护性。注解源码主要在 java.lang.annotation 包中，内部结构如下所示。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017093001.png)

# 1 API

下面介绍相关 api，主要介绍一些常用的api，其他 api 可查看官方文档。

## 1.1 Interface

Annotation 是所有注解类型的公共接口，也就是我们任何一个注解都是隐式实现了这个接口。在开发过程中，我们没有继承这个接口，而是使用 @interface 声明一个注解。

```java
public interface Annotation {
    boolean equals(Object obj);// 比较      
    int hashCode();// hash 值    
    String toString();// 获取注解的字符串表示    
    Class<? extends Annotation> annotationType();// 获取注解的类型
}
```

## 1.2 Enum

### 1.2.1 ElementType

ElementType 在 @Target 中使用，主要是描述一个注解的使用区域。

```java
public enum ElementType {
    TYPE, // 类，接口，枚举
    FIELD, // 字段或枚举的常量
    METHOD, // 方法
    PARAMETER, // 方法参数
    CONSTRUCTOR, // 构造函数
    LOCAL_VARIABLE, // 局部变量
    ANNOTATION_TYPE, // 注解类型
    PACKAGE, // 包
    TYPE_PARAMETER, // Type parameter declaration
    TYPE_USE // Use of a type
}
```
ElemenType 可以多个同时使用，在后面会为大家代码演示。

###1.2.2 RetentionPolicy

RetentionPolicy 在 @Retention 中使用，主要是描述一个注解的保留阶段。

```java
public enum RetentionPolicy {
    SOURCE,// 注解只存在与源码中，编译器无法识别
    CLASS,// 编译器可识别注解，运行时无法获取
    RUNTIME// 注解在运行时可以通过反射机制获取
}
```

多数情况，我们都是设置为RUNTIME。但是有的时候会有特殊需求，我们会设置其他级别，如 `@Override` 。

## 1.3 Annotation Type

## 1.3.1 Documented	

Documented 表明该注解可通过工具生成javadoc文档。

## 1.3.2 Inherited	

Documented 表明子类可以获取父类的注解。如父类中使用了该类型的注解，我们在子类通过java 反射机制时，也可获取该注解。

## 1.3.3 Native	

Native 定义的注解可以在常量值使用。

## 1.3.4 Repeatable	

Repeatable 表明注解是可以重复的。如我们想在一个注解中使用另一个注解则需要使用 Repeatable。

## 1.3.5 Retention	

Retention 指明注解的保留阶段，如 `@Retention(RetentionPolicy.SOURCE)` 表示只存在于源代码中。

## 1.3.6 Target

Target 表明注解的使用区域。如 `@Target({ElementType.FIELD,  ElementType.METHOD})` 定义该注解只能用于字段和方法。

# 2 实战

我们定义一个简单的注解，其中包含所有可使用的区域。

```java
@Target({ElementType.TYPE, ElementType.CONSTRUCTOR, ElementType.METHOD, ElementType.PARAMETER, ElementType.FIELD,
        ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
public @interface AnnotationT {
    String value() default "";
}
```

注解的使用也很简单，这里我们定义了两个类，主要是为了测试注解的继承。

```java
public class UserAnnotationBase {

    @AnnotationT(value = "超类成员变量注解")
    public String str1;

}

@AnnotationT(value = "类注解")
public class UserAnnotation extends UserAnnotationBase {

    @AnnotationT(value = "成员变量注解")
    public String str;

    @AnnotationT(value = "构造方法注解")
    public UserAnnotation() {
    }

    @AnnotationT(value = "类方法注解")
    public void test(String str1, @AnnotationT(value = "方法参数注解") String str) {
    }

}
```

注解解析使用java反射机制，这里只展示解析功能，相关应用场景可自行脑补。

```java
@Test
public void test() throws Exception {
    this.getBean(UserAnnotation.class);

}

public <T> T getBean(Class<T> requiredType) throws Exception {
    // 类注解
    for (Annotation annotaion : requiredType.getDeclaredAnnotations()) {
        System.out.println(annotaion + " / 类：" + requiredType);
    }
    // 类成员变量注解
    for (Field field : requiredType.getFields()) {
        if (field.isAnnotationPresent(AnnotationT.class)) {
            AnnotationT a = (AnnotationT) field.getAnnotation(AnnotationT.class);
            System.out.println(a + " / 修饰符：" + Modifier.toString(field.getModifiers()) + "；类型：" + field.getType() +
                    "名称：" + field.getName());
        }
    }
    // 构造方法注解
    for (Constructor method : requiredType.getConstructors()) {
        if (method.isAnnotationPresent(AnnotationT.class)) {
            AnnotationT a = (AnnotationT) method.getAnnotation(AnnotationT.class);
            System.out.println(a + " / " + method.getName());
        }
    }
    // 类方法注解
    for (Method method : requiredType.getDeclaredMethods()) {
        if (method.isAnnotationPresent(AnnotationT.class)) {
            AnnotationT a = (AnnotationT) method.getAnnotation(AnnotationT.class);
            System.out.println(a + " / 方法名称：" + method.getName());
            int index = 0;
            Class<?>[] parameterTypes = method.getParameterTypes();
            for (Annotation[] tt : method.getParameterAnnotations()) {
                for (Annotation t : tt) {
                    System.out.println(t + " / 参数" + index + "；类型" + parameterTypes[index]);
                }
                index++;
            }
        }
    }
    return null;
}
```

这里的解析测试使用了 junit，解析结果如下所示。

```java
@annotation.AnnotationT(value=类注解) / 类：class annotation.UserAnnotation
@annotation.AnnotationT(value=成员变量注解) / 修饰符：public；类型：class java.lang.String名称：str
@annotation.AnnotationT(value=超类成员变量注解) / 修饰符：public；类型：class java.lang.String名称：str1
@annotation.AnnotationT(value=构造方法注解) / annotation.UserAnnotation
@annotation.AnnotationT(value=类方法注解) / 方法名称：test
@annotation.AnnotationT(value=方法参数注解) / 参数1；类型class java.lang.String
```

&#160;

----------

# Appendix


## Sample Code

[Java](https://github.com/937447974/Java)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-09-30 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974