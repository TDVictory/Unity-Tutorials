# 物体的激活
如何使用SetActive和activeSelf / activeInHierarchy 函数在Hierarchy视图中处理场景内游戏对象的激活与否。

## 物体激活
```
using UnityEngine;
using System.Collections;

public class ActiveObjects : MonoBehaviour
{
    void Start ()
    {
        gameObject.SetActive(false);
    }
}
```
## 状态检测
```
using UnityEngine;
using System.Collections;

public class CheckState : MonoBehaviour
{
    public GameObject myObject;
    
    
    void Start ()
    {
        Debug.Log("Active Self: " + myObject.activeSelf);
        Debug.Log("Active in Hierarchy" + myObject.activeInHierarchy);
    }
}
```


[返回上一级](/Scripting/Beginner-Gameplay-Scripting.md)

[返回主页](/README.md)
