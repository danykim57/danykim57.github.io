---
title:  "가장 낮은 부모 노드"
header:
teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - ps
tags:
  - PS
---

https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/

이진 탐색이 가능한 이진 트리에서 p노드와 q노드가 주어질 때 두 노드에 가장 가까운 부몬 노드를 찾는 문제이다.

두 노드, p와 q와 비교를 하였을 때 현재 노드의 값이 두 노드보다 작을 때와 클 때 재귀를 돌려서

두 조건이 맞지 않을 때까지 반복을 하다가 맞지 않을 때 현재 노드를 반환하게 만들면 된다.

```c++
bool isP = false;
bool isQ = false;
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (root->val < p->val && root->val < q->val) return lowestCommonAncestor(root->right, p, q);
    if (root->val > p->val && root->val > q->val) return lowestCommonAncestor(root->left, p , q);
    return root;
}
```
