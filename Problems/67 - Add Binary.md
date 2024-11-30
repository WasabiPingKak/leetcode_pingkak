# 67 - Add Binary

## 原題目連結
[67. Add Binary](https://leetcode.com/problems/add-binary/description/)

## 題意
給兩個字串(strings) `a` 跟 `b`，代表兩個二進位值，回傳這兩個二進位值相加後的和，結果一樣以二進位值的字串回傳。

## Example
```
Input: a = "1010", b = "1011"
Output: "10101"
```

## 解法
將兩個字串靠最右邊對齊，做直式加法，記得處理進位。

## 直式加法的慣例寫法
```c++
    // 需要一個變數紀錄進位值
    int carry = 0;

    // 從最右邊開始加，一路往左進位
    for (int i = digits.size() - 1; i >= 0; i--) {
        // digit_a 與 digit_b 代表目前要計算和的那一位
        // 做第 i 位的加法，要記得把上一位的進位值加進去
        digits_sum[i] += carry + digit_a + digit_b;

        // 如果被加數有進位，除以 2 取得進位值(carry)
        carry = digits_sum[i] / 2;

        // 算出除以 2 的餘數，以留下個位數的值
        digits_sum[i] %= 2;

        // 迴圈進入下一輪，繼續計算左邊一位的和
    }
```

## 解答
直式加法要靠右對齊，所以我們將兩個字串 `a` 跟 `b` 分別用 `idx_a` 與 `idx_b` 指向要相加的位數，用 `carry` 紀錄進位值。

`a[idx_a]` 代表第 `idx_a` 位的 `字元(char)`，將 `a[idx_a]` 減去 ASCII `'0'`得到真正可運算的`整數值(int)`。

注意題目輸入的字串 `a` 跟 `b` 長度不一定相等，也沒有保證哪一個比較長，在迴圈遍歷時要特別處理 idx 超出邊界的情況。

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        // idx_a 與 idx_b 從字串的最右邊開始往左走。
        int idx_a = a.length() - 1;
        int idx_b = b.length() - 1;
        string ans;

        // 需要一個變數紀錄進位值
        int carry = 0;

        // 當兩個 idx 都小於 0，代表兩個字串都遍歷完畢，結束迴圈
        while (idx_a >= 0 || idx_b >= 0) {
            // 取得目前 idx 指向的字元，並且轉換成整數型態 digit_a, digit_b
            // 如果 idx < 0，代表該字串長度比較短，所以沒有字元的位置直接當成 0 處理
            // FYI: 這裡的寫法用到了 ternary operator
            int digit_a = (idx_a >= 0) ? a[idx_a] - '0' : 0;
            int digit_b = (idx_b >= 0) ? b[idx_b] - '0' : 0;

            // 從最右邊的字元開始，逐字元做加法，記得把上一位的進位加進去
            int digit_sum = digit_a + digit_b + carry;

            // 如果被加數有進位，除以 2 取得進位值(carry)，本題是二進位所以除以 2
            carry = digit_sum / 2;

            // 留下沒有進位的餘數值
            digit_sum %= 2;

            // 把當前字元的和放入答案中
            ans.push_back(digit_sum + '0');

            // 兩個 index 左移一個字元
            idx_a--;
            idx_b--;
        }

        // 最後可能還是會有一個進位，補上這個進位
        if (carry > 0) {
            ans.push_back('1');
        }

        // 因為前面計算答案的字元是由左至右放進 ans 內
        // 所以 ans 內的字串的順序是反的，最後再反過來
        reverse(ans.begin(), ans.end());

        return ans;
    }
};
```