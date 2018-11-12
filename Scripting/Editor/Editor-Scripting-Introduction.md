# 编辑器脚本介绍
你可以使用Unity内的编辑器脚本来使得让您的游戏设计师甚至自己更轻松。通过一小段代码，你可以将一些较繁琐的视图窗口使用自动化来配置你的行为状态，并且在配置改变时提供可视化的反馈。我们创建一个简单的抛射系统来延时一些可以被编辑器编写的部分。

在这篇教程中
- 如何将方法展现在视图窗口中。
- 如何使用控件生成自定义图标。
- 如何使用字段属性来自定义视图窗口。

## Get Started with Simple Techniques（从简单技术开始）
我们先创建一个Projectile类，使得玩家能分配Rigidbody字段，也提供了物理模拟行为。然后我们将扩展这个类来使得我们更容易使用。

```
public class Projectile : MonoBehaviour
{
    public Rigidbody rigidbody;
}
```

当你将上述组件添加至GameObject上，你也会需要一个刚体组件（Rigidbody component）。我们可以通过使用RequireComponent属性来自动添加刚体组件（当首次添加该组件且物体下没有刚体组件时）。

```
[RequireComponent(typeof(Rigidbody))]
public class Projectile : MonoBehaviour
{
    public Rigidbody rigidbody;
}
```

为了更好的效果，我们自动将刚体组件分配到Rigidbody字段。我们使用Reset方法执行此操作，该方法在您首次将组件添加到GameObject时调用。您还可以通过右键单击Inspectoer视图中的组件设置手动选择“Reset”菜单项来调用Reset方法。

```
[RequireComponent(typeof(Rigidbody))]
public class Projectile : MonoBehaviour
{
    public Rigidbody rigidbody;
    void Reset()
    {
        rigidbody = GetComponent<Rigidbody>();
    }
}
```

最后，我们可以通过使用HideInInspector属性隐藏刚体字段，以此来最小化检查器GUI占用的宝贵屏幕空间。我们还可以通过在字段声明中使用'new'关键字来删除编辑器警告。

```
[RequireComponent(typeof(Rigidbody))]
public class Projectile : MonoBehaviour
{
    [HideInInspector] new public Rigidbody rigidbody;
    void Reset()
    {
        rigidbody = GetComponent<Rigidbody>();
    }
}
```

这些都是非常简单的技术，您可以在所有组件上使用这些技术来保持干净整洁，并最大限度地减少配置错误。

## Simple Inspector Customisation（简单的检视窗口自定义）
![](/Image/Scripting/Editor/an-introduction-to-editor-scripting-0.png)

我们来看下一个类，Launcher类。它生成了一个新的名为Projectile的刚体组件，并且将其的速度进行了修改，这样它会以一个特定的速度发射。（实际上它会发射任何具有刚体组件的预制体）。

```
public class Launcher : MonoBehaviour
{
    public Rigidbody projectile;
    public Vector3 offset = Vector3.forward;
    public float velocity = 10;

    public void Fire()
    {
        var body = Instantiate(
            projectile, 
            transform.TransformPoint(offset), 
            transform.rotation);
        body.velocity = Vector3.forward * velocity;
    }
}
```

我们做的第一件事就是在检视视图GUI上创建一个滑条，这样我们就能给velocity（速度）字段一个范围属性。设计者就可以迅速滑动并改变其值来测试不同速度的影响，抑或是输入一个确切数值。我们给“Fire”字段一个快捷菜单属性，这样我们就可以通过右键检视窗口中组件头标来运行该方法。您也可以使用任何方法（只要它具有零参数）来向组件添加编辑器功能。

```
public class Launcher : MonoBehaviour
{
    public Rigidbody projectile;
    public Vector3 offset = Vector3.forward;
    [Range(0, 100)] public float velocity = 10;
        
    [ContextMenu("Fire")]
    public void Fire()
    {
        var body = Instantiate(
            projectile, 
            transform.TransformPoint(offset), 
            transform.rotation);
        body.velocity = Vector3.forward * velocity;
    }
}
```

从这个例子说开来去，我们需要编写一个Editor类来扩展Launcher组件的功能。这个类具有CustomEditor属性用来告诉Unity这个自定义的编辑器将运用于哪个组件。当场景被渲染时，OnSceneGUI方法被调用，通过该方法允许我们在Scene视图中绘制一些小物体。正因为它是一个编辑器类，**它必须被放置在你项目内某个名字为“Editor”的文件夹内**。

```
using UnityEditor;

[CustomEditor(typeof(Launcher))]
public class LauncherEditor : Editor
{
    void OnSceneGUI()
    {
        var launcher = target as Launcher;
    }
}
```

