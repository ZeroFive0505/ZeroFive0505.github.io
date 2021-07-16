---
layout: post
title: 블록 게임
date: 2021-07-05 07:49:34 +0900
categories: Programmers-Level4
---

# 블록 게임
## 문제 링크 -> [Programmers](https://programmers.co.kr/learn/courses/30/lessons/42894)


# 첫 시도
블록이 위에서 내려온다면 특정 모양 빼고는 조건을 만족하지 않는다고 생각해서 각각 모양과 비교해서 만약 조건을 만족한다면 해당 블록의 영역을 0으로 없애는 방법으로 풀어봤다.. 결과는 역시나 대참사.

# 코드
```c++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

vector<vector<int>> shape1 = {
    {1, 0, 0},
    {1, 1, 1}
};

vector<vector<int>> shape2 = {
    {0, 1},
    {0, 1},
    {1, 1}
};

vector<vector<int>> shape3 = {
    {1, 0},
    {1, 0},
    {1, 1}
};

vector<vector<int>> shape4 = {
    {0, 0, 1},
    {1, 1, 1}
};

vector<vector<int>> shape5 = {
    {0, 1, 0},
    {1, 1, 1}
};

bool VerticalCheck(vector<vector<int>>& board, int y, int x)
{
    bool hit = false;
    while (1)
    {
        if (y <= -1)
            break;


        if (board[y][x] != 0)
        {
            hit = true;
            break;
        }

        y--;
    }

    return hit;
}

bool Check(vector<vector<int>>& board, int y, int x)
{
    bool check = true;
    for (int i = 0; i < shape1.size(); i++)
    {
        for (int j = 0; j < shape1[0].size(); j++)
        {
            if (y + i >= board.size() || x + j >= board[0].size())
            {
                check = false;
                break;
            }

            if (shape1[i][j] != 0 && board[y + i][x + j] != 0)
                continue;

            if (shape1[i][j] == 0 && board[y + i][x + j] == 0)
            {
                if (VerticalCheck(board, y + i, x + j))
                {
                    check = false;
                    break;
                }
            }
        }

        if (!check)
            break;
    }

    if (check)
    {
        for (int i = 0; i < shape1.size(); i++)
        {
            for (int j = 0; j < shape1[0].size(); j++)
            {
                board[y + i][x + j] = 0;
            }
        }

        return true;
    }

    check = true;

    for (int i = 0; i < shape2.size(); i++)
    {
        for (int j = 0; j < shape2[0].size(); j++)
        {
            if (y + i >= board.size() || x + j >= board[0].size())
            {
                check = false;
                break;
            }

            if (shape2[i][j] != 0 && board[y + i][x + j] != 0)
                continue;

            if (shape2[i][j] == 0 && board[y + i][x + j] == 0)
            {
                if (VerticalCheck(board, y + i, x + j))
                {
                    check = false;
                    break;
                }
            }
        }

        if (!check)
            break;
    }

    if (check)
    {
        for (int i = 0; i < shape2.size(); i++)
        {
            for (int j = 0; j < shape2[0].size(); j++)
            {
                board[y + i][x + j] = 0;
            }
        }

        return true;
    }

    check = true;

    for (int i = 0; i < shape3.size(); i++)
    {
        for (int j = 0; j < shape3[0].size(); j++)
        {
            if (y + i >= board.size() || x + j >= board[0].size())
            {
                check = false;
                break;
            }

            if (shape3[i][j] != 0 && board[y + i][x + j] != 0)
                continue;


            if (shape3[i][j] == 0 && board[y + i][x + j] == 0)
            {
                if (VerticalCheck(board, y + i, x + j))
                {
                    check = false;
                    break;
                }
            }
        }

        if (!check)
            break;
    }

    if (check)
    {
        for (int i = 0; i < shape3.size(); i++)
        {
            for (int j = 0; j < shape3[0].size(); j++)
            {
                board[y + i][x + j] = 0;
            }
        }

        return true;
    }

    check = true;

    for (int i = 0; i < shape4.size(); i++)
    {
        for (int j = 0; j < shape4[0].size(); j++)
        {
            if (y + i >= board.size() || x + j >= board[0].size())
            {
                check = false;
                break;
            }

            if (shape4[i][j] != 0 && board[y + i][x + j] != 0)
                continue;

            
            if (shape4[i][j] == 0 && board[y + i][x + j] == 0)
            {
                if (VerticalCheck(board, y + i, x + j))
                {
                    check = false;
                    break;
                }
            }
        }

        if (!check)
            break;
    }


    if (check)
    {
        for (int i = 0; i < shape4.size(); i++)
        {
            for (int j = 0; j < shape4[0].size(); j++)
            {
                board[y + i][x + j] = 0;
            }
        }

        return true;
    }

    check = true;


    for (int i = 0; i < shape5.size(); i++)
    {
        for (int j = 0; j < shape5[0].size(); j++)
        {
            if (y + i >= board.size() || x + j >= board[0].size())
            {
                check = false;
                break;
            }

            if (shape5[i][j] != 0 && board[y + i][x + j] != 0)
                continue;


            if (shape5[i][j] == 0 && board[y + i][x + j] == 0)
            {
                if (VerticalCheck(board, y + i, x + j))
                {
                    check = false;
                    break;
                }
            }
        }

        if (!check)
            break;
    }


    if (check)
    {
        for (int i = 0; i < shape5.size(); i++)
        {
            for (int j = 0; j < shape5[0].size(); j++)
            {
                board[y + i][x + j] = 0;
            }
        }

        return true;
    }


    return false;
}

int solution(vector<vector<int>> board) 
{
    int answer = 0;

    for (int i = 0; i < board.size(); i++)
    {
        for (int j = 0; j < board[0].size(); j++)
        {
            if (board[i][j] != 0)
            {
                if (Check(board, i, j))
                    answer++;
            }
        }
    }

    return answer;
}

```

