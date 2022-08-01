---
title:  "유일한 경로"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - ps
tags:
  - PS
---

https://leetcode.com/problems/unique-paths/

Bottom-up dp나 memoization을 이용하여서 푸는 문제이다

로봇은 오른쪽으로나 아래방향으로 밖에 움직이지 못한다.

0번 행렬과 0번 컬럼은 전부 1이 되고

dp[i][j] = dp[i - 1][j] + dp[i][j - 1]을 이용하여서

풀 수 있는 문제이다.

```c++
class Solution {
public:
    int dp[101][101] = {0,};
    int uniquePaths(int m, int n) {
        dp[0][0] = 1;
        for (int i = 1; i < m; i++) {
            dp[i][0] = 1;
        }
        for (int j = 1; j < n; j++) {
            dp[0][j] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
};
```
