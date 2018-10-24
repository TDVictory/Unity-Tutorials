# 按键响应
通过GetKey、GetButton、GetAxis获取按键响应。

GetKey、GetButton其后缀为Down则为按下瞬间为真，后缀为Up则为弹起瞬间为真，无后缀则按下期间均为真.

GetAxis其后缀为Raw时只会返回-1，0，1值，无后缀则会对输出值进行一个平滑处理
## GetKey
```
using UnityEngine;

public class Test : MonoBehaviour {

    void Update()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            Debug.Log("GetKey");
        }
    }
}
```
## GetButton
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Test : MonoBehaviour {

    void Update()
    {
        if (Input.GetButton("Jump"))
        {
            Debug.Log("GetButton");
        }
    }
}
```
## GetAxis
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Test : MonoBehaviour {

    void Update()
    {
        Debug.Log(Input.GetAxis("Horizontal"));
    }
}
```

[返回上一级](/Scripting/Beginner-Gameplay-Scripting.md)

[返回主页](/README.md)
