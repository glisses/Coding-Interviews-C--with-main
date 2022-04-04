# *Coding Interviews* C++

<div align="center"><p>
    <a href="https://github.com/glisses/Coding-Interviews-Cplusplus-with-main/pulse">
      <img src="https://img.shields.io/github/last-commit/glisses/Coding-Interviews-Cplusplus-with-main?color=%4dc71f&label=Last%20Commit&logo=github&style=flat-square"/>
    </a>
    <a href="https://github.com/glisses/Coding-Interviews-Cplusplus-with-main/blob/main/LICENSE">
      <img src="https://img.shields.io/github/license/glisses/Coding-Interviews-Cplusplus-with-main?label=License&logo=GNU&style=flat-square"/>
    </a>
</p>
</div>

本文严格按照[LeetCode上《剑指offer》学习计划](https://leetcode-cn.com/study-plan/lcof/?progress=wp5ibz5)的顺序撰写。代码皆为C++。

**动机**：LeetCode题解没有主程序，对我在本地测试造成了麻烦。想提供一份包含测试的完整程序，供大家使用。

对于《剑指offer》上的每道题，本文包含：

- **该题的LeetCode链接**
- **解题思路（简单题可能略过）**
- **完整代码（输入在main函数中）**
- **样例输出**
- **执行用时、内存消耗在LeetCode评测的排名**

代码的`.cpp`和`exe`文件见[./code](./code)中。

​                 

截至目前，本文中代码在所有CPP提交中，**执行用时平均击败96.82%的用户，内存消耗平均击败81.61%的用户**。

你也可以在[我的博客](https://blog.fishercat.top/2022/02/18/Coding-Interviews-C++-with-main/)中浏览以下内容，会有更好的阅读体验。

​                           

## Day 1

### [剑指 Offer 09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

#### 【解题思路】

栈的特点是先进后出，队列的特点是先进先出。

两个栈，stack1负责插入，stack2负责删除。

对于插入操作，插入stack1即可；

对于删除操作，若stack2非空，则stack2.pop()；若stack2为空，则将stack1中所有值按顺序pop到stack2中。然后stack2再pop。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

class CQueue {
    stack<int> stack1, stack2;
public:
    CQueue() {
        while (!stack1.empty())
            stack1.pop();
        while (!stack2.empty())
            stack2.pop();
    }
    
    void appendTail(int value) {
        stack1.push(value);
    }
    
    int deleteHead() {
        if (stack1.empty() && stack2.empty())
            return -1;
    
        if (!stack2.empty())
        {
            int ret = stack2.top();
            stack2.pop();
            return ret;
        }
        
        while(!stack1.empty())
        {
            stack2.push(stack1.top());
            stack1.pop();
        }
        int ret = stack2.top();
        stack2.pop();
        return ret;
    }
};
int main()
{
    CQueue* obj = new CQueue();
    cout<<(obj->deleteHead())<<endl;
    obj->appendTail(5);
    obj->appendTail(2);
    cout<<obj->deleteHead()<<endl;
    cout<<obj->deleteHead()<<endl;
    return 0;
}
```



#### 【样例输出】

``` c++
-1
5
2
```



#### 【**执行用时、内存消耗排名**】

执行用时：212 ms, 在所有 C++ 提交中击败了99%的用户

内存消耗：101MB, 在所有 C++ 提交中击败了78%的用户

通过测试用例：55 / 55

​                          

### [剑指 Offer 30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

#### 【解题思路】

正常stack1，让stack2存放stack1中的**不上升**序列。注意是**不上升序列**而不是**下降序列**，不然无法处理有相同值的情况。我在这儿RE了。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

class MinStack {
    stack <int> stack1, stack2;
public:
    /** initialize your data structure here. */
    MinStack() {
        while (!stack1.empty()) stack1.pop();
        while (!stack2.empty()) stack2.pop();
    }
    
    void push(int x) {
        stack1.push(x);
        if (stack2.empty() or x<=stack2.top())  stack2.push(x);
    }
    
    void pop() {
        if (stack2.top()==stack1.top()) stack2.pop();
        stack1.pop();
    }
    
    int top() {
        return stack1.top();
    }
    
    int min() {
        return stack2.top();
    }
};

int main()
{
    MinStack minStack = MinStack();
    minStack.push(0);
    minStack.push(1);
    minStack.push(0);
    cout<<minStack.min()<<endl;
    minStack.pop();
    cout<<minStack.min()<<endl;

    return 0;
}
```



#### 【样例输出】

``` c++
0
0
```



#### 【**执行用时、内存消耗排名**】

执行用时：12ms, 在所有 C++ 提交中击败了100%的用户

内存消耗：14.6MB, 在所有 C++ 提交中击败了81%的用户

通过测试用例：19 / 19

​                          

## Day 2

### [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

#### 【解题思路】

递归

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};

class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        if (!head) return {};
        vector <int> a = reversePrint(head->next);
        a.push_back(head->val);
        return a;
    }
};

