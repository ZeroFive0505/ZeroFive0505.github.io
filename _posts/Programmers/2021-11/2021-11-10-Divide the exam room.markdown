---
layout: post
title: 시험장 나누기
date: 2021-11-10 09:22:43 +0900
categories: Programmers-Level5
---

# 시험장 나누기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/81305)

# 풀이
풀이는 [이 곳](https://yabmoons.tistory.com/684)을 참조했다...

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

const int MAX = 10001;

const int INF = 987654321;

int root;
int divideCnt;
bool canDivide;
bool isChild[MAX];
int leftChild[MAX];
int rightChild[MAX];

vector<int> NUM;

int FindCase(int cur, int l, int r, int mid)
{
    if (cur + l + r <= mid)
        return 1;
    if (cur + l > mid && cur + r > mid)
        return 2;
    if (cur + l > mid)
        return 3;
    if (cur + r > mid)
        return 4;

    return 5;
}

int DFS(int cur, int mid)
{
    // 현재 노드가 자식 노드라면
    // 나눌 수 없다. 따라서 0을 반환
    if (cur == -1)
        return 0;

    // 만약 현재 노드가 최대 상한선 mid보다 크다면
    // 나눠도 mid보다 낮추는게 불가능하다.
    // 따라서 더 이상 나누질 못한다.
    if (NUM[cur] > mid)
    {
        canDivide = false;
        return -1;
    }

    // 왼쪽, 오른쪽의 자식 수를 가져온다.
    int lc = DFS(leftChild[cur], mid);
    int rc = DFS(rightChild[cur], mid);

    // 만약 여기서도 더 이상 나눌수 없다면 0을 반환한다.
    if (!canDivide)
        return 0;

    // 총 5가지 경우의 수가 존재한다.
    switch (FindCase(NUM[cur], lc, rc, mid))
    {
    // 1번 케이스 현재노드 왼쪽 오른쪽 자식노드의 합이 mid보다 작거나 같을 경우는
    // 그대로 반환
    case 1:
        return NUM[cur] + lc + rc;
    // 2번 케이스 현재노드와 왼쪽과 오른쪽 자식의 합이 mid 초과라면
    // 왼쪽 오른쪽 자식을 잘라낸다.
    case 2:
        divideCnt += 2;
        return NUM[cur];
    case 3:
    // 3번 케이스 현재 노드와 왼쪽 자식노드의 합이 mid 초과라면
    // 오른쪽 자식을 가져간다.
        divideCnt += 1;
        return NUM[cur] + rc;
    case 4:
    // 3번 케이스의 반대
    // 왼쪽 자식을 가져간다.
        divideCnt += 1;
        return NUM[cur] + lc;
    case 5:
    // 5번 케이스 왼쪽, 오른쪽 모든 노드를 취하면 mid를 넘고
    // 그 둘중 하나만 가져가면 mid를 넘지 않을 경우에는
    // 그 둘중에 최소가 되는 값을 가져간다.
    // 그룹의 합을 최소치로 만들어야하기에
        divideCnt += 1;
        return NUM[cur] + min(lc, rc);
    }
}

bool Search(int k, int mid)
{   
    // 나눈 횟수는 초기화
    divideCnt = 0;
    // 나눌 수 있는지는 처음에는 참
    canDivide = true;

    // 루트노드에서 시작한다.
    DFS(root, mid);

    // 나눈 횟수가 k 이상이 되면 조건에 위배된다.
    // k - 1번 나눠야 k의 서브트리가 나온다.
    if (divideCnt >= k)
        return false;

    // 또는 나누지 못할 경우
    if (!canDivide)
        return false;

    return true;
}

// 이진탐색을 실시한다.
// 여기서 mid를 최대 상한선으로 설정하고
// 만약 k개 미만으로 나눌 수 있다면
// 답을 갱신한다.
int BinarySearch(int k)
{
    int result = INF;

    int start = 1;

    int end = 10000 * 10000;

    while (start <= end)
    {
        int mid = (start + end) / 2;

        // mid를 상한선으로 뒀을때 
        // 나누기가 가능한가?
        if (Search(k, mid))
        {
            // 가능하다면 상한선을 낮춘다.
            // 최소값을 찾아야한다.
            end = mid - 1;
            result = min(result, mid);
        }
        // 아닐 경우 상한선을 늘린다.
        else
            start = mid + 1;
    }

    return result;
}

int solution(int k, vector<int> num, vector<vector<int>> links)
{
    int answer = 0;

    // 자식노드를 초기화한다.
    fill(leftChild, leftChild + MAX, -1);
    fill(rightChild, rightChild + MAX, -1);

    for (size_t i = 0; i < links.size(); i++)
    {
        int left = links[i][0];
        int right = links[i][1];

        // 만약 왼쪽 자식이 있을 경우에는
        if (left != -1)
        {
            // 현재 노드 i에 left를 넣는다.
            leftChild[i] = left;
            // left 노드는 자식노드로 설정
            isChild[left] = true;
        }

        // 위와 같다.
        if (right != -1)
        {
            rightChild[i] = right;
            isChild[right] = true;
        }
    }

    // 모든 노드를 순회하면서
    // 자식이 아닌 노드 즉 루트노드를 찾는다.
    for (size_t i = 0; i < num.size(); i++)
    {
        if (!isChild[i])
        {
            root = i;
            break;
        }
    }

    NUM = num;

    answer = BinarySearch(k);

    return answer;
}
```