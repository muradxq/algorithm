# Catalan Number

The `n`th Catalan number is given directly in terms of binomial coefficients by

$$
C_n = \dfrac{1}{n+1}{2n \choose n} = \dfrac{(2n)!}{(n+1)!n!}=\prod_{k=2}^{n}\dfrac{n+k}{k}\quad \text{for } n \ge 0
$$

```cpp
// OJ: https://leetcode.com/problems/unique-binary-search-trees
// Author: github.com/lzl124631x
// Time: O(N)
// Space: O(1)
class Solution {
public:
    int numTrees(int n) {
        long long ans = 1, i;
        for (i = 1; i <= n; ++i) ans = ans * (i + n) / i;
        return ans / i;
    }
};
```

## Problems

* [96. Unique Binary Search Trees \(Medium\)](https://leetcode.com/problems/unique-binary-search-trees/)

Related:

* [241. Different Ways to Add Parentheses (Medium)](https://leetcode.com/problems/different-ways-to-add-parentheses/)

## Reference

* [https://en.wikipedia.org/wiki/Catalan\_number](https://en.wikipedia.org/wiki/Catalan_number)



