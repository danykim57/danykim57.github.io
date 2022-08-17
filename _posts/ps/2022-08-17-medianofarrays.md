---
title:  "두 배열의 중간값"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - ps
tags:
  - PS
---

https://leetcode.com/problems/median-of-two-sorted-arrays/

두개의 정렬된 배열들의 숫자들 중에서 중간값을 찾는 문제이다.

이분탐색이나 분할정복을 이용하여서 풀어도 되지만

단순하게 정렬을 이용하여도 가능하다.

첫번째 배열과 두번째 배열을 합친 이후에

정렬을 시키고 전체 숫자들의 수가 홀수 값이면 중간 숫자를 아니면

중간의 두개 숫자의 평균값을 내주면 된다.

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        double ans = 0;
        for(int i = 0; i < nums2.size(); i++)
            nums1.push_back(nums2[i]);
        
        sort(nums1.begin(), nums1.end());
        
        int mid = nums1.size() / 2;
        
        if(nums1.size() % 2 == 0){
            ans = (double) (nums1[mid] + nums1[mid - 1]) / 2;
        }
        else if (nums1.size() % 2 == 1){
            ans = nums1[mid];
        }
        return ans;
    }
};
```