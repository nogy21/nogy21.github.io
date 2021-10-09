---
title: "WEB(13) - 미니프로젝트 - 회원관리 시스템"

date: 2021-10-08
last_modified_at: 2021-10-09


categories:
  - Java Web Programming

tags:
  - [kosta, 미니프로젝트, 회원관리]

---

목차

* [요구사항](#요구사항)
* [Use case diagram](#use-case-diagram)
* [구현 순서](#구현-순서)
* [파일 목록](#파일-목록)
* [Step1 기본세팅](#step1)
* [Step2 아이디로 회원 조회](#step2)
* [Step3 로그인, 로그아웃](#step3)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>



---

이번 시간에는 미니프로젝트(회원관리 시스템)를 진행해보겠습니다.<br>

<br>

연습 목적의 미니 프로젝트로 그동안 배운 내용들을 종합적으로 활용해보는 프로그램입니다. <br>

요구 사항을 토대로 요구 분석, 설계, 구현이 진행됩니다.<br>

<br><br>

요구 분석은 UML의 Use Case Diagram을 사용했습니다.<br>

설계 부분은 프로젝트가 간단하기도 하고 지금까지 배운 내용을 활용해보는 부분에 중점을 두기에 생략했습니다.
(_실제로 프로젝트를 진행한다면 UML의 Class Diagram, ERD를 사용할 수 있고, UI 설계는 전문적으로 프론트를 하지 않기 때문에 카카오 오븐, 발사믹 목업 등을 사용할 수 있습니다._)

구현은 use case diagram에서 추출한 기능들을 어떤 기능부터 구현할 지 순서를 정하고 파일 리스트와 구조를 미리 그려본 뒤 진행했습니다.(_팀프로젝트를 진행한다면 Use Case별로 업무를 분담해 진행합니다_)

<br><br><br>

---

> ## 요구사항

- 비로그인 상태에서 회원 아이디로 **회원 정보를 검색**할 수 있다(이름, 주소)
- 비로그인 상태에서 **회원 가입**이 가능하다
- 회원 가입 시 반드시 아이디 **중복 확인 과정**을 거쳐서 중복된 아이디가 아닐 경우 회원 가입이 되도록 한다.
  선택적으로 고객이 원할 경우 **프로필 사진 업로드**가 가능하도록 한다.
- 회원인 경우 **로그인**, 로그아웃 기능을 사용할 수 있다
- 회원은 주소로 **회원 정보를 검색**할 수 있다(주소를 통해 회원 리스트를 확인 가능)
- 회원은 자신의 **회원 정보를 수정**할 수 있다(아이디는 제외. 비밀번호, 이름, 주소)

<br><br>

---

> ## Use Case Diagram

<img src="https://user-images.githubusercontent.com/70495425/136647492-6b77a6ec-7c8f-4db5-8221-36b41448f237.png" alt="image" style="zoom:67%;" />

<br>

요구 사항을 바탕으로 use case diagram을 그렸습니다.

actor와 use case 추출에 중점을 두었고, 예상 가능한 것들은 생략했습니다.

(_예를 들면, 로그아웃은 로그인 내장 기능으로 사용이 가능하며 로그인 기능이 있으면 당연히 제공될 기능이기에 생략했습니다._)

<br>

<br>

---

> ## 구현 순서

use case diagram으로 기능을 추출했으니, 구현 순서를 정하겠습니다.

구현 순서는 정답이 없기에 프로젝트를 진행할 때에는 팀원들과 조율을 해서 정해야 합니다.<br>

다만 간단하고 확실하게 끝낼 수 있는 부분부터 시작하는 것이 효율성이 높다고 합니다.

<br>

수업 중에는 개개인마다 진행을 다르게 하면 설명을 해주시기 어렵기 때문에 아래와 같이 진행하기로 했습니다.<br>

`아이디로 회원조회` -> `로그인`, `로그아웃 `-> `주소로 회원리스트조회` -> `회원정보수정` -> `회원가입`(`아이디 중복확인`)

<br>회원 가입과 아이디 중복확인 기능이 마지막인 이유는 아직 배우지 않은 기술들을 사용해야하기 때문입니다.(팝업, `opener`, `hidden`)<br>

<br>
<br>

> ##  파일 구조

파일 구조는 다음과 같습니다.

| MVC        | 파일 이름                                                    |
| ---------- | ------------------------------------------------------------ |
| Model      | `MemberVO`, `MemberDAO`                                      |
| View       | `css/home.css`<br />`index.jsp`(_비로그인시: 아이디로 회원조회폼, 로그인폼, 회원가입링크<br/>로그인시: 아이디로 회원조회폼, 주소로 회원리스트조회폼, OO님(로그인폼 대체), 로그아웃링크, 회원정보수정링크_)<br />아이디로 회원조회: `findbyid-ok.jsp`, `findbyid-fail.jsp`<br/>로그인: `login-fail.jsp`에서 alert 후 `index.jsp`로 이동, 로그인 성공 시 `index.jsp`<br/>로그아웃: `index.jsp`에서 로그인 상태 시 링크 제공, 로그아웃 성공되면 다시 `index.jsp`로 이동<br/>주소로 회원리스트조회: `index.jsp`, 조회 결과: `findbyaddress-result.jsp`<br/>회원정보수정: `update-form.jsp`, `update-result.jsp`<br/>회원가입: `register-form.jsp`, `register-result.jsp`, `idcheck-ok.jsp`(팝업), `idcheck-fail.jsp`(팝업) |
| Controller | `FindMemberByIdServlet`, `LoginServlet`, `LogoutServlet`, `FindMemberListByAddressServlet`, `UpdateMemberServlet`, `RegisterMemberServlet`, `IdCheckServlet` |

<br><br>

---

> ## Step1

본격적으로 구현을 해보도록 하겠습니다.

기본 세팅을 위해 첫 페이지인 `index.jsp`와 간단한 css, 재사용성을 높이기 위한 `template.jsp`, 그리고 model 쪽의 `MemberVO`와 `MemberDAO`(_메서드를 제외한 DB 연동 코드_)를 작성해보겠습니다. 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>회원관리</title>
		<link rel="stylesheet" type="text/css" href="css/home.css">
	</head>
	<body>
		<div class="container">
			<h3>Model2 MVC 회원관리</h3>
             <a href="index.jsp">Home</a>
		</div>
	</body>
</html>
```

이렇게 첫 화면의 뼈대를 잡아 놓고 그대로 복사해서 `template.jsp`를 만들어두겠습니다. 다른 화면을 구성할 때 재활용하면 시간을 절약할 수 있습니다.

<br>

`MemberVO` 클래스에서는 String 타입의 `id`, `password`, `name`, `address` 변수를 private으로 선언하고 기본생성자, 모든 변수를 사용하는 생성자, getter, setter, toString()을 만듭니다.

추가적으로 리로드 될 때 세션 정보를 유지시키기 위해 객체 직렬화를 합니다.

<br>

`MemberDAO`클래스에서는 최근 배운 Singleton 디자인 패턴을 적용해서 생성자에 private으로 다른 클래스에서의 생성을 막아주고, `MemberDAO` 인스턴스를 private static으로 생성한 뒤 public static 메서드로 인스턴스를 반환받을 수 있도록 했습니다.

그리고 DB와의 연동을 위해  Connection URL과 사용자명, 비밀번호를 미리 선언해두었고, 사용한 자원들을 닫아주는 close 메서드를 만들어두었습니다. `MemberVO`와 `MemberDAO`의 기본 세팅은 JDBC 수업에서 많이 연습했기에 생략하도록 하겠습니다.

<br>

다음은 css 코드입니다. 백엔드 중심의 수업이기에 프론트 쪽의 기술을 많이 배우진 않습니다. 

나중에는 템플릿을 활용하는 정도로만 화면을 꾸며줄 예정이며, 이번 시간에는 기본 html이 가시성이 많이 떨어지기에 적당히 볼 수 있을 정도로 css를 작성했습니다.

```css
@charset "UTF-8";
.container{
	padding-top: 15px;
	padding-left: 50px;
	padding-right: 50px;
}
a{
	text-decoration: none;
	color: #ff9966;
}
a:hover {
	text-decoration: none;
	color: #99ccff;
}
table{
	border-collapse: collapse; 
}
table,td,th{
	border: 1px solid black;
}
td,th{
	padding: 15px;
}
```

<br><br>

---

> ## Step2

이제 첫번째 기능인 아이디로 회원 조회 기능을 구현해보겠습니다.

<img src="https://user-images.githubusercontent.com/70495425/136652081-4131278f-ff26-4478-ade8-e179fc82201b.png" alt="image" style="zoom:67%;" />

`index.jsp`에서 사용자가 회원 아이디를 입력하면 해당 데이터를 `FindMemberByIdServlet`으로 전송합니다.<br>전달받은 데이터를 `MemberDAO`의 `findMemberById()`의 매개변수로 할당해서 호출합니다.<br>

 `MemberDAO`에서는 일치하는 회원 정보가 있는지 `DB`와 연동해 확인한 뒤 결과에 따라 `findby-ok.jsp` 또는 `findbyid-fail.jsp`로 `forward`방식으로 이동시킵니다.<br>

 그리고 결과를 확인한 페이지에서는 다시 `index.jsp`로 이동할 수 있도록 합니다.

<br><br>

코드로 확인해보겠습니다. 

우선 `index.jsp`에서  해당 기능을 구현하기 위한 코드만 부분적으로 살펴보겠습니다.

```jsp
<form action="FindMemberByIdServlet">
    <input type="text" name="memberId" required="required" placeholder="아이디">
    <button>검색</button>
</form>
```

`form`태그를 사용해서 사용자가 입력한 데이터를 `action` 속성에 할당한 url로 전송합니다.

`button`태그는 `default` 타입이 `submit`이기에 타입 명시는 생략했으며, 나중에 css로 꾸며줄 상황이 생길 수 있어서 `input`태그 대신 `button`태그를 사용했습니다.

<br>

---

`FindMemberServlet`의 코드입니다.

```java
package org.kosta.webstudy18.controller;

import java.io.IOException;
import java.sql.SQLException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.kosta.webstudy18.model.MemberDAO;
import org.kosta.webstudy18.model.MemberVO;

/**
 * Servlet implementation class FindMemberByIdServlet
 */
@WebServlet("/FindMemberByIdServlet")
public class FindMemberByIdServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;       
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String id=request.getParameter("memberId");
		try {
			MemberVO vo=MemberDAO.getInstance().findMemberById(id);
			String path=null;
			if(vo==null) {
				path="findbyid-fail.jsp";
			}else {
				request.setAttribute("mvo", vo);
				path="findbyid-ok.jsp";
			}
			request.getRequestDispatcher(path).forward(request, response);
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
}
```

<br>

정보 조회 목적의 서블릿이기에 http 요청 방식으로 get 방식을 사용했습니다.<br>

아이디를 통해 정보가 있는지 없는지를 판단해서 결과에 따라 페이지를 다르게 전송해야 합니다.

`getParameter()`메서드를 활용해 전달받은 데이터를 `findMemberById()`메서드의 인자로 넣고 메서드를 실행해 반환받은 결과값을 `MemberVO` 타입의 `vo`에 할당합니다.

그래서 이 `vo`에 값이 들어있다면 id와 일치하는 정보가 있다는 의미이니 `setAttribute()`를 사용해 정보를 저장한 뒤 `findbyid-ok.jsp`로 전송시키고, null이 나온다면, `findbyid-fail.jsp`로 전송시킵니다.

조회 기능은 시스템에 변화가 생기지 않는(동작을 재반복해도 문제가 없는) 작업이기 때문에 `forward` 방식으로 이동합니다.

<br>

---

전달받은 id가 존재하는지 판단하는 `findMemberById()` 코드입니다.

```java
public MemberVO findMemberById(String id) throws SQLException{
    MemberVO vo=null;
    Connection con=null;
    PreparedStatement pstmt=null;
    ResultSet rs=null;
    try {
        con=DriverManager.getConnection(url,username,userpass);
        String sql="select name, address from member where id=?";
        pstmt=con.prepareStatement(sql);
        pstmt.setString(1, id);
        rs=pstmt.executeQuery();
        if(rs.next()) {
            vo=new MemberVO(id, null, rs.getString(1), rs.getString(2));
        }
    }finally {
        closeAll(rs, pstmt, con);
    }
    return vo;
}
```

해당 기능 역시 JDBC에서 많이 연습했기 때문에 자세한 설명은 생략하겠습니다.

<br>

---

이제 조회 결과에 따른 화면을 보여주는  `findbyid-ok.jsp`와 `findbyid-fail.jsp`를 확인하겠습니다.

조회 결과가 없는 경우가 간단하기 때문에 먼저 진행하도록 하겠습니다.

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
        <script type="text/javascript">
            alert("<%=request.getParameter("memberId")%> 아이디와 일치하는 회원정보가 없습니다!");
            location.href="index.jsp";
        </script>
    </body>
</html>
```

`forward`방식의 이동은 request가 유지되기 때문에 `getParameter`메서드로 id를 받아와 위와 같이 문구를 `alert()` 방식으로 출력해주고 다시 첫 페이지인 `index.jsp`로 이동시킵니다.

<br>

---

조회 결과 일치하는 회원 정보가 존재하는 경우입니다.

```jsp
<%@page import="org.kosta.webstudy18.model.MemberVO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
    <head>
	    <meta charset="UTF-8">
		<title>findbyid-ok</title>
		<link rel="stylesheet" type="text/css" href="css/home.css">
    </head>
	<body>
		<div class="container">
			<a href="index.jsp">Home</a>
			<%MemberVO vo=(MemberVO) request.getAttribute("mvo"); %>
			<h3>검색결과</h3>
			<table>
			    <thead>
			        <tr>
			            <th>아이디</th>
			            <th>이름</th>
			            <th>주소</th>
			        </tr>
			    </thead>
			    <tbody>
			        <tr>
			            <td><%=vo.getId() %></td>
			            <td><%=vo.getName() %></td>
			            <td><%=vo.getAddress() %></td>
			        </tr>
			    </tbody>
			</table>
		</div>
	</body>
</html>
```

`FindMemberServlet`에서 `setAttribute()`로 저장해둔 데이터를 `getAttribute()`를 사용해 불러와 변수에 담아서 활용했습니다.

테이블을 활용해 해당 회원의 정보를 출력했습니다.

<br>



<br>

<br>

---

> ## Step3

로그인 기능 구현입니다.

<img src="https://user-images.githubusercontent.com/70495425/136654104-7260db37-4d53-4993-8fdb-b2a79e11a168.png" alt="image" style="zoom:67%;" />

로그인에 실패할 경우 `login-fail.jsp` 에서 alert로 로그인 실패를 출력한 후 다시 `index.jsp`로 이동합니다.

로그인에 성공할 경우 바로 `index.jsp`로 이동하는데 로그인 폼 대신 "OO님 반갑습니다"라는 메세지와 로그아웃 링크를 보여줍니다.

<br>

로그인폼은 다음과 같습니다.

```jsp
<form action="LoginServlet" method="post">
    <input type="text" name="id" placeholder="아이디" required="required"><br>
    <input type="password" name="password" placeholder="패스워드" required="required"><br>
    <button>로그인</button>
</form>
```

아이디와 비밀번호는 주소창에 노출되어서는 안되기 때문에 post 방식으로 요청합니다.

<br>

`LoginServlet` 코드입니다.

```java
package org.kosta.webstudy18.controller;

import java.io.IOException;
import java.sql.SQLException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.kosta.webstudy18.model.MemberDAO;
import org.kosta.webstudy18.model.MemberVO;

/**
 * Servlet implementation class LoginServlet
 */
@WebServlet("/LoginServlet")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	    String id=request.getParameter("memberId");
	    String password=request.getParameter("memberPassword");
	    String url=null;
	    try {
            MemberVO vo=MemberDAO.getInstance().login(id, password);
            if(vo!=null) {
                request.getSession().setAttribute("mvo", vo);
                url="index.jsp";
            }else {
                url="login-fail.jsp";
            }
            response.sendRedirect(url);
        } catch (SQLException e) {
            e.printStackTrace();
        }
	}
}
```

조회 기능과 비슷하지만 로그인 기능은 시스템에 변화가 생기는 기능이기에 `forward`방식이 아닌 `redirect`방식으로 이동합니다. 그래서 데이터를 저장할 때에도 `session`을 활용해야 합니다. 

28번 라인에서의 `getSession()`메서드로 기존 세션을 리턴받아서 `setAttribute()`로 정보를 할당했습니다.

(`HttpSession session=request.getSession();`과 같이 선언한 뒤 `session.setAttribute();`를 작성한 것과 같습니다.)

<br>

위에서 이름을 미리 작성한 `login()`메서드를 작성하겠습니다. `MemberDAO`에서 구현하며 해당 기능만 확인하겠습니다.

```java
public MemberVO login(String id,String password) throws SQLException {
    MemberVO vo=null;
    Connection con=null;
    PreparedStatement pstmt=null;
    ResultSet rs=null;
    try {
        con=DriverManager.getConnection(url, username, userpass);
        String sql="select name,address from member where id=? and password=?";
        pstmt=con.prepareStatement(sql);
        pstmt.setString(1, id);
        pstmt.setString(2, password);
        rs=pstmt.executeQuery();
        if(rs.next())
            vo=new MemberVO(id,password,rs.getString(1),rs.getString(2));
    }finally {
        closeAll(rs, pstmt, con);
    }
    return vo;
}
```

로그인은 입력받은 아이디, 비밀번호가 DB의 데이터와 일치하는지 확인하는 기능을 구현해야 합니다.

위의 코드와 같이 sql문에서 `and`를 사용해 한 번에 구현해도 되고, 입력받은 아이디와 일치하는 데이터를 불러온 뒤 비밀번호의 일치 여부를 다시 판단할 수 도 있습니다.(_대부분의 상황이 그렇듯 다양한 구현이 가능합니다._)

일치하는 데이터가 있다면, 해당 객체의 정보를 `vo`에 할당해서 반환하는 기능입니다.

<br>

기능 구현을 마쳤으니, 화면을 구성하는 `login-fail.jsp`와 `index.jsp`를 확인하겠습니다.

로그인에 실패한 경우 간단히 작성할 수 있습니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>login-fail</title>
</head>
<body>
<script type="text/javascript">
	alert("로그인 되지 않았습니다! \n아이디 또는 비밀번호를 확인하세요");
	location.href="index.jsp";
</script>
</body>
</html>
```

위와 같이 로그인 실패 문구를 `alert`로 출력한 뒤 `index.jsp`로 이동합니다.

<br>

<br>

그런데 로그인에 성공한 경우 로그인 성공 페이지가 아닌 `index.jsp`로 바로 돌아가야 합니다.

실제로 저희가 사용하는 대부분의 사이트들은 로그인에 성공하더라도 이전 화면과 똑같은 첫 페이지로 이동하게 됩니다.

다만, 로그인에 성공한 뒤 보이는 화면이 이전과는 다른 양상을 보일텐데, 저희는 Scriptlet으로 조건문을 활용해서 표현해보도록 하겠습니다.

```jsp
{% raw %}
<%
MemberVO mvo = (MemberVO) session.getAttribute("mvo");
if (mvo == null) {
%>		
<form action="LoginServlet" method="post">
    <input type="text" name="id" placeholder="아이디" required="required"><br>
    <input type="password" name="password" placeholder="패스워드" required="required"><br>
    <button>로그인</button>
</form>
<%} else {%>
<%=mvo.getName() %>님 반갑습니다 <br>
<%}%>
{% endraw %}
```

위의 코드는 비로그인 상태에서 보여지는 화면입니다.

2번 라인의 코드에서는 세션에 저장해둔 정보를 불러와 사용자의 정보가 있는지 판단하며 3번 라인에서 조건문을 활용해 해닥 객체가 없다면 로그인을 할 수 있도록 로그인폼을 보여줍니다.

반환받은 객체가 null이 아니라면 로그인 상태라는 의미이기에 사용자의 이름과 함께 "OO님 반갑습니다"라는 문구를 보여줍니다.

<br>

다음은 로그아웃 기능입니다.

<img src="https://user-images.githubusercontent.com/70495425/136654139-4183591a-0311-42c6-af83-62381d3880fc.png" alt="image" style="zoom:67%;" />

로그아웃 기능은 로그인 상태를 해제해주기만 하면 되기 때문에 로그인 기능에 비해 상당히 간단합니다. 

`index.jsp`에서 로그인 상태에 있을 경우 로그아웃폼을 제공합니다.

```jsp
<%
} else {
%>
<%=mvo.getName() %>님 반갑습니다 <br>
<script type="text/javascript">
    function logout(){
        document.getElementById("logoutForm").submit();
    }
</script> 
<form action="LogoutServlet" method="post" id="logoutForm"></form>  
<a href="#" onclick="logout()">로그아웃</a>
<%
}
%>
```

사용자에게 데이터를 입력받아 전송해야 할 필요가 없기 때문에 a 태그로 링크를 걸었습니다.<br>

그런데 a href 링크는 get 요청 방식이기 때문에 로그인이나 로그아웃 기능에는 적합하지 않습니다.<br>

그래서 post 방식을 적용하기 위해 로그아웃 폼을 post방식으로 만들어두고, a 태그에서는 `onclick`속성으로 js 함수인 `logout()`에 연결시킵니다.<br>

`logout()`에서는 다시 `LogoutServlet`에 연결되는 로그아웃폼에 submit()되도록 작성합니다. 폼을 특정하기 위해 id를 만들어서 연결시켰습니다.

<br>

위와 같은 방식이 상당히 복잡해 보이기는 합니다. 처음부터 로그아웃 폼을 버튼으로 작성하면 간단하게 구현할 수도 있겠지만, 로그인이나 로그아웃 기능을 링크로도 url에 노출시키지 않고 작성할 수 있다는 점에 의미가 있는 것 같습니다.

<br>

마지막으로 `LogoutServlet`을 확인해보고 이번 글을 마치도록 하겠습니다.

```java
package org.kosta.webstudy18.controller;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/**
 * Servlet implementation class LogoutServlet
 */
@WebServlet("/LogoutServlet")
public class LogoutServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // request.getSession().invalidate();
        HttpSession session = request.getSession(false);
        if (session != null) {
            session.invalidate();
        }
        response.sendRedirect("index.jsp");
    }
}

```

21~24번 라인까지의 코드를 20번 라인과 같이 간단하게 구현할 수도 있습니다.<br>

그런데 자세히 살펴보면 똑같은 기능을 할 것 같지만 만약 로그인 상태에서 세션이 만료된다면 상황이 약간 달라지게 됩니다.<br>

20번 라인의 코드에서는 session이 만료되었을 때 `getSession()`으로 인해 새로운 세션이 생성되고, 새롭게 생성된 세션이 무효화 처리됩니다. 불필요한 작업을 하게 되는 것이지요.<br>

그런데 21~24번 라인은 `getSession(false)`를 사용해서 세션 객체에 null 값이 들어갈경우 무효화 작업을 생략하고 첫 화면으로 이동할 수 있게 됩니다.

세션 객체가 유지되고 있을 경우에만 무효화 처리를 진행하게 되는 것 입니다.

<br><br>

앞 부분에서 로그아웃을 링크로 구현하는 부분과도 비슷한 것 같은데, 사소한 차이이고 반드시 알아야 하는 부분은 아닌 것 같습니다.

하지만 개인적으로는 이런 작은 부분들도 세심하게 관찰하고, 깊게 생각하는 자세가 더 좋게 보이는 것 같습니다.

<br>

---

아직 구현을 못 한 기능들이 많은데 다음 글에서 구현해보도록 하겠습니다.

<br>

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

