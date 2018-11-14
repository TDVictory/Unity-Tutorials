# 创建样条曲线工具
在本教程中，你将会了解到
- 如何在场景视图中使用高度小工具
- 如何使用**SerializedProperty** 和 **SerializedObject**实例来修改组件。
- 如何完成一个自定义的Inspector GUI。
- 如何响应、拦截、使用GUI事件
- 如何查询内部编辑器状态以自定义工具行为

## Creating the Component（创建组件）
如果我们创建了一个指定样条工具API的接口，我们可以使用这个接口而不是一个确切的类，这将使我们可以在实现接口与集成其他未来可能使用的其他任意系统，只要它们也会使用这个接口。

这个接口内容将包含适用于大多数样条算法的一般方法。它包含创建与调整样条的方法，以及查询样条各种信息的方法。

## The Spline Interface (ISpline.cs)（样条接口）
```
/// <summary>
/// A interface for general spline data.
/// NB: - All Vector3 arguments and Vector3 return values are in world space.
///     - All t arguments specify a uniform position along the spline, apart
///       from the GetNonUniformPoint method.
/// </summary>
public interface ISpline
{
    Vector3 GetNonUniformPoint(float t);
    Vector3 GetPoint(float t);


    Vector3 GetLeft(float t);
    Vector3 GetRight(float t);
    Vector3 GetUp(float t);
    Vector3 GetDown(float t);
    Vector3 GetForward(float t);
    Vector3 GetBackward(float t);


    float GetLength(float stepSize);


    Vector3 GetControlPoint(int index);
    void SetControlPoint(int index, Vector3 position);
    void InsertControlPoint(int index, Vector3 position);
    void RemoveControlPoint(int index);


    Vector3 GetDistance(float distance);
    Vector3 FindClosest(Vector3 worldPoint);


    int ControlPointCount { get; }
}
```

## An empty class (SplineComponent.cs)(一个空类)
如果我们使用了该类的默认实现，我们将会得到下述类。该类内并没有做什么工作，但是给我们存根来输入所有需要的方法，来满足ISpline接口的要求。
```
public class SplineComponent : MonoBehaviour, ISpline
{
    public int ControlPointCount { get { throw new System.NotImplementedException(); } }


    public Vector3 FindClosest(Vector3 worldPoint)
    {
        throw new System.NotImplementedException();
    }


    public Vector3 GetBackward(float t)
    {
        throw new System.NotImplementedException();
    }


    public Vector3 GetControlPoint(int index)
    {
        throw new System.NotImplementedException();
    }


    public Vector3 GetDistance(float distance)
    {
        throw new System.NotImplementedException();
    }


    public Vector3 GetDown(float t)
    {
        throw new System.NotImplementedException();
    }


    public Vector3 GetForward(float t)
    {
        throw new System.NotImplementedException();
    }


    public Vector3 GetLeft(float t)
    {
        throw new System.NotImplementedException();
    }


    public float GetLength(float stepSize)
    {
        throw new System.NotImplementedException();
    }


    public Vector3 GetNonUniformPoint(float t)
    {
        throw new System.NotImplementedException();
    }


    public Vector3 GetPoint(float t)
    {
        throw new System.NotImplementedException();
    }


    public Vector3 GetRight(float t)
    {
        throw new System.NotImplementedException();
    }


    public Vector3 GetUp(float t)
    {
        throw new System.NotImplementedException();
    }


    public void InsertControlPoint(int index, Vector3 position)
    {
        throw new System.NotImplementedException();
    }


    public void RemoveControlPoint(int index)
    {
        throw new System.NotImplementedException();
    }


    public void SetControlPoint(int index, Vector3 position)
    {
        throw new System.NotImplementedException();
    }
}
```

## The Interpolator（插值器）
这是一个hermite样条插值函数。它包含4个向量（a和b是控制点，c和d是开始和终止点），u参数则用于指定插值位置。
```
internal static Vector3 Interpolate(Vector3 a, Vector3 b, Vector3 c, Vector3 d, float u)
    {
        return (
            0.5f *
            (
                (-a + 3f * b - 3f * c + d) *
                (u * u * u) +
                (2f * a - 5f * b + 4f * c - d) *
                (u * u) +
                (-a + c) *
                u + 2f * b
            )
        );
    }
```

## The Data(数据)
我们需要一些区域来存储那些被我们的内插函数使用的数据。

这部分自己上机测的莫名其妙，完全没能get到教程中的点，所以这章暂时gg。留个链接[创建样条曲线工具](https://unity3d.com/cn/learn/tutorials/topics/scripting/creating-spline-tool?playlist=17117)

[返回上一级](/Scripting/Editor.md)

[返回主页](/README.md)
