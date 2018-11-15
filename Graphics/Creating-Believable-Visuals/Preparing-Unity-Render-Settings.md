# 准备Unity渲染设置
以下建议侧重于实现可信的视觉目标。了解Unity的渲染功能如何用于逼真地模拟现实世界，这将使您能够快速实现项目可信的视觉目标。 有关更深入的信息，请访问Unity照明和渲染教程。

## 线性渲染模式
简单来说，这会将Unity设置为使用物理精确数学进行照明和着色计算，然后将最终输出转换为最适合监视器的格式。

要指定伽玛或线性工作流程，请转到Edit > Project Settings > Player，然后打开Player Settings。然后转到Other Settings > Rendering并将“Color Space(颜色空间)”更改为“Linear(线性)”。

定义颜色空间应该是项目中最早的决策之一，因为它会对最终的阴影和光照结果产生巨大影响。 Unity有很好的文档解释每个工作流程。

## 渲染模式
在Spotlight隧道示例场景中，使用延迟渲染模式。这允许内容创建者有效地使用多个动态光源，组合多个反射立方体贴图并能够使用Unity 2017+中现有的屏幕空间反射功能。

在Graphic Settings > Rendering Path中设置，或在Camera > Rendering Path中设置

有关更深入的渲染模式信息，请参阅Unity文档。

## 高动态范围（HDR）相机
当渲染可信照明时，就像现实生活一样，内容创作者将处理亮度高于1（高动态范围HDR）的照明值和发光表面。然后需要将这些值重新映射到适当的屏幕范围（色调映射(tonemapping)）。此设置对于允许Unity相机处理这些高值而不是剪辑它至关重要。要启用此功能，请选择场景中的主摄像头。确保在检查器选项卡中检查所选摄像机的HDR。

## HDR光照贴图编码（可选）
“Spotlight Tunnel”样本场景未使用烘焙照明，但是如果您计划使用高强度（HDR）烘焙照明，建议将光照贴图编码设置为HDR光照贴图以确保烘焙光照结果一致。该选项可在编辑>项目>播放器设置>其他设置>光照贴图编码（仅限Unity 2017.3+）下找到。 可以在此处找到Lightmap编码的详细信息。

## Tonemapper为您的场景
要正确显示HDR照明，需要在项目中启用tonemapper。确保在项目中安装了 Unity Post Process资产。

后处理堆栈V1中的设置:(“Spotlight隧道”场景中使用的版本。）

在项目中创建后期处理配置文件资产并将其配置为：

启用Color Grading > Tonemapper > ACES （Academy Color Encoding Standards）
启用Dithering,Dithering(递色技术)允许场景减轻由HDR场景输出的8位/通道引入的条带伪像。现代发动机使用这种技术来规避16M色彩输出的限制。
暂时将其余设置保留在tonemapper中。
选择“Main Camera（主摄像头）”和Add component（添加组件），Post Processing Behaviour（后处理行为）。

将先前创建的后处理配置文件分配给配置文件槽。对于Post Process堆栈V2，请参阅包的自述文件，因为它目前处于Beta开发阶段。

## 为视口启用图像效果
这使用户可以在使用“场景”视图时始终查看色调映射器。

![](/Image/Graphics/Creating-Believable-Visuals/staying-on-track-in-making-believable-visual-in-unity-copy-7.jpg)

注意色调映射场景中的高光再现和暗隧道值分离改进。如果您查看非色调映射的场景，您可以看到高光如何不会聚合为统一的颜色。（在这种情况下，黄色的烈日）。

此设置基本上试图复制数码相机如何捕获具有固定曝光的场景（未启用曝光适应/眼睛适应功能）。

![](/Image/Graphics/Creating-Believable-Visuals/staying-on-track-in-making-believable-visual-in-unity-copy-8.jpg)

此时，内容创建者已经实现了适当的基础场景渲染设置，应该能够提供具有广泛内容的可信结果。

[返回上一级](/Graphics/Creating-Believable-Visuals.md)

[返回主页](/README.md)
