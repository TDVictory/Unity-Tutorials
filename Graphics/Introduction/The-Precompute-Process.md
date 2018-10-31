# 预处理过程（The Precompute Process）
在Unity中，预计算是在后台执行，无论是自动流程或是手动启用，在计算期间你都可以继续编辑你的游戏对象而不受影响。

预计算的时候，编辑器会在右下角显示一个蓝色进度条，不同的算法会有不同的运算阶段，进度条上方也会显示阶段名称与进度。

![](/Image/Graphics/Introduction/progressbar.jpg)
*进度条显示Unity的预计算的当前状态。*

在上面的示例中，我们可以看到我们处于11中的任务5，即“Clustering”，并且在该任务完成之前剩余108个作业，并且预计算转到任务6.下面列出了各个阶段：

| Precomputed Realtime GI() | Baked GI | 
| - | - | 
| 01 - Create Geometry | 01 - Create Geometry | 
|02 - Layout Systems|02 - Atlassing|
|03 - Create Systems			|03 - Create Baked Systems|
|04 - Create Atlas		|	04 - Baked Resources|
|05 - Clustering		|	05 - Bake AO|
|06 - Visibility	|		06 - Export Baked Texture|
|07 - Light Transport		|	07 - Bake Visibility|
|08 - Tetrahedralize Probes	|		08 - Bake Direct|
|09 - Create ProbeSet		|	09 - Ambient and Emissive|
| |10 - Create Bake Systems|
|Probes		|	11 - Bake Runtime|
| |12 - Upsampling Visibility|
|01 - Ambient Probes	|		13 - Bake Indirect|
|02 - Baked/Realtime Ref. Probes	|		14 - Final Gather|
| |15 - Bake ProbesSet|
| |16 - Compositing|

## Starting a Precompute(启用一个预计算)
只有静态对象会被纳入Unity的GI预计算，要启动预计算首先必须最少要有一个标记为Static（静态）对象，这可以单独完成设置，也可以通过从层次结构面板中选择多个GameObject来完成。

从Inspector面板，将对象的Static勾选起来，这会将该物体所有跟静态对象相关的标志打开，包含导航标志或是批处理标志，这或许不是你想要的。针对预计算只要把”Lighting Static”这个标志打勾即可。更细部的控制，只要点选属性接口Static右边的下拉式选单即可，此外，从Window里的Lighting接口也能指定设定静态对象。

如果你的场景设为自动(Lighting-Scene-Auto)，Unity的预计算就会自动启动，否则就需要用下列的流程手动执行。

## Auto/Manual Precompute（自动/手动预计算）
假如Lighting面板底下Auto这个选项是被勾选的(Lighting-Scene-Auto)，则只要对场景中的静态几何体进行更改，此预计算将在后台自动开始处理。

但是，如果未选择“自动”，你将需要点击在Auto旁边的”Build”按钮手动启动预计算，这会用同样的方式进行预计算，让你比较好控制计算的开始时间。

手动启动预计算会对场景所有的照明与各方面进行重新评估并重新计算，如果你希望有选择性的计算反射探针（Reflection probes），可以从”Build”旁边的下拉选单来选择。

## GI Cache（GI缓存）
物类时在Baked GI或预先计算的实时GI中，Unity都会在“GI缓存”中缓存（存储）有关场景照明的数据，并会尽可能重复使用此数据，以便在预计算期间节省时间。您对场景所做更改的数量和性质将决定可以重复使用的数据量（如果有的话）。

此缓存存储在Unity项目之外，可以使用（Preference-GI Cache-Clear Cache）进行清除。清除这意味着需要从头开始重新计算预计算的所有阶段，因此这可能是耗时的。但是在某些情况下，您可能需要减少磁盘使用量，这可能会有所帮助。

[返回上一级](/Graphics/Introduction-to-Lighting-and-Rendering.md)

[返回主页](/README.md)
