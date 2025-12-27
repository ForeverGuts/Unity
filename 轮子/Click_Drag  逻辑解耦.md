---
tags:
  - 轮子
  - 交互
  - UI
aliases:
time: 2025-12-27
---
制作成预制体 将它挂载在需要的对象下  
使用脚本监听逻辑  
就可以解耦逻辑


==可以将Click与Drag的代码分为两个脚本==

![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224721-0.png)  

```
public class Click_Interface : MonoBehaviour, IBeginDragHandler, IDragHandler, IEndDragHandler, IPointerClickHandler

    {

        private event EventHandler onclick;

        //用于检测被鼠标点击 触发的逻辑

        public void OnDrag(PointerEventData eventData)

        {

            Debug.Log($"拖拽中: {gameObject.name}");

        }

        public void OnBeginDrag(PointerEventData eventData)

        {

            //TODO: 开始拖拽时的逻辑

        }

        public void OnEndDrag(PointerEventData eventData)

        {

        }

        public void OnPointerClick(PointerEventData eventData)

        {

            //TODO: 添加建筑的对应的Task到任务池中  如已经在池中 则升级任务池中该任务的优先级

           Debug.Log($"点击了: {gameObject.name}");

            OnClickLogic();

        }

        public void AddOnClickListener(EventHandler handler)

        {

            onclick += handler;

        }

        public void OnClickLogic()

        {

            onclick?.Invoke(this, EventArgs.Empty);

        }

        private void OnDestroy()

        {

            //移除所有监听

            onclick = null;

        }

    }
```


 ![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224723-1.png)