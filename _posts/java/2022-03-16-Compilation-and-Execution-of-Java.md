---
title: "Java의 실행 과정"

date: 2022-03-16
last_modified_at: 2021-03-16

categories:
  - Java

tags:
  - ["컴파일 언어", "인터프리터 언]

---

목차

- [컴파일 언어와 인터프리터 언어](#컴파일-언어와-인터프리터-언어)

- [Java 실행 과정](#java-실행-과정)

- [정리](#정리)

- [참고 자료](#참고)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> # 컴파일 언어와 인터프리터 언어

 일반적으로 Java의 수행 속도는 C언어보다 느리고, Python보다는 빠르다고 하는데, 이는 컴파일 방식과 인터프리터 방식의 차이에서 기인된다고 볼 수 있습니다.

 C언어는 대표적인 컴파일 언어로, 소스 코드를 기계어로 번역하는 빌드 과정에서는 인터프리터 언어에 비해 시간이 걸리지만, 런타임 시에는 번역을 마친 기계어를 바로 실행하기 때문에 빠른 수행 속도를 보입니다. 반면, Python과 같은 인터프리터 언어는 빌드 과정 없이 바로 실행이 가능하지만, 한 줄씩 실시간으로 번역을 통해 실행되기 때문에 컴파일 언어에 비해 수행 속도가 느립니다.

 그리고 Java는 초기에 인터프리터 방식으로만 사용되었지만, 느린 속도를 개선하기 위해 JDK 1.2.2 버전부터 JIT Compiler가 추가되었습니다. 하지만 모든 코드를 컴파일하는 방식은 자원 소모가 크기에 컴파일 방식과 인터프리터 방식을 함께 사용하게 되었습니다.

<br>

---

> # Java 실행 과정

 이제 본격적으로 Java의 실행 과정을 살펴보겠습니다.

 컴파일 과정과 런타임 과정을 간단히 살펴보면 아래의 그림과 같습니다.

![image](https://user-images.githubusercontent.com/70495425/158527802-43650f96-f235-43f0-b1ae-2a2d4eb42991.png)

_(이미지 출처: [Execution Process of Java Program in Detail](https://simplesnippets.tech/execution-process-of-java-program-in-detail-working-of-just-it-time-compiler-jit-in-detail/))_



 우선 개발자가 코드를 작성하면, 자바 컴파일러(JDK에 포함된 자바 컴파일러, `javac`)가 소스 코드(`.java`) 파일을 JVM이 이해할 수 있는 바이트코드(`.class`)로 변환합니다. 이 과정을 컴파일 과정이라고 합니다. 

 그리고 바이트코드로 변환된 파일은 JVM을 통해 실행되는데, JVM 내의 `Class Loader`가 런타임 시점에 바이트 코드를 메모리에 끌어올립니다. 

- _Class Loader_: 클래스 파일을 로드하고, 검증 과정과 변수와 클래스를 초기화하고 메모리에 할당하는 역할 수행

 이후 실행 엔진을 통해 바이트코드를 OS가 이해할 수 있는 기계어로 해석되고, 명령어 단위로 실행됩니다.(_동일한 바이트코드를 JVM이 OS에 맞게 해석해서 실행하기 때문에 'Java는 플랫폼에 독립적이다'라는 특징이 있습니다_) 그리고 바이트코드를 해석하는 과정은 `Interpreter` 방식과 `JIT Compiler` 방식이 있습니다.

- Interpreter: 명령어를 하나씩 읽어서 해석하고, 같은 코드도 매번 해석해야 하기에 속도가 느리다.

- JIT(Just In Time): 실행 시점에서 인터프리트 방식으로 기계어 코드를 생성하면서 그 코드를 캐싱하여, 같은 함수가 여러 번 불릴 때 매번 기계어 코드를 생성하는 것을 방지. 다만, 초기 구동 시 컴파일에 시간과 메모리를 소모하기 때문에 실행 시간이 짧은 경우 효율이 떨어지기도 함. (_Java에서만 사용되는 컴파일 방식이 아니며, Java에서는 JDK 1.2부터 Java HotSpot VM을 지원하기 시작해, 1.3부터 기본으로 탑재_)

<br>

> ## 정리

 실행 과정을 간략히 정리해보겠습니다.

1. 자바 소스코드(`.java`) 작성

2. 자바 컴파일러(`javac`)가 소스코드를 바이트코드(`.class`) 파일로 변환
   (개발자가 IDE를 사용해 코드를 작성한 뒤 저장을 하면 자동으로 클래스 파일을 생성)

3. JVM 구동 명령어에 의해 JVM에 전달
   (IDE에서 실행 버튼을 누르면 JVM의 클래스 로더에 바이트 코드가 전달됨)

4. 클래스 로더는 전달받은 바이트코드를 메모리에 올림

5. 실행 엔진이 메모리에 올라온 바이트코드를 해석하여 실행

<br>

<br>

> ### 참고

[자바 [JAVA] - Comparable 과 Comparator의 이해](https://st-lab.tistory.com/243)

[JAVA1 - 4.2. 실행 - Java의 동작원리 - YouTube](https://www.youtube.com/watch?v=9V0rdrm59X4)

[[객체 중심 Java] #03 1장 - Java 프로그램의 실행과정 이해 - YouTube](https://www.youtube.com/watch?v=8uPwUnQw14I)

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
