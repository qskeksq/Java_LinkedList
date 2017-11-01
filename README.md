# LinkedList

## 1. 자료구조 정의
- 자료구조
데이터를 효율적으로 사용하도록 구조적으로 저장하고 관리하는 것. 즉, 자료구조를 사용하여 데이터 추가, 삭제, 검색, 업뎃에 자료를 보다 효율적으로 관리한다.

- 알고리즘
표현되거나 저장된 데이터를 대상으로 문제를 해결하는 방법

- 컬렉션
자료 관리 기술. java.util. 패키지에 자료구조에 사용하는 1.인터페이스 2. 인터페이스 구현 클래스 3. 관련 알고리즘을 정의해 놓은 것, 이것을 컬렉션 프레임워크라고 한다.

## 2. 자료구조 사용 목적
- 상황에 맞는 적절한 자료구조를 이용하게 되면 데이터 처리의 효율을 높일 수 있다
- 예) "So long have I longed for the redemption of my soul"

추상화(인터페이스)
- 다양한 언어로 구현되기 때문에 실제 코드는 다르지만 추상적인 개념에 대해서만 알고 있으면 언어에 종속되지 않고 자료구조를 확장할 수 있다.


## 3. 자료구조 종류
(1) 선형 구조
- 배열
- 링크드 리스트
- 스택 
- 큐
- 데크

(2) 비선형 구조
- 트리
- 그래프


## (1) 링크드 리스트(선형 배열)

### 1. 정의
각 자료에 대해 인덱스를 부여하지 않고 데이터가 서로 이전 데이터, 다음 데이터에 대한 참조만 가지고 있으므로 데이터 추가, 삭제시 연관된 모든 인덱스를 변경할 필요 없이 이전 데이터, 다음 데이터에 대한 참조만 변경하면 된다.
현재 데이터와 다음 데이터에 대한 참조를 가진 Node(노드) 객체의 연결로 이루어진다.

### 2. 종류
- 단순 연결 리스트(Simply Linked List)
- 이중 연결 리스트(Doubly Linked List)
- 이중 단말 연결 리스트(Double Ended Linked List)


### 3. 배열과 링크드 리스트 비교
- 배열의 장점
링크드리스트 < 배열 < 이진트리

- 배열의 단점
생성할 때 지정한 크기를 바꿀 수 없다. 초기에 크기를 예측할 수 있는 경우 배열이 가장 효율적이지만 예상했던 데이터보다 크거나 작은 경우가 빈번한 경우 계속해서 생성해 줘야 한다. 

데이터를 중간에 삽입, 삭제하는 경우 데이터를 모두 복사한 후 밀어주거나 당겨줘야 한다. 따라서 삽입된 데이터나 앞 뒤만 처리하는 것이 아니라 삽입, 삭제에 따라 이동이 추가적으로 발생한다.

cf) ArrayList의 동작 원리 
- DEFAULT_CAPACITY, ensureCapacity -> grow -> System.arrayCopy

- 링크드 리스트의 장점
데이터의 삽입과 삭제에 효율적이다

데이터가 삽입될 때마다 생성되는 노드는 독립적인 공간의 메모리이기 때문에 물리적인 메모리의 범위 내에서는 제한이 없다.

메모리 재사용 가능하다. 자료를 삭제할 경우 노도의 참조가 사라지기 때문에 G.C에 의해 가용 메모리로 전환

- 링크드 리스트 단점
배열에 비해 검색이 느리다. 특정 데이터를 검색할 경우에도 처음부터 순차적으로 검색해야 한다.


### 4. 구현

#### a. 필요한 멤버
- node : 데이터와 다음 노드를 참조하는 객체. 링크드 리스트는 노드의 연결 관계로 이루어진다.

```java
public class LinkedList<T> {

public Node head;	// 처음 값
public Node tail;	// 마지막 값
public int size;	// 전체 사이즈

// 노드
public class Node {
    private Node next;
    private T data;
    
    public Node(T data) {
        this.data = data;
    }
}
```

#### b. 처음에 값 추가

```java
// 처음에 값 추가
public void addFirst(T data) {
	// 처음에 추가할 노드 생성
	Node newNode = new Node(data);
	// 새로 생성한 노드의 다음 노드가를 현재 head로 지정
	newNode.next = head;
	// 헤드에 현재 노드를 설정
	head = newNode;
	// size
	size++;
	
	// 만약 linkedList가 처음 초기화 되어 tail이 없는 경우
	if(tail == null) {
		tail = head;
	}
}
```

#### c. 마지막에 값 추가

```java
// add로 할 수도 있지만 tail값을 이용하지 않으면 처음부터 끝 값까지 찾아와야 한다.
public void addLast(T data) {
	if(size == 0) {
		addFirst(data);
	} else {
		// 마지막에 추가할 노드 생성
		Node newNode = new Node(data);
		// 현재 마지막 값이 새로 생성한 마지막 값을 가리킨다
		tail.next = newNode;
		// tail을 새로 생성한 노드로 갱신해준다
		tail = newNode;
		// size
		size++;
	}
}
```

#### d. 중간 값 추가

```java
// 원하는 위치에 값 추가
// 참고로 인덱스는 0부터 시작
public void insert(int index, T data) {
	
	if(index < 0 || index > size) {
		return;
	} else if(index == 0) {
		addFirst(data);
		return;
	} else if(index == size) {
		addLast(data);
		return;
	} 
	// index(원하는 위치) 전 node 찾아오기
	Node prevNode = getNode(index-1);
	// 현재 index에 있는 node
	Node curNode = prevNode.next;
	// 중간에 추가할 노드 생성
	Node newNode = new Node(data);
	// 이전 노드의 next값 변경
	prevNode.next = newNode;
	// 현재 노드의 다음 노드를 curNode로 설정
	newNode.next = curNode;
	// size
	size++;

}
```

#### e. 처음 값 삭제

```java
public void removeFirst() {

	if(size == 0 || head == null) {
		return;
	}
	// 현재 head에 head의 다음 값을 복사해서 전달
	// head = head.next 해주면 첫 값의 참조를 끊어낼 수 없다.
	Node temp = head;
	head = temp.next;
	// 첫 값을 null 처리 해줘서 참조 관계를 끊어준다.
	temp = null;
	size--;
}
```

#### f. 삭제

```java
// 마지막 node 삭제
public void remove(int index) {
	
	if(index == 0) {
		removeFirst();
		return;
	} 
	// 이전 노드 가져오기
	Node prevNode = getNode(index-1);
	// 삭제하려는 노드
	Node curNode = prevNode.next;
	// 삭제하는 노드의 다음 노드
	Node nextNode = curNode.next;
	
	// 이전 노드가 index 다음 노드를 가리킨다
	prevNode.next = nextNode;
	// 현재 노드 참조 끊어주기
	curNode = null;
	// size
	size--;
}
```

### g. 값 가져오기

```java
// 값 리턴
public T get(int index) {
	Node node = getNode(index);
	return node.data;
}
```

### 5. 결론
- 저장할 데이터의 최대 개수가 정해져 있고 2.리스트 중간에 데이터 삽입, 삭제가 많지 않고 3.인덱스를 이용한 빠른 검색이 필요할 경우는 배열을 사용

- 저장할 데이터의 개수가 정해져 있지 않고 2.리스트의 중간에 데이터를 삽입하거나 삭제하는 작업이 많고 3.특정 위치의 데이터를 검색하는 작업이 많지 않은 경우 연결 리스트 사용