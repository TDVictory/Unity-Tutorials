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
创建一个名为SimpleIKSolver.cs

[返回上一级](/Scripting/Editor.md)

[返回主页](/README.md)
