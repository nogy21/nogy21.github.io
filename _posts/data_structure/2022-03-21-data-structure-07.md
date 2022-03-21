---
title: "Heap & Tree(2)"

date: 2022-03-21
last_modified_at: 2022-03-21


categories:
  - DataStructure

tags:
  - ["부스트코스", "자바로 구현하고 배우는 자료구조"]

---

<br>

- [Tree](#tree)
  - [Complete Tree & Full Tree](#complete-tree-&-full-tree)
  - [Tree Traversal](#tree-traversal)
  - [Expression Trees](#expression-trees)
  - [Node Class](#node-class)
  - [Recursive Add Methods](#recursive-add-methods)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> # Tree

이전 글에서 소개해드린 Tree의 용어 설명입니다. 간단히 보고 지나가겠습니다.

![image](https://user-images.githubusercontent.com/70495425/159165695-08ea8c30-a890-4e3b-8d1f-9e4f5cbe25f3.png)

(출처: [트리(그래프) - 나무위키](https://namu.wiki/w/%ED%8A%B8%EB%A6%AC(%EA%B7%B8%EB%9E%98%ED%94%84)))

가계도처럼 <u>노드를 나무 형태로 연결한 구조</u>를 트리라고 합니다. 트리에 있는 각각의 요소는 <u>노드</u>입니다. 위 사진에서처럼 노드는 부모, 자식 형태로 이어집니다.

- <u>root</u>: 가장 중요한 <u>트리의 시작 부분</u>입니다. 트리는 항상 `root`를 통해 탐색하게 됩니다.

- <u>leaf/leaves</u>: 자식이 딸려있지 않은 말단 부분입니다.

- <u>edge</u>: 두 노드를 연결하는 선입니다. 뿌리로부터의 간선의 수에 따라 **level**을 나눕니다.

- siblings: 동일한 부모를 갖는 형제 노드들을 말합니다.

- level: 깊이를 나타내는 노드의 집합입니다. 뿌리부터 시작합니다.

<br>

<br>

> ## Complete Tree & Full Tree

두 종류의 트리를 살펴보겠습니다.

완전 트리와 정 트리입니다.

![image](https://user-images.githubusercontent.com/70495425/159258423-8b5e52e5-5fbe-4979-8ccb-dfdd3d10a040.png)

완전 트리 (Complete Tree): <u>모든 잎이 아닌 노드가 2개의 자식 노드를 가지고</u> 있고 <u>마지막 줄은 왼쪽에서 오른쪽 순서로 채워져 있는 트리</u>입니다.

![image](https://user-images.githubusercontent.com/70495425/159258445-708d28f2-92b4-4afa-b1c0-3e14fa550c1b.png)

정 트리 (Full Tree): <u>모든 잎이 아닌 노드가 2개의 자식 노드를 가지고 </u>있고 <u>모든 잎이 같은 레벨에 있는 트리</u>입니다.

<br>

<br>

> ## Tree Traversal

트리를 순회하는 방법을 알아보겠습니다. 

먼저 다른 모든 노드를 지나기 전(Pre)에 루트 노드를 방문하는 전위 순회입니다.

<img src="https://user-images.githubusercontent.com/70495425/159265113-f22db5a0-a262-48a6-892b-924726f507c1.png" title="" alt="image" width="377">

- 전위 순회 (Pre order traversal)
  : 루트 노드 시작 -> 왼쪽 자식 노드 -> 오른쪽 자식 노드
- 중위 순회 (In order traversal)
  : 왼쪽 자식 노드 시작 -> 루트 노드 -> 오른쪽 자식 노드
- 후위 순회 (Post order traversal)
  : 왼쪽 자식 노드 시작 -> 오른쪽 자식 노드 -> 루트 노드
- 너비 우선 선회/레벨 순서 순회 (Breadth first traversal/level order traversal)
  : 가장 위에 있는 노드 시작 -> 왼쪽 자식에서 오른쪽 자식(같은 레벨) -> 아래로

<img title="" src="https://user-images.githubusercontent.com/70495425/159265840-60ea539b-4aeb-4ed3-818e-697d4f2ce6c5.png" alt="image" width="363">

(_노란색은 중위 순회, 초록색은 후위 순회입니다_)

<br>

<br>

> ## Expression Trees

표현식 트리

<img src="https://user-images.githubusercontent.com/70495425/159266649-497f2bd8-e7ff-49c2-b4ef-d50ed303d524.png" title="" alt="image" width="181">

알고리즘 문제 또는 정보처리기사 자격증 공부를 하다 보면 전위 표기식, 후위 표기식을 들어보셨을 것 같습니다.

표현식 트리를 활용하면 복잡한 식도 트리 형식으로 표현할 수 있습니다.

<img title="" src="https://user-images.githubusercontent.com/70495425/159267241-56df6514-446b-48be-b523-89fa58a2c603.png" alt="image" width="393" data-align="center">

중위 표기식: `( ( (22 / 11) + 3) * (6 + 5) ) - 50`

후위 표기식: `22 11 / 3 + 6 5 + * 50 -`

후위 표기식은 저희가 흔히 사용하는 중위 표기식과 달리 우선순위를 고려할 필요가 없습니다. 후위/전위 표기식은 전체 방정식을 즉시 해석할 수 있는 컴퓨터에게 유리한 표현법입니다.

<br>

<br>

> ## Node Class

트리에서 노드는 항상 작은 것이 왼쪽에 위치하도록 합니다.

<img src="https://user-images.githubusercontent.com/70495425/159269332-7631f3e7-bf17-47e0-979d-1cff40081b44.png" title="" alt="image" width="369">

요소를 탐색할 때, 예를 들어 위의 그림에서 12를 찾는다고 하면 12는 루트 노드인 10보다 크기 때문에 오른쪽을 확인합니다. 

왼쪽에는 10보다 작은 요소만 존재하기에 아예 고려할 필요가 없어지는 것이지요. 

다시 12와 15를 비교한 후 더 작은 값이므로 왼쪽을 탐색하면 12가 나오게 됩니다.

<br>

그래서 트리를 따라 내려가는 과정은 탐색 범위가 절반씩 줄어들기 때문에 `logN`의 시간복잡도를 가지게 됩니다.

코드는 어떻게 작성할 수 있을까요?

예전에 연결리스트를 공부하며 배웠던 노드 개념을 확장하면 코드의 작성이 가능합니다.

<img src="https://user-images.githubusercontent.com/70495425/159270696-f8466c16-e7be-45de-b05a-eec77b7a93e1.png" title="" alt="image" width="234">

연결 리스트에서는 노드가 `next` 포인터와 `data`를 갖고 있었는데, 트리에서는 `left`와 `right` 포인터, `data`를 가지는 형태입니다.

사실 `leaf` 노드까지 탐색한 뒤에는 부모 노드로 되돌아가야 하므로 `parent` 포인터도 필요합니다. 그런데 `parent` 포인터가 존재할 경우, 3개의 포인터(_부모 노드와 자식 노드 2개_)가 같은 노드를 가리키는 상황이 발생할 수 있습니다. 

그래서 이번에는 간단하게 `left`, `right` 포인터만 갖는 형태로 코드로 표현해보겠습니다.

```java
class Node<E> {
    E data;
    Node <E> left, right;

    public Node(E obj) {
        this.data = obj;
        left = right = null;
    }    
}
```

<br>

<br>

> ## Recursive Add Methods

트리에 새로운 데이터를 추가하는 재귀 함수를 알아보겠습니다.

<img title="" src="https://user-images.githubusercontent.com/70495425/159272631-e938d679-ce7e-4950-bb4b-0c45a8a5cf08.png" alt="image" width="310">

그림과 같이 7개의 노드를 갖는 트리에 20을 추가해보겠습니다.

우선 20을 `root`인 10과 비교하고, 20이 더 큰 값이므로 오른쪽으로 내려갑니다.

다시 15와 비교한 뒤 더 큰 값이므로 오른쪽으로 내려가고, 18과 비교한 뒤 다시 오른쪽으로 내려갑니다.

현재 18은 자식 노드가 없으므로 18의 `right` 포인터는 `null`을 가리키는 것 과 같습니다. `null`인 부분에 20이라는 노드를 추가하면 되겠습니다.

<br>

이렇게 <u>트리에 데이터를 추가</u>할 때에는 항상 맨 아래의 <u>잎 노드에 딸린 빈 노드에 추가</u>하게 됩니다.

얼만큼을 내려가야 빈 노드가 나오는지 알 수 없기 때문에 재귀 함수를 이용해서 `add` 메소드를 구현해보도록 하겠습니다

```java
private void add(E obj, Node<E> node) {
    if (((Comparable<E>)obj).compareTo(node.data) > 0) {
        // go to the right
        if(node.right == null) {
            node.right = new Node<E>(obj);
            return;
        }
        return add(obj, node.right);
    } 
    if (node.left == null) {
        node.left = new Node<E>(obj);
        return;
    }
    return add(obj.node.left);
}
```

고려해볼 사항은 만약 새롭게 추가할 데이터가 이미 존재하는 노드의 데이터와 동일한 값일 경우입니다.

위의 코드에 기반하면, 예를 들어 10을 추가한다고 했을 때 10은 노드의 데이터에 들어있는 10보다 크지 않으므로 첫 번째 조건문을 만족하지 못합니다.

그래서 왼쪽으로 데이터가 내려가고, 6과 7보다 크기 때문에, 7의 오른쪽 자식 노드에 데이터가 추가됩니다.

아주 간단하게 동일한 값을 왼쪽이 아닌 오른쪽에 위치하게 할 수 있는데, 첫 번째 조건문에 등호를 추가하면 됩니다.(`((Comparable)obj).compareTo(node.data) > 0` -> `((Comparable)obj).compareTo(node.data) >= 0`) 다만, 메소드를 작성할 때 <u>일관성 있게 작성할 수 있도록 주의</u>해야 합니다.

<br>

이렇게 작성한 메소드를 다른 누군가 편하게 사용할 수 있도록 해야 하는데, 동시에 함부로 내부 코드를 수정하게 해서는 안됩니다.

그래서 위의 메서드는 비공개로 처리하고, 새로운 공개 메소드에서 위의 메소드를 활용할 수 있도록 오버로딩을 해보겠습니다.

```java
public void add(E obj) {
    if (root == null) {
        root = new Node<E>(obj);
    } else {
        add(obj.root);
    }
    currentSize++;
}
```

`root`는 전역 변수이고, 트리는 이 루트 노드를 통해서만 접근이 가능합니다.

위와 같이 `add` 메소드를 오버로딩하면, 사용자는 새로운 데이터를 추가할 수 있으면서도 `add` 메소드의 실질적인 내부 구현 코드로는 접근이 불가능하게 됩니다.



<br>

<br>

> ### 참고

[자바로 구현하고 배우는 자료구조 > 4.힙(Heap) & 트리(Tree) : 부스트코스](https://www.boostcourse.org/cs204/joinLectures/212812?isDesc=false)

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