int main()
{
    ListNode* head = new ListNode(1);
    ListNode* node2 = new ListNode(3);
    ListNode* node3 = new ListNode(2);
    head->next = node2;
    node2->next = node3;
    Solution solution = Solution();
    
    vector <int> ans = solution.reversePrint(head);
    // requires C++ 11
    for (auto i = ans.begin(); i != ans.end(); i++) cout << *i << ' ';

    return 0;
}
```



#### 【样例输出】

``` c++
2 3 1
```



#### 【**执行用时、内存消耗排名**】

执行用时：0ms, 在所有 C++ 提交中击败了100%的用户

内存消耗：11MB, 在所有 C++ 提交中击败了7%的用户

通过测试用例：24 / 24

​               

### [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

#### 【解题思路】

用head和tail表示已经处理好了的链表部分的头指针和尾指针。

每次将tail后的一个节点放到最前面。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

//Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
};

class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        auto tail = head;
        while (tail && tail->next)  //判断tail是为了防止输入是空链表 
        {
            auto swap = tail->next->next;
            tail->next->next = head; 
            head = tail->next;
            tail->next = swap;
        }
        return head;
    }
};

int main()
{
    ListNode* head = new ListNode(1);
    ListNode* node2 = new ListNode(2); head->next = node2;
    ListNode* node3 = new ListNode(3); node2->next = node3;
    ListNode* node4 = new ListNode(4); node3->next = node4;
    ListNode* node5 = new ListNode(5); node4->next = node5;

    Solution solution = Solution();
    ListNode* ans = solution.reverseList(head);
    // requires C++ 11
    for (auto i = ans; i; i=i->next) cout<<i->val<<"->";
    cout<<"NULL\n";
    return 0;
}
```

#### 【样例输出】

``` c++
5->4->3->2->1->NULL
```

#### 【**执行用时、内存消耗排名**】

执行用时：4 ms, 在所有 C++ 提交中击败了94.34%的用户

内存消耗：8.1 MB, 在所有 C++ 提交中击败了61.40%的用户

通过测试用例：27 / 27

​                          

### [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

#### 【解题思路】

参考了leetcode官方题解的第二种解法。

遍历链表三次：

- 第一遍，对每个节点产生复制节点
- 第二遍，处理复制节点的random指针
- 第三遍，分开原链表和复制链表

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std; 
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};

class Solution {
public:
    Node* copyRandomList(Node* head) {
        if (!head) return NULL;
        Node* curr = head;
        Node* copy = NULL;
        
        //第一遍，产生复制节点S'
        while (curr)
        {
            copy = new Node(curr->val);
            copy->next = curr->next;
            curr->next = copy;
            curr = copy->next;        
        }
        copy = head->next;
        
        //第二遍，处理S'->random 
        curr = head;
        while (curr)
        {
            if (curr->random)
                curr->next->random = curr->random->next; // amazing
            curr = curr->next->next;
        }
        
        //第三遍，分开原链表head和复制链表copy 
        curr = head;
        while (curr)
        {
            auto x = curr->next->next;
            if (x) curr->next->next = x->next;
            curr->next = x;
            curr = x;
        }
        
        return copy;
    }
};

int main()
{
    Node* head = new Node(7);
    Node* node1 = new Node(13); head->next = node1;
    Node* node2 = new Node(11); node1->next = node2;
    Node* node3 = new Node(10); node2->next = node3;
    Node* node4 = new Node(1);  node3->next = node4;
    node1->random = head;
    node2->random = node4;
    node3->random = node2;
    node4->random = head;
    
    Solution solution = Solution();
    Node* ans = solution.copyRandomList(head);
    for (auto i=ans; i; i=i->next) 
    {
        cout<<i->val<<" ";
        if (i->random) cout<<i->random->val; else cout<<"NULL";
        cout<<endl;
    }
    return 0;
}
```

#### 【样例输出】

``` c++
7 NULL
13 7
11 1
10 11
1 7
```

#### 【**执行用时、内存消耗排名**】

执行用时：4 ms, 在所有 C++ 提交中击败了97.67%的用户

内存消耗：11 MB, 在所有 C++ 提交中击败了58.37%的用户

通过测试用例：18 / 18



​                           

## Day 3

### [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

#### 【解题思路】

没啥好说的。不过这个代码写法来自LeetCode用户[Charge yourself](https://leetcode-cn.com/u/lcuserlixj/)。学习了。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    string replaceSpace(string s) {
        string ret = "";
        for (auto c:s)
            if (c==' ')
                ret.push_back('%'), ret.push_back('2'), ret.push_back('0');
            else
                ret.push_back(c);
        return ret;
    }
};

int main()
{
    Solution solution = Solution();
    string s = "We are happy.";
    cout<<solution.replaceSpace(s)<<endl;
    return 0;
}
```

