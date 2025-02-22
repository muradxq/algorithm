# Union Find

## Implementation

### Basic

Note that there is a path compression in the `find` function.

The time complexity of one operation in `UnionFind` with `N` elements and path compression only is amortized `O(logN)`.

```cpp
class UnionFind {
    vector<int> id;
public:
    UnionFind(int n) : id(n) {
        iota(begin(id), end(id), 0);
    }
    int find(int a) {
        return id[a] == a ? a : (id[a] = find(id[a]));
    }
    void connect(int a, int b) {
        id[find(a)] = find(b);
    }
    bool connected(int a, int b) {
        return find(a) == find(b);
    }
};
```

### Get Size of Union

We can use a `size` array to keep track of the size of each union.

`cnt` keeps track of the number of unions.

```cpp
class UnionFind {
    vector<int> id, size;
    int cnt;
public:
    UnionFind(int n) : id(n), size(n, 1), cnt(n) {
        iota(begin(id), end(id), 0);
    }
    int find(int a) {
        return id[a] == a ? a : (id[a] = find(id[a]));
    }
    void connect(int a, int b) {
        int x = find(a), y = find(b);
        if (x == y) return;
        id[x] = y;
        size[y] += size[x];
        --cnt;
    }
    bool connected(int a, int b) { return find(a) == find(b); }
    int getSize(int a) { return size[find(a)]; }
    int getCount() { return cnt; }
};
```

### Merge using Rank

The time complexity of one operation in `UnionFind` with `N` elements, path compression and union by rank is amortized `O(alpha(N))` where `alpha(N)` is the inverse function of Ackermann function. Note that `O(alpha(N))` is even more efficient than `O(logN)`.

![](../../data-structure/union-by-rank.png)

```cpp
class UnionFind {
    vector<int> id, rank;
    int cnt; // `cnt` is the number of connected components in the graph
public:
    UnionFind(int n) : id(n), rank(n, 0), cnt(n) {
        iota(begin(id), end(id), 0);
    }
    int find(int a) {
        return id[a] == a ? a : (id[a] = find(id[a]));
    }
    void connect(int i, int j) {
        int p = find(i), q = find(j);
        if (p == q) return;
        if (rank[p] > rank[q]) id[q] = p; // always use the node with GREATER rank as the root
        else {
            id[p] = q;
            if (rank[p] == rank[q]) rank[q]++;
        }
        --cnt;
    }
    bool connected(int i, int j) { return find(i) == find(j); }
    int getCount() { return cnt; }
};
```

## Problems

### Static Connection

* [130. Surrounded Regions (Medium)](https://leetcode.com/problems/surrounded-regions/)
* [200. Number of Islands (Medium)](https://leetcode.com/problems/number-of-islands/)
* [737. Sentence Similarity II (Medium)](https://leetcode.com/problems/sentence-similarity-ii)
* [924. Minimize Malware Spread (Hard)](https://leetcode.com/problems/minimize-malware-spread/)
* [947. Most Stones Removed with Same Row or Column (Medium)](https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/)
* [952. Largest Component Size by Common Factor (Hard)](https://leetcode.com/problems/largest-component-size-by-common-factor/)
* [959. Regions Cut By Slashes (Medium)](https://leetcode.com/problems/regions-cut-by-slashes/) **Split into sub-cells**
* [990. Satisfiability of Equality Equations (Medium)](https://leetcode.com/problems/satisfiability-of-equality-equations/)
* [1202. Smallest String With Swaps (Medium)](https://leetcode.com/problems/smallest-string-with-swaps/)
* [1319. Number of Operations to Make Network Connected (Medium)](https://leetcode.com/problems/number-of-operations-to-make-network-connected/)

### Dynamic Connection

Involves multiple steps to build the connection gradually.

* [305. Number of Islands II (Hard)](https://leetcode.com/problems/number-of-islands-ii)

### Back in Time

Since Union Find can only BUILD connection gradually, when we see problems that BREAK connection gradually with multiple steps, we can traverse the steps in reverse order so as to BUILD the connections gradually.

* [803. Bricks Falling When Hit (Hard)](https://leetcode.com/problems/bricks-falling-when-hit)
* [1970. Last Day Where You Can Still Cross (Hard)](https://leetcode.com/problems/last-day-where-you-can-still-cross)
