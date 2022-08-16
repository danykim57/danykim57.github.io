---
title:  "첫번째 나오는 유일한 글자"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - ps
tags:
  - PS
---

https://leetcode.com/problems/first-unique-character-in-a-string/

Map에 나오는 글자들의 숫자를 넣으면서 글자길이만큼 돌리고 나서

다시 글자를 돌리면서 Map을 조회하면서 Value 값이 1인 녀석이 나오면 리턴 끝까지 돌렸는데

없으면 -1을 리턴한다.

```c++
class Solution {
public:
    int firstUniqChar(string s) {
        map<char, int> m;
        for (auto c : s) {
            m[c]++;
        }
        for (int i = 0; i < s.length(); i++) {
            if (m[s[i]] == 1) return i;
        }
        return -1;
    }
};

```