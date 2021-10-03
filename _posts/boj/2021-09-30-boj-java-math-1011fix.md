---
title: "백준 알고리즘 - 1011번 Fly me to the Alpha Centauri "

date: 2021-09-27

last_modified_at: 2021-10-02

categories:
  - BOJ

tags:
  - [algorithm, "1011"]
---

# Fly me to the Alpha Centauri 

| 시간 제한 | 메모리 제한 | 제출  | 정답  | 맞은 사람 | 정답 비율 |
| :-------- | :---------- | :---- | :---- | :-------- | :-------- |
| 2 초      | 512 MB      | 67250 | 19464 | 15348     | 30.069%   |

## 문제

우현이는 어린 시절, 지구 외의 다른 행성에서도 인류들이 살아갈 수 있는 미래가 오리라 믿었다. 그리고 그가 지구라는 세상에 발을 내려 놓은 지 23년이 지난 지금, 세계 최연소 ASNA 우주 비행사가 되어 새로운 세계에 발을 내려 놓는 영광의 순간을 기다리고 있다.

그가 탑승하게 될 우주선은 Alpha Centauri라는 새로운 인류의 보금자리를 개척하기 위한 대규모 생활 유지 시스템을 탑재하고 있기 때문에, 그 크기와 질량이 엄청난 이유로 최신기술력을 총 동원하여 개발한 공간이동 장치를 탑재하였다. 하지만 이 공간이동 장치는 이동 거리를 급격하게 늘릴 경우 기계에 심각한 결함이 발생하는 단점이 있어서, 이전 작동시기에 k광년을 이동하였을 때는 k-1 , k 혹은 k+1 광년만을 다시 이동할 수 있다. 예를 들어, 이 장치를 처음 작동시킬 경우 -1 , 0 , 1 광년을 이론상 이동할 수 있으나 사실상 음수 혹은 0 거리만큼의 이동은 의미가 없으므로 1 광년을 이동할 수 있으며, 그 다음에는 0 , 1 , 2 광년을 이동할 수 있는 것이다. ( 여기서 다시 2광년을 이동한다면 다음 시기엔 1, 2, 3 광년을 이동할 수 있다. )

![img](https://www.acmicpc.net/upload/201003/rlaehdgur.JPG)

김우현은 공간이동 장치 작동시의 에너지 소모가 크다는 점을 잘 알고 있기 때문에 x지점에서 y지점을 향해 최소한의 작동 횟수로 이동하려 한다. 하지만 y지점에 도착해서도 공간 이동장치의 안전성을 위하여 y지점에 도착하기 바로 직전의 이동거리는 반드시 1광년으로 하려 한다.

김우현을 위해 x지점부터 정확히 y지점으로 이동하는데 필요한 공간 이동 장치 작동 횟수의 최솟값을 구하는 프로그램을 작성하라.

## 입력

입력의 첫 줄에는 테스트케이스의 개수 T가 주어진다. 각각의 테스트 케이스에 대해 현재 위치 x 와 목표 위치 y 가 정수로 주어지며, x는 항상 y보다 작은 값을 갖는다. (0 ≤ x < y < 231)

## 출력

각 테스트 케이스에 대해 x지점으로부터 y지점까지 정확히 도달하는데 필요한 최소한의 공간이동 장치 작동 횟수를 출력한다.

## 예제 입력 1 복사

```
3
0 3
1 5
45 50
```

## 예제 출력 1 복사

```
3
3
4
```



---

> ## 해설

문제를 이해하는 것 부터 어려움이 있었던 것 같습니다.

함께 알고리즘 스터디를 하고 있는 김선생님의 글을 참고했습니다.([김선생님 블로그](https://kbj96.tistory.com/))

저희는 x에서 y지점까지 도달하는 최소한의 동작 횟수를 구해야 합니다.

그런데 이동할 수 있는 범위는 이전 이동 거리(n)를 기점으로 n-1, n, n+1 만큼의 거리만큼만 이동할 수 있고, 최종 목적지에 도달하기 직전에는 반드시 1만큼의 이동거리를 가져야 합니다.

여기까지만 해도 괜찮을 것 같은데 여기서 추가적으로 감소하는 거리에도 n-1, n, n+1만큼만 이동이 가능하기 때문에, -1만큼씩 줄여서 마지막 이동거리가 1이 되게끔 해야 합니다.

주어진 거리를 통해 최소 이동 회수를 구해야 하는데, 문제만으로는 방법을 생각하기 어려워서 직접 값을 넣어보면서 확인해봤습니다.

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/135435856-97767c89-6b08-43a8-94a3-281bb735d71c.png" alt="image" style="zoom:50%;" /></p>

생각보다 아이디어가 떠오르지 않아서 17번째까지 작성을 했습니다.

첫번째의 Distance가 저희에게 주어진 입력값이고, 가운데에는 각 횟수별 이동값입니다(노란 강조 부분은 최대 이동값입니다). 마지막은 결과값인 최소 이동횟수(Count)입니다.

카운트가 증가하는 숫자값들을 보시면 해당 거리의 수가 1, 2, 4, 6, 9, 12, 16... 과 같습니다.

일단 힌트를 더 찾아보기 위해 파란색으로 하이라이트한 부분들만 보겠습니다.

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/135448120-1c32aa40-33dd-493d-b218-3b1ad0048154.png" alt="image" style="zoom:67%;" /></p>

최대이동거리를 n이라고 가정하겠습니다. 

위의 표를 보시면 홀수의 카운트에서 총 이동거리가 n의 제곱이 되는 것을 보실 수 있습니다.

짝수에서는 \\(n^2+n\\) 이 되는 것 같은데 제곱이 키워드가 되는 것 같으니 여기에 중점을 두고 최대이동거리와 카운트의 관계를 하나씩 살펴보겠습니다.

총 이동거리가 1부터 하나씩 증가하면 카운트는 `2n-1`, `2n`, `2n+1`,`2n-1`,`2n`,`2n`,`2n+1`... 와 같은데 `2n-1`, `2n`, `2n+1`이 반복되는 것 같습니다.

자세히 살펴보면 총 이동거리가 \\(n^2\\)과 같다면 카운트는 `2n-1`, 총 이동거리가 \\(n^2\\)보다 크고 \\(n^2+n\\)보다 작다면 `2n`,  \\(n^2+n\\)보다 크다면 `2n+1`이 된다는 것을 알 수 있습니다.



지금까지 찾아낸 정보들을 취합하면 아래와 같이 이동거리와 최대이동거리의 관계에 따라 카운트가 결정됩니다.

<img src="https://user-images.githubusercontent.com/70495425/135478549-43cd4329-ec1f-40b6-b0ba-dab72ee283c6.png" alt="image" style="zoom:50%;" />

코드를 통해 확인해보도록 하겠습니다.

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
			int x = Integer.parseInt(st.nextToken());
			int y = Integer.parseInt(st.nextToken());
			flyToAlpha(y - x);
		}
	}

	private static void flyToAlpha(int x) {
		int n = (int) Math.sqrt(x);
		if (x == n * n) {
			System.out.println(2 * n - 1);
		} else if (n * n < x && x <= n*n+n) {
			System.out.println(2 * n);
		} else {
			System.out.println(2 * n + 1);
		}
	}
}
```





<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

