---
title: "백준 알고리즘 - 1002번 터렛"

date: 2021-10-02

categories:
  - BOJ

tags:
  - [algorithm, "1002", 수학]
---

# 터렛 

| 시간 제한 | 메모리 제한 | 제출   | 정답  | 맞은 사람 | 정답 비율 |
| :-------- | :---------- | :----- | :---- | :-------- | :-------- |
| 2 초      | 128 MB      | 126224 | 25394 | 20041     | 21.004%   |

## 문제

조규현과 백승환은 터렛에 근무하는 직원이다. 하지만 워낙 존재감이 없어서 인구수는 차지하지 않는다. 다음은 조규현과 백승환의 사진이다.

<img src="https://user-images.githubusercontent.com/70495425/135625846-01f0d91d-25fb-4c22-899a-421785ec88a5.png" alt="image" style="zoom:67%;" />

이석원은 조규현과 백승환에게 상대편 마린(류재명)의 위치를 계산하라는 명령을 내렸다. 조규현과 백승환은 각각 자신의 터렛 위치에서 현재 적까지의 거리를 계산했다.

조규현의 좌표 (x1, y1)와 백승환의 좌표 (x2, y2)가 주어지고, 조규현이 계산한 류재명과의 거리 r1과 백승환이 계산한 류재명과의 거리 r2가 주어졌을 때, 류재명이 있을 수 있는 좌표의 수를 출력하는 프로그램을 작성하시오.

## 입력

첫째 줄에 테스트 케이스의 개수 T가 주어진다. 각 테스트 케이스는 다음과 같이 이루어져 있다.

한 줄에 x1, y1, r1, x2, y2, r2가 주어진다. x1, y1, x2, y2는 -10,000보다 크거나 같고, 10,000보다 작거나 같은 정수이고, r1, r2는 10,000보다 작거나 같은 자연수이다.

## 출력

각 테스트 케이스마다 류재명이 있을 수 있는 위치의 수를 출력한다. 만약 류재명이 있을 수 있는 위치의 개수가 무한대일 경우에는 -1을 출력한다.

## 예제 입력 1 복사

```
3
0 0 13 40 0 37
0 0 3 0 7 4
1 1 1 1 1 5
```

## 예제 출력 1 복사

```
2
1
0
```

---

이번 문제는 좌표평면 위의 두 점에서 각각 일정 거리만큼 떨어진 하나의 점의 좌표를 구하는 문제입니다.

입력값을 보시면 x1, y1, r1과 x2, y2, r2를 한 줄에 공백을 두고 입력받습니다.

좌표 평면 위에서 이 값들의 의미를 생각해보면, (x1,y1) 위치에서 r1(반지름)만큼의 거리에 떨어진 점들의 집합은 원이 되겠죠. (x2, y2), r2 역시 마찬가지일 겁니다. 

이렇게 두 개의 원이 저희가 찾는 류재명이 있을 수 있는 위치가 됩니다. 

하지만 한 사람이 동시에 여러 곳에 위치할 수 없으니 존재할 수 있는 가능성은 두 원의 접점, 즉 두 원이 겹치는 부분에서만 존재하게 됩니다.

원이 겹치는 경우의 수를 생각해보면 크게 네 개의 상황을 생각해 볼 수 있을 것 같습니다.

|                             상황                             |  값  |
| :----------------------------------------------------------: | :--: |
|                   원의 두 점이 겹치는 경우                   |  2   |
|           원이 한 점에서만 겹치는 경우(외접, 내접)           |  1   |
|           두 원이 일치하는 경우(중심, 반지름 일치)           |  -1  |
| 만나지 않는 경우(원 밖에 존재 or 중심 일치 반지름 다른 경우) |  0   |



먼저 조건이 비교적 간단한 두 원이 일치하는 경우를 확인하겠습니다.

<img src="https://user-images.githubusercontent.com/70495425/135704944-4e050e6e-c398-47d0-bedc-bc06a0642e95.png" alt="동일한 원" style="zoom:80%;" />

​	1.원의 중심 A(x1, y1), B(x2, y2)가 일치하고 반지름 r1, r2가 일치하는 경우 동일한 원이 나오게 됩니다.

> `x1==x2 && y1==y2 && r1==r2`인 경우 `-1` 출력

<br>

두 원의 위치 관계는 원의 중심으로부터의 거리(d)와 반지름(r)을 이용하면 표현할 수 있습니다.

전에 풀었던 택시 기하학 문제에서 나왔던 유클리드 기하학에서의 두 좌표 사이의 거리를 구하는 공식을 이용해서 진행합니다. d의 길이는 두 원의 중심으로부터의 거리입니다.

> `두 원의 중심으로부터의 거리(d)` = \\(\sqrt{(x1-x2)^2+(y1-y2)^2} \\)

그런데 루트를 사용할 경우 double형 계산으로 오차값이 생겨날 수 있기에 코드를 진행할 때에는 d의 비교값에 제곱을 해서 진행하도록 하겠습니다.

<br>





접점이 존재하지 않는 경우를 살펴보겠습니다.

<img src="https://user-images.githubusercontent.com/70495425/135708165-397f48c7-13a2-4c54-8649-1338b8725da3.png" alt="image" style="zoom:67%;" /><img src="https://user-images.githubusercontent.com/70495425/135708182-28bfa9d0-ed4b-4174-b024-c3cf223d4c9c.png" alt="image" style="zoom: 67%;" />

