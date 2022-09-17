### 题目使用 `EOF` 表示输入结束时：
```c
while (scanf("%d", &n) != EOF)
while (~scanf("%d", &n))  // 当输入为 EOF 时，-1 求反为 0
while (cin >> n)
while (cin >> a >> b)

```

```cpp
int string2int (string s) {
    stringstream ss(s);
    int num;
    ss >> num;
    return num;
}
```

```c
int int2c_str (int n) {
    char str[10];
    // sprintf 写入到字符串
    sprintf(str, "%d", n);
}
```

## 字符串输入（C++ 超时则考虑用 C 语言输入）

输入是一整行的字符串的：
```c
// C language
char buf[20];
gets(buf);
```
C++ 写法：

如果用 string buf; 来保存：
```cpp
getline(cin, buf);
```
如果用 char buf[255]; 来保存：
```cpp
cin.getline(buf, 255);
```
在多个字符串之间用一个或多个空格分隔：
```c
scanf("%s %s", str1, str2);
```
字符串之间用回车符作分隔，使用 `gets` 函数：
```c
gets(str1);
gets(str2);
```
通常情况下，接受短字符用 `scanf` 函数，接受长字符用 `gets` 函数。

而 `getchar` 函数每次只接受一个字符，一般使用 `c = getchar()` 。

## 链表输入
### 有序链表合并（ACM 模式）
```cpp
#include<iostream>
#include<vector>
using namespace std;
 
struct Listnode {
	int val;
	Listnode* next;
	Listnode(int x) :val(x), next(NULL) {}
};
 
Listnode* createList(vector<int>& nums) {
	if (nums.size() == 0) {
		return NULL;
	}
	Listnode* head = new Listnode(nums[0]);
	Listnode* cur = head;
 
	for (int i = 1; i < nums.size(); i++) {
		cur->next = new Listnode(nums[i]);
		cur = cur->next;
	}
	return head;
}
 
Listnode* merge(Listnode* l1, Listnode* l2) {
	Listnode* dummy = new Listnode(0);
	Listnode* cur = dummy;
	while (l1 != NULL && l2 != NULL) {
		if (l1->val < l2->val) {
			cur->next = l1;
			l1 = l1->next;
		}
		else {
			cur->next = l2;
			l2 = l2->next;
		}
		cur = cur->next;
	}
	cur->next = l1 ? l1 : l2;
	return dummy->next;
}
 
int main() {
	int a;
	vector<int> nums;
	while (cin >> a) {
		nums.push_back(a);
		if (cin.get() == '\n')
			break;
	}
	Listnode* head1 = createList(nums);
	nums.clear();
	while (cin >> a) {
		nums.push_back(a);
		if (cin.get() == '\n')
			break;
	}
	Listnode* head2 = createList(nums);
	Listnode* ans = merge(head1, head2);
	while (ans) {
		cout << ans->val << " ";
		ans = ans->next;
	}
	return 0;
}
```