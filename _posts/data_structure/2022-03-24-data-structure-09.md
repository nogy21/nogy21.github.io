---
title: "트리 응용(AVL & RB)(2)"

date: 2022-03-24
last_modified_at: 2022-03-25


categories:
  - DataStructure

tags:
  - ["부스트코스", "자바로 구현하고 배우는 자료구조"]

---

<br>

- [Red Black Tree](#red-black-tree)
  - [Class](#class)
  - [Add](#add)
  - [checkColor & correctTree](#checkcolor-&-correcttree)
  - [Rotate](#rotate)
- [정리](#정리)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> # Red Black Tree

이번에 소개해드릴 Tree는 Red Black Tree 입니다.

이전의 AVL Tree와 마찬가지로  균형을 유지하는 트리이며, 모든 노드를 빨간색이나 검은색으로 표시하고 규칙에 따라 색상을 전환 또는 회전하는 아이디어로 Red Black Tree라고 불립니다. 자세한 규칙은 다음과 같습니다. 

<br>

### **규칙**

1. 모든 노드는 빨간색 or 검은색 
2. 루트는 항상 검은색
3. 새로 추가되는 노드는 항상 빨간색
4. 루트에서 잎 노드로 가는 모든 경로에는 같은 수의 검은색 노드 존재
5. 어떤 경로에서도 빨간색 노드 2개가 연속으로 위치 X
6. 모든 빈 노드는 검은색

<br>

### **균형을 맞추는 방법**

1) 이모 노드가 검은색 => 회전
   회전을 하고 나면 부모 노드는 검은색, 두 자식 노드는 빨간색이 되어야 합니다.

2) 이모 노드가 빨간색 => 색상 전환
   색상 전환을 하고 나면 부모 노드는 빨간색, 두 자식 노드는 검은색이 되어야 합니다.

<img src="https://user-images.githubusercontent.com/70495425/159861116-1815a843-a78b-43a9-b3f9-03f50c7fded8.png" title="" alt="image" width="165">

예를 들면, 위와 같은 상황에서 7은 연속된 빨간색 노드를 사용했으므로 규칙을 위반합니다. 그리고 7의 이모 노드인 1은 빨간색이기에 균형을 맞추기 위해 색상 전환이 필요합니다.

<img src="https://user-images.githubusercontent.com/70495425/159862787-639426c0-4708-487b-af7c-df302d33c70c.png" title="" alt="image" width="431">

이번에는 위의 상황에서 6을 새롭게 추가해보겠습니다.

<img src="https://user-images.githubusercontent.com/70495425/159862926-dc7082ab-33e3-4fb3-96e5-b79f5f14c8fb.png" title="" alt="image" width="213">

마찬가지로 7과 6이 빨간색으로 연속되므로 규칙을 위반하고, 6의 이모 노드를 찾아보니 빈 노드가 나옵니다. 빈 노드는 검은색으로 간주하기 때문에, 회전을 하면 균형이 맞춰지게 됩니다.

<img src="https://user-images.githubusercontent.com/70495425/159863374-fe9b0089-9462-408b-af24-dd7d0a14e39c.png" title="" alt="image" width="626">

<br>

이와 같은 방식으로 1, 3, 5, 6, 7, 8, 9, 10까지의 숫자를 RB 규칙에 따라 배열하면 다음과 같이 나타낼 수 있습니다.

<img src="https://user-images.githubusercontent.com/70495425/159860870-b90d9363-83d8-4db1-8368-e2c1185779e1.png" title="" alt="image" width="401">

<br>

<br>

> ## Class

`RedBlackTree` 클래스는 다음과 같습니다. 
불리언 값을 가진 **black**로 참이면 검은색, 거짓이면 빨간색을 표시합니다. 그리고 이모 노드를 알아내기 위해 left, right, parent 노드를 가리키는 포인터뿐만 아니라 불리언 값을 가진 **isLeftChild**를 사용합니다.

```java
public class RedBlackTree<K,V> implements RedBlackI<K,V> {
    Node<K,V> root;
    int size;

    class Node<K,V> {
        K key;
        V value;
        Node<K,V> left, right, parent;
        boolean isLeftChild, black;

        public Node (K key, V value) {
            this.key = key;
            this.value = value;
            left = right = parent = null;
            black = false; // red
            isLeftChild = false; // 아직 모르기에 상관 없음
        }
    }
}
```

<br>

<br>

> ## Add

add 메소드의 동작 방식은 AVL 트리와 동일합니다. 단, isLeftChild가 추가되기 때문에 이를 고려해주어야 합니다.

add 메소드에 대한 코드는 다음과 같습니다. 트리가 비어있으면 노드를 추가하고 비어있지 않다면 재귀 add 메소드를 호출합니다.

```java
public void add(K key, V value){
    Node<K,V> node = new Node<K,V>(key, value);
    // 비었는지 확인(무언가를 데이터 구조에 추가할 때 가장 먼저 할 일)
    if (root == null) {
        root = node;
        root.black = true;
        size++;
        return;
    }
    // 트리에 노드가 있을 경우 어디에 추가해야하는지 확인
    add(root, node);
    size++;
}
// add 재귀함수, 내부클래스
private void add (Node<K,V> parent, Node<K,V> newNode) {
    // newNode의 data가 parent의 data보다 크면 트리의 오른쪽에 추가
    if (((Comparable<K>)newNode.key).compareTo(parent.key) > 0) {
        if (parent.right == null) {
            parent.right = newNode;
            newNode.parent = parent;
            newNode.isLeftChild = false;
            return;
        }
        return add(parent.right, newNode);
    }    
    // newNode의 data가 parent의 data보다 작거나 같으면 트리의 왼쪽에 추가
    if (parent.left == null) {
        parent.left = newNode;
        newNode.parent = parent;
        newNode.isLeftChild = true;
        return;
    }
    return add(parent.left, newNode);
    // 규칙 준수 확인
    checkColor(newNode);
}
```

<br>

<br>

> ## checkColor & correctTree

레드 블랙 트리 규칙을 만족하는지 확인하고(`checkColor()`) 색상 전환(`correctTree()`)을 하는 색상 확인 메소드에 대해 알아보도록 하겠습니다.

```java
public void checkColor(Node<K,V> node) {
    // 루트는 항상 검은색이므로 색 확인 X
    if (node == root) {
        return;
    }
    // 빨간 노드 2개 연속(규칙 위반)
    if (!node.black && !node.parent.black) {
        correctTree(node);
    }
    // 부모 노드 계속 확인
    checkColor(node.parent);
}

public void correctTree(Node<K,V> node) {
    // node의 부모 노드가 왼쪽 자식 -> 이모 노드는 조부모 노드의 오른쪽 자식
    if (node.parent.isLeftChild) {
        // 이모 노드가 검은색 (= 비어있는 경우 포함)
        if(node.parent.parent.right == null || node.parent.parent.right.black) {
            return rotate(node);
        }
        //  이모 노드가 빨간색
        if (node.parent.parent.right != null) {
            node.parent.parent.right.black = true;
        }
        node.parent.parent.black = false;
        node.parent.black = true;
        return;
    } else {
        if(node.parent.parent.left == null || node.parent.parent.left.black) {
            return rotate(node);
        }
        if (node.parent.parent.left != null) {
            node.parent.parent.left.black = true;
        }
        node.parent.parent.black = false;
        node.parent.black = true;
        return;
    }
```

<br>

<br>

> ## Rotate

현재 노드와 부모 노드가 각각 오른쪽 자식인지 왼쪽 자식인지에 따라 필요한 회전이 달라집니다. 각각의 경우에 대해 코딩하면 다음과 같습니다.

```java
public void rotate(Node<K,V> node){ // 문제 발생 노드를 매개변수로
    // 현재 노드가 왼쪽 자식
    if (node.isLeftChild) {
        // 부모 노드가 왼쪽 자식
        if (node.parent.isLeftChild) {
            // 부모&자신 왼쪽 -> 조부모 노드를 우측 회전
            rightRotate(node.parent.parent);
            node.black = false;
            node.parent.black = true;
            // 형제 노드
            if(node.parent.right != null) {
                node.parent.right.black = false;
            }
            return;
        }
        // 부모 노드가 오른쪽 자식 -> 조부모 노드를 우측-좌측 회전
        rightleftRotate(node.parent.parent);
        node.black = true;
        node.right.black = false;
        node.left.black = false;
        return;
    } else {
        if (node.parent.isLeftChild) {
            leftRotate(node.parent.parent);
            node.black = false;
            node.parent.black = true;
            // 형제 노드
            if(node.parent.left != null) {
                node.parent.left.black = false;
            }
            return;
        }
        // 부모 노드가 오른쪽 자식 -> 조부모 노드를 우측-좌측 회전
        leftRightRotate(node.parent.parent);
        node.black = true;
        node.right.black = false;
        node.left.black = false;
        return;
    }
}
```


<br>

<br>

> ## 참고

[자바로 구현하고 배우는 자료구조 > 5.트리 응용(AVL & RB) : 부스트코스](https://www.boostcourse.org/cs204/joinLectures/212813?isDesc=false)

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