#### 【样例输出】

``` c++
We%20are%20happy.
```

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：5.9 MB, 在所有 C++ 提交中击败了95.39%的用户

通过测试用例：27 / 27

​                                                 	

### [剑指 Offer 58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

#### 【解题思路】

略 

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    string reverseLeftWords(string s, int n) {
        return s.substr(n)+s.substr(0,n);
    }
};

int main()
{
    Solution solution = Solution();
    string s = "abcdefg";
    int k = 2;
    cout<<solution.reverseLeftWords(s,k)<<endl;
    return 0;
}
```

#### 【样例输出】

``` c++
cdefgab
```

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：7.7 MB, 在所有 C++ 提交中击败了21.72%的用户

通过测试用例：34 / 34

#### 【备注】

看到一种更棒的写法：

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：7 MB, 在所有 C++ 提交中击败了88.84%的用户

通过测试用例：34 / 34

``` c++
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        n = n % s.size();
        reverse(s.begin(), s.begin() + n);
        reverse(s.begin() + n, s.end());
        reverse(s.begin(), s.end());
        return s;
    }
};
```

​                          

## Day 4

​                          

### [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

#### 【解题思路】

用一个数组判断是否访问过

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        vector<bool> vis(nums.size());
        for (int i=0;i<vis.size();i++) vis[i]=false;
        
        for (auto x:nums)
            if (!vis[x]) vis[x] = true;
            else return x;
        return -1;  // denotes no repeat nums
    }
};

int main()
{
    vector <int> nums = {2, 3, 1, 0, 2, 5, 3};
    Solution solution = Solution();
    cout<< solution.findRepeatNumber(nums)<< endl;
    return 0; 
}
```

#### 【样例输出】

``` c++
2
```

#### 【**执行用时、内存消耗排名**】

执行用时：20 ms, 在所有 C++ 提交中击败了99.25%的用户

内存消耗：22.3 MB, 在所有 C++ 提交中击败了96.92%的用户

通过测试用例：25 / 25

#### 【备注】

看到一种更棒的写法：

时间复杂度O(n)，**空间复杂度O(1)**。

思想是用下标进行控制，把nums[nums[i]]减掉n（即数组中数的上限）。如果某个nums[nums[i]]还没减掉n就是负数了，那就表示nums[i]重复了。

``` c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        int n = nums.size();
        for(int i = 0; i < n; i++){
            int k = nums[i];
            if(k < 0) k += n;
            if(nums[k] < 0) return k;
            nums[k] -= n;
        }
        
        return -1;
    }
};
```

​                                                    

### [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

#### 【解题思路】

因为是有序的数组，所以可以用二分查找。

时间复杂度O(log n)，空间复杂度O(1)。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l=0, r=nums.size()-1;
        if (r<0) return 0;
        
        while (l<r)
        {
            int mid=(l+r)>>1;
            if (nums[mid]<target) l=mid+1; else r=mid;
        }
        if (nums[l]!=target) return 0;
        
        r = nums.size()-1;
        while (l<r)
        {
            int mid = (l+r)>>1;
            if (nums[mid]>target) r=mid-1; else break;
        }
        while (nums[r]!=target && r>=l) r--;
        
        return r-l+1;
    }
};

int main()
{
    vector <int> nums = {5,7,7,8,8,8,8,8,8,8,10};
//    vector <int> nums = {};
    int target = 8;
    Solution solution = Solution();
    cout<< solution.search(nums, target)<< endl;
    return 0; 
}
```

#### 【样例输出】

``` c++
7
```

#### 【**执行用时、内存消耗排名**】

执行用时：4 ms, 在所有 C++ 提交中击败了91.69%的用户

内存消耗：12.8 MB, 在所有 C++ 提交中击败了80.90%的用户

通过测试用例：88 / 88

​                                                    

### [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

#### 【解题思路】

二分。idx和nums[idx]有对应关系。

注意边界情况：即缺失数字0或数字n-1的情况。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int missingNumber(vector<int>& nums) {
        if (nums[0]!=0) return 0;
        int l=0, r=nums.size()-1;
        if (nums[r]==r) return r+1;
        
        while (l<r)
            if (nums[(l+r)>>1]==(l+r)>>1) l=((l+r)>>1)+1; else r=(l+r)>>1;
        
        return l; 
    }
};

int main()
{
    vector <int> nums = {0,1,2,3,4,5,6,7,9};
//    vector <int> nums = {0,1}; //一种很坑的情况 
    Solution solution = Solution();
    cout<< solution.missingNumber(nums)<< endl;
    return 0; 
}
```

#### 【样例输出】

``` c++
8
```

#### 【**执行用时、内存消耗排名**】

执行用时：12 ms, 在所有 C++ 提交中击败了86.68%的用户

