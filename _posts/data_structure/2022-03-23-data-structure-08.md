---
title: "트리 응용(AVL & RB)(1)"

date: 2022-03-23
last_modified_at: 2022-03-23


categories:
  - DataStructure

tags:
  - ["부스트코스", "자바로 구현하고 배우는 자료구조"]

---

<br>

- [AVL Tree](#avl-tree)
  - [Node](#node)
  - [Add](#add)
  - [CheckBalance](#checkbalance)
  - [Rebalance](#rebalance)
- [정리](#정리)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> # AVL Tree

스스로 균형을 잡는 이진 트리 자료 구조인 AVL트리에 대해 살펴보도록 하겠습니다.

<img src="https://user-images.githubusercontent.com/70495425/159698396-4aab8fdc-16c3-4320-ba16-b6c35698080e.png" title="" alt="image" width="587">

AVL 트리에서는 <u>왼쪽과 오른쪽의 높이 차이가 항상 1보다 작거나 같아야</u> 합니다.

규칙을 위반한 경우 회전을 통해 문제를 해결할 수 있습니다. 주의해야 할 점은 회전을 한 후에 새로운 위반사항이 생겼는지 파악해야한다는 것 입니다.

이제 어떻게 코드로 구현하면 좋을지 살펴보도록 하겠습니다.

<br>

> ## Node

우선 AVL 트리에서 사용되는 노드 클래스부터 확인하겠습니다.

`left`, `right` 노드뿐만 아니라, 기능을 간단하게 구현하기 위한 부모 노드에 대한 포인터도 있습니다.

```java
class Node<T> {
    T data;
    Node<T> left;
    Node<T> right;
    Node<T> parent;

    public Node(T obj) {
        data = obj;
        parent = left = right = null;
    }
}
```

<br>

> ## Add

트리에 요소를 추가하는 add 메소드에 대한 코드입니다. 

클래스를 생성 후, 트리가 비어있으면 노드를 추가하고 비어있지 않다면 add 메소드를 재귀로 호출합니다.

```java
// 생성자
public AVLTree() {
    root = null;
    currentSize = 0;
}
// add 메소드
public void add(E obj) {
    Node<E> node = new Node<E>(obj);
    // 트리가 비어있을 경우
    if (root == null) {
        root = node;
        currentSize++;
        return;
    }
    /* 트리에 노드가 있을 경우, 왼쪽이나 오른쪽으로 계속해서 내려가서 
       노드를 추가할 알맞은 위치를 찾도록 메소드를 재귀로 호출 */
    add(root, node);
}
```

재귀로 호출되는 `add` 메서드는 다음과 같습니다.

```java
public void add(Node<E> parent, Node<E> newNode) {
    if (((Comparable<E>)newNode.data.compareTo(parent.node.data) > 0) {
        if (parent.right == null) {
            parent.right = newNode;
            newNode.parent = parent;
            currentSize++;
        } else {
            add(parent.right, newNode);
        }
    } else {
        if (parent.left == null) {
            parent.left = newNode;
            newNode.parent = parent;
            currentSize++;
        } else {
            add(parent.left, newNode);
        }
    }
    // 규칙 준수 여부 확인 
    checkBalance(newNode);
}
```

<br>

> ## checkBalance

`checkBalance` 메소드를 통해 높이의 차이가 1보다 큰지 확인하도록 하겠습니다.

```java
public void checkBalance(Node<E> node) {
    if ((height(node.left) - height(node.right) > 1) ||
        (height(node.left) - height(node.right) < -1)
    ) {
        rebalance(node);
    }
    if (node.parent == null) {
        return;
    }
    // 루트에 도달할 때까지 확
    checkBalance(node.parent);
}
```

<br>

> ## Rebalance

`checkBalance()`를 통해 불균형이 일어난다는 사실을 알아냈고, 불균형이 일어난 위치는 `rebalance` 메소드를 통해 알아내도록 하겠습니다.

어느 쪽에서 균형이 깨졌는지 확인하고 회전을 하여 균형을 유지합니다.

```java
public void rebalance(Node<E> node) {
    // 왼쪽 서브 트리가 더 길면
    if (height(node.left) - height(node.right) > 1) {
        // 왼쪽 서브 트리 > 오른쪽 서브 트리
        if(height(node.left.left) > height(node.left.right)) {
            node = rightRotate(node); // 우측 회전
        } else { // 왼쪽 서브 트리 < 오른쪽 서브 트리
            node = leftRightRotate(node); // 좌측-우측 회전
        }
    } else { // 왼쪽 자식 < 오른쪽 자식 
        // 왼쪽 서브 트리 > 오른쪽 서브 트리
        if(height(node.left.left) > height(node.left.right))  {
            node = rightLeftRotate(node); // 우측-좌측 회전
        } else { // 왼쪽 서브 트리 < 오른쪽 서브 트리
            node = leftRotate(node); // 좌측 회전  
        }
    }
    // 루트로 올 때까지 반복합니다.
    if (node.parent == null) {
        root = node;
    }
}
```

<br>

> ## Adding Data

마지막으로 하나씩 데이터를 추가해서 AVL 트리를 만드는 과정을 살펴보겠습니다.
AVL 트리에 숫자를 바로 추가하기에 앞서 서브 트리를 그려야 합니다. 서브 트리를 그려서 규칙을 준수하기 위해 어떤 회전이 필요한지 생각해야 합니다.

우선 첫 번째 숫자는 43이고, 차례대로 18, 22를 추가해보겠습니다.

![image](https://user-images.githubusercontent.com/70495425/159751555-29d30780-ebea-41ff-9e45-5d1213c7d791.png)

왼쪽과 오른쪽 높이의 차가 1을 초과하는 시점에서 균형이 깨지므로, 회전이 필요합니다. 위의 상황에서는 왼쪽 자식의 오른쪽 서브트리에서 균형이 깨졌습니다. 그러므로 좌측-우측 회전을 진행했습니다.

다시 숫자를 추가해보겠습니다. 9, 21, 6을 추가하면 다시 균형이 깨집니다.

![image](https://user-images.githubusercontent.com/70495425/159850339-16253e3c-12e7-4f81-bfe1-e34359a5e00e.png)

왼쪽 자식의 왼쪽 서브트리에서 불균형이 생기므로 우측 회전이 필요합니다.

<br>

이렇게 해서 43에 18, 22, 9, 21, 6, 8, 20, 63, 50, 62, 51을 순서대로 추가한 결과는 다음과 같습니다.

![image](https://user-images.githubusercontent.com/70495425/159746572-35dd458c-55ea-4650-ae0c-cc29d1cb7995.png)

<br>

앞에서 만든 `add` 메소드를 활용하여 AVL 트리를 만드는 과정을 살펴봤습니다.

<br>

<br>

> ## 정리

이렇게 왼쪽과 오른쪽의 높이 차이를 이용해서 균형을 이루는 이진 트리인 AVL 트리에 대해 알아봤습니다.

다음 강의에서는 비슷한 균형 이진 탐색 트리인 레드 블랙 트리에 대해 알아보도록 하겠습니다.

<br>

<br>

> ## 참고

[자바로 구현하고 배우는 자료구조 > 5.트리 응용(AVL & RB) : 부스트코스](https://www.boostcourse.org/cs204/joinLectures/212813?isDesc=false)

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
