# Thuật toán C++ cho LeetCode / NeetCode

> Phần 1: các hàm thuật toán **có sẵn** trong thư viện chuẩn (gọi là dùng).
> Phần 2: các **kỹ thuật giải bài** bạn tự viết, xếp theo chủ đề của NeetCode 150.

Xem thêm: [Cấu trúc dữ liệu C++](README.md)

## Mục lục

**Phần 1 – Hàm thuật toán có sẵn (STL)**

1. Sắp xếp
2. Tìm kiếm
3. Min / Max
4. Đếm và tính tổng
5. Biến đổi mảng
6. Hoán vị
7. Toán học

**Phần 2 – Kỹ thuật giải bài**

1. Hashing (đếm / tra cứu bằng hashmap)
2. Prefix Sum (tổng tiền tố)
3. Two Pointers (hai con trỏ)
4. Sliding Window (cửa sổ trượt)
5. Monotonic Stack (ngăn xếp đơn điệu)
6. Binary Search (tìm kiếm nhị phân)
7. Linked List: Fast & Slow Pointer, Dummy Node
8. Tree DFS (đệ quy trên cây)
9. BFS theo tầng
10. Backtracking (quay lui)
11. Heap / Top K
12. Graph DFS / BFS trên lưới
13. Topological Sort (sắp xếp tô-pô)
14. Union-Find (DSU)
15. Dijkstra (đường đi ngắn nhất có trọng số)
16. Dynamic Programming (quy hoạch động)
17. Greedy (tham lam)
18. Intervals (xử lý khoảng)
19. Bit Manipulation (thao tác bit)

**Phụ lục:** Bảng nhận diện nhanh và bảng độ phức tạp theo giới hạn n

---

# PHẦN 1 – HÀM THUẬT TOÁN CÓ SẴN (STL)

Đa số nằm trong `#include <algorithm>`, một số trong `<numeric>`. Trên LeetCode dùng `#include <bits/stdc++.h>` là có đủ.

Các hàm này nhận **khoảng iterator** `[first, last)`: tính từ `first`, **không** tính `last`. Vì vậy `v.begin(), v.end()` nghĩa là cả mảng, còn `v.begin(), v.begin() + k` là k phần tử đầu.

## 1. Sắp xếp

| Hàm | Tác dụng | Độ phức tạp |
|---|---|---|
| `sort(v.begin(), v.end())` | Sắp tăng dần | O(n log n) |
| `sort(v.begin(), v.end(), greater<int>())` | Sắp giảm dần | O(n log n) |
| `sort(v.begin(), v.end(), cmp)` | Sắp theo quy tắc tự định nghĩa (lambda / hàm so sánh) | O(n log n) |
| `stable_sort(v.begin(), v.end(), cmp)` | Như `sort` nhưng **giữ nguyên thứ tự** các phần tử bằng nhau | O(n log n) |
| `partial_sort(v.begin(), v.begin()+k, v.end())` | Chỉ sắp đúng k phần tử nhỏ nhất ở đầu | O(n log k) |
| `nth_element(v.begin(), v.begin()+k, v.end())` | Đưa phần tử đứng thứ k (nếu sắp) vào đúng vị trí, bên trái ≤, bên phải ≥ | O(n) trung bình |
| `is_sorted(v.begin(), v.end())` | Kiểm tra mảng đã tăng dần chưa | O(n) |

```cpp
// Sắp mảng điểm theo khoảng cách tới gốc
sort(points.begin(), points.end(), [](auto& a, auto& b) {
    return a[0]*a[0] + a[1]*a[1] < b[0]*b[0] + b[1]*b[1];
});

// Sắp theo phần tử thứ 2, bằng nhau thì theo phần tử thứ 1
sort(v.begin(), v.end(), [](auto& a, auto& b) {
    if (a.second != b.second) return a.second < b.second;
    return a.first < b.first;
});
```

> Hàm so sánh phải trả `true` khi `a` **đứng trước** `b`, và dùng `<` chứ không dùng `<=` (dùng `<=` có thể làm chương trình lỗi).

## 2. Tìm kiếm

| Hàm | Tác dụng | Độ phức tạp |
|---|---|---|
| `find(v.begin(), v.end(), x)` | Iterator tới x đầu tiên, không có thì trả `v.end()` | O(n) |
| `find_if(v.begin(), v.end(), pred)` | Phần tử đầu tiên thỏa điều kiện `pred` | O(n) |
| `binary_search(v.begin(), v.end(), x)` | Mảng **đã sort**: `true` nếu có x | O(log n) |
| `lower_bound(v.begin(), v.end(), x)` | Mảng **đã sort**: iterator tới phần tử đầu tiên **≥ x** | O(log n) |
| `upper_bound(v.begin(), v.end(), x)` | Mảng **đã sort**: iterator tới phần tử đầu tiên **> x** | O(log n) |

```cpp
vector<int> v = {1, 3, 3, 5, 7};
int i = lower_bound(v.begin(), v.end(), 3) - v.begin();   // 1 (chỉ số)
int j = upper_bound(v.begin(), v.end(), 3) - v.begin();   // 3
int soLan3 = j - i;                                       // 2: số lần xuất hiện của 3
```

> Lấy **chỉ số** từ iterator: trừ đi `v.begin()`.

## 3. Min / Max

