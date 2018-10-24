# 延时调用

## Invoke
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Test : MonoBehaviour
{
    public GameObject target;

    void Start()
    {
        Invoke("SpawnObject", 2f);
    }

    void SpawnObject()
    {
        Instantiate(target, transform.position, transform.rotation);
    }
}
```
## InvokeRepeating
```
using UnityEngine;
using System.Collections;

public class InvokeRepeating : MonoBehaviour 
{
    public GameObject target;
    
    
    void Start()
    {
        InvokeRepeating("SpawnObject", 2, 1);
    }
    
    void SpawnObject()
    {
        Instantiate(target, transform.position, transform.rotation);
    }
}
```
[返回上一级](/Scripting/Beginner-Gameplay-Scripting.md)

[返回主页](/README.md)
