# Unity编辑器扩展——菜单项
Unity编辑器允许添加和内置菜单一样的自定义菜单。添加经常需要的常用功能，使得可直接从编辑器UI访问会非常有用。

## Adding Menu Items（添加菜单项）
为了给顶层的工具栏添加一个新菜单，你需要创建一个编辑器脚本（一个放置在名为Editor文件夹下任意位置的脚本文件）菜单项通过脚本以菜单项属性静态创建。

举个例子，为你团队/公司创建一个新的“工具”菜单（抑或是一个以你公司命名的顶部菜单）。

```
using UnityEngine;
using UnityEditor;
 
public class MenuItems
{
    [MenuItem("Tools/Clear PlayerPrefs")]
    private static void NewMenuOption()
    {
        PlayerPrefs.DeleteAll();
    }
}
```
这可以创建一个名为Tools的编辑器窗口，并在其下设置一个名为Clear PlayerPrefs的菜单项。

![](/Image/Scripting/Editor/MenuItems01.png)

在已有的菜单下添加新的菜单选项也是可行的（例如在Window菜单下）， 如果需要的话，通过在菜单下创建多级子菜单对于组织结构有着很大的帮助。

```
using UnityEngine;
using UnityEditor;
 
public class MenuItemsExample
{
    // Add a new menu item under an existing menu
 
    [MenuItem("Window/New Option")]
    private static void NewMenuOption()
    {
    }
 
    // Add a menu item with multiple levels of nesting
 
    [MenuItem("Tools/SubMenu/Option")]
    private static void NewNestedOption()
    {
    }
}
```
结果如下

![](/Image/Scripting/Editor/MenuItems02.png)

## Hotkeys（热键）
为了帮助高级用户以及键盘能手更快地工作，新的菜单项可以分配热键——快捷键组合将可以自动执行它们。

支持的键位如下：
- %——CTRL（Windows）/ CMD（OSX）
- #——Shift
- &——Alt
- LEFT/RIGHT/UP/DOWN——Arrow keys
- F1…F2——F keys
- HOME, END, PGUP, PGDN

通过向它们添加下划线前缀来添加不属于键序列的字符键（例如：_g表示快捷键“G”）。

热键字符组合添加到菜单项路径的末尾，前面有空格），如以下示例所示：

```
// Add a new menu item with hotkey CTRL-SHIFT-A
 
[MenuItem("Tools/New Option %#a")]
private static void NewMenuOption()
{
}
 
// Add a new menu item with hotkey CTRL-G
 
[MenuItem("Tools/Item %g")]
private static void NewNestedOption()
{
}
 
// Add a new menu item with hotkey G
[MenuItem("Tools/Item2 _g")]
private static void NewOptionWithHotkey()
{
}
```
带有热键的菜单项将会展示用于启动的快捷键。举个例子来说，上面代码在菜单上的效果如下：

![](/Image/Scripting/Editor/MenuItems03.png)

注意：覆写热键是不会生效的，使用相同的热键来定义复数菜单项的结果是只有其中一种会被按键所响应。

## Special Paths（特殊路径）
显然，菜单项的路径控制着你新添加的菜单项将会被放置在哪个顶部菜单项下。

Unity也有一些作为快捷菜单的“特殊”路径（那些通过点击右键弹出的菜单项）：
- Assets——以此路径添加的菜单项将放置在“Assets”顶部菜单栏下，同样也会在project view（项目视图）中点击右键的菜单栏中出现。
- Assets/Create——以此路径添加的菜单项将放置在project view（项目视图）中“Create”按键列表内。
- CONTEXT/ComponentName——以此路径添加的菜单项将放置在Inspector视图中对组件右键操作的菜单列表中。

以下是如何使用这些特殊路径的一些示例：
```
// Add a new menu item that is accessed by right-clicking on an asset in the project view
 
[MenuItem("Assets/Load Additive Scene")]
private static void LoadAdditiveScene()
{
    var selected = Selection.activeObject;
    EditorApplication.OpenSceneAdditive(AssetDatabase.GetAssetPath(selected));
}
 
// Adding a new menu item under Assets/Create
 
[MenuItem("Assets/Create/Add Configuration")]
private static void AddConfig()
{
    // Create and add a new ScriptableObject for storing configuration
}
 
// Add a new menu item that is accessed by right-clicking inside the RigidBody component
 
[MenuItem("CONTEXT/Rigidbody/New Option")]
private static void NewOpenForRigidBody()
{
}
```
此代码段的结果是以下新菜单选项：

