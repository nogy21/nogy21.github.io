---
title: "WEB(13) - 미니프로젝트 - 회원관리 시스템(3) - 수업 46일차"

date: 2021-10-12
last_modified_at: 2021-11-13


categories:
  - Java Web Programming

tags:
  - [kosta, 미니프로젝트, 회원관리]

---

목차

* [파일 구조](#파일-구조)
* [Step6 회원가입](#step6)
  * [비밀번호 일치 여부 확인](#비밀번호-일치-여부-확인)
  * [아이디 중복 확인](#아이디-중복-확인)
    * [hidden](#hidden)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>



---

지난 글에 이어 미니프로젝트(회원관리 시스템)를 진행하겠습니다.<br>

<br>

이미 회원정보수정까지 구현을 마쳤습니다.

`아이디로 회원조회` -> `로그인`, `로그아웃 `-> `주소로 회원리스트조회` -> `회원정보수정` 	_완료_
-> `회원가입`(`아이디 중복확인`)

<br>

이번 글에서는 회원가입 기능을 구현해보겠습니다.

<br>
<br>

> ##  파일 구조

| MVC        | 파일 이름                                                    |
| ---------- | ------------------------------------------------------------ |
| Model      | `MemberVO`, `MemberDAO`                                      |
| View       | `css/home.css`<br />`index.jsp`(_비로그인시: ~~아이디로 회원조회폼, 로그인폼,~~ 회원가입링크<br/>로그인시: ~~아이디로 회원조회폼,~~ 주소로 회원리스트조회폼, ~~OO님(로그인폼 대체)~~, 로그아웃링크, 회원정보수정링크_)<br />~~아이디로 회원조회:~~ ~~`findbyid-ok.jsp`~~, ~~`findbyid-fail.jsp`~~<br/>~~로그인: `login-fail.jsp`에서 alert 후 `index.jsp`로 이동, 로그인 성공 시 `index.jsp`~~<br/>~~로그아웃: `index.jsp`에서 로그인 상태 시 링크 제공, 로그아웃 성공되면 다시 `index.jsp`로 이동~~<br/>~~주소로 회원리스트조회: `index.jsp`, 조회 결과: `findbyaddress-result.jsp`<br/>회원정보수정: `update-form.jsp`, `update-result.jsp`~~<br/>회원가입: `register-form.jsp`, `register-result.jsp`, `idcheck-ok.jsp`(팝업), `idcheck-fail.jsp`(팝업) |
| Controller | ~~`FindMemberByIdServlet`~~, ~~`LoginServlet`~~, ~~`LogoutServlet`~~, ~~`FindMemberListByAddressServlet`, `UpdateMemberServlet`~~, `RegisterMemberServlet`, `IdCheckServlet` |

<br><br>

---

> ## Step6

회원 가입 기능의 구조부터 살펴보도록 하겠습니다.

<br>

<img src="https://user-images.githubusercontent.com/70495425/137587524-366c6acd-37a9-4d81-8bf6-125c1a73597f.png" alt="image" style="zoom:80%;" />

<br>

위와 같은 구조로 만들 예정입니다.

첫 페이지인 `index.jsp`에서 회원가입 버튼 or 링크를 누르면 `register-form.jsp`로 이동하게 되고 회원 정보를 입력하게 됩니다.

그런데 아이디 중복 확인을 하지 않을 경우 회원가입이 진행되지 않도록 팝업창을 띄워줄 예정입니다. 

또한 비밀번호가 일치하지 않을 경우에도 알림창을 띄워서 회원가입이 되지 않도록 하며, 모두 정상적으로 입력한 뒤 가입 버튼을 누를 경우에만 `register-result.jsp`에서 회원 가입 결과 화면을 보여주고 다시 홈페이지로 돌아갈 수 있도록 링크를 제공하겠습니다.<br>

작업의 구분을 위해 초록색 선과 노란색 선으로 구조를 나눴는데, 순서 상으로는 아이디 중복 확인이 우선시 되어야 하지만 저희가 아직 배우지 않은 기능을 사용해야 하기 때문에 기본적인 회원가입 기능 부터 우선 구현해보도록 하겠습니다.

<br>

<br>

---

`index.jsp`에서는 간단하게 다음과 같이 회원가입 페이지로 이동할 수 있는 링크만 제공하도록 하겠습니다.

```jsp
<a href="register-form.jsp">회원가입</a>
```

<br>

`register-form.jsp`를 확인해보겠습니다.

```jsp
<%@page import="org.kosta.webstudy18.model.MemberDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>register-form</title>
		<link rel="stylesheet" type="text/css" href="css/home.css">
	</head>
    <body>
        <div class="container">
            <a href="index.jsp">Home</a>
            <hr>
              <script type="text/javascript">
                function checkId(){}
                function checkRegForm(){}
              </script>
            <form action="RegisterMemberServlet" method="post" onsubmit="return checkRegForm()">
              <input type="text" name="id" id="userId" placeholder="아이디" required="required">
              <input type="hidden" id="flag" value="">
              <button type="button" onclick="checkId()">중복 확인</button><br><br>
              <input type="password" name="password" id="passCheck" placeholder="비밀번호" required="required"><br><br>
              <input type="password" name="passwordConfirm" id="passConfirm" placeholder="비밀번호확인" required="required"><br><br>
              <input type="text" name="name" placeholder="이름" required="required"><br><br>
              <input type="text" name="address" placeholder="주소" required="required"><br><br>
              <button>가입하기</button>
            </form>
        </div>
    </body>
</html>
```

기본적인 form부터 작성하고, 해당 서블릿과 DAO 기능을 작성한 뒤에 다시 돌아와서 비밀번호 일치와 아이디 중복 확인 기능을 작성하도록 하겠습니다.

폼에 들어가는 기본 인풋 태그들의 내용은 다른 페이지와 크게 다를 바 없지만, `hidden`태그가 있는데, 이 부분 역시 아이디 중복 확인 기능에서 중요한 기능이므로 잠시 후에 설명을 하도록 하겠습니다.

<br>

<br>

`RegisterMemberServlet`의 코드입니다.

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
 * Servlet implementation class RegisterMemberServlet
 */
@WebServlet("/RegisterMemberServlet")
public class RegisterMemberServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        try {
            MemberVO vo = new MemberVO(request.getParameter("id"), request.getParameter("password"),
                    request.getParameter("name"), request.getParameter("address"));
            MemberDAO.getInstance().registerMember(vo);
            response.sendRedirect("register-result.jsp");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

<br>

회원가입에 입력한 내용들은 url 상에 노출되어서는 안되므로 `post`방식으로 요청을 합니다.

또한 `post` 방식이기에 인코딩을 `utf-8`로 설정합니다.

<br>

`MemberDAO` 클래스의 회원가입 기능을 호출해야 하는데, 사용자에게 입력받은 데이터를 전달해줘야 하기 때문에 `MemberVO` 클래스 객체를 생성해 `vo` 변수에 담아서 전달하고 등록 기능은 반환받을 데이터가 필요하지 않으니 데이터의 저장이나 전달 없이 `register-result.jsp`로 이동하도록 합니다.

이동 방식은 `redirect`로 합니다.

<br>

`registerMember()`의 코드는 다음과 같습니다.

```java
public void registerMember(MemberVO vo) throws SQLException {
    Connection con=null;
    PreparedStatement pstmt=null;
    try {
        con=DriverManager.getConnection(url,username,userpass);
        String sql="insert into member(id,password,name,address) values(?,?,?,?)";
        pstmt=con.prepareStatement(sql);
        pstmt.setString(1, vo.getId());
        pstmt.setString(2, vo.getPassword());
        pstmt.setString(3, vo.getName());
        pstmt.setString(4, vo.getAddress());
        pstmt.executeUpdate();
    }finally {
        closeAll(pstmt, con);
    }
}
```

<br>

<br>

결과 페이지도 간단히 작성해보겠습니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>register-result</title>
		<link rel="stylesheet" type="text/css" href="css/home.css">
	</head>
	<body>
		<div class="container">
            <a href="index.jsp">Home</a>
            <hr>
            <h3>회원가입을 축하드립니다!</h3>
        </div>
	</body>
</html>
```

<br>

<br>

이렇게 기본적인 기능들은 구현을 마쳤습니다.

이전 기능들에서 작성해본 부분들이었으며, 이제 비밀번호 일치 여부와 아이디 중복 확인 순으로 남은 기능들을 구현해보도록 하겠습니다.

<br>

<br>

---

> ### 비밀번호 일치 여부 확인

비밀번호 일치 여부를 확인하는 기능은 javascript를 사용해서 구현하도록 하겠습니다.<br>

앞에 기본 구조를 작성한 `register-form.jsp`로 돌아가보겠습니다.<br>

form은 작성을 마쳤고 `script`에 `function checkId()`과 `function checkRegForm()`를 내용만 비운 채로 작성해둔 상태였습니다.<br>

여기서 `checkRegForm()`에 패스워드가 일치하지 않을 경우 알림창을 보여주도록 하겠습니다.<br>

```javascript
function checkRegForm(){
    if(document.getElementById("passCheck").value!=document.getElementById("passConfirm").value){
        alert("패스워드가 일치하지 않습니다!");
        return false;
    }
}
```

조건문을 사용해서 사용자가 비밀번호에 입력한 `value`와 비밀번호확인에 입력한 `value`의 일치 여부를 판단하면 되는 간단한 기능입니다.

<br>

<br>

---

> ### 아이디 중복 확인

아이디 중복 확인은 미리 말씀드렸듯이 지금까지 사용한 기능만으로는 구현이 어렵습니다.<br>

`hidden` 태그를 사용해서 작업의 수행 여부를 저장하는데, 우선 이 기능에 대해 가볍게 살펴보도록 하겠습니다.

<br>

> #### hidden

: `hidden` 태그는 **사용자 화면에 보이지 않는 속성**입니다. 서버로 특정 `name`과 `value`를 할당해 전달하고자 할 때 사용을 하는데, javascript로 특정 정보를 제어하고자 할 때에도 사용하곤 합니다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>hidden tag</title>
</head>
<body>
	<form action="step14-hidden-action.jsp">
		<input type="hidden" name="command" value="insert">
		<button>전송1</button>
	</form>
	<hr>
	<form action="step14-hidden-action.jsp">
		<input type="hidden" name="command" value="delete">
		<button>전송2</button>
	</form>
</body>
</html>
```

위와 같이 폼을 작성하면 다음 그림과 같이 버튼만 화면에 노출됩니다.

![image](https://user-images.githubusercontent.com/70495425/137591448-5b4c2a86-f014-4755-982a-f6d85d0af787.png){:.aligncenter}

그렇지만 전송 버튼을 누르게 될 경우 아래와 같이 `hidden`태그 안에 미리 작성해둔 `value`의 값이 출력되는 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/70495425/137591475-d8cf6820-c4f6-443b-a3b1-d6ed4cf6a611.png){:.aligncenter}

<br><br>

이 기능을 이용해서 아이디 중복 확인이 동작될 경우에만 hidden 상태를 변경하도록 합니다.

그래서 기존 회원가입 폼에서 `<input type="hidden" id="flag" value="">`와 같이 `hidden` 태그를 작성했었습니다.

`hidden`의 `id` 값으로 `flag`를 지정하고 `value`는 공백으로 기입했고, 인증 될 경우 `flag`에 인증받은 아이디를 할당해서 중복 확인 여부를 판단합니다.

<br>

추가적으로 `focus` 기능과 인증받지 못 한 경우 값을 초기화 할 수 있는 기능을 추가하도록 하고, 위의 기능들을 사용할 때 팝업 기능을 사용한다고 했는데 이 역시 javascript의 기능을 활용해서 팝업창을 띄워보도록 하겠습니다.<br>

팝업창을 띄우는 기능은 `window.open()`을 사용하며, `open()`의 인자로 팝업창을 띄울 주소와, 이름, 크기를 지정해줍니다.<br>

다음과 같이 작성할 수 있습니다.

`window.open("IdCheckServlet?id="+idComp.value,"mypopup","width=400, height=600, left=400, top=400, resizable = yes");`

<br>

스크립트 내의 기능을 확인해보겠습니다.

```javascript
function checkId(){
    let idComp=document.getElementById("userId");
    if(idComp.value==""){
        alert("아이디를 입력하세요!");
        idComp.focus();
    }else{
        window.open("IdCheckServlet?id="+idComp.value,"mypopup","width=400, height=600, left=400, top=400, resizable = yes");
    }
}
function checkRegForm(){
    if(document.getElementById("flag").value!=document.getElementById("userId").value){
        alert("아이디 중복확인이 필요합니다!");
        return false;
    }
    if(document.getElementById("passCheck").value!=document.getElementById("passConfirm").value){
        alert("패스워드가 일치하지 않습니다!");
        return false;
    }
}
```

위에서부터 천천히 살펴보면, 아이디 체크 기능으로 공란일 경우 아이디를 입력하라는 알림과 함께 아이디 입력 칸에 포커스를 해줍니다.<br>

공란이 아닐 경우 팝업을 띄우면서 쿼리 스트링을 이용하여 `userId`를 전달해주고, 전달한 아이디의 중복 확인 여부를 확인합니다.<br>

그리고 `checkRegForm()`에서 작성한 아이디 중복 확인 알림은 사용자가 아이디 중복확인을 한 뒤 회원가입 버튼을 누르기 전에 아이디를 변경한 상황에 대비해서 사용자가 다시 아이디 중복 확인을 받도록 하기 위해 작성한 코드입니다.

<br>

이제 아이디 중복 확인 여부를 확인하는 `IdCheckServlet`을 확인해보겠습니다.

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

/**
 * Servlet implementation class IdCheckServlet
 */
@WebServlet("/IdCheckServlet")
public class IdCheckServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        String id = request.getParameter("id");
        try {
            boolean flag = MemberDAO.getInstance().idcheck(id);
            String path = null;
            if (flag) {
                path = "idcheck-fail.jsp";
            } else {
                path = "idcheck-ok.jsp";
            }
            request.setAttribute("id", id);
            request.getRequestDispatcher(path).forward(request, response);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

전달받은 아이디를 저장하고, `MemberDAO`에서 작성할 `idcheck()`메서드의 인자로 다시 전달합니다.

<br>

`idcheck`메서드에서는 전달받은 아이디와 일치하는 정보가 DB에 있을 경우 `true`를 반환하고, 존재하지 않을 경우 `false`를 반환하도록 하겠습니다.

그래서 `IdCheckServlet`에서는 반환 결과에 따라 결과 페이지를 다르게 하여 `forward`방식으로 이동시키도록 하며 해당 결과 페이지에서 id값을 사용할 수 있도록 `setAttribute()` 메서드로 `id`값을 저장했습니다.

<br>

<br>

`idcheck()`메서드를 확인해보겠습니다.

```java
public boolean idcheck(String id) throws SQLException {
    boolean flag=false;
    Connection con=null;
    PreparedStatement pstmt=null;
    ResultSet rs=null;
    try {
        con=DriverManager.getConnection(url,username,userpass);
        String sql="select count(*) from member where id=?";
        pstmt=con.prepareStatement(sql);
        pstmt.setString(1, id);
        rs=pstmt.executeQuery();
        if(rs.next()&&rs.getInt(1)==1) {
            flag=true;
        }
    }finally {
        closeAll(pstmt, con);
    }
    return flag;
}
```

sql문으로 `select count(*) from member where id=?`과 같이 작성하여 입력한 아이디와 일치한 정보가 있을 경우 `count`값이 1이 되도록 하고 없을 경우에는 0이 나오게 됩니다.<br>

그래서 조건문을 사용해서 결과가 존재하고 첫 번째 값이 1인 경우(일치할 경우) `flag`에 `true`를 할당해서 반환하도록 하고, 기존의 `IdCheckServlet`에서는 `true`를 리턴받을 경우 `path`에 `idcheck-fail.jsp`를 저장해 아이디 중복 알림을 보여주도록 합니다.<br>

아래는 `idcheck-ok.jsp`와 `idcheck-fail.jsp`의 코드입니다.<br>

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>idcheck-ok</title>
		<link rel="stylesheet" type="text/css" href="css/home.css">
        <script type="text/javascript">
        opener.document.getElementById("flag").value="<%=request.getParameter("id")%>";
        function closePopup(){
            opener.document.getElementById("passCheck").focus();
            self.close();
        }
        </script>
	</head>
	<body bgcolor="yellow" onunload="closePopup()">
		<div class="container">
            <%=request.getParameter("id") %> 아이디는 사용 가능합니다!
            <input type="button" value="확인" onclick="closePopup()">
        </div>
	</body>
</html>
```

<br>

현 페이지에서는 아이디 중복 확인을 한 아이디를 10번 라인의 코드와 같이 `hidden`의 `value`값에 저장하여 사용자가 회원가입 버튼을 누를 때 다시 사용해서 값을 비교할 수 있도록 합니다.

<br>

그리고 11번 라인의 `closePopup()` 함수를 보시면, 팝업창에서는 해당 페이지를 오픈한 부모창으로부터 데이터를 가져오기 위해 `opener`라는 기능을 사용합니다. `opener` 객체는 자신을 띄운 웹페이지를 제어할 수 있습니다.

<br>

그래서 12번 라인에서 패스워드를 입력할 인풋에 포커스 기능을 사용할 수 있고, 자기 자신을 가리키는 `self` 키워드를 사용해서 `close()`로 창을 닫을 수 있도록 합니다.

<br>

그리고 추가적으로 `closePopup()` 기능에 `onunload` 속성을 사용해서 사용자가 확인 버튼이 아닌 웹페이지 우측 상단 x 표시의 창닫기를 할 경우에도 `closePopup()` 기능이 적용되도록 했습니다.

<br>

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
	<head>
        <meta charset="UTF-8">
        <title>idcheck-fail</title>
        <link rel="stylesheet" type="text/css" href="css/home.css">
        <script type="text/javascript">
        	opener.document.getElementById("flag").value="";
	        function closePopup(){
	        	let idComp=opener.document.getElementById("userId");
	        	idComp.value="";
	        	idComp.focus();
	            self.close();
	        }
        </script>
	</head>
	<body>
		<div class="container">
            <%=request.getParameter("id") %> 아이디는 중복! 사용 불가합니다!!
            <input type="button" value="확인" onclick="closePopup()">
		</div>
	</body>
</html>
```

<br>

인증받은 아이디가 아닌 경우에는 `idcheck-ok.jsp`와는 반대로 `opener`를 활용해서 `flag`와 아이디 입력값에 공백을 할당하고, 유저가 다시 아이디 입력칸에 바로 입력을 할 수 있도록 `focus()` 기능을 적용해줍니다.

<br>

<br>

---

이것으로 웹 수업에 첫 미니 프로젝트를 마치겠습니다.

<br>



<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

