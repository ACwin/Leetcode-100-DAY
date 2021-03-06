### 1.题目描述

 **[剑指 Offer II 029. 排序的循环链表](https://leetcode-cn.com/problems/4ueAj6/)** 
 
给定循环单调**非递减**列表中的一个点，写一个函数向这个列表中插入一个新元素 insertVal ，使这个列表仍然是循环**升序**的。

给定的可以是这个列表中任意一个顶点的指针，并不一定是这个列表中最小元素的指针。

如果有多个满足条件的插入位置，可以选择任意一个位置插入新的值，插入后整个列表仍然保持有序。

如果列表为空（给定的节点是 null），需要创建一个循环有序列表并返回这个节点。否则。请返回原先给定的节点。
![图片](https://user-images.githubusercontent.com/42907149/141479861-862f84d8-1c6b-48b0-8dda-a2b4098eac30.png)

---------------------------------------------------

### 2.思路分析：

- 链表无节点或者只存在一个节点，则直接新节点即可；
- 插入的值大于链表中原有的最大值，或者小于链表中原有的最小值，则新节点插入原来值的最大值和最小值之间；
- 插入的值介于原链表最大值和最小值之间，那么新节点插入点的前一个节点的值应该不大于新节点的值，后一个节点的值应该不小于新节点的值。

### 3.代码实现：

**JAVA实现：**

```java
/*
// Definition for a Node.
class Node {
    public int val;
    public Node next;

    public Node() {}

    public Node(int _val) {
        val = _val;
    }

    public Node(int _val, Node _next) {
        val = _val;
        next = _next;
    }
};
*/

class Solution {
    public Node insert(Node head, int insertVal) {
         Node node = new Node(insertVal);
        if (head == null) {
            head = node;
            head.next = head;
        }
        else if (head.next == head) {
            head.next = node;
            node.next = head;
        }
        else {
            insertCore(head, node);
        }
        return head;
    }

    private void insertCore(Node head, Node node) {
        Node cur = head;
        Node next = head.next;
        Node biggest = head;
        while (!(cur.val <= node.val && next.val >= node.val) && next != head) {
            cur = next;
            next = next.next;
            if (cur.val >= biggest.val) {
                biggest = cur;
            }
        }
        if (cur.val <= node.val && next.val >= node.val) {
            cur.next = node;
            node.next = next;
        }
        else {
            node.next = biggest.next;
            biggest.next = node;
        }
    }
};

```

**C++实现：**
```C++
/*
class Node {
public:
    int val;
    Node* next;

    Node() {}

    Node(int _val) {
        val = _val;
        next = NULL;
    }

    Node(int _val, Node* _next) {
        val = _val;
        next = _next;
    }
};
*/
class Solution {
public:
    Node* insert(Node* head, int insertVal) {
        Node* node = new Node(insertVal);
      	//处理链表为空的情况
        if (head == nullptr) {
            head = node;
            head->next = head;
        }//处理链表中只有一个节点的情况
        else if (head->next == head) {
            head->next = node;
            node->next = head;
        }
        else { //链表中超过一个节点的情况
            insertCore(head, node);
        }
        return head;
    }

    void insertCore(Node* head, Node* node) {
        Node* cur = head;
        Node* next = head->next;
        Node* biggest = head;
      //试图找到相邻的两个节点cur和next，使待插入值大于cur且小于next
        while (!(cur->val <= node->val && next->val >= node->val) && next != head) {
            cur = next;
            next = next->next;
          //实时更新最大值
            if (cur->val >= biggest->val) {
                biggest = cur;
            }
        }
        if (cur->val <= node->val && next->val >= node->val) {
            cur->next = node;
            node->next = next;
        } //如果没有找到符合条件的节点，说明新节点比所有的节点值都大或者都小
        else {//那么就把新节点插入到值最大的节点后面(也是最小值前面)
            node->next = biggest->next;
            biggest->next = node;
        }
    }
};

```