内存消耗：16.6 MB, 在所有 C++ 提交中击败了91.05%的用户

通过测试用例：122 / 122

​                          

## Day 5

​                          

### [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

#### 【解题思路】

从右上角开始找，每次要么左移一格，要么下移一格。时间复杂度O(n+m)。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        int row = matrix.size();
        if (row<1) return false;
        int column = matrix[0].size();
        if (column<1) return false;
        
        int indexI = 0, indexJ = column-1;
        while (indexI<row && indexJ>=0 && matrix[indexI][indexJ]!=target)
        {
            if (matrix[indexI][indexJ]>target) indexJ--; else
                                               indexI++;
        }
        
        return indexI<row && indexJ>=0 && matrix[indexI][indexJ]==target;
    }
};

int main()
{
    Solution solution = Solution();
    vector<vector<int>> matrix = 
              {
              {1,   4,  7, 11, 15},
              {2,   5,  8, 12, 19},
              {3,   6,  9, 16, 22},
              {10, 13, 14, 17, 24},
              {18, 21, 23, 26, 30}
              };
//                {{-5}
//                };
    int target = 20;        
    
    cout<<solution.findNumberIn2DArray(matrix, target)<<endl;
    return 0;
}
```

#### 【样例输出】

``` c++
0
```

#### 【**执行用时、内存消耗排名**】

执行用时：16 ms, 在所有 C++ 提交中击败了96.93%的用户

内存消耗：12.6 MB, 在所有 C++ 提交中击败了74.41%的用户

通过测试用例：129 / 129

​                                                

### [剑指 Offer 11. 旋转数组的最小数字](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

#### 【解题思路】

这题虽然我一开始想到了二分，但又想了想，如果序列中绝大部分数字是重复的，就二分不了了，于是选择写线性去了。结果官方题解是二分和线性的结合体：能二分的时候二分，不能二分的时候就一个个移动。

---

学到了：官方题解的二分， mid=low+((high-low)>>1)而不是mid=(low+high)>>1，这是用减法来防止加法溢出。

---



#### 【完整代码】

线性O(n)：

``` c++
class Solution {
public:
    int minArray(vector<int>& numbers) {
        if (numbers.size()==1) return numbers[0];
        if (numbers.size()==2) return min(numbers[0],numbers[1]);
        
        for (int i=0;i<numbers.size()-1;i++)
            if (numbers[i]>numbers[i+1])
                return numbers[i+1];
        return numbers[0];
    }
};
```

二分，平均时间复杂度$O_{avg}(log_{2}x)$：

``` c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int minArray(vector<int>& numbers) {
        int n = numbers.size()-1;
        
        int left = 0, right = n; // denotes the possible area of min
        while (left<right)
        {
            int mid = left + ((right-left)>>1);
            if (numbers[mid]>numbers[right]) left = mid+1; else
            if (numbers[mid]<numbers[left]) right = mid; else
                                            right--;                        
        }
        return numbers[left];
    }
};

int main()
{
    Solution solution = Solution();
    vector<int> numbers = {2,0,1,1,1};        
    
    cout<<solution.minArray(numbers)<<endl;
    return 0;
}
```

#### 【样例输出】

``` c++
0
```

#### 【**执行用时、内存消耗排名**】              

执行用时：4 ms, 在所有 C++ 提交中击败了85.51%的用户

内存消耗：11.7 MB, 在所有 C++ 提交中击败了97.55%的用户

​             

### [剑指 Offer 50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

#### 【解题思路】

俩数组，一个存某个字母访问了几次，一个存第一次访问该字母是在哪个位置。

visit数组用了点技巧，用vis[char-'a']表示访问一次以上，vis[char-'a'+26]表示访问两次以上。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    char firstUniqChar(string s) {
        bool vis[26+26];
        int first[26];
        memset(vis,false,sizeof(vis));
        memset(first,0,sizeof(first));

        for (int index=0; index<s.length(); index++)
            if (vis[s[index]-'a'+26]) continue; else
                if (vis[s[index]-'a']) vis[s[index]-'a'+26]=true; else
                                     vis[s[index]-'a']=true, first[s[index]-'a']=index;
        
        int ans=s.length(); //s[ans] denotes the firstUnicChar
        for (int index=0; index<26; index++)
            if (vis[index] && !vis[index+26] && first[index]<ans) ans=first[index];
        return (ans<s.length())? s[ans] : ' ';
    }
};

int main()
{
    string s = "abaccdeff";
    Solution* solution = new Solution();
    cout<< solution->firstUniqChar(s)<<endl; 
    return 0;
}
```

#### 【样例输出】

``` c++
b
```

#### 【**执行用时、内存消耗排名**】

执行用时：12 ms, 在所有 C++ 提交中击败了97.52%的用户

内存消耗：10.3 MB, 在所有 C++ 提交中击败了90.70%的用户

通过测试用例：104 / 104

​                 

## Day 6

