---
title: "WEB(07) - Session&cookie - 수업 38일차"

date: 2021-09-28
last_modified_at: 2021-09-28


categories:
  - Java Web Programming

tags:
  - [kosta, session, cookie]

---



목차

* [Session과 Cookie](#session과-cookie)
  * [Cookie](#cookie)
  * [HttpSession(or Session)](#httpsession(or-session))
  * [예제1 Cookie](#예제1)
  * [예제2 Session](#예제2)



<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

이번 시간에는 Session과 Cookie에 대해 알아보겠습니다.<br><br>

두 용어의 설명에 앞서 HTTP의 특성에 대해 알아야 합니다. 

왜냐하면 Http Protocol의 Stateless한 특징의 해결책으로서 나온 개념이 Session과 Cookie이기 때문입니다.<br>

<br>

http 프로토콜은 **stateless 프로토콜**로 **효율성을 위해** 이전 요청의 **상태를 유지하지 않습니다.** 만약 모든 요청에 대한 정보를 유지한다면 요청이 늘어날수록 서버에 막대한 부담이 가게 됩니다.<br><br>

stateless한 특성 덕분에 서버에 부담은 줄 수 있는데, 이러한 상태에서는 로그인 정보나 장바구니에 물건을 담아둔 정보 등의 다양한 정보를 기록하지 못 하게 될 테고 페이지가 갱신될 때 마다 새로운 작업을 필요로 하게 됩니다.

<br>

이러한 불편함을 해소하기 위해 나온 개념이 바로 **Session**과 **Cookie**입니다.

<br>

<br>

---

> ## Session과 Cookie

: 사전적으로 `Session`은 (특정한 활동을 위한)시간 또는 기간이라는 뜻을 가지고 있습니다. 프로그래밍에서는 하나의 클라이언트가 프로그램을 시작해서 종료될 때 까지를 하나의 `Session`이라고 합니다.<br>

그렇지만 이번에 저희가 배울 `Session`은 흔히 `HttpSession`으로도 불리며 ***사용자 상태 정보를 서버 측에 저장하는 용도***로 사용합니다.<br>

그리고 Cookie는 ***사용자 상태 정보를 사용자 측에 저장하는 용도***로 사용합니다.<br><br>

Session의 관리와 활용을 이해하기 위해서는 Cookie에 대해서도 알아야 하기에 먼저 Cookie가 무엇인지 알아보겠습니다. 

<br><br><br>

> ## Cookie

​	: 서버가 브라우저(client)로 전송하는 작은 기록 정보 파일을 쿠키라고 합니다.<br>

**특징**

- **<mark style='background-color: #eafafa'>클라이언트 상태 정보(상대적으로 중요하지 않은 정보)를 클라이언트 측에 저장</mark>**
  (서버의 부담이 적다)
- 클라이언트는 서버에 요청 시 자신이 가진 데이터를 Http 요청 정보에 담아서 보내며 **key-value 형태로 관리**
- 저장 용량의 제한이 있다(4kb)
- 저장 데이터(쿠키정보)는 문자열만 가능
- dafault 유효 시간: 브라우저 실행 동안
- 사이트 별로 관리

<br>주요메소드

- `getName()`: 쿠키의 이름을 가져온다. String 리턴
- `getValue()`: 쿠키에 설정된 값을 가져온다. String 리턴
- `setValue(String value)`: 쿠키 값을 설정한다.
- `getMaxAge()`: 쿠키의 유효 시간을 가져온다.
- `setMaxAge(int sec)`: 쿠키의 유효 시간을 설정한다

<br>

쿠키 생성

- `javax.servlet.http.Cookie` 사용
- `Cookie c = new Cookie(“popup”,”no”);`
- `response.addCookie(c);` // 응답 시 쿠키가 전송됨<br>

![image](https://user-images.githubusercontent.com/70495425/135758412-cef9992a-b708-4570-93f4-8f7d578a6cd8.png){:.aligncenter}

클라이언트가 보내온 쿠키 정보 조회

- `Cookie[] cc = request.getCookies();` // 보내온 쿠키가 없으면 null 리턴<br>

![image](https://user-images.githubusercontent.com/70495425/135758543-a3046ced-c08a-4abf-b3b8-6048b8570353.png){:.aligncenter}

<br>

<br><br>

> ## HttpSession(or Session)

: Session 또는 HttpSession이라고 부르며 **<mark style='background-color: #eafafa'>사용자 상태 정보를 서버 측에 저장</mark>**하기 위해 사용합니다. <br>

특징

- 상대적으로 **중요한 정보를 저장** 
  ex)로그인, 로그아웃 등
- 저장 **용량** 및 **데이터 타입의 제한 X**
- WAS에 세션 유지 시간이 별도로 설정되어 있음
  (apache-tomcat의 경우 apache-tomcat/conf/web.xml에 30분으로 세션 유효 시간이 설정되어 있음 -> 변경 가능)

<br>

**세션유지기간**은 다음과 같이 세 가지 경우가 존재합니다.

- 지정한 유효시간(tomcat: 30분) 내에 새로운 요청이 없으면 세션 만료
- 브라우저 종료 시까지
- 로그아웃 실행 전 까지

<br>

**주요 메서드**

- `HttpServletRequset`의 `getSession()`: 기존 세션이 존재하면 세션 반환, 없으면 새로 생성
  (`request.getSession(true)`와 동일)
- `HttpServletRequset`의 `getSession(false)`: 기존 세션 존재하면 세션 반환, 없으면 null 반환

<br>

- `HttpSession`의 `setAttribute(name,value)`: 세션에 String 타입의 name과 Object 타입의 value를 할당해서 저장
- `HttpSession`의 `getAttribute(name)`: 세션에 저장된 attribute 정보를 name으로 검색해서 Object 타입의 value를 반환

<br>

-  `HttpSession`의 `invalidate()`: 세션을 무효화시킨다(로그아웃 시 사용)

<br><br><br>

> ## Session 관리

: 하나의 Session동안 사용자와 관련된 정보들을 계속해서 유지하도록(_하나의 세션동안 여러 번의 요청과 응답이 반복될 수 있습니다_) 관리하는 것을 말합니다.<br>

![image](https://user-images.githubusercontent.com/70495425/135760058-90767f4f-bfb2-4a82-94ed-3558ecfeea75.png){:.aligncenter}

<br><br>

![image](https://user-images.githubusercontent.com/70495425/135760081-cc16f082-d3b4-4432-864a-595e375dcbbd.png){:.aligncenter}

<br><br><br>

---

> ## 예제1

쿠키를 생성해서 저장하고 불러오는 예제를 진행해보겠습니다.<br><br>

전체 코드는 다음과 같습니다. (_html 내용은 jsp에서 작성해야 하지만 연습을 위해 하나의 자바 클래스 파일에서 진행했습니다. 이에 대한 설명은 생략하겠습니다_) 

```java
package step1;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.Date;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SetCookieServlet
 */
@WebServlet("/SetCookieServlet")
public class SetCookieServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out=response.getWriter();
		out.println("<!DOCTYPE html>");
		out.println("<html>");
		out.println("<head>");
		out.println("<meta charset='UTF-8'>");
		out.println("<title>home</title>");
		out.println("</head>");
		out.println("<body>");
		String name=getServletName();
		out.println("<h3>"+name+"</h3>");
		//시간 정보 
		String time=new Date().toString().replace(" ", "-");
		//쿠키 생성 
		Cookie cookie=new Cookie("time",time);
		//쿠키 유효시간을 설정 ( 만약 설정하지 않으면 브라우저를 끄면 쿠키도 사라진다 ) 
		cookie.setMaxAge(60);//60초 동안 유지 
		//쿠키를 클라이언트 측으로 전송 
		response.addCookie(cookie);
		out.println("cookie를 클라이언트 측으로 전달<br><br>");
		out.println("cookie에 저장한 time 정보:"+time);
		out.println("<br><br><a href=GetCookieServlet>GetCookieServlet에서 저장한 time cookie 정보확인</a>");
		out.println("</body>");
		out.println("</html>");
		out.close();
	}
}
```

<br>위의 코드를 실행해보면 다음과 같이 화면에 출력됩니다.

![image](https://user-images.githubusercontent.com/70495425/138582777-8f980017-4bab-4dca-878c-40907f0a6450.png)

33번 라인에서 `new Date()`메서드로 시간 정보를 생성한 뒤 35번라인에 쿠키를 생성해 방금 만든 시간 정보를 쿠키에 저장합니다.<br>

37번 라인에서는 쿠키 유효 시간은 60초로 설정해서 브라우저를 끄더라도 1분간 쿠키가 유지되도록 처리하였으며 해당 쿠키를 39번 라인에서 클라이언트 측으로 전송하게 됩니다.

<br>

방금 클라이언트 측으로 전송한 쿠키 정보를 확인해보기 위해 `GetCookieServlet` 의 코드를 작성해보겠습니다.

```java
package step2;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class GetCookieServlet
 */
@WebServlet("/GetCookieServlet")
public class GetCookieServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		out.println("<!DOCTYPE html>");
		out.println("<html>");
		out.println("<head>");
		out.println("<meta charset='UTF-8'>");
		out.println("<title>home</title>");
		out.println("</head>");
		out.println("<body>");
		String name = getServletName();
		out.println("<h3>" + name + "</h3>");
		Cookie[] cookies=request.getCookies();
		if(cookies!=null) {
			boolean flag=false;
			for(int i=0;i<cookies.length;i++) {
				if(cookies[i].getName().equals("time")) {
					out.println("client로부터 받아온 time cookie value:"+cookies[i].getValue());
					flag=true;
					break;
				}
			}
			if(flag==false)
				out.println("time 쿠키가 존재하지 않습니다");
		}else {
			out.println("쿠키가 존재하지 않습니다");			
		}
		out.println("<br><br><a href=SetCookieServlet>SetCookieServlet으로 이동</a>");		
		out.println("</body>");
		out.println("</html>");
		out.close();
	}
}
```

클라이언트 측의 요청을 통해 `cookie` 정보를 받아와 출력해서 확인해보는 코드입니다.<br>

33번 라인의 코드를 보시면 web application에 저장된 쿠키를 가져오는 `getCookies()`메서드를 사용해서 쿠키를 반환받는데, 쿠키 정보는 일반적으로 하나만 저장하지 않고 여러 개의 쿠키가 함께 저장됩니다.<br>

그래서 반환받을 때 역시 쿠키 타입의 배열을 선언해서 저장하도록 하고, 만약 저장된 쿠키가 하나도 존재하지 않는다면 `null` 값이 저장됩니다.<br>

그래서 35번 라인에서는 조건문으로 쿠키가 존재할 경우에 한해 반복문을 통해 쿠키 정보(`getName()` 활용)를 확인합니다.<br>

저희가 원하는 시간 정보가 맞을 경우 해당 정보(`getValue()` 활용)만을 출력하도록 합니다.<br>

만약 시간 정보 쿠키가 존재하지 않는다면 44번 라인과 같이 time 쿠키가 존재하지 않는다고 출력해주고, 쿠키 정보 자체가 존재하지 않을 경우에는 34번 조건문에 해당하지 않기에 45번 else문이 실행되며 쿠키가 존재하지 않는다는 문구를 출력합니다.

![image](https://user-images.githubusercontent.com/70495425/138582901-803c900a-93b0-4e24-b469-3d8c123789b0.png)

<br>

<br>


---

> ## 예제2

이번에는 세션 정보를 저장하고 불러오는 예제입니다.

<br>

설명이 필요치 않은 코드가 많아서 공부가 필요한 부분만 설명하도록 하겠습니다.
(전체 코드는 [깃허브 링크](#https://github.com/nogy21/TIL/tree/master/Kosta224/WEB/webstudy10-Session-Cookie/src/main/java)를 통해 확인하실 수 있습니다.)

<br>

앞에서 연습해본 쿠키 예제와 비슷한 방식으로 진행하겠습니다. <br>

먼저 볼 코드는 `SessionOneServlet` 파일입니다. 어노테이션을 사용했으며 `doGet()` 메서드를 작성했습니다. <br>

아래는 `doGet()` 메서드 내부 코드의 일부분입니다.

```java
String name=getServletName();
out.println("<h3>"+name+"</h3>");
HttpSession session = request.getSession();
out.println("session id: "+session.getId()+"<br>");
session.setAttribute("mvo", new MemberVO("java","1234","아이유","종로"));
out.println("세션에 회원 정보를 저장<br><br>");
out.println("<a href=SessionTwoServlet>SessionTwoServlet으로 이동</a>");
```

우선 `getServletName()`메서드를 통해 서블릿 이름을 확인했습니다.(_HttpServlet은 ServletConfig를 implements하기 때문에 바로 호출이 가능합니다_)<br>

3번 라인에서 `getSession()`메서드를 통해 세션 정보를 불러오는데 인자값을 작성하지 않았기에 세션 정보가 없다면 새로 생성해서 반환합니다.<br>

5번 라인에서는 세션에 `MemberVO` 객체를 할당했습니다.<br>

저장한 회원 정보를 7번 라인에서 제공한 링크로 이동해 확인해보도록 하겠습니다.

<br>

<br>

아래는 `SessionTwoServlet`의 일부 코드입니다.

```java
String name=getServletName();
out.println("<h3>"+name+"</h3>");
HttpSession session=request.getSession(false);
if(session!=null) {
    out.println("session id:"+session.getId()+"<br>");
    //회원 객체를 세션으로부터 리턴받아 회원의 name을 출력해본다
    //session의 getAttribute(name)을 이용
    MemberVO mvo=(MemberVO) session.getAttribute("mvo");
    out.println("회원이름:"+mvo.getName());
}else {
    out.println("세션이 존재하지 않습니다.");
}
out.println("<br><br><a href=SessionOneServlet>SessionOneServlet으로 이동</a>");
out.println("<br><br><a href=SessionThreeServlet>SessionThreeServlet으로 이동</a>");
```

이번에는 세션을 불러오는데 `getSession(false)`와 같이 인자로 `false`를 지정하여 기존 세션이 존재하면 기존 세션을 반환하고 그렇지 않으면 `null`을 반환하도록 했습니다.<br>

이를 통해 앞에서 저장한 세션 정보를 명확히 확인할 수 있습니다.<br>

세션 객체가 존재할 경우에만 저장한 정보를 받아와 확인해보고 아닐 경우 존재하지 않는다는 문구를 출력하게 했습니다.

<br>

<br>

다음으로 `SessionOneServlet`의 링크와 `SessionThreeServlet` 링크를 함께 제공해서 세션 정보를 저장하고 불러오고 무효화시키는 연습을 유기적으로 확인할 수 있도록 했습니다.

<br>

`SessionThreeServlet`에서 세션 무효화 처리를 적용해보겠습니다.

```java
String name=getServletName();
out.println("<h3>"+name+"</h3>");
HttpSession session=request.getSession(false);
if(session!=null) {
    session.invalidate();
    out.println("세션을 무효화시킴(session invalidate 처리)");
}else {
    out.println("세션이 존재하지 않습니다.");
}
out.println("<br><br><a href=SessionOneServlet>SessionOneServlet으로 이동</a>");
out.println("<br><br><a href=SessionTwoServlet>SessionTwoServlet으로 이동</a>");
```

이번에도 `getSession(false)`에서 `false` 인자를 할당해 세션이 존재할 경우에만 무효화 처리를 할 수 있도록 했습니다.<br>

세션 무효화 처리는 흔히 로그아웃에서 사용되는 기능으로 해당 세션을 무효화시킵니다.

`session.invalidate()`과 같이 `invalidate()` 메서드를 사용합니다.<br>

세션이 존재하지 않는다면 무효화 처리가 필요치 않으므로 조건문을 통해 불필요한 작업을 진행하지 않게끔 했습니다.

<br>

<br>

![session](https://user-images.githubusercontent.com/70495425/138583859-eba1fe4e-a2cd-4472-8479-0720753ed0fa.gif)

<br>

<br>

---

이것으로 오늘 복습은 마치도록 겠습니다.<br>

<br><br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}<br>

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

