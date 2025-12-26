---
tags:
  - Collider
  - Trigger
aliases:
time: 2025-12-26
---

[[Collider2D]]在Inspector的==IsTrigger==勾选为true后启用
![[Pasted image 20251226153405.png]]![[Pasted image 20251226153407.png]]
建议==父物体==使用==碰撞体==

==子物体==单独使用多个==触发器==
![[Pasted image 20251226153409.png]]
![[Pasted image 20251226153411.png]]![[Pasted image 20251226153413.png]]![[Pasted image 20251226153416.png]]![[Pasted image 20251226153418.png]]![[Pasted image 20251226153420.png]]物体本体只有一个IsTrigger的Collider2D   脚本中的Ontrigger2D只检测本物体的Collider2D  .子物体的IsTrigger的Collider2D不会检测