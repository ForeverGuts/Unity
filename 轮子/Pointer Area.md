---
tags:
  - 交互
  - UI
aliases:
time: 2025-12-25
---

# 对于UI与2D场景中 希望建立白名单区域或者黑名单区域  
当鼠标放置在区域之上 便获取到信号

![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224630-0.png)  
![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224635-1.png)

(默认关闭Image与BoxCollider2D 避免出现错误阻挡)  
但是启动的时候需要有Image组件与Collider组件参与

![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224638-2.png)   
# ==Area==
```
using UnityEngine.UI;

public class Area : MonoBehaviour, IPointerEnterHandler, IPointerExitHandler

{

    public AreaName areaName;

    public bool isInArea = false;

    public Image placeImage;

    public Collider2D place;

    private void Awake()

    {

        placeImage = GetComponent<Image>();

        place = GetComponent<Collider2D>();

    }

    public void OnPointerEnter(PointerEventData eventData)

    {

        isInArea = true;

    }

    public void OnPointerExit(PointerEventData eventData)

    {

        isInArea = false;

    }

}


```

# ==InteractionConfiger==
```
 using UnityEngine.UI;

//在管理器中 使用字典与枚举 方便获取某个区域的激活信息

public enum AreaName

{

    Place,

    Other

}

/// <summary>

    /// 将含有Area组件的子物体加入字典Areas中

    /// 根据Area组件中的Area字段作为键

    /// </summary>

public class InteractionConfiger : BaseManager<InteractionConfiger>

{

    public Dictionary<AreaName, Area> Areas = new Dictionary<AreaName, Area>();

    protected override void Init()

    {

        Area[] areas = transform.GetComponentsInChildren<Area>(true);

        foreach (Area area in areas)

        {

            if (Areas.ContainsKey(area.areaName))

            {

                Debug.LogError("InteractionArea Init Error: 存在重复的Area键 " + area.areaName);

                continue;

            }

            Areas.Add(area.areaName, area);

        }

    }

 /// <summary>

    /// 隐藏指定区域

    /// </summary>

    /// <param name="areaName"></param>

    public void Conceal_Area(AreaName areaName)

    {

        if (Areas.ContainsKey(areaName))

        {

            Areas[areaName].placeImage.enabled = false;

            Areas[areaName].place.enabled = false;

        }

        else

        {

            Debug.LogError("InteractionArea Conceal_Area Error: 不存在该Area键 " + areaName);

        }

    }

    /// <summary>

    /// 显示指定区域

    /// </summary>

    /// <param name="areaName"></param>

    public void Show_Area(AreaName areaName)

    {

        if (Areas.ContainsKey(areaName))

        {

            Areas[areaName].placeImage.enabled = true;

            Areas[areaName].place.enabled = true;

        }

        else

        {

            Debug.LogError("InteractionArea Show_Area Error: 不存在该Area键 " + areaName);

        }

    }

}
```
 ![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224640-3.png)   ![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224644-4.png)