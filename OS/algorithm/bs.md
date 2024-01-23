# 笔试题

## 京东

1.用于找到数组 b，满足 (ai + bi) % i == 0 且 b 中的所有元素均不相同

在这个程序中，findArrayB 函数接受一个整数数组 a，并返回一个满足条件的数组 b。主函数演示了如何使用这个函数，并打印出数组A和数组B的内容。
```cpp
#include <iostream>
#include <vector>
#include <unordered_set>

std::vector<int> findArrayB(const std::vector<int>& a) {
    int n = a.size();
    std::vector<int> b(n);
    std::unordered_set<int> used;

    for (int i = 0; i < n; ++i) {
        int currentBi = n * i - a[i];

        // 确保 b 中的元素均不相同
        while (used.count(currentBi) > 0) {
            ++currentBi;
        }

        b[i] = currentBi;
        used.insert(currentBi);
    }

    return b;
}

int main() {
    std::vector<int> arrayA = {2, 3, 5, 7, 11}; // 你的arrayA
    std::vector<int> arrayB = findArrayB(arrayA);

    std::cout << "arrayA: ";
    for (int num : arrayA) {
        std::cout << num << " ";
    }

    std::cout << "\narrayB: ";
    for (int num : arrayB) {
        std::cout << num << " ";
    }

    return 0;
}
```

2.使用面向对象的设计来模拟兽游戏的问题。在这里，你需要创建一个基类 Player，并派生出两个类 Human 和 Beast，这两个类都有一个虚函数用于攻击其他玩家

在这个示例中，Player 是基类，Human 和 Beast 分别是派生类。它们都有一个 attack 函数，通过虚函数的机制，实现了在运行时根据对象的实际类型调用相应的攻击函数。这样，你可以轻松扩展游戏，添加新的角色类型，而不需要复杂的 if-else 逻辑。
```cpp
#include <iostream>
#include <string>

class Player {
public:
    Player(const std::string& name) : name(name) {}

    virtual void attack(Player& other) const {
        std::cout << name << " attacks " << other.getName() << std::endl;
    }

    const std::string& getName() const {
        return name;
    }

private:
    std::string name;
};

class Human : public Player {
public:
    Human(const std::string& name) : Player(name) {}

    // 重写攻击函数
    void attack(Player& other) const override {
        std::cout << getName() << " (Human) attacks " << other.getName() << std::endl;
    }
};

class Beast : public Player {
public:
    Beast(const std::string& name) : Player(name) {}

    // 重写攻击函数
    void attack(Player& other) const override {
        std::cout << getName() << " (Beast) attacks " << other.getName() << std::endl;
    }
};

int main() {
    Human human("Alice");
    Beast beast("Goblin");

    // 模拟对战
    human.attack(beast);
    beast.attack(human);

    return 0;
}
```

3.你正在爬楼梯。它需要 n 阶你才能到达楼顶，每次你可以爬 1 或 2 个台阶。问有多少种不同的方法可以爬到楼顶。

在这个示例中，climbStairs 函数接受一个整数 n，表示楼梯的阶数，返回爬到楼顶的不同方法数量。这个问题的解法是典型的动态规划问题，使用数组保存中间结果，从而避免重复计算。
```cpp
#include <iostream>
#include <vector>

int climbStairs(int n) {
    if (n <= 2) {
        return n;
    }

    // 创建一个数组用于保存每个阶梯的爬法数量
    std::vector<int> dp(n + 1, 0);

    // 初始条件
    dp[1] = 1;
    dp[2] = 2;

    // 动态规划，从第三阶开始计算
    for (int i = 3; i <= n; ++i) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }

    return dp[n];
}

int main() {
    int n = 5; // 你要爬的楼梯数

    int ways = climbStairs(n);

    std::cout << "There are " << ways << " ways to climb the stairs." << std::endl;

    return 0;
}
```

4.n道题，时间为t，完全可以分完全解决，初步解决和放弃，分别会得到不同的分数，在有限的时间t下怎么规划才能拿最高的分?


在这个示例中，t 和 s 分别表示每道题目需要的时间和得分，T 表示总时间。 maxScore 函数返回在有限时间内获得的最大分数
```cpp
#include <iostream>
#include <vector>

int maxScore(std::vector<int>& t, std::vector<int>& s, int T) {
    int n = t.size();  // 题目数量

    // 初始化二维数组
    std::vector<std::vector<int>> dp(T + 1, std::vector<int>(n + 1, 0));

    // 动态规划
    for (int i = 1; i <= T; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (i >= t[j - 1]) {
                dp[i][j] = std::max(dp[i][j - 1], std::max(dp[i - t[j - 1]][j - 1] + s[j - 1], dp[i - t[j - 1]][j] + s[j - 1]));
            } else {
                dp[i][j] = dp[i][j - 1];
            }
        }
    }

    return dp[T][n];
}

int main() {
    std::vector<int> t = {2, 3, 4, 1, 2};
    std::vector<int> s = {4, 7, 5, 1, 3};
    int T = 10;

    int result = maxScore(t, s, T);

    std::cout << "Maximum Score: " << result << std::endl;

    return 0;
}
```

