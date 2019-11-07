---
layout: post
title: Python을 이용한 연결리스트(LinkedList) 구현하기
categories: Algorithm
comments: true
---


정말 오랜만에 포스팅하게 되네요. 이번이 마지막 학기라 신경 쓸게 너무 많아서..
그래도 자료구조나 알고리즘이나 리액트 장고 등등 틈틈히 공부하고 있습니다.

이번애는 __Python__ 을 이용한 링크드 리스트를 한번 구현해보도록 하겠습니다.

링크드 리스트는 값과 다음 노드에 대한 포인터(참조)가 포함된 노드로 이루어진 선형 리스트인데요.

![LInkedlist](https://user-images.githubusercontent.com/40786401/68408044-f217ff00-01c7-11ea-9226-5eab6950fea9.png)

보시는 바와 같이 Head는 링크드 리스트의 앞 부분이고 맨 마지막 부분은 Tail으로 구성되어 있습니다.
그리고 마지막 부분에 다음 부분은 NULL(Python은 None)을 갖습니다.

링크드 리스트의 __장점__ 으로는
+ 여러 데이터 공간을 미리 할당하지 않아도 된다.
+ 삽입이 간단하다. 항목 생성 후 포인터 값만 변경해주면 된다.

그리고 __단점__ 으로는
+ 항목 접근이 오래 걸린다.
+ 물리적으로 인접한 메모리에 위치해있지 않다.
+ 연결을 위한 별도의 데이터 공간이 필요하고 저장공간 효율이 높지 않다.
+ 연결 정보를 찾는 시간이 필요하므로 접근 속도가 느리다.
+ 중간 데이터 삭제시, 앞 뒤 데이터의 연결을 재구성해야하는 부가적인 작업이 필요하다.

어째 장점보다 단점이 더 많은 것 같네요..

## 동작
+ Add: 연결리스트 Tail 다음에 새로운 노드를 추가시키는 함수
+ Delete: 노드를 제거하는 함수


그러면 간단하게 한번 구현해보도록 하겠습니다.

일단 연결 리스트 클래스를 만들기 전에 노드 클래스부터 정의합니다.
노드를 생성할때 data값을 매개변수로 받고 next값을 지정하지 않는다면 None으로 객체를 호출합니다.
~~~python
class Node:

    def __init__(self, data, next=None):
        self.data = data
        self.next = next
~~~

그리고 링크드 리스트를 클래스를 정의합니다.
간단하게 tail 부분 없이 head로만 구성된 링크드 리스트를 구현하겠습니다.
~~~python
class LinkedList:

    def __init__(self, data):
        self.head = Node(data)
~~~
### Add
~~~python
def add(self, data):
     new_node = Node(data)

     if self.head == '':
         self.head = new_node  # 방어 코드
     else:
         cur = self.head
         while cur.next:
             cur = cur.next
         cur.next = new_node

~~~
새로운 노드를 생성하고(new_node) 만약 head에 아직 노드가 없다면 새로운 노드를 head로 지정합니다.
그렇지 않다면 링크드 리스트를 순회하는 __cur__ (Current의 약자)를 선언하고 cur.next가 None일 때까지 순회하면서
링크드 리스트 마지막에 새로운 노드를 삽입합니다.

### Delete
~~~python
def delete(self, data):

     # 1
     if self.head == None:
         return 'No data here'

     cur = self.head # 처음부터 순회하기 위해 cur 변수 선언

     # head 데이터인 경우 2
     if cur.data == data:
         self.head = self.head.next
         del cur

     else:
         # 3,4
         cur = self.head
         while cur.next:
             if cur.next.data == data:
                 temp = cur.next
                 cur.next = cur.next.next
                 del temp
             else:
                 cur = cur.next
~~~

Delete는 다소 복잡할 수 있습니다. 천천히 하나씩 설명하겠습니다.
몇가지 생각해야할 경우가 있습니다.

1. 노드가 존재하지 않을 경우
2. 지울려고 하는 노드가 헤드인 경우
3. 지울려고 하는 노드가 헤드와 마지막 사이인 경우
4. 지울려고 하는 노드가 마지막인 경우

헤드가 데이터인 경우에는 이제 head의 데이터를 지울거니까 self.head를 다음 노드로 선언해놓고(self.head.next) 현재 cur을 삭제합니다.
그리고 데이터를 순회하면서 계속 다음으로 넘어가면서 찾고, 만약 다음 노드가 데이터와 일치한다면 다음 노드를 temp 변수에 담아놓고 현재 노드의 다음을 cur.next(다음)이 아닌 그 다음으로 지정합니다. 그리고 temp를 삭제합니다.
Delete를 저는 이해하기 정말 오래 걸렸는데 사진도 참고하시면서 보면 도움이 될 것 같습니다.

![Linkedlist_deletion](https://user-images.githubusercontent.com/40786401/68411399-c26bf580-01cd-11ea-96a1-fdab2d52fa76.png)


다음에는 양방향 링크드 리스트를 포스팅하겠습니다.
