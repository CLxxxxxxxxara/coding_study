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
