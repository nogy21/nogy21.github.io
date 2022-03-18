---
title: "자바의 특성과 알고리즘 기본 - 2"

date: 2022-02-07
last_modified_at: 2022-02-09


categories:
  - DataStructure

tags:
  - ["자바로 구현하고 배우는 자료구조", "부스트코스"]

---

<br>

목차

- [자바 특성 및 알고리즘 기본](#자바-특성-및-알고리즘-기본)
  
  - [객체지향 프로그래밍](#객체지향-프로그래밍)
  
  - [Comparable 인터페이스](#Comparable-인터페이스)
  
  - [제너릭 프로그래밍](#제너릭-프로그래밍)
  
  - [매개변수화 타입](#매개변수화-타입)
  
  - [Autoboxing](#Autoboxing)
  
  - [예외](#예외)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> ## 객체지향 프로그래밍

객체의 메모리 할당 방법에 대해 간단히 알아보겠습니다.

```java
Student s = new student();
```

위와 같이 `new`를 사용하여 객체(인스턴스) `student`를 만들면 JVM은 코드를 읽고 메모리가 얼마나 필요한지 계산하고, 그만큼의 공간을 **Heap에 할당**합니다. 그리고 Heap에 있는 공간을 가리키는 4바이트짜리 포인터를 하나 만들게 됩니다.

![image](https://user-images.githubusercontent.com/70495425/152676406-aefaf2e1-f93e-475b-a0f6-37878a16d230.png)

<br>

이번에는 상속 관계를 살펴보겠습니다.

```java
public class Person{
}
public class Student extends Person{
}
public class Undergraduate extends Student{
}
```

위와 같이 클래스들이 존재하면, <u>학생이 사람에 속하게 되고, 학부생이 다시 학생에 속하게 되기에 학부생 또한 사람에 속하게 됩니다</u>.

이제 `Student` 클래스를 만들면 JVM은 이 클래스가 `Person`을 상속한다는 것을 알아채고 `Person` 클래스에 있는 변수와 메소드를 모두 가져옵니다. 마찬가지로, `Undergraduate` 클래스를 만들면 `Student` 클래스에 있는 변수와 메소드를 모두 가져옵니다.

### **특징 1. 단일 상속**

어떤 프로그래밍 언어에서는 여러 클래스를 상속받을 수 있는데, 이를 *다중 상속* 이라고 합니다. 그런데 만약 상속받은 여러 클래스에 동일한 변수가 있을 경우, 어느 것을 상속받아야 하는지 모르게 됩니다. 이런 상황을 처리하는 방법이 존재하기는 하지만, Java와 여러 다른 프로그래밍 언어에서는 애초에 이러한 *다중 상속을 허용하지 않습니다*.

### **특징 2. 상속받는 클래스의 정보만** 갖고 있습니다.

![](https://cphinf.pstatic.net/mooc/20210428_208/1619586320261nfFIt_PNG/mceclip1.png)

위의 구조에서 학부생은 교직원이나 관리 직원들에 대한 정보는 가지고 있지 않고, <u>트리를 올라갈 수만 있습니다</u>.

### **특징 3. 상속받는 클래스의 공간**을 함께 할당합니다.

```java
Undergraduate u = new Undergraduate();
```

상속을 받은 상태로, 새로운 객체 `Undergraduate u`를 정의하면, <u>u에는 4바이트가 할당</u> 됩니다. 그리고 이 포인터가 가리키는 힙에는 <u>Undergraduate에 맞는 공간이 할당</u> 됩니다. `Undergraduate`에 있는 `year`, `Student` 클래스에 들어 있는 모든 변수, `Person` 클래스에 들어 있는 `redid`, `name`, `email` 등의 변수도 힙에 들어갑니다.

```java
Student s = new Undergraduate();
```

위와 같이, `s`라는 이름의 `Student`를 만들 수도 있습니다. 이렇게 코드를 적으면 <u>Undergraduate 객체에 필요한만큼의 공간</u>을 할당받고 `Person`의 변수, `Student`의 변수, `Undergraduate`의 변수를 모두 알 수 있습니다. 따라서, <u>Student 클래스의 변수를 파악_ 하고 메모리를 계산할 수 있습니다.</u>

```java
Undergraduate u = new Student();
```

하지만 반대로 할 수는 없습니다.이런 코드를 쓰게 되면 힙에 할당되는 공간은 <u>Student에 해당하는 만큼</u>입니다. 이렇게 되면 Student에 포함된 내용과 Person에 포함된 내용만 알 수 있고, <u>Undergraduate 클래스의 내용은 접근할 수 없기 때문에 이 코드는 컴파일되지 않습니다</u>.

<br>

> ## Comparable 인터페이스

객체의 비교 방법인 Comparable 인터페이스에 대해 알아보겠습니다.

자바에서는 문자열을 비교할 때 `equals()` 메서드를 사용합니다. `==` 연산자를 사용할 경우 대상의 주소값을 비교하기 때문에(`Call By Reference`) 올바른 결과값을 얻을 수가 없는데, 그러면 `equals()` 메서드는 어디에서 오는 걸까요?

`Object` 클래스에서 오는 걸까요? `String` 클래스에서 오는 걸까요?

```java
String one = "hello world";
String two = "hello world";
// 문자열 비교
if(one.equals(two))
    System.out.println("they are the same");
```

```java
Object one = "hello world";
Object two = "hello world";
// 객체 비교
if(one.equals(two))
    System.out.println("they are the same");
```

위의 두 코드는 모두 실행이 되고, 조건문은 상반된 결과가 나오게 됩니다.

첫 번째 비교는 참이 나오고, 두 번째 비교는 거짓이 나오죠. `String` 클래스의 `equals()` 메서드는 문자열을 비교하고, `Object` 클래스의 `equals()` 메서드는 *메모리 주소* 를 비교하기 때문입니다. `String` 클래스는 `Object` 클래스의 `equals()`를 오버라이드 하고 있습니다.

그래서 객체 간의 비교를 원한다면, `equals()` 메서드를 사용할 경우 해당 클래스에서 오버라이드한 메서드를 사용하는지, 혹은 `Object` 클래스의 메서드를 사용하는지 알 수가 없기 때문에 객체 클래스의 `equals()` 메서드 외에도 비교를 할 수 있는 방법이 있습니다.

`Comparable` 인터페이스를 통해 객체에서 원하는 자료형으로 비교가 가능합니다. `Comparable` 인터페이스는 <u>같은 자료형 의 다른 객체 하나를 인자로 받아와 비교하는 `compareTo()` 메서드</u>를 사용합니다. 

그런데 `equals()`와는 달리 두 객체를 단순히 같은지만 비교하지 않고, 인자에 비해 큰지 작은지에 대해서도 알 수 있습니다. 

```java
/*  `a.compareTo(b)`는 
    `a`가 `b`보다 작을 때는 0보다 작은 수,
    `a`와 `b`가 같으면 0, 
    `a`가 `b`보다 크면 0보다 큰 수를 반환합니다. */
public int compareTo(T obj)
    a.compareTo(b)
    if (a < b) return < 0;
    if (a == b) return 0;
    if (a > b) return > 0;
```

그리고 아래와 같이 `Comparable` 인터페이스를 만들면 자료형에 알맞은 데이터가 들어와 `compareTo()`를 통해 같은 자료형의 데이터를 비교할 수 있습니다.

```java
if(((Comparable<T>) data).compareTo(obj)==0)
```

<br>

> ## 제너릭 프로그래밍

이번에는 데이터 형식에 의존하지 않는 제너릭 프로그래밍에 대해 알아보겠습니다.

*제너릭 프로그래밍은 **다양한 자료형**의 객체에 대해 작성한 코드를 **재사용**한다는 객체 지향 기법입니다.*

제너릭 프로그래밍의 목표는 <u>1가지의 코드만 작성해서 이를 다른 자료형에 대해 재사용</u>할 수 있게 만드는 겁니다.

1. 제네릭을 사용하면 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있다.

2. 클래스 외부에서 타입을 지정해주기 때문에 따로 타입을 체크하고 변환해줄 필요가 없다. 즉, 관리하기가 편하다.

3. 비슷한 기능을 지원하는 경우 코드의 재사용성이 높아진다.

<br>

> ## 매개변수화 타입

제너릭 프로그래밍을 구현하기 위한 방법으로 매개변수화 타입을 사용할 수 있습니다. _꺾쇄괄호 <> 안에 Type Parameter를 넣어_ 컴파일 시 구체적인 타입이 결정되도록 하는 방법입니다.

```javascript
// 클래스
public class LinkedList
public class LinkedLilst<E>

// 함수
public void addFirst(String S)
public void addFirst(E obj)

public String removeFirst()
public E removeFirst()
```

위와 같이 매개변수화 타입을 사용할 수 있으며, 생성자의 경우에만 예외적으로 `E`를 사용하지 않습니다.

예시로, 다음 강의에서 배울 연결 리스트에 사용되는 노드를 확인해보겠습니다. 아래와 같이 어떤 자료형이든 담을 수 있는 제너릭 노드를 만들 수 있습니다.

```java
// 제너릭 노드
class Node<E>{
    E data;
    Node<E> next;
    public Node(E obj){
        data=obj;
        next=null;
    }
}
```

배열의 경우 `E[] storage = (E[]) new Object[size];`와 같이 캐스팅이 반드시 필요하며, `E[] storage = new E[size];`와 같이 E의 배열로 바로 정의하고자 하는 경우 컴파일 에러가 발생하게 됩니다.

<br>

> ## Autoboxing

기본 자료형은 객체가 아니기에 메소드를 상속받을 수 없습니다. 이를 위해 객체 버전의 기본 자료형인 래퍼클래스가 존재합니다. `byte`는 `Byte`, `int`는 `Integer` 등으로 바꿔서 사용을 할 수 있는데, jdk 1.5 이상부터 `autoboxing`과 `unboxing` 기능을 제공해주고 있습니다. 

`autoboxing`을 통해 기본 자료형을 객체로 사용하기 위해 사용자가 명시적으로 래퍼클래스로 변환을 시키지 않고도 기본 자료형을 객체처럼 자연스럽게 사용할 수 있었습니다.

<br>

> ## 예외

개발을 하던 도중 에러가 발생할 경우, 저희는 에러 메시지를 통해 무엇이 잘못되었는지 정보를 얻을 수 있습니다. 디버깅하는 과정을 도와주는 것이지요.

예외 상황을 설정하기 위해 `Exception`을 상속하는 방법을 살펴보겠습니다.

```java
// Exception 클래스 상속
public class FileFormatException extends Exception{
    public FileFormatException (){
        // super 호출
        super();
    }
    public FileFormatException (String s){
        super(s);
    }
}

// 예외 상황이 발생하면 throw
throw new FileFormatException("Your file is not well formatted")
```

위 코드에서와 같이 **Exception 클래스를 상속**받고 생성자를 만든 후, 생성자 안에서 **super를 호출**하면 예외 상황에 대한 클래스를 만들 수 있습니다. (super는 만약 어떤 것을 상속받았을 때 상속받은 클래스의 생성자를 호출한다는 의미입니다.)

   

이후 예외 상황이 발생하였을 때 **throw**를 사용하면, 그 예외 상황의 이름으로 에러가 발생하게 됩니다.

올바른 예외 상황을 설정함으로써 코드를 개인화하고 디버깅에 큰 도움을 받을 수 있게 되는거죠.

<br>

---

> ## 마치며

자바의 특성에 대해 살펴봤습니다. 이미 자바 개념에 익숙했기 때문에 대부분 쉽게 이해할 수 있는 내용들이었지만, 다시금 기초를 다지는 시간을 가질 수 있었습니다.

다음 강의부터 본격적으로 자료 구조 수업이 시작됩니다. 선형 자료구조부터 해시, 힙&트리, 트리, 정렬까지의 강의 내용을 차근차근 정리해보도록 하겠습니다.

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
