### 17일차(쓰레드의 동기화 및 자바의 컬렉션개요및 작성법)

#### 자동 boxing (자바의 기본자료형-->자바의 객체형으로 변환)

```java
printDouble(143.67);//double d=143.67
  static void printDouble(Double obj2) { //123.45->toString()=>"123.45"
    System.out.println(obj2.toString());
}
```

#### 자동 unboxing (자바의 객체형(Wrapper)->기본자료형으로 변환 (계산할때))

```java
System.out.println("자동 unboxing");
//unboxing->자바의 객체형(Wrapper)->기본자료형으로 변환
//int obj=10;
Integer obj=new Integer(10);//Double obj=new Double(23.5);
//int sum=obj.intValue()+20;  //객체형->기본자료형+기본자료형->계산X
//Integer->int으로 변환
int sum=obj+20;
System.out.println("sum=>"+sum);
```

---

#### Singleton패턴 작성 순서 ★★★

```java
package j0619;
//클래스 내부에서 특정객체를 한개만 생성->대여->반납
public class Singleton {

    //1.객체를 생성->한개만 생성(공유할 수 있도록->정적멤버변수)
    //형식)private static 클래스명 객체명=null;
    private static Singleton instance=null;//선언

    //2.객체를 생성->생성자를 호출->형식) private 생성자(){}
    private Singleton() {
        System.out.println("인스턴스(=객체)생성");
    }
    //3.공유객체를 다른 모든 사람들에게 공유->정적메서드
    //형식)public static 클래스의 자료형 정적메서드명() 선언해서 작성
    public static Singleton getInstance() {
        if(instance==null) { //아직 안만들어져 있는 상태
            //만들고자하는 객체를 공유객체로 등록->synchronized(클래스명.class){}
            synchronized(Singleton.class) {
                if(instance==null) {
                    instance=new Singleton();
                }
            }
        }//outer if
        return instance;
    }
}
```

```java
package j0619;

public class Main {
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        //외부에서 접근이 불가->객체생성 X->생성자 호출 X
        //Singleton ob1=new Singleton();
        //정적 메서드를 이용->객체를 대여
        Singleton ob1=Singleton.getInstance();
        Singleton ob2=Singleton.getInstance();
        Singleton ob3=Singleton.getInstance();

        //객체명은 다르지만 주소값은 하나->하나의 객체임을 증명
        System.out.println("ob1=>"+ob1);
        System.out.println("ob2=>"+ob2);
        System.out.println("ob3=>"+ob3);
        if(ob1==ob2)  //주소값을 비교->같다면, 일반변수라면 내용을 비교
            System.out.println("ob1==ob2");
        else
            System.out.println("ob1!=ob2");
    }
}
```

---

#### 데이터 구조에 따른 컬렉션 인터페이스 분류 ★★★

#### 1. Set 인터페이스

-	데이터가 중복 저장이 안된다

-	저장되는 순서가 없다.

	-	HashSet

#### 2. List 인터페이스

-	중복저장이 가능하다

-	저장순서가 있다(인덱스 번호로 구분)

-	Vector->ArrayList, LinkedList

#### 3. Map 인터페이스

-	표형태로 저장(키,값)

-	값을 저장 시 키를 부여하여 키를이용해서 검색하므로 검색속도가 빠르다.

-	Hashtable, HashMap => 웹에서 세션값을 저장 시 사용한다.

---

#### List - Vector, ArrayList 비교

| 구분                 | Vector                                                  | ArrayList/ LinkedList                                                     |
|----------------------|---------------------------------------------------------|---------------------------------------------------------------------------|
| 객체생성             | Vector v = new Vector                                   | ArrayList list = new ArrayList();<br> LinkedList list = new LinkedList(); |
| 추가                 | v.add("포도") <br> v.addElement(100)                    | list.add("포도")                                                          |
| 크기                 | v.size(); ★// length 아니야!                            | list.size();                                                              |
| 중간 값 추가         | v.insertElementAt(3.14, 1) <br> v.inesertAt(값, 인덱스) | list.add(1, 3.14);                                                        |
| 수정                 | v.setElementAt(수정 객체명, 수정인덱스위치)             | list.set(수정인덱스, 수정객체명)                                          |
| 해당 인덱스 가져오기 | v.elementAt(i)                                          | list.get(i);                                                              |

##### List - Vector, ArrayList 값 출력

1.	Vector

```java
for (int i = 0; i<v.size(); i++) { //// size!!!!!
  System.out.println(v.elementAt(i));
}
```

1.	ArrayList, LinkedList

```java
for(int i = 0; i<list.size(); i++) {
  System.out.println(list.get(i));
}
```

#### 1. List - Vector, ArrayList 비교

