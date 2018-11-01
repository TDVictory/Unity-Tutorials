# 从何而始
在我们开始探索Unity内部的渲染之前，我们首先需要将我们的资产转换成适合我们最终目标的格式。在你的数字内容制作工具（Digital Content Creation，DCC）与Unity间设置一个合适的工作流程是十分困难的。虽然我们不需要考虑从大量可用的DCC工具导出的步骤，但是依然有一些我们需要思考的问题：

## Scale and units（缩放与单位）
缩放与你的计量单位在一个逼真的场景中扮演着重要的角色。在许多“真实世界”创建时，推荐使用一个Unity单位等价于一米（100cm），因为许多物理系统都以这个单位大小做假设模拟。

## Unit translation DCC 3D software to Unity（DCC 3D软件到Unity内的单位转化）
为了保持DCC 3D软件与Unity之间的一致性，验证导入的物体的缩放比与大小是一个好想法。DCC 3D软件（3ds Max/Maya/Blender/Houdini 等）拥有单位设置以及缩放设置在FBX的输出配置中（参考每个软件的手册来配置）。总体来说，将这些工具设置成以cm运作，并自动缩放比输出FB将契合Unity的输入缩放比。当然每次在开始一个工程时都确认是否匹配最使人放心。

验证导出设置的快速测试是在DCC 3D软件中创建一个简单的1x1x1m的方块。然后可以将其导入Unity。默认的Unity Cube（GameObject-3D Objec-Cube）为1x1x1m，因此是一个有用的比例参考，可与您导入的模型进行比较。

![](/Image/Graphics/Creating-Believable-Visuals/staying-on-track-in-making-believable-visual-in-unity-copy-1.jpg)

当Unity Inspector中的Transform组件中的scale设置为1,1,1时，这些立方体应该看起来相同。

注意：Maya和3DsMax可能会覆盖您的默认单位设置，具体取决于上次打开的文件。请注意，在“internal unit（内部单元）”中具有不同的设置时，3D软件可以在工作区以不同的单位展示。这可能会引起一些冲突。

![](/Image/Graphics/Creating-Believable-Visuals/staying-on-track-in-making-believable-visual-in-unity-copy-2.jpg)

*在3ds Max中的例子*

## Point of reference scale model（模型比例参考点）
使用占位符或草绘几何体遮挡场景时，使用参考点比例模型可能会有所帮助。根据您正在制作的场景，选择可与场景相关的模型比例参考点。在Spotlight隧道样本场景案例中，选择了一个普通的公园长椅。这并不意味着场景必须与现实生活完全相同，相反，它允许物体间相对比例具有一致性，即使场景具有夸大的比例的倾向。

![](/Image/Graphics/Creating-Believable-Visuals/staying-on-track-in-making-believable-visual-in-unity-copy-3.jpg)

## Texture output and channels（纹理输出及通道）
与Unity期望在3D软件和Unity之间进行一致的单位转换相同，纹理内部的信息需要包含确切的信息，以便在添加到材质时提供正确的结果。

Substance Painter的预设配置示例，用于输出与Unity Standard不透明材质一起使用的纹理。

![](/Image/Graphics/Creating-Believable-Visuals/staying-on-track-in-making-believable-visual-in-unity-copy-3.jpg)

注意：与将环境光遮挡（AO）导出为单独的纹理相比，将多个通道打包到单个纹理（如Metallic，AO，Gloss）可节省纹理内存。这是使用Unity Standard材料的典型方法。

纹理创作软件（如Photoshop和Substance Painter）将通过适当的配置输出一致且可预测的纹理。然而，在某些情况下，内容创建者可能会混淆主要处理alpha通道。

下面的例子显示PNG中的“transparency（透明度）”可能会让Photoshop中的作者感到困惑，因为它在没有外部插件的情况下处理PNG Alpha的原生方式。在这种情况下，具有专用Alpha通道的未压缩32位TGA可能是更好的选择，假设源纹理文件大小不是问题。

![](/Image/Graphics/Creating-Believable-Visuals/staying-on-track-in-making-believable-visual-in-unity-copy-5.jpg)

Photoshop创作的“transparent（透明）”PNG上面有alpha作为黑色值，而带有专用Alpha通道的TGA显示预期值。当每个纹理被分配给标准材质后，会将alpha通道读取为平滑度（smoothness），结果是具有PNG纹理的材质会的意外地反转平滑度，而具有TGA纹理的材质显示如上所示的预期结果。

## Normal map direction（法线贴图方向）

Unity使用以下解释读取切线空间法线贴图：红色通道X +为“Right（右）”，绿色通道Y +为“Up（上）”（OpenGL样式）。

例如，3ds Max“Render to Texture（渲染到纹理）”法线贴图默认将绿色通道Y +输出为“下”。这将导致沿Y轴反转的曲面方向，从而在接受光照时产生无效结果。

![](/Image/Graphics/Creating-Believable-Visuals/staying-on-track-in-making-believable-visual-in-unity-copy-6.jpg)

要验证法线贴图方向，请创建一个带凹面斜面的简单平面（上例中的中间图片）并将其烘焙到平面。然后将烘焙的法线贴图分配到Unity中具有可识别光方向的平面中，并查看是否有任何轴被反转。有关正确的轴设置，请参阅Digital Content Creation软件手册。

[返回上一级](/Graphics/Creating-Believable-Visuals.md)

[返回主页](/README.md)