# 풀이
인터넷에서 다른 사람들의 풀이법을 참고해봤다. 

### [Youtube](https://www.youtube.com/watch?v=YqiqrhRSi54)

간단하게 2X3, 3X2 형태의 직사각형 만든 다음에 빈곳이 있다면 위에서부터 채울 수 있는지 또는 빈칸은 2곳인지를 체크하고 같은 숫자로 이루어졌는지를 계속 확인한다. 그렇게 검사가 통과되면 0으로 채워서 지우고 다시 배열의 왼쪽 위부터 다시 시작해서 갯수를 센다.

# 코드
```C++
#include <iostream>
#include <string>
#include <vector>

using namespace std;

int N;
vector<vector<int>> Board;

bool CanFill(int row, int col)
{
    for (int i = 0; i < row; i++)
    {
        if (Board[i][col] != 0)
            return false;
    }

    return true;
}

bool Find(int row, int col, int h, int w)
{
    int emptyCnt = 0;
    int lastValue = -1;

    for (int r = row; r < row + h; r++)
    {
        for (int c = col; c < col + w; c++)
        {
            if (Board[r][c] == 0)
            {
                if (!CanFill(r, c))
                    return false;
                emptyCnt++;
                if (emptyCnt > 2)
                    return false;
            }
            else
            {
                if (lastValue != -1 && lastValue != Board[r][c])
                    return false;
                lastValue = Board[r][c];
            }
        }
    }

    for (int r = row; r < row + h; r++)
    {
        for (int c = col; c < col + w; c++)
        {
            Board[r][c] = 0;
        }
    }

    return true;
}

int solution(vector<vector<int>> board) 
{
    Board = board;
    N = board.size();
    int answer = 0;
    int cnt;

    do
    {
        cnt = 0;

        for (int i = 0; i < N; i++)
        {
            for (int j = 0; j < N; j++)
            {
                if (i <= N - 2 && j <= N - 3 && Find(i, j, 2, 3))
                {
                    cnt++;
                }
                else if(i <= N - 3 && j <= N - 2 && Find(i, j, 3, 2))
                {
                    cnt++;
                }
            }
        }

        answer += cnt;

    } while (cnt);

    return answer;
}
```