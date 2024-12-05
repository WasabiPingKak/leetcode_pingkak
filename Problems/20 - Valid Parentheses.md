# 20 - Valid Parentheses

## 原題目連結
[20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/)

## 題意
給你一段只包含大中小括號的字串，也就是字串裡只包含這些字元: `'('`, `')'`, `'{'`, `'}'`, `'['`, `']'`。

判斷這組括號是否合法。


## Example
```
Example 1:
Input: s = "()"
Output: true

Example 2:
Input: s = "()[]{}"
Output: true

Example 3:
Input: s = "(]"
Output: false

Example 4:
Input: s = "([])"
Output: true
```

## 解法
經典的 Stack 資料結構練習題。

### 什麼是 Stack
如果你不知道什麼是 Stack，建議鼓勵推薦你先去網路上查一下這個經典的資料結構是什麼。

在這裡還是簡單解釋一下它的特性。

C++ 的 STL 有提供 stack 的標準容器，stack 的特性就是像一個品客洋芋片的罐子，只有一個開口可以放東西進去跟拿東西出來，而且你只能看到且只能拿最上面的東西。

所以，先放進去的東西會堆在最底部，最後才拿得出來，也就是俗稱的 FILO (First In, Last Out)。

Stack 這個標準容器最常用的成員函式(Member function)為
* `top()`：看一下最頂端的元素
* `push(x)`：把 x 放到 stack 的頂端
* `pop()`：把 stack 最頂端的元素刪掉
* `empty()`：判斷這個 stack 是不是空的

不過在 C++ STL 裡，對一個空的 stack 使用 `top()` 與 `pop()` 會爆炸，所以要先檢查 `empty()`。

### 解題思路
合法的括號結構是「後開的括號需要先關閉」，這正好符合堆疊的特性。

例如，`( [ { } ] )` 中，最後開的 `'{'` 需要最先被處理來匹配閉合。

所以在這一題的思路中，按照順序，遇到左括號直接放進 stack，遇到右括號時，檢查這個右括號是否能跟 stack 頂端的左括號合法的閉合，若可以，將左括號由 stack 的頂端中移除。

當所有的括號都被處理完，且 stack 為空的的時候，代表所有的括號都有正確的匹配。

反之，當所有的括號都被處理完，但 stack 不為空，代表有左括號沒有被正確的閉合，式子不合法。

或，處理到右括號時但 stack 的頂端沒有相同類型的左括號，無法合法閉合移除 stack 頂端的左括號，代表括號類型交錯，式子同樣不合法。

```c++
class Solution {
public:
    bool isValid(string s) {
        // 宣告一個元素為字元(char)的 stack 容器
        stack<char> stack_brackets;

        for (int i = 0; i < s.size(); i++) {
            // 逐字元判斷，若是左括號，直接 push 到 stack 裡
            if (s[i] == '(' || s[i] == '[' || s[i] == '{') {
                stack_brackets.push(s[i]);
            } else {
                // 走到這裡代表目前的字元是右括號: ')',']','}'
                if (stack_brackets.empty()) {
                    // 如果現在要處理的字元是右括號，但是 stack 是空的
                    // 代表右括號太多了，無法合法閉合
                    return false;
                }

                // 把 stack 頂端的元素撿出來看
                char top = stack_brackets.top();

                // 根據右括號的類型，確認是下面三種樣式的那一種
                if (top == '(' && s[i] == ')'
                    || top == '[' && s[i] == ']'
                    || top == '{' && s[i] == '}') {
                    // 只要符合其中一個配對就是合法的配對
                    // 所以移除 stack 頂端的 左括號
                    stack_brackets.pop();
                } else {
                    // 如果上面三個樣式都不符合
                    // 代表括號類型交錯出現
                    // 式子不合法
                    return false;
                }
            }
        }

        // 最後判斷 stack 是否為空
        // 如果 stack 不為空，代表左括號太多了，沒有合法閉合，return false
        // 如果 stack 是空的，代表所有的括號類型都正確的閉合，return true
        return stack_brackets.empty();
    }
};
```

## 時間複雜度
O(n)

我們只用了一次迴圈遍歷完字元就得到解答，所以是線性時間。