5.排序然后从最大的开始选，如果比他小的差值成立里就相乘

在这个示例中，maxProductDifference 函数接受一个整数向量 nums，首先对它进行降序排序，然后选取最大的两个元素和最小的两个元素，计算它们的差值的乘积。这是一个贪心的选择，但请注意，贪心算法并不总是能够提供全局最优解。在一些情况下，需要证明算法的正确性。
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int maxProductDifference(std::vector<int>& nums) {
    // 先排序
    std::sort(nums.begin(), nums.end(), std::greater<int>());

    // 选取最大的两个元素和最小的两个元素
    int max1 = nums[0];
    int max2 = nums[1];
    int min1 = nums[nums.size() - 1];
    int min2 = nums[nums.size() - 2];

    // 计算差值的乘积
    int result = (max1 * max2) - (min1 * min2);

    return result;
}

int main() {
    std::vector<int> nums = {4, 2, 5, 9, 7, 4, 8};
    
    int result = maxProductDifference(nums);

    std::cout << "Maximum Product Difference: " << result << std::endl;

    return 0;
}
```

### LC1064 不动点

给定已经按升序排列、由不同整数组成的数组 A，返回满足 A[i] == i 的最小索引 i。
如果不存在这样的 i，返回 -1。

```
示例 1：
输入：[-10,-5,0,3,7]
输出：3
解释：
对于给定的数组，A[0] = -10，A[1] = -5，A[2] = 0，A[3] = 3，因此输出为 3 。

示例 2：
输入：[0,2,5,8,17]
输出：0
示例：
A[0] = 0，因此输出为 0 。

示例 3：
输入：[-10,-5,3,4,7,9]
输出：-1
解释： 
不存在这样的 i 满足 A[i] = i，因此输出为 -1 。
 
提示：
1 <= A.length < 10^4
-10^9 <= A[i] <= 10^9
```

```cpp
class Solution {
public:
    int fixedPoint(vector<int>& A) {
    	for(int i = 0; i < A.size(); ++i)
    	{
    		if(A[i] == i)
    			return i;
    	}
    	return -1;
    }
};
```

### LC9.回文数
给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

例如，121 是回文，而 123 不是。

```
示例 1：

输入：x = 121
输出：true
示例 2：

输入：x = -121
输出：false
解释：从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
示例 3：

输入：x = 10
输出：false
解释：从右向左读, 为 01 。因此它不是一个回文数。
 
提示：

-231 <= x <= 231 - 1
```

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }
        int revertedNumber = 0;
        while (x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }
        return x == revertedNumber || x == revertedNumber / 10;
    }
};
```

### LC152.乘积最大子数组
给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

测试用例的答案是一个 32-位 整数。

子数组 是数组的连续子序列。

```
示例 1:

输入: nums = [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
示例 2:

输入: nums = [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
 

提示:

1 <= nums.length <= 2 * 104
-10 <= nums[i] <= 10
nums 的任何前缀或后缀的乘积都 保证 是一个 32-位 整数
```

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int maxF = nums[0], minF = nums[0], ans = nums[0];
        for (int i = 1; i < nums.size(); ++i) {
            int mx = maxF, mn = minF;
            maxF = max(mx * nums[i], max(nums[i], mn * nums[i]));
            minF = min(mn * nums[i], min(nums[i], mx * nums[i]));
            ans = max(maxF, ans);
        }
        return ans;
    }
};
```

### LC215.数组中的第K个最大元素
给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。

请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

你必须设计并实现时间复杂度为 O(n) 的算法解决此问题。

```
示例 1:

输入: [3,2,1,5,6,4], k = 2
输出: 5
示例 2:

输入: [3,2,3,1,2,4,5,5,6], k = 4
输出: 4
 

提示：

1 <= k <= nums.length <= 105
-104 <= nums[i] <= 104
```

```cpp
class Solution {
public:
    int quickselect(vector<int> &nums, int l, int r, int k) {
        if (l == r)
            return nums[k];
        int partition = nums[l], i = l - 1, j = r + 1;
        while (i < j) {
            do i++; while (nums[i] < partition);
            do j--; while (nums[j] > partition);
            if (i < j)
                swap(nums[i], nums[j]);
        }
        if (k <= j)return quickselect(nums, l, j, k);
        else return quickselect(nums, j + 1, r, k);
    }

    int findKthLargest(vector<int> &nums, int k) {
        int n = nums.size();
        return quickselect(nums, 0, n - 1, n - k);
    }
};
```