| Hàm | Tác dụng | Độ phức tạp |
|---|---|---|
| `max(a, b)` / `min(a, b)` | Lớn nhất / nhỏ nhất của 2 giá trị (phải **cùng kiểu**) | O(1) |
| `max({a, b, c})` | Lớn nhất của nhiều giá trị | O(k) |
| `*max_element(v.begin(), v.end())` | Giá trị lớn nhất của mảng | O(n) |
| `*min_element(v.begin(), v.end())` | Giá trị nhỏ nhất của mảng | O(n) |
| `max_element(...) - v.begin()` | **Chỉ số** của phần tử lớn nhất | O(n) |
| `INT_MAX`, `INT_MIN`, `LLONG_MAX` | Giá trị giới hạn, dùng để khởi tạo biến min/max | — |

> Lỗi hay gặp: `max(a, 0LL)` khi `a` là `int` sẽ không biên dịch được vì khác kiểu. Viết `max<long long>(a, 0)` hoặc ép kiểu cho giống nhau.

## 4. Đếm và tính tổng

| Hàm | Tác dụng | Độ phức tạp |
|---|---|---|
| `count(v.begin(), v.end(), x)` | Số lần x xuất hiện | O(n) |
| `count_if(v.begin(), v.end(), pred)` | Số phần tử thỏa điều kiện | O(n) |
| `accumulate(v.begin(), v.end(), 0)` | Tổng mảng (`<numeric>`). Giá trị khởi tạo quyết định kiểu kết quả: dùng `0LL` nếu tổng có thể tràn `int` | O(n) |
| `partial_sum(v.begin(), v.end(), pre.begin())` | Tạo mảng tổng tiền tố (`<numeric>`) | O(n) |

```cpp
long long tong = accumulate(nums.begin(), nums.end(), 0LL);
int soChan = count_if(nums.begin(), nums.end(), [](int x) { return x % 2 == 0; });
```

## 5. Biến đổi mảng

| Hàm | Tác dụng | Độ phức tạp |
|---|---|---|
| `reverse(v.begin(), v.end())` | Đảo ngược mảng / chuỗi | O(n) |
| `fill(v.begin(), v.end(), x)` | Gán toàn bộ bằng x | O(n) |
| `iota(v.begin(), v.end(), 0)` | Gán 0, 1, 2, … (`<numeric>`), hay dùng khởi tạo mảng cha cho Union-Find | O(n) |
| `swap(a, b)` | Hoán đổi 2 biến | O(1) |
| `unique(v.begin(), v.end())` | Mảng **đã sort**: dồn các phần tử khác nhau lên đầu, trả iterator tới cuối phần đó | O(n) |
| `v.erase(unique(v.begin(), v.end()), v.end())` | Kết hợp với `sort` để **xóa phần tử trùng** | O(n) |
| `rotate(v.begin(), v.begin()+k, v.end())` | Xoay trái mảng k vị trí | O(n) |

```cpp
// Xóa phần tử trùng
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());
```

## 6. Hoán vị

| Hàm | Tác dụng | Độ phức tạp |
|---|---|---|
| `next_permutation(v.begin(), v.end())` | Biến mảng thành hoán vị **kế tiếp** theo thứ tự từ điển; trả `false` nếu đã là hoán vị cuối | O(n) |
| `prev_permutation(v.begin(), v.end())` | Hoán vị liền trước | O(n) |

```cpp
sort(v.begin(), v.end());                 // bắt đầu từ hoán vị nhỏ nhất
do {
    // xử lý hoán vị v
} while (next_permutation(v.begin(), v.end()));
```

> Phỏng vấn thường yêu cầu **tự viết** sinh hoán vị bằng backtracking (bài Permutations), không chỉ gọi hàm này.

## 7. Toán học

| Hàm | Tác dụng | Độ phức tạp |
|---|---|---|
| `abs(x)` | Giá trị tuyệt đối | O(1) |
| `gcd(a, b)` / `lcm(a, b)` | Ước / bội chung (`<numeric>`, C++17) | O(log min) |
| `pow(a, b)` | Lũy thừa, trả về **số thực**: không dùng cho số nguyên lớn | O(1) |
| `sqrt(x)` | Căn bậc hai (số thực) | O(1) |
| `a % m` | Chia lấy dư. Với số âm, kết quả có thể âm: dùng `(a % m + m) % m` | O(1) |

---

# PHẦN 2 – KỸ THUẬT GIẢI BÀI

Mỗi kỹ thuật gồm: **Ý tưởng**, **Dấu hiệu nhận biết** (đọc đề thấy gì thì nghĩ tới nó), **Khung code**, **Độ phức tạp**, và **Bài NeetCode** để luyện.

## 1. Hashing (đếm / tra cứu bằng hashmap)

**Ý tưởng:** đổi bộ nhớ lấy tốc độ. Lưu những gì đã gặp vào `unordered_set` / `unordered_map` để tra cứu O(1), thay cho vòng lặp lồng nhau O(n²).

**Dấu hiệu:** "có trùng không", "đếm số lần", "tìm cặp có tổng / hiệu bằng k", "nhóm các phần tử giống nhau".

