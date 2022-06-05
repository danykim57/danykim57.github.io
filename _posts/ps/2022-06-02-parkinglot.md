---
title:  "주차 요금 계산"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - ps
tags:
  - PS
---

- 해쉬와 정렬을 이용하여서 풀어야하는 문제
- 문제에서 주어진 계산식을 믿고 잘사용하는게 좋다.
- sstream 헤더는 istringstream을 이용하기 위해서 넣었다.
- istringstream은 스페이스를 이용하여 문자 구별을 하는 string 처리에 탁월하다.
- 여기서는 우선순위큐로 정렬을 해주었지만 간단하게 vector 정렬을 커스터마이징 하는 것도 방법이다.
- math.h의 ceil() 함수는 float이나 double에서만 이용이 가능하다.

```c++
#include <string>
#include <vector>
#include <iostream>
#include <sstream>
#include <map>
#include <unordered_map>
#include <queue>
#include <math.h>

using namespace std;

int baseMin, baseCost, unitMin, unitCost;

struct info {
    string carRegiId; int cost;
    bool operator< (const info &a) const {
        return carRegiId > a.carRegiId;
    }
};

int calculateCost(int totalMin) {
    if (totalMin <= baseMin) return baseCost;
    int res = baseCost + ceil((double)(totalMin - baseMin) / (double)unitMin) * unitCost;
    return res;
}

vector<int> solution(vector<int> fees, vector<string> records) {
    vector<int> answer;
    //cout << ceil(3 / 2);
    unordered_map<string, string> m; // <carRegiId, startTime>
    unordered_map<string, int> times; //<carRegiId, accumulated time>
    priority_queue<info> pq; //<carRegiId, cost>
    baseMin = fees[0];
    baseCost = fees[1];
    unitMin = fees[2];
    unitCost = fees[3];
    
    for (auto record : records) {
        string time, carRegiId, status;
        istringstream iss(record);
        iss >> time >> carRegiId >> status;
        // cout << time << ' ' << carRegiId << ' ' << status << '\n';
        if (status == "IN") {
            m[carRegiId] = time;
        }
        
        else if (status == "OUT") {
            string start = m[carRegiId];
            m.erase(m.find(carRegiId));
            int startHour = stoi(start.substr(0, 2));
            int startMin = stoi(start.substr(3, 2));

            // cout << startHour << ' ' << startMin << '\n';
            int endHour = stoi(time.substr(0, 2));
            int endMin = stoi(time.substr(3, 2));
            // cout << endHour << ' ' << endMin << '\n';
            int hour = endHour - startHour;
            int min = endMin - startMin;
            if (min < 0) {
                hour--;
                min += 60;
            }
            int totalMin = hour * 60 + min;
            times[carRegiId] += totalMin;
        }
    }
    
    for (auto rest : m) {
        string start = rest.second;
        int startHour = stoi(start.substr(0, 2));
        int startMin = stoi(start.substr(3, 2));
        int endHour = 23;
        int endMin = 59;
        int hour = endHour - startHour;
            int min = endMin - startMin;
            if (min < 0) {
                hour--;
                min += 60;
            }
            int totalMin = hour * 60 + min;
            times[rest.first] += totalMin;
    }
    
    for (auto time : times) {
        pq.push({time.first, calculateCost(time.second)});
    }
    while (!pq.empty()) {
        answer.push_back(pq.top().cost);
        pq.pop();
    }
    return answer;
}
```