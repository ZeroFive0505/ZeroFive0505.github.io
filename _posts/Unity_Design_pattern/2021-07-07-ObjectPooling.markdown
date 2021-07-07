---
layout: post
title: Unity Object pooling
date: 2021-07-07 16:48:15 +0900
categories: Unity DesignPattern
---

# Unity 오브젝트 풀링 패턴

## 오브젝트 풀링을 이용한 효율적인 메모리 관리
고전게임에서는 지금의 하드웨어와 달리 메모리가 많이 부족했기에 메모리 관리를 효율적으로 해야했다. 이 메모리 관리를 위해 화면에 표시또는 소환이 되는 투사체 등을 정해진 양만큼 써야했다. 가장 핵심이 되는 풀링을 관리하는 코드는 다음과 같다.

# Pool
```C#
....
    private void Start()
    {
        pooledItems = new List<GameObject>();

        foreach(PoolItem item in items)
        {
            for(int i = 0; i < item.amount; i++)
            {   
                // 초기에는 정해진 양만큼 풀에 넣는다.
                GameObject obj = Instantiate(item.prefab);
                obj.SetActive(false);
                pooledItems.Add(obj);
            }
        }
    }

    public GameObject Get(string tag)
    {
        for(int i = 0; i < pooledItems.Count; i++)
        {
            // 만약 풀에서 아이템을 꺼낼 경우에는 태그가 일치하면서 비활성화 된 아이템이 있다면 그 아이템을 반환한다.
            if(!pooledItems[i].activeInHierarchy && pooledItems[i].tag == tag)
            {
                return pooledItems[i];
            }
        }

        foreach(PoolItem item in items)
        {
            // 만약 아이템이 확장이 가능하면
            if(item.prefab.tag == tag && item.expandable)
            {
                // 새롭게 하나 만들고 다시 풀에 넣는다.
                GameObject obj = Instantiate(item.prefab);
                obj.SetActive(false);
                pooledItems.Add(obj);
                return obj;
            }
        }

        return null;
    }
....
```

# Spawner
이제 소환을 하면된다. 풀을 가지고 소환을하는 코드는 다음과 같다.
```C#
    void SpawnBall()
    {
        // 풀에서 해당 태그의 아이템을 가져오고
        GameObject item = Pool.singleton.Get("Ball");
        // 만약 존재한다면 값을 초기화하고 활성화한다.
        if (item != null)
        {
            item.transform.position = transform.position;
            Vector3 randOffset = new Vector3(Random.Range(-transform.localScale.x, transform.localScale.x), 0,
                Random.Range(-transform.localScale.z, transform.localScale.z));
            item.transform.position += randOffset;
            item.SetActive(true);
        }
    }
```

# 결과


![Img](https://user-images.githubusercontent.com/39051679/124718970-9de79400-df41-11eb-8342-1be56c71bd06.jpg)

위와 같은 설정창을 볼 수 있다. 일단 총합은 10개 그리고 확장은 불가능하게 해놓고 결과를 보면 다음과 같다.

![Img](https://user-images.githubusercontent.com/39051679/124719339-fdde3a80-df41-11eb-8199-909847436216.gif)

![Img](https://user-images.githubusercontent.com/39051679/124719425-151d2800-df42-11eb-93c2-e8be34428fa0.jpg)

최대 공은 10개 밖에 소환이안되며 매 0.1초마다 소환을 하고 싶어도 풀에 제한이 걸려있어서 소환이 불가능하다.

다음으로는 확장이 가능할 경우이다.

![Img](https://user-images.githubusercontent.com/39051679/124719851-8a88f880-df42-11eb-80a8-02447315eb4b.gif)

확장이 가능해지면 무수히 많은 공이 소환되는 것을 볼 수 있다.

![Img](https://user-images.githubusercontent.com/39051679/124720049-b3a98900-df42-11eb-8a45-75c85f2a890d.gif)

확장이 불가능했을때와 달리 순식간에 늘어나는 풀의 용량을 볼 수 있다. 처음에는 빠르게 증가하지만 어느정도 풀이 커지면 늘어나는 속도가 많이 줄어드는 것을 볼 수 있었다.