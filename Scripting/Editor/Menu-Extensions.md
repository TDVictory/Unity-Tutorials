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

![](/Image/Scripting/Editor/MenuItems01.jpg)

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

![](/Image/Scripting/Editor/MenuItems02.jpg)

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



