#### toString

-	어떤 클래스 혹은 객체가 있을 때 객체화 시킨다

```java
package org.opentutorials.javatutorials.progenitor;

class Calculator{
    int left, right;

    public void setOprands(int left, int right){
        this.left = left;
        this.right = right;
    }
    public void sum(){
        System.out.println(this.left+this.right);
    }

    public void avg(){
        System.out.println((this.left+this.right)/2);
    }
}

public class CalculatorDemo {

    public static void main(String[] args) {

        Calculator c1 = new Calculator();
        c1.setOprands(10, 20);
        System.out.println(c1);
    }
}

```

```
org.opentutorials.javatutorials.progenitor.Calculator@000nbvxbx
-------------------------------------패키지-----클래스----@주소값
```
