---
title: "백준 알고리즘 - 10872번 팩토리얼"

date: 2021-10-03

last_modified_at: 2021-10-03

categories:
  - BOJ

tags:
  - [algorithm, 재귀, "10872"]
---

> # 팩토리얼 

| 시간 제한 | 메모리 제한 | 제출  | 정답  | 맞은 사람 | 정답 비율 |
| :-------- | :---------- | :---- | :---- | :-------- | :-------- |
| 1 초      | 256 MB      | 82751 | 40924 | 34299     | 49.981%   |

## 문제

0보다 크거나 같은 정수 N이 주어진다. 이때, N!을 출력하는 프로그램을 작성하시오.

## 입력

첫째 줄에 정수 N(0 ≤ N ≤ 12)이 주어진다.

## 출력

첫째 줄에 N!을 출력한다.

## 예제 입력 1 복사

```
10
```

## 예제 출력 1 복사

```
3628800
```

## 예제 입력 2 복사

```
0
```

## 예제 출력 2 복사

```
1
```



---



<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

> ## 해설

재귀 함수를 만들어서 진행하면 비교적 간단하게 풀 수 있습니다.

비슷한 문제를 처음 접했을 때에는 단순히 반복문을 사용해서 접근을 했는데, 다른 분들이 재귀 함수를 사용해서 풀이하신 것을 보고 다소 충격적이었던 기억이 있습니다.

나는 코드 라인이 이렇게 긴데... 저렇게 짧게 구현할 수도 있구나 싶은 마음이었던 것 같습니다.

코드는 간단한 편이니 우선 재귀를 사용하는 목적을 살펴보도록 하겠습니다.<br>

위키백과에서 설명하는 재귀의 정의는 다음과 같습니다.

> 컴퓨터 과학에 있어서 **재귀**(再歸, Recursion)는 **자신을 정의할 때 자기 자신을 재참조하는 방법**을 뜻하며, 이를 프로그래밍에 적용한 **재귀 호출**(Recursive call)의 형태로 많이 사용된다. 

<br>재귀함수를 사용하면 변수 사용이 줄어들고, 보통 가독성이 좋아지는 경우가 많습니다.

하지만 자칫 스택오버플로우(_사용 가능한 스택 메모리보다 더 많은 공간을 사용하려 할 경우 프로그램 충돌 가능_)가 발생할 수 있습니다.

그러므로 남용해서는 안되고 알고리즘이 재귀를 통한 표현이 적절하다고 판단되는 경우 사용하는 것이 좋은 것 같습니다.<br>

대표적인 경우가 이번 예제와 같은 팩토리얼, 피보나치 수열, 하노이의 탑 등이 있습니다.<br><br><br>







이제 문제를 살펴보겠습니다.

n이 주어졌을때 n보다 작거나 같은 모든 양의 정수의 곱을 팩토리얼이라고 합니다.<br>

10!를 예를 들면, 10 * 9 * 8 * 7 * ... * 1 과 같은 방식이 되겠죠.<br>

n * (n-1) * ((n-1)-1) ... 계속해서 자기 자신보다 1 작은 숫자를 곱하고, 곱한 숫자가 1이 될 때까지 반복하는 기능을 만들면 되겠습니다.<br>

포인트는 함수의 값을 반환할 때 자신에게 n-1을 넘겨서 자기 자신을 다시 호출하도록 하는 것 입니다.





팩토리얼 기능을 코드로 확인해보겠습니다.<br>

```java
private static int factorial(int n) {
    if (n <= 1)
        return n;
    else
        return factorial(n - 1) * n;
}
```

<br>위와 같이 팩토리얼 메서드를 만들 수 있습니다.<br>

다만, 문제에서는 `0`을 입력했을 때 `1`이 출력되었으니, 이 부분에 유의해서 작성한 메서드를 수정하시면 되겠습니다.<br>

저는 메서드 내 첫번째 조건문에 `if(n <= 1) return = 1;`와 같이 1보다 작거나 같은 값이 입력될 경우 1을 반환하도록 수정했습니다.<br>



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws NumberFormatException, IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        System.out.println(factorial(N));
    }

    private static int factorial(int n) {
        if (n <= 1)
            return 1;
        else
            return factorial(n - 1) * n;
    }
}
```





<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>
