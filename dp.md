# 📚 动态规划 (Dynamic Programming, DP)

动态规划是一种在数学、计算机科学和经济学中使用的，通过把**复杂问题分解成更简单的子问题**来解决优化问题的技术。

它与分治法类似，但关键区别在于：**子问题之间存在重叠**。DP 通过储存并复用这些重叠子问题的解，避免了重复计算，从而大大提高了效率。

## ✨ DP 的两大特征

1.  **最优子结构 (Optimal Substructure)**: 问题的最优解包含着其子问题的最优解。
2.  **重叠子问题 (Overlapping Subproblems)**: 在求解过程中，许多子问题会被重复求解。

## 📏 经典问题一：最长递增子序列 (LIS)

给定一个数列 $A = (a_1, a_2, \dots, a_n)$，求出它的一个子序列，使得这个子序列元素是**严格递增**的，并且长度**最长**。

### 定义状态

我们定义 $DP[i]$ 表示：以 $a_i$ 这个元素**结尾**的最长递增子序列的长度。

### 状态转移方程

要计算 $DP[i]$，我们需要向前看，遍历所有 $j < i$。如果 $a_j < a_i$，则说明 $a_i$ 可以接在以 $a_j$ 结尾的递增子序列后面。因此：

$$DP[i] = \max(\{DP[j] + 1 \mid j < i, a_j < a_i\} \cup \{1\})$$

最终答案是所有 $DP[i]$ 中的最大值。

### 代码示例 (C++)

```cpp
int LIS(const std::vector<int>& a) {
    int n = a.size();
    if (n == 0) return 0;
    
    std::vector<int> dp(n, 1); // 初始化，每个元素本身都是长度为1的递增子序列
    int max_len = 1;
    
    for (int i = 1; i < n; ++i) {
        for (int j = 0; j < i; ++j) {
            // 如果 a[i] 可以接在以 a[j] 结尾的子序列后面
            if (a[j] < a[i]) {
                dp[i] = std::max(dp[i], dp[j] + 1);
            }
        }
        max_len = std::max(max_len, dp[i]);
    }
    return max_len;
}
