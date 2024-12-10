# 19 - Remove Nth Node From End of List

## 原題目連結
[19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/description/)

## 題意
給一個指向 Linked-list 的 `head`，把倒數第 n 個節點刪掉，然後一樣回傳這個 list 的 `head`。

## Example
```
Example 1:
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]

Example 2:
Input: head = [1], n = 1
Output: []

Example 3:
Input: head = [1,2], n = 1
Output: [1]
```

## 解法
用兩個指標 `p`, `q` 來控制間距 n，先讓 `q` 從 `p` 的位置前進 (n-1) 位，然後兩個指標一起平移直到 `q` 掉出 Linked-list 的最後一個節點。

`q` 掉出去的那一刻，`p` 指向的下一個節點就是要被砍掉的節點。

有時候對 Linked-list 的操作，我們會加入一個 dummy head 來簡化程式的操作邏輯。

Dummy head 就像幾何作圖中的輔助線一樣，可以讓你的程式邏輯在過程中變得乾淨，不用針對 `head` 節點做邊界處理，用完記得擦掉就好。

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        // 宣告一個區域變數 dummy 作為 dummy head
        // 宣告為區域變數的原因為離開這個函式的作用域之後
        // dummy 佔用的記憶體位置就會自動被銷毀
        auto dummy = ListNode(-1, head);
        auto* p = &dummy;
        auto* q = &dummy;

        // 讓 q 前進 n-1 位
        for (int i = 0; i < n; i++) {
            q = q->next;
        }

        // p ,q 一起向前平移直到 q 掉出 list
        while (q->next != nullptr) {
            p = p->next;
            q = q->next;
        }

        // 此時 p 的下一個節點就是要被刪掉的節點
        ListNode* toBeDelete = p->next;
        p->next = p->next->next;

        // 雖然題目沒有要求
        // 但這裡手動釋放記憶體是一個好習慣
        delete toBeDelete;

        // 回傳 dummy head 的下一個節點，也就是真正的 head
        //
        // 由於 dummy head 是區域變數
        // 離開作用域之後就會被自動銷毀
        // 不用手動管理 dummy 的記憶體
        return dummy.next;
    }
};
```