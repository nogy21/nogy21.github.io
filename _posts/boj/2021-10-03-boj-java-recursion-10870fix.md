---
title: "백준 알고리즘 - 10870번 피보나치 수 5"

date: 2021-10-04

last_modified_at: 2021-10-04

categories:
  - BOJ

tags:
  - [algorithm, 재귀, "10870"]
---

> # 피보나치 수 5 

| 시간 제한 | 메모리 제한 | 제출  | 정답  | 맞은 사람 | 정답 비율 |
| :-------- | :---------- | :---- | :---- | :-------- | :-------- |
| 1 초      | 256 MB      | 46739 | 29456 | 25653     | 63.744%   |

## 문제

피보나치 수는 0과 1로 시작한다. 0번째 피보나치 수는 0이고, 1번째 피보나치 수는 1이다. 그 다음 2번째 부터는 바로 앞 두 피보나치 수의 합이 된다.

이를 식으로 써보면 Fn = Fn-1 + Fn-2 (n ≥ 2)가 된다.

n=17일때 까지 피보나치 수를 써보면 다음과 같다.

0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, 987, 1597

n이 주어졌을 때, n번째 피보나치 수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 n이 주어진다. n은 20보다 작거나 같은 자연수 또는 0이다.

## 출력

첫째 줄에 n번째 피보나치 수를 출력한다.

## 예제 입력 1 복사

```
10
```

## 예제 출력 1 복사

```
55
```

---



<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

> ## 해설

문제란을 보시면 피보나치 수에 대한 설명이 있습니다.

0번째와 1번째를 제외한 2번째 수부터 앞 두 피보나치 수의 합으로 구성되는 수열입니다.

**\\(F_n = F_{n-1} + F_{n-2} (n ≥ 2)\\)**

저희는 입력값의 위치에 해당하는 수열을 출력하도록 해야 합니다.<br>

예를 들어, 열 번째 위치에 해당하는 수를 출력하고자 할 때 

**\\(F_{10}\\)**은 **\\(F_{9}+F_8\\)**이고,

**\\(F_9\\)**와 **\\(F_8\\)**은 다시 각각 **\\(F_8+F_7\\)**과 **\\(F_7+F_6\\)**과 같고 **\\(F_1+F_0\\)**이 될 때 까지 반복하면 답을 구할 수 있게 됩니다.

<br>

어렵게 생각할 필요 없이 이전 문제였던 팩토리얼을 떠올리시면 이번 문제 역시 쉽게 푸실 수 있을 것 같습니다.

포인트는 함수의 값을 반환할 때 자신에게 n-1과 n-2를 넘겨서 자기 자신을 다시 호출하도록 하는 것 입니다.





피보나치 수열의 기능을 코드로 확인해보겠습니다.<br>

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws NumberFormatException, IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        System.out.println(FibonacciSeq(N));
    }

    private static int FibonacciSeq(int n) {
        if (n <= 1) {
            return n;
        } else
            return FibonacciSeq(n - 1) + FibonacciSeq(n - 2);
    }
}

```

<br>위와 같이 피보나치 메서드를 만들 수 있습니다.<br>

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>
