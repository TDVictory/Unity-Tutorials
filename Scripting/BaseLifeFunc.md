# 基础生命周期函数
在Unity内创建C#脚本，自动生成的初始脚本中会自动继承MonoBehavior类，并提供给两个函数方法Start和Update。
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
## Start与Update
```
void Start () {
        Debug.Log("Start");
}
	
void Update () {
        Debug.Log("Update");
}
```
## Awake
```
    void Awake()
    {
        Debug.Log("Awake");
    }
```
## FixUpdate
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

## Unity的生命周期图

![](/Image/Unity生命周期图.jpg)

[返回上一级](/Scripting/Beginner-Gameplay-Scripting.md)

[返回主页](/README.md)