​                          

### [面试题32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

#### 【解题思路】

bfs

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

//Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
 
class Solution {
public:
    vector<int> ret;
    queue<TreeNode*> tree;
    vector<int> levelOrder(TreeNode* root) {
        ret.clear();
        while (!tree.empty()) tree.pop();
        if (!root) return ret;

        ret.push_back(root->val);
        tree.push(root);
        while (!tree.empty())
        {
            TreeNode* curr = tree.front();
            tree.pop();
            if (curr->left) ret.push_back(curr->left->val), tree.push(curr->left);
            if (curr->right) ret.push_back(curr->right->val), tree.push(curr->right);
        }

        return ret;
    }
};

int main()
{
    TreeNode* root = new TreeNode(3);
    TreeNode* node1 = new TreeNode(9);
    TreeNode* node2 = new TreeNode(20);
    TreeNode* node3 = new TreeNode(15);
    TreeNode* node4 = new TreeNode(7);
    
    root->left = node1; root->right = node2;
    node2->left = node3; node2->right = node4;
    
    Solution solution = Solution();
    for (auto x:solution.levelOrder(root))
        cout<<x<<" ";
    cout<<endl;
    return 0;
}
```

#### 【样例输出】

``` c++
3 9 20 15 7
```

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：11.6 MB, 在所有 C++ 提交中击败了98.00%的用户

通过测试用例：34 / 34

​                          

### [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

#### 【解题思路】

很巧妙！用每一轮队列的size来控制，将相同深度的节点放在return vector的同一层。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

//Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
 
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ret;
        queue<TreeNode*> tree;
        if (!root) return ret;

        tree.push(root);
        while (!tree.empty())
        {
            int num = tree.size();
            ret.push_back({});

            while (num--)
            {
                TreeNode* currNode = tree.front();
                ret.back().push_back(currNode->val);
                tree.pop();

                if (currNode->left) tree.push(currNode->left);
                if (currNode->right) tree.push(currNode->right);                
            }

        }

        return ret;
    }
};
int main()
{
    TreeNode* root = new TreeNode(3);
    TreeNode* node1 = new TreeNode(9);
    TreeNode* node2 = new TreeNode(20);
    TreeNode* node3 = new TreeNode(15);
    TreeNode* node4 = new TreeNode(7);
    
    root->left = node1; root->right = node2;
    node2->left = node3; node2->right = node4;
    
    Solution solution = Solution();
    for (auto x:solution.levelOrder(root))
    {
        for (auto y:x)
            cout<<y<<" ";    
        cout<<endl;    
    }

    return 0;
}
```

#### 【样例输出】

``` c++
3
9 20
15 7
```

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：12 MB, 在所有 C++ 提交中击败了97.90%的用户

通过测试用例：34 / 34

​                              

### [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

#### 【解题思路】

没啥好说的。在上一题的基础上加个reverse。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

