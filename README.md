# Cấu trúc dữ liệu C++ cho LeetCode / NeetCode

> Khái niệm · Tác dụng · Các hàm thường dùng và độ phức tạp

Xem thêm: [Thuật toán C++ – hàm STL và kỹ thuật giải bài](ALGORITHMS.md)

## Cách đọc tài liệu

- Mỗi cấu trúc có 3 phần: **Khái niệm** (nó là gì), **Tác dụng** (dùng khi nào, kèm bài NeetCode ví dụ) và **Các hàm** (cú pháp, tác dụng, độ phức tạp), cuối cùng là một đoạn code mẫu.
- Trên LeetCode có thể dùng `#include <bits/stdc++.h>` và `using namespace std;` cho đủ mọi thư viện.
- Độ phức tạp: O(1) là tức thì, O(log n) là rất nhanh, O(n) là tỉ lệ với số phần tử. Với n tới 10^5, cố gắng giữ tổng thuật toán ở O(n) hoặc O(n log n).

## Mục lục

1. vector – mảng động
2. string – chuỗi ký tự
3. pair – cặp giá trị
4. unordered_set – tập hợp (không thứ tự)
5. set / multiset – tập hợp có thứ tự
6. unordered_map – từ điển (không thứ tự)
7. map – từ điển có thứ tự
8. stack – ngăn xếp (LIFO)
9. queue – hàng đợi (FIFO)
10. deque – hàng đợi hai đầu
11. priority_queue – hàng đợi ưu tiên (heap)
12. ListNode – danh sách liên kết
13. TreeNode – cây nhị phân
14. Các hàm thư viện `<algorithm>` hay dùng kèm
15. Tổng kết độ phức tạp

## Bảng chọn nhanh: gặp yêu cầu nào thì dùng cấu trúc nào

| Bài toán cần… | Dùng | Bài ví dụ |
|---|---|---|
| Đã gặp phần tử x chưa? | `unordered_set` | Contains Duplicate |
| Đếm số lần xuất hiện | `unordered_map` | Valid Anagram |
| Tra giá trị → chỉ số | `unordered_map` | Two Sum |
| Dữ liệu cần luôn có thứ tự | `set / map` | Time Based Key-Value Store |
| Khớp cặp, "cái gần nhất trước đó" | `stack` | Valid Parentheses, Daily Temperatures |
| Duyệt theo tầng, đường ngắn nhất (không trọng số) | `queue (BFS)` | Rotting Oranges |
| Max/min trong cửa sổ trượt | `deque` | Sliding Window Maximum |
| Lấy liên tục phần tử lớn/nhỏ nhất, Top K | `priority_queue` | Kth Largest, Top K Frequent |
| Mảng thông thường, mảng dp | `vector` | Climbing Stairs |

---

## 1. vector – mảng động

`#include <vector>`

### Khái niệm

Mảng có thể **tự co giãn kích thước**. Các phần tử nằm liền nhau trong bộ nhớ nên truy cập theo chỉ số rất nhanh, giống mảng thường `a[i]`.

### Tác dụng

- Cấu trúc dùng nhiều nhất: hầu hết input của LeetCode là `vector<int>& nums`.
- Thay cho mảng tĩnh `a[100001]`: không cần đoán trước kích thước.
- Làm mảng phụ: mảng đếm, mảng dp, danh sách kề của đồ thị (`vector<vector<int>>`).

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `vector<int> v;` | Tạo vector rỗng | O(1) |
| `vector<int> v(n, 0);` | Tạo vector n phần tử, tất cả bằng 0 | O(n) |
| `v[i]` | Truy cập phần tử thứ i (không kiểm tra vượt giới hạn) | O(1) |
| `v.push_back(x)` | Thêm x vào **cuối** | O(1)* |
| `v.pop_back()` | Xóa phần tử **cuối** | O(1) |
| `v.back() / v.front()` | Lấy phần tử cuối / đầu | O(1) |
| `v.size()` | Số phần tử | O(1) |
| `v.empty()` | `true` nếu rỗng | O(1) |
| `v.clear()` | Xóa hết phần tử | O(n) |
| `v.insert(v.begin()+i, x)` | Chèn x vào vị trí i (phải dời các phần tử sau) | O(n) |
| `v.erase(v.begin()+i)` | Xóa phần tử ở vị trí i | O(n) |
| `v.resize(k)` | Đổi kích thước thành k | O(k) |
| `v.begin() / v.end()` | Iterator đầu / sau phần tử cuối, dùng cho sort, reverse… | O(1) |

