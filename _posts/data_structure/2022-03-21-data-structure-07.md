---
title: "Heap & Tree(2)"

date: 2022-03-21
last_modified_at: 2022-03-22


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
  - [Contains](#contains)
  - [Remove](#remove)
  - [Rotations](#rotations)

- [정리](#정리)

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

> ## Contains

특정 요소가 트리에 포함되어있는지 확인하는 함수인 `contains(E obj)` 메소드를 알아보겠습니다.

`add`메소드와 마찬가지로 공개 메서드를 통해 사용자가 접근이 가능하도록 하고, 실내 내부 구현 코드는 비공개 메소드로 작성합니다.

```java
public boolean contains(E obj) {
    // 특정 요소의 존재 여부를 판단하기 위해서는 루트를 통해 트리에 접근해야 함
    return contains(obj, root);
}
```

동작 원리 역시 `add` 메소드와 유사합니다.

1. 루트에서 시작 
2. 트리 규칙 준수하며 탐색
3. 그 <u>요소를 찾으면</u> `true` 반환, <u>null인 노드에 다다르면</u> `false` 반환

코드로 구현하면 다음과 같습니다.

```java
private boolean contains(E obj, Node<E> node) {
    if (node == null) {
        return false;
    }
    if (((Comparable<E>)obj).compareTo(node.data) == 0) {
        return true;
    }
    if (((Comparable<E>)obj).compareTo(node.data) > 0) {
        return contains(obj, node.right);
    }
    return contains(obj, node.left);
}
```

<br>

<br>

> ## Remove

특정 요소를 제거하는 `remove` 메소드입니다.

<img src="https://user-images.githubusercontent.com/70495425/159392623-c445f0ec-5ee2-4556-bc00-ccf9a190e9ae.png" title="" alt="image" width="289">

트리에서 특정 요소를 제거할 때에는 자식 노드의 개수를 고려해야 합니다.

1. *잎 노드 제거*
   말단에 위치하기 때문에 해당 노드를 가리키는 부모 노드의 포인터를 `null`로 설정하면 됩니다.

2. *자식 노드가 하나인 노드 제거*
   해당 노드의 부모 노드의 포인터를 해당 노드의 자식 노드로 향하게 하면 됩니다.(*부모 노드에서 사용한 포이터와 동일한 포인터 `left` of `right` 사용*)
   
   <img src="https://user-images.githubusercontent.com/70495425/159392670-fa87a084-1255-4ae3-af8d-f9e50f2c40db.png" title="" alt="image" width="189">

3. *자식 노드가 2개인 노드 제거*
   
   <img src="https://user-images.githubusercontent.com/70495425/159393002-7475b71e-7b1c-4301-92ea-04bced2071aa.png" title="" alt="image" width="290">
   자식 노드가 2개인 10을 제거해본다고 가정하겠습니다. 
   앞의 두 상황과는 달리 고려할 사항이 있어 보입니다.
   왼쪽 자식 노드인 6을 루트로 올리면 오른쪽 자식 노드의 처리가 애매해지고, 이런 방식으로 해결한다면 트리를 새로 만드는 것과 크게 다를 바 없어 보입니다.
   그런데 이전에 배운 내용 중에 이 상황을 해결할 실마리가 있습니다. 
   바로 <u>잎 노드를 제거</u>하는 것 입니다.
   
   위의 그림을 보시면, 루트 노드의 자리에 잎 노드 중 어떤 것이 위치하면 트리의 전체 구조를 변경하지 않아도 괜찮을지 보이시나요? 
   8과 12를 사용하면 됩니다.(_8과 12를 각각 중위 후속자와 중위 선임자라고 부릅니다_)
   
   즉, 자식 노드가 2개인 노드는 <u>중위 후속자 또는 중위 선임자와 자리를 바꾼 뒤 제거</u>하면 됩니다.

<br>

- 중위 후속자(*in order successor*)
  : 제거하고자 하는 노드에서 시작하여 왼쪽으로 한 번 내려갔다가 계속 오른쪽으로 내려간 곳의 잎 노드. <u>제거하고자 하는 노드보다 작은 노드들 중에서 가장 큰 노드</u>.   
  (*중위 순회 방식으로 노드를 탐색할 때 루트 노드를 방문하기 직전에 만나게 되는 노드이기 때문에 중위 후속자라고 부름*)

- 중위 선임자(*in order predessor*)
  : 제거하고자 하는 노드에서 시작하여 오른쪽으로 한 번 내려갔다가 계속 왼쪽으로 내려간 곳의 잎 노드. <u>제거하고자 하는 노드보다 큰 노드들 중에서 가장 작은 노드</u>

<br>

<br>

> ## Rotations

<img src="https://user-images.githubusercontent.com/70495425/159395389-dfce3471-83c8-4397-83a1-74bc7ab71e00.png" title="" alt="image" width="287">

위와 같은 트리에서 탐색할 때 시간복잡도는 어떻게 될까요? 그림에 써있다시피 `O(log n)`입니다.

그렇다면 아래의 트리는 어떨까요?

<img src="https://user-images.githubusercontent.com/70495425/159395425-7b7a2c14-871e-4e50-8972-e39fda3a17b4.png" title="" alt="image" width="285">

45도로 기울어진 연결 리스트와 같죠? 이 역시 트리라고 할 수 있습니다. 

다만, 시간복잡도는 `O(n)`이 소요되겠죠. 사실상 엉터리 트리라고 볼 수 있습니다.

<br>

<u>균형 잡힌 트리</u>를 만들기 위해 <u>트리의 노드 위치를 바꾸는 과정을 회전</u>이라고 합니다.

연결 리스트처럼 <u>한 방향으로 나열된 트리는 균형 잡혀 있지 않다</u>고 표현합니다. 균형 잡힌 트리에서는 특정 요소를 탐색하는 시간 복잡도가 `O(log n)`이지만 균형 잡히지 않은 트리에서는 연결 리스트와 같은 `O(n)`이 됩니다. 따라서, <u>데이터를 효율적으로 관리하려면 트리를 균형 있게 만들어야</u> 합니다.

균형을 측정하는 방법에 대해서도 나중에 배우게 될텐데, 우선 불균형한 트리를 가정하고 회전하는 방법을 알아보겠습니다.

<u>조부모 노드, 부모 노드, 자식 노드의 크기 관계에 따라</u> <u>우측 회전, 혹은 좌측 회전</u>을 합니다. 트리를 재정렬하면 <u>항상 중간 크기의 요소가 가장 위에 있는 루트</u>가 됩니다.

1. <u>왼쪽 서브 트리 불균형</u>
   예를 들어, 루트 노드인 10과 자식 노드인 6을 갖는 트리에서 4를 추가한다고 가정하면, 10(조부모 노드) > 6(부모 노드) > 4(자식 노드)의 트리가 생깁니다.
   
   <img title="" src="https://user-images.githubusercontent.com/70495425/159395111-651dad8c-b953-4f4b-8147-b7e6b1179556.png" alt="image" width="530">
   <br>
   이 불균형한 트리를 <u>우측 회전</u>으로 조부모 노드를 부모 노드의 오른쪽 자식 노드 위치로 옮겨주면 균형 잡힌 트리가 됩니다.

   

2. <u>오른쪽 서브 트리 불균형</u>
   
    <img src="https://user-images.githubusercontent.com/70495425/159395126-91507b0c-1f66-4720-80f1-53223319e66b.png" title="" alt="image" width="530">
   
   <br>
   크기 관계는 (조부모 노드) < (부모 노드) < (자식 노드)입니다.
   <u>좌측 회전</u>을 하여 조부모 노드를 부모 노드의 왼쪽 자식 노드 위치로 옮겨줍니다.

<br>

<u>트리가 한 쪽으로 치우치지 않은 경우</u>에는 어떻게 해결할까요? 이럴 경우, <u>우측 회전과 좌측 회전을 모두 사용</u>하여 불균형을 해소합니다.

3. <u>오른쪽 자식의 왼쪽 서브 트리에서 불균형</u>
   
   <img src="https://user-images.githubusercontent.com/70495425/159402381-48a59552-eef8-429a-956c-cd72d6b3e45d.png" title="" alt="image" width="530">
   <br>
   크기 관계는 (부모 노드) > (자식 노드) > (조부모 노드)입니다. 
   자식 노드에 대해 <u>부모 노드를 우측 회전 후 좌측 회전</u>을 하여 조부모 노드를 부모 노드의 왼쪽 자식 노드 위치로 옮겨줍니다.

   

2. 불균형이 왼쪽 자식의 오른쪽 서브 트리에서 나타날 경우
   
   <img src="https://user-images.githubusercontent.com/70495425/159402390-b2ecd43d-d405-4f88-8317-9bfc9bf1557e.png" title="" alt="image" width="530">
   <br>
   크기 관계는 (부모 노드) > (조부모 노드) > (자식 노드)입니다. 
   자식 노드에 대해 <u>부모 노드를 좌측 회전 후 우측 회전</u>을 하여 조부모 노드를 부모 노드의 오른쪽 자식 노드 위치로 옮겨줍니다.

회전을 할 때 중요한 것은 <u>중간값이 루트가 되도록</u> 재정렬하는 것 입니다.

<br>

<br>

이제 회전에 대한 개념을 알았으니, 코드로 구현해보겠습니다.

다음과 같이 임시 포인터를 사용하여 좌측 회전, 우측 회전을 구현합니다.

```java
// 좌측 회전: 조부모 노드를 부모 노드의 왼쪽 자식 노드 위치로 옮깁니다.
public Node<E> leftRotate(Node<E> node) {
    Node<E> tmp = node.right; // set temp = grandparents right child
    node.right = tmp.left; // set grandparents right child = temp left child
    tmp.left = node; // set temp left child = grandparent 
    return tmp; // use temp instead of grandparent
}

// 우측 회전: 조부모 노드를 부모 노드의 오른쪽 자식 노드 위치로 옮깁니다.
public Node<E> leftRotate(Node<E> node) {
    Node<E> tmp = node.left; 
    node.left = tmp.right; 
    tmp.right = node; 
    return tmp;
}
```

   

트리가 한 쪽으로 치우치지 않을 경우, 우측 회전과 좌측 회전을 둘 다 사용해야 합니다. 위에서 구현한 우측 회전, 좌측 회전 함수를 활용하여 아래와 같이 구현합니다.

```java
// 우측 회전 후 좌측 회전
public Node<E> rightLeftRotate(Node<E> node) {
    node.right = rightRotate(node.right);
    return leftRotate(node);
}

// 좌측 회전 후 우측 회전
public Node<E> leftRightRotate(Node<E> node) {
    node.left = leftRotate(node.left);
    return rightRotate(node);
}
```

<br>

<br>

> ## 정리

두 편의 글을 통해 힙과 트리에 대해 알아봤습니다.

이진 트리를 기준으로 학습을 했으며, 완전 트리에 속하는 최대 힙과 최소 힙 그리고 트리의 탐색 방법, 회전 등을 배웠습니다. 

모두 계층구조를 갖는 자료구조로 평균 탐색의 시간복잡도가 `O(log n)`으로 빠르다는 장점이 살펴봤습니다.

다음 학습 내용은 트리의 응용 구조인 `AVL Tree`와 `Red Black Tree`를 배울 예정입니다.

<br>

<br>

> ### 참고

[자바로 구현하고 배우는 자료구조 > 4.힙(Heap) & 트리(Tree) : 부스트코스](https://www.boostcourse.org/cs204/joinLectures/212812?isDesc=false)

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
