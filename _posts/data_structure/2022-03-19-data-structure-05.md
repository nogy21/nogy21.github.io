---
title: "Hash(2)"

date: 2022-03-19
last_modified_at: 2022-03-19


categories:
  - DataStructure

tags:
  - ["부스트코스", "자바로 구현하고 배우는 자료구조"]

---

<br>

- [Hash](#hash)
  - [Rehashing](#rehashing)
  - [Hash Class](#hash-class)
  - [Constructor](#constructor)
  - [Hash Methods](#hash-methods)
  - [Hash Iterator](#hash-iterator)
- [정리](#정리)

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131687801-2b295fb7-6e22-4e70-a1ef-a7dc85b96796.png" alt="sun cloud" height="10%" width="10%" /></p>

---

> # Hash

지난 글에서는 해시에 대한 소개와 해시 함수, 해시 충돌, 체인 해시에 대해 알아봤습니다.

이번에는 해시의 크기를 조정하는 재해싱과 해시 클래스, 내부 클래스, 생성자, 다양한 메서드를 살펴보겠습니다.

<br>

> ## Rehashing

해시를 사용할 때, 적재율이 높으면(_해시가 많이 찬다면_) 데이터를 검색하는 과정에서 시간이 소요되기 때문에 크기를 조정하는 작업이 필요합니다.

<img title="" src="https://user-images.githubusercontent.com/70495425/159109929-d77dc4ae-a7e8-4cf8-988a-6d617fbfc770.png" alt="image" width="248">

(_체인 해시. 그림과 같이 6개의 버킷에 9개의 요소가 들어가면, 적재율이 1.5로 매우 높아 조정이 필요합니다._)

일반적인 배열은 배열이 꽉 찬다면 기존 크기보다 큰 새로운 배열을 생성한 뒤 기존 배열의 데이터를 옮기게 됩니다.

그런데 체인 해시는 연결 리스트의 위치를 그대로 옮기면 문제가 발생합니다.

테이블 크기가 변했기 때문에 정보를 다시 찾거나 제거하려 할 때 문제가 발생하는 것이지요.

<img src="https://user-images.githubusercontent.com/70495425/159110398-c01f2878-8208-4e3e-889a-684b1507828c.png" title="" alt="image" width="316">

<br>

이를 해결하기 위해 아래와 같이 각 요소의 위치를 초기화한 후, <u>처음부터 다시 위치를 지정</u>합니다.

1. 크기가 2배인 배열 생성

2. data의 index를 다시 결정하여 연결 리스트의 요소들을 옮김

```java
// data의 index 결정
int idx = x.hashCode(s);
idx = idx & ox7FFFFFFF;
idx = idx % tableSize;
```

<img title="" src="https://user-images.githubusercontent.com/70495425/159110515-0ca1bce8-5b0f-4307-ac61-9ad6b8821fdb.png" alt="image" width="328">

<br>

<br>

> ## Hash Class

   체인 해시는 해시 요소마다 키와 값이 들어있습니다. 키와 값을 저장하기 위한 내부 클래스는 다음과 같습니다.

```java
// 해시 클래스
public class Hash<K, V> implements HashI<K, V> {
    // HashElement 클래스는 해시 요소의 비교를 위해 Comparable 인터페이스를 구현
    class HashElement <K, V> implements Comparable <HashElement<K, V>>{
        // 키와 값 정의
        K key;
        V value;
        public HashElement (K key, V value) {
            this.key = key;
            this.value = value;
        }

        // 원하는 대로 (k to k) or (v to v) or (k,v to k,v) 비교 정의
        public int compareTo (HashElement<K, V> o) {
            return (((Comparable<K>)this.key).compareTo(o.key));
        }
    }

    // 전역 변수, 생성자 호출 전에는 만들어지지 않는다
    int numElements, tableSize;
    double maxLoadfactor;
    LinkedList<HashElement<K, V>> [] harray;
}
```

위의 `HashElement`는 `Hash` 클래스 안에서 선언된 `Inner Class`이기 때문에 다른 곳에서 사용될 수 없고, 또한 `equals()`, `toString()`, `hashCode()`가 필요하지 않다고 판단할 수 있습니다.

이 내부 클래스는 데이터를 담당해서 데이터를 추가할 때 사용됩니다.

<br>

<br>

> ## Constructor

지금까지 해시의 키와 값을 저장해줄 내부 클래스 HashElement를 살펴보았습니다. 이번에는 해시를 구현하는 생성자를 만들겠습니다.

```java
public class Hash<K, V> implements HashI<K, V> {
    LinkedList<HashElement<K, V>>[] harray;

    // 생성자를 통해 사용자가 테이블의 크기 설정 가능
    public Hash (int tableSize){
        this.tableSize = tableSize;
        // 제너릭으로 배열을 만드는 것은 까다롭기 때문에 배열 객체 생성 후 캐스팅
        harray = (LinkedList<HashElement<K, V>>[]) new LinkedList[tableSize];

        // 배열 접근 시 연결 리스트가 존재하는지 확인 -> 존재하지 않으면 생성
        // 그래서 미리 초기화를 하면 편리(비어있기에 소모값도 적음)
        for (int i=0; i<tableSize; i++){
            harray[i] = new LinkedList<HashElement<K, V>>();
        }

        maxLoadFactor = 0.75;
        numElements=0;
    }
}
```

<br>

<br>

---

> ## Hash Methods

해시 함수에 사용되는 내부 기등들을 살펴보겠습니다.

<br>

> ### add method

해시에 요소를 추가하는 add 메소드입니다. (_배열의 크기가 작다면 재조정하도록 합니다_)

```java
public boolean add(K key, V value){
    // 재조정하는 위치는 add 전,후 모두 가능
    if (loadFactor() > maxLoadFactor)
        resize(tableSize*2);

    // 키, 값 저장할 객체 생성
    HashElement<K,V> he = new HashElement(key, value);

    // 추가할 위치(인덱스) 탐색
    int hashval = key.hashCode();
    hashval = hashval & 0x7FFFFFFF;
    hashval = hashval % tableSize;

    // 연결 리스트에 추가(addFirst, addLast 관계 X)
    harray[hashval].add(he);

    // 요소 개수 갱신(증가)
    numElements++;
    return true;
}
```

<br>

> ### remove method

remove 메소드에서는 크기 조정이나 객체 생성이 필요치 않습니다.   

```java
public boolean remove(K key, V value){
    // index 찾기
    int hashval = key.hashCode();
    hashval = hashval & 0x7FFFFFFF;
    hashval = hashval % tableSize;

    // 해당하는 index의 키와 값 제거
    harray[hashval].remove(he);

    numElements--;
    return true;
}
```

<br>

> ### getValue method

키를 통해 값을 찾는 `getValue` 메소드를 확인해보겠습니다.

키의 index를 구한 뒤, 해시에서 그 index를 찾을 때까지 반복합니다. 그리고 key의 값이 동일하면 value를 반환합니다.

```java
public V getValue(K key){
    // index 찾기
    /* int hashval = key.hashCode();
    hashval = hashval & 0x7FFFFFFF;
    hashval = hashval % tableSize; */
    int hashval = key.hashCode() & 0x7FFFFFFF % tableSize;

    // index를 찾을 때까지 반복
    for (HashElement<K, V> he : harray[hashval]) {
        // Comparable 사용하여 key 비교
        if (((Comparable<K>)key).compareTo(he.key) == 0){
            return he.val;
                }
        }
    return null;
}
```

<br>

> ### resize method

연결 리스트가 너무 길어질 경우(_적재율이 높은 경우_) <u>해시의 크기를 조절하는</u> resize 메소드입니다. 

새로운 연결 리스트 배열을 만들고 해시의 모든 연결 리스트에 있는 요소의 키와 값을 각각 찾아내야 합니다.

모든 데이터를 복사하고 복사본을 만들기 때문에 복잡도가 높습니다. 그래서 생성자에서 사용자가 직접 `tableSize`를 설정할 수 있도록 합니다.

```java
public void resize(int newSize){
    // newSize 크기의 새로운 연결 리스트 생성
    LinkedList<HashElement<K, V>>[] new_array = 
        (LinkedList<HashElement<K, V>>[]) LinkedList[newSize];

    // 초기화
    for (int i=0; i<newSize; i++)
        new_array[i] = new LinkedList<HashElement<K, V>>();

    // index에 맞게 값 채워넣기
    for (k key : this) {
        V val = getValue(key);
        HashElement<K,V> he = new HashElement<K, V>(key, val);
        int hashVal = (key.hashCode() & 0x7FFFFFFF) % newSize;
        new_array[hashVal].add(he);
    }

    // 데이터 갱신
    // new_array는 resize 내에서만 존재하므로 hash_array에 덮어쓰기
    hash_array=new_array;
    tableSize=newSize;
}
```

<br>

> ### Hash Iterator

대부분의 언어에서 해시를 사용하면 상수 시간 안에 데이터의 접근/추가/삭제가 가능하지만, 키의 순서는 보장되지 않습니다.

그래서 이번에는 모든 키를 어떤 순서로든지 반환하도록 하는 반복자를 살펴보겠습니다.

모든 키에 대해 반복하여 해시의 전체 내용을 살펴보기 때문에 시간 복잡도는  O(n) 입니다.   

```java
// 키에 연결된 연결 리스트의 내용을 살펴보는 함수
class IteratorHelper<T> implements Iterator<T>{
    T[] keys; // 키로 이루어진 배열
    int position; // 키를 탐색할 때 현재 위치를 나타냄

    // 생성자는 해시를 탐색하며 연결 리스트에 접근해 키를 찾아 배열에 입력
    public IteratorHelper(){
        keys = (T[]) Object[numElements];
        int p = 0;
        for (int i=0; i<tableSize; i++) {
            LinkedList<HashElement<K, V>> list = hash.array[i];
            for (HashElement<K, V> h : list)
                keys[p++] = (T) h.key();
        }
        position =  0;
    }

    // 반환할 객체가 남았는지 확인
    public boolean hasNext()
        return position < keys.length;

    // 반환할 객체가 있다면 반환한 뒤 카운터 증가
    public T next(){
        if (!hasNext())
            return null;
        return keys[position++];
    }
}
```

<br>

<br>

---

> ## 정리

해시에 대해 알아봤습니다.

평균적인 데이터 처리의 시간복잡도가 `O(1)` 로 앞서 배웠던 연결 리스트에 비해 빠른 처리 속도가 큰 장점입니다.

하지만 최악의 경우에는 `O(n)`의 시간복잡도가 소요되고, 해시 함수에 의존적이며 순서가 보장되지 않는다는 단점이 있습니다.

<br>

<br>

<br>

> ### 참고

[자바로 구현하고 배우는 자료구조 > 3.해시(Hash) : 부스트코스](https://www.boostcourse.org/cs204/joinLectures/212446?isDesc=false)

---

<p align="center"><img src="https://user-images.githubusercontent.com/70495425/131689647-b4d2206e-7ec4-4f7f-a734-6c3bf77c80c3.png" height="10%" width="10%"></p>

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}
