# 1 简介

反射机制就是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为java语言的反射机制。

用一句话总结就是反射机制可以在运行时知道任意一个类的属性和方法。

Java 反射机制拥有如下优点和缺点。

1. 优点：可以实现动态创建对象和编译，体现出很大的灵活性，特别是在J2EE的开发中它的灵活性就表现的十分明显。比如，一个大型的软件，不可能一次就把把它设计的很完美，当这个程序编译后，发布了，当发现需要更新某些功能时，我们不可能要用户把以前的卸载，再重新安装新的版本，假如这样的话，这个软件肯定是没有多少人用的。采用静态的话，需要把整个程序重新编译一次才可以实现功能的更新，而采用反射机制的话，它就可以不用卸载，只需要在运行时才动态的创建和编译，就可以实现该功能。
2. 缺点：对性能有影响。

测试代码，后面基于此做讲解。

基类 Base.java

```java
package com.yjcocoa.reflection;

public class Base {

    public String superField;

    public void superMethod() {

    }

}
```

子类 User.java

```java
package com.yjcocoa.reflection;

public class User extends Base {

    public static String language = "JAVA";
    private String name;
    public String qq;

    public static User user(String name, String qq) {
        return new User(name, qq);
    }

    public User() {
    }

    public User(String name, String qq) {
        this.name = name;
        this.qq = qq;
    }

    private User(String name) { // 私有构造器
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", qq='" + qq + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    private void setName(String name) { // 私有 setter
        this.name = name;
    }

}
```

# 2 Class

类是 java.lang.Class 类的实例对象，通过反射机制我们可以使用不同的获取 class。

```java
Class class1 = User.class;
System.out.println(class1.getName());
//第二种
User demo2 = new User();
Class c2 = demo2.getClass();
System.out.println(c2.getName());
//第三种
Class class3 = Class.forName("com.yjcocoa.reflection.User");
System.out.println(class3.getName());
```

输出

```
com.yjcocoa.reflection.User
com.yjcocoa.reflection.User
com.yjcocoa.reflection.User
```

# 3 Field

字段的反射对象是 java.lang.reflect.Field。

```java
Class uc = User.class;
// 获取所有字段
for (Field field : uc.getDeclaredFields()) {
    System.out.println(field);
}
System.out.println();

// 获取所有 public 字段，包含父类字段
for (Field field : uc.getFields()) {
    System.out.println(field);
}
System.out.println();

// private 字段 name 赋值
Field field = uc.getDeclaredField("name");
field.setAccessible(true);
User user = new User();
field.set(user, "YJ"); // set 方法
System.out.println(user.getName() + "；" + field.get(user)); // get 方法
```

输出

```
public static java.lang.String com.yjcocoa.reflection.User.language
private java.lang.String com.yjcocoa.reflection.User.name
public java.lang.String com.yjcocoa.reflection.User.qq

public static java.lang.String com.yjcocoa.reflection.User.language
public java.lang.String com.yjcocoa.reflection.User.qq
public java.lang.String com.yjcocoa.reflection.Base.superField

YJ；YJ
```

# 4 Constructor

构造函数的反射对象是 java.lang.reflect.Constructor。

```java
Class uc = User.class;
// 获取所有构造器
for (Constructor constructor : uc.getDeclaredConstructors()) {
    System.out.println(constructor);
}
System.out.println();
// 获取所有public 构造器
for (Constructor constructor : uc.getConstructors()) {
    System.out.println(constructor);
}
System.out.println();
// private 构造器 初始化
Constructor<User> constructor = uc.getDeclaredConstructor(String.class);
constructor.setAccessible(true); // private 需要开启允许访问
User user = constructor.newInstance("YJ");
System.out.println(user);
```

输出

```java
public com.yjcocoa.reflection.User(java.lang.String,java.lang.String)
public com.yjcocoa.reflection.User()
private com.yjcocoa.reflection.User(java.lang.String)

public com.yjcocoa.reflection.User(java.lang.String,java.lang.String)
public com.yjcocoa.reflection.User()

User{name='YJ', qq='null'}
```

# 5 Method

方法的反射对象是 java.lang.reflect.Method。

```java
Class uc = User.class;
// 获取所有方法
for (Method method : uc.getDeclaredMethods()) {
    System.out.println(method);
}
System.out.println();
// 获取所有 public 方法，包含父类方法
for (Method method : uc.getMethods()) {
    System.out.println(method);
}
System.out.println();

// private 方法调用
Method method = uc.getDeclaredMethod("setName", String.class);
method.setAccessible(true);
User user = new User();
method.invoke(user, "YJ");
System.out.println(user);
```

输出

```java
public java.lang.String com.yjcocoa.reflection.User.toString()
public java.lang.String com.yjcocoa.reflection.User.getName()
private void com.yjcocoa.reflection.User.setName(java.lang.String)
public static com.yjcocoa.reflection.User com.yjcocoa.reflection.User.user(java.lang.String,java.lang.String)

public java.lang.String com.yjcocoa.reflection.User.toString()
public java.lang.String com.yjcocoa.reflection.User.getName()
public static com.yjcocoa.reflection.User com.yjcocoa.reflection.User.user(java.lang.String,java.lang.String)
public void com.yjcocoa.reflection.Base.superMethod()
public final void java.lang.Object.wait(long,int) throws java.lang.InterruptedException
public final void java.lang.Object.wait() throws java.lang.InterruptedException
public final native void java.lang.Object.wait(long) throws java.lang.InterruptedException
public boolean java.lang.Object.equals(java.lang.Object)
public native int java.lang.Object.hashCode()
public final native java.lang.Class java.lang.Object.getClass()
public final native void java.lang.Object.notify()
public final native void java.lang.Object.notifyAll()

User{name='YJ', qq='null'}
```

&#160;

----------

# Appendix

## Related Documentation

[JAVA反射与注解](https://www.daidingkang.cc/2017/07/18/java-reflection-annotations/)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-12-15 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974