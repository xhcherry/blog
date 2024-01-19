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