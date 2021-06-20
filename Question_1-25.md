**Q1. A parent class has 2 variables final int a = 20; static int b; and child class has 2 variables with same name int a, b;.**
```java
class A{
  final int a = 20;
  static int b;
}
class B extends A{
 int a,b;
}

public class Main{
    public static void main(String[] args) {
        B b = new B();
        b.a = 30;
        b.b = 30;
    }
}

```
* Can I change value of a? 
* What would be the value of b among all instances of child class?

**Ans.**
As per 
1. Inheritance rule or Concrete class rule, variables a,b are different variables than super.a, super.b. So their values can be changed. When I say childObj.b, the closest declaration will be used i.e. non-static b.

**Q2. An interface has a variable int a = 20;**
* Can child have member with same name?
* Can child class modify `a` in its constructor?
* Whether child class member a would become final?

**Ans.**
* As per rules of Inheritance or Concrete class, variable `a` in child class and interface are different. So child class can have variable `a`.
* As per rules of Final, value of `a` can't be changed.
* As per rules of  Interface or Concrete class Rule, `a` is independent and will not become final.

```java

// Online IDE - Code Editor, Compiler, Interpreter
interface inface{
    int a = 20;
}
class B  implements inface{}

class C implements inface{
 int a = 40; //allowed
}

public class Main{
    public static void main(String[] args) {
        B b = new B();
        C c = new C();
        System.out.println(b.a); //20
        System.out.println(c.a); //40

        //b.a = 30; //Error
        c.a = 30;
        System.out.println(c.a); //30
    }
}

```
**Q3. Can we create another instance of same class in its constructor?**
**Ans**. Yes. But it may fall in loop and can cause JVM Stack OutOfMemory error.

**Q4. Whether a class/ interface can extend itself?**
**Ans**. As per Common Rules for Concrete class and Interface, No.

**Q5. Whether an interface and abstract class can have main()?**
**Ans**. As per Abstract class rule, it can have main(). As per Interface rule, it can have `public void main()`, static abstract methods are not allowed . (But from Java 8 since interface can have static methods, it is allowed)

**Q6. Whether an abstract class can extend**
* abstract class
* concrete class
* Interface

**Ans.**
* As per Abstract class rule, yes. 
* As per Common Rules for Abstract class and Interface, implementation of abstract methods of parent is optional.

**Q7. Whether a class can bypass implementation of abstract methods?**
**Ans**. Yes, if it is an abstract class. (As per Common Rules for Abstract class and Interface)

**Q8. Why a parent class/interface can refer child class?**
**Ans**. To support polymorphism. You can use single entity as entry point to accept all its type.

**Q9. What if a child class extend a class and implement an interface which have method with same signature?**
**Ans**. 

* As per Inheritance rule, child class need to implement method declared in Interface.
* As per rule of Concrete class, child class can have method with same signature.

```java
interface inface{
    void method(int a);
}
class A{
  void method(int a){
      System.out.println("Class A");
  }
}
class B extends A implements inface{
 //overrided method from interface. Hence, public
 public void method(int a){
      System.out.println("Class B");
  }
}

public class Main{
    public static void main(String[] args) {
        A a = new A();
        A b = new B();
        a.method(1); //Class A
        b.method(1); //Class B
    }
}
```

**Q10. What if an abstract class and interface has same method signature and a concrete class extends both?**
**Ans**. 
* As per rule of Inheritance and Concrete class, child class need to implement abstract method

Since both are having same signature, it'll not create ambiguity. Consider it as 2 people are asking you to do the same thing.

```java
interface inface{
    void method(int a);
}
abstract class A{
  abstract void method(int a);
}
class B extends A implements inface{
 public void method(int a){
      System.out.println("Class B");
  }
}

public class Main{
    public static void main(String[] args) {
        A b = new B();
        b.method(1); //Class B
    }
}
```

**Q11. Can we re-declare field members in child class with other modifiers?**
**Ans**. As per Inheritance rule or Concrete class rule, yes.