```cpp
// Two Sum: với mỗi số, kiểm tra "phần bù" đã gặp chưa
unordered_map<int, int> pos;               // giá trị -> chỉ số
for (int i = 0; i < nums.size(); i++) {
    int need = target - nums[i];
    if (pos.count(need)) return {pos[need], i};
    pos[nums[i]] = i;
}
return {};
```

**Độ phức tạp:** O(n) thời gian, O(n) bộ nhớ.

**Bài NeetCode:** Contains Duplicate, Valid Anagram, Two Sum, Group Anagrams, Top K Frequent Elements, Longest Consecutive Sequence.

## 2. Prefix Sum (tổng tiền tố)

**Ý tưởng:** tính trước `pre[i]` = tổng `nums[0..i-1]`. Khi đó tổng đoạn `[l, r]` = `pre[r+1] - pre[l]`, tính trong O(1).

**Dấu hiệu:** "tổng đoạn con", "đếm số mảng con có tổng = k", "tích / tổng của mọi phần tử trừ chính nó".

```cpp
int n = nums.size();
vector<long long> pre(n + 1, 0);
for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + nums[i];
long long tongDoan = pre[r + 1] - pre[l];     // tổng nums[l..r]

// Đếm số mảng con có tổng = k (prefix sum + hashmap)
unordered_map<long long, int> cnt;
cnt[0] = 1;
long long sum = 0; int ans = 0;
for (int x : nums) {
    sum += x;
    if (cnt.count(sum - k)) ans += cnt[sum - k];
    cnt[sum]++;
}
```

**Độ phức tạp:** O(n) tạo mảng, O(1) mỗi truy vấn.

**Bài NeetCode:** Product of Array Except Self (dùng tích tiền tố + hậu tố).

## 3. Two Pointers (hai con trỏ)

**Ý tưởng:** dùng 2 chỉ số di chuyển trên mảng thay cho 2 vòng lặp lồng nhau. Hai kiểu chính:
- **Hai đầu đi vào:** `l = 0`, `r = n-1`, tiến dần vào giữa (mảng thường **đã sort**).
- **Cùng chiều:** một con trỏ đọc, một con trỏ ghi.

**Dấu hiệu:** mảng / chuỗi **đã sắp xếp**, "tìm cặp", "palindrome", "đảo ngược tại chỗ", "xóa phần tử tại chỗ".

```cpp
// Two Sum II (mảng đã sort)
int l = 0, r = nums.size() - 1;
while (l < r) {
    int s = nums[l] + nums[r];
    if (s == target) return {l + 1, r + 1};
    if (s < target) l++;       // tổng nhỏ quá -> tăng bên trái
    else r--;                  // tổng lớn quá -> giảm bên phải
}

// Valid Palindrome
int i = 0, j = s.size() - 1;
while (i < j) {
    if (!isalnum(s[i])) { i++; continue; }
    if (!isalnum(s[j])) { j--; continue; }
    if (tolower(s[i]) != tolower(s[j])) return false;
    i++; j--;
}
return true;
```

**Độ phức tạp:** O(n) (cộng thêm O(n log n) nếu phải sort trước).

**Bài NeetCode:** Valid Palindrome, Two Sum II, 3Sum, Container With Most Water, Trapping Rain Water.

## 4. Sliding Window (cửa sổ trượt)

**Ý tưởng:** duy trì một đoạn liên tiếp `[l, r]`. Mở rộng `r` từng bước; khi đoạn **vi phạm điều kiện** thì thu hẹp `l`. Mỗi phần tử vào và ra cửa sổ tối đa một lần.

**Dấu hiệu:** "mảng con / chuỗi con **liên tiếp** dài nhất / ngắn nhất thỏa điều kiện", "đoạn độ dài k".

```cpp
// Longest Substring Without Repeating Characters
unordered_map<char, int> cnt;
int l = 0, best = 0;
for (int r = 0; r < s.size(); r++) {
    cnt[s[r]]++;                          // đưa s[r] vào cửa sổ
    while (cnt[s[r]] > 1) {               // vi phạm: có ký tự trùng
        cnt[s[l]]--;                      // bỏ s[l] ra
        l++;
    }
    best = max(best, r - l + 1);
}
return best;
```

**Độ phức tạp:** O(n).

**Bài NeetCode:** Best Time to Buy and Sell Stock, Longest Substring Without Repeating Characters, Longest Repeating Character Replacement, Permutation in String, Minimum Window Substring, Sliding Window Maximum.

## 5. Monotonic Stack (ngăn xếp đơn điệu)

**Ý tưởng:** giữ stack luôn tăng (hoặc giảm). Khi phần tử mới phá vỡ tính đơn điệu, pop các phần tử ở đỉnh: phần tử mới chính là "phần tử lớn hơn kế tiếp" của chúng.

**Dấu hiệu:** "phần tử lớn hơn / nhỏ hơn **gần nhất** bên phải / trái", "bao nhiêu ngày nữa thì ấm hơn", "hình chữ nhật lớn nhất".

```cpp
// Daily Temperatures
int n = temps.size();
vector<int> ans(n, 0);
stack<int> st;                                    // lưu chỉ số, nhiệt độ giảm dần
for (int i = 0; i < n; i++) {
    while (!st.empty() && temps[st.top()] < temps[i]) {
        int j = st.top(); st.pop();
        ans[j] = i - j;                           // i là ngày ấm hơn đầu tiên sau j
    }
    st.push(i);
}
return ans;
```

