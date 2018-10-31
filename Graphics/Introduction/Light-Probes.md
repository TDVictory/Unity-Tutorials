# 光照探头
Unity的Baked或Precomputed Realtime GI系统仅考虑静态对象。为了使交互式场景元素或角色等动态对象能够获取到和静态几何体所接收到的相同的反射光，我们需要将这种光照信息记录为一种格式，可以在游戏过程中快速读取并用于我们的光照方程。

我们通过在世界中放置采样点然后从所有方向捕获光来实现此目的。然后将这些点记录的颜色信息编码成一组值（或系数），这些值可以在游戏过程中快速评估。在Unity中，我们将这些样本点称为“Light Probes”。

![](/Image/Graphics/Introduction/lightprobes_3.png)

*场景使用Light Probes。注意在光照变化的区域——例如阴影或颜色过渡——它们将以更大的密度放置。*

Light Probes允许移动物体响应同样复杂的反射光照，这会影响我们的光照贴图，无论是使用Baked GI还是Precomputed Realtime GI。对象的网格渲染器将在其位置周围寻找光照探针并在它们的值之间进行混合。这是透过找寻由光探头所产生的一个四面体，然后决定哪个四面体的落入对象的轴向，这样就能让场景内的动态对象正确地接受光信息，如果没有放置光探头，动态对象就无法接受全局光照的信息，造成动态对象比场景还要暗。

默认情况下，场景中没有Light Probes，因此需要使用Light Probe Group（GameObjects-Light-Light-Probe Group）放置这些Light Probes。

假如全局光照里的Auto是打勾的(Lighting->Scene->Auto)，当光源或是静态对象更新时，光探头信息也会实时更新，没打勾的话必须点Build运算才会更新。

[返回上一级](/Graphics/Introduction-to-Lighting-and-Rendering.md)

[返回主页](/README.md)