![](/Image/Scripting/Editor/MenuItems04.png)

*Assets(project view右键)菜单*

![](/Image/Scripting/Editor/MenuItems05.png)

*Assets下的“CREATE”按键菜单*

![](/Image/Scripting/Editor/MenuItems06.png)

*组件内的菜单选项*

## Validation（生效）
一些菜单项只有在特定情况下才有意义，而其他时候不应该被启用。可以通过添加生效模式（validation method），依据它们所使用的场景来启用/禁用菜单项。

生效模式（validation method）是静态模式，通过菜单项属性标记，当设置为“True”时开启生效判定。

生效模式应与其判定是否生效的菜单具有相同的菜单路径（即该菜单需要创建一个菜单项和一个验证是否生效项），并应返回布尔值以确定菜单项是否处于激活状态。

例如，生效模式可用于仅在项目视图下向纹理资源添加右键单击菜单：

```
[MenuItem("Assets/ProcessTexture")]
private static void DoSomethingWithTexture()
{
}
 
// Note that we pass the same path, and also pass "true" to the second argument.
[MenuItem("Assets/ProcessTexture", true)]
private static bool NewMenuOptionValidation()
{
    // This returns true when the selected object is a Texture2D (the menu item will be disabled otherwise).
    return Selection.activeObject.GetType() == typeof(Texture2D);
}
```

当在project view中右击任意不是纹理（Texture）的物件时，该菜单项将不会激活。

![](/Image/Scripting/Editor/MenuItems07.png)

## Controlling Order with Priority（通过优先级控制顺序）
优先级时一个可以被指定给菜单项的数字（通过菜单项属性赋值），以此来控制在根目录上展示的顺序。

菜单项以分配的优先级数值来进行以50为分界线的分组。

```
[MenuItem("NewMenu/Option1", false, 1)]
private static void NewMenuOption()
{
}
 
[MenuItem("NewMenu/Option2", false, 2)]
private static void NewMenuOption2()
{
}
 
[MenuItem("NewMenu/Option3", false, 3)]
private static void NewMenuOption3()
{
}
 
[MenuItem("NewMenu/Option4", false, 51)]
private static void NewMenuOption4()
{
}
 
[MenuItem("NewMenu/Option5", false, 52)]
private static void NewMenuOption5()
{
}
```

以上的代码示例结果是菜单项分为两组，依据它们所分配的优先级。

![](/Image/Scripting/Editor/MenuItems08.png)

如果需要在已有的Unity菜单下添加并整理菜单项，将无法避免一些“猜测工作”，因为大多时内置的菜单项都使用了优先级。另一个选项是使用像Reflector的工具，并且查看Unity的构建编辑器窗口的源代码（像UnityEditor.CreateBuildInWindows）。

## Related Classes（相关类）
下面列出了一些与添加新菜单项相关的额外类。

### MenuCommand
当为Inspector添加一个新的菜单时（使用上述的“CONTEXT/Component”），有时需要获取对实际组件的引用（例如修改其数据）。

这可以通过向定义新菜单项的静态方法添加MenuCommand参数来完成：
```
[MenuItem("CONTEXT/RigidBody/New Option")]
private static void NewMenuOption(MenuCommand menuCommand)
{
    // The RigidBody component can be extracted from the menu command using the context field.
    var rigid = menuCommand.context as Rigidbody;
}
```
我感觉官方这个讲的不明白，于是去搜了一下官方的API，文档描述的更详细，代码如下：
```
// Add context menu named "Do Something" to context menu
using UnityEngine;
using UnityEditor;

public class Something : EditorWindow
{
    // Add menu item
    [MenuItem("CONTEXT/Rigidbody/Do Something")]
    static void DoSomething(MenuCommand command)
    {
        Rigidbody body = (Rigidbody)command.context;
        body.mass = 5;
        Debug.Log("Changed Rigidbody's Mass to " + body.mass + " from Context Menu...");
    }
}
```
我们可以看到通过MenuCommand.context获取并强转成Rigidbody后，就可以通过该按键修改组件的参数。

### ContextMenu
这条属性将被



[返回上一级](/Scripting/Editor.md)

[返回主页](/README.md)