> *O(1) trung bình (amortized): thỉnh thoảng vector phải cấp phát lại bộ nhớ, nhưng tính trung bình vẫn là O(1).

### Ví dụ

```cpp
vector<int> v = {3, 1, 2};
v.push_back(5);                 // {3, 1, 2, 5}
sort(v.begin(), v.end());       // {1, 2, 3, 5}
for (int x : v) cout << x;      // duyệt từng phần tử
vector<vector<int>> grid(m, vector<int>(n, 0));  // ma trận m x n
```

---

## 2. string – chuỗi ký tự

`#include <string>`

### Khái niệm

Một dãy ký tự (`char`), hoạt động gần giống `vector<char>` nhưng có thêm nhiều hàm xử lý chuỗi.

### Tác dụng

- Các bài về chuỗi: Valid Anagram, Valid Palindrome, Longest Substring…
- Làm key cho hashmap, ví dụ bài Group Anagrams dùng chuỗi đã sort làm key.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `s[i]` | Ký tự thứ i | O(1) |
| `s.size() / s.length()` | Độ dài chuỗi | O(1) |
| `s.push_back(c) / s += c` | Thêm ký tự hoặc chuỗi vào cuối | O(1)* |
| `s.pop_back()` | Xóa ký tự cuối | O(1) |
| `s.substr(i, len)` | Lấy chuỗi con bắt đầu từ i, dài len | O(len) |
| `s.find(t)` | Vị trí xuất hiện đầu tiên của t, không có thì trả `string::npos` | O(n·m) |
| `to_string(x)` | Đổi số thành chuỗi | O(số chữ số) |
| `stoi(s) / stoll(s)` | Đổi chuỗi thành int / long long | O(n) |
| `isalnum(c) / isdigit(c) / isalpha(c)` | Kiểm tra c là chữ hoặc số / số / chữ | O(1) |
| `tolower(c) / toupper(c)` | Đổi c thành chữ thường / hoa | O(1) |
| `c - 'a'` | Đổi chữ cái thường thành số 0–25, hay dùng làm chỉ số mảng đếm | O(1) |

### Ví dụ

```cpp
string s = "Hello";
s += " World";                     // "Hello World"
string t = s.substr(0, 5);         // "Hello"
int cnt[26] = {0};
for (char c : "abca") cnt[c - 'a']++;   // đếm tần suất chữ cái
```

---

## 3. pair – cặp giá trị

`#include <utility>`

### Khái niệm

Gói **2 giá trị** (có thể khác kiểu) thành một biến. Truy cập bằng `.first` và `.second`.

### Tác dụng

- Lưu (giá trị, chỉ số), (tọa độ x, y), (khoảng cách, đỉnh)…
- Bỏ vào `priority_queue` hoặc `sort`: pair tự so sánh theo `first` trước, nếu bằng thì so `second`.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `pair<int,int> p = {a, b};` | Tạo cặp (a, b) | O(1) |
| `make_pair(a, b)` | Cách khác để tạo cặp | O(1) |
| `p.first / p.second` | Phần tử thứ nhất / thứ hai | O(1) |
| `auto [x, y] = p;` | Tách cặp thành 2 biến (C++17) | O(1) |

### Ví dụ

```cpp
vector<pair<int,int>> v = {{3, 0}, {1, 1}, {2, 2}};  // (giá trị, chỉ số)
sort(v.begin(), v.end());      // sắp theo giá trị: {1,1}, {2,2}, {3,0}
for (auto [val, idx] : v) cout << val << " " << idx;
```

---

## 4. unordered_set – tập hợp (không thứ tự)

`#include <unordered_set>`

### Khái niệm

Tập hợp các giá trị **không trùng nhau**, được lưu bằng bảng băm (hash table). Kiểm tra "có phần tử x không" rất nhanh, nhưng các phần tử **không có thứ tự**.

### Tác dụng

