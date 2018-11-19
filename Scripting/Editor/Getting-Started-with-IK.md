# IK入门
逆向运动学（IK）是一个计算为了使最终关节到达某个位置，所引发的一系列关节旋转的过程。举个例子，IK可以被用来计算肩膀，手肘，手腕这些用于让手指到达空间中某个点所引发的旋转。

在Unity中，有须有中创建IK解决方案的方法，这些都不是很好理解。然而，有时候只需要一个简单的解决方案即可获得合理的最终结果。这篇教程包含了一个针对四肢这样3到4个关节的简单解决方案，并且展示如何通过一些自定义的Unity编辑器脚本，来支撑开发者使用该解决方案。

## Setting up a test scene for development（设置测试场景以进行开发）
您将需要一个臂段模型，其设置的前轴与单位前轴匹配（x：0，y：0，z：1），枢轴点设置为关节应旋转的相同位置。Segment.fbx在满足这些要求的示例项目中提供~~然而我没有找到在哪有示例项目~~。

![](/Image/Scripting/Editor/getting-started-with-ik-0.png)

*模型的正确位置与朝向*

创建一个空的游戏对象，作为你手臂的根节点。创建第二个空的子游戏对象，作为主要枢轴。然后，附加三个段来创建一个手臂。最后，在层次结尾处创建一个空的GameObject，并将其命名为“Tip”。这个位置就是我们用于触及目标位置的点。

![](/Image/Scripting/Editor/getting-started-with-ik-1.png)

*Hierarchy视图设置*

使用场景视图中的位置控制柄，布置游戏对象，使其类似于机械臂，类似于下面的屏幕截图。注意“tip”游戏物体位于最后一个段的最后。

![](/Image/Scripting/Editor/getting-started-with-ik-2.png)

现在可以保存的场景了

## Writing Some Code!（编写代码）
### The actual IK solver（实际的IK解算器）
我们的IK算法将使用余弦定律和一些欺骗性伎俩来计算出最终的effector（最后一个臂段）对应角度。effector是tip的transform父级。pivot是根之后的第一个transform。这在我们的链中间留下了两个transform。如果我们手动定位pivot和effector，我们可以使用余弦定律来计算两个中间transform的正确旋转。

### The Monobehaviour class(Monobehaviour类)
创建一个名为SimpleIKSolver.cs的脚本。我们需要添加用于控制我们transform链的字段，以及用于计算的数据。

```
public class SimpleIKSolver : MonoBehaviour
{

    public Transform pivot, upper, lower, effector, tip;
    public Vector3 target = Vector3.forward;
    public Vector3 normal = Vector3.up;




    float upperLength, lowerLength, effectorLength, pivotLength;
    Vector3 effectorTarget, tipTarget;
}
```

这个行为类需要绑定在pivot游戏物体上。但是你在开始干之前，我们需要创建一个方法来使得设置组件视图不那么难受。为你的类添加下述方法，然后把该类作为pivot游戏物体的组件。
```
void Reset()
{
    pivot = transform;
    try
    {
        upper = pivot.GetChild(0);
        lower = upper.GetChild(0);
        effector = lower.GetChild(0);
        tip = effector.GetChild(0);
    }
    catch (UnityException)
    {
        Debug.Log("Could not find required transforms, please assign manually.");
    }
}
```

当组件第一次附加到一个游戏物体上时，这个方法将自动运行。它会尝试寻找所需要的游戏物体并且将它们绑定到视图窗口中的正确插槽位置。但是，如果它并不能找到对应的游戏物体，它会打印出一条游泳的信息。

我们现在需要为我们的三角运算来收集一些数据。我们所需要的数据是每个臂段的长度。我们将会在运行中的Awake方法内完成，因此我们知道针对现在hierarchy视图中的设置来说是完全正确的。

```
void Awake()
{
    upperLength = (lower.position - upper.position).magnitude;
    lowerLength = (effector.position - lower.position).magnitude;
    effectorLength = (tip.position - effector.position).magnitude;
    pivotLength = (upper.position - pivot.position).magnitude;
}
```

接下来，Update函数将会计算目标的位置，并且会调用Solve方法（我们等会再来完成）。

```
void Update()
{
    tipTarget = target;
    effectorTarget = target + normal * effectorLength;
    Solve();
}
```

现在我们开始耍些小聪明。我们直接使用normal字段来设置最终段的方向。这个字段将会被设置为一个常量，或者是基于碰撞法线或一些其他的机理而动态改变的。正如我们所了解最终段的朝向，通过我们所知道的朝向和effector的长度，我们可以计算出effector的transform的目标位置。

## The IK Algorithm（IK算法）



[返回上一级](/Scripting/Editor.md)

[返回主页](/README.md)
