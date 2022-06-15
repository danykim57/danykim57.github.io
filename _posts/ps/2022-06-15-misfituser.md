---
title:  "불량 사용자"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - ps
tags:
  - PS
---

[프로그래머스 - 불량 사용자](https://programmers.co.kr/learn/courses/30/lessons/64064)

- dfs를 통해서 banned_id 사이즈에 맞는 조합들을 user_id에서 뽑는다.
- 조합에 들어갈 수 있는 user_id는 banned_id의 개체와 같은 사이즈여야한다.
- 조합의 user_id의 개체와 banned_id는 '*'를 제외하고 같은 글자가 같은 위치에 있어야한다.
- 만들어진 조합을 하나의 문자열로 변환시켜서 set에 삽입하여서 중복제거를 해준다.

```c++
#include <bits/stdc++.h>
using namespace std;
set<string> s;
bool visited[10];
void dfs(int level, vector<string> &user_id, vector<string> &banned_id) {
    if (level == banned_id.size()) {
        string str = "";
        for (int i = 0; i < user_id.size(); i++) {
            if (visited[i])
            str += user_id[i];
        }
        s.insert(str);
        return;
    }
    
    for (int i = 0; i < user_id.size(); i++) {
        bool flag = true;
        
        if (visited[i]) continue;
        if (user_id[i].length() != banned_id[level].length()) continue;
        
        for (int j = 0; j < user_id[i].length(); j++) {
            if (banned_id[level][j] == '*') continue;
            if (user_id[i][j] != banned_id[level][j]) {
                flag = false;
                break;
            }
        }
        
        if (flag) {
            visited[i] = true;
            dfs(level + 1, user_id, banned_id);
            visited[i] = false;
        }
    }
}

int solution(vector<string> user_id, vector<string> banned_id) {
    dfs(0, user_id, banned_id);
    return s.size();
}
```