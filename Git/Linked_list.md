# Linked List

Linked List란 일반적인 파이썬 배열 리스트와 달리 각 데이터가 노드로 연결된 형태의 리스트를 뜻한다. 일반적인 CRUD가 어떻게 이뤄지는지 탐구하여 본다.

## 클래스 지정

Linked List에는 두 클래스가 존재한다. 하나는 노드를 이어주는 클래스, 다른 하나는 리스트 그 자체이다. 데이터 입출력은 리스트에서 담당하고 노드는 각 데이터를 연결하는 역할을 맡는다.

클래스 생성을 통해 리스트의 기본 구조를 지정한다.

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```

노드는 위와 같이 구성되어 있다. 생성자 __init__으로 데이터를 연결하는 것과 다음 노드를 찾아갈 수 있게 지정해준다.

이제 리스트를 생성한다.

```python
class LinkedList:
    def __init__(self):
        self.head = None
        self.length = 0
        self.tail = None
```

노드의 머리와 꼬리를 지정하여 데이터가 뒤에서부터 차례로 이어지는 append, 앞에서 끼워넣는 prepend가 가능하도록 한다. length를 통해 리스트의 길이 또한 입력한다.

## append

데이터를 이으려면 어떻게 해야 할까? LinkedList는 각 노드가 다음 데이터, 그 다음 데이터에도 계속 연결되어있는 형태로 만들어진다. 따라서 데이터가 append 된다면 노드도 계속 이어줘야만 한다.

append 기능을 만들어보자.

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
        self.length = 0
        self.tail = None

    def append(self,data):
        node = Node(data)
```

일단 노드를 이어야 할테니 Node에 data를 입력 받은 변수를 node로 지정하고 시작한다.

리스트가 비어있다면 head와 tail에 node를 이어준다.

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
        self.length = 0
        self.tail = None

    def append(self,data):
        node = Node(data)

        if self.head is None:
            self.head = node
            self.tail = node
```

node는 머리끼리 이어진다. head와 length, tail은 LinkedList 인스턴스에서 관리하고 나머지 데이터들은 모두 Node 인스턴스가 관리한다. 데이터를 찾아갈 때는 head에서 계속 next를 탐색하게 되고, tail은 항상 가장 마지막의 데이터와 연결되도록 만들어야 한다.

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
        self.length = 0
        self.tail = None

    def append(self,data):
        node = Node(data)

        if self.head is None:
            self.head = node
            self.tail = node

        else:
            self.tail.next = node
            self.tail = node

        self.length += 1
```

처음 head에 아무것도 없다면, head와 tail 모두 노드를 이어주지만 이미 head 데이터가 있는 경우라면 next를 통해 노드를 이어주고 tail 또한 재지정하도록 만든다.

이제 함수를 불러와 실행해보면 데이터가 잘 이어지는 모습을 볼 수 있을 것이다.

```python
list = LinkedList()

list.append("Hello World")
list.append(55)
list.append(True)
```

## prepend

prepend란 앞에서부터 데이터를 끼워넣는 것이다. 이미 head가 지정된 데이터는 어디로 이동해야 할까? 앞에서 추가되는 데이터에 head 노드를 연결하고 next를 원래 head였던 데이터에게 연결시켜야하지 않을까?

함수를 보면 그렇게 복잡하지는 않다.

```python
def prepend(self, data):
    node = Node(data)
    node.next = self.head
    self.head = node
    self.length += 1
```

우선 데이터는 항상 Node 인스턴스로 관리되니 그쪽에 넣어두고, 새로이 추가된 데이터의 next가 원래 head node였던 데이터에게 연결되도록 한다. 그럼 원래 head였던 데이터는 새로 낑겨 들어온 데이터에 밀려나고 next 노드가 되어버린다.

그리고 앞에서 끼어들어온 데이터의 node를 head로 만들고 길이를 추가하면 앞에서부터 데이터를 끼워넣는 로직이 완성된다.

