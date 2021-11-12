### 1.题目描述
![图片](https://user-images.githubusercontent.com/42907149/141491743-697bca7c-7d43-4754-8fa6-2cffc7bd02be.png)


### 2.代码实现：

**C++版本**
```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummy = new ListNode(0, head);    //使用哨兵节点（虚拟头节点）
        ListNode *front = head,*back = dummy;
        while(n--){
            front = front->next; 
        }
        while(front != nullptr){
            front = front->next;
            back = back->next;
        }
        //删除
        back->next= back->next->next;
        return dummy->next;

    }
};
```


**java版本**
```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);    //使用哨兵节点（虚拟头节点）
        ListNode front = head,back = dummy;
         for(int i=0;i<n;i++){
            front = front.next; 
        }
        while(front != null){
            front = front.next;
            back = back.next;
        }
        //删除
        back.next= back.next.next;
        return dummy.next;

    }
}
```
