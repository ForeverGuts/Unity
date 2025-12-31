---
tags:
  - Unity
  - 设计模式
aliases:
time: 2025-12-29
---
[依赖注入](../编程思想/设计框架/依赖注入.md)
[反转控制](../编程思想/设计框架/反转控制.md)
[UnityContainer(下)](UnityContainer(下).md)
# ==使用限制(后文还有注意事项)==
最主要的限制来自某些**移动和主机平台（如 iOS、Android IL2CPP 后端等）**。这些平台要求代码**提前编译（AOT）**，不允许在运行时动态生成代码[](https://docs.unity3d.com/cn/2017.4/Manual/ScriptingRestrictions.html)。这会影响依赖注入容器在运行时通过反射动态创建类型的能力。
![](assets/UnityContainer(上)/file-20251229214835994.png)
![](assets/UnityContainer(上)/file-20251229214835975.png)

# ==概念==
![](assets/UnityContainer(上)/file-20251229214835974%201.png)
![](assets/UnityContainer(上)/file-20251229214835974.png)![](assets/UnityContainer(上)/file-20251229214835973%202.png)![](assets/UnityContainer(上)/file-20251229214835973%201.png)![](assets/UnityContainer(上)/file-20251229214835973.png)![](assets/UnityContainer(上)/file-20251229214835972.png)![](assets/UnityContainer(上)/file-20251229214835971%201.png)![](assets/UnityContainer(上)/file-20251229214835971.png)![](assets/UnityContainer(上)/file-20251229214835970%201.png)![](assets/UnityContainer(上)/file-20251229214835970.png)
# ==注意事项==
![](assets/UnityContainer(上)/file-20251229214835969.png)