**Độ phức tạp:** O(n), vì mỗi phần tử được push và pop tối đa một lần.

**Bài NeetCode:** Valid Parentheses (stack thường), Min Stack, Daily Temperatures, Car Fleet, Largest Rectangle in Histogram.

## 6. Binary Search (tìm kiếm nhị phân)

**Ý tưởng:** mỗi bước loại **một nửa** khoảng tìm kiếm. Dùng được khi có tính **đơn điệu**: điều kiện sai ở một bên và đúng ở bên kia.

Hai dạng:
- **Tìm trên mảng đã sort.**
- **Tìm trên đáp án:** đáp án nằm trong `[lo, hi]` và có hàm `ok(x)` dạng sai…sai đúng…đúng. Tìm x nhỏ nhất làm `ok(x)` đúng.

**Dấu hiệu:** mảng đã sort / đã xoay, "tìm giá trị **nhỏ nhất / lớn nhất** sao cho…", giới hạn n hoặc giá trị rất lớn (10⁹).

```cpp
// Dạng 1: tìm target trong mảng đã sort
int l = 0, r = nums.size() - 1;
while (l <= r) {
    int mid = l + (r - l) / 2;          // tránh tràn số so với (l + r) / 2
    if (nums[mid] == target) return mid;
    if (nums[mid] < target) l = mid + 1;
    else r = mid - 1;
}
return -1;

// Dạng 2: tìm trên đáp án – Koko Eating Bananas
int lo = 1, hi = *max_element(piles.begin(), piles.end());
while (lo < hi) {
    int k = lo + (hi - lo) / 2;
    long long hours = 0;
    for (int p : piles) hours += (p + k - 1) / k;   // làm tròn lên
    if (hours <= h) hi = k;          // ăn kịp -> thử tốc độ nhỏ hơn
    else lo = k + 1;                 // không kịp -> phải nhanh hơn
}
return lo;
```

**Độ phức tạp:** O(log n) cho dạng 1; O(n · log(giá trị)) cho dạng 2.

**Bài NeetCode:** Binary Search, Search a 2D Matrix, Koko Eating Bananas, Find Minimum in Rotated Sorted Array, Search in Rotated Sorted Array, Time Based Key-Value Store, Median of Two Sorted Arrays.

## 7. Linked List: Fast & Slow Pointer, Dummy Node

**Ý tưởng:**
- **Fast & slow:** con trỏ nhanh đi 2 bước, con trỏ chậm đi 1 bước. Nếu có chu trình thì hai con trỏ sẽ gặp nhau; khi con trỏ nhanh tới cuối thì con trỏ chậm đang ở giữa.
- **Dummy node:** tạo một node giả đứng trước `head` để không phải xử lý riêng trường hợp đầu danh sách.

**Dấu hiệu:** "phát hiện chu trình", "tìm node giữa", "xóa node thứ n từ cuối", "ghép / sắp xếp lại danh sách".

```cpp
// Linked List Cycle
ListNode *slow = head, *fast = head;
while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
    if (slow == fast) return true;
}
return false;

// Merge Two Sorted Lists với dummy node
ListNode dummy(0);
ListNode* tail = &dummy;
while (l1 && l2) {
    if (l1->val < l2->val) { tail->next = l1; l1 = l1->next; }
    else                   { tail->next = l2; l2 = l2->next; }
    tail = tail->next;
}
tail->next = l1 ? l1 : l2;
return dummy.next;
```

**Độ phức tạp:** O(n) thời gian, O(1) bộ nhớ.

**Bài NeetCode:** Reverse Linked List, Merge Two Sorted Lists, Linked List Cycle, Reorder List, Remove Nth Node From End of List, Find the Duplicate Number.

## 8. Tree DFS (đệ quy trên cây)

**Ý tưởng:** giải bài toán cho cây bằng cách giải cho **cây con trái** và **cây con phải**, rồi ghép kết quả. Luôn bắt đầu bằng **base case** `if (!root)`.

Ba thứ tự duyệt:
- **Preorder:** gốc → trái → phải.
- **Inorder:** trái → gốc → phải. Với BST, inorder cho ra dãy **tăng dần**.
- **Postorder:** trái → phải → gốc. Dùng khi cần kết quả của con trước, ví dụ tính chiều cao.

**Dấu hiệu:** gần như mọi bài có `TreeNode*`.

```cpp
// Khung chung
int dfs(TreeNode* root) {
    if (!root) return 0;                    // 1. base case
    int left  = dfs(root->left);            // 2. giải cây con trái
    int right = dfs(root->right);           // 3. giải cây con phải
    return 1 + max(left, right);            // 4. ghép kết quả (ở đây: chiều cao)
}

// Validate BST: truyền khoảng giá trị hợp lệ xuống
bool valid(TreeNode* node, long long lo, long long hi) {
    if (!node) return true;
    if (node->val <= lo || node->val >= hi) return false;
    return valid(node->left, lo, node->val) && valid(node->right, node->val, hi);
}
// gọi: valid(root, LLONG_MIN, LLONG_MAX)
```

**Độ phức tạp:** O(n) thời gian, O(h) bộ nhớ cho ngăn xếp đệ quy (h = chiều cao cây).

