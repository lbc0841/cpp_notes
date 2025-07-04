---
title: "二分搜"

---

## 二分搜 Binary Search

### 時間複雜度

暴力遍歷 $` O(N) `$<br>
二分搜索 $` O(logN) `$<br>

使用二分搜索的目的是提升速度<br>
通常在 **需要多次查詢** 或是 **查詢範圍很大** 時會很有優勢

<details>
<summary>查詢次數對比</summary>

***
對於 $` 10^{6} `$ 筆資料的 vector 而言<br>
暴力總共需要比較 $` \color{Red} 10^{6} `$ 次<br>
而二分搜索則是 $` \left \lceil log_{2}10^{6} \right \rceil = \color{Red} 20 `$ 次

</details>

### 使用條件

二分搜用在 <font color=#ff0000>排好序</font> 的陣列上

### 加法溢位

m 為 l 與 r 的中點，也就是 $` \left \lfloor \frac{l+r}{2}\right \rfloor `$<br>
可以寫成<br>
`m = (l+r)/2;`<br>
<br>
但是數字過大時，l+r 會 <font color=#ff0000>溢位</font><br>
這時可以改寫成<br>
`m = l + (r-l)/2;`

### 惡補數學

閉區間 [l, r] 為 $` \left\{ x \in \mathbb{R}, l \leq x \leq r \right\} `$<br>
開區間 (l, r) 為 $` \left\{ x \in \mathbb{R}, l < x < r \right\} `$<br>
左閉右開 [l, r) 為 $` \left\{ x \in \mathbb{R}, l \leq x < r \right\} `$<br>
左開右閉 (l, r] 為 $` \left\{ x \in \mathbb{R}, l < x \leq r \right\} `$<br>
<br>
而程式碼中因為索引只能是整數<br>
所以 $ x \in \mathbb{Z} $<br>

2 2 2 5 6 6 7 8 9 9 10

### 二分搜模板

```cpp
while(/* 區間不為空時執行 */){
    int m = l + (r-l)/2;
    if(/* 判斷條件 */) /* 縮區間 */
    else /* 縮區間 */
}
```

## 區間

以下二分搜以 lower_bound 舉例<br>
(lower_bound：找到第一個 ≥ 目標的數)

c++ algorithm 已經有 lower_bound()<br>
因此以下代碼的函數以 my_lower_bound 命名

**以下代碼的**<br>
`l` 表示 left<br>
`r` 表示 right<br>
`m` 表示 middle<br>
`nums` 為一個由小到大排好序的 vector<br>
`target` 為要從中尋找的值

### 閉區間 [l, r]

```cpp
int my_lower_bound(vector<int>& nums, int target){

    // 閉區間 [l, r]
    int l = 0, ;
    int r = nums.size()-1; 

    while(l <= r){ // 區間不為空
        int m = l + (r-l)/2;

        if(nums[m] < target)
            l = m + 1;  // 縮區間 -> [m+1, r]
        else
            r = m - 1;  // 縮區間 -> [l, m-1]
    }

    return l;
}

```


### 左閉右開 [l, r)

包含 l：`nums[l]` 是有效的<br>
不包含 r：`nums[r]` 是無效的，不會取到

`l == r` 時，[l, r) 區間是空的所以跳出迴圈

```cpp
int my_lower_bound(vector<int>& nums, int target){

    // 左閉右開區間 [l, r)
    int l = 0;
    int r = nums.size();

    while(l < r){ // 區間不為空
        int m = l + (r-l)/2;

        if(nums[m] < target) 
            l = m + 1; // 縮區間 -> [m+1, r)
        else  
            r = m; // 縮區間 -> [l, m)
    }

    return l; // 或 r 也可以
}
```

**誤解 1**<br>
當 `nums[m] == target` 時，
執行的是 `r = m;`<br>
但 r 是右開區間的邊界<br>
不應該是有效索引<br>
為什麼這樣符合「右開」的定義？<br>
<br>
左閉右開指的是**搜索區間**，而不是目標值存在的區間<br>
`nums[m] == target` 時 `r = m;`<br>
下一次就會在 `[l, m)` 中進行搜索

### 開區間 (l, r)

```cpp
int my_lower_bound(vector<int>& nums, int target){

    // 開區間 (l, r)
    int l = -1;
    int r = nums.size();

    while(l+1 < r){ // 區間不為空
        int m = l + (r-l)/2;

        if(nums[m] < target)
            l = m; // 縮區間 -> (m, r)
        else
            r = m; // 縮區間 -> (r, m)
    }

    return 
}
```

### 總結：閉 → m±1，開 → m

## 尋找其他目標

要尋找的目標不一定是 **第一個 ≥ 目標的數**<br>
也有可能是<br>
**第一個 > 目標的數**<br>
`...`

upper_bound：找到 ≥ 目標的索引<br>
lower_bound：找到 ≥ 目標的索引

## 選擇什麼? 開區間? 閉區間?

個人喜歡用什麼就用什麼<br>
沒有規定，只要能 AC 都可以

但是建議使用左閉右開，符合 C++ 的習慣<br>
像是 sort vector 就是左閉右開<br>
`sort(v.begin(), v.end());`

## if 中的判斷條件

實際上寫題目不可能只有簡單的 `nums[m] < target`<br>
就能判斷要縮 l 或是縮 r<br>
<br>
這時候可以寫成一個函數

```cpp
bool f(/* 傳入相關的值 */){
    bool result;

    // 處理判斷

    return result;
}
```

根據 f 的回傳值做判斷

```cpp
while(/* 區間不為空時執行 */){
    int m = l + (r-l)/2;
    if( f(/* 傳入相關的值 */) ) /* 縮區間 */
    else /* 縮區間 */
}
```

## 特別注意

### 1. if 中的判斷條件需要與你所使用的區間相符

### 2. 注意處理找不到目標的情況

找不到目標時二分搜會返回 `nums.size()`<br>
直接帶入 `nums[nums.size()]` 會發生錯誤


## 題目<br>
Leetcode<br>
[704. 二分查找](https://leetcode.cn/problems/binary-search/description/)<br>
[35. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/description/)<br>
[744. 寻找比目标字母大的最小字母](https://leetcode.cn/problems/find-smallest-letter-greater-than-target/description/)<br>
[1351. 统计有序矩阵中的负数](https://leetcode.cn/problems/count-negative-numbers-in-a-sorted-matrix/description/)<br>

Zerojudge<br>
[c575. APCS 2017-0304-4基地台](https://zerojudge.tw/ShowProblem?problemid=c575)