//Definition for a binary tree node.
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
 
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ret;
        queue<TreeNode*> tree;
        if (!root) return ret;

        tree.push(root);
        bool odd = true;
        while (!tree.empty())
        {
            int num = tree.size();
            ret.push_back({});
                                                 
            while (num--)
            {
                TreeNode* currNode = tree.front();
                ret.back().push_back(currNode->val);
                tree.pop();

                if (currNode->left) tree.push(currNode->left);
                if (currNode->right) tree.push(currNode->right);                
            }
            
            if (!odd) reverse(ret.back().begin(), ret.back().end());
            odd^=1;
        }

        return ret;
    }
};
int main()
{
    TreeNode* root = new TreeNode(3);
    TreeNode* node1 = new TreeNode(9);
    TreeNode* node2 = new TreeNode(20);
    TreeNode* node3 = new TreeNode(15);
    TreeNode* node4 = new TreeNode(7);
    
    root->left = node1; root->right = node2;
    node2->left = node3; node2->right = node4;
    
    Solution solution = Solution();
    for (auto x:solution.levelOrder(root))
    {
        for (auto y:x)
            cout<<y<<" ";    
        cout<<endl;    
    }

    return 0;
}
```

#### 【样例输出】

``` c++
3
20 9
15 7
```

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：11.9 MB, 在所有 C++ 提交中击败了99.57%的用户

通过测试用例：34 / 34               

​                           

​                  

​                

## Day 7

### [剑指 Offer 26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

#### 【解题思路】

递归

#### 【完整代码】

``` c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSameStructure(TreeNode* A, TreeNode* B)
    {
        if (A == nullptr) return false;
        if (A->val != B->val) return false;
        if (B->left && !isSameStructure(A->left, B->left)) return false;
        if (B->right && !isSameStructure(A->right, B->right)) return false;
        return true;
    }

    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (!B) return false;
        if (isSameStructure(A, B)) return true;
        if (A->left && isSubStructure(A->left, B)) return true;
        if (A->right && isSubStructure(A->right, B)) return true;
        return false;
    }
};
```

#### 【样例输出】

本题比较简单，就没写主程序。

#### 【**执行用时、内存消耗排名**】

执行用时：32 ms, 在所有 C++ 提交中击败了91.36%的用户

内存消耗：32.9 MB, 在所有 C++ 提交中击败了41.02%的用户

通过测试用例：48 / 48

​                   

### [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

#### 【解题思路】

递归

#### 【完整代码】

``` c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* mirrorTree(TreeNode* root) {
        if (root == nullptr) return nullptr;
        TreeNode* left = mirrorTree(root->right);
        TreeNode* right = mirrorTree(root->left);
        root->left = left;
        root->right = right;
        return root;
    }
};	
```

#### 【样例输出】

本题比较简单，就没写主程序。

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：8.9 MB, 在所有 C++ 提交中击败了64.52%的用户

通过测试用例：68 / 68

​                   

### [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

#### 【解题思路】

递归

#### 【完整代码】

``` c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isPair(TreeNode* L, TreeNode* R){
        if (!L && !R) return true;
        if (!L || !R) return false;
        if (L->val != R->val) return false;
        return isPair(L->left, R->right) and isPair(L->right, R->left);
    }
    bool isSymmetric(TreeNode* root) {
        if (!root) return true;
        return isPair(root->left, root->right);
    }
};
```

#### 【样例输出】

本题比较简单，就没写主程序。

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：15.8 MB, 在所有 C++ 提交中击败了88.05%的用户

通过测试用例：195 / 195

​                                     

​               

## Day 8

### [剑指 Offer 10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

#### 【解题思路1】

思想是用a,b两个变量，分别存储fib[n]和fib[n+1]。

从知乎上看到的，很妙的解法。时间复杂度O(N)，空间复杂度O(1)。

#### 【完整代码1】

``` c++
class Solution {
public:
    int fib2(int a, int b, int n){
        if (n<1) return a;
        return fib2(b, (a+b)%1000000007, n-1);
    }
    int fib(int n) {
        return fib2(0, 1, n);
    }
};
```

#### 【解题思路2】

把斐波那契数列的公式写成矩阵乘法的形式，然后就可以用矩阵快速幂加速，O(log N)的时间复杂度。

#### 【完整代码2】

``` c++
class Solution {
public:
    void matrixMultiply(int A[][2], int B[][2]){
        int C[2][2];
        for (int i=0;i<2;i++)
        for (int j=0;j<2;j++)
            C[i][j] = (1ll*A[i][0]*B[0][j] + 1ll*A[i][1]*B[1][j])%1000000007;
        
        for (int i=0;i<4;i++) A[i/2][i%2]=C[i/2][i%2];
    }
    int fib(int n) {
        int A[2][2], B[2][2];
        for (int i=0;i<4;i++) A[i/2][i%2]=0, B[i/2][i%2]=1; B[0][0]=0;
        A[0][0]=1; A[1][1]=1;

        while (n)
        {
            if (n&1) matrixMultiply(A, B);
            n/=2;
            matrixMultiply(B, B);
        }
        return A[1][0];
    }
};
```

> https://www.cnblogs.com/alexandergan/p/12225814.html中介绍了C++二维数组传参的方法

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：5.7 MB, 在所有 C++ 提交中击败了97.66%的用户

通过测试用例：51 / 51

​                         

### [剑指 Offer 10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

#### 【解题思路】

与上一题基本相同。

#### 【完整代码】

``` c++
class Solution {
public:
    int fib(int a, int b, int n){
        if (n<1) return a;
        return fib(b, (a+b)%1000000007, n-1);
    }
    int numWays(int n) {
        return fib(1, 1, n);
    }
};
```

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：5.8 MB, 在所有 C++ 提交中击败了72.21%的用户

通过测试用例：51 / 51

​                 

### [剑指 Offer 63. 股票的最大利润](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

#### 【解题思路】

如果必在第i天出售，则在前i-1天中股票最低价时购入是最划算的。贪心。

#### 【完整代码】

``` c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int min_price = 1e9, ans = 0;
        for (auto price:prices){
            ans = max(ans, price-min_price);
            min_price = min(min_price, price);
        }
        return ans;
    }
};
```

#### 【**执行用时、内存消耗排名**】

执行用时：4 ms, 在所有 C++ 提交中击败了88.50%的用户

内存消耗：12.4 MB, 在所有 C++ 提交中击败了86.17%的用户

通过测试用例：200 / 200

​            

​               

## Day 9

### [剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

#### 【解题思路】

贪心。负滴不要。

#### 【完整代码】

``` c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int ans=nums[0], sum=nums[0];
        for (int i=1; i<nums.size(); i++) {
            sum = max(sum+nums[i], nums[i]);
            ans = max(ans, sum);
        }
        return ans;
    }
};
```

#### 【**执行用时、内存消耗排名**】

执行用时：12 ms, 在所有 C++ 提交中击败了95.54%的用户

内存消耗：22.3 MB, 在所有 C++ 提交中击败了82.43%的用户

通过测试用例：202 / 202

​                        

### [剑指 Offer 47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

#### 【解题思路】

经典的dp问题。

#### 【完整代码】

``` c++
class Solution {
public:
    int maxValue(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        for (int i=1; i<m; i++) grid[i][0] += grid[i-1][0];
        for (int j=1; j<n; j++) grid[0][j] += grid[0][j-1];

        for (int i=1; i<m; i++)
        for (int j=1; j<n; j++)
            grid[i][j] += max(grid[i-1][j], grid[i][j-1]);
        return grid[m-1][n-1];
    }
};
```

#### 【**执行用时、内存消耗排名**】

执行用时：4 ms, 在所有 C++ 提交中击败了95.48%的用户

内存消耗：8.8 MB, 在所有 C++ 提交中击败了93.37%的用户

通过测试用例：61 / 61

​                        

​                           

## Day 10

### [剑指 Offer 46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

#### 【解题思路】

简单动态规划。

备注：题解用了to_string()，省去了我把num拆成每一位放进a数组的过程。

#### 【完整代码】

``` c++
class Solution {
public:
    int translateNum(int num) {
        if (num<10) return 1;
        int cnt=0;
        vector<int> a(10);
        while (num) a[cnt++]=num%10, num/=10;
        for (int i=0;i<(cnt+1)/2;i++) swap(a[i],a[cnt-i-1]);

        int p = 1;
        int q = (a[0]!=0 && a[0]*10+a[1]<26)? 2:1;
        for (int i=2;i<cnt;i++) {
            int x = p;
            p = q;
            if (a[i-1]!=0 && a[i-1]*10+a[i]<26) q += x;            
        }

        return q;
    }
};
```

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：5.7 MB, 在所有 C++ 提交中击败了97.49%的用户

通过测试用例：49 / 49

​                  

### [剑指 Offer 48. 最长不含重复字符的子字符串](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

#### 【解题思路】

我写了一个很丑的orz

因为这题有坑，字符串不只包含a~z，还可能有空格啥的。我为了处理未知的字符，用了map。

然后去题解那里一看，原来C++的数组下标也可以是字符，会转成对应的ASCII码。

#### 【我的代码】

``` c++
class Solution {
public:
        int lengthOfLongestSubstring(string s) {
            map<char, int> indexOfChar;

            int ans = 0, left = 0, right = 0;
            while (right < s.length()) {
                char c = s[right];
                if (indexOfChar.find(c) == indexOfChar.end()) {
                    indexOfChar[c] = right;
                    right++;
                    continue;
                }
                ans = max(ans, right-left);
                for (int j = left; j<indexOfChar[c]; j++)
                    indexOfChar.erase( indexOfChar.find(s[j]) );
                left = indexOfChar[c]+1;
                indexOfChar[c] = right++;
            }
            return max(int(s.length())-left, ans);
    }
};


