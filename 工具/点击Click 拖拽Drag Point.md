---
tags:
  - UI
  - Unity
aliases:
time: 2025-12-26
---
前置：# [[世界_屏幕_UI_GUI坐标]]
[[UI物体互动的检测流程]]
[[Click_Drag  逻辑解耦]]
1.内置OnMouse衍生方法 (针对2D贴图)  
OnMouseDrag()

OnMouseDown()  
OnMouseUp()

OnMouseOver()

OnMouseEnter()

OnMouseExit()

OnMouseUpAsButton()
[类似OnTrigger衍生方法]
![[Pasted image 20251226164503.png]]
如果对于UI  GUI 对象 推荐使用2. EventTrigger或者接口3.Interface（GUI现在在Unity文档中过旧被移除了）

# 2.EventTrigger(针对UI)  
为UI物体 加入EventTrigger组件 可以设置其中的事件
![[Pasted image 20251226164521.png]]
### ==注意==:transform为UI物体  处于UI坐标系   Input处于屏幕坐标系
### 但是缺可以直接赋值（前提是摄像机类型为 OverLay      因为overlay是让UI画布被固定了   始终面对屏幕）
![[Pasted image 20251226164558.png]]![[Pasted image 20251226164600.png]]
IPointerClick是用于管理点击的接口  IDrag适用于鼠标拖拽的接口

如果一个物体同时挂载了多个Collider 可以尝试将优先级更高的点击Collider的Z轴降低(靠近摄像机)

   条件：如果使用于游戏场景1.需要在主相机中添加组件 PhysicsRaycaster2D / PhysicsRaycaster3D

 2.游戏物体上增加组件Collider2D/Collider3D

 3.为了避免报错 EventSystem最好
 ![[Pasted image 20251226164615.png]]![[Pasted image 20251226164617.png]]![[Pasted image 20251226164620.png]]![[Pasted image 20251226164622.png]]
 Ipointer
 ![[Pasted image 20251226164751.png]]![[Pasted image 20251226164753.png]]![[Pasted image 20251226164756.png]]![[Pasted image 20251226164759.png]]