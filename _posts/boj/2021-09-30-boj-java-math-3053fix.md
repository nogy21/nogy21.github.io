---
title: "백준 알고리즘 - 3053번 택시 기하학"

date: 2021-09-30

categories:
  - BOJ

tags:
  - [algorithm, "3053", 택시기하학]
---

> # 택시 기하학

| 시간 제한 | 메모리 제한 | 제출  | 정답  | 맞은 사람 | 정답 비율 |
| :-------- | :---------- | :---- | :---- | :-------- | :-------- |
| 1 초      | 128 MB      | 33008 | 14457 | 12650     | 43.559%   |

## 문제

19세기 독일 수학자 헤르만 민코프스키는 비유클리드 기하학 중 택시 기하학을 고안했다.

택시 기하학에서 두 점 T1(x1,y1), T2(x2,y2) 사이의 거리는 다음과 같이 구할 수 있다.

D(T1,T2) = |x1-x2| + |y1-y2|

두 점 사이의 거리를 제외한 나머지 정의는 유클리드 기하학에서의 정의와 같다.

따라서 택시 기하학에서 원의 정의는 유클리드 기하학에서 원의 정의와 같다.

원: 평면 상의 어떤 점에서 거리가 일정한 점들의 집합

반지름 R이 주어졌을 때, 유클리드 기하학에서 원의 넓이와, 택시 기하학에서 원의 넓이를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 반지름 R이 주어진다. R은 10,000보다 작거나 같은 자연수이다.

## 출력

첫째 줄에는 유클리드 기하학에서 반지름이 R인 원의 넓이를, 둘째 줄에는 택시 기하학에서 반지름이 R인 원의 넓이를 출력한다. 정답과의 오차는 0.0001까지 허용한다.

## 예제 입력 1 복사

```
1
```

## 예제 출력 1 복사

```
3.141593
2.000000
```

## 예제 입력 2 복사

```
21
```

## 예제 출력 2 복사

```
1385.442360
882.000000
```

## 예제 입력 3 복사

```
42
```

## 예제 출력 3 복사

```
5541.769441
3528.000000
```



---



<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

> ## 해설

유클리드 기하학과 택시 기하학에 관한 문제입니다. 

이전 문제인 직각삼각형의 문제와 이어지기도 하는데, 유클리드 기하학에서 두 점 사이의 거리는 직각삼각형에 대한 피타고리스의 정리에서 구할 수 있습니다.

![image](https://user-images.githubusercontent.com/70495425/135378717-1c81632e-fc2a-479c-9f67-4d356df2c3e5.png)

그런데 택시 기하학에서의 두 점 사이의 거리는 문제의 설명처럼 다음과 같습니다.

![image](https://user-images.githubusercontent.com/70495425/135378882-17c9106e-5e36-4b83-87b8-10c1bc9756b8.png)

간단하게 수를 대입해서 확인해보겠습니다.

A(1,1)라는 점과 B(6,6)라는 점이 있다고 가정하겠습니다.

<img src="https://user-images.githubusercontent.com/70495425/135401040-2f038595-8727-4b28-872f-b38bdc445d26.png" alt="image" style="zoom:40%;" />

택시 기하학에서의 정의에 따르면 |6-1| + |6-1| = 10의 거리가 나옵니다.

원의 모양 또한 다르게 나타날 것 입니다.

원점으로부터 3만큼 떨어진 곳에 위치한 점들의 집합을 그려보면 다음과 같습니다.

<img src="https://user-images.githubusercontent.com/70495425/135379429-cf257321-63f7-49ba-9000-315d1a4912f0.png" alt="유클리드 기하학" style="zoom: 67%;" /><img src="https://user-images.githubusercontent.com/70495425/135403954-b29779bb-057e-41bb-839b-a185c0d21c7f.png" alt="image" style="zoom: 33%;" />

좌측은 저희가 알고 있는 유클리드 기하학에 따른 원이고, 우측은 택시 기하학에 따른 원입니다.

이제 넓이는 구하는 공식을 구해서 코드를 진행하면 되겠습니다.

유클리드 기하학의 경우 파이 * 반지름의 제곱으로 구할 수 있고,

택시 기하학의 경우 그래프를 통해 2 * 반지름의 제곱으로 구할 수 있다는 것을 알 수 있습니다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;


public class Main {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		double r= Double.parseDouble(br.readLine());
		String u = String.format("%.6f",EuclidianGeometryCircleArea(r));
		String t = String.format("%.6f",TaxiCabGeometryCircleArea(r));
		System.out.println(u+"\n"+t);
	}
	public static double EuclidianGeometryCircleArea(double a) {
		return Math.PI*a*a;
	}
	public static double TaxiCabGeometryCircleArea(double a) {
		return 2*a*a;
	}
}
```

소수점은 5번째자리까지 표기하고, 오차가 0.0001까지 허용되기에 `String.format()`을 사용해서 진행했습니다.



```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		int r = sc.nextInt();
		System.out.printf("%f\n%f", Math.PI*r*r, (double)2*r*r);
	}
}
```





<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>