| 구분                 | Vector                                                  | ArrayList/ LinkedList                                                     |
|----------------------|---------------------------------------------------------|---------------------------------------------------------------------------|
| 객체생성             | Vector v = new Vector                                   | ArrayList list = new ArrayList();<br> LinkedList list = new LinkedList(); |
| 추가                 | v.add("포도") <br> v.addElement(100)                    | list.add("포도")                                                          |
| 크기                 | v.size(); ★// length 아니야!                            | list.size();                                                              |
| 중간 값 추가         | v.insertElementAt(3.14, 1) <br> v.inesertAt(값, 인덱스) | list.add(1, 3.14);                                                        |
| 수정                 | v.setElementAt(수정 객체명, 수정인덱스위치)             | list.set(수정인덱스, 수정객체명)                                          |
| 해당 인덱스 가져오기 | v.elementAt(i)                                          | list.get(i); //인덱스 값                                                  |

#### 2. List - Vector, ArrayList 값 출력

1.	Vector

```java
for (int i = 0; i<v.size(); i++) { //// size!!!!!
  System.out.println(v.elementAt(i));
}
```

1.	ArrayList, LinkedList

```java
for(int i = 0; i<list.size(); i++) {
  System.out.println(list.get(i));
}
```

#### 3. Hashtable

| Hashtable | | | --- | --- | | 선언 | Hashtable<Integer, String> h1 = new Hashtable<Integer, String>(); | | 추가 | h1.put(1, "one"); h1.put(2, "two"); | | 키값을 이용한 출력 | String str = h1.get(2); |

#### 4. 검색 기능 Enumeration, Iterator

| 구분           | Enumeration                               | Iterator                               |
|----------------|-------------------------------------------|----------------------------------------|
| 생성           | Enumeration\<String\> eu = h1.elements(); | Iterator\<String\> ih = h1.iterator(); |
| 값 있는지 체크 | eu.hasMoreElements() <br> //true, false   | ih.hasNext();                          |
| 값 떠내기      | eu.nextElement()                          | ih.next();                             |

#### 5. 검색기능 Enumeration, Iterator 선언 및 출력

```java
Enumeration<String> eu = h1.elements(); // elements!!!! 다른곳(set) 전부 element임
while(eu.hasMoreElements()){ /////////////이부분 Elements!
  System.out.println(eu.nextElement());
}

Iterator<String> ih = h1.iterator();
while(ih.hasNext()){
  System.out.println(ih.next());
}
```

---

#### 예제 모음 (Vector, ArrayList, LinkedList, Hashtable)

```java
package j0619;

import java.util.*;//Scanner ,java.io.*(입출력)

public class VectorTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        //클래스명<자료형>-->꺼내올때 형변환할 필요가 없다.
        Vector v=new Vector();
         //Vector<String> v=new Vector<String>();//new Vector(1,1); //초기값,증가치
         v.add("테스트");//v.add(new String("테스트"));->0
         //v.add(100);//->v.add(new Integer(100)); 자동 boxing
         v.addElement("임시테스트2");
         v.add("테스트2");
         System.out.println("v의 크기(size)=>"+v.size());
         //값출력
         for(int i=0;i<v.size();i++) {
             String temp=(String)v.elementAt(i);
             System.out.println("temp=>"+temp);
             //System.out.println("temp=>"+v.elementAt(i));//자동형변환
         }
         //다양한 값을 저장
         Vector v2=new Vector();
         v2.add(new Character('a'));//v2.add('c');//0
         //3.141592->1
         v2.addElement(100);//1->2-->100->"Set"
         //중간에 값을 추가(insert)
         v2.insertElementAt(3.141592, 1);//저장할 객체명,삽입할 인덱스번호
        //수정->setElementAt(수정할 객체명,수정할 인덱스위치)
         v2.setElementAt("Set",2);
         //출력
         for(int i=0;i<v2.size();i++) {
             System.out.println("v2의 값=>"+v2.elementAt(i));
         }
    }
}
```

```java
package j0619;

import java.util.*;

public class ArrayListTest {
    public static void main(String[] args) {
        // TODO Auto-generated method stub
        //Vector->메모리를 많이 차지->ArrayList,LinkedList
        //ArrayList<String> list=new ArrayList<String>();
        LinkedList<String> list=new LinkedList<String>();
        list.add("포도");//0-->뒤에다 추가-->수정 포도->오렌지
        list.add("딸기");//1
        //키위----------->2
        list.add("복숭아");//2->3
        //중간에 삽입->add(삽입할 위치,삽입할 객체명)
        list.add(2,"키위");
        //수정->set(수정할 위치,수정할 값)
        list.set(0, "오렌지");
        //삭제->remove(삭제할 데이터명) or remove(인덱스번호)=>중복데이터
        list.remove("키위");
        //배열->검색방법1
        for(int i=0;i<list.size();i++) {  //get(인덱스번호)
            String temp=list.get(i);  //제너릭이 적용된 컬렉션은 꺼내올때 형변환필요X
            System.out.println("temp=>"+list.get(i));
        }
        //검색방법2=>확장 for문->배열이나 컬렉션객체의 값을 꺼내올때
        System.out.println("==확장 for문==");
        for(String s:list)  //for(자료형 출력변수명:배열 또는 컬렉션객체명)
            System.out.println(s);
    }
}
```