让我们添加OnSceneGUI方法，以便我们可以有一个小部件，来允许我们显示和调整场景视图内的偏移位置（Launcher脚本中的offset）。因为偏移是相对于父变换存储的，所以我们需要使用Transform.InverseTransformPoint和Transform.TransformPoint方法将偏移位置转换为世界空间以供Handles.PositionHandle方法使用，并返回到本地空间来存储偏移字段。

```
using UnityEditor;

[CustomEditor(typeof(Launcher))]
public class LauncherEditor : Editor
{
    void OnSceneGUI()
    {
        var launcher = target as Launcher;
        var transform = launcher.transform;
        launcher.offset = transform.InverseTransformPoint(
            Handles.PositionHandle(
                transform.TransformPoint(launcher.offset), 
                transform.rotation));
    }
}
```

我们也同样创建一个自定义的Projectile编辑器类。让我们先给Projectile类添加一个damageRadius字段，这将被用于计算其他游戏物体受到抛射器的影响。

```
[RequireComponent(typeof(Rigidbody))]
public class Projectile : MonoBehaviour
{
    [HideInInspector] new public Rigidbody rigidbody;
    public float damageRadius = 1;
    
    void Reset()
    {
        rigidbody = GetComponent<Rigidbody>();
    }
}
```

我们可能想要在damageRadius字段中添加一个简单的Range属性，但是我们可以通过在场景视图中可视化这个字段来做得更好。我们为Projectile组件创建了另一个Editor类，并使用Handles.RadiusHandle可视化该字段，并允许在场景视图中对其进行调整。

```
using UnityEditor;

[CustomEditor(typeof(Projectile))]
public class ProjectileEditor : Editor
{
    void OnSceneGUI()
    {
        var projectile = target as Projectile;
        var transform = projectile.transform;
        projectile.damageRadius = Handles.RadiusHandle(
            transform.rotation, 
            transform.position, 
            projectile.damageRadius);
    }
}

```

我们还应该添加一个Gizmo，这样我们就可以在没有可渲染几何体的情况下在场景视图中看到它。这里我们使用DrawGizmo属性来指定一个方法，用于绘制Projectile类的Gizmo。这也可以通过在Projectile类本身中实现OnDrawGizmos和OnDrawGizmosSelected来完成，但是在可能的情况下将编辑器功能与游戏功能分开是更好的做法，因此我们使用DrawGizmo属性。

```
using UnityEditor;

[CustomEditor(typeof(Projectile))]
public class ProjectileEditor : Editor
{
    [DrawGizmo(GizmoType.Selected | GizmoType.NonSelected)]
    static void DrawGizmosSelected(Projectile projectile, GizmoType gizmoType)
    {
        Gizmos.DrawSphere(projectile.transform.position, 0.125f);
    }
    
    void OnSceneGUI()
    {
        var projectile = target as Projectile;
        var transform = projectile.transform;
        projectile.damageRadius = Handles.RadiusHandle(
            transform.rotation, 
            transform.position, 
            projectile.damageRadius);
    }
}
```

## Widgets in the Scene View（场景视图内的窗口小部件）
![](/Image/Scripting/Editor/an-introduction-to-editor-scripting-2.png)

我们也可以在OnSceneGUI中使用Editor IMGUI方法，以此来创建任意种类的场景视图编辑器控制。我们将要使用一个按键中在场景视图中开放Launcher组件的Fire方法。我们计算出屏幕空间上偏右的矩形空间对应的世界坐标系，将在该处绘制GUI。当然，我们想要在，只有在游戏运行时，而不是编辑模式下调用Fire方法。因此我们将Fire方法的响应包装进**EditorGUI.DisabledGroupScope**之中，这代表了按钮只有在运行模式下才会被激活。

```
using UnityEditor;

[CustomEditor(typeof(Launcher))]
public class LauncherEditor : Editor
{
    void OnSceneGUI()
    {
        var launcher = target as Launcher;
        var transform = launcher.transform;
        launcher.offset = transform.InverseTransformPoint(
            Handles.PositionHandle(
                transform.TransformPoint(launcher.offset), 
                transform.rotation));
        Handles.BeginGUI();
        var rectMin = Camera.current.WorldToScreenPoint(
            launcher.transform.position + 
            launcher.offset);
        var rect = new Rect();
        rect.xMin = rectMin.x;
        rect.yMin = SceneView.currentDrawingSceneView.position.height - 
            rectMin.y;
        rect.width = 64;
        rect.height = 18;
        GUILayout.BeginArea(rect);
        using (new EditorGUI.DisabledGroupScope(!Application.isPlaying))
        {
            if (GUILayout.Button("Fire"))
                launcher.Fire();
        }
        GUILayout.EndArea();
        Handles.EndGUI();
    }
}
```

