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




[返回上一级](/Scripting/Editor.md)

[返回主页](/README.md)
