---
layout: post
title: 길 찾기 게임
date: 2021-08-05 08:42:04 +0900
categories: Programmers-Level3
---

# 길 찾기 게임
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42892)

# 풀이
먼저 주어진 노드 정보를 정렬을 했다. 기준은 Y의 값이 최우선으로 Y 값을 기준으로 내림차 순으로 정렬하고 Y의 값이 같을 경우 X의 값을 기준으로 오름차 순으로 정렬한다. 그 이후 트리에 쭉 집어넣고 전위, 후위 순회를 한 결과를 반환하면 된다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

struct sNode
{
    int x;
    int y;
    int id;

    sNode(int x = 0, int y = 0, int id = -1)
    {
        this->x = x;
        this->y = y;
        this->id = id;
    }
};

struct sTree
{
    int id;
    int x;
    sTree* left;
    sTree* right;

    sTree(int id, int x, sTree* left, sTree* right)
    {
        this->id = id;
        this->x = x;
        this->left = left;
        this->right = right;
    }
};


vector<int> preOrder;
vector<int> postOrder;


void PreOrder(sTree* node)
{
    if (node)
    {
        preOrder.push_back(node->id);
        PreOrder(node->left);
        PreOrder(node->right);
    }
}

void PostOrder(sTree* node)
{
    if (node)
    {
        PostOrder(node->left);
        PostOrder(node->right);
        postOrder.push_back(node->id);
    }
}

vector<vector<int>> solution(vector<vector<int>> nodeinfo) 
{
    vector<vector<int>> answer;

    vector<sNode> v;

    int id = 1;

    for (int i = 0; i < nodeinfo.size(); i++)
    {
        int x = nodeinfo[i][0];
        int y = nodeinfo[i][1];

        v.push_back(sNode(x, y, id++));
    }

    sort(v.begin(), v.end(), [](const sNode& a, const sNode& b) {
        if (a.y != b.y)
            return a.y > b.y;
        else
            return a.x < b.x;
    });

    sTree* root = nullptr;

    root = new sTree(v.front().id, v.front().x, nullptr, nullptr);

    for (int i = 1; i < v.size(); i++)
    {
        sTree* parent = nullptr;
        sTree* cur = root;

        while (cur)
        {
            parent = cur;
            if (v[i].x < cur->x)
                cur = cur->left;
            else
                cur = cur->right;
        }

        if (parent->x > v[i].x)
            parent->left = new sTree(v[i].id, v[i].x, nullptr, nullptr);
        else
            parent->right = new sTree(v[i].id, v[i].x, nullptr, nullptr);
    }



    PreOrder(root);
    PostOrder(root);

    answer.push_back(preOrder);
    answer.push_back(postOrder);
    

    return answer;
}
```