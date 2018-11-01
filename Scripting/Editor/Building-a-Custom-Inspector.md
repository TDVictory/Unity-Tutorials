# Building a Custom Inspector
## LevelScript
在下述脚本中，我们设置了一个公共变量experience，和一个可获取的公共变量Level，Level的值是experience的750分之一，但是在Inspector视图上无法显示Level。
```
sing UnityEngine;
using System.Collections;

public class LevelScript : MonoBehaviour 
{
    public int experience;
    
    public int Level
    {
        get { return experience / 750; }
    }
}
```

我们通过修改编辑器界面来显示Level，编写脚本如下
```
using UnityEngine;
using System.Collections;
using UnityEditor;

[CustomEditor(typeof(LevelScript))]
public class LevelScriptEditor : Editor 
{
    public override void OnInspectorGUI()
    {
        LevelScript myTarget = (LevelScript)target;
        
        myTarget.experience = EditorGUILayout.IntField("Experience", myTarget.experience);
        EditorGUILayout.LabelField("Level", myTarget.Level.ToString());
    }
}
```
首先我们引用了UnityEditor命名空间，并通过CustomEditor属性通知Unity将要编辑LevelScript的界面。同时该类必须从Editor派生。

只要Unity在Inspector中显示编辑器，就会执行OnInspectorGUI中的代码。你可以在这里放置任何GUI代码 - 它就像OnGUI对游戏一样，但是在Inspector中运行。编辑器定义了可用于访问被检查对象的目标属性。这是我们的自定义检查器的样子：
