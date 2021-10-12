---
title: "WEB(05) - Servlet - 수업 36일차"

date: 2021-09-24
last_modified_at: 2021-09-26


categories:
  - Java Web Programming

tags:
  - [kosta, servlet]

---



목차

* [Web Architecture](#web-architecture)
* [Servlet 계층구조의 이해](#servlet-계층구조의-이해)
* [Servlet LifeCycle](#servlet-lifecycle)
* [ServletConfig & ServletContext](#servletconfig-&-servletcontext)
  * [Initialization Parameters](#initialization-parameters)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> ## Web Architecture

지금까지 client, WAS, database에 대해 배웠었습니다. 오전 수업에서는 이 관계를 [웹의 구조(관련 글 작성 예정)](../)를 바라보는 관점에서 정리하는 시간을 가졌습니다.

![image](https://user-images.githubusercontent.com/70495425/134761586-8f27208f-796a-4e80-9fed-b9914b218ad3.png){:.aligncenter}<br><br>



---

> ### Servlet 계층 구조의 이해

이번에는 서블릿의 계층 구조를 알아보고 이러한 구조의 장점은 무엇인지 알아보도록 하겠습니다.

![servlet-hierarchy](https://user-images.githubusercontent.com/70495425/134604156-d0344b5a-94e8-4e92-b3c1-6a5d514ac56a.jpg){:.aligncenter}

서블릿의 계층 구조는 위와 같습니다.<br>

Interface가 위쪽에 있고 아래쪽에서는 다양한 interface들을 implements하는 구조로 이루어져 있습니다.<br>



API를 통해 보다 자세히 살펴보도록 하겠습니다.(java 수업 때는 JavaSE API를 참고했지만 DB, Servlet 부터는 JavaEE API를 참고합니다)<br>

javax.servlet 패키지에 보시면 Servlet Interface가 있습니다.

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/134769429-fc623fb2-c6e7-4338-a1ea-c5ab1fccaf3b.png" alt="image" style="zoom:80%;" /></p>

그림의 아래부분을 보시면 모든 서블릿들이 반드시 구현하도록 강제하는 메소드를 정의한다고 합니다.<br>

왜 모든 서블릿이 이 인터페이스를 반드시 구현해야 하는지 살펴보면 서블릿을 초기화하는 메서드들을 정의해 놓은 인터페이스이기 때문입니다.<br>

클라이언트의 요청에 대해 서비스하고, 서버로부터 서블릿을 제거하기 위해서는 서블릿을 초기화해야 합니다. 

Servlet 인터페이스는 이러한 초기화 작업을 하는 메서드를 정의해놓은 최상위 인터페이스입니다.(이 과정은 아래에 life-cycle 내용에서 자세히 설명드리도록 하겠습니다)<br>

<br>

처음의 계층구조에서 확인할 수 있는데 저희는 GenericServlet 또는 HttpServlet으로 Servlet 인터페이스를 구현할 수 있습니다.<br>

특징을 정리해보면 아래의 그림과 같습니다.

![image](https://user-images.githubusercontent.com/70495425/134770189-01861b02-a4c4-4b97-99d9-25d421837e7c.png){:.aligncenter}



이제 다시 Servlet 계층 구조를 생각해보면 interface들이 상위에 있고 중간에 abstract class가 있다는 것은 알겠습니다.<br> 그렇다면 왜 구현체들을 바로 만들지 않고 interface 중심의 설계를 했을까요?<br>

자바 수업에서 JDBC 내용을 배울 때, 클라이언트가 구현부를 모르더라도 다양한 DB 벤더사의 제품들을 JDBC interface를 통해 다룰 수 있다고 배운 적이 있습니다. 

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/134770549-e340266a-21ad-400e-bb74-91b9b993d1c1.png" alt="image" style="zoom:60%;" /></p>

(수업 내용을 캡쳐한 자료입니다)

<br>



Servlet API 역시 Servlet Interface(Servlet, ServletRequest, ServletResponse, HttpSession 등)들을 중심으로 정의하여 실제 구현 클래스들은 개별 WAS 제품군에서 정의합니다.

<p align="center"><img src="C:\Users\Yong Lee\AppData\Roaming\Typora\typora-user-images\image-20210925205805496.png" alt="image-20210925205805496" style="zoom:60%;" /></p>

<br>이러한 설계를 통해 다형성을 적용할 수 있습니다.<br>

일반 웹 어플리케이션 개발 진영에서는 Servlet API, 즉 인터페이스를 보고 개발하도록 하고, 
실제 구현부는 개별 WAS 제품군에서 작성한 클래스가 동작하도록 합니다.<br>



그래서 웹 개발자들은 하위 구현체를 보지 않아도 사용이 가능하고 표준화가 이루어지게 됩니다.<br> 또한 WAS가 변경되더라도 특정한 프로그램의 수정 없이 배포되어 실행될 수 있고 이는 Web Application과 개별 WAS 제품군과의 결합도를 낮춰 유지보수성을 향상시킵니다.<br>



간단히 정리하자면 interface 중심의 설계로 다양한 DB vendor(제품군)들을 하나의 소통방식으로 각각의 동작을 수행하게 합니다.<br>

<br><br>

---

> ## Servlet LifeCycle

서블릿의 생명 주기에 대해 살펴보도록 하겠습니다.<br>

앞에서 잠깐 언급을 드렸는데 Servlet/JSP 계층구조의 최상위 인터페이스는 Servlet이고 요청에 대한 서비스`service()`와 삭제`destroy()`, 초기화`init()`하는 메서드들이 있습니다.<br>

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/134771407-4561e9b0-f09d-4601-92a5-3cf3d9050da6.png" alt="image" style="zoom:80%;" /></p>

위의 그림과 같이 메서드들이 사용되며 생명주기를 형성하며, WAS(정확히는 서블릿이 배포된 컨테이너)가 Servlet의 생명주기를 관리합니다.<br>

![image](https://user-images.githubusercontent.com/70495425/135938505-8503cd29-1a1a-4206-83ce-3e1d14df1677.png){:.aligncenter}



Servlet Container는 web.xml(Deployment Descriptor)를 로딩하고 서블릿 객체를 생성합니다.<br>

메서드들의 역할을 보면 다음과 같습니다.

| 메서드명    | 역할                                                         |
| ----------- | ------------------------------------------------------------ |
| `init()`    | 해당 서블릿을 초기화합니다. 서블릿마다 한 번 실행됩니다.     |
| `service()` | 해당 서블릿이 클라이언트에게 서비스하기 위해 실행됩니다(내부적으로 `doGet()` or `doPost()`로 연결). 클라이언트가 요청할 때마다 실행됩니다. |
| `destroy()` | 해당 서블릿이 서비스 종료되기 직전에 호출합니다(WAS를 중지할 때 실행). <br />ex) 백업할 자료가 있는 경우 -> 해당 메서드 내에서 직렬화하여 저장 |

<br>

![image](https://user-images.githubusercontent.com/70495425/134771992-2e4a13b4-febb-4579-bc62-b2125d8f32d4.png){:.aligncenter}





예제 코드를 통해 위의 과정을 확인해보겠습니다.

```java
package step2;

import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CountServlet
 */
public class CountServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private String path="C:\\kosta224\\iotest\\count";
	private int count;
    public CountServlet() {
        super();
       System.out.println("CountServlet 객체생성");
    }
    @Override
    	public void init() throws ServletException {
    		File countFile=new File(path);
    		if(countFile.isFile()) {
    			try {
					DataInputStream dis=new DataInputStream(new FileInputStream(countFile));
					count=dis.readInt();
					dis.close();
				} catch (FileNotFoundException e) {				
					e.printStackTrace();
				} catch (IOException e) {					
					e.printStackTrace();
				}
    		}else {
    			count++;
    		}
    		System.out.println("init 실행");
    	}
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("service() 실행");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out=response.getWriter();
		out.println("<h3>CountServlet service 계열 메서드 doGet method 실행");
		out.println(" 접속수:"+count+"</h3>");
		count++;
		out.close();
	}
	@Override
		public void destroy() {			
			try {
				DataOutputStream dos=new DataOutputStream(new FileOutputStream(path));
				dos.writeInt(count);				
				dos.close();
			} catch (IOException e) {				
				e.printStackTrace();
			}
			System.out.println("destroy 실행- 조회수 백업");
		}
}
```



실행해보면 콘솔에서는 아래와 같이 정상적인 수행이 되었는지 출력을 통해 확인이 가능하고 브라우저에서는 접속 수를 확인할 수 있습니다. 

![image](https://user-images.githubusercontent.com/70495425/134772863-458a4df7-d559-4414-8e86-a60839252027.png){:.aligncenter}

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/135813631-8f462180-0a9f-4d44-b3a2-f315ce5afca1.png" alt="image" style="zoom:68%;" /></p>

<br>새로고침을 통한 접속수 변화 확인

![image](https://user-images.githubusercontent.com/70495425/134772958-aa974172-cc23-44d8-9bbf-a60ddaf8523c.png){:.aligncenter}

<br>그리고 서버를 중단시키면 파일을 생성해서 해당 접속수를 저장하도록 합니다.

![image](https://user-images.githubusercontent.com/70495425/134772991-87aea5d7-a41e-47bf-826e-6c13abb53515.png){:.aligncenter}

서버를 다시 실행하면 저장된 접속수를 불러와서 다시 사용할 수 있게 됩니다.<br>

<br>



위의 상황을 면접 상황에서 질문을 받아 대답한다고 생각하고 접근해보겠습니다.<br>

ex) LifeCycleServlet에 클라이언트가 10명이 접속해서 서비스를 받았다.

<details>
<summary>Q1. LifyCycleServlet 객체는 몇 개 생성되는가?</summary>
<div markdown="1">       
1개
</div>
</details>

<details>
<summary>Q2. init 메서드는 몇 번 실행되는가?</summary>
<div markdown="1">       
한 번
</div>
</details>

<details>
<summary>Q3. service 메서드는 몇 번 실행되는가?</summary>
<div markdown="1">       
열 번
</div>
</details>

<details>
<summary>Q4. destroy메서드는 몇 번 실행되는가?</summary>
<div markdown="1">       
서비스 종료(WAS 중지) 직전에 한 번
</div>
</details>

간단한 질문이지만 4번 질문에서 섣부르게 한 번이라고 단정짓지 않도록 유의해야 합니다.





---

> ## ServletConfig & ServletContext

ServerConfig에서 config는 configuration의 줄임말로 설정의 의미를 갖고 있습니다. <br>context는 일반적으로 문맥이라는 뜻을 갖고 있는데 computer science에서는 비교적 다양한 상황에서 사용되어 '특정 연관성이 있는 정보의 묶음', '어떤 객체를 핸들링하기 위한 접근 수단' 등의 의미로 사용되고 있는 것 같습니다.<br>ServletContext에서의 Context 역시 의미에 대해 의견이 다양한 것 같습니다.<br>

스택오버플로우에서 이에 관한 사용자들의 의견을 살펴보면  web application의 특정 정보 또는 기능을 담는 것을 지칭한는 의견이 많았고, 수업에서도 web application과 같은 맥락으로 사용되고 있습니다. (_여담이지만 ServletContext에서의 Context가 의미를 명확하게 담지 못한 케이스라는 의견이 많은 것으로 보입니다._ - [참조](https://stackoverflow.com/questions/4737011/what-does-context-in-servletcontext-mean))<br>

API document 설명을 살펴보겠습니다.

![image](https://user-images.githubusercontent.com/70495425/134773671-966d8802-93e7-47df-803c-c263f07c069d.png)

ServletConfig 객체는 servlet container에 의해 사용되는데 초기화 과정에서 서블릿에 정보를 넘겨줍니다.

<br>

![image](https://user-images.githubusercontent.com/70495425/134773699-45409b9f-d965-4871-b4c6-93fac9949a1d.png)

ServletContext는 서블릿이 컨테이너와 소통하기 위한 메서드들을 정의합니다.



표로 간단히 특징을 살펴보겠습니다.

| ServletConfig                                                | ServletContext                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 개별 Servlet의 설정 정보를 저장하는 객체                     | Web Application내의 모든 서블릿과 jsp가 공유하는 자원(필요 시 정보를 set/get 가능) |
| Servlet Container에 의해 `init()` 호출 시점에 ServletConfig 객체가 주입 | Web Application시작 시점에 생성되고 종료 직전에 소멸         |
| 초기 파라미터(init-param), ServletContext 객체 주소값 등이 ServletConfig에 저장되어 전달된다 | Servlet의 `getServletContext()` 를 통해 참조                 |
| Servlet객체당 하나씩 생성된다.                               | Web Application당 하나 생성된다.                             |

<br>

<br>

---

> ### Initialization Parameters

먼저 초기 파라미터의 특징을 간단히 살펴보자면,

- 변경 가능성 있는 문자열을 web.xml에 설정해둔 뒤 Servlet/JSP에서 호출해서 사용합니다
- 문자열 변경 시 소스코드의 변경 없이 수정이 가능합니다
- `getInitParameter(String name):String` 메서드를 사용합니다

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/134795331-cda92a9a-0178-4b87-a832-b8711b8df990.png" alt="image" style="zoom:80%;" /></p>

<br>

범위에 따라 ServletConfig와 ServletContext를 이용한 방법이 있습니다.<br>

- Servlet 인스턴스가 사용할 초기 파라미터의 설정은 ServletConfig를 이용합니다.
  `<servlet>`태그의 sub태그인 `<init-param>`을 이용해 설정합니다.
- web application 내 모든 Servlet이 사용할 초기 파라미터의 설정은 ServletContext를 이용합니다.
  `<context-param>`태그를 이용해 설정합니다.

<br>

<br>

---

> ### Attribute

: Servlet간에 공유하는 객체를 지칭합니다.

<br>

#### Scope

- Attribute를 저장하는 공간으로 공유 범위에 따라 다음의 세 가지 범위로 나눕니다.
  1. **request scope**: `HttpServletRequest` 이용
     - Request가 살아있는 동안 공유
  2. **session scope**: `HttpSession` 이용(Session은 다음 글에서 자세히 다룰 예정입니다.)
     - 한 명의 client(web browser)가 살아있는 동안 공유
  3. **application scope**: `ServletContext` 이용
     - Application이 시작해서 종료될 때까지 공유

<br>

#### 관리 메소드

: Attribute는 key-value로 관리됩니다.

- `setAttribute(String key, Object value)`
- `getAttribute(String key): Object`
- `removeAttribute(String key)`
- `getAttributeNames(): Enumeration`

<br>

<br>

<br>

간단한 예제를 통해 위의 개념들을 어떤 방식으로 사용하는지 살펴보도록 하겠습니다.

1.ServletConfig 테스트입니다.

 ```java
package step3;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ServletConfigTestServlet
 */
public class ServletConfigTestServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
  
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out=response.getWriter();
		out.println("<h3>ServletConfig study</h3>");
		String name=this.getServletConfig().getServletName();
		out.println("ServletConfig로부터 Servlet Name을 반환받음:"+name);
		String frameworkConfig=getServletConfig().getInitParameter("contextConfigLocation");
		out.println("<hr>framework 설정파일:"+frameworkConfig);
		out.close();
	}
}
 ```

<br>

<br>

web.xml(DD)는 다음과 같습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://Java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee; http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>webstudy07-servlet-LifeCycle-inst</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>
  <servlet>
    <description></description>
    <display-name>ServletConfigTestServlet</display-name>
    <servlet-name>MyServletConfigTestServlet</servlet-name>
    <servlet-class>step3.ServletConfigTestServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>springmvc-config.xml</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>MyServletConfigTestServlet</servlet-name>
    <url-pattern>/ServletConfigTestServlet</url-pattern>
  </servlet-mapping>
</web-app>
```

DD에서 `<init-param>`태그를 보시면 `name`과 `value`의 쌍으로 정보를 입력합니다. 아직 SpringMVC를 배우지 않았지만 나중에 위와 같이 오늘 배운 내용을 사용하게 될 것 같습니다.

<br><br>

실행시켜보면 다음과 같이 정보가 정상적으로 전달되고 저장되는 것을 확인하실 수 있습니다.

<img src="https://user-images.githubusercontent.com/70495425/134795469-2dd01d3c-940a-4200-ab82-2217a8e2406c.png" alt="image" style="zoom:67%;" />

<br><br><br>

2.ServletContext 테스트입니다.

 ```java
package step4;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
/**
 * Servlet implementation class ContextTest1
 */
public class ContextTest1 extends HttpServlet {
	private static final long serialVersionUID = 1L;
    /**
     * @see HttpServlet#HttpServlet()
     */
    public ContextTest1() {
        super();
    }
	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out=response.getWriter();
		String name=getServletConfig().getServletName();
		out.println("<h3>"+name+"에서 ServletContext를 테스트</h3>");
		String info=getServletConfig().getServletContext().getInitParameter("security");
		out.println("ServletContext로부터 정보를 가져옴:"+info);
		out.close();
	}
}
 ```

<br><br>

 ```java
package step5;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ContextTest2
 */
public class ContextTest2 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public ContextTest2() {
        super();
        // TODO Auto-generated constructor stub
    }
	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out=response.getWriter();
		String name=getServletConfig().getServletName();
		out.println("<h3>"+name+"에서 ServletContext를 테스트</h3>");
		String info=getServletConfig().getServletContext().getInitParameter("security");
		out.println("ServletContext로부터 정보를 가져옴:"+info);
		out.close();
	}

}
 ```

<br>

web.xml(DD)는 다음과 같습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://Java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee; http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>webstudy07-servlet-LifeCycle-inst</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>
  <servlet>
    <description></description>
    <display-name>ContextTest1</display-name>
    <servlet-name>ContextTest1</servlet-name>
    <servlet-class>step4.ContextTest1</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>ContextTest1</servlet-name>
    <url-pattern>/ContextTest1</url-pattern>
  </servlet-mapping>
  <servlet>
    <description></description>
    <display-name>ContextTest2</display-name>
    <servlet-name>ContextTest2</servlet-name>
    <servlet-class>step5.ContextTest2</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>ContextTest2</servlet-name>
    <url-pattern>/ContextTest2</url-pattern>
  </servlet-mapping>
  <context-param>
    <param-name>security</param-name>
    <param-value>spring-security.xml</param-value>
  </context-param>
</web-app>
```

<br>

실행해보면 다음과 같이 ContextTest1, ContextTest2 모두 동일한 자원을 공유한다는 것을 확인할 수 있습니다. 다른 브라우저(client)에서 실행하더라도 객체는 유지되고 동일한 객체임을 보여줍니다.

<img src="https://user-images.githubusercontent.com/70495425/134795697-1973e14b-8763-4cea-9480-1b1e575f02da.png" alt="image" style="zoom:80%;" />





이것으로 오늘 복습은 마치도록 하겠습니다.

<br>

마지막에 배운 ServletConfig, ServletContext의 내용은 나중에 Spring의 DispathcerServlet이나 maven, gradle 등의 툴을 사용할 때 직접 다뤄보면서 익힐 수 있는 내용으로 지금은 가볍게 이런 기능들이 있다는 정도로만 알고 넘어가도록 하겠습니다.

<br>

오늘 수업 내용은 명확한 쓰임새나 구조를 이해하기에 어려움이 있었습니다. 다음 주에는 보다 실습 위주로 진행을 할 예정이기에 일주일간 배운 웹 수업 내용을 보다 깊이 이해할 수 있도록 해야겠습니다.<br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