**Q12. If a child class override method() where method() of parent class throws an IOException but child class’s method() doesn’t, do we need to catch it or throw IOException for further level if we create object of child class and call method()?**
**Ans**. As per Inheritance Rule, Yes.

If it's a handled exception then it is not needed to handle
```java

abstract class A{
  abstract void method (int a) throws RuntimeException;
}
class B extends A{
 public void method(int a){
      System.out.println("Class B");
  }
}

public class Main{
    public static void main(String[] args) {
        A b = new B();
        b.method(1); //Class B
    }
}
```
But unhandeled errors, need to be handled gracefully;
```java
import java.io.IOException;

abstract class A{
  abstract void method (int a) throws IOException;
}
class B extends A{
 public void method(int a){
      System.out.println("Class B");
  }
}

public class Main{
    public static void main(String[] args) {
        A b = new B();
        b.method(1); //Error
    }
}
```


**Q13. Why child class reference can’t refer to object of parent class?**
**Ans**. Parent-Child relationship defines “type of” relationship. It means child is type of parent. It doesn’t mean child contains parent. 

```
Animal a = new Fox();
Animal b = new Fish();
b = a;
```

If, suppose, child class can refer parent class object then

```
Animal a = new Fox();
Animal b = new Fish();
Fish f = a;
```
will be correct which is against of inheritance concept.

**Q14. Can an abstract class be final?**
**Ans**. As per Abstract class Rule and Final rule, No.

**Q15. Why a class cannot be static? But a nested class can be static.**
**Ans**. Any entity can be static only if it can be used in static way. Since a concrete class can’t have any value because it is just a byte code so it can’t be used in static way. Hence, a concrete class can’t be static.
Nested class follows the rule of member field. So it can be static.

**Q16. Can a non-static nested class have static members? Explain with reason.**
**Ans**. As per static rule, No.

```java

class A{
  class B{ //aka inner class
      int a = 20;
      //static int e = 40; //Error
  }
  static class C{
      static int a = 30;
      int d = 40;
  }
}

public class Main{
    public static void main(String[] args) {
        A a = new A();
        System.out.println(a.new B().a); //20
        System.out.println(A.C.a); //30
        System.out.println(new A.C().d); //40
    }
}
```
**Q17. Can a nested class be abstract or static abstract?**
**Ans**. As per Nested class Rule,yes.

**Q18. Can any concrete class have abstract methods?**
**Ans**. As per Concrete class Rule, no.

**Q19. Can a non-static abstract class have static members? If it is nested?**
**Ans**. Since nested class (whether it is abstract or not) follows rules of member field. So as per Static rule a static nested class can have static or non-static (local variable of static parents are static by default) members. But a non-static nested class can't have static members.

**Q20. Can an anonymous class have abstract methods?**
**Ans**. As per Concrete class Rule, No.

**Q21. Can we change interface field value in method/constructor of a class implementing that interface.**
**Ans**. As per Concrete class Rule, and Interface Rule, No.

**Q22.Can an abstract class has static final, abstract static final, abstract static, and abstract final methods?**
**Ans**. 
* *static final* – As per Abstract class rule, yes.
* *abstract static final* – As per Abstract Rule, Final Rule, and Static rule, No.
* *abstract static* - As per Abstract Rule, and Static rule, No.
* *abstract final* - As per Abstract Rule, and Final Rule, No.


**Q23. Why Interface variables are static final by default? But abstract class variables are not?**
**Ans**. Since member fields never be overridden and interface can't be instantiated so interface variables are static final. As per abstract class rule, abstract class members are simple by default.

**Q24. Can I declare a variable in interface with final and static keyword?**
**Ans**. Yes. However it is not required.

**Q25. Can I declare a method in interface with final or static keyword?**
**Ans** As per Interface rule, Final Rule, and Static rule, No.


**Q25. Why the member fields of an Interface are final but not the methods?**
**Ans** Interfaces are ment  to be implemented by other classes which are not necessarily relevant. Hence, they may change the value of a variable. So if a class change the value of a field member, It'll impact the other classes. Whereas, method's declaration is local to particular class.
