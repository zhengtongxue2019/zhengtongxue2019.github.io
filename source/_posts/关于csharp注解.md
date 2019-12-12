---
title: 关于csharp注解
date: 2019-11-19 17:03:12
tags:
---
## 特性Attribute
特性Attribute 是用于在运行时传递程序中各种元素，如类、方法、结构、枚举、组件等的行为信息的声明性标签。
可以通过特性向程序添加声明性信息。
一个声明性标签是通过放置在它所应用的元素前面的方括号[]中来描述。

特性Attribute 用于添加元数据，如编译器指令和注释、描述、方法、类等其他信息。
.NET提供两种类型的特性：预定义特性和自定义特性。 

特性Attribute就是一种“附着物”——就像牡蛎吸附在船底或礁石上一样。 
这些附着物的作用是为它们的附着体追加上一些额外的信息。

特性Attribute的作用是为元数据添加内容。 
元数据可以被工具支持，比如：编译器用元数据来辅助编译，调试器用元数据来调试程序。

一般来讲，Attribute大部分用在框架设计与组件设计中，通过Attribute机制将业务代码与权限代码很好的分离开发，代码更简洁，可理解性更强。
<!--more-->
## 预定义特性
.NET提供了三种预定义特性：
* AttributeUsage 描述了如何使用一个自定义特性类。它规定了特性可应用到的项目的类型。
```
[AttributeUsage(
   validon,//参数 validon 规定特性可被放置的语言元素。它是枚举器 AttributeTargets 的值的组合。默认值是 AttributeTargets.All。
   AllowMultiple=allowmultiple,//参数 allowmultiple（可选的）为该特性的 AllowMultiple 属性（property）提供一个布尔值。如果为 true，则该特性是多用的。默认值是 false（单用的）。
   Inherited=inherited//参数 inherited（可选的）为该特性的 Inherited 属性（property）提供一个布尔值。如果为 true，则该特性可被派生类继承。默认值是 false（不被继承）。
)]
```
示例：
```
[AttributeUsage(AttributeTargets.Class |
AttributeTargets.Constructor |
AttributeTargets.Field |
AttributeTargets.Method |
AttributeTargets.Property, 
AllowMultiple = true)]
```

* Conditional 标记了一个条件方法，其执行依赖于指定的预处理标识符。
```
[Conditional(
   conditionalSymbol
)]
```
示例：
```
[Conditional("DEBUG")]
public static void Message(string msg)
```

* Obsolete 标记了不应被使用的程序实体。它可以让您通知编译器丢弃某个特定的目标元素。
```
[Obsolete(
   message,//参数 message，是一个字符串，描述项目为什么过时以及该替代使用什么。
   iserror //参数 iserror，是一个布尔值。如果该值为 true，编译器应把该项目的使用当作一个错误。默认值是 false（编译器生成一个警告）。
)]
```

示例：
```
[Obsolete("Don't use OldMethod, use NewMethod instead", true)]
static void OldMethod()
```

## 使用自定义的注解Attribute

.Net 框架允许创建自定义特性，用于存储声明性的信息，且可在运行时被检索。该信息根据设计标准和应用程序需要，可与任何目标元素相关。

创建并使用自定义特性包含四个步骤：
1、声明自定义特性
2、构建自定义特性
3、在目标程序元素上应用自定义特性
4、通过反射访问特性
最后一个步骤包含编写一个简单的程序来读取元数据以便查找各种符号。元数据是用于描述其他数据的数据和信息。该程序应使用反射来在运行时访问特性。


## 自定义Attribute的示例

### 在目标程序元素上应用自定义特性
//注解 模块化
namespace CustomPlugin
{
    [Module(ModuleName = "CustomPlugin", OnDemand = true)]
    public class CustomPlugin : IModule
    {
	}
}

### 自定义特性&构建自定义特性
//注解，描述注解可以用来修饰哪些，声明可以用来修饰类，不支持AllowMultiple
using System;
namespace Tomato.Modularity
{
	//声明自定义特性
    [AttributeUsage(AttributeTargets.Class, AllowMultiple = false)]
    public sealed class ModuleAttribute : Attribute
    {
	//构建自定义特性
        public ModuleAttribute();

        public string ModuleName { get; set; }
        public bool OnDemand { get; set; }
    }
}

## 通过反射获得特性
```
System.Reflection.MemberInfo info = typeof(MyClass);
object[] attributes = info.GetCustomAttributes(true);
```

来自张逸的[.Net平台AOP技术研究](https://www.cnblogs.com/wayfarer/articles/256909.html)，有深度需要多看看。
```
System.Reflection.MemberInfo info = typeof(Bar);
CallTracingAttribute attribute = (CallTracingAttribute) Attribute.GetCustomAttribute(info,typeof(CallTracingAttribute));
if (attribute != null)
{
	 Console.WriteLine(“Tracing Information:{0}”,attribute.TracingInfo);
}
```


## 依赖的系统提供的注解

### AttributeUsageAttribute
//注解，描述注解可以用来修饰哪些，声明可以用来修饰类，ComVisible可见
using System.Runtime.InteropServices;
namespace System
{
    [AttributeUsage(AttributeTargets.Class)]
    [ComVisible(true)]
    public sealed class AttributeUsageAttribute : Attribute
    {
        public AttributeUsageAttribute(AttributeTargets validOn);

        public bool AllowMultiple { get; set; }
        public bool Inherited { get; set; }
        public AttributeTargets ValidOn { get; }
    }
}

### ComVisibleAttribute
//注解，描述注解可以用来修饰哪些，声明可以用来修饰这些类型，不支持Inherited，ComVisible可见
namespace System.Runtime.InteropServices
{
    [AttributeUsage(AttributeTargets.Assembly | AttributeTargets.Class | AttributeTargets.Struct | AttributeTargets.Enum | AttributeTargets.Method | AttributeTargets.Property | AttributeTargets.Field | AttributeTargets.Interface | AttributeTargets.Delegate, Inherited = false)]
    [ComVisible(true)]
    public sealed class ComVisibleAttribute : Attribute
    {
        public ComVisibleAttribute(bool visibility);

        public bool Value { get; }
    }
}


### AttributeTargets
//注解，描述注解可以用来修饰哪些，声明可以用来表示ComVisible可见，支持Flags
using System.Runtime.InteropServices;
namespace System
{
    [ComVisible(true)]
    [Flags]
    public enum AttributeTargets
    {
        Assembly = 1,
        Module = 2,
        Class = 4,
        Struct = 8,
        Enum = 16,
        Constructor = 32,
        Method = 64,
        Property = 128,
        Field = 256,
        Event = 512,
        Interface = 1024,
        Parameter = 2048,
        Delegate = 4096,
        ReturnValue = 8192,
        GenericParameter = 16384,
        All = 32767
    }
}

### FlagsAttribute
//注解，描述注解可以用来修饰哪些，声明可以用来修饰枚举，不支持Inherited，ComVisible可见
using System.Runtime.InteropServices;
namespace System
{
    [AttributeUsage(AttributeTargets.Enum, Inherited = false)]
    [ComVisible(true)]
    public class FlagsAttribute : Attribute
    {
        public FlagsAttribute();
    }
}

## 关于AOP
AOP中的动态代理就是利用Attribute实现的，所以要想搞清楚动态代理，先要弄明白Attribute。
* 参见[AOP技术基础](https://www.cnblogs.com/wayfarer/articles/241024.html)
* 参见[深入浅出Attribute （上）——Attribute初体验](https://blog.51cto.com/liutiemeng/29201)