```

#### 【题解代码】

``` c++
class Solution {
public:
        int lengthOfLongestSubstring(string s) {
            vector<int> dp(128,-1);//存储每个字符最后出现的位置
            int i=0,j=0,res=0;
            for(;j<s.size();j++){    
                if(dp[s[j]]<i)//前面的子串不含新加的字符
                    res=max(res,j-i+1);
                else//当前字符在之前的子串中出现过            
                    i=dp[s[j]]+1;//更新i，使得i到j没有重复字符
                dp[s[j]]=j;//更改当前字符出现的位置                               
            }
        return res;
    }
};
```

#### 【**执行用时、内存消耗排名**】

执行用时：4 ms, 在所有 C++ 提交中击败了97.83%的用户

内存消耗：7.6 MB, 在所有 C++ 提交中击败了80.13%的用户

通过测试用例：987 / 987

​                  

​                    

## Day 11

### [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

#### 【解题思路】

让前一个节点指向后一个节点，即跳过这个节点。

注意本题说明：若使用 C 或 C++ 语言，你不需要 `free` 或 `delete` 被删除的节点

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;
 
//Definition for singly-linked list.
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(NULL) {}
    ListNode* vectorToListNode(vector<int> &a);
};

ListNode* ListNode::vectorToListNode(vector<int> &a) {
	ListNode* dummyRoot = new ListNode(0);
	ListNode* ptr = dummyRoot;
	for (auto x:a) {
		ptr->next = new ListNode(x);
		ptr = ptr->next;
	}
	ptr = dummyRoot->next;
	delete dummyRoot;	//don't need to deal with empty vector particularly
	return ptr;
}

class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
    	if (head->val == val) return head->next;
    	
		for (auto ptr=head; ptr; ptr=ptr->next) 
			if (ptr->next->val == val) {
				ptr->next = ptr->next->next;
				break;
			}
		return head;
    }
};

int main()
{
	vector<int> a = {4,5,1,9};
	int val = 5;
	
	ListNode* head;
	head = head->vectorToListNode(a);
	for (auto ptr=head; ptr; ptr = ptr->next)
		cout<< ptr->val;
	cout<<endl;
	
	Solution* solution = new Solution();
	for (auto ptr=solution->deleteNode(head, val); ptr; ptr = ptr->next)
		cout<< ptr->val;
	cout<<endl;
	return 0;
}
```

