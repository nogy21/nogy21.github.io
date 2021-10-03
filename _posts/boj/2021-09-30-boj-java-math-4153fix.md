---
title: "백준 알고리즘 - 4153번 직각삼각형"

date: 2021-09-27

categories:
  - BOJ

tags:
  - [algorithm, "4153", 직각삼각형]
---

# 직각삼각형

| 시간 제한 | 메모리 제한 | 제출  | 정답  | 맞은 사람 | 정답 비율 |
| :-------- | :---------- | :---- | :---- | :-------- | :-------- |
| 1 초      | 128 MB      | 34277 | 17989 | 16187     | 52.542%   |

## 문제

과거 이집트인들은 각 변들의 길이가 3, 4, 5인 삼각형이 직각 삼각형인것을 알아냈다. 주어진 세변의 길이로 삼각형이 직각인지 아닌지 구분하시오.

## 입력

입력은 여러개의 테스트케이스로 주어지며 마지막줄에는 0 0 0이 입력된다. 각 테스트케이스는 모두 30,000보다 작은 양의 정수로 주어지며, 각 입력은 변의 길이를 의미한다.

## 출력

각 입력에 대해 직각 삼각형이 맞다면 "right", 아니라면 "wrong"을 출력한다.

## 예제 입력 1 복사

```
6 8 10
25 52 60
5 12 13
0 0 0
```

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

> ## 해설

세 변의 길이를 통해 직각삼각형인지 판단하는 문제입니다.

피타고라스의 정리를 사용해서 접근해보겠습니다.

가장 큰 변의 제곱이 나머지 두 변의 제곱의 합과 같다면 직각 삼각형이기에 right을 출력하도록 하고 이 과정을 반복하되 세 변의 길이가 모두 0이 입력된다면 프로그램을 종료하도록 합니다.



첫번째 풀이는 단순하게 각 변의 길이를 비교하고, 가장 큰 변의 길이의 제곱이 다른 두 변의 제곱의 합과 같을 경우 "right", 아닐 경우 "wrong"이 출력되도록 진행했습니다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;


public class Main {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		while(true) {
			StringTokenizer st=new StringTokenizer(br.readLine());
			int a = Integer.parseInt(st.nextToken());
			int b = Integer.parseInt(st.nextToken());
			int c = Integer.parseInt(st.nextToken());
			if(a==0 && b==0 && c==0)break;
			if(a > b && a > c) {
				isARightTriangle(a,b,c);
			}else if( b > a && b > c) {
				isARightTriangle(b,a,c);
			}else {
				isARightTriangle(c,a,b);
			}
		}
	}
	public static void isARightTriangle(int a, int b, int c) {
		if(a*a == b*b + c*c) {
			System.out.println("right");
		}else {
			System.out.println("wrong");
		}
	}
}
```





---

두 번째 풀이입니다.

알고리즘 스터디를 함께 하고 있는 김선생님의 코드를 참고했습니다.

Math 인터페이스의 `max()` 메서드를 사용해서 가장 큰 값을 구합니다

그런데 가장 큰 값을 특정하기 어렵기에 각 변의 제곱을 모두 더한 값과 가장 큰 값의 제곱의 두 배와 비교했습니다.

삼항연산자를 사용하면 코드를 더욱 줄일 수 있습니다.





```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		while(true) {
			StringTokenizer st=new StringTokenizer(br.readLine());
			int a = Integer.parseInt(st.nextToken());
			int b = Integer.parseInt(st.nextToken());
			int c = Integer.parseInt(st.nextToken());
			if(a==0 && b==0 && c==0) break;
			int max = Math.max(a, Math.max(b, c));
			System.out.println(a*a + b*b + c*c == 2*max*max ? "right":"wrong");
		}
	}
}
```



<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