​	2-1.동심원(원의 중심이 같고 반지름이 다른 경우)

>`x1==x2 && y1==y2 && r1!=r2`인 경우 0 출력

​	2-2.멀리 떨어진 원(두 원의 중심으로부터의 거리가 두 원의 반지름의 합보다 큰 경우)

> `d > r+r'`인 경우 0 출력

2번 케이스를 2-1과 2-2로 한정짓고 진행을 해서 어려움을 겪었었습니다. 

2-3의 경우는 다음의 그림과 같이 중점이 동일하지 않더라도 원 안에 다른 원이 내접하지 않은 상태로 존재 할 수 있습니다.

<img src="https://user-images.githubusercontent.com/70495425/135708147-2a42395b-0f61-4bd7-868b-7be24421c59e.png" alt="image" style="zoom:67%;" />

​	2-3.큰 원 안에 작은 원(두 원의 중심으로부터의 거리가 두 원의 반지름의 차보다 작은 경우)

> `d < r-r'`인 경우 0 출력





<br>

이제 접점이 하나인 경우를 살펴보겠습니다.

하나의 접점은 큰 원 안에 작은 원이 내접하거나, 바깥의 한 점에서 외접하는 경우가 있겠습니다.

<br>

외접하는 경우부터 살펴보겠습니다.

<img src="https://user-images.githubusercontent.com/70495425/135708469-1e1f6864-88bf-42da-a9ce-1a25078978b4.png" alt="image" style="zoom:67%;" />

​	3.1.외접(두 원의 중심으로부터의 거리와 두 반지름의 합이 일치)

> `d == r+r'`인 경우 1 출력

다음은 내접하는 경우입니다.

<img src="https://user-images.githubusercontent.com/70495425/135708487-52bd8131-6524-489a-aa87-b66c92caeed5.png" alt="image" style="zoom:67%;" />

​	3.2.내접(두 원의 중심으로부터의 거리가 두 반지름의 차과 동일한 경우)

> `d == r-r'`인 경우 1 출력

여기서 두 반지름의 차를 구할 때 어떤 반지름이 더 큰 값을 가질 지 알 수 없기에 절대값을 씌워야 합니다.

그런데 저는 double형 계산으로 인한 오차를 막기 위해 d를 제곱해서 진행할 예정이기에 절대값 사용 없이 진행하도록 하겠습니다.





마지막으로 두 접점이 생기는 경우는 아래와 같은데, 두 개의 원이 가질 수 있는 접점은 0, 1, 2개 뿐이지 다른 경우의 수를 모두 명확하게 확인했다면 남은 상황이 모두 두개의 접점이 생기는 상황이 되겠습니다.

<img src="https://user-images.githubusercontent.com/70495425/135708489-afe9e8a9-e69f-4978-8357-3be530549c94.png" alt="image" style="zoom:67%;" />

<br><br>

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {
	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int T = Integer.parseInt(br.readLine());
		for (int i = 0; i < T; i++) {
			StringTokenizer st = new StringTokenizer(br.readLine());
			int x1 = Integer.parseInt(st.nextToken());
			int y1 = Integer.parseInt(st.nextToken());
			int r1 = Integer.parseInt(st.nextToken());
			int x2 = Integer.parseInt(st.nextToken());
			int y2 = Integer.parseInt(st.nextToken());
			int r2 = Integer.parseInt(st.nextToken());

			System.out.println(findLocation(x1, y1, r1, x2, y2, r2));
		}
	}

	private static int findLocation(int x1, int y1, int r1, int x2, int y2, int r2) {
		int d = (int) (Math.pow(x2 - x1, 2) + Math.pow(y2 - y1, 2));
		// 1. 반지름과 원의 중심이 동일한 원
		if (x1 == x2 && y1 == y2 && r1 == r2) return -1;
		// 2-1. 중심 동일, 반지름이 다른 경우
		else if(x1==x2&&y1==y2)	return 0;
		// 2-2. 큰 원 안에 작은 원, 내접하지 않음 (중심으로부터의 거리가 반지름의 차보다 작은 경우)
		else if (d < Math.pow(r1 - r2, 2)) return 0;
		// 2-3. 서로 멀리 떨어진 원 (중심으로부터의 거리가 반지름의 합보다 큰 경우)
		else if (d > Math.pow(r1 + r2, 2)) return 0;
		// 3-1. 접점이 하나인 경우(외접)
		else if (d == Math.pow(r1 + r2, 2)) return 1;
		// 3-2. 접점이 하나인 경우(내접)
		else if (d == Math.pow(Math.abs(r1 - r2), 2)) return 1;
		// 접점이 둘
		else return 2;
	}
}
```





[이방인님(ST)의 블로그](https://st-lab.tistory.com/)를 보고 많이 배웠습니다. 2-3번 조건을 찾지 못할 때 풀이를 참고했는데, double형 연산 시 오차 가능성 또한 염두하지 않고 진행했었다는 것을 깨달았습니다.

비단 이번 문제 뿐 아니라 종종 이방인님 글을 통해 많이 배우곤 했습니다. 그동안 대수롭지 않게 글을 보고 지나쳐왔다는 생각이 문득 들어서 감사한 마음을 담아 글 말미에 짧게나마 적어봅니다.

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>
