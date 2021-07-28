---
layout: post
title: 징검다리 건너기
date: 2021-07-29 07:41:34 +0900
categories: Programmers-Level3
---

# 징검다리 건너기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/64062)

# 풀이
Parametric search 문제, 이분탐색과 매우 유사하다. 먼저 최솟값은 1 최댓값은 배열에서 가장 큰 값을 기준으로 잡고 탐색을 진행한다. mid는 점프의 거리를 나타낸다. 여기서 mid만큼 점프를 했을때 이 징검다리를 건너갈 수 있는지를 확인하고 가능하다면 정답을 갱신하고 좀 더 큰 값의 범위를 키우기 위해서 left를 mid + 1만큼 늘려주고 불가능하다면 right를 mid - 1로 갱신해준다. 건널 수 있는지를 판단하기 위해서는 배열을 순회하면서 배열에서 점프의 거리 횟수를 감소시켰을때 0보다 작아진다면 카운트를 늘리고 이 카운트가 k와 똑같거나 더 커진다면 거짓을 반환하고 아닐경우는 참을 반환한다.

# 코드
```c++
#include <iostream>
#include <string>
#include <algorithm>
#include <vector>

using namespace std;

bool CanAcross(const vector<int>& stones, const int mid, const int k)
{
    int cnt = 0;

    for (int i = 0; i < stones.size(); i++)
    {
        if (stones[i] - mid < 0)
            cnt++;
        else
            cnt = 0;

        if (cnt >= k)
            return false;
    }

    return true;
}

int solution(vector<int> stones, int k) 
{
    int answer = 0;

    int left = 1;
    int right = *max_element(stones.begin(), stones.end());

    while (left <= right)
    {
        int mid = (left + right) / 2;

        if (CanAcross(stones, mid, k))
        {
            answer = max(answer, mid);
            left = mid + 1;
        }
        else
            right = mid - 1;
    }

    return answer;
}
```