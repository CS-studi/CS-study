# Stack & Queue

# Stack

- LIFO(Last In First Out) 방식의 자료구조
- top : 가장 최근에 들어온 데이터를 가리킴, pop/push 연산이 이루어지는 위치
- 운영체제의 stack 영역에는 함수의 지역변수, 매개변수가 저장됨
    - stack overflow : 함수가 과도하게 호출되어 운영체제의 스택영역이 가득찬 경우
    - stack underflow : 비어있는 스택에서 원소를 추출할 경우
- 활용 예시
    - 웹 브라우저 뒤로가기 기능, 실행 취소 기능, 후위 표기법, DFS 구현


<details>
<summary>Array로 구현한 Stack</summary>


```java
import java.util.EmptyStackException;

public class StackArray {

    int top;
    int size;
    int[] stack;

    public StackArray(int size) {
        this.size = size;
        stack = new int[size];
        top = -1;
    }

    public void push(int item) {
        if (top >= size) {
            throw new StackOverflowError();
        }

        stack[top++] = item;
    }

    public int pop() {
        if (top == 0) {
            throw new EmptyStackException();
        }
        int data = stack[top];
        stack[top--] = 0;
        return data;
    }

    public int search(int target) {
        for (int i = 0; i < top; i++) {
            if (stack[i] == target) {
                return i;
            }
        }
        return -1;
    }
}
```

</details>


<details>
<summary>LinkedList로 구현한 Stack</summary>

```java
import java.util.EmptyStackException;

public class StackLinked {
    public static void main(String[] args) {
        StackLinked stack = new StackLinked();
        for (int i = 0; i < 10; i++) {
            stack.push(i);
        }

        stack.display(); // 9-> 8-> 7-> 6-> 5-> 4-> 3-> 2-> 1-> 0->
        System.out.println(stack.pop()); // 9
        stack.display(); // 8-> 7-> 6-> 5-> 4-> 3-> 2-> 1-> 0->
    }

    private Node top;

    public StackLinked() {
        top = null;
    }

    public boolean isEmpty() {
        return top == null;
    }

    public void push(int item) {
        Node node = new Node(item);
        node.next = top; // 연결
        top = node; // top은 Stack의 가장 최근에 들어온 데이터를 가리킨다.
    }

    public int pop() {
        if (top == null) {
            throw new EmptyStackException();
        }

        Node node = top;
        top = top.next;
        return node.item;
    }

    public int search(int target) {
        int id = 0;
        Node temp = top;

        while (temp != null) {
            if (temp.item == target) {
                return id;
            }

            temp = temp.next;
            id++;
        }

        return -1;
    }

    public void display() {
        if (top == null) {
            throw new EmptyStackException();
        }

        Node temp = top;
        while (temp != null) {
            System.out.print(temp.item + "-> ");
            temp = temp.next;
        }
        System.out.println();
    }

    public class Node {
        private int item;
        private Node next;

        public Node(int item) {
            this.item = item;
            next = null;
        }
    }
}
```


</details>


    
 
    
    

# Queue

- FIFO(First In First Out) 방식의 자료구조
- front : 가장 첫 번째 원소를 가리킴, 삭제연산(dequeue)이 수행됨
- rear : 가장 끝 원소를 가리킴, 삽입연산(enqueue)이 수행됨
- 활용 예시
    - 프로세스 스케쥴링, BFS 구현, LRU(오랫동안 사용하지 않는 데이터를 먼저 삭제) 알고리즘

<details>
<summary>Array로 구현한 Queue</summary>


```java
public class QueueArray {
    int front;
    int rear;
    int[] queue;

    public QueueArray(int size) {
        queue = new int[size];
        front = rear = 0;
    }

    public boolean isEmpty() {
        return front == rear;
    }

    public boolean isFull() {
        return rear == queue.length - 1;
    }

    public void enqueue(int item) {
        if (isFull()) {
            System.out.println("queue is full");
            return;
        }

        queue[rear++] = item;
    }

    public int dequeue() {
        if (isEmpty()) {
            System.out.println("queue is empty");
            return -1;
        }
        int data = queue[front];
        // 모든 원소를 한칸 앞으로 이동시킨다.
        for (int i = 0; i < rear - 1; i++) {
            queue[i] = queue[i + 1];
        }
        if (rear < queue.length) {
            queue[rear] = 0;
        }
        rear--;

        return data;
    }

    public void display() {
        if (isFull()) {
            System.out.println("queue is empty");
            return;
        }

        for (int i = front; i < rear; i++) {
            System.out.print(queue[i] + " <- ");
        }
        System.out.println();
    }
}
```

</details>



    

<details>
<summary>LinkedList로 구현한 Queue</summary>

