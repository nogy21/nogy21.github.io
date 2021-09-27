---
title: "WEB(03) - js 기초 & Servlet - 수업 34일차"

date: 2021-09-16
last_modified_at: 2021-09-20


categories:
  - Java Web Programming

tags:
  - [kosta, tomcat, dom]

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

목차

* [자바스크립트 기초](#자바스크립트-기초)
  * [div & span, innerHTML](#div-&-span,-innerhtml)
    * [value VS innerHTML](#value-vs-innerhtml)
  * [operator](#operator)
  * [array](#array)
  * [checkbox](#checkbox)
  * [submit](#submit)
  
  
  

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>



---

## 자바스크립트 기초

자바스크립트 기초 수업을 이어가겠습니다. 

<br>

웹 수업 첫번째 날에 `<div>`와 `<span>`의 영역 차이에 대해 설명 드린 적이 있습니다.



이번에는 특정한 영역을 지정하여 `innerHTML` 기능을 활용해보겠습니다.

<br>

w3schools.com에서는 `innerHTML`을 다음과 같이 정의하고 있습니다.

<img src="https://user-images.githubusercontent.com/70495425/134161862-4f56c0e0-7dbf-4160-8fe2-7bb436f5ad9b.png" alt="image" style="zoom:80%;" />

element의 내용을 정하거나 반환해주는 기능이라고 하는데, 이것만으로는 아직 정확한 용도를 이해하기 어려워서 예시 코드를 살펴보겠습니다.

<br>

<img src="https://user-images.githubusercontent.com/70495425/134162123-38adb246-ac2a-42b6-9032-e74ee8b191ff.png" alt="image" style="zoom:80%;" />

`id`가 `"demo"`인 `<p> element`의 내용을 `"Paragraph changed!"`와 같이 바꿔주는 기능인 것 같습니다.<br>

그러면 이제 수업 때 진행한 코드를 살펴보면서 `innerHTML`을 어떤 식으로 사용했는지, 또 `<span>`과 `<div>`영역의 차이를 `innerHTML`을 활용해서 살펴보도록 하겠습니다.<br>본 예제는 가독성을 위해 css와 js 모두 한 파일에 작성했습니다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>div span innerHTML 연습</title>
<style type="text/css">
#testDiv {
	background-color: orange;
}
#testSpan {
	background-color: lime;
}
h3 {
	background-color: yellow;
}
</style>
<script type="text/javascript">
	function changeDivInfo() {
		//div id testDiv 요소가 가지고 있는 html 정보를 alert으로 출력해본다 innerHTML 을 GET
		alert(document.getElementById("testDiv").innerHTML);
		//div id testDiv인 요소의 내부 html 정보를 다시 할당한다 innerHTML을 SET 
		document.getElementById("testDiv").innerHTML = "<h3>즐거운 수요일 저녁입니다^^</h3>";
	}
	function changeSpanInfo() {
		document.getElementById("testSpan").innerHTML = "<font color=red>즐거운 저녁식사되세요</font>";
	}
</script>
</head>
<body>
	<div id="testDiv">이 부분은 id testDiv로 지정된 div 영역입니다</div>
	<div>이 부분은 div 영역입니다</div>
	<!-- span tag 내부에 h tag 를 적용할 수 없음. h tag의 영역은 div와 같이 가로화면을 100%로 차지해서 span영역을 넘기 때문  -->
	<h3><span id="testSpan">이 부분은 id testSpan으로 지정된 span영역입니다</span></h3>
    <span>이 부분은 span 영역입니다</span>
	<br><br>
	<input type="button" value="div정보변경" onclick="changeDivInfo()">
	<input type="button" value="span정보변경" onclick="changeSpanInfo()">
	<!-- 위 버튼을 클릭하면 즐거운 저녁식사되세요 로 변경된다  -->
</body>
</html>
```

29~38번 라인에 해당하는 바디 태그를 보시면 2개의 `<div>`와 1개의 `<span>`을 각각 다르게 설정하여 영역별 차이를 확인하려 합니다.<br>가장 먼저 보이는 영역은 `<div>` 영역인데 `id`를 지정해서 다음에 오는 `<div>`와 차이를 둡니다. 그리고 `<span>`영역 역시 마찬가지로 `id`를 지정한 것과 아무것도 지정하지 않은 `<span>`을 만들어서 차이를 확인해보겠습니다.<br>

36, 37번 라인에서는 `button` 타입의 `input`을 만들어서 각각 클릭을 하면 `function`이 작동하도록 합니다.<br>

<br>



바디 태그에서 조금 위로 올라오면 17~27번 라인은 js, 6~16번 라인은 css 코드입니다.



js 코드에서 이번에 사용해볼 `innerHTML`의 `get`, `set`기능을 모두 활용해보고 css 코드로 각 영역별 색상을 지정해서 차이를 확인해봅니다.

<img src="https://user-images.githubusercontent.com/70495425/134171030-e95f1fc4-1348-4dac-80ad-cb18b01b098f.png" alt="image" style="zoom:55%;" />

버튼을 누르면 아래와 같이 영역의 색과 문구가 변경되고, div정보변경을 다시 누르면 변경된 문구를 `alert`로도 확인할 수 있습니다.(다만 `alert`로 띄워진 창에는 `<h3>` 등의 태그는 적용되지 않고 문자 그대로 출력됩니다.)<br>

여기서 눈여겨볼 만한 부분은 `<h3>` 태그가 적용된 부분의 위아래로 공간이 적용되는 점입니다.

<img src="https://user-images.githubusercontent.com/70495425/134171651-76ca2c41-fa52-404e-90b4-10dd5b3606de.png" alt="image" style="zoom:80%;" />

<img src="https://user-images.githubusercontent.com/70495425/134171796-5385cb27-268b-4449-9e48-b8330f077ac4.png" alt="image" style="zoom:70%;" />

위와 같이 새로운 공백이 추가되게 됩니다. <br>

<img src="https://user-images.githubusercontent.com/70495425/134172305-d5436b5c-e718-49c1-81a5-cc7f49623e93.png" alt="image" style="zoom:80%;" />

<img src="https://user-images.githubusercontent.com/70495425/134172147-a169eb21-b632-483c-b011-a0f8200a17b6.png" alt="image" style="zoom:80%;" />

기존의 `<div>`,`<span>`영역은 그대로이고 이 위에 `<h3>` 영역이 덮어씌워지는 것을 확인할 수 있습니다.<br>

`h`태그의 영역이 `span`태그의 영역보다 더 크기 때문에 `span`태그 내부에 `h`태그는 적용되지 않습니다.<br>



---

#### value VS innerHTML

---

`value`와 `innerHTML`의 차이점에 대해 간단히 짚고 넘어가자면, 둘 모두 값을 나타내지만 `value`는 태그 안의 값(입력 양식)을 `innerHTML`은 태그 사이에 입력된 값을 나타낸다는 차이점이 있습니다.

```html
<input value="hi"/>
input.value == "hi";

<div>hello</div>
div.innerHTML == "hello";
```





<br>

<br>

---

### Operator

---

예제를 통해 js 연산자에 대해 간단히 알아보고 넘어가도록 하겠습니다.



{% capture text-capture %}
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>javascript 연산자</title>
<script type="text/javascript">
    function test() {
        //let : ecma6 block level scope 변수 선언시 사용  
        let name1="즐추석";
        let name2="즐추석 ";
        console.log(name1.length);//3
        console.log(name2.length);//4
        console.log(name1==name2);//문자열 비교 false
        console.log(name1==name2.trim());//true : trim() 으로 양여백을 제거 
        //var : function level scope , 기존에 사용하는 변수 선언 
        var age1=21;//정수형
        var age2="21";//문자형 
        console.log(age1==age2);//true 비교연산자 == ( 강제 형변환 : 숫자로 변환  )
        console.log(age1===age2);//false 비교연산자 === (타입 체크 , 엄격한 비교)
        var age3=age1+2;
        console.log(age3);//23
        var age4=age2+2;
        console.log(age4);//212  문자열이므로 
        var age5=parseInt(age2)+2;//parseInt() 로 정수형으로 변환해서 연산 
        console.log(age5);//23
        var age6=age1+age2;
        console.log(age6);//2121
        console.log(age6+2);//21212
    }
</script>
</head>
<body>
<input type="button" value="확인" onclick="test()">
</body>
</html>
```
{% endcapture %}
{% include widgets/toggle-field.html toggle-name="toggle-thats" button-text="전체 코드" toggle-text=text-capture %}



```javascript
let name1="즐추석";
let name2="즐추석 ";
console.log(name1.length);//3
console.log(name2.length);//4
console.log(name1==name2);//문자열 비교 false
console.log(name1==name2.trim());//true : trim() 으로 양여백을 제거 
```

9번, 10번 라인에서 `let`을 사용하여 변수를 선언하고 11, 12번 라인에서 `.length`기능으로 `console.log()`을 통해 문자열의 길이를 확인할 수 있습니다.<br>첫번째 변수와 두번째 변수는 글자는 같지만 두번째 변수에 공백을 하나 추가했기 때문에 13번 라인에서 `==` 동등연산자로 확인해보면 `false`가 나타납니다. 14번 라인에서는 두번째 변수에 `trim()`기능으로 양여백을 제거해서 두 변수의 값이 일치하는 것을 확인하실 수 있습니다.<br>

```javascript
var age1=21;//정수형
var age2="21";//문자형 
console.log(age1==age2);//true 비교연산자 == ( 강제 형변환 : 숫자로 변환  )
console.log(age1===age2);//false 비교연산자 === (타입 체크 , 엄격한 비교)
```

16,17번 라인에서는 정수형 타입과 문자형 타입의 변수를 선언합니다.<br> 18번 라인에서는 동등연산자로(`==`), 19번 라인에서는 일치연산자(`===`)로 값을 비교하는데, 동등연산자 `==`는 강제 형변환(숫자로 변환됨)이 일어나서 `true`를 반환하고 일치연산자`===`는 타입을 체크하기에 `false`를 반환합니다.<br>

```javascript
var age3=age1+2;
console.log(age3);//23
var age4=age2+2;
console.log(age4);//212  문자열이므로 
var age5=parseInt(age2)+2;//parseInt() 로 정수형으로 변환해서 연산 
console.log(age5);//23
var age6=age1+age2;
console.log(age6);//2121
console.log(age6+2);//21212
```

20~28번 라인에서는 정수형, 문자형 타입의 변수간의 연산 결과를 확인해봅니다.



---

### Array

javascript에서의 배열을 알아보겠습니다.<br>가시적인 확인을 위해 버튼을 만들어서 버튼을 누르면 배열이 생성되어 `alert`로 확인하는 방식으로 코드를 작성해보겠습니다.<br>

바디 태그에 다음과 같이 버튼을 작성하고,

```html
<button type="button" onclick="testArray1()">배열테스트1</button>
```

js로 기능을 작성하면 됩니다.

```javascript
function testArray1() {
    let arr=new Array(3);
    arr[0]="소막창";
    arr[1]="참치";
    arr[2]="치킨";
    for(let i=0;i<arr.length;i++){
        alert(arr[i]);
    }
}
```

배열테스트1 버튼을 누르면 아래와 같은 팝업이 연속적으로 뜨면서 배열이 생성되었음을 확인할 수 있습니다.

<img src="https://user-images.githubusercontent.com/70495425/134197500-5c4c2917-eb6c-4b42-a742-8ff9fe1b1c83.png" alt="image" style="zoom:80%;" />

<br>

이번에는 `checkbox` 타입의 메뉴와 주문 버튼을 만들어서 체크한 메뉴명을 출력하는 예제를 진행해보겠습니다.

```html
<form>
	<input type="checkbox" name="menu" value="닭갈비">~닭갈비~<br>
	<input type="checkbox" name="menu" value="공기밥">~공기밥~<br>
	<input type="checkbox" name="menu" value="참이슬">~참이슬~<br>
	<input type="button" value="주문" onclick="testArray2()">
</form>
```

위와 같이 `form`태그에 `input`으로 `checkbox` 타입의 메뉴와 `button`타입의 주문 버튼을 만들어서 주문 버튼을 누르면 `testArray2()`가 작동하도록 합니다. (여기서 `checkbox` 타입은 네모난 박스 형태로 사용자가 누르면 v모양의 체크가 표시되고 다시 누르면 체크가 해제되는 기능을 합니다)

```javascript
function testArray2(){
    let m=document.getElementsByName("menu");
    //checkbox 입력양식 요소 객체의 속성 중에 checked가 있다. (체크상태 - true)
    //for문을 이용해서 선택(체크)한 메뉴에 대해 메뉴명을 alert로 각각 출력한다
    for(let i=0; i<m.length; i++){
        if(m[i].checked)
            alert(m[i].value);
    }
}
```

아래와 같이 체크한 메뉴들을 팝업으로 확인할 수 있습니다.

<img src="https://user-images.githubusercontent.com/70495425/134199226-0f7e1f72-4857-4e1d-a0f8-6f8961ea2472.png" alt="image" style="zoom:67%;" />

<br>





---

### submit

: form의 submit 메서드는 입력된 값을 서버로 전송할 때 사용합니다.<br>기능 자체는 submit 버튼과 동일합니다.

<br>

예제 코드를 통해 살펴보겠습니다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>form submit</title>
</head>
<body>
 <script type="text/javascript">
     function confirmForm(){
    	 return confirm("전송하시겠습니까?");//confirm()은 확인:true, 취소:false 반환
     }
 </script>
<form action="step13-server.jsp" onsubmit="return confirmForm()">
별명 <input type="text" name="nick" required="required"><br>
나이 <input type="number" name="age" required="required"><br>
<!-- <input type="submit" value="전송"> -->
<button>전송</button><!-- 아래 type=submit과 동일 -->
<!-- <button type="">전송</button> -->
</form>
</body>
</html>
```

13번 라인부터 `form`을 작성하는데, `action`속성에는 요청을 처리할 서버의 url이 들어갑니다.

`onsubmit`속성은 `"return false"`가 오는 경우 전송을 막아줍니다.

<br>16번 라인에서 `submit`기능을 사용하는데, `<button>`태그만 명시되어 있는 것을 보실 수 있습니다. 

`button`의 기본 타입이 `submit`이기 때문에 타입을 명시하지 않아도 되고, `<input type="submit" value="전송">`과 18번 라인은 동일한 기능을 수행하게 됩니다.<br>



전송 여부를 사용자에게 맡기기 위해 `onsubmit()`에서 명시했던 `confirmForm()`메서드를 8~12번 라인에서 작성합니다.<br>`confirm` 메서드는 창을 띄우며 사용자가 확인을 누를 경우 true, 취소는 false값을 반환받습니다.<br>





이렇게 전송한 정보를 서버측이 정상적으로 전달받았다는 것을 확인하기 위하 간단한 jsp 작성을 하겠습니다. 자세한 내용은 이후 jsp 수업에서 자세히 다룰 예정입니다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>server jsp</title>
</head>
<body>
 서버에서 정보를 전달받아 응답한다<br><br>
 별명:<%=request.getParameter("nick") %> 나이:<%=request.getParameter("age") %>
</body>
</html>
```

위와 같이 작성하면 처음에 사용자가 입력한 별명과 나이의 value가 정상적으로 전송되었는지 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/70495425/134797750-d58f7b8b-4df8-4a4f-aff4-9cb2cff2ee70.png){:.aligncenter}





---

참고: jsp 내용이 나왔기에 간단히 구현하는 위치에 대해 설명드리겠습니다.

- src/main/java -> java main class
- src/main/webapp -> html, javascript, css, jsp
- src/main/webapp/web-inf -> .xml(설정 문서)
- src/main/webapp/web-inf/lib -> maven, ...(외부 오픈소스 라이브러리)

<br><br>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

