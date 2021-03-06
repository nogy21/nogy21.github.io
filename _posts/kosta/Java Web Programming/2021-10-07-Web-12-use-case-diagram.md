---
title: "WEB(12) - Use Case Diagram - 수업 43일차"

date: 2021-10-07
last_modified_at: 2021-10-09


categories:
  - Java Web Programming

tags:
  - [kosta, "use case diagram"]

---

목차

* [Use Case Diagram](#use-case-diagram)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> ## Use Case Diagram

: 어플리케이션 분석 설계를 위한 UML(Unified Modeling Language) 중 요구분석을 위한 다이어그램으로 **시스템에 요구되는 기능을 사용자 관점에서 나타냅니다**.

<br>

크게 두 가지 핵심 단위로 use case와 actor가 있습니다.

1. Use Case: 사용자 관점의 기능 단위(서비스 단위)를 나타냅니다.
   (ex - 도서 검색, 도서 대여) 
2. Actor: 시스템 외부에 존재하면서 시스템과 상호 작용하는 개체를 나타냅니다.
   (ex - 고객, 관리자)

<br>

수업에서는 StarUML이라는 툴을 사용해서 use case diagram을 그려봤습니다.<br>

우선 StarUML에서 use case diagram을 사용하기 위해 설정 정보를 변경해줘야 하는데, Model 탭에서 Add Diagram 항목에 Use Case Diagram으로 변경해서 사용하도록 합니다.

<br>

연습을 위해 도서관리라는 간단한 서비스를 상정하여 진행하도록 하겠습니다.<br>

우선 Use Case Subject를 통해 도서관리라는 큰 박스를 만들어줍니다.<br>

이 안에 Use Case(기능 단위)를 생성하고, 밖에는 Actor(객체)를 생성해서 아래와 같이 나타냅니다.<br>

![image](https://user-images.githubusercontent.com/70495425/136310617-d9000ae3-e66b-40e0-82ef-a9d0d0eca906.png){:.aligncenter}

개체와 유즈케이스와의 관계는 보통 Association으로 표현합니다.<br>

의존 관계(Dependency)는 크게 두 종류를 사용합니다.

- include: 기본 Use Case가 실행되기 위해서는 반드시 다른 특정 Use Case의 행위를 포함해야 한다는 것을 의미합니다.

  ex) 책 대여 Use Case ---------> 회원 인증 Use Case

- extend: Use Case가 특정 Use Case에 정의된 행위로 선택적으로 추가 확장될 수 있다는 것을 나타냅니다.

  ex) 책 대여 Use Case <--------- 부록 CD 대여 Use Case

<br>

추가적으로 색상을 나눈 이유는 1차 개발과 2차 개발로 나누기 위해서입니다.<br>

작은 규모의 프로젝트라고 하더라도 이렇게 업무를 단계별로 나누어서 진행하는 것이 바람직합니다.<br>

<br>

---

오후에는 미니 프로젝트 진행을 위한 요구분석을 진행했는데, 다음 글을 통해 미니 프로젝트 전체 내용을 총 세 편으로 나누어 글을 작성하고자 합니다.

<br>

---

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

