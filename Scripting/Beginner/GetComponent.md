# 组件的获取
如何使用GetComponent函数来获取并调用其他脚本或组件的属性。在场景中分别创建物体A和物体B，物体A挂载UsingOtherComponents脚本以及MyScript脚本，物体B挂载AnotherScript脚本。

## UsingOtherComponents

```
using UnityEngine;
using System.Collections;

public class UsingOtherComponents : MonoBehaviour
{
    public GameObject otherGameObject;    
    private MyScript MyScript;
    private AnotherScript anotherScript;
    
    void Awake ()
    {
        MyScript = GetComponent<MyScript>();
        AnotherScript = otherGameObject.GetComponent<AnotherScript>();
    }
    
    
    void Start ()
    {
        Debug.Log("My score is " + MyScript.playerScore);
        Debug.Log("Other player's score is " + AnotherScript.playerScore);
        AnotherScript.ShowScore();
    }
}
```
## MyScript
```
using UnityEngine;
using System.Collections;

public class MyScript : MonoBehaviour
{
    public int playerScore = 100;
}
```
## AnotherScript
```
using UnityEngine;
using System.Collections;

public class AnotherScript : MonoBehaviour
{
    public int playerScore = 90;
    
    public void ShowScore(){
      Debug.Log("Score is " + playerScore);
    }
}
```

[返回上一级](/Scripting/Beginner-Gameplay-Scripting.md)

[返回主页](/README.md)
