---
title:  "숫자 블록"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - BackEnd
tags:
  - PS
---

[숫자블록 프로그래머스](https://programmers.co.kr/learn/courses/30/lessons/12923)

- 소수 구하기의 활용 문제
- 에라토스테네스의 체에서 소수를 찾을 때 제곱근까지만 확인하면 되는 것을 이용한다
- 가장 큰 소수값을 구하는 것이 블록의 숫자를 구하는 방법이다

```c++
#include <string>
#include <vector>
#include <math.h>
using namespace std;

int check(int n) {
    int root = sqrt(n);
    for (int i = 2; i <= root; i++) {
        int div = n / i;
        if (n % i == 0 && n / i <= 10000000) return n / i;
    }
    return 1;
}

vector<int> solution(long long begin, long long end) {
    vector<int> answer;
    for (int i = begin; i <= end; i++) {
        answer.push_back(check(i));
    }
    if (begin == 1) answer[0] = 0;
    return answer;
}
```