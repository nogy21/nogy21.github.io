---
title: "WEB(08) - jsp basic - 수업 39일차"

date: 2021-09-29
last_modified_at: 2021-09-29


categories:
  - Java Web Programming

tags:
  - [kosta, jsp, jsp syntax, jsp life cycle]

---

목차

* [JSP란?](#jsp)
  * [JSP 기본 문법](#jsp-기본-문법)
  * [JSP LifeCycle](#jsp-lifecycle)
* [예제 코드 1](#예제-코드-1)
* [예제 코드 2](#예제-코드-2)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> ## JSP

: Java Server Page의 약자로 **동적인 웹페이지**를 위한 자바 기반의 서버 사이드 스크립트 언어입니다.

특징

- 서블릿과는 다르게 HTML 상에서 **자바코드(or jsp tag)를 삽입하는 형태**로 개발합니다
- JSP는 **WAS(Web Container)에 의해 java로 생성되고 컴파일되어 실행**됩니다
  (생성된 java class는 HttpServlet의 자식 클래스입니다)
- 생성된 자바 파일은 tomcat/work 디렉토리에 저장됩니다

참고)_Model2 Architecture MVC에서 Model은 java beans, **View**는 **JSP**, Controller는 Servlet이 담당합니다._

<br>

<br>

---

> ## JSP 기본 문법

| 종류                       | 문법        | 설명                                                         |
| -------------------------- | ----------- | ------------------------------------------------------------ |
| jsp 주석                   | `<%-- --%>` | 주석. 참고) html 주석: `<!-- -->`                            |
| scriptlet <br />스크립틀릿 | `<% %>`     | java code를 작성. `service`메서드 내에 자바 코드로 삽입.     |
| expression                 | `<%= %>`    | `out.print()` 역할. 화면 출력용                              |
| declaration<br />선언      | `<%! %>`    | 멤버 변수, 메서드 정의 시 사용                               |
| directive<br />지시자      | `<%@ %>`    | jsp 문서 정보를 웹 컨테이너에게 전달(한글 처리 방식, 문서 타입, import, errorPage 등) |

<br>

이클립스에서 jsp 파일을 처음 생성하면 다음과 같이 작성이 되어 있습니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

</body>
</html>
```

최상단부의 **<%@ %>태그**를 통해 **jsp 파일임을 명시**합니다. 그래서 이 부분이 제대로 작성되지 않는다면 jsp로 인식되지 않습니다.

그 외의 부분은 html 파일과 동일하며 먼저 설명드렸다시피 jsp는 HTML 상에서 자바코드(or jsp tag)를 삽입해서 작성합니다.

<br>

<br>

---

> ## JSP LifeCycle

- jsp의 생성주기는 서블릿과 동일한 구조를 갖고 입습니다.
  차이점은 jsp를 이용해 `.java`를 생성하고 `.class`로 컴파일해서 실행하며 `tomcat/work` 디렉토리에 생성합니다

![image](https://user-images.githubusercontent.com/70495425/135720869-edc05093-af3e-47ba-af81-340473dfb53c.png)

<br>

<br>

---

> ## 예제 코드 1

간단한 jsp 파일을 작성해보겠습니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>step1 jsp basic</title>
</head>
<body>
	<%-- jsp 주석 : client 측에 전달되지 않는다 --%>
	jsp 기본
	<br><br>
	<%
	// 아래 코드는 service 메서드내에 삽입된다 	
	for (int i = 0; i < 10; i++) {
	%>
	<font color="green" size="5"><%=i + 1%>.가을전어회</font>
	<br>
	<%}%>
	<br>
	<br>
	<%-- 인스턴스 변수 선언 --%>
	<%!private int count;%>
	<%-- service메서드 내에 out.print(count++) 로 코드가 생성  --%>
	접속수
	<%=count++%>
</body>
</html>
```

10번 라인을 보시면 html 내에서는 <%-- --%>의 주석 코드를 사용하지만, 14번 라인의 코드는 스크립틀릿(`<% %>`) 내에서 주석을 사용하기에 자바 파일에서 사용하는 `//`와 같이 주석을 사용합니다.<br>

자바 코드의 사용 역시 동일하며 반복문은 13~19번 라인과 같이 스크립틀릿을 일부분씩 사용하고 내용 부분은 html 내용을 삽입하여 사용할 수 있습니다.<br>

스크립틀릿은 내부 실행 함수인 `sevice()` 내에서 실행되기에 인스턴스 변수의 선언은 `<%! %>`태그를 사용해 `service()` 밖에 선언될 수 있도록 합니다.<br>

실행시켜보면 다음과 같이 작동하는 것을 확인할 수 있습니다.

<img src="https://user-images.githubusercontent.com/70495425/135719705-7e6f5f57-ef44-4137-8757-cace685172ef.png" alt="image" style="zoom:80%;" />

<br>

자바 파일에서는 어떻게 작성이 되었는지 직접 확인해보도록 하겠습니다.<br>

![image](https://user-images.githubusercontent.com/70495425/135719671-12082efc-6012-4139-a08b-a538f3381f79.png)

위와 같이 web-tomcat의 work 디렉토리 하위 경로에 java 파일이 생성되었습니다.

전체 코드를 보면 총 188라인으로 긴 편이어서 저희가 작성한 부분만 확인하도록 하겠습니다.<br>

<img src="https://user-images.githubusercontent.com/70495425/135719816-6ea60993-0d12-423e-8292-6e3377ac4137.png" alt="image" style="zoom:67%;" />

상단부에 패키지와 임포트한 클래스들, 저희가 작성한 `step1_jsp`클래스 선언부가 보입니다.<br>

19번 라인에는 jsp 파일의 23번 라인에 작성했던 `count`변수가 선언되어 있습니다.<br>

![image](https://user-images.githubusercontent.com/70495425/135720665-67649d21-66e8-4dd2-bc84-da7ca8edb9fc.png)

74번 라인부터 `_jspInit()`, `_jspDestroy()`, `_jspService()`메서드가 생성되어 있는 것을 확인할 수 있습니다.<br>

그리고 다음 그림에서 `_jspService()`메서드 안에 저희가 작성해둔 코드가 있다는 것 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/70495425/135720735-bf8c8316-5dce-4f1d-8540-5613de19893f.png)

<br>

<br>

---

> ## 예제 코드 2

이번에는 main/java/ 경로에 model 폴더를 만들고 FoodVO 파일을 만들겠습니다. <br>

여기서 `name`, `maker`, `price` 변수를 생성한 뒤 생성자와 get, set, toString 메서드를 작성한 뒤 jsp파일에서 여기서 작성한 FoodVO 클래스를 이용하도록 하겠습니다.<br>

<br>

코드는 다음과 같이 작성합니다.

<img src="https://user-images.githubusercontent.com/70495425/135721024-d2dd39b8-d4c8-41af-8373-fede9083f726.png" alt="carbon" style="zoom:80%;" />

첫번째 라인에서 `FoodVO` 클래스를 `import` 했고, 4~6번 라인에서 해당 객체를 생성한 뒤 body 태그에 테이블을 작성했습니다.<br>

<br>

생성한 테이블에 객체의 데이터들을 호출해서 출력하는 코드이고, 실행해보면 다음과 같이 출력되는 것을 확인하실 수 있습니다.<br>

![image](https://user-images.githubusercontent.com/70495425/135721100-9463fac1-b836-4489-8fab-b6c8ab301b3d.png)

<br>

<br>

---

데이터를 늘려서 리스트를 작성한다면, 반복문을 사용해서 코드를 줄일 수 있습니다.

```jsp
<%@page import="model.FoodVO"%>
<%@page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%
ArrayList<FoodVO> list = new ArrayList<FoodVO>();
list.add(new FoodVO("신라면", "농심", 1500));
list.add(new FoodVO("진라면", "오뚜기", 1300));
list.add(new FoodVO("참이슬", "진로", 1400));
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>jsp list 표현</title>
<link rel="stylesheet" type="text/css" href="css/home.css">
</head>
<body>
	<table>
		<thead>
			<tr>
				<th>순번</th>
				<th>이름</th>
				<th>제조사</th>
				<th>가격</th>
			</tr>
		</thead>
		<tbody>
			<%
			for (int i = 0; i < list.size(); i++) {
			%>
			<tr>
				<td><%=i + 1%></td>
				<td><%=list.get(i).getName()%></td>
				<td><%=list.get(i).getMaker()%></td>
				<td><%=list.get(i).getPrice()%></td>
			</tr>
			<%
			}
			%>
		</tbody>
	</table>
</body>
</html>
```

<br>

<br>

실행 결과는 다음과 같습니다.

![image](https://user-images.githubusercontent.com/70495425/135721219-5e2bd49a-0c6f-4554-8c38-1321315e42af.png)

<br>

<br>

---

마지막으로 에러 페이지를 작성해보겠습니다.<br>

현재 jsp 페이지에서 error가 발생하면 `<%@ %>` 태그 안에 지정한 링크를 클라이언트에게 응답하게 하는 설정입니다.

<img src="https://user-images.githubusercontent.com/70495425/135721328-e8bec749-2e65-45d7-9c6d-2ea87d4f7ee6.png" alt="carbon" style="zoom:80%;" />

실행 결과는 다음과 같습니다.

<img src="https://user-images.githubusercontent.com/70495425/135721343-d8d24e60-2c8a-4dab-969c-8efffed23de8.png" alt="image" style="zoom:80%;" />

<br>

에러 페이지를 설정하지 않았다면 사용자에게 불필요한 다양한 정보가 다음과 같이 노출될 수 있습니다.

![image](https://user-images.githubusercontent.com/70495425/135721396-afc930c6-c994-493c-962a-01861a12e90a.png)



---

이것으로 오늘 복습은 마치도록 겠습니다.<br>



다음 시간에는 예제를 통해 jsp 문법 연습을 해보도록 하겠습니다.

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

