---
title: "Heap & Tree(1)"

date: 2022-03-20
last_modified_at: 2022-03-20


categories:
  - DataStructure

tags:
  - ["부스트코스", "자바로 구현하고 배우는 자료구조"]

---

<br>

- [용어 소개](#용어-소개)

- [Heap](#heap)
  
  - [Add, Remove](#add,-remove)
  - [TrickleUp Method](#trickleup-method)
  - [TrickleDown Method](#trickledown-method)
  - [Sorting](#sorting)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> # 용어 소개

이번 강의에서는 Heap과 Tree에 대해 배울 예정입니다.

본격적인 내용 진행에 앞서 용어들을 간단히 살펴보겠습니다.

![image](https://user-images.githubusercontent.com/70495425/159165695-08ea8c30-a890-4e3b-8d1f-9e4f5cbe25f3.png)

(출처: [트리(그래프) - 나무위키](https://namu.wiki/w/%ED%8A%B8%EB%A6%AC(%EA%B7%B8%EB%9E%98%ED%94%84)))

위의 그림은 Tree를 나타내고 있습니다.(_일반적인 나무 모양을 뒤집어서 생각하시면 됩니다_)

가계도처럼 <u>노드를 나무 형태로 연결한 구조</u>를 트리라고 합니다. 트리에 있는 각각의 요소는 <u>노드</u>입니다. 위 사진에서처럼 노드는 부모, 자식 형태로 이어집니다.

- <u>root</u>: 가장 중요한 <u>트리의 시작 부분</u>입니다. 트리는 항상 `root`를 통해 탐색하게 됩니다.

- <u>leaf/leaves</u>: 자식이 딸려있지 않은 말단 부분입니다.

- <u>edge</u>: 두 노드를 연결하는 선입니다. 뿌리로부터의 간선의 수에 따라 **level**을 나눕니다.

- siblings: 동일한 부모를 갖는 형제 노드들을 말합니다.

- level: 깊이를 나타내는 노드의 집합입니다. 뿌리부터 시작합니다.

<br>

<br>

> # Heap

힙은 트리와 비슷한 구조를 가지는데, 최대 힙과 최소 힙 두 가지 종류가 있습니다.

- 최대 힙(Max Heap): 부모 노드 > 자식 노드

- 최소 힙(Min Heap): 부모 노드 < 자식 노드

![image](https://user-images.githubusercontent.com/70495425/159165191-5883470b-4bda-4244-b481-843f737cf102.png)

(왼쪽: 최대 힙 - *숫자 12를 부모로 갖는 level3에서 노드가 꽉 차야 합니다*, 오른쪽: 최소 힙)

힙은 <u>최댓값 및 최솟값을 찾아내는 연산을 빠르게 하기 위해 고안된 완전이진트리를 기반</u>으로 한 자료구조입니다. (_완전이진트리란, 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져 있으며, 마지막 레벨의 모든 노드는 가능한 한 가장 왼쪽에 있는 트리_)

그리고 최대 힙에서는 노드의 개수를 통해 높이를 알 수 있습니다.

![image](https://user-images.githubusercontent.com/70495425/159165779-26edab57-4f57-4e73-8940-2e72b4d00556.png)

![image](https://user-images.githubusercontent.com/70495425/159165784-636b148f-12ec-491f-9339-b01320885623.png)

(_노드 개수(n)을 이용해 `level`, `height`과  `log(n+1)-1`의 값이 같다는 것을 알 수 있음_)

<br>

<br>

> ## Add, Remove

앞서 힙에 대해 설명할 때 규칙에 따라 부모 노드와 자식 노드의 위치가 정해진다고 했습니다. 그래서 새로운 데이터를 추가하거나 제거할 때에도 힙의 규칙을 지켜야 합니다. 

<u>최대 힙이면 부모 노드가 자식 노드보다 커야 하고 최소 힙은 자식 노드가 부모 노드보다 커야 합니다.</u>

   

*노드 추가(Trickle Up)*

<img title="" src="https://user-images.githubusercontent.com/70495425/159166435-3a297a3a-c303-42cb-bdfe-a9db88aa5da5.png" alt="image" width="329">

그림과 같이 최대 힙이 있을 때, 새로운 노드로 27을 추가하려고 합니다.

그런데 27은 부모 노드인 10보다 크기 때문에 규칙을 위반하게 되겠죠.

그래서 문제를 해결하기 위해 10과 27의 위치를 바꾸면, 다시 부모 노드와 값을 비교해서 규칙을 지키도록 하면 됩니다.

 ![](https://cphinf.pstatic.net/mooc/20210525_22/1621923502381xjAl3_PNG/mceclip0.png)

1. 비어있는 공간에 노드를 추가
2. 부모 노드보다 큰 숫자인지 확인하고 만약 그렇다면 두 노드를 교환

<br>

추가를 알아봤는데, 제거는 어떻게 하면 될까요?

힙에서의 제거는 특정 요소를 지정할 수 없고, <u>항상 루트 요소를 제거</u> 합니다.

루트를 제거한 뒤에는 규칙을 준수하며 빈 자리를 메꾸면 되겠습니다.

_노드 제거(Trickle Down)_

 ![](https://cphinf.pstatic.net/mooc/20210525_285/1621923648003hSn4O_PNG/mceclip1.png)

1. 루트 제거
2. 트리의 마지막 요소를 루트에 삽입
3. 루트에서 시작해 두 자식 중 큰 노드와 바꿔 힙 규칙 준수

<br>

<br>

> ## TrickleUp Method

힙은 완전이진트리이기 때문에 노드의 위치 다음과 같은 성질을 가집니다.  

_인덱스는 0부터 시작하며 root - left - right 순으로 지정하면 아래의 식을 만족합니다_
children: `2 * parent + 1` or `2 * parent + 2` 
parent: `floor((child-1)/2)`

<img title="" src="https://user-images.githubusercontent.com/70495425/159167306-71e9f6e6-a459-40ac-a764-f223ac6e90a0.png" alt="image" width="274">

   

이 성질을 이용하여 노드 추가 과정을 코드로 작성하면 다음과 같습니다.  

```java
int lastposition; // 루트로부터 떨어진 거리, 즉 요소를 어디까지 넣었는지 기록
E[] array = (E[]) new Object[size];
public void add(E obj){
    array[++lastposition] = obj; // 1. 노드 추가
    trickleup(lastposition); // 2. 규칙 준수 확인
}
public void trickleup(int position){
    if (position == 0)
        return;
    int parent = (int) Math.floor((position-1)/2)
    if (((Comparable<E>) array[position]).compareTo(array.parent) > 0) {
        swap(position, parent);
        trickleup(parent);
    }
}
public void swap(int from, int to){
    E tmp = array[from];
    array[from] = array[to];
    array[to] = tmp;
}
```

<br>

> ## TrickleDown Method

힙에서 제거는 루트의 제거를 의미한다고 설명드렸습니다.

루트를 제거한 뒤에도 규칙을 만족하기 위해서는 힙에 있는 마지막 요소로 빈 구멍을 메우고, 자식 노드와 비교해서 교체하는 작업을 반복하면 됩니다. 작업이 종료되는 기준은 배열의 길이(`lastposition`)보다 `2*parent+1`, `2*parent+2`의 결과값이 큰 경우 종료하게 됩니다.(*다만, 자식이 하나만 존재하는 경우도 고려해야 합니다*)

루트 제거 과정을 코드로 작성하면 다음과 같습니다.

```java
public E remove(){
    E tmp = array[0]; // 루트를 바로 제거하지 않고 스왑함으로써 remove 반복 시 오름차순으 정렬
    swap(0, lastposition--); // 스왑 이후 lastposition을 감소시켜 배열에서 제거
    trickleDown(0);
    return tmp;
}
public void trickleDown(int parent){
    int left = 2*parent + 1;
    int right = 2*parent + 2;
    // 왼쪽 자식이 마지막 요소인 경우
    if (left==lastposition && 
            (((Comparable<E>)array[parent]).compareTo(array[left]) < 0){
        swap(parent, left)
        return;
    }
    // 오른쪽 자식이 마지막 요소인 경우
    if (right==lastposition && 
            (((Comparable<E>)array[parent]).compareTo(array[right]) < 0){
        swap(parent, right)
        return;
    }
    // 자식이 마지막 요소와 같거나 큰 경우
    if (left >= lastposition || right >= lastposition)
        return;
    // 왼쪽 자식이 클 때
    if (array[left] > array[right] && array[parent] < array[left]) {
        swap(parent, left);
        trickleDown(left);
    }
    // 오른쪽 자식이 클 때
    else if (array[parent] < array[right]){
        swap(parent, right);
        trickleDown(right);
    }
}
```

<br>

<br>

> ## Sorting

힙 규칙에 맞게 숫자의 순서를 정렬하는 힙 정렬 알고리즘을 살펴보겠습니다.

임의의 숫자들을 나열한 뒤 힙 규칙에 맞게 `TrickleDown`을 반복하면 숫자가 정렬됩니다.(루트 제거 시 바로 제거하지 않고, `swap`을 사용한 이유)

<img title="" src="https://user-images.githubusercontent.com/70495425/159169341-586ce731-1263-4db1-b0f9-dd78a3341b88.png" alt="image" width="295">

(`root`가 22였던 최대 힙에서 `TrickleDown`을 실행했을 때 내부적으로 동작하는 첫 번째 과정. 22를 바로 제거하지 않고, 마지막 요소와 `swap`한 뒤 연결 해제)

<img title="" src="https://user-images.githubusercontent.com/70495425/159169935-335b6f69-9838-48bf-a6c4-1e943b935bcc.png" alt="image" width="294">

<img title="" src="https://user-images.githubusercontent.com/70495425/159169971-6110bc6f-aee4-4dd1-a398-639592c05861.png" alt="image" width="295">

위 과정을 반복하면 정렬이 이뤄집니다.

1. `22 | 17 | 19 | 12 | 15 | 11 | 7 | 6 | 9 | 10 | 5`

2. `5 | 17 | 19 | 12 | 15 | 11 | 7 | 6 | 9 | 10 | 22(X)`

3. `19 | 17 | 11 | 12 | 15 | 5 | 7 | 6 | 9 | 10 | 22(X)`

4. `10 | 17 | 11 | 12 | 15 | 5 | 7 | 6 | 9 | 19(X) | 22(X)`

5. `17 | 15 | 11 | 12 | 10 | 5 | 7 | 6 | 9 | 19(X) | 22(X)`

6. `9 | 15 | 11 | 12 | 10 | 5 | 7 | 6 | 17(X) | 19(X) | 22(X)`

7. ...

8. `5 | 6(X) | 7(X) | 9(X) | 10(X) | 11(X) | 12(X) | 15(X) | 17(X) | 19(X) | 22(X)`

힙 정렬 알고리즘은 두 수를 비교해서 하나를 고르기 때문에 `O(nlogn)`의 시간 복잡도를 가집니다.

또한, 힙 정렬은 숫자들의 순서를 바꿔 정렬하기 때문에 데이터의 복사본을 만들 필요가 없다는 장점이 있습니다.

<br>

<br>

> ### 참고

[자바로 구현하고 배우는 자료구조 > 4.힙(Heap) & 트리(Tree) : 부스트코스](https://www.boostcourse.org/cs204/joinLectures/212812?isDesc=false)

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