**Bài NeetCode:** Invert Binary Tree, Maximum Depth, Diameter of Binary Tree, Balanced Binary Tree, Same Tree, Subtree of Another Tree, Lowest Common Ancestor of a BST, Validate BST, Kth Smallest Element in a BST, Binary Tree Maximum Path Sum.

## 9. BFS theo tầng

**Ý tưởng:** dùng `queue`, xử lý **từng tầng một**: ở đầu mỗi vòng, `q.size()` chính là số node của tầng hiện tại.

**Dấu hiệu:** "theo từng tầng / mức", "nhìn từ bên phải", "đường đi ngắn nhất khi mỗi bước tốn như nhau".

```cpp
// Binary Tree Level Order Traversal
vector<vector<int>> res;
if (!root) return res;
queue<TreeNode*> q;
q.push(root);
while (!q.empty()) {
    int sz = q.size();                   // số node ở tầng này
    vector<int> level;
    for (int i = 0; i < sz; i++) {
        TreeNode* node = q.front(); q.pop();
        level.push_back(node->val);
        if (node->left)  q.push(node->left);
        if (node->right) q.push(node->right);
    }
    res.push_back(level);
}
return res;
```

**Độ phức tạp:** O(n).

**Bài NeetCode:** Binary Tree Level Order Traversal, Binary Tree Right Side View, Count Good Nodes in Binary Tree.

## 10. Backtracking (quay lui)

**Ý tưởng:** xây đáp án từng bước. Ở mỗi bước, thử từng lựa chọn: **chọn → đệ quy → bỏ chọn**. Thực chất là duyệt hết mọi khả năng một cách có hệ thống.

**Dấu hiệu:** "liệt kê **tất cả**": tập con, hoán vị, tổ hợp, cách chia chuỗi, đặt quân hậu. n thường nhỏ (≤ 20).

```cpp
// Subsets – khung chuẩn
vector<vector<int>> res;
vector<int> cur;

void backtrack(vector<int>& nums, int start) {
    res.push_back(cur);                       // mỗi trạng thái là một tập con
    for (int i = start; i < nums.size(); i++) {
        cur.push_back(nums[i]);               // 1. chọn
        backtrack(nums, i + 1);               // 2. đệ quy
        cur.pop_back();                       // 3. bỏ chọn (quay lui)
    }
}
```

> Có phần tử trùng (Subsets II, Combination Sum II): **sort** trước, rồi thêm `if (i > start && nums[i] == nums[i-1]) continue;` trong vòng lặp để bỏ nhánh trùng.

**Độ phức tạp:** thường là hàm mũ, ví dụ O(2ⁿ · n) cho tập con, O(n! · n) cho hoán vị.

**Bài NeetCode:** Subsets, Combination Sum, Permutations, Subsets II, Combination Sum II, Word Search, Palindrome Partitioning, Letter Combinations of a Phone Number, N-Queens.

## 11. Heap / Top K

**Ý tưởng:** muốn lấy **k phần tử lớn nhất**, giữ một **min-heap** kích thước k. Mỗi khi heap vượt quá k thì pop phần tử nhỏ nhất. Cuối cùng heap còn đúng k phần tử lớn nhất, và `top()` là phần tử lớn thứ k.

**Dấu hiệu:** "k phần tử lớn nhất / nhỏ nhất / gần nhất / xuất hiện nhiều nhất", "liên tục lấy phần tử lớn nhất", "trung vị của dòng dữ liệu".

```cpp
// Top K Frequent Elements
unordered_map<int, int> freq;
for (int x : nums) freq[x]++;

// min-heap theo tần suất: pair (tần suất, giá trị)
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
for (auto& [val, f] : freq) {
    pq.push({f, val});
    if (pq.size() > k) pq.pop();         // bỏ phần tử ít xuất hiện nhất
}
vector<int> res;
while (!pq.empty()) { res.push_back(pq.top().second); pq.pop(); }
return res;
```

**Độ phức tạp:** O(n log k).

**Bài NeetCode:** Kth Largest Element in a Stream, Last Stone Weight, K Closest Points to Origin, Kth Largest Element in an Array, Task Scheduler, Find Median from Data Stream, Merge k Sorted Lists.

## 12. Graph DFS / BFS trên lưới

**Ý tưởng:** coi mỗi ô của lưới là một đỉnh, nối với 4 ô kề. Từ một ô, loang (DFS hoặc BFS) sang các ô kề hợp lệ và đánh dấu đã thăm để không đi lại.

**Dấu hiệu:** lưới 2D `grid[i][j]`, "đếm số đảo / vùng", "diện tích lớn nhất", "lan truyền từ nhiều nguồn" (dùng BFS nhiều nguồn).

```cpp
// Number of Islands – DFS
int dr[4] = {-1, 1, 0, 0}, dc[4] = {0, 0, -1, 1};   // 4 hướng

void dfs(vector<vector<char>>& g, int r, int c) {
    if (r < 0 || c < 0 || r >= g.size() || c >= g[0].size() || g[r][c] != '1') return;
    g[r][c] = '0';                                   // đánh dấu đã thăm
    for (int d = 0; d < 4; d++) dfs(g, r + dr[d], c + dc[d]);
}

int numIslands(vector<vector<char>>& grid) {
    int cnt = 0;
    for (int i = 0; i < grid.size(); i++)
        for (int j = 0; j < grid[0].size(); j++)
            if (grid[i][j] == '1') { dfs(grid, i, j); cnt++; }
    return cnt;
}
```

