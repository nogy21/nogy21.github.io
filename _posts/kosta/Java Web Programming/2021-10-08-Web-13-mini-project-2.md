---
title: "WEB(13) - 미니프로젝트 - 회원관리 시스템(2)"

date: 2021-10-10
last_modified_at: 2021-10-10


categories:
  - Java Web Programming

tags:
  - [kosta, 미니프로젝트, 회원관리]

---

목차

* [파일 구조](#파일-구조)
* [Step4 주소로 회원 리스트 조회](#step4)
* [Step5 회원 정보 수정](#step5)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>



---

지난 글에 이어 미니프로젝트(회원관리 시스템)를 진행하겠습니다.<br>

<br>

구현 순서를 보면 아래와 같은데, 이미 이전 글에서 로그인, 로그아웃 기능까지 구현을 마쳤습니다.

`아이디로 회원조회` -> `로그인`, `로그아웃 `-> `주소로 회원리스트조회` -> `회원정보수정` -> `회원가입`(`아이디 중복확인`)

<br>

이번 글에서는 주소로 회원 리스트 조회와 회원정보수정 기능을 구현해보겠습니다.

<br>
<br>

> ##  파일 구조

| MVC        | 파일 이름                                                    |
| ---------- | ------------------------------------------------------------ |
| Model      | `MemberVO`, `MemberDAO`                                      |
| View       | `css/home.css`<br />`index.jsp`(_비로그인시: ~~아이디로 회원조회폼, 로그인폼,~~ 회원가입링크<br/>로그인시: ~~아이디로 회원조회폼,~~ 주소로 회원리스트조회폼, ~~OO님(로그인폼 대체)~~, 로그아웃링크, 회원정보수정링크_)<br />~~아이디로 회원조회:~~ ~~`findbyid-ok.jsp`~~, ~~`findbyid-fail.jsp`~~<br/>~~로그인: `login-fail.jsp`에서 alert 후 `index.jsp`로 이동, 로그인 성공 시 `index.jsp`~~<br/>~~로그아웃: `index.jsp`에서 로그인 상태 시 링크 제공, 로그아웃 성공되면 다시 `index.jsp`로 이동~~<br/>주소로 회원리스트조회: `index.jsp`, 조회 결과: `findbyaddress-result.jsp`<br/>회원정보수정: `update-form.jsp`, `update-result.jsp`<br/>회원가입: `register-form.jsp`, `register-result.jsp`, `idcheck-ok.jsp`(팝업), `idcheck-fail.jsp`(팝업) |
| Controller | ~~`FindMemberByIdServlet`~~, ~~`LoginServlet`~~, ~~`LogoutServlet`~~, `FindMemberListByAddressServlet`, `UpdateMemberServlet`, `RegisterMemberServlet`, `IdCheckServlet` |

<br><br>

---

> ## Step4

주소로 회원 리스트 조회 기능입니다.<br>

사실 이 기능은 실제 웹사이트에서는 볼 수 없을 법한 기능이지만, 리스트를 반환받는 연습을 해보기 적합하기에 구현해보도록 하겠습니다.<br>

<img src="https://user-images.githubusercontent.com/70495425/136699427-8707f6bb-7419-49f1-91c6-8ae08ecafc1c.png" alt="image" style="zoom:80%;" />

`index.jsp`에서는 해당 기능만 확인하겠습니다.

```jsp
<form action="FindMemberListByAddressServlet">
    <input type="text" name="address" required="required" placeholder="주소">
    <button>조회</button>
</form>
```

해당 기능은 로그인 상태에서만 작동하도록 해야 합니다. 그래서 `index.jsp`에서 연결된 세션 정보가 있을 경우(`vo!=null`인 경우)에만 보여지도록 작성했습니다.<br>

단순한 조회 기능이기에 get 방식을 사용해서 `FindMemberListByAddressServlet`으로 요청하며 코드는 다음과 같습니다.

```java
package org.kosta.webstudy18.controller;

import java.io.IOException;
import java.sql.SQLException;
import java.util.ArrayList;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.kosta.webstudy18.model.MemberDAO;
import org.kosta.webstudy18.model.MemberVO;

/**
 * Servlet implementation class FindMemberListByAddressServlet
 */
@WebServlet("/FindMemberListByAddressServlet")
public class FindMemberListByAddressServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        String address = request.getParameter("address");
        try {
            ArrayList<MemberVO> list = MemberDAO.getInstance().findMemberListByAddress(address);
            request.setAttribute("memberList", list);
            request.getRequestDispatcher("findbyaddress-result.jsp").forward(request, response);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

jsp에서 사용자가 입력한 주소를 `getParameter()`메서드를 사용해서 `address` 변수에 할당하고, `findMemberListByAddress()`메서드의 인자값으로 전달하며 전달한 주소에 해당하는 회원 정보를 반환받습니다.<br>

그런데 해당 주소와 일치하는 회원 정보는 여러 명일 수 있기 때문에 `ArrayList`를 사용해서 회원 정보를 반환받도록 합니다.<br>

회원 정보의 유무와 관계 없이 일단 결과 페이지로 동일하게 `forward`방식으로 이동하고, 그 전에 `findMemberListByAddress()` 메서드로 반환받은 리스트를 `setAttribute()`메서드로 저장합니다. `forward`방식의 이동이기에 request는 유지됩니다.<br>

<br>

`findMemberListByAddress()`메서드의 코드를 확인한 뒤 결과 페이지를 보도록 하겠습니다.

```java
public ArrayList<MemberVO> findMemberListByAddress(String address) throws SQLException {
    ArrayList<MemberVO> list=new ArrayList<MemberVO>();
    Connection con=null;
    PreparedStatement pstmt=null;
    ResultSet rs=null;
    try {
        con=DriverManager.getConnection(url,username,userpass);
        String sql="select id, name from member where address=?";
        pstmt=con.prepareStatement(sql);
        pstmt.setString(1, address);
        rs=pstmt.executeQuery();
        while(rs.next()) {
            list.add(new MemberVO(rs.getString(1), null, rs.getString(2),address));
        }
    } catch (SQLException e) {
        e.printStackTrace();
    }finally {
        closeAll(rs, pstmt, con);
    }
    return list;
}
```

<br>

<br>

결과 페이지인 `findbyaddress-result.jsp`의 코드입니다.

```jsp
<%@page import="java.util.ArrayList"%>
<%@page import="org.kosta.webstudy18.model.MemberVO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>findbyaddress-result</title>
		<link rel="stylesheet" type="text/css" href="css/home.css">
	</head>
	<body>
		<div class="container">
			<a href="index.jsp">Home</a>
			<h3><%=request.getParameter("address")%>에 사는 회원리스트</h3>
			<hr>
			<%
			    @SuppressWarnings("unchecked")
		        ArrayList<MemberVO> list= (ArrayList<MemberVO>)request.getAttribute("memberList"); 
			%>
			<table>
			    <thead>
			        <tr>
			            <th>아이디</th>
			            <th>이름</th>
			        </tr>
			    </thead>
			    <tbody>
			        <%for(int i=0;i<list.size();i++){ %>
			        <tr>
			            <td><%=list.get(i).getId()%></td>
			            <td><%=list.get(i).getName()%></td>
			        </tr>
			        <%} %>
			    </tbody>
			</table>
		</div>
	</body>
</html>
```

전에 저장한 정보를 불러오기 위해 `getAttribute()`를 사용합니다. <br>

그런데 `Object` 타입을 반환하는 `getAttribute()`를 사용하면 타입 캐스팅이 필요합니다. 예를 들면 이전에는 스트링 타입으로의 형변환을 `(String)request.getAttribute("")`와 같은 형식으로 사용했습니다.<br>

이번에는 `ArrayList` 타입의 배열을 사용했기 때문에 마찬가지로 `(ArrayList<MemberVO>)` 타입으로 Object down casting을 했는데 `Type safety: Unchecked cast from Object to ArrayList<MemberVO>`와 같은 비확인 경고를 마주하게 됩니다.<br>

여기에서 사용해주는 코드가 `@SuppressWarnings("unchecked")` 입니다. 단순히 "unchecked" 문구를 무시하게 해주는 코드인데, 실행에 아무런 영향을 주지 않기에 이와 같이 처리를 해주었습니다.

다른 방법으로는 wrapper class를 생성해주는 방법이 있는데, 다소 복잡한 과정인데다 wrapper class에 대한 개념을 제대로 배우지 않은 시점이므로 생략하고 이런 게 있다는 정도로만 하고 넘어가도록 하겠습니다.

<br>

출력은 테이블 형식으로 보여지도록 했는데, 리스트에 null 값이 들어있을 경우에는 빈 테이블이 보여집니다.

<br>

<br>

---

> ## Step5

회원 정보 수정 기능입니다.

![image](https://user-images.githubusercontent.com/70495425/136699659-b1afb83b-f2df-4249-b4c9-d41c7bbe40a3.png){:.aligncenter}

`index.jsp`에서 세션에 저장된 정보가 있을 경우(로그인 상태) 회원정보수정 링크가 보여지도록 합니다.

링크를 누르면 `update-form.jsp`에서 회원 정보를 수정할 수 있는 입력 폼이 제공되고, 여기에서 입력된 정보는 `UpdateMemberServlet`으로 가서 model과 연동해(`update() 메서드 호출`) 해당 회원의 정보를 수정하게 합니다.<br>

수정을 마치면 `update-result.jsp`에서 수정 결과를 보여주고 다시 `index.jsp`로 이동할 수 있도록 합니다.

<br>

<br>

`index.jsp`부터 확인해보겠습니다.

```jsp
<a href="update-form.jsp">회원정보수정</a>
```

`index.jsp`에서의 회원 정보 수정은 링크로 제공하며, 여기에서는 입력할 데이터가 없기 때문에 간단하게 입력 폼을 제공하는 `update-form.jsp`로 이동할 수 있도록 합니다.

<br>

```jsp
<%@page import="org.kosta.webstudy18.model.MemberVO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<link rel="stylesheet" type="text/css" href="css/home.css">
		<title>update-form</title>
	</head>
	<body>
		<div class="container">
	        <a href="index.jsp">Home</a>
	        <hr>
	        <%
	        MemberVO vo= (MemberVO)session.getAttribute("mvo");
	        if(vo!=null){
	        %>
			<form action="UpdateMemberServlet" method="post">
			  <input type="text" name="id" value="<%=vo.getId() %>" readonly="readonly"><br>
			  <input type="text" name="password" value="<%=vo.getPassword() %>" required="required"><br>
			  <input type="text" name="name" value="<%=vo.getName() %>" required="required"><br>
			  <input type="text" name="address" value="<%=vo.getAddress() %>" required="required"><br>
			  <button class="button">회원정보수정</button>
			</form>
			<%
			}else {
                response.sendRedirect("index.jsp");
			}
			%>
		</div>
	</body>
</html>
```

혹시라도 세션 만료로 세션 객체와의 연결이 해제된 경우, 로그인이 해제된 상태이기 때문에 조건문을 사용해서 회원 정보를 수정하지 못하도록 바로 `index.jsp`로 돌아가도록 만듭니다.

<br>

세션을 통해 `MemberVO`의 인스턴스 정보를 불러올 수 있다면 사용자에게 `password`,`name`,`address`를 수정할 수 있는 폼을 제공합니다.<br>

그런데 `id`는 수정되면 안 되지만 보이기는 해야 되기 때문에 `readonly` 속성을 사용해서 사용자가 수정할 수 없도록 만들어줍니다.<br>

사용자가 입력을 모두 마치면 해당 정보가 url 상에 노출되지 않도록 `post`방식으로 `UpdateMemberServelt`으로 전송합니다.

<br>

---

```java
package org.kosta.webstudy18.controller;

import java.io.IOException;
import java.sql.SQLException;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import org.kosta.webstudy18.model.MemberDAO;
import org.kosta.webstudy18.model.MemberVO;

/**
 * Servlet implementation class UpdateMemberServlet
 */
@WebServlet("/UpdateMemberServlet")
public class UpdateMemberServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	    HttpSession session=request.getSession(false);
	    if(session==null||session.getAttribute("mvo")==null) {
	        response.sendRedirect("index.jsp");
	        return;
	    }
	    request.setCharacterEncoding("utf-8");
	    String id=request.getParameter("id");
	    String password=request.getParameter("password");
	    String name=request.getParameter("name");
	    String address=request.getParameter("address");
	    try {
		    MemberVO vo=new MemberVO(id,password,name,address);
		    MemberDAO.getInstance().updateMember(vo);
		    MemberDAO.getInstance().updateMember(vo);
		    session.setAttribute("mvo", vo);
		    response.sendRedirect("update-result.jsp");
        } catch (SQLException e) {
		    e.printStackTrace();
        }
	}
}
```

23번 라인을 보시면 `redirect` 전송을 위해 미리 `session` 객체를 불러오는데 `getSession()`메서드의 인자값으로 `false`를 지정합니다.

전 페이지와 마찬가지로 혹시 수정 페이지에서 세션 연결이 해제된 경우를 대비한 조치입니다.<br>

24번 라인을 보시면 세션이 null이거나 세션에서 불러온 `MemberVO`의 인스턴스 정보를 저장한 `getAttribute("mvo")`의 결과값이 null인 경우 `index.jsp`로 이동하도록 하고, 메서드 실행이 종료될 수 있도록 `return;`을 선언합니다.<br>

`return;`을 사용하지 않는다면 `else` 구문으로 아래 전체 코드를 감싸줄 수 있습니다.

<br>

세션 연결이 해제되지 않고, 사용자가 입력한 수정 정보가 존재한다면 우선 `post`방식은 요청 정보에 대한 별도의 한글 처리가 필요하므로 `setCharacterEncoding()`메서드로 `utf-8` 설정을 맞춰줍니다.<br>

그리고 사용자가 입력한 값을 `updateMember()` 메서드의 인자로 넘겨줘서 DB와 연동해 회원 정보를 수정하도록 합니다.

<br>

수정된 정보를 세션에 새롭게 저장하고, `update-result.jsp`로 이동하게 됩니다.

`updateMember()` 메서드를 먼저 살펴보고 결과 페이지를 확인하겠습니다.

```java
public void updateMember(MemberVO vo) throws SQLException {
    Connection con=null;
    PreparedStatement pstmt=null;
    try {
        con=DriverManager.getConnection(url,username,userpass);
        String sql="update member set password=?, name=?, address=? where id=?";
        pstmt=con.prepareStatement(sql);
        pstmt.setString(1, vo.getPassword());
        pstmt.setString(2, vo.getName());
        pstmt.setString(3, vo.getAddress());
        pstmt.setString(4, vo.getId());
        pstmt.executeUpdate();
    } finally {
        closeAll(pstmt, con);
    }
}
```

<br>

<br>

결과 페이지인 `update-result.jsp`의 코드입니다.

```jsp
<%@page import="org.kosta.webstudy18.model.MemberVO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>update-result</title>
		<link rel="stylesheet" type="text/css" href="css/home.css">
	</head>
	<body>
		<div class="container">
			<a href="index.jsp">Home</a>
			<hr>
			<h3>회원정보 수정완료</h3>
			<br>
			<%
			MemberVO vo= (MemberVO)session.getAttribute("mvo");
			%>
			아이디: <%=vo.getId() %><br>
			이름: <%=vo.getName() %><br>
			주소: <%=vo.getAddress() %><br>
		</div>
    </body>
</html>
```

세션에 저장한 정보를 불러와서 출력해주는 정도로 간단하게 구현했습니다.

<br>

<br>

---

이제 회원가입 기능만 남았습니다.

아이디 중복 확인과 비밀번호 확인을 같이 해야 등록이 되도록 처리해줘야 하며, 아이디 중복 확인은 아직 배우지 않은 팝업 기능을 사용해서 구현할 예정입니다.

<br>

<br>

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

