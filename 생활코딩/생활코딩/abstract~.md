### abstract

-	**abstract 된 클래스나 메서드**는 반드시 **상속을 강제**하고 **오버라이딩**을 통하여 사용 가능하다.
-	부모클래스의 메서드는 시그니처만 정의해 놓고 실제 동작부분은 상속받은 하위클래스의 책임으로 위임하고있다.
-	abstract 메소드는 내용이 {} 없는 메소드여야 한다
-	abstract 클래스 안에는 abstract로 선언된 메소드, 선언 안된 메소드 둘다 선언 가능하다
-	abstract 로 선언된 메소드는 상속된 메서드에 반드시 지정해야한다.

```java
package org.opentutorials.javatutorials.abstractclass.example1;
abstract class A{

  public abstract int b();

    //본체가 있는 메소드는 abstract 키워드를 가질 수 없다.
    //public abstract int b(){};

    public abstract int c();

    public void d() { System.out.println("abstract선언 안된 d_메소드");}
    //추상 클래스 내에는 추상 메소드가 아닌 메소드가 존재 할 수 있다.

}

class B extends A {
  public int b () {
		return 1;
	}

public int c() {
	// TODO Auto-generated method stub
	return 0;
}

  //abstract 메소드를 무조건 다 선언해주어야한다
}

public class Test {
    public static void main(String[] args) {
       // A obj = new A();
    	B obj2 = new B();
    	int num = obj2.b();
    }
}

```

---

#### 추상 클래스를 사용하는 용도

-	기능의 공통된 부분은 부모클래스에 정의하고 용도에 따라서 달라지는 부분은 하위클래스에 정의함으로써 사용자가 정의하도록 규약한다.

```java
package org.opentutorials.javatutorials.abstractclass.example3;
abstract class Calculator{
    int left, right;
    public void setOprands(int left, int right){
        this.left = left;
        this.right = right;
    }
    public abstract void sum();  
    public abstract void avg();
    public void run(){
        sum();
        avg();
    }
}
class CalculatorDecoPlus extends Calculator {
    public void sum(){
        System.out.println("+ sum :"+(this.left+this.right));
    }
    public void avg(){
        System.out.println("+ avg :"+(this.left+this.right)/2);
    }
}
class CalculatorDecoMinus extends Calculator {
    public void sum(){
        System.out.println("- sum :"+(this.left+this.right));
    }
    public void avg(){
        System.out.println("- avg :"+(this.left+this.right)/2);
    }
}
public class CalculatorDemo {
    public static void main(String[] args) {
        CalculatorDecoPlus c1 = new CalculatorDecoPlus();
        c1.setOprands(10, 20);
        c1.run();

        CalculatorDecoMinus c2 = new CalculatorDecoMinus();
        c2.setOprands(10, 20);
        c2.run();
    }

}

```
