### 1.题目描述：

多级双向链表中，除了指向下一个节点和前一个节点指针之外，它还有一个子链表指针，可能指向单独的双向链表。这些子列表也可能会有一个或多个自己的子项，依此类推，生成多级数据结构，如下面的示例所示。

给定位于列表第一级的头节点，请扁平化列表，即将这样的多级双向链表展平成普通的双向链表，使所有结点出现在单级双链表中。

![图片](https://user-images.githubusercontent.com/42907149/141486608-29a0a9c4-48bf-40da-94e5-6143ebcc4e9b.png)


------------

### 2.解题思路：

该题选择用dfs方法

三种情况讨论：
- 将node与node下一节点next连接
- 将node与child连接
- 将last 与next连接

**展平的规则是一个节点的子链展平之后将插入该节点和它的下一个节点之间**

### 3. 代码实现：

**java版本：**

```JAVA
/*
// Definition for a Node.
class Node {
    public int val;
    public Node prev;
    public Node next;
    public Node child;
};
*/
//dfs与回溯
class Solution {
    public Node flatten(Node head) {
        dfs(head);
        return head;
    }

    public Node dfs(Node node) {
        Node cur = node;
        // 记录链表的最后一个节点
        Node last = null;

        while (cur != null) {
            Node next = cur.next;
            //  如果有子节点，那么首先处理子节点
            if (cur.child != null) {
                Node childLast = dfs(cur.child);

                next = cur.next;
                //  将 node 与 child 相连
                cur.next = cur.child;
                cur.child.prev = cur;

                //  如果 next 不为空，就将 last 与 next 相连
                if (next != null) {
                    childLast.next = next;
                    next.prev = childLast;
                }

                // 将 child 置为空
                cur.child = null;
                last = childLast;
            } else {
                last = cur;
            }
            cur = next;
        }
        return last;
    }
}


```
**C++版本：**

```C++
class Solution {
public:
    Node* flatten(Node* head) {
        flattenGetTail(head);
        return head;
    }

    //将当前层的链表展平
    Node* flattenGetTail(Node* head)
    {
        Node* node = head;
        Node* tail = nullptr;

        while (node != nullptr)
        {
            Node* next = node->next;                            //保存后一节点
            if (node->child != nullptr)
            {
                //这里注意：child代表子链表的头节点，childTail代表子链表的尾节点
                Node* child = node->child;
                Node* childTail = flattenGetTail(child);  //递归展平子链表，并返回子的尾节点

                node->child = nullptr;                          //切断child节点，只有切断了才变成双链表
                node->next = child;                             //将展平的子链表头结点插入到node之后
                child->prev = node;
                childTail->next = next;                         //将展平的子链表尾节点插入到next之前
                if (next != nullptr) next->prev = childTail;
                
                tail = childTail;                               //将tail指向尾节点
            }
            else
            {
                tail = node;        //没有child节点的情况，最后就直接返回当前节点
            }

            node = next;            //前往下一节点，即：node = node->next
        }

        return tail;                //返回该层链表的最后一个节点
    }
};



```
