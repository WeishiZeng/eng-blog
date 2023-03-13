---
layout: post
title: Binary Lifting
date: '2020-06-26T22:53:02.002-07:00'
author: Weishi Zeng
tags: algorithms
---

# Binary Lifting

## Goal
Find N-th ancestor of a given tree node in O(logN) time.

## Tree Presentation
You are given a tree with `n` nodes numbered from `0` to `n-1` in the form of a parent array, where `parent[i]` is the parent of node `i`.
The root of the tree is node 0.

## Key Idea
1. Precompute every node's `2^n` parents, n = 0, 1, 2, ... Thus it's O(1) to find any node's 1, 2, 4, 8, 16 -th parent.
2. Any `k` can be written in binary form. E.g. `k = 10 = 0b1010 = 2^3 + 2^1`
3. A node's `k`th parent is same as its `k1`th parent's `k2`th parent, where `k = k1 + k2`. E.g. A node's 10th parent is its 8th parent's 2nd parent.

## Key Implementation
### Precompute all nodes' `2^n` parents
1. Definition: `dp[i][j]`: node `i`'s `2^j` parent
2. node `i`'s' `2^j` parent = its `2^(j-1)` parent's `2^(j-1)` parent.
3. Transition function: `dp[i][j] = dp[dp[i][j-1]][j-1]`.
4. `dp[i][j]` depends on `dp[?][j-1]`

#### Time complexity
Precompute walk through all `n` nodes and compute `log(n)` parents. Time complexity is `O(NlgN)`

#### Space complexity
dp matrix requires space of `O(NlgN)`

```java
//the biggest jump we can make before move beyond k
//max j while 2^j <= k
int maxJ = (int) (Math.log(n) / Math.log(2)); //maxPow < 32
int[][] dp = new int[n][maxJ];

// populate all node's first parent
for (int i = 0; i < n; i++) {
    dp[i][0] = parent[i];
}

for (int j = 1; j < maxJ; j++) { // calculate all 2^j parents
    for (int i = 0; i < n; i++) { // walk through all nodes
        if (dp[i][j-1] == -1) { // if 2^j-1 parent doesn't exist
            dp[i][j] = -1;
        } else {
            dp[i][j] = dp[dp[i][j-1]][j-1];
        }
    }
}
```
### Get `k`th ancestor
#### Time complexity
K is decomposed to sum of 2^?. There're at most lg(K) of such terms.
Time complexity is `O(lgN)`

```java
public int getKthAncestor(int node, int k) {
    int res = node;

    // decompose k into sum of 2^?
    // k <= 2^maxJ
    for (int n = maxJ; n >= 0; n--) {
        if ((k & (1 << n)) != 0) { //there's a 1 at nth bit
            if (res == -1) {
                return -1;
            }
            res = dp[res][n];
        }
    }
    return res;
}
```

## Another Binary Lifting
With a smart jump algorithm, smart means we can bound its time complexity to `O(lgN)`, then we can get rid of the `dp[i][j]` array and just use a single jump array of size `n`. This reduce the space complexity to `O(N)` from `O(NlgN)`

### Key Idea
1. `jump[n]` stores node i's next jump
2. `depth[n]` array stores node i's current depth
3. jump if you can, else move one step forward
```java
public int getKthAncestor(int node, int k) {
    while (depth[node] > k) {
        if (depth[jump[node]] < k) {
            node = parent[node];
        } else {
            node = jump[node];
        }
    }
    return node;
}
```
4. Precompute `jump[n]`.
```java
// compute jump for a given node
if (parent.depth - parent.jump.depth == parent.jump.depth - parent.jump.jump.depth) {
    // if parent have two equal length jump forward
    // then jump to cover the two equal length jump
    node.jump = parent.jump.jump;
} else {
    // make one step jump
    node.jump = parent;
}
```

### Why & Who
1. A good intuitive explanation with drawings.
Check it out [here](https://codeforces.com/blog/entry/74847).
2. Original paper from *Eugene W. MYERS* in 1983: [An applicative random-access stack](https://publications.mpi-cbg.de/Myers_1983_6328.pdf).

## Additional Thoughts - Pow(x, n)
### Goal
Compute `x^n` in `O(lgN)` time.
### Comparison
1. In binary lifting, 5 jumps = 4 jumps + 1 jumps
2. In Pow(x, n), `x^5 = x^4 * x = (x^2) * (x^2) * x`
```java
// recursive solution
public double Pow(double x, int n) {
    // 0^0 = 1
    // 0^-2 = infinity

    if (n == 0) {
        return 1;
    }

    if (n < 0) {
        return 1 / (myPow(x, -(n+1)) * x);
        //return 1 / myPow(x, -n); //stackoverflow for 1^-2147483648
    }

    if (x == 0) {
        return 0;
    }

    if (n % 2 == 0) {
        return myPow(x * x, n/2);
    } else {
        return x * myPow(x, n-1);
    }
}
```
```java
// iterative solution
public double Pow(double x, int n) {
   if (n == 0) {
        return 1;
    }

    if (n < 0) {
        return 1 / (myPow(x, -(n+1)) * x);
        //return 1 / myPow(x, -n); //stackoverflow for 1^-2147483648
    }

    if (x == 0) {
        return 0;
    }

    double answer = 1.0;
    while (n > 0) {
        if (n % 2 == 1) {
            answer = answer * x;
        }
        n = n / 2;
        x = x * x;
    }
    return answer;
}
```
