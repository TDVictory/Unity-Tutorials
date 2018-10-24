# 时间间隔
Time.deltaTime 是运行中每帧之间的时间间隔

```
using UnityEngine;
using System.Collections;

public class Test : MonoBehaviour
{
    public float speed = 2f; 

    void Update ()
    {
         if(Input.GetKey(KeyCode.RightArrow))
            transform.position += new Vector3(speed * Time.deltaTime, 0.0f, 0.0f);
    }   
}
```