#### 【样例输出】

``` c++
4519
419
```

#### 【**执行用时、内存消耗排名**】

执行用时：8 ms, 在所有 C++ 提交中击败了86.65%的用户

内存消耗：8.9 MB, 在所有 C++ 提交中击败了99.28%的用户

通过测试用例：37 / 37

​                     

### [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

#### 【解题思路】

双指针，ptr遍历到末尾，Kth则表示从ptr倒数第K个节点。

#### 【完整代码】

``` c++
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        ListNode* ptr = head;
        ListNode* Kth = head;
        for (int i=0;i<k-1;i++) ptr = ptr->next;

        for (; ptr->next!=nullptr; ptr = ptr->next)
            Kth = Kth->next;
        return Kth;
    }
};
```

#### 【**执行用时、内存消耗排名**】

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：10.2 MB, 在所有 C++ 提交中击败了91.92%的用户

通过测试用例：208 / 208

​                    

## Day 12

### [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

#### 【解题思路】

经典题，俩指针遍历俩链表，哪个值小哪个继续挪

#### 【完整代码】

``` c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* dummyRoot = new ListNode(0);
        ListNode* ptr = dummyRoot;

        while (l1!=nullptr && l2!=nullptr) {
            if (l1->val < l2->val) ptr->next = l1, l1 = l1->next;
                else                   ptr->next = l2, l2 = l2->next;
            ptr = ptr->next;
        }
        ptr->next = (l1 != nullptr)? l1 : l2;

        ptr = dummyRoot->next;
        delete dummyRoot;
        return ptr;
    }
};
```

#### 【**执行用时、内存消耗排名**】

执行用时：16 ms, 在所有 C++ 提交中击败了93.80%的用户

内存消耗：18.6 MB, 在所有 C++ 提交中击败了93.20%的用户

通过测试用例：218 / 218

​                     

### [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

#### 【解题思路】

优雅。看了题解做的。

把两个链表拼接起来，这样总会走到公共节点。

#### 【完整代码】

``` c++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* ptrA = headA;
        ListNode* ptrB = headB;
        while (headA!=headB) {
            headA = (headA)? headA->next : ptrB;
            headB = (headB)? headB->next : ptrA;
        }
        return headA;
    }
};
```

#### 【**执行用时、内存消耗排名**】

执行用时：32 ms, 在所有 C++ 提交中击败了97.60%的用户

内存消耗：14 MB, 在所有 C++ 提交中击败了97.99%的用户

通过测试用例：45 / 45

​                     

​                              

​                          

## 其他

### [32. 最长有效括号](https://leetcode-cn.com/problems/longest-valid-parentheses/)

#### 【解题思路】

用counter记录，遇到左括号就+1，遇到右括号就-1。         

用数组index[x]存放counter值为x时的合法字符串的左端点下标。           

如果遇到一个右括号，那么index[counter+2]就会变得不合法。不合法记为1e9。        

由于这个过程中，counter可能因为右括号很多而变为负数，所以用counter_convert存放取模后的值。

#### 【完整代码】

``` c++
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    int index[30001];

    int longestValidParentheses(string s) {
        int counter = 0;
        int ans = 0;                    // length of the longest valid parentheses
        int n = s.length();

        for (int i=0; i<n+1; i++) index[i]=1e9;
        
        for (int i=0; i<s.length(); i++){
            char c = s[i];
            assert (c=='(' or c==')');  // only () in the string
            counter = (c=='(')? counter+1 : counter-1;
            int counter_convert = (counter+n)%n;    // to avoid negative index

            if (c==')'){
                ans = max(ans, i-index[(counter_convert+1)%n]+1);
                index[(counter_convert+2)%n] = 1e9;   // becomes invalid
            } else
                index[counter_convert] = min(index[counter_convert], i);
        
        }
        return ans;
    }
};

int main()
{
	string s = ")()())";
	
	Solution solution = Solution();
	cout<<solution.longestValidParentheses(s)<<endl;
	return 0;
}
```

#### 【样例输出】

``` c++
4
```

#### 【**执行用时、内存消耗排名**】

时间和空间复杂度都是O(n)

执行用时：0 ms, 在所有 C++ 提交中击败了100.00%的用户

内存消耗：6.7 MB, 在所有 C++ 提交中击败了86.75%的用户