## pop

pop이란 리스트 맨 뒤에 위치한 데이터를 반환하고 없애버리는 명령어다. 현재 LinkedList에서 맨 끝의 node를 삭제하려면 어떻게 하면 될까?

1. 데이터가 없을 경우

데이터가 없다면 그냥 None 값을 반환하고 끝내버리면 될 것이다.

```python
def pop(self):
    if self.tail is None:
        return None
```

2. 데이터가 있을 경우

데이터의 길이가 어떻든 가장 마지막 데이터까지 찾아가야만 한다. 데이터 찾아가는 로직을 어떻게 만들면 좋을까?

우선 head를 node로 지정하고 조건을 검사해봐야겠다. 데이터가 1개만 들어있는 리스트일 수도 있으니까. 만약 다음 노드가 없다면 head에 None 값을 넣고 리스트 길이를 -1 한 뒤 node의 data를 반환하면 될 것이다. 그럼 리스트 첫 번째 데이터 값이 출력되고 사라질 것이다.

```python
def pop(self):
    if self.tail is None:
        return None

    node = self.head
    if node.next is None:
        self.head = None
        self.length -= 1
        return node.data
```

그럼 데이터가 여러 개라면? 그때는 while 반복문을 돌려야 한다.

현재 위치한 노드를 prev(이전)으로 놓고 다음 노드를 현재 노드로 바꿔주면서 하나씩 데이터를 타고 이동한다. 다음 노드가 없다면 반복문이 종료되고 연산을 수행할 것이다.

```python
while node.next:
    prev = node
    node = node.next
```

반복문이 끝나고 마지막 데이터가 node에 담겼다. 그러면 tail을 이전 node에 연결시키고 현재 데이터를 None 값으로 만든 뒤 길이를 -1 해주고 데이터를 반환하면 되겠다.

```python
while node.next:
    prev = node
    node = node.next

result = node.data
self.tail = prev
prev.next = None
self.length -= 1

return result
```

print 해본다면 맨 뒤의 데이터는 사라져있을 것이다. 사실 node만 끊는 것으로 데이터를 삭제시키는게 어떻게 가능한가 궁금할 수 있다.

파이썬에는 Garbage Collector라는 녀석이 존재한다. 별 연관성 없어진 데이터는 자동으로 치워줘 메모리를 정리하는 역할을 하기에 아무런 참조 node 없이 방치된 데이터는 수거해버리는 것이다. 그래서 데이터를 삭제하는 것과 같은 효과를 낼 수 있는 것이다.

## getter, setter, delete
### 1. getter

데이터를 조회, 수정, 삭제하는 일반적인 CRUD 기능들이다. 데이터를 조회부터 해보자.

파이썬에서 배열의 index를 조회하려면 어떻게 했었나 생각해보자. 요소가 5개 들어있는 리스트에서 3번째 요소를 찾고 싶다면 index 2번째 값을 찾으면 될 것이다.

```python
list = [1, 2, 3, 4, 5]
print(list[2])
```

이것을 찾아가는 로직을 짜보자. 파이썬은 인덱스에 음수를 넣으면 자동으로 맨 뒤부터 차례대로 돌아가지만 그러면 좀 복잡하니 우리는 정수값 index만 탐색토록 할 것이다.

```python
def get(self, index):
    if index < 0 or index >= self.length:
        raise IndexError("인덱스 범위를 벗어났습니다.")
```

그러니 인덱스 범위를 리스트 길이까지만 정해주자.

만약 0이라면 그냥 head에 있는 data를 꺼내 보여주면 될 것이다.

```python
if index == 0:
    return self.head.data
```

그렇지 않다면 for문을 통해 데이터를 찾아가도록 한다.

```python
node = self.head
for i in range(0, index):
    node = node.next

return node.data
```