- Trả lời câu hỏi "đã gặp số này chưa?": Contains Duplicate, Longest Consecutive Sequence.
- Loại bỏ phần tử trùng.
- Đánh dấu đỉnh đã thăm trong đồ thị, khi đỉnh không đánh số liên tiếp.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `unordered_set<int> s;` | Tạo tập rỗng | O(1) |
| `s.insert(x)` | Thêm x; nếu x đã có thì không làm gì | O(1) tb |
| `s.count(x)` | 1 nếu x có trong tập, 0 nếu không | O(1) tb |
| `s.find(x) != s.end()` | Cách khác để kiểm tra x có trong tập | O(1) tb |
| `s.erase(x)` | Xóa x | O(1) tb |
| `s.size() / s.empty()` | Số phần tử / kiểm tra rỗng | O(1) |
| `unordered_set<int> s(v.begin(), v.end());` | Tạo tập từ vector, tự loại phần tử trùng | O(n) |

> "tb" = trung bình. Trường hợp xấu nhất (nhiều phần tử va chạm hash) có thể lên O(n), nhưng trên LeetCode gần như không gặp.

### Ví dụ

```cpp
unordered_set<int> seen;
for (int x : nums) {
    if (seen.count(x)) return true;   // đã gặp -> trùng
    seen.insert(x);
}
```

---

## 5. set / multiset – tập hợp có thứ tự

`#include <set>`

### Khái niệm

Giống `unordered_set` nhưng các phần tử **luôn được sắp tăng dần** (cài đặt bằng cây nhị phân cân bằng). Đổi lại, mỗi thao tác chậm hơn: O(log n). `multiset` thì **cho phép phần tử trùng**.

### Tác dụng

- Cần vừa tra cứu nhanh vừa giữ dữ liệu có thứ tự.
- Tìm phần tử nhỏ nhất/lớn nhất liên tục trong khi vẫn thêm và xóa được.
- Tìm phần tử "gần nhất" ≥ x bằng `lower_bound`.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `s.insert(x)` | Thêm x, tập tự giữ thứ tự | O(log n) |
| `s.count(x)` | Số lần x xuất hiện (set chỉ có 0 hoặc 1) | O(log n) |
| `s.erase(x)` | Xóa x (multiset: xóa **tất cả** các x) | O(log n) |
| `s.erase(s.find(x))` | multiset: chỉ xóa **một** phần tử x | O(log n) |
| `*s.begin()` | Phần tử **nhỏ nhất** | O(1) |
| `*s.rbegin()` | Phần tử **lớn nhất** | O(1) |
| `s.lower_bound(x)` | Iterator tới phần tử đầu tiên **≥ x** | O(log n) |
| `s.upper_bound(x)` | Iterator tới phần tử đầu tiên **> x** | O(log n) |

### Ví dụ

```cpp
set<int> s = {5, 1, 3};          // tự sắp: {1, 3, 5}
cout << *s.begin();              // 1 (nhỏ nhất)
auto it = s.lower_bound(2);      // trỏ tới 3
if (it != s.end()) cout << *it;
```

---

## 6. unordered_map – từ điển (không thứ tự)

`#include <unordered_map>`

### Khái niệm

Lưu các cặp **key → value**, mỗi key chỉ xuất hiện một lần. Giống từ điển: tra key thì ra value. Cài đặt bằng bảng băm nên tra cứu trung bình O(1).

### Tác dụng

- **Đếm tần suất**: `freq[x]++` (Valid Anagram, Top K Frequent Elements).
- Lưu "giá trị → chỉ số" để tra nhanh, như bài Two Sum.
- Nhóm dữ liệu theo key, như bài Group Anagrams.
- Là cấu trúc **quan trọng nhất** cho phần Arrays & Hashing.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `unordered_map<int,int> m;` | Tạo map rỗng (key int, value int) | O(1) |
| `m[key] = value` | Gán value cho key; nếu key chưa có thì tạo mới | O(1) tb |
| `m[key]++` | Tăng value. Nếu key chưa có, value được **tự tạo bằng 0** rồi tăng lên | O(1) tb |
| `m.count(key)` | 1 nếu key có trong map, 0 nếu không | O(1) tb |
| `m.find(key)` | Iterator tới cặp có key đó, hoặc `m.end()` nếu không có | O(1) tb |
| `m.erase(key)` | Xóa cặp có key đó | O(1) tb |
| `m.size() / m.empty()` | Số cặp / kiểm tra rỗng | O(1) |
| `for (auto& [k, v] : m)` | Duyệt từng cặp key–value (thứ tự ngẫu nhiên) | O(n) |

