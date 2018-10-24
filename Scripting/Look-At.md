# 朝向
通过**LookAt**函数来使一个物体面朝向另一个物体
```
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour
{
    public Transform target;
    
    void Update ()
    {
        transform.LookAt(target);
    }
}
```
[返回上一级](/Scripting/Beginner-Gameplay-Scripting.md)

[返回主页](/README.md)