---

```java
package j0619;

import java.util.*;

//제너릭?, Map(Hashtable,HashMap)+검색(Enumeration,Iterator)

public class EnumTest {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
         Vector<String> v=new Vector<String>();
         v.add("test1"); v.add("test2"); v.add("test3");
         System.out.println("해쉬테이블(Map)");
         Hashtable<Integer,String> h=new Hashtable<Integer,String>();
         h.put(1,"홍길동"); h.put(2, "테스트");h.put(3, "010-1234-8976");
         String tel=h.get(3);//get(검색할 키명)->키를 알고 있으면 검색속도가 제일 빠르다
         System.out.println("tel=>"+tel);

         //1.공통으로 검색(Enumeration->배열의 값을 출력)
         Enumeration<String> eu=v.elements();//String->(String)Object
         while(eu.hasMoreElements()) //검색해서 꺼내올 객체가 존재한다면
             System.out.println(eu.nextElement());
         //2.Iterator->책꽂이에 들어가 있는 책을 연상->책을 꺼내올때 순서중요X
         Iterator<String> ih=v.iterator();
         while(ih.hasNext())//검색해서 꺼내올 객체가 존재한다면
             System.out.println(ih.next());
    }
}
```

---

##### 17 끝

---

18일차(자바의 컬렉션 작성법2 네트워크 개요 및 작성법)

#### 인터페이스의 객체를 얻어오는 방법 ★★★

1.	new 연산자 이용하는 경우

```
	Interface<String> I = new Interface<String>(); => 불가능
	자기자신의 객체를 생성 할 수 없다!
	Set set = new Set(); 은 불가능!!!!
	자식클래스를 통한 new 연산자를 이용한 객체 생성 가능
	예외) Set set = new HashSet();
	부모인터페이스 자료형 인터페이스객체명 = new 자식클래스명();

  왼쪽 자료형과 오른쪽 자료형이 다를 경우 오른쪽이 자식클래스이고 왼쪽이 부모 클래스이다.

```

1.	메서드의 매개변수를 통해서 얻어오는 경우=>웹

2.	메서드의 반환형을 통해서 얻어오는 경우 iterator<String> ih2=h.values().iterator();

---

#### 제너릭의 개요 및 장점

```
<b>컬랙션에 데이터를 저장</b>할 때
만약 String이라는 데이터타입의 객체만 넣고 싶은데 Integer타입도 들어가게 되었을 경우 에러 발생 안한다
때문에 <b>데이터 꺼내올 경우 알게되고</b> 유지보수가 어렵다
그렇기에 <b>처음 객체 생성</b> 시 자료형을 지정해주는데 형식은 <b>클래스명<자료형> 으로 선언해준다.
```

#### 제너릭의 장점은

```
1. 지정된 자료형외에는 저장불가(명확하다)
2. 꺼내올때 명시적인 형변환을 할 필요가 없다.
(들어갈 때 add(Object o)로 들어가므로 Object 데이터 타입으로 들어가서 형변환 해주어야하는데 안해도된다
```

---

#### 제너릭 종류

1.	class 클래스명<T>{}
2.	T는 모든 데이터 타입을 수용하겠다는 표시로
3.	이 클래스를 통해서 객체 생성시 객체의 자료형을 미리 지정해준다.

4.	와일드카드

2-1. <?>

-	<? extends Object> 와 동일하다 <br>(why? 자바에서 만드는 클래스의 최상위 클래스는 Object이기 때문이자)
-	어떠한 자료형이든 다 처리해줄 수 있는 제너릭이다

```java
 public static void printData(List<?> list) {
   for( Object obj : list) {
     System.out.println(obj);
   }
 }
```

2-2. <? extends T>* ? 자식클래스, T 부모클래스* 상속관계로 이루어진 클래스만 자료형으로 받겠다는 표시* 부모클래스와 그에 해당하는 자식클래스까지 자료형으로 입력 받겠다는 의미

```java
public static void printData(List<? extends Person> list){
  for( Object obj : list ){
    System.out.println(obj);
  }
}
```

2-3. <? super T>* ?는 부모클래스 T 자식클래스* 자식클래스를 고정으로 지정해주고 자식클래스와 연관이 있는 부모클래스는 모두 매개변수로 허용해준다

```java
public static void printData(List<? extends Man> list){
  for( Object obj : list ){
    System.out.println(obj);
  }
}
```

1.	class Vector<E>
2.	E는 요소를 의미한다
3.	ArrayList<Person> list = new ArrayList<Person>();
4.	list.add(p);
5.	ArrayList라는 컬렉션객체에 Person이라는 개인정보가 담긴 객체를 저장한다.

6.	class Hashtable/class hashtable <K,V>

7.	K->키값을 지정, 키값에 해당하는 Value값 저장시킨다.

8.	세션처리(웹)->로그인정보
