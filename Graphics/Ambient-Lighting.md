# 环境光
对于场景的整体外观和亮度的重要贡献是“ambient lighting（环境光）”。这也可以被认为是从各个方向影响场景中的物体的全局光源。

根据您选择的艺术风格，环境光在许多情况下都很有用。一个例子是明亮的卡通风格渲染，其中暗阴影可能是不合需要的，或者可能将照明手绘成纹理。如果您需要在不调整单个灯光的情况下增加场景的整体亮度，环境光也很有用。

如果不使用Unity的光照预计算（precomputed lighting）解决方案，环境光将不会被遮挡，因此在物理上不准确。但是，如果场景中启用了烘培全局光照（Baked GI）或实时预计算全局光照（Precomputed Realtime GI），则此“‘skylight”将被场景中的对象阻挡，从而产生更逼真的效果。

![](/Image/Graphics/Introduction/ambientlightab_0.png)
*没有任何光线的相同场景（左）和仅环境光（右）。请注意当对环境光强度进行更改时，可见Skybox如何不会更改。*

![](/Image/Graphics/Introduction/ambientlightc_0.png)
*现在通过将对象标记为静态来使用预计算实时GI。注意在表面之间的接触区域中光线现在是如何被遮挡的。*

使用环境光的一个显着优点是渲染成本低廉，因此特别适用于可能需要最小化场景中灯光数量的移动应用。

可以在Lighting窗口中从Environment Lighting部分（Lighting-Scene-Ambient Source）控制“环境光照”。

默认值是“Ambient Source”属性设置为“Skybox”。在这种情况下，Skybox是默认的Skybox，为场景的Ambient Lighting提供蓝色色调。“环境光源”的其他选项包括纯色或“渐变”，它是在半球上应用的简单颜色渐变。

请注意，更改环境光源的颜色不会影响可见的Skybox，而只会影响场景中的光照颜色。

[返回上一级](/Graphics/Introduction-to-Lighting-and-Rendering.md)

[返回主页](/README.md)