그럼 사용자가 지정한 index 길이만큼 for문이 돌 거고 그곳의 데이터를 찾아낼 것이다. index 값으로 2를 입력했다 가정해보자. 그럼 리스트의 3번째 데이터를 뽑아낼 것이다. for문이 0번째 루프, 1번째 루프, 2번째 루프에서 멈추고 인덱스 2번째의 데이터를 돌려줘 보여주게 될 것이다.

완성된 코드는 다음과 같다.

```python
    def get(self, index):
        if index < 0 or index >= self.length:
            raise IndexError("인덱스 범위를 벗어났습니다.")
        
        if index == 0: return self.head.data
        
        node = self.head
        for i in range(0, index):
            node = node.next

        return node.data
```

### 2. setter

set이란 기존에 입력된 데이터를 바꿔치기하는 것이다. 여기는 몇 번째 인덱스의 데이터를 무슨 데이터로 바꿀 것인지 알려주는 2개의 매개변수를 받아야 한다.

입력 받은 데이터를 각 자리에 맞게 바꿔치기하려면 getter와 로직이 크게 다르지 않다.

```python
def __setitem__(self, index, data):
    if index < 0 or index >= self.length:
            raise IndexError("인덱스 범위를 벗어났습니다.")

    if index == 0:
        self.head.data = data
        return

    if index == self.length -1:
        self.tail.data = data
        return

    node = self.head
    for i in range(0, index):
        node = node.next
    
    node.data = data

    return node.data
```

인덱스가 0이라면 당연히 head 데이터를 바꿔줄 것이고, 그렇지 않다면 for문을 통해 사용자가 입력한 index까지 내려간 다음 그곳의 node 데이터를 새 데이터로 바꿔버리면 완성된다.

### 3. delete

그럼 데이터를 삭제하려면 어떻게 하면 될까? pop은 맨 마지막 데이터만 없애면 되는 거였다. 따라서 next와 tail 노드를 끊어주면 알아서 사라졌지만, 여기서는 노드 조정을 좀 더 세심하게 해줘야 한다.

특정 인덱스의 데이터를 삭제하는 로직을 생각해보자.

일단 인덱스 범위를 지정하는 조건문은 그대로 들고 온다.

```python
def __delitem__(self, index):
    if index < 0 or index >= self.length:
            raise IndexError("인덱스 범위를 벗어났습니다.")
    
    if index == 0:
        self.head = self.head.next
        self.length -= 1
        return

    if index == self.length -1:
        self.pop()
        return

```

인덱스가 0이라면 조정할 노드는 하나 뿐이다. head로 연결된 노드를 그 다음 데이터로 이어버리면 되니까. 마지막 인덱스라면 그저 pop 함수 불러와 실행해버리면 끝난다.

그럼 중간에 낀 데이터는 어떻게 없애버릴 수 있을까?

head를 prev 변수값에 넣고 for문을 통해 index까지 접근한다. 여기서 주의해야 할 점은 range(0, index)를 그냥 넣으면 원하는 index+1 위치의 값을 날려버린다는 사실이다. 노드를 재연결하는 과정도 조금 복잡하다.

```python
prev = self.head
for i in range(0, index -1):
    prev = prev.next

prev.next = prev.next.next
self.length -= 1
```

현재 prev는 head에 있다. for문을 통해 그 다음 값을 새로운 prev에 넣는다. 원하는 위치까지 간 노드의 바로 다음 노드와 연결시키면 노드가 끊어진 데이터는 없어져버린다. prev 변수의 다음 다음 값을 prev.next에 넣으면 노드가 재연결되는 것이다.

아래는 완전한 함수이다.

```python
def __delitem__(self, index):
    if index < 0 or index >= self.length:
        raise IndexError("인덱스 범위를 벗어났습니다.")
    
    if index == 0:
        self.head = self.head.next
        self.length -= 1
        return
    
    if index == self.length - 1:
        self.pop()
        return
    
    prev = self.head
    for i in range(0, index -1):
        prev = prev.next
    
    prev.next = prev.next.next
    self.length -= 1
```