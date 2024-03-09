# LeetCode 刷题笔记
## 链表
### 回文链表
234. https://leetcode.cn/problems/palindrome-linked-list/description/

思路：

双指针 一快一慢，找到链表的后半的head（因为是单链表，不能反向遍历）

把后半reverse过来，再与前半进行比较

**注意reverse函数的写法**

``` c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode* fast, *slow;
        slow = head;
        fast = head;
        while(fast!=nullptr)
        {
            slow = slow->next;
            if (slow == nullptr) return true;
            ListNode *temp = fast->next;
            if (temp==nullptr) fast = temp;
            else fast = temp->next;

        }
        ListNode* right = Reverse(slow);
        ListNode* left = head;
        while(right!=nullptr)
        {
            if(right->val != left->val)
            {
                return false;
            }
            else
            {
                right = right->next;
                left = left-> next;
            }
        }
        return true;

    }
    ListNode* Reverse(ListNode *head)
    {
        ListNode *pre = nullptr, *cur = head;   // Define two pointers
        while (cur != nullptr) {   // Loop until the last node of the list is reached
            ListNode *next = cur->next;    // Store the next pointer temporarily
            cur->next = pre;               // Reverse the pointer
            pre = cur;                     // Move the pre pointer one step
            cur = next;                    // Move the cur pointer one step forward
        }
        return pre;            // Return the new head
    }
};
```
### 分隔链表
725 https://leetcode.cn/problems/split-linked-list-in-parts/description/

思路：

把每一个分隔开来的小链表看作一个小篮子，先遍历链表确定每个篮子里面装几个数字，再把链表split

要注意最后要求返回的list的形式，每一个元素是一个链表的指针
```python
class Solution:
    def splitListToParts(self, head: Optional[ListNode], k: int) -> List[Optional[ListNode]]:
        basket = [0]*k
        node_cur = head
        i=0
        while node_cur:
            basket[i]+=1
            i = (i+1)%k
            node_cur = node_cur.next

        final_list=[]
        for i in range(k):
            num = basket[i]
            if num == 0:
                small_list = None
            else:
                small_list = head
                prev = None
                curr = head
                for n in range(num):
                    prev = curr
                    curr = curr.next
                    

                head = curr
                prev.next = None
                
            final_list.append(small_list)
        return final_list
            
```
### 奇偶链表
328 https://leetcode.cn/problems/odd-even-linked-list/description/

思路不难，但是要注意的是循环结束的判断条件
```python
class Solution:
    def oddEvenList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        
        if head and head.next:
            odd_ptr = head
            even_ptr = head.next
        else :
            return head
        odd_head, even_head = odd_ptr, even_ptr
        odd_cur, even_cur = odd_head, even_head
        while even_cur :
            odd_ptr=odd_ptr.next.next
            if even_ptr.next:
                even_ptr = even_ptr.next.next
            else:
                even_ptr = None
            if not odd_ptr:
                break
            odd_cur.next = odd_ptr
            even_cur.next = even_ptr
            odd_cur = odd_cur.next
            even_cur = even_cur.next
        odd_cur.next = even_head
        return odd_head
```
