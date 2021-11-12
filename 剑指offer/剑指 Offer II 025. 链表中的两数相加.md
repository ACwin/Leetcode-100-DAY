### 1.题目描述

![图片](https://user-images.githubusercontent.com/42907149/141490695-293280dd-79a1-4b64-8b5b-de360768e561.png)

### 2.代码实现：

**C++版本**

```c++
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        //若任一链表为空，则直接返回另一链表
        if (l1 == nullptr || l2 == nullptr) return l1 == nullptr ? l2 : l1;

        l1 = reverseList(l1), l2 = reverseList(l2);     //反转链表，便于进行加法运算
        return reverseList(addList(l1, l2));            //相加后再反转，最后返回结果
    }

    ListNode* reverseList(ListNode* head)               //反转链表模板
    {
        if (head == nullptr) return head;
        ListNode *pre = head, *cur = head->next;
        
        while (cur)
        {
            ListNode *ne = cur->next;
            cur->next = pre;
            pre = cur, cur = ne;
        }

        head->next = nullptr;
        return pre;
    }

    ListNode* addList(ListNode* l1, ListNode* l2)       //链表相加
    {
        ListNode *dummy = new ListNode(0), *p = dummy;

        int carry = 0;              //进位标志
        while (l1 || l2)
        {
            //链表不为空就将当前的两个节点的值加起来，为空就加0
            carry += (l1 == nullptr ? 0 : l1->val) + (l2 == nullptr ? 0 : l2->val);
            
            //如果当前节点有进位，就取模后为1，没有进位就保持原值
            //p = p->next = xx 表示 p->next = xx, p = p->next两条语句的组合
            p = p->next = new ListNode(carry % 10);
            
            //如果有进位，则carry余1， 没有进位则carry重置为0
            carry /= 10;

            if (l1) l1 = l1->next;
            if (l2) l2 = l2->next;
        }

        //判断最高位是否有进位，有的话就再追加一个1即可
        if (carry) p->next = new ListNode(1);
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
    //反转链表
    private ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while (cur != null) {
            ListNode tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;
    }
    //  进位加一
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        l1 = reverseList(l1);
        l2 = reverseList(l2);
        ListNode reverHead = addReversed(l1,l2);
        return reverseList(reverHead);
        
        
    }
    private ListNode addReversed(ListNode l1, ListNode l2){
        ListNode dummy = new ListNode(0);
        ListNode sumNode = dummy;
        int carray = 0;
        while(l1 != null || l2 != null){
            int sum = (l1 == null ? 0 : l1.val) + (l2 == null ? 0 : l2.val) + carray;

            carray =  sum >= 10 ? 1 : 0;
            sum = sum >= 10 ? sum - 10 : sum;
            ListNode newNode = new ListNode(sum);

            sumNode.next = newNode;
            sumNode = sumNode.next;

            l1 = l1 == null ? null : l1.next;
            l2 = l2 == null ? null : l2.next;

        }

        if(carray > 0){
            sumNode.next = new ListNode(carray);
        }
        return dummy.next;
    }
}
```
