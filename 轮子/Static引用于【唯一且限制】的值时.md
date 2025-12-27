---
tags:
  - Csharp
aliases:
time: 2025-12-25
---
```
public static class Choices
{
    private static Choice currentChoice;
    public static void Set_Choice(Choice card)
    {
        currentChoice = card;
        InteractionConfiger.Instance.Show_Place();
    }
    public static void Clear_Choice()
    {
        currentChoice = null;
        InteractionConfiger.Instance.Conceal_Place();
    }
    public static Choice Get_Choice()
    {
        return currentChoice;
    }
}
```

![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224712-0.png)  
![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224716-1.png)  

比如对于手中卡牌的选取 使用静态保存 便于读取与使用