游戏设计中的物理学很难调试，所以让我们为设计师添加一个助手，它可以显示飞行时间1秒后射弹的位置。我们需要弹丸的质量来计算这个位置，因此我们在尝试计算之前检查刚体字段是否为空。为了清晰起见，我们还从启动器对象到偏移位置绘制了一条虚线（使用Handles.DrawDottedLine），让设计人员知道此位置句柄正在修改偏移字段，而不是变换位置。我们还使用Handles.Label为偏移句柄添加标签。

这是使用具有DrawGizmo属性的方法完成的，方法与ProjectileEditor相同。我们还添加了一个Undo.RecordObject调用，在EditorGUI.ChangeCheckScope的帮助下，我们可以在更改偏移时记录撤消操作。（如果您之前没有看过using语句，可以在MSDN上阅读它。）

![](/Image/Scripting/Editor/an-introduction-to-editor-scripting-3.png)

```using UnityEditor;

[CustomEditor(typeof(Launcher))]
public class LauncherEditor : Editor
{
    [DrawGizmo(GizmoType.Pickable | GizmoType.Selected)]
    static void DrawGizmosSelected(Launcher launcher, GizmoType gizmoType)
    {
        var offsetPosition = launcher.transform.position + launcher.offset;
        Handles.DrawDottedLine(launcher.transform.position, offsetPosition, 3);
        Handles.Label(offsetPosition, "Offset");
        if (launcher.projectile != null)
        {
            var endPosition = offsetPosition + 
                (launcher.transform.forward * 
                launcher.velocity / 
                launcher.projectile.mass);
            using (new Handles.DrawingScope(Color.yellow))
            {
                Handles.DrawDottedLine(offsetPosition, endPosition, 3);
                Gizmos.DrawWireSphere(endPosition, 0.125f);
                Handles.Label(endPosition, "Estimated Position");
            }
        }
    }

    void OnSceneGUI()
    {
        var launcher = target as Launcher;
        var transform = launcher.transform;

            using (var cc = new EditorGUI.ChangeCheckScope())
            {
               var newOffset = transform.InverseTransformPoint(

               Handles.PositionHandle(
                   transform.TransformPoint(launcher.offset),
                   transform.rotation));

               if(cc.changed)
               {
                   Undo.RecordObject(launcher, "Offset Change");
                   launcher.offset = newOffset;
               }
           }

        Handles.BeginGUI();
        var rectMin = Camera.current.WorldToScreenPoint(
            launcher.transform.position + 
            launcher.offset);
        var rect = new Rect();
        rect.xMin = rectMin.x;
        rect.yMin = SceneView.currentDrawingSceneView.position.height - 
            rectMin.y;
        rect.width = 64;
        rect.height = 18;
        GUILayout.BeginArea(rect);
        using (new EditorGUI.DisabledGroupScope(!Application.isPlaying))
        {
            if (GUILayout.Button("Fire"))
                launcher.Fire();
        }
        GUILayout.EndArea();
        Handles.EndGUI();
    }
}
}
```

![](/Image/Scripting/Editor/an-introduction-to-editor-scripting-4.png)


如果你在编辑器中进行尝试，你将会发现位置估计并不准确。让我们考虑重力并改变计算，通过一秒的飞行时间来轨迹绘制Handles.DrawAAPolyLine和Gizmos.DrawWireSphere的弯曲路径。如果我们使用Handles.DrawingScope来更改小部件的颜色，我们不需要担心在方法完成时将其设置回先前的颜色。

```
[DrawGizmo(GizmoType.Pickable | GizmoType.Selected)]
static void DrawGizmosSelected(Launcher launcher, GizmoType gizmoType)
{
{
    var offsetPosition = launcher.transform.TransformPoint(launcher.offset);
    Handles.DrawDottedLine(launcher.transform.position, offsetPosition, 3);
    Handles.Label(offsetPosition, "Offset");
    if (launcher.projectile != null)
    {
        var positions = new List<Vector3>();
        var velocity = launcher.transform.forward * 
            launcher.velocity / 
            launcher.projectile.mass;
        var position = offsetPosition;
        var physicsStep = 0.1f;
        for (var i = 0f; i <= 1f; i += physicsStep)
        {
            positions.Add(position);
            position += velocity * physicsStep;
            velocity += Physics.gravity * physicsStep;
        }
        using (new Handles.DrawingScope(Color.yellow))
        {
            Handles.DrawAAPolyLine(positions.ToArray());
            Gizmos.DrawWireSphere(positions[positions.Count - 1], 0.125f);
            Handles.Label(positions[positions.Count - 1], "Estimated Position (1 sec)");
      }
    }
}
}
```

  

[返回上一级](/Scripting/Editor.md)

[返回主页](/README.md)