```cpp
// Rotting Oranges – BFS nhiều nguồn
queue<pair<int,int>> q;
int fresh = 0, m = grid.size(), n = grid[0].size();
for (int i = 0; i < m; i++)
    for (int j = 0; j < n; j++) {
        if (grid[i][j] == 2) q.push({i, j});      // mọi quả thối là nguồn
        else if (grid[i][j] == 1) fresh++;
    }
int minutes = 0;
int dr[4] = {-1, 1, 0, 0}, dc[4] = {0, 0, -1, 1};
while (!q.empty() && fresh > 0) {
    int sz = q.size();
    for (int k = 0; k < sz; k++) {
        auto [r, c] = q.front(); q.pop();
        for (int d = 0; d < 4; d++) {
            int nr = r + dr[d], nc = c + dc[d];
            if (nr < 0 || nc < 0 || nr >= m || nc >= n || grid[nr][nc] != 1) continue;
            grid[nr][nc] = 2; fresh--;
            q.push({nr, nc});
        }
    }
    minutes++;
}
return fresh == 0 ? minutes : -1;
```

**Độ phức tạp:** O(m · n).

**Bài NeetCode:** Number of Islands, Max Area of Island, Clone Graph, Walls and Gates, Rotting Oranges, Pacific Atlantic Water Flow, Surrounded Regions.

## 13. Topological Sort (sắp xếp tô-pô)

**Ý tưởng:** sắp các đỉnh của đồ thị có hướng sao cho mọi cạnh `u → v` thì `u` đứng trước `v`. Thuật toán **Kahn**: đếm bậc vào (`indeg`), đưa các đỉnh có bậc vào bằng 0 vào queue, lấy ra và "xóa" cạnh của nó. Nếu cuối cùng không lấy ra đủ n đỉnh thì đồ thị **có chu trình**.

**Dấu hiệu:** "môn học tiên quyết", "thứ tự thực hiện công việc", "có làm xong hết được không", quan hệ phụ thuộc.

```cpp
// Course Schedule: prerequisites[i] = {a, b} nghĩa là học b trước a
vector<vector<int>> adj(n);
vector<int> indeg(n, 0);
for (auto& p : prerequisites) {
    adj[p[1]].push_back(p[0]);
    indeg[p[0]]++;
}
queue<int> q;
for (int i = 0; i < n; i++) if (indeg[i] == 0) q.push(i);
int done = 0;
while (!q.empty()) {
    int u = q.front(); q.pop();
    done++;
    for (int v : adj[u])
        if (--indeg[v] == 0) q.push(v);
}
return done == n;          // false nếu có chu trình
```

**Độ phức tạp:** O(V + E).

**Bài NeetCode:** Course Schedule, Course Schedule II, Alien Dictionary.

## 14. Union-Find (DSU)

**Ý tưởng:** quản lý các **nhóm rời nhau**. Mỗi phần tử trỏ tới "cha"; gốc là đại diện của nhóm. Hai thao tác: `find(x)` tìm đại diện, `unite(a, b)` gộp hai nhóm. Có **nén đường đi** thì gần như O(1) mỗi thao tác.

**Dấu hiệu:** "đếm số thành phần liên thông", "hai đỉnh có cùng nhóm không", "cạnh nào thừa tạo ra chu trình".

```cpp
vector<int> parent;

int find(int x) {
    if (parent[x] != x) parent[x] = find(parent[x]);    // nén đường đi
    return parent[x];
}

bool unite(int a, int b) {
    int ra = find(a), rb = find(b);
    if (ra == rb) return false;          // đã cùng nhóm -> cạnh này tạo chu trình
    parent[ra] = rb;
    return true;
}

// Khởi tạo: mỗi phần tử là một nhóm riêng
// parent.resize(n); iota(parent.begin(), parent.end(), 0);
```

**Độ phức tạp:** gần như O(1) mỗi thao tác.

**Bài NeetCode:** Redundant Connection, Number of Connected Components in an Undirected Graph, Graph Valid Tree.

## 15. Dijkstra (đường đi ngắn nhất có trọng số)

**Ý tưởng:** luôn mở rộng đỉnh có **khoảng cách nhỏ nhất** hiện tại (lấy từ min-heap), rồi cập nhật khoảng cách cho các đỉnh kề. Chỉ đúng khi trọng số **không âm**.

**Dấu hiệu:** đồ thị có **trọng số**, "thời gian / chi phí nhỏ nhất từ một đỉnh tới các đỉnh khác".

```cpp
// Network Delay Time: times[i] = {u, v, w}, đỉnh đánh số 1..n
vector<vector<pair<int,int>>> adj(n + 1);
for (auto& t : times) adj[t[0]].push_back({t[1], t[2]});

vector<int> dist(n + 1, INT_MAX);
priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;  // (dist, đỉnh)
dist[k] = 0;
pq.push({0, k});
while (!pq.empty()) {
    auto [d, u] = pq.top(); pq.pop();
    if (d > dist[u]) continue;                  // bản ghi cũ, bỏ qua
    for (auto [v, w] : adj[u])
        if (dist[u] + w < dist[v]) {
            dist[v] = dist[u] + w;
            pq.push({dist[v], v});
        }
}
int ans = *max_element(dist.begin() + 1, dist.end());
return ans == INT_MAX ? -1 : ans;
```

