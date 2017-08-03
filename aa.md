### 자바 객체를 생성하는 방법

#### 1. new 연산자를 이용

##### 1-1. 클래스

```java
Scanner sc = new Scanner(System.in);
```

##### 1-2 인터페이스★★★

```java
부모인터페이스 자료형 인터페이스객체명 = new 자식클래스명();
Set set = new HashSet();
```

```
Interface<String> I = new Interface<String>(); => 불가능
자기자신의 객체를 생성 할 수 없다!
Set set = new Set(); 은 불가능!!!!
자식클래스를 통한 new 연산자를 이용한 객체 생성 가능
예외) Set set = new HashSet();
부모인터페이스 자료형 인터페이스객체명 = new 자식클래스명();

왼쪽 자료형과 오른쪽 자료형이 다를 경우 오른쪽이 자식클래스이고 왼쪽이 부모 클래스이다.
```

#### 2. 메서드의 매개변수를 통해서 객체 얻어오기

-	call By Refefence(참조에 의한 전달방법)
-	매개변수를 전달(객체(주소)를 전달->call By Reference(참조에 의한 전달방법)

```java
package j0609;

//caller method->work method
//매개변수를 전달(객체(주소)를 전달->call By Reference(참조에 의한 전달방법)
//main(원본)---->change_color(원본)=>원본값은 변경
class RGBColor{
    int r,g,b;//0,0,0
    RGBColor(int r,int g,int b){
        this.r=r;//color.r=-1;
        this.g=g;//color.g=-1;
        this.b=b;//color.b=-1
    }
}

public class CallByRef {
    public static void main(String[] args) {
        RGBColor color=new RGBColor(-1,-1,-1);
        System.out.println("color=>"+color);
        System.out.println("before:red="+color.r+",green="+color.g+",blue="+color.b);
        change_color(color);
        System.out.println("after:red="+color.r+",green="+color.g+",blue="+color.b);
    }

    //색깔을 변경시켜주는 메서드->매개변수 O ,반환값 X
    static void change_color(RGBColor color1) { //(* color1)
        System.out.println("color1=>"+color1);
        color1.r+=10;//r=r+10
        color1.g+=50;//g=g+50;
        color1.b+=100;//b=b+100
        System.out.println
            ("메서드내부의 r="+color1.r+",g="+color1.g+",b="+color1.b);
    }
}
```

#### 3. 메서드의 반환형을 통해서 객체를 얻어오는 방법

-	iterator<String> ih2=h.values().iterator();
-	Runtime 객체명 = Runtime.getRuntime();
-	현재 실행중인 자바프로그램과 연관된 실행객체를 반환시켜주는 메서드
-	Returns the runtime object associated with the current Java application.

```java
public class GCCollector {
    public static void main(String[] args) {
        // TODO Auto-generated method stub
       //Runtime클래스->API문서->생성자 먼저->생성자X->new연산자 사용X
        byte test[]=new byte[1024];
        Runtime r=Runtime.getRuntime();
        System.out.println("사용메모리양:"+(r.totalMemory()-r.freeMemory())/1024+"K");
        test=null;//주소값이 지워지게되면 test데이터들은 test객체가 주소를 잃어서 데이터들이 주소값을 잃어 붕떠버리게된다.
         // 즉 쓸모없는 데이터 생성된다.
        System.gc();//garbage collector 수동으로 메모리 제거 제거->System.exit(0)//프로그램 종료
        System.out.println("사용메모리양:"+(r.totalMemory()-r.freeMemory())/1024+"K");
    }
}
```
