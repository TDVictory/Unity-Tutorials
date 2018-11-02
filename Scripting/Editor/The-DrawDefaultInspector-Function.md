# DrawDefaultInspector 功能
DrawDefaultInspector功能允许我们只使用一条语句，在自定义的视图上轻松地再建默认的视图。对于我们只想要在视图上增加新物件而不是改变已有的物件时非常有用。

## SomeScript
示例脚本
```
using UnityEngine;
using System.Collections;
using UnityEditor;

public class SomeScript : MonoBehaviour 
{
    public int level;
    public float health;
    public Vector3 target;
}
```

## SomeScriptEditor
编辑器脚本
```
using UnityEngine;
using System.Collections;
using UnityEditor;

[CustomEditor(typeof(SomeScript))]
public class SomeScriptEditor : Editor 
{
    public override void OnInspectorGUI()
    {
        DrawDefaultInspector();

        EditorGUILayout.HelpBox("This is a help box", MessageType.Info);
    }
}
```
通过DrawDefaultInspector()我们不需要再绘制int、float、Vector3三类物件窗口，而后续添加的Helpbox会之间附加在原有的窗口下方。

[返回上一级](/Scripting/Editor.md)

[返回主页](/README.md)
