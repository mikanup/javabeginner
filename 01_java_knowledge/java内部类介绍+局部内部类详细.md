# 20230518

### 内部类（整体分类+局部内部类）

Java内部类是指在一个类内部定义的类。内部类可以访问外部类的成员，包括私有成员，而外部类也可以访问内部类的成员。它们之间形成了一种特殊的关系，内部类提供了更好的封装和组织代码的方式。

Java内部类的主要类型有以下几种：

1. 成员内部类（Member Inner Class）：它是定义在外部类的成员位置上的类。成员内部类可以访问外部类的所有成员，包括私有成员。创建成员内部类的实例需要先创建外部类的实例，例如：**`OuterClass.InnerClass innerObj = outerObj.new InnerClass();`**
2. 静态内部类（Static Inner Class）：它是使用static关键字修饰的内部类。静态内部类与外部类的实例无关，可以直接通过外部类访问，而不需要创建外部类的实例。例如：**`OuterClass.StaticInnerClass innerObj = new OuterClass.StaticInnerClass();`**
3. **局部内部类（Local Inner Class）**：它是定义在方法内部的类。局部内部类只能在方法内部使用，并且对外部世界是隐藏的。局部内部类可以访问方法的局部变量，但是这些局部变量必须声明为final。
4. **匿名内部类（Anonymous Inner Class）**：它是没有名字的内部类，用于简化代码编写。匿名内部类通常作为接口的实现或者抽象类的子类，可以在创建对象的同时定义类的结构和行为。

四种分类

定义在外部类局部位置上（比如方法内）：

1. 局部内部类（有类名）
2. 匿名内部类（没有类名）

定义在外部类的成员位置上：

1.成员内部类（没用statci修饰）

2.静态内部类（使用static修饰）

使用内部类的好处包括：

- 封装性：内部类可以访问外部类的私有成员，从而实现更好的封装性和数据隐藏。
- 组织性：内部类可以将相关的类组织在一起，使代码更加清晰和可读。
- 回调功能：内部类常用于实现回调函数，通过实现接口或继承抽象类来定义回调方法。

需要注意的是，内部类与外部类之间存在一定的耦合性，过多或不合理地使用内部类可能会导致代码的可维护性和可读性下降。因此，在使用内部类时应该根据实际需求和设计原则进行合理的选择和使用。

```java
package com.cs.practice.inner_;

public class InnerClass01 {
    public static void main(String[] args) {
        Outer02 outer02 = new Outer02();
        outer02.m1();
        // 确认outer02的hashcode
        // com.cs.practice.inner_.Outer02@4f023edb
        System.out.println(outer02);
    }
}

class Outer{

    // 属性
    private int n1 = 100;

    // 构造方法
    public Outer(int n1) {
        this.n1 = n1;
    }

    // 方法
    public void m1(){
        System.out.println("m1()");
    }

    // 代码块
    {
        System.out.println("code block");
    }

    // 内部类
    class Inner{

    }
}

// 外部类
class Outer02{

    // 私有属性
    private int n1 = 100;
    private void m2(){
        System.out.println("m2 outer");
    }

    // 方法
    public void m1(){

        // 局部内部类(本质仍旧是类)
        // 定义在方法或代码块中
        // 不能添加访问修饰符，但是可以使用final修饰
        // 作用域：仅仅在定义它的方法或代码块中
        class Inner02{
            private int n1 = 200;
            // 可以直接访问外部类的所有成员，包含私有的
            public void f1(){
                // 如果外部类和局部内部类的成员重名时，默认遵循就近原则
                // 所以下面访问的是自己类的n1 200
                System.out.println("n1 = " + n1);
                // 如果要访问外部类的成员，就要使用外部类名.this.成员
                // Outer02.this本质是外部类的对象，就是哪个对象调用了m1，Outer02.this就是哪个对象
                System.out.println("Outer02 n1 = " + Outer02.this.n1);
                // Outer02.this hashcode= com.cs.practice.inner_.Outer02@4f023edb
                System.out.println("Outer02.this hashcode= " + Outer02.this);
                m2();
            }
        }

        // 如果Inner02没有被final修饰，就可以被其他局部内部类继承
        // 如果不想被继承，就必须加上final
        class Inner03 extends Inner02{

        }

        // 外部类在方法中，可以创建Inner02对象，然后调用方法
        Inner02 inner02 = new Inner02();
        inner02.f1();
    }
}
```