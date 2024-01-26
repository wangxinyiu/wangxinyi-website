---
title: Manual implementation of data structures
type: page
---
- [Implementation of Stack](#implementation-of-stack)
	- [Using Linked List](#using-linked-list)
	- [Using Array](#using-array)
- [Implementation of Queue](#implementation-of-queue)
	- [Using Linked List](#using-linked-list-1)
	- [Using Array](#using-array-1)
  
## Implementation of Stack
### Using Linked List
   - Should we use Linked List or Double-Linked List?
```java
class ListNode {
	int value;
	ListNode next;
	
	// ListNode prev;
	public ListNode (int value) {
		this.value = value;
	}
}

public class Stack {
	// push, pop, peek
	// 必须有一个head
	private ListNode head;
	private int size;
	
	// constructor -> 最好是把 initialization 写到 constructor 里面。
	public Stack () {
		head = null;
		size = 0;
	}
	
	// 返回值是boolean 因为 如果超过size 需要返回false
	// 但是是有
	public boolean push (int value) {
		// I assume the linked list won't be full.
		ListNode newHead = new ListNode (value);
		NewHead.next = head;
		head = newHead;
		size++;
		return true;
	}
	
	// 用Integer是因为如果是null的话希望返回null
	public Integer pop () {
		if (isEmpty()) {
			return null;
		}
		ListNode res = head;
		head = head.next;
		// 断开那个节点
		res.next = null;
		size--;
		return res.value;
	}

	public Integer peek () {
		if (isEmpty()) {
			return null;
		}
		// 会自动 boxing 成 Integer
		return head.value;
	}

	public int size () {
		return size;
	}
	
	public boolean isEmpty () {
		return size == 0;
	}
}
```
### Using Array
```java
public class BoundedStack () {
public class BoundedStack {
    // pop push peek
    int[] array;
    // 因为stack是头进去头出 所以我们需要一个 pointer 去记录我现在存到哪了
    // 我第一个有意义的数 -> 一定要想清楚head的意义
    int head;

    // constructor -> 因为是 bounded 的，所以在初始化的时候需要规定 capacity.
    public BoundedStack (int cap) {
        this.array = new int[cap];
        this.head = 0;
    }

    public boolean push (int value) {
        if (isFull()) {
            return false;
        }
        array[head++] = value;
        return true;
    }

    public Integer peek() {
        if (head == 0) {
            return null;
        }
        return array[head];
    }

    public Integer pop() {
        if (head == 0) {
            return null;
        }
        head--;
        Integer res = array[head];
        return res;
    }

    public int size() {
        return head;
    }

    public boolean isEmpty() {
        return size() == 0;
    }

    public boolean isFull(){
        return size() == array.length;
    }
}
```

## Implementation of Queue
### Using Linked List
```java
class ListNode {
	int value;
	ListNode next;
	
	// ListNode prev;
	public ListNode (int value) {
		this.value = value;
	}
}

// offer, poll, peek
public class Queue {
	// 设置成 private
	// member variable
	private ListNode head;
	// stack 不需要 tail 是因为头进头出
	// 但是 queue 是尾进头出 所以需要一个 tail 
	private ListNode tail;
	private int size;
	// initialize the data in Constructor
	public Queue () {
		this.head = null;
		this.tail = null;
		this.size = 0;
	}
	
	public boolean offer (int value) {
		ListNode node = new ListNode(value);
		if (head == null) {
			tail = node;
			head = node;
		} else {
			tail.next = node;
			tail = tail.next;		
		}
		size++;
		return true;
	}
	
	public Integer peek () {
		return head == null ? null : head.value;	
	}

	public Integer poll () {
		if (isEmpty()) {
			return null;
		}
		ListNode node = head;
		head = head.next;
		node.next = null;
		size--;
		return node.value;
	}

	public int size() {
		return size;
	}

	public boolean isEmpty() {
		return size == 0;
	}
}
```

### Using Array
- 环形链表Cyclic
```java
public class BoundedQueue () {
	// offer pop peek size isEmpty
	int[] array;
	int head; //第一个有意义的 element
	int tail; //最后一个有意义的 element
	// 因为是环形链表 所以我需要维护 一个size
	int size;
	public BoundedQueue (int cap) {
		this.array = new int[cap];
		this.head = 0;
		this.tail = 0;	
		this.size = 0;
	}

	public boolean offer (int value) {
		if (isFull) {
			return false;
		}
		array[tail] = ele;
		tail = tail + 1 == array.length ? 0 : tail + 1;
		size++;
		return true;
	}

	public Integer peek() {
		return isEmpty() ? null : array[head];
	}

	public int size () {
		return size;
	}

	public boolean isEmpty() {
		return size == 0;
	}
	
	public boolean isFull() {
		return size == array.length;
	}
}
```