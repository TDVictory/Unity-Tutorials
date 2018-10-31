# 反射
## ‘Reflection Source（反射源）
默认情况下，场景中的对象使用Unity的“StandardShader（标准着色器）”进行渲染。标准着色器是一个基于物理的着色器(PBS)。这试图通过模仿物理特性（例如反射率）和现实世界中存在的“能量守恒”原理来准确地表示光在对应材质上的反应过程。

使用标准着色器时，每种材质都具有基于其“specularity（镜面反射度）”或“metalness（金属度）”的反射度。如果没有足够强大的硬件来实时追踪光线反射，我们必须依靠预渲染反射。我们使用cubemap（立方体贴图）——从天空或从“Reflection Probe（反射探针）”衍生的世界的6面图像——执行此操作，其从空间中的特定点渲染环境，将结果写入纹理。然后通过材质的着色器将其与其他光照和表面数据混合，以逼近像我们在现实世界中看到的那样的效果。

![](/Image/Graphics/Introduction/reflectionsource_0.png)

*默认情况下，场景中具有高Specular/ Metal数值的材质反射投影Skybox，由Sky Lighting Panel的Reflection Source属性定义。可以通过选择不同的源或向场景添加反射探测器来更改此行为。*

默认情况下，Unity场景中的对象将反映Skybox。但是，可以使用在Lighting窗口中“Reflection Source”属性来全局更改此行为。可以使用Skybox，或者可以使用自定义立方体贴图。这个“Reflection Source”可以被认为是场景中所有对象使用的场景立方体贴图（scene-wide cubemap），除非通过添加新的反射探针来覆盖。

## Reflection Probes（反射探针）
通常我们不希望物体在Unity场景中简单地反映Skybox。在许多情况下，物体可能被天空阻挡或“遮挡”。它们可能在室内或在桥梁或隧道等建筑特征下面。为了创建更精确的反射，我们需要使用“Reflection Probes（反射探测器）”对采样对象进行采样。这些探针从三维空间中的位置渲染世界，并将结果写入立方体贴图。然后，附近的物体可以使用它来给人一种反映周围世界的印象。

可以通过（GameObject-Light-Reflection Probe）添加反射探针。

反射探测器的位置将决定生成的立方体贴图的外观，以及因此在反射中“看到”的内容。通常出于性能原因，最好使用尽可能少的探针。请记住，**反射探测并不是为了给出物理上准确的结果，而是给人以游戏世界中反射的印象**。在大多数情况下，整个场景中的一些放置良好的探头就足够了。

![](/Image/Graphics/Introduction/reflectionprobeab_0.png)
*左图：使用了默认反射设置的场景，右图：添加了反射探针的结果*

在Reflection Probe的Inspector面板中，我们可以设置探针的“Type”属性，以便在“Baked”，“Custom”或“Realtime”之间进行选择。应该注意的是，实时（Realtime）反射探针对性能非常不利，因为我们有效地为每个探测器渲染场景6次。在某些特定情况下需要进行实时反射探测并且这种费用是合理的，但作为一般规则，Baked Reflection Probes更受欢迎，因为它们的性能要好得多。

请注意，如果从“Inspector”面板顶部的“Static”下拉列表中标记为“Reflection Probe Static”，则游戏对象仅对Baked Reflection Probes可见。相反，实时（Realtime）探针会渲染世界上所有可见的游戏对象，除非应用剔除蒙版（culling mask）。

[返回上一级](/Graphics/Introduction-to-Lighting-and-Rendering.md)

[返回主页](/README.md)