> Bẫy hay gặp: chỉ cần **đọc** `m[key]` khi key chưa tồn tại là map sẽ tự thêm key đó với value 0. Muốn chỉ kiểm tra thì dùng `count` hoặc `find`.

### Ví dụ

```cpp
// Two Sum: tìm 2 chỉ số có tổng = target
unordered_map<int,int> pos;          // giá trị -> chỉ số
for (int i = 0; i < nums.size(); i++) {
    int need = target - nums[i];
    if (pos.count(need)) return {pos[need], i};
    pos[nums[i]] = i;
}
```

---

## 7. map – từ điển có thứ tự

`#include <map>`

### Khái niệm

Giống `unordered_map` nhưng các key **luôn được sắp tăng dần**. Mỗi thao tác O(log n).

### Tác dụng

- Cần duyệt các key theo thứ tự.
- Cần tìm key gần nhất ≥ x, ví dụ bài Time Based Key-Value Store.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `m[key] = value / m[key]++` | Như unordered_map | O(log n) |
| `m.count(key) / m.find(key)` | Kiểm tra / tìm key | O(log n) |
| `m.erase(key)` | Xóa key | O(log n) |
| `m.begin()->first` | Key **nhỏ nhất** | O(1) |
| `m.rbegin()->first` | Key **lớn nhất** | O(1) |
| `m.lower_bound(x)` | Iterator tới key đầu tiên ≥ x | O(log n) |
| `m.upper_bound(x)` | Iterator tới key đầu tiên > x | O(log n) |

### Ví dụ

```cpp
map<string,int> score = {{"bob", 7}, {"amy", 9}};
for (auto& [name, s] : score)    // duyệt theo thứ tự key: amy, bob
    cout << name << " " << s << "\n";
```

---

## 8. stack – ngăn xếp (LIFO)

`#include <stack>`

### Khái niệm

**Vào sau – ra trước** (Last In First Out), giống chồng đĩa: chỉ thêm và lấy được ở **đỉnh**.

### Tác dụng

- Kiểm tra dấu ngoặc: Valid Parentheses.
- **Monotonic stack**, tìm phần tử lớn hơn tiếp theo: Daily Temperatures, Largest Rectangle in Histogram.
- Tính biểu thức: Evaluate Reverse Polish Notation.
- Thay đệ quy bằng vòng lặp khi duyệt DFS.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `stack<int> st;` | Tạo stack rỗng | O(1) |
| `st.push(x)` | Đặt x lên đỉnh | O(1) |
| `st.top()` | Xem phần tử ở đỉnh (không xóa) | O(1) |
| `st.pop()` | Xóa phần tử ở đỉnh (**không trả về giá trị**) | O(1) |
| `st.empty() / st.size()` | Kiểm tra rỗng / số phần tử | O(1) |

> Luôn kiểm tra `!st.empty()` trước khi gọi `top()` hoặc `pop()`, nếu không sẽ lỗi runtime.

### Ví dụ

```cpp
// Valid Parentheses
stack<char> st;
for (char c : s) {
    if (c == '(') st.push(')');
    else if (c == '[') st.push(']');
    else if (c == '{') st.push('}');
    else {
        if (st.empty() || st.top() != c) return false;
        st.pop();
    }
}
return st.empty();
```

---

## 9. queue – hàng đợi (FIFO)

`#include <queue>`

### Khái niệm

**Vào trước – ra trước** (First In First Out), giống xếp hàng: thêm vào ở **cuối**, lấy ra ở **đầu**.

### Tác dụng

- **BFS** (duyệt theo chiều rộng): Rotting Oranges, Number of Islands, đường đi ngắn nhất trên lưới.
- Duyệt cây theo từng tầng: Binary Tree Level Order Traversal.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `queue<int> q;` | Tạo hàng đợi rỗng | O(1) |
| `q.push(x)` | Thêm x vào **cuối** | O(1) |
| `q.front()` | Xem phần tử **đầu** | O(1) |
| `q.back()` | Xem phần tử **cuối** | O(1) |
| `q.pop()` | Xóa phần tử **đầu** (không trả về giá trị) | O(1) |
| `q.empty() / q.size()` | Kiểm tra rỗng / số phần tử | O(1) |

### Ví dụ

