---
title: "WEB(06) - Servlet Annotation- 수업 37일차"

date: 2021-09-27
last_modified_at: 2021-10-23


categories:
  - Java Web Programming

tags:
  - [kosta, annotation]

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

목차

* [Annotation](#annotation)
  * [예제](#예제)
  * [loadOnStartUp](#loadonstartup)
  * [WebListener](#weblistener)

<br>

---

> ## Annotation

일반적으로 **어노테이션**은 컴파일 및 런타임 시점에 **시스템에 영향을 주는 의미 있는 주석**을 말합니다.<br>

그런데 저희가 이번에 배울 서블릿에서의 어노테이션은 기존 `web.xml`의 `url-pattern`의 설정과 같은 효과를 가지는 서블릿 3.0 이상에서 지원하는 기술입니다.

<br>

그래서 서블릿 3.0 이상부터는 어노테이션의 표기 방법인 **`@`를 서블릿 상단부에 명시**하게되면 소스코드에서 **설정 정보를 기술**할 수 있게 되었으며, **서블릿 매핑 또한 자동적으로 지원**해줍니다.<br>

url 매핑 기법을 굳이 `web.xml`과 어노테이션 두 가지로 나눠서 사용하는 이유는 **편리함** 때문입니다.<br>자바 파일이 많지 않다면 `web.xml`에서 직접 작성하는 방식이 크게 불편하지 않을 수 있습니다만, 파일이 많아진다면 하나씩 매핑해주는 일이 결코 반갑지 않게 되겠죠.

<br>

그래서 이를 자동적으로 처리해줄 수 있도록 하는 기능이 어노테이션입니다.

<br>

간단히 역할을 보면,

> **XML: 소스 코드와 설정의 분리**<br>

> **Annotation: 소스코드 상에 설정 정보를 기술**

정도로 짧게 소개할 수 있을 것 같습니다. 실제 업무에서는 일반적으로 **전역적인 설정은 xml에** 기술하고, **설계 시 확정되는 부분은 Annotation**으로 선택적으로 설정하여 사용한다고 합니다.

<br>

정리하자면 기존에 DD에서 해주던 서블릿 매핑 작업을 어노테이션 기술을 적용하면 **클래스 위에 어노테이션을 명시**하는 것 만으로도 손쉽게 **서블릿 매핑이 가능**하게 되었고, 필요에 따라 xml과 어노테이션을 함께 사용합니다.

<br>

그러면 직접 어노테이션 설정 방법을 확인해보도록 하겠습니다.

<br>

<br>

---

> ### 예제

다음과 같이 어노테이션을 적용해서 코드를 작성했습니다.

```java
import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/as")
public class AnnotationServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;     
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out=response.getWriter();
		out.println("Annotation 설정 기반 서블릿 테스트");
		out.close();
	}
}
```

10번 라인을 보시면 어노테이션으로 url 주소를 명시했습니다.

<br>

실행해 보면 다음과 같이 url 주소에 어노테이션에 작성한 url이 참조된 것을 확인하실 수 있습니다.

![image](https://user-images.githubusercontent.com/70495425/138553406-04e4d943-af63-4eb8-b22e-5965c1c774d0.png)

<br>

<br>

---

> ### loadOnStartup

이번에는 웹어플리케이션 실행 시점에 미리 해당 서블릿을 초기화 하도록 하는 `web.xml`의 `load-on-startup` 설정을 어노테이션 방식으로 설정해서 적용이 되는지 확인하겠습니다.<br>(_원래 서블릿은 브라우저의 최초 요청 시 `init()`을 호출하며 메모리에 적재되고 기능이 수행됩니다. 그래서 최초 요청에 한해 실행 시간이 보다 길어질 수 있는데 이런 단점을 보완하기 위한 기능이 `load-on-startup`기능입니다._)

```java
import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet(urlPatterns = "/LifeCycleServlet",loadOnStartup = 1)
public class LifeCycleServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    public LifeCycleServlet() {
        super();
        System.out.println("LifeCycleServlet 객체 생성");
    }
    
    @Override
    public void init() throws ServletException {
    	System.out.println("init() 실행");
    }
    
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out=response.getWriter();
		out.println("Annotation 설정 기반 서블릿 테스트 "+getServletConfig().getServletName());
		System.out.println("service() -> doGet() 실행");
		out.close();
	}
    
	@Override
	public void destroy() {
		System.out.println("destroy() 실행");
	}
}
```

10번 라인을 보시면 다음과 같이 `loadOnStartup` 속성을 어노테이션에 작성했습니다. `@WebServlet(urlPatterns = "/LifeCycleServlet",loadOnStartup = 1)`

결과를 확인해보시면 아래와 같이 컨테이너가 실행되자마자 함께 서블릿을 미리 생성한 것을 확인하실 수 있습니다.

![image](https://user-images.githubusercontent.com/70495425/138553556-d783d392-5b14-4a93-bb89-90d02a735427.png)

<br>

만약 위의 코드에서 일반적인 어노테이션을 적용했다면 아래와 같이 컨테이너가 먼저 실행되고 난 뒤 브라우저 요청 시 서블릿이 생성되고 `init()` 메서드가 호출됩니다.

![image](https://user-images.githubusercontent.com/70495425/138553731-8394659f-baaa-4db5-88cd-9c9adfd863ac.png)

<br>

<br>

---

> ### WebListener

마지막으로 어노테이션으로 `ServletContextListner` 설정을 확인해보겠습니다.

<br>

`ServletContextListener`는 웹 어플리케이션의 시작 또는 종료 시점에 호출되는 메서드를 정의한 인터페이스입니다.

보통 `web.xml`에서 `<listener>` 태그를 사용해 설정을 하는데, 이번에는 어노테이션으로 해당 기능을 적용해보겠습니다.

```java
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class TestListener implements ServletContextListener {

    public TestListener() {}

    public void contextDestroyed(ServletContextEvent sce)  { 
         System.out.println("contextDestroyed");
    }

    public void contextInitialized(ServletContextEvent sce)  { 
         System.out.println("contextInitialized");
    }
	
}
```

5번 라인과 같이 `@WebListener` 라는 어노테이션을 작성했습니다. 해당 파일은 브라우저에서 실행되지 않고 콘솔창에서 기능의 수행만 확인해보려 합니다.

<br>

10번 라인의 `contextDestroyed()` 메서드는 웹어플리케이션이 종료되기 직전 또는 ServletContext가 해제되기 직전에 호출됩니다.

<br>

14번 라인의 `contextInitialized()` 메서드는 웹어플리케이션 시작 시점 또는 ServletContext가 생성되고 난 직후에 호출됩니다.

<br>

프로그램을 실행시킨 후 콘솔창을 확인해보면 다음과 같이 `listener` 기능이 정상적으로 적용된 것을 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/70495425/138554130-9c36abc1-0816-4f62-80d8-061d810362f1.png)

![image](https://user-images.githubusercontent.com/70495425/138554146-2a7eb672-bef0-463c-ad88-a3953a5d0690.png)

<br>

<br>


---

지금까지 수업에서는 `web.xml`을 공부하기 위해 낮은 버전의 서블릿을 사용했는데, 앞으로는 3.0 이상의 버전을 사용해서 직접 xml 파일을 수정하지 않고, 기술의 편리함을 느끼며 수업을 진행할 예정입니다.<br>





[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

