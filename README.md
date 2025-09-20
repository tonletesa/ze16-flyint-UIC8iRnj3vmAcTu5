[合集 - .NET 技术合集(24)](https://github.com)

[1..NET 文件上传服务设计2023-06-15](https://github.com/ZYPLJ/p/17483162.html)[2..NET项目中使用HtmlSanitizer防止XSS攻击2023-06-12](https://github.com/ZYPLJ/p/17475646.html)[3..NET 中使用RabbitMQ初体验2023-07-21](https://github.com/ZYPLJ/p/17572104.html)[4..NET中使用RabbitMQ延时队列和死信队列2023-07-30](https://github.com/ZYPLJ/p/17591838.html)[5.认识.NET 日志系统2023-08-28](https://github.com/ZYPLJ/p/17663487.html)[6..NET 认识日志系统-22023-08-30](https://github.com/ZYPLJ/p/17667970.html)[7..NET 日志系统-3 结构化日志和集中日志服务2023-09-01](https://github.com/ZYPLJ/p/17671522.html)[8.EF Core并发控制2023-09-03](https://github.com/ZYPLJ/p/17675494.html)[9..Net Framework使用Autofac实现依赖注入2023-09-12](https://github.com/ZYPLJ/p/17694895.html)[10.ASP.NET Core使用Hangfire定时发布文章2023-10-15](https://github.com/ZYPLJ/p/17765413.html)[11.记录EF 排序配上自定义的比较器2023-10-23](https://github.com/ZYPLJ/p/17783188.html)[12..NET Core MVC基础之页面传值方式📃2023-11-25](https://github.com/ZYPLJ/p/17854903.html)[13..NET Core MVC基础之返回文件类型2023-12-11](https://github.com/ZYPLJ/p/17895143.html)[14..NET Core WebAPI项目部署iis后Swagger 404问题解决2024-03-06](https://github.com/ZYPLJ/p/18057885)[15.在C#中进行单元测试2024-06-27](https://github.com/ZYPLJ/p/18270869)[16.在C#中使用RabbitMQ做个简单的发送邮件小项目2024-07-02](https://github.com/ZYPLJ/p/18279034)[17..NET Core搭配Vue开源弹幕效果，实现一个评论小项目。好玩！2024-09-09](https://github.com/ZYPLJ/p/18403223)[18..NET9 中替换Swagger使用Scalar01-22](https://github.com/ZYPLJ/p/18686930)[19.简单说说C#中委托的使用-0105-26](https://github.com/ZYPLJ/p/18897174)[20.基于SharpIco开发图片转ICO工具网站06-30](https://github.com/ZYPLJ/p/18957808)[21.SharpIcoWeb开发记录篇07-02](https://github.com/ZYPLJ/p/18961664)[22.dotnet Minimal APIs实现动态注册端点07-15](https://github.com/ZYPLJ/p/18985930):[cmespeed楚门加速器](https://77yingba.com)[23..Net Minimal APIs实现动态注册服务07-17](https://github.com/ZYPLJ/p/18988989)

24.简单来讲讲C#中的锁09-20

收起

# 🔑 简单来讲讲C#中的锁

## ✨ 前言

今天来说说C#中的锁，锁在日常开发中还是很常用的，但是用的不得当，或者骚操作比较多那么就会导致死锁，从而导致系统崩溃。

后面我会出一系列文章，来讲讲C#里面的代码和技巧，通过不断的学习积累，以达到我的跳槽目标。

文章中有任何错误的地方都可以指出，博主也在不断的学习中~

## 📖 简述

下面问问`AI`来简单了解下什么是锁，`AI`分析还是挺详细的。

### 🧩 通俗理解

* 在现实生活里，**锁**就像一把门锁。
* 如果一个人进了房间并把门反锁，别人就得在外面等他出来。
* 等里面的人出来并开锁后，下一个人才能进去。

👉 编程里的“锁”也是一样的：
它保证**同一时刻只有一个线程能进入临界区（共享资源的代码块）**，从而避免混乱。

---

### ⚙️ 技术定义

在 **并发编程** 中，**锁（Lock）**是一种同步机制，用来 **控制多个线程对共享资源的并发访问**。

* 没有锁时：多个线程可能同时修改同一个变量、文件、数据库记录 → 造成 **数据竞争 (Race Condition)**。
* 有了锁后：一个线程进入临界区时，其它线程必须等待 → 保证 **数据一致性**。

### 🔒 锁的关键特性

1. **互斥性**
   * 一次只能有一个线程持有锁。
   * 其他线程必须等待。
2. **可见性**
   * 线程释放锁前的修改，对之后获取锁的线程是可见的。
3. **可重入性（C# 的 lock 是可重入的）**
   * 同一线程可以多次进入同一把锁，而不会死锁自己。

## 💡 举例

下面就举个例子讲讲什么情况下就需要用到锁。

```
int a = 0;

// 并行++ 
Parallel.For(0, 1000, _ => { 
    a++;
});

// 会发现 a 的值小于 1000，因为并行操作导致了线程之间的竞争。
Console.WriteLine(a);
```

正常情况下，a的值应该是等于`1000`的，但由于这里使用了`Parallel.For`，会导致多个线程对同一个值进行`++`操作，从而导致最终的结果没有1000次。

那么如何避免这种情况呢，可以使用锁去避免。

```
// 使用锁解决线程竞争问题
object obj = new object();

int b = 0;

Parallel.For(0, 1000, _ => { 
    lock (obj) {
        b++;
    }
});

// 现在 b 的值一定是 1000，因为锁确保了同一时间只有一个线程可以执行 b++ 操作。
Console.WriteLine(b);
```

这里使用了一个`object`类型作为锁对象，这也是常见的锁对象，不一定非得使用object类型，其他引用类型也行。

### 🖼️ 运行结果

[![1](https://img2024.cnblogs.com/blog/3091176/202509/3091176-20250920162959269-751912822.png)](https://img2024.cnblogs.com/blog/3091176/202509/3091176-20250920162959269-751912822.png)

## 🚀 .net9 新的锁对象

上面已经通过简单的例子了解到了什么是锁，已经怎么使用锁，那么在`.net9`中可以直接使用`lock`对象作为锁。

> ℹ️ 在使用`.net9`创建项目时，如果使用`object`类型的锁，`rider`编辑器会提示使用`Lock`类型作为锁。

### ⏳ 老写法

```
public class Lock
{
    private readonly object _lock = new();
    public void Foo()
    {
        lock (_lock)
        {
            Thread.Sleep(3000);
            Console.WriteLine("In Foo");
        }
    }
}
```

[![2](https://img2024.cnblogs.com/blog/3091176/202509/3091176-20250920163024532-1447890800.png)](https://img2024.cnblogs.com/blog/3091176/202509/3091176-20250920163024532-1447890800.png)

### ⚡ 新写法

```
public class Deadlock
{
    private readonly Lock _lock = new();
    public void Foo()
    {
        lock (_lock)
        {
            Thread.Sleep(3000);
            Console.WriteLine("In Foo");
        }
    }
}
```

### 📊 比较

| 特性 | 传统的 `object` + `lock(obj)`（Monitor-based） | 用 `System.Threading.Lock` + `lock(newLock)` |
| --- | --- | --- |
| 锁对象类型意图性 | 任意引用类型，不专门为锁“设计” | 专门的锁类型，用意明确 |
| 编译器识别/处理 | 用 `Monitor.Enter/Exit`； `lock(object)` 被编译器转换为 Monitor 操作 | 如果 `lock` 的目标是 `Lock` 类型，编译器 special-case 使用 `EnterScope()/Dispose()` 的新方式 |
| 内部机制 | 使用 `Monitor`、SyncBlocks、Thin locks 等，涉及 object header，可能有额外开销 | 新语义可以减少某些 Monitor 的开销，scope 块式释放锁，可能在某些场景性能更优 |
| 性能 | 在高并发且锁竞争严重的场景下性能可能成为瓶颈 | 在同样场景下可有更好的性能（但具体提升依赖于运行情况和 contention） |
| 可读性／安全性 | 用 `object`，可能误用；不容易一眼看出这是锁对象 | 用 `Lock` 类型，代码语义直接告诉你“这是用来加锁的” |

## 📝 总结

锁是并发编程里的“双刃剑”。

* **用得好** 👉 能保证线程安全，避免数据错乱。
* **用不好** 👉 容易掉进性能陷阱，甚至导致死锁，拖垮整个系统。

在 .NET 9 之前，我们习惯用 `object` 作为锁对象，但语义模糊，容易被误用。

而新的 `System.Threading.Lock` 专门为锁而生，让代码更直观，也在某些场景下带来性能提升。

所以：

* **写 demo、小项目** → 用 `lock(object)` 依旧没问题。
* **写业务、追求可维护性和性能** → 建议上手 .NET 9 的 `Lock`，让代码更优雅、更安全。

👉 学会合理使用锁，能让你的程序更加稳定，也能减少“背锅”的机会。

## 🔗 相关链接

* 【.NET 9新增的锁对象（Lock）是怎么一回事？】 [https://www.bilibili.com/video/BV1rLp2zMEua/?share\_source=copy\_web&vd\_source=fce337a51d11a67781404c67ec0b5084](https://github.com)

* [🔑 简单来讲讲C#中的锁](#-%E7%AE%80%E5%8D%95%E6%9D%A5%E8%AE%B2%E8%AE%B2c%E4%B8%AD%E7%9A%84%E9%94%81)
* [✨ 前言](#-%E5%89%8D%E8%A8%80)
* [📖 简述](#-%E7%AE%80%E8%BF%B0)
* [🧩 通俗理解](#-%E9%80%9A%E4%BF%97%E7%90%86%E8%A7%A3)
* [⚙️ 技术定义](#%EF%B8%8F-%E6%8A%80%E6%9C%AF%E5%AE%9A%E4%B9%89)
* [🔒 锁的关键特性](#-%E9%94%81%E7%9A%84%E5%85%B3%E9%94%AE%E7%89%B9%E6%80%A7)
* [💡 举例](#-%E4%B8%BE%E4%BE%8B)
* [🖼️ 运行结果](#%EF%B8%8F-%E8%BF%90%E8%A1%8C%E7%BB%93%E6%9E%9C)
* [🚀 .net9 新的锁对象](#-net9-%E6%96%B0%E7%9A%84%E9%94%81%E5%AF%B9%E8%B1%A1)
* [⏳ 老写法](#-%E8%80%81%E5%86%99%E6%B3%95)
* [⚡ 新写法](#-%E6%96%B0%E5%86%99%E6%B3%95)
* [📊 比较](#-%E6%AF%94%E8%BE%83)
* [📝 总结](#-%E6%80%BB%E7%BB%93)
* [🔗 相关链接](#-%E7%9B%B8%E5%85%B3%E9%93%BE%E6%8E%A5)

\_\_EOF\_\_

![](https://github.com/ZYPLJ)妙妙屋（zy） - **本文链接：** [https://github.com/ZYPLJ/p/19102575](https://github.com)
- **关于博主：** 评论和私信会在第一时间回复。或者[直接私信](https://github.com)我。
- **版权声明：** 本博客所有文章除特别声明外，均采用 [BY-NC-SA](https://github.com "BY-NC-SA") 许可协议。转载请注明出处！
- **声援博主：** 如果您觉得文章对您有帮助，可以点击文章右下角**【[推荐](javascript:void(0);)】**一下。
