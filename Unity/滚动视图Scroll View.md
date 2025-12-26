---
tags:
  - Unity
  - UI
aliases:
time: 2025-12-26
---
# 主体
![[Pasted image 20251226165857.png]]
# Scroll View对象
作为Scroll View组件的载体,修改滚动视图的模式

并且可以通过修改Recttransform修改视图的整体尺寸
![[Pasted image 20251226165935.png]]
# Viewport对象

作为遮盖Mask,它的尺寸,image变量不会对滚动视图造成影响

最好不要尝试通过改动Recttransform来修改视图画面
![[Pasted image 20251226165952.png]]
# Content对象

作为内容的载体,不具备任何组件,内容可以作为子对象，在其之下

通过修改该RectTransform的高度  可以修改曲柄的大小
![[Pasted image 20251226170004.png]]
假如Content的内容用GridLayoutGroup管理  
可以使用以下代码自动修改曲柄尺寸
![[Pasted image 20251226170029.png]]
![[Pasted image 20251226170032.png]]