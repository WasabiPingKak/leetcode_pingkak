# 66 - Plus One

## 原題目連結
[66. Plus One](https://leetcode.com/problems/plus-one/description/)

## 題意
題目會給你一個整數陣列(array)，每個陣列元素都是一個正整數個位數字，你要將整個陣列中的值按照順序視為一個大整數。

將這個大整數加一之後，用原本輸入的陣列格式回傳結果。

## Example
```
Input: digits = [4,3,2,1]
Output: [4,3,2,2]

解釋: 輸入陣列中表示的正整數是 4321.
4321 + 1 = 4322.
所以答案就是回傳 [4,3,2,2]
```

## 解法
原則上這種長整數加法問題都是直接模擬直式加法，從最右邊的位數開始加，然後記得處理進位。

## 直式加法的慣例寫法
```c++
    // 假設陣列 digits[] 記錄了大整數每一位的值

    // 需要一個變數紀錄進位值
    int carry = 0;

    // 從最右邊開始加，一路往左進位
    for (int i = digits.size() - 1; i >= 0; i--) {
        // 做第 i 位的加法，要記得把上一位的進位值加進去
        digits[i] += carry + /*(根據題意看這裡還要加什麼有的沒的)*/;

        // 如果被加數有進位，除以 10 取得進位值(carry)
        carry = digits[i] / 10;

        // 算出除以 10 的餘數，以留下個位數的值
        digits[i] %= 10;

        // 迴圈進入下一輪，繼續計算左邊一位的和
    }
```

## 解答
```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        // 紀錄進位值
        int carry = 0;

        // 最後一位加 1
        digits[digits.size() - 1]++;

        // 用迴圈從右至左處理逐個位數的進位
        for (int i = digits.size() - 1; i >= 0; i--) {
            digits[i] += carry;
            carry = digits[i] / 10;
            digits[i] %= 10;
        }

        // 全部陣列內的值都加完，進位還在，就代表陣列最左邊要在插入一個 1
        if (carry == 1) {
            digits.insert(digits.begin(), 1);
        }

        return digits;
    }
};
```



