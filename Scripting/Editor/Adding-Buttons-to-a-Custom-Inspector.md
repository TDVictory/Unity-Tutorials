# 为自定义视图添加按键
在Unity中，我们可以给编辑器窗口添加按键，因此我们可以从我们的脚本中调用函数。如此做可以给我们编写脚本来强化我们的工作流程。

## ObjectBuilderScript
```
using UnityEngine;
using System.Collections;

public class ObjectBuilderScript : MonoBehaviour 
{
    public GameObject obj;
    public Vector3 spawnPoint;

    
    public void BuildObject()
    {
        Instantiate(obj, spawnPoint, Quaternion.identity);
    }
}
```
## ObjectBuilderEditor
```
using UnityEngine;
using System.Collections;
using UnityEditor;

[CustomEditor(typeof(ObjectBuilderScript))]
public class ObjectBuilderEditor : Editor
{
    public override void OnInspectorGUI()
    {
        DrawDefaultInspector();
        
        ObjectBuilderScript myScript = (ObjectBuilderScript)target;
        if(GUILayout.Button("Build Object"))
        {
            myScript.BuildObject();
        }
    }
}
```

[返回上一级](/Scripting/Editor.md)

[返回主页](/README.md)