**Độ phức tạp:** O((V + E) log V).

**Bài NeetCode:** Network Delay Time, Swim in Rising Water, Cheapest Flights Within K Stops (biến thể Bellman-Ford), Min Cost to Connect All Points (cây khung nhỏ nhất – Prim).

## 16. Dynamic Programming (quy hoạch động)

**Ý tưởng:** bài toán lớn được ghép từ các **bài toán con lặp lại**. Lưu kết quả bài toán con để không tính lại. Bốn bước:

1. **Trạng thái:** `dp[i]` (hoặc `dp[i][j]`) nghĩa là gì?
2. **Công thức chuyển:** `dp[i]` tính từ các trạng thái nhỏ hơn như thế nào?
3. **Base case:** giá trị khởi đầu.
4. **Đáp án:** nằm ở ô nào?

**Dấu hiệu:** "số cách", "giá trị lớn nhất / nhỏ nhất", "có làm được không", mà mỗi bước có **lựa chọn** và các lựa chọn ảnh hưởng tới bước sau.

```cpp
// 1D – Climbing Stairs: dp[i] = số cách lên bậc i
vector<int> dp(n + 1);
dp[0] = 1; dp[1] = 1;
for (int i = 2; i <= n; i++) dp[i] = dp[i - 1] + dp[i - 2];
return dp[n];

// 1D – House Robber: dp[i] = tiền lớn nhất khi xét tới nhà i
// dp[i] = max(dp[i-1] (bỏ nhà i), dp[i-2] + nums[i] (cướp nhà i))
int prev2 = 0, prev1 = 0;                  // chỉ cần 2 giá trị trước -> O(1) bộ nhớ
for (int x : nums) {
    int cur = max(prev1, prev2 + x);
    prev2 = prev1; prev1 = cur;
}
return prev1;

// 1D – Coin Change: dp[a] = số đồng xu ít nhất để tạo ra số tiền a
vector<int> dp(amount + 1, INT_MAX);
dp[0] = 0;
for (int a = 1; a <= amount; a++)
    for (int c : coins)
        if (c <= a && dp[a - c] != INT_MAX)
            dp[a] = min(dp[a], dp[a - c] + 1);
return dp[amount] == INT_MAX ? -1 : dp[amount];
```

```cpp
// 2D – Longest Common Subsequence: dp[i][j] = LCS của a[0..i-1] và b[0..j-1]
int m = a.size(), n = b.size();
vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
for (int i = 1; i <= m; i++)
    for (int j = 1; j <= n; j++)
        if (a[i - 1] == b[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
        else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
return dp[m][n];
```

> Cách học DP: thử viết **đệ quy** trước, thêm **memo** (lưu kết quả) để thành top-down, rồi mới chuyển sang **vòng lặp** (bottom-up).

**Độ phức tạp:** số trạng thái × chi phí mỗi bước chuyển.

**Bài NeetCode:**
- 1D: Climbing Stairs, Min Cost Climbing Stairs, House Robber, House Robber II, Longest Palindromic Substring, Decode Ways, Coin Change, Maximum Product Subarray, Word Break, Longest Increasing Subsequence, Partition Equal Subset Sum.
- 2D: Unique Paths, Longest Common Subsequence, Best Time to Buy and Sell Stock with Cooldown, Coin Change II, Target Sum, Interleaving String, Edit Distance, Distinct Subsequences, Burst Balloons, Regular Expression Matching.

## 17. Greedy (tham lam)

**Ý tưởng:** ở mỗi bước, chọn phương án **tốt nhất ngay lúc đó** và không quay lại. Chỉ đúng khi chứng minh được lựa chọn cục bộ dẫn tới tối ưu toàn cục, nên luôn thử phản ví dụ trước khi dùng.

**Dấu hiệu:** "nhảy tới cuối được không", "số bước ít nhất", "tổng mảng con lớn nhất", thường có sort trước.

```cpp
// Maximum Subarray (Kadane): nếu tổng đang âm thì bắt đầu lại từ phần tử hiện tại
int best = nums[0], cur = 0;
for (int x : nums) {
    cur = max(x, cur + x);
    best = max(best, cur);
}
return best;

// Jump Game: theo dõi vị trí xa nhất có thể tới
int reach = 0;
for (int i = 0; i < nums.size(); i++) {
    if (i > reach) return false;            // không tới được ô i
    reach = max(reach, i + nums[i]);
}
return true;
```

**Độ phức tạp:** thường O(n) hoặc O(n log n) nếu có sort.

**Bài NeetCode:** Maximum Subarray, Jump Game, Jump Game II, Gas Station, Hand of Straights, Merge Triplets to Form Target Triplet, Partition Labels, Valid Parenthesis String.

## 18. Intervals (xử lý khoảng)

**Ý tưởng:** gần như luôn **sort theo điểm bắt đầu** trước. Sau đó duyệt một lượt: nếu khoảng mới chồng lên khoảng trước (`start ≤ end trước`) thì gộp, không thì tách.

**Dấu hiệu:** input dạng `[start, end]`: lịch họp, gộp khoảng, chèn khoảng, xóa ít khoảng nhất để không chồng nhau.

