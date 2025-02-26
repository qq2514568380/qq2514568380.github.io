# 每日一题-01背包(动规)


{{< music auto="https://music.163.com/#/playlist?id=60198" >}}

# 每日一题-01背包(动规)

### 题目

```
给定 N 个物品和一个容量为 W 的背包。第 i 个物品的重量为 weight[i]，价值为 value[i]。求解如何选择物品装入背包，使得总价值最大？
```

### 思路

### **🌟 核心思想**

- **每次只考虑一件物品**：“如果这件物品装进背包，剩下的空间怎么用？”
- 比较两种选择
  - 不装这件物品：价值 = 之前的最优解
  - 装这件物品：价值 = 这件物品的价值 + 剩余空间的最优解
- **选价值更大的那个**！

### 伪代码

```java
如果 当前物品重量 > 背包容量：
    dp[i][w] = dp[i-1][w]  # 装不下，继承之前的结果
否则：
    dp[i][w] = max(
        dp[i-1][w],  # 不装这件
        dp[i-1][w-物品重量] + 物品价值  # 装这件
    )
```

### 完整代码

```java
    public static int knapsack(int[] weights, int[] values, int capacity) {
        int n = weights.length;
        // 创建动态规划表：dp[i][w] 表示前i个物品在容量w下的最大价值
        int[][] dp = new int[n + 1][capacity + 1];

        // 填充表格
        for (int i = 1; i <= n; i++) {          // 遍历每个物品
            int weight = weights[i - 1];        // 当前物品重量
            int value = values[i - 1];          // 当前物品价值
            for (int w = 1; w <= capacity; w++) { // 遍历每个背包容量
                if (weight > w) {
                    // 当前物品太重，装不下 → 继承前i-1个物品的结果
                    dp[i][w] = dp[i - 1][w];
                } else {
                    // 可以装下 → 比较两种情况，取最大值：
                    // 1. 不装当前物品 → dp[i-1][w]
                    // 2. 装当前物品 → dp[i-1][w - weight] + value
                    dp[i][w] = Math.max(dp[i - 1][w], dp[i - 1][w - weight] + value);
                }
            }
        }
        return dp[n][capacity];
    }

```