```cpp
// BFS khung chuẩn
queue<int> q;
q.push(start); visited[start] = true;
while (!q.empty()) {
    int u = q.front(); q.pop();
    for (int v : adj[u])
        if (!visited[v]) { visited[v] = true; q.push(v); }
}
```

---

## 10. deque – hàng đợi hai đầu

`#include <deque>`

### Khái niệm

Thêm và xóa được ở **cả hai đầu** trong O(1), lại truy cập được theo chỉ số `dq[i]`.

### Tác dụng

- **Monotonic deque**: Sliding Window Maximum.
- Khi cần vừa dùng như stack vừa dùng như queue.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `dq.push_back(x) / dq.push_front(x)` | Thêm vào cuối / đầu | O(1) |
| `dq.pop_back() / dq.pop_front()` | Xóa ở cuối / đầu | O(1) |
| `dq.back() / dq.front()` | Xem phần tử cuối / đầu | O(1) |
| `dq[i]` | Truy cập theo chỉ số | O(1) |
| `dq.empty() / dq.size()` | Kiểm tra rỗng / số phần tử | O(1) |

### Ví dụ

```cpp
deque<int> dq;          // lưu chỉ số, giá trị giảm dần
for (int i = 0; i < n; i++) {
    while (!dq.empty() && nums[dq.back()] <= nums[i]) dq.pop_back();
    dq.push_back(i);
    if (dq.front() <= i - k) dq.pop_front();       // ra khỏi cửa sổ
    if (i >= k - 1) res.push_back(nums[dq.front()]);
}
```

---

## 11. priority_queue – hàng đợi ưu tiên (heap)

`#include <queue>`

### Khái niệm

Luôn đưa phần tử **lớn nhất** lên đầu (max-heap mặc định). Thêm và xóa O(log n), xem phần tử lớn nhất O(1).

### Tác dụng

- Bài "Top K": Kth Largest Element, Top K Frequent Elements, K Closest Points.
- Thuật toán **Dijkstra** (đường đi ngắn nhất).
- Merge k Sorted Lists, Find Median from Data Stream.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `priority_queue<int> pq;` | **Max-heap**: `top()` là phần tử lớn nhất | O(1) |
| `priority_queue<int, vector<int>, greater<int>> pq;` | **Min-heap**: `top()` là phần tử nhỏ nhất | O(1) |
| `pq.push(x)` | Thêm x | O(log n) |
| `pq.top()` | Xem phần tử ưu tiên nhất | O(1) |
| `pq.pop()` | Xóa phần tử ưu tiên nhất | O(log n) |
| `pq.empty() / pq.size()` | Kiểm tra rỗng / số phần tử | O(1) |

> Mẹo: muốn có min-heap nhanh với số nguyên thì `push(-x)` vào max-heap, khi lấy ra đổi dấu lại.

### Ví dụ

```cpp
// Kth Largest: giữ min-heap kích thước k
priority_queue<int, vector<int>, greater<int>> pq;
for (int x : nums) {
    pq.push(x);
    if (pq.size() > k) pq.pop();   // bỏ phần tử nhỏ nhất
}
return pq.top();                   // phần tử lớn thứ k
```

---

## 12. ListNode – danh sách liên kết

`LeetCode khai báo sẵn`

### Khái niệm

Mỗi **node** chứa một giá trị và một **con trỏ** tới node tiếp theo. Node cuối trỏ tới `nullptr`. Khác vector, các node không nằm liền nhau trong bộ nhớ nên **không truy cập theo chỉ số** được, chỉ đi lần lượt từ đầu.

### Tác dụng

- Toàn bộ phần Linked List: Reverse Linked List, Merge Two Sorted Lists, Linked List Cycle…
- Chèn và xóa node ở giữa trong O(1) khi đã có con trỏ tới vị trí đó.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `struct ListNode { int val; ListNode* next; };` | Định nghĩa node (LeetCode cho sẵn ở comment đầu bài) | — |
| `new ListNode(x)` | Tạo node mới có giá trị x | O(1) |
| `node->val` | Giá trị của node | O(1) |
| `node->next` | Node kế tiếp (`nullptr` nếu là node cuối) | O(1) |
| `ListNode dummy(0);` | **Node giả** đứng trước head, giúp xử lý trường hợp đầu danh sách dễ hơn | O(1) |

### Ví dụ