```cpp
// Merge Intervals
sort(intervals.begin(), intervals.end());
vector<vector<int>> res;
for (auto& in : intervals) {
    if (res.empty() || in[0] > res.back()[1]) res.push_back(in);   // không chồng
    else res.back()[1] = max(res.back()[1], in[1]);                 // chồng -> gộp
}
return res;
```

> Bài "xóa ít khoảng nhất" (Non-overlapping Intervals): sort theo **điểm kết thúc**, giữ lại khoảng kết thúc sớm nhất.

**Độ phức tạp:** O(n log n).

**Bài NeetCode:** Insert Interval, Merge Intervals, Non-overlapping Intervals, Meeting Rooms, Meeting Rooms II, Minimum Interval to Include Each Query.

## 19. Bit Manipulation (thao tác bit)

**Ý tưởng:** làm việc trực tiếp trên biểu diễn nhị phân của số.

| Phép toán | Ý nghĩa |
|---|---|
| `a & b` | AND: bit bằng 1 khi cả hai đều là 1 |
| `a \| b` | OR: bit bằng 1 khi một trong hai là 1 |
| `a ^ b` | XOR: bit bằng 1 khi hai bit khác nhau. **`x ^ x = 0`, `x ^ 0 = x`** |
| `~a` | Đảo tất cả các bit |
| `a << k` / `a >> k` | Dịch trái (nhân 2ᵏ) / dịch phải (chia 2ᵏ) |
| `(x >> i) & 1` | Lấy bit thứ i |
| `x & (x - 1)` | Xóa bit 1 thấp nhất |
| `__builtin_popcount(x)` | Đếm số bit 1 |

**Dấu hiệu:** "phần tử xuất hiện một lần, các phần tử khác xuất hiện hai lần", "đếm bit", "cộng hai số không dùng dấu +", "đảo bit".

```cpp
// Single Number: các cặp giống nhau XOR với nhau thành 0
int res = 0;
for (int x : nums) res ^= x;
return res;

// Counting Bits: số bit 1 của i = số bit 1 của (i >> 1) + bit cuối của i
vector<int> ans(n + 1, 0);
for (int i = 1; i <= n; i++) ans[i] = ans[i >> 1] + (i & 1);
return ans;
```

**Độ phức tạp:** thường O(n) hoặc O(1) cho mỗi số.

**Bài NeetCode:** Single Number, Number of 1 Bits, Counting Bits, Reverse Bits, Missing Number, Sum of Two Integers, Reverse Integer.

---

# PHỤ LỤC

## Bảng nhận diện nhanh: đọc đề thấy gì → nghĩ tới kỹ thuật nào

| Đề bài có… | Thử kỹ thuật |
|---|---|
| "Có trùng không", "đếm", "tìm cặp có tổng k" (mảng chưa sort) | Hashing |
| Mảng **đã sort**, tìm cặp / bộ ba | Two Pointers |
| Mảng / chuỗi con **liên tiếp** dài nhất / ngắn nhất | Sliding Window |
| Tổng đoạn con, "số mảng con có tổng = k" | Prefix Sum + Hashing |
| "Phần tử lớn hơn gần nhất", ngoặc lồng nhau | Stack / Monotonic Stack |
| Mảng đã sort, "giá trị nhỏ nhất sao cho…", giới hạn tới 10⁹ | Binary Search |
| Chu trình trong danh sách, node ở giữa | Fast & Slow Pointer |
| `TreeNode*` | Tree DFS (đệ quy) |
| "Theo tầng", đường ngắn nhất không trọng số | BFS |
| "Liệt kê **tất cả**", n ≤ 20 | Backtracking |
| "k lớn nhất / nhỏ nhất / gần nhất" | Heap |
| Lưới 2D, đếm vùng | Graph DFS / BFS |
| Quan hệ phụ thuộc, "tiên quyết" | Topological Sort |
| Gộp nhóm, thành phần liên thông | Union-Find |
| Đường đi ngắn nhất **có trọng số** | Dijkstra |
| "Số cách", "lớn nhất / nhỏ nhất" với lựa chọn ở mỗi bước | Dynamic Programming |
| Chọn tốt nhất tại chỗ mà không cần quay lại | Greedy |
| Input dạng `[start, end]` | Sort + Intervals |
| "Xuất hiện một lần / hai lần", bit | Bit Manipulation (XOR) |

## Giới hạn n → độ phức tạp cần đạt

Máy chấm chạy được khoảng 10⁸ phép tính mỗi giây. Nhìn giới hạn n để đoán thuật toán đề bài mong đợi:

| Giới hạn n | Độ phức tạp chấp nhận được | Thường là |
|---|---|---|
| n ≤ 12 | O(n!) | Hoán vị, backtracking |
| n ≤ 20 | O(2ⁿ) | Tập con, backtracking, bitmask |
| n ≤ 500 | O(n³) | DP 3 chiều |
| n ≤ 5 000 | O(n²) | DP 2 chiều, 2 vòng lặp |
| n ≤ 10⁵ – 10⁶ | O(n log n) hoặc O(n) | Sort, heap, binary search, two pointers, sliding window, hashing |
| n ≤ 10⁹ trở lên | O(log n) hoặc O(1) | Binary search, toán học |
