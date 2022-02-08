---
title: "Comparable, Comparator"

date: 2022-02-07
last_modified_at: 2021-02-08

categories:
  - Java

tags:
  - ["Comparable", "Comparator"]

---

목차

* [Comparable? Comparator?](#Comparable?-Comparator?)
  * [Comparable](#Comparable)
  * [Comparator](#Comparator)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> # Comparable? Comparator?

`Comparable`과 `Comparator` 모두 인터페이스로, <u>객체를 비교할 수 있도록 메서드</u>를 제공하고 있습니다. 그리고 <u>객체를 정렬하기 위해</u> 많이 사용되고 있습니다.

  

두 인터페이스의 비교에 앞서 자바로 배우는 자료구조 수업을 들으면서 `Comparable` 인터페이스에 대해 알아봤었고, 동등연산자(`==`)와 `equals()` 메서드의 사용을 살펴본 적이 있습니다. 

- 동등연산자`==`: 주소값을 비교하며 기본형(Primitive Type)을 대상으로 적용(_객체 유형은 동일한 유형에 한해 적용이 가능_)

- `equals()`: 객체의 내용물을 비교하며 참조형(Reference Type)을 대상으로 적용

  

그리고 `compareTo(T o)` 메서드는 `Interface Comparable<T>`가 구현되어 있는 객체에서 사용 가능한 메서드로, 자기 자신과 매개변수로 전달받은 객체를 비교해서 <u>자신의 값이 작다면 음수, 같다면 0, 크다면 양수를 반환하는 형태</u>를 보였습니다.

  

이러한 `compareTo(T o)` 메서드를 오버라이드하면 사용자가 정의한 기준에 따라 정렬 기능을 사용할 수 있습니다. `Comparator` 인터페이스 역시 `complare(T o1, T o2)` 메서드가 두 매개변수 객체를 비교하는 기능을 제공해주기에 오버라이드를 통해 정렬 기능에 자주 사용됩니다.

  

차이점이라면 `Comparable`은 자기 자신과 매개변수 객체를 비교하고, `Comparator`는 두 매개변수 객체를 비교하며, 구현 방법에서도 차이를 보입니다.

  

두 수의 비교 결과에 따른 정렬 기능 작동 방식은, <u>음수 또는 0의 경우 변화가 없고, 양수일 경우 두 원소의 위치를 바꾸게</u> 합니다.(_참고로 정렬은 별도의 설정이 없다면 오름차순을 기준으로 합니다._)

  

먼저 `Comparable` 인터페이스를 활용한 예제를 살펴보겠습니다.

```java
//Java Program to demonstrate the use of Java Comparable.
//Creating a class which implements Comparable Interface

import java.util.ArrayList;
import java.util.Collections;

class Student implements Comparable<Student> {
    int rollno;
    String name;
    int age;

    Student(int rollno, String name, int age) {
        this.rollno = rollno;
        this.name = name;
        this.age = age;
    }

    /* 아래와 동일
    public int compareTo(Student st){
        if(age==st.age)
            return 0;
        else if(age>st.age)
            return 1;
        else
            return -1;
    }*/

    @Override
    public int compareTo(Student st) {
        return age - st.age;
    }

}

//Creating a test class to sort the elements
public class ComparableTest {
    public static void main(String args[]) {
        ArrayList<Student> al = new ArrayList<Student>();
        al.add(new Student(101, "Vijay", 23));
        al.add(new Student(106, "Ajay", 27));
        al.add(new Student(105, "Jai", 21));

        Collections.sort(al);
        for (Student st : al) {
            System.out.println(st.rollno + " " + st.name + " " + st.age);
        }
    }
}
```

주목해야 할 부분은 `compareTo()` 메서드를 오버라이드 하는 부분입니다. 

자기 자신의 나이와 다른 학생의 나이를 비교해서 음수, 0, 또는 양수가 반환됩니다. 

이렇게 정의한 학생 클래스의 인스턴스를 `sort()` 메서드를 통해 정렬을 하면 아래와 같이 나이를 오름차순으로 정렬하여 결과가 출력됩니다.

```
105 Jai 21
101 Vijay 23
106 Ajay 27
```

<br>  

다음은 `Comparator`를 활용한 예제입니다.

```java
//Java Program to demonstrate the use of Java Comparator

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

class Student2 {
    int rollno;
    String name;
    int age;

    Student2(int rollno, String name, int age) {
        this.rollno = rollno;
        this.name = name;
        this.age = age;
    }
}

class AgeComparator implements Comparator<Student2> {
    @Override
    public int compare(Student2 s1, Student2 s2) {
        /*if (s1.age == s2.age)
            return 0;
        else if (s1.age > s2.age)
            return 1;
        else
            return -1;*/
        return s1.age - s2.age;
    }
}

class NameComparator implements Comparator<Student2> {
    @Override
    public int compare(Student2 s1, Student2 s2) {
        return s1.name.compareTo(s2.name);
    }
}

class ComparatorTest {
    public static void main(String args[]) {
        //Creating a list of Student2s
        ArrayList<Student2> al = new ArrayList<Student2>();
        al.add(new Student2(101, "Vijay", 23));
        al.add(new Student2(106, "Ajay", 27));
        al.add(new Student2(105, "Jai", 21));

        System.out.println("Sorting by Name");
        //Using NameComparator to sort the elements
        Collections.sort(al, new NameComparator());
        //Traversing the elements of list
        for (Student2 st : al) {
            System.out.println(st.rollno + " " + st.name + " " + st.age);
        }

        System.out.println("sorting by Age");
        //Using AgeComparator to sort the elements
        Collections.sort(al, new AgeComparator());
        //Travering the list again
        for (Student2 st : al) {
            System.out.println(st.rollno + " " + st.name + " " + st.age);
        }

    }
}
```

`compare()` 메서드는 2개의 인자를 필요로 합니다. `sort()` 메서드를 사용할 때에 `Collection` 타입의 인자만이 아니라, `Comparator`인터페이스를 구현한 클래스의 인스턴스를 함께 인자로 넣어준 뒤 정렬을 하면, 사용자가 정의한 기준에 따라 정렬이 되어 출력이 되는것을 확인할 수 있습니다. 



결과는 다음과 같습니다.

```
Sorting by Name
106 Ajay 27
105 Jai 21
101 Vijay 23

Sorting by Age       
105 Jai 21
101 Vijay 23
106 Ajay 27
```

<br>

> ### 참고

[Difference between Comparable and Comparator - javatpoint](https://www.javatpoint.com/difference-between-comparable-and-comparator)

[자바 [JAVA] - Comparable 과 Comparator의 이해](https://st-lab.tistory.com/243)



---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
