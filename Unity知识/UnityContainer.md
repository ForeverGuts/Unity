---
tags:
  - Unity
  - 可扩展
  - 设计模式
aliases:
time: 2025-12-29
---
# ==使用限制(后文还有注意事项)==
最主要的限制来自某些**移动和主机平台（如 iOS、Android IL2CPP 后端等）**。这些平台要求代码**提前编译（AOT）**，不允许在运行时动态生成代码[](https://docs.unity3d.com/cn/2017.4/Manual/ScriptingRestrictions.html)。这会影响依赖注入容器在运行时通过反射动态创建类型的能力。
![](assets/UnityContainer/file-20251229210817594.png)
![](assets/UnityContainer/file-20251229210817594%201.png)

# ==概念==
![](assets/UnityContainer/file-20251229211215414.png)
![](assets/UnityContainer/file-20251229213719287.png)![](assets/UnityContainer/file-20251229213730769.png)![](assets/UnityContainer/file-20251229213737108.png)![](assets/UnityContainer/file-20251229213742276.png)![](assets/UnityContainer/file-20251229213759927.png)![](assets/UnityContainer/file-20251229213808975.png)![](assets/UnityContainer/file-20251229213820834.png)![](assets/UnityContainer/file-20251229213826570.png)![](assets/UnityContainer/file-20251229213831228.png)
# ==注意事项==
![](assets/UnityContainer/file-20251229213840464.png)