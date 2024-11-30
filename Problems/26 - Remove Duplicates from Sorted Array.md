# 26 - Remove Duplicates from Sorted Array

## 原題目連結
[26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)

## 題意
給定一個陣列(array)包含一連串非遞減序列的元素，請刪除陣列中重複的元素，然後把剩下不重複的元素在原本的陣列中往左集中。

題目特別要求不可以使用額外的空間(in-place)，也是就是不能宣告新的陣列(array)或容器(container)來存放答案。

回傳值 k 代表這個陣列最後剩下多少不重複的元素。

## Example
```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: 你的函式回傳 k = 5，因為陣列中剩下的不重複元素分別為 0, 1, 2, 3, 4 五個。

然後因為傳入的陣列 nums 是 call by reference，因此直接對它修改即可。
陣列中第 k 個元素之後的內容不重要，不會被檢查到。
```

## 解法
理論上不會有人這樣寫程式。

但題目這樣要求了，我們就用兩個指標 `p`, `q` 來紀錄需要處理的元素位置。

`p` 指向要保留的元素位置，`q` 向後尋找與 `p` 不重複的元素值。

當 `q` 找到不重複值的時候，把這個不重複值放到 `p + 1` 的位置，然後 `p`, `q` 都向後移一位。

`p` 最後會停在保留的不重複元素的最後一個 `index`，但回傳值 `k` 要求的是不重複的元素數量，因此回傳 `p + 1`。

參數 `nums` 是 call by reference，所以滿足 in-place 的題意。

雖然理論上不會有人這樣寫程式。

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() == 1) {
            return 1;
        }

        int p = 0;
        int q = p + 1;

        while (q < nums.size()) {
            if (nums[p] != nums[q]) {
                nums[p + 1] = nums[q];
                p++;
            }
            q++;
        }

        return p + 1;
    }
};
```
