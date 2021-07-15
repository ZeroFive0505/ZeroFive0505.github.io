---
layout: post
title: 표 편집
date: 2021-07-12 07:13:09 +0900
categories: Programmers-Level3
---

# 표 편집
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/81303)

# 첫 시도
C++에서 기본으로 제공하는 리스트와 최근에 지운 행을 복구할때는 스택을 이용했다. 하지만 순회에 너무 시간이 걸리고 정확도에서 하나 시간초과에 효율성에서는 3개 빼고 전부 다 시간초과가 떴다. 그래서 스택에 `iterator`자체를 저장해보고 했지만 효율성 좋아졌지만 정확도에서 실패와 오류가 너무 떴다.. 아마 값 자체를 지워버려서 복구에 문제가 생기는 것 같다. 그래서 값을 지우는게 아니라 잠시 빼놓는 기능이 있나 인터넷을 확인해봐도 C++에서 제공하는 `list`에는 중간에 있는 값을 지우는게 아니라 빼놓는 것을 제공하지 않았다..

# 코드
```c++
string solution(int n, int k, vector<string> cmd)
{
    string answer;
    for (int i = 0; i < n; i++)
        answer.push_back('X');
    list<int> rows;
    stack<pair<int, int>> rewind;

    for (int i = 0; i < n; i++)
        rows.push_back(i);

    list<int>::iterator it = rows.begin();
    list<int>::iterator temp;

    for (int i = 0; i < k; i++)
        it++;

    for (int i = 0; i < cmd.size(); i++)
    {
        string order = cmd[i];

        if (order.size() == 1)
        {
            switch (order[0])
            {
            case 'C':
                temp = it;
                if (*it != 0)
                {
                    temp--;
                    rewind.push({ *temp, *it });
                    temp++;
                }
                else if (*it == 0)
                    rewind.push({ -1, 0 });

                it++;
                if (it == rows.end())
                {
                    for (int j = 0; j < 2; j++)
                        it--;
                }
                rows.erase(temp);
                break;
            case 'Z':
                pair<int, int> t = rewind.top();
                rewind.pop();
                temp = rows.begin();
                if (t.first != -1)
                {
                    while (*temp != t.first)
                        temp++;
                    temp++;
                    rows.insert(temp, t.second);
                }
                else if (t.first == -1)
                    rows.insert(temp, t.second);
                break;
            }
        }
        else
        {
            char c = 0;
            int num = 0;

            for (int j = 0; j < order.size(); j++)
            {
                if (isalpha(order[j]))
                    c = order[j];

                if (isdigit(order[j]))
                    num = num * 10 + order[j] - '0';
            }

            switch (c)
            {
            case 'U':
                for (int j = 0; j < num; j++)
                    it--;
                break;
            case 'D':
                for (int j = 0; j < num; j++)
                    it++;
                break;
            }
        }
    }

    for (it = rows.begin(); it != rows.end(); it++)
        answer[*it] = 'O';

    return answer;
}
```

# 두번째 시도
이번에는 그냥 이중 연결 리스트를 직접 만들었다.. 그 결과 통과했다.. STL이 만능은 아닌거같다.

# 코드
```c++
#include <iostream>
#include <stack>
#include <list>
#include <string>
#include <ctype.h>
#include <vector>

using namespace std;

struct DNode
{
    int data;
    DNode* left;
    DNode* right;

    DNode(int data, DNode* left, DNode* right) : data(data), left(left), right(right) {}
};

struct DList
{
    DNode* dummy;
    DNode* head;
    DNode* tail;

    void Init()
    {
        dummy = new DNode(0, nullptr, nullptr);
        head = nullptr;
        tail = nullptr;
    }

    void PushFront(const int data)
    {
        if (head == nullptr)
        {
            head = new DNode(data, nullptr, nullptr);
            tail = head;
            head->left = dummy;
            head->right = dummy;
            dummy->right = head;
            dummy->left = tail;
        }
        else
        {
            DNode* temp = new DNode(data, nullptr, nullptr);
            dummy->right->left = temp;
            dummy->right = temp;
            temp->left = dummy;
            temp->right = head;
            head = temp;
        }
    }

    void PushAt(DNode* at, DNode* restore)
    {
        restore->left = at;
        restore->right = at->right;
        at->right->left = restore;
        at->right = restore;
    }

    DNode* RemoveAt(DNode* at)
    {
        at->left->right = at->right;
        at->right->left = at->left;

        return at;
    }

    void PushBack(const int data)
    {
        if (tail == nullptr)
        {
            tail = new DNode(data, nullptr, nullptr);
            head = tail;
            tail->left = dummy;
            tail->right = dummy;
            dummy->left = tail;
            dummy->right = tail;
        }
        else
        {
            DNode* temp = new DNode(data, nullptr, nullptr);
            tail->right->left = temp;
            temp->left = tail;
            temp->right = dummy;
            tail->right = temp;
            tail = temp;
        }
    }

    void PrintForward() const
    {
        DNode* cur;

        for (cur = dummy->right; cur != dummy; cur = cur->right)
        {
            cout << cur->data << " ";
        }
        cout << "\n";
    }

    void PrintBackward() const
    {
        DNode* cur;
        for (cur = dummy->left; cur != dummy; cur = cur->left)
        {
            cout << cur->data << " ";
        }
        cout << "\n";
    }
};


string solution(int n, int k, vector<string> cmd)
{
    string answer;
    for (int i = 0; i < n; i++)
        answer.push_back('X');
    DList rows;
    rows.Init();
    stack<pair<DNode*, DNode*>> rewind;

    for (int i = 0; i < n; i++)
        rows.PushBack(i);

    DNode* it = rows.dummy->right;
    DNode* temp;

    for (int i = 0; i < k; i++)
        it = it->right;

    for (int i = 0; i < cmd.size(); i++)
    {
        string order = cmd[i];

        if (order.size() == 1)
        {
            switch (order[0])
            {
            case 'C':
                rewind.push({ it->left, it });
                temp = it;
                it = it->right;
                rows.RemoveAt(temp);
                if (it == rows.dummy)
                    it = it->left;
                break;
            case 'Z':
                pair<DNode*, DNode*> t = rewind.top();
                rewind.pop();
                rows.PushAt(t.first, t.second);
                break;
            }
        }
        else
        {
            char c = 0;
            int num = 0;

            for (int j = 0; j < order.size(); j++)
            {
                if (isalpha(order[j]))
                    c = order[j];

                if (isdigit(order[j]))
                    num = num * 10 + order[j] - '0';
            }

            switch (c)
            {
            case 'U':
                for (int j = 0; j < num; j++)
                    it = it->left;
                break;
            case 'D':
                for (int j = 0; j < num; j++)
                    it = it->right;
                break;
            }
        }
    }

    for (it = rows.dummy->right; it != rows.dummy; it = it->right)
        answer[it->data] = 'O';

    return answer;
}
```