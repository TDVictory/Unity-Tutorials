# 生成与销毁
通过使用Instantiate在运行时创建克隆预制体（Prefab）。
通过使用Destroy在运行时销毁物体。

## 生成物体
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Test : MonoBehaviour
{
    public Rigidbody projectile;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            Rigidbody clone;
            clone = Instantiate(projectile, transform.position, transform.rotation);
            clone.velocity = transform.TransformDirection(Vector3.forward * 10);
        }
    }
}
```
## 销毁物体
```
    void Start()
    {
        Destroy (gameObject, 3f);
    }
```

[返回上一级](/Scripting/Beginner-Gameplay-Scripting.md)

[返回主页](/README.md)
