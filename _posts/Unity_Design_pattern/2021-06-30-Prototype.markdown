---
layout: post
title: Unity Prototype pattern
date: 2021-06-30 13:57:18 +0900
categories: Unity
---

# Unity Prototype 패턴

## 자가복제를 하는 프로토 타입 패턴

Flyweight 패턴과 많이 유사하다. 자가복제를 한다는 말은 즉 원본이되는 객체를 하나 만들면 동일한 객체를 만들때 계속해서 만들어둔 객체 복사해서 쓰는것이다.

![Image](https://user-images.githubusercontent.com/39051679/123902771-63fa1900-d9a8-11eb-82de-34e3ab11ee86.gif)

간단하게 마우스를 클릭할때 마다 구를 생성하는 장면이다.
구를 생성하는 코드가 중요한데 다음과 같다.

```c#
   // 하나의 유일한 구.
    static GameObject sphere;

    public static GameObject GenerateSphere(Vector3 pos)
    {
        // 만약 구가 아직 생성되지 않았다면 새로 만든다.
        if(sphere == null)
        {
            CreateSphere(Vector3.zero);
            sphere.SetActive(false);
        }

        // 아래의 코드는 이미 만들어진 구로부터 데이터를 가져오는 과정.
        GameObject sphereClone = new GameObject();
        sphereClone.AddComponent<MeshFilter>();
        sphereClone.AddComponent<MeshRenderer>();
        sphereClone.GetComponent<MeshFilter>().sharedMesh = sphere.GetComponent<MeshFilter>().sharedMesh;
        MeshRenderer renderer = sphereClone.GetComponent<MeshRenderer>();
        renderer.sharedMaterial = sphere.GetComponent<MeshRenderer>().sharedMaterial;
        sphereClone.AddComponent<Rigidbody>();
        sphereClone.AddComponent<SphereCollider>();
        sphereClone.name = "Sphere(Clone)";
        sphereClone.SetActive(true);
        sphereClone.transform.position = pos;
        sphereClone.GetComponent<Rigidbody>().AddForce(new Vector3(Random.Range(-1, 1), Random.Range(-1, 1), Random.Range(-1, 1)));
        return sphereClone;
    }
    .....
```

# 결과

![Image](https://user-images.githubusercontent.com/39051679/123902933-b3404980-d9a8-11eb-9565-74f67134e104.jpg)
![Image](https://user-images.githubusercontent.com/39051679/123902937-b4717680-d9a8-11eb-98cd-f230f8dbe2b0.jpg)

이런식으로 비록 위치 정보는 좀 달르고 생성된 시간도 다르지만 메쉬는 같은 데이터를 쓰고있다.

![Image](https://user-images.githubusercontent.com/39051679/123903044-e551ab80-d9a8-11eb-9d7b-a2676bff31cc.gif)

마지막으로 유니티의 에디터를 커스텀해서 간단하게 구를 소환, 저장할 수 있게 만들었다. 이 또한 처음에 원본이 되는 구가 없을 시에는 새롭게 하나를 생성하고 그 이후에는 같은 구를 계속해서 쓴다.

![Image](https://user-images.githubusercontent.com/39051679/123903108-07e3c480-d9a9-11eb-8615-65df7d920cfd.jpg)

위와같이 구의 정보가 저장되었다.