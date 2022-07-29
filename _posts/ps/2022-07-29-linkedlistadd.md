---
title:  "링크드 리스트를 이용한 숫자합"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - ps
tags:
  - PS
---

leetcode : (https://leetcode.com/problems/add-two-numbers/);

링크드리스트 2개에 들어있는 숫자의 합을 구하는 문제이다.

리스트 2개에 들어있는 숫자가 있을 때까지 while 문을 돌립니다

숫자의 합이 10 이상인 경우에 더해지는 값을 기록해둡니다.

ternary operation을 활용하여서 리스트들의 값 확인과

리스트의 포인터 값을 변경해줍니다.

주의해야 할점은

node의 next 포인터를 항상 ListNode 생성자를 이용해서 할당시켜주어야

포인터에 값이 할당되지 않아서 일어나는 예외상황을 피할 수 있습니다. 


```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode(0);
        ListNode* node = head;
        int val = 0;
        while (l1 || l2 || val != 0) {
            int x = l1 ? l1->val : 0;
            int y = l2 ? l2->val : 0;
            int sum = val + x + y;
            val = sum / 10;
            node->next = new ListNode(sum % 10);
            node = node->next;
            l1 = l1 ? l1->next : nullptr;
            l2 = l2 ? l2->next : nullptr;
        }
        return head->next;
    }
};
```