```java
public class QueueLinked {

    public static void main(String[] args) {
        QueueLinked queue = new QueueLinked();
        for (int i = 0; i < 10; i++) {
            queue.enqueue(i);
        }
        queue.display(); // 0 - 1 - 2 - 3 - 4 - 5 - 6 - 7 - 8 - 9 -
        for (int i = 0; i < 5; i++) {
            queue.dequeue();
        }
        queue.display(); // 5 - 6 - 7 - 8 - 9 -
    }

    Node front, rear;

    public QueueLinked() {
        front = rear = null;
    }

    public boolean isEmpty() {
        return front == null && rear == null;
    }

    public void enqueue(int item) {
        Node node = new Node(item);
        if (isEmpty()) {
            front = rear = node;
        } else {
            rear.next = node;
            rear = node;
        }
    }

    public int dequeue() {
        if (isEmpty()) {
            System.out.println("queue is empty");
            return -1;
        }

        int data = front.item;
        front = front.next;

        if (front == null) {
            rear = null;
        }

        return data;
    }

    public void display() {
        if (isEmpty()) {
            System.out.println("queue is empty");
            return;
        }

        Node node = front;
        while (node != null) {
            System.out.print(node.item + " - ");
            node = node.next;
        }
        System.out.println();
    }

    public class Node {
        private int item;
        private Node next;

        public Node(int item) {
            this.item = item;
            next = null;
        }
    }
}
```
    


</details>


<details>
<summary>Stack으로 구현한 Queue</summary>


```java
import java.util.Stack;

public class QueueWithStack {

    private Stack<Integer> s1 = new Stack<>();
    private Stack<Integer> s2 = new Stack<>();

    public void enqueue(int item) {
        while (!s1.empty()) {
            s2.push(s1.pop());
        }

        s1.push(item);

        while (!s2.empty()) {
            s1.push(s2.pop());
        }
    }

    public int dequeue() {
        if (s1.empty()){
            System.out.println("queue is empty");
            return -1;
        }

        return s1.pop();
    }
}
```



</details>


  
    

# Deque

- queue 두 개를 좌우로 뒤집어 붙인 자료구조
- 양 쪽 끝(front, rear)에서 삽입, 삭제 연산을 모두 수행할 수 있음
- scroll : 입력이 한 쪽에서만 이루어지고, 출력은 양 쪽에서 이루어지는 deque
- shelf : 입력이 양 쪽에서 이루어지고, 출력이 한 쪽에서만 이루어지는 deque



<details>
<summary>Array로 구현한 Deque</summary>

```java
public class DequeWithArray {

    public static void main(String[] args) {
        DequeWithArray deque = new DequeWithArray(20);
        for (int i = 1; i <= 5; i++) {
            deque.insertFront(i);
        }
        deque.display(); // 5 4 3 2 1
        for (int i = 1; i <= 5; i++) {
            deque.insertRear(-i);
        }
        deque.display(); // 5 4 3 2 1 -1 -2 -3 -4 -5
    }

    private int arr[];
    private int front;
    private int rear;
    private int size;

    public DequeWithArray(int size) {
        arr = new int[100];
        front = -1;
        rear = 0;
        this.size = size;
    }

    public boolean isFull() {
        return ((front == 0 && rear == size - 1) || front == rear + 1);
    }

    public boolean isEmpty() {
        return front == -1;
    }

    public void insertFront(int item) {
        if (isFull()) {
            System.out.println("overflow");
            return;
        }

        if (front == -1) {
            front = rear = 0;
        } else if (front == 0) {
            front = size - 1;
        } else {
            front--;
        }

        arr[front] = item;
    }

    public void insertRear(int item) {
        if (isFull()) {
            System.out.println("overflow");
            return;
        }

        if (front == -1) {
            front = rear = 0;
        } else if (rear == size - 1) {
            rear = 0;
        } else {
            rear++;
        }

        arr[rear] = item;
    }

    public int deleteFront() {
        if (isEmpty()) {
            System.out.println("queue underflow");
            return -1;
        }
        int data = arr[front];
        if (front == rear) {
            front = rear = -1;
        } else if (front == size - 1) {
            front = 0;
        } else {
            front++;
        }

        return data;
    }

    public int deleteRear() {
        if (isEmpty()) {
            System.out.println("queue underflow");
            return -1;
        }

        int data = arr[rear];
        if (front == -1) {
            front = rear = -1;
        } else if (rear == 0) {
            rear = size - 1;
        } else {
            rear--;
        }
        return data;
    }

    public void display() {
        if (isEmpty()) {
            System.out.println("queue is empty");
            return;
        }

        if (front < rear) {
            for (int i = front; i <= rear; i++) {
                System.out.print(arr[i] + " ");
            }
            System.out.println();
        } else {
            for (int i = front; i < size; i++) {
                System.out.print(arr[i] + " ");
            }
            for (int i = 0; i <= rear; i++) {
                System.out.print(arr[i] + " ");
            }
            System.out.println();
        }
    }
}
```
</details>

