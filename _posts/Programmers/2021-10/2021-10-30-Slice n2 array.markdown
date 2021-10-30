---
layout: post
title: n^2 배열 자르기
date: 2021-10-30 08:05:55 +0900
categories: Programmers-Leve2
---

# n^2 배열 자르기
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/87390)

# 첫 시도
간단하게 반복문을 돌려서 일단 제출해봤는데 역시 시간이 초과가 떴다.

```c++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, long long left, long long right) 
{
    vector<int> answer;

    vector<vector<int>> arr(n, vector<int>(n, 0));

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j <= i; j++)
        {
            arr[i][i - j] = i + 1;
            arr[i - j][i] = i + 1;
        }
    }

    vector<int> temp;

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < n; j++)
        {
            temp.push_back(arr[i][j]);
        }
    }

    for (int i = left; i <= right; i++)
    {
        answer.push_back(temp[i]);
    }

    return answer;
}
```

# 두번째 시도
맵을 써봤는데 오히려 2중 반복문을 돌렸을때보다 느려졌다. 따라서 시간초과

```c++
#include <iostream>
#include <map>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, long long left, long long right) 
{
    vector<int> answer;

    map<long long, int> hashMap;

    int index = 0;
    for (int i = 0; i < n; i++)
    {
        if (i == 0)
        {
            for (int j = 0; j < n; j++)
            {
                hashMap[index++] = i + j + 1;
            }
        }
        else
        {
            int num = i + 1;
            for (int j = 0; j <= i; j++)
            {
                hashMap[index++] = num;
            }

            num++;

            for (int j = i + 1; j < n; j++)
            {
                hashMap[index++] = num;
                num++;
            }
        }
    }

    for (long long i = left; i <= right; i++)
        answer.push_back(hashMap[i]);

    return answer;
}
```

# 풀이
2차원 배열의 인덱스가 1부터 차근차근 증가한다고 보면 행은 나눗셈의 몫으로 열은 나머지로 나타낼 수 있다. 또한 2차원 배열에 들어간 값은 위에서 구한 행과 열 값에 더 큰 값에 +1을 한 값이다. 따라서 i값이 left부터 right까지 순회한다고 할때 max(i / n, i % n) + 1로 해당 인덱스의 값을 구할 수 있다.

# 코드
```c++
#include <iostream>
#include <algorithm>
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, long long left, long long right) 
{
    vector<int> answer;


    for (long long i = left; i <= right; i++)
    {
        answer.push_back(max(i / n, i % n) + 1);
    }

    return answer;
}
```