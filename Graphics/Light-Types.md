# 光照类型
我们现在已经介绍了在开始在Unity中点亮场景之前需要考虑的一些项目设置。希望此时您应该为您的目标平台配置适当的项目（通常用于移动设备的烘焙GI和Gamma颜色空间，用于独立PC或次世代游戏控制台的预计算实时GI和线性颜色空间）。

让我们继续看看可用于实现游戏中所需照明的工具。（只做一个大致上的介绍）

## Directional Lights（方向光）
主要运用于模拟太阳光。

![](/Image/Graphics/Introduction/directionallight.jpg)

定向灯不会随着距离而减小。由于它们会影响场景中的所有曲面（除非被剔除），因此在使用“延迟渲染（Deferred Rendering path）”路径时会产生性能成本。请记住，使用此渲染技术时，灯光的性能成本与其照明的像素数量有关。然而，尽管有成本，但性能至少是一致的，因此更容易平衡。

## Point Lights(点光源)
点光源可以被认为是3D空间中的一个点，光从各个方向发射。这些对于创建灯泡，武器发光或爆炸等效果非常有用。

![](/Image/Graphics/Introduction/lighttype-point_1.png)

为点光源启用阴影可能很昂贵，因此必须谨慎使用。点光源要求阴影必须为六个世界方向渲染六次，而在较慢的硬件上，这可能是不可接受的性能成本。

当在场景中加入点光源时要注意，目前它们不支持阴影的间接反射，这代表由点光源产生的光线，只要在距离内有可能会穿过对象反射到另外一面，这可能会导致墙壁或地板”漏光”，因此放置点光源要格外注意，然而如果是采用Backed GI的话，就不会有这类的问题产生。

## Spotlights（聚光灯）
聚光灯向前（+ Z）方向投射一个光锥，它们可以用作路灯，壁灯或动态使用的效果，用于创建像手电筒一样的效果。

![](/Image/Graphics/Introduction/spotlight1_0.jpg)

![](/Image/Graphics/Introduction/lighttype-spot_1.png)

和点光源一样，使用预计算GI时，聚光灯不支持间接光阴影，这表示灯光会穿过几何影响到另外一面，因此放置投射灯要特别注意。

## Area Lights（区域光）
区域光可以当作是摄影用的柔光灯，在Unity里面他们被定义为单面往Z轴发射光线的矩形，目前只能和烘焙GI一起使用，区域光会均匀的照亮作用区域，虽然区域光没有范围属性可以调整，但是光的强度也是会随着距离光源越远而递减。

![](/Image/Graphics/Introduction/lighttype-area_1.png)

[返回上一级](/Graphics/Introduction-to-Lighting-and-Rendering.md)

[返回主页](/README.md)
