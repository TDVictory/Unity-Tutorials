# 选择色彩空间
除了要选好著色路径之外，点亮你的项目之前，选择一个”色彩空间(Color Space)”也是很重要的事情，色彩空间决定采用哪种算法来计算照明或材质加载时的颜色混合，这会对游戏的画面真实感有很大的影响，但在许多情况下，决定使用哪个Color Space可能会受到目标平台硬件限制的影响。

## Linear Color Space（线性色彩空间）
推荐比较接近真实的色彩空间是Linear，你可以从PlayerSetting里面找到”Color Space”来设定(Edit-ProjectSetting-Player)

设定Linear的优点是会让场景内的提供给着色器的颜色也会因为光强度增加变亮，如果换成”Gamma”色彩空间，亮度马上会转为以白色，这对图像的质量有损。

![](/Image/Graphics/Introduction/linearcolourspace_1.png)

*采用Linear和Gamma颜色空间的图像对照表，可以注意到切换成Gamma时，当光强度增加时，颜色会快速变为白色。*

Linear另一个好处是着色器能在没有Gamma补偿的情况下对贴图进行取样。这有助于确保颜色值在整个渲染管道的整个过程中保持一致。能提高色彩和计算的精度，最后屏幕的输出结果更为真实。

可惜的是Linear颜色空间有些手机平台甚至有些游戏机不支持，应该说PC或是一些新手机硬件和次世代游戏机才会支持Linear颜色空间，在这种情况下，就得用Gamma方法替代了。

[返回上一级](/Graphics/Introduction-to-Lighting-and-Rendering.md)

[返回主页](/README.md)
