# 组件的启用
修改enabled的值（True/False）来开启和关闭组件。
```
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour
{
    public Light myLight;    
    
    void Start ()
    {
        myLight = GetComponent<Light>();
    }
    
    
    void Update ()
    {
        if(Input.GetKeyUp(KeyCode.Space))
        {
            myLight.enabled = !myLight.enabled;
        }
    }
}
```
[返回上一级](/Scripting/Beginner-Gameplay-Scripting.md)

[返回主页](/README.md)
