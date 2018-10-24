# 基础生命周期函数
在Unity内创建C#脚本，自动生成的初始脚本中会自动继承MonoBehavior类，并提供给我们两个函数方法Start和Update。
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Test : MonoBehaviour {

	// Use this for initialization
	void Start () {
		
	}
	
	// Update is called once per frame
	void Update () {

	}
}
```
在开始讲解前，我们首先在按照如下编写脚本并保存。
```
	void Start () {
        Debug.Log("Start");
	}
	
	void Update () {
        Debug.Log("Update");
    }
```
然后将我们将该脚本挂载在场景内随意物体上，运行并观察运行结果。
再添加Awake方法，运行并观察运行结果。
```
    void Awake()
    {
        Debug.Log("Awake");
    }
```
最后我们再修改一下Update内的内容，
并添加FixUpdate方法，运行并观察运行结果。
```
    void FixedUpdate ()
    {
        Debug.Log("FixedUpdate time :" + Time.deltaTime);
    }   
    
    void Update ()
    {
        Debug.Log("Update time :" + Time.deltaTime);
    }   
```

对照Unity的生命周期图思考一下每个函数是用来做什么的？

![](/Image/Unity生命周期图.jpg)

[返回上一级](/Scripting/Beginner-Gameplay-Scripting.md)

[返回主页](/README.md)