```cpp
// Reverse Linked List
ListNode* prev = nullptr;
ListNode* cur = head;
while (cur) {
    ListNode* nxt = cur->next;   // giữ node kế tiếp
    cur->next = prev;            // đảo chiều mũi tên
    prev = cur;
    cur = nxt;
}
return prev;                     // head mới
```

---

## 13. TreeNode – cây nhị phân

`LeetCode khai báo sẵn`

### Khái niệm

Mỗi node có một giá trị và **2 con trỏ** `left`, `right` tới con trái và con phải. Node gốc gọi là **root**; node không có con gọi là **lá**.

### Tác dụng

- Toàn bộ phần Trees: Invert Binary Tree, Maximum Depth, Same Tree, Validate BST…
- Hầu như mọi bài cây đều giải bằng **đệ quy** (DFS) hoặc **queue** (BFS theo tầng).
- Cây nhị phân tìm kiếm (BST): con trái < node < con phải.

### Các hàm thường dùng

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `struct TreeNode { int val; TreeNode *left, *right; };` | Định nghĩa node (LeetCode cho sẵn) | — |
| `root->val` | Giá trị node | O(1) |
| `root->left / root->right` | Con trái / con phải (`nullptr` nếu không có) | O(1) |
| `if (!root) return ...;` | **Base case** của đệ quy: gặp node rỗng thì dừng | — |

### Ví dụ

```cpp
// Maximum Depth of Binary Tree
int maxDepth(TreeNode* root) {
    if (!root) return 0;                          // base case
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}
```

---

## 14. Các hàm thư viện `<algorithm>` hay dùng kèm

Không phải cấu trúc dữ liệu, nhưng gần như bài nào cũng dùng tới khi thao tác với vector và string.

| Hàm / cú pháp | Tác dụng | Độ phức tạp |
|---|---|---|
| `sort(v.begin(), v.end())` | Sắp tăng dần | O(n log n) |
| `sort(v.begin(), v.end(), greater<int>())` | Sắp giảm dần | O(n log n) |
| `sort(v.begin(), v.end(), [](auto& a, auto& b){ return a[1] < b[1]; })` | Sắp theo tiêu chí tự định nghĩa (lambda) | O(n log n) |
| `reverse(v.begin(), v.end())` | Đảo ngược mảng hoặc chuỗi | O(n) |
| `max(a, b) / min(a, b)` | Lớn nhất / nhỏ nhất của 2 số | O(1) |
| `*max_element(v.begin(), v.end())` | Phần tử lớn nhất của mảng | O(n) |
| `*min_element(v.begin(), v.end())` | Phần tử nhỏ nhất của mảng | O(n) |
| `accumulate(v.begin(), v.end(), 0LL)` | Tổng các phần tử (cần <numeric>) | O(n) |
| `lower_bound(v.begin(), v.end(), x)` | Mảng **đã sort**: iterator tới phần tử đầu tiên ≥ x | O(log n) |
| `upper_bound(v.begin(), v.end(), x)` | Mảng **đã sort**: iterator tới phần tử đầu tiên > x | O(log n) |
| `swap(a, b)` | Hoán đổi 2 biến | O(1) |
| `INT_MAX / INT_MIN` | Giá trị lớn nhất / nhỏ nhất của int, dùng làm giá trị khởi tạo | — |

## 15. Tổng kết độ phức tạp

| Cấu trúc | Thêm | Xóa | Tìm / truy cập | Có thứ tự? |
|---|---|---|---|---|
| `vector` | O(1) cuối | O(1) cuối, O(n) giữa | O(1) theo chỉ số | Theo vị trí |
| `unordered_set / map` | O(1) tb | O(1) tb | O(1) tb | Không |
| `set / map` | O(log n) | O(log n) | O(log n) | Có (tăng dần) |
| `stack` | O(1) đỉnh | O(1) đỉnh | Chỉ xem đỉnh | LIFO |
| `queue` | O(1) cuối | O(1) đầu | Chỉ xem đầu/cuối | FIFO |
| `deque` | O(1) 2 đầu | O(1) 2 đầu | O(1) theo chỉ số | Theo vị trí |
| `priority_queue` | O(log n) | O(log n) top | O(1) top | Chỉ biết max/min |
| `ListNode` | O(1)* | O(1)* | O(n) | Theo liên kết |

\* Khi đã có sẵn con trỏ tới vị trí cần chèn hoặc xóa.
