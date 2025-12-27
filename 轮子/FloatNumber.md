![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224652-0.png)

特效字体 浮动或者渐隐字体
 
```
using UnityEngine;
using UnityEngine.UI;
public class FloatNumber : MonoBehaviour
{
    //
```

父对象  

```
    [SerializeField] private RectTransform BoundNums;
    [SerializeField] private RectTransform UpwardNums;
    //
```

修改`FloatNum`的初始位移  

```
    [SerializeField] private Vector3 offset_Bound;
    [SerializeField] private Vector3 offset_Upward;
    [SerializeField] private GameObject BoundNum_Prefab;
    [SerializeField] private GameObject UpwardNum_Prefab;
    //FloatNum
```

的对象池  

```
    [SerializeField] private List\<BoundNum\> boundNums_Sequence = new List\<BoundNum\>();
    [SerializeField] private List\<UpwardNum\> upwardNums_Sequence = new List\<UpwardNum\>();
```
 
```
    private void Awake()
    {
        BoundNums = transform.Find("BoundNums").GetComponent\<RectTransform\>();
        UpwardNums = transform.Find("UpwardNums").GetComponent\<RectTransform\>();
        //upwardNums_Null
    }
    /// \<summary\>
    /// 
```

从对象池中获取一个`UpwardNum`对象  
    `///` 若不足则生成`10`个新的`BoundNum`存储在对象池中  

```
    /// \</summary\>
    /// \<param name="worldPos"\>\</param\>
    /// \<param name="text"\>\</param\>
    public void DeSequence_BoundNum(Vector3 worldPos, string text)
    {
        if (boundNums_Sequence.Count == 0)
        {
            for (int i = 0; i \< 10; i++)
            {
                BoundNum newBoundNum = Instantiate(BoundNum_Prefab, BoundNums).GetComponent\<BoundNum\>();
                boundNums_Sequence.Add(newBoundNum);
                newBoundNum.transform.gameObject.SetActive(false);
                newBoundNum.transform.SetParent(BoundNums);
                //Init_BoundNum(newBoundNum, worldPos, text);
            }
        }
        BoundNum boundNum = boundNums_Sequence[0];
        boundNums_Sequence.RemoveAt(0);
        boundNum.transform.SetParent(BoundNums);
        boundNum.transform.gameObject.SetActive(true);
        Init_BoundNum(boundNum, worldPos, text);
    }
    /// \<summary\>
    /// 
```

将使用完毕的`BoundNum`放回对象池  

```
    /// \</summary\>
    /// \<param name="boundNum"\>\</param\>
    public void EnSuquence_BoundNum(BoundNum boundNum)
    {
        boundNum.Reset();
        boundNums_Sequence.Add(boundNum);
        boundNum.transform.SetParent(BoundNums);
        boundNum.transform.gameObject.SetActive(false);
    }
    /// \<summary\>
    /// 
```

在指定世界坐标位置生成随机方向跳动的字符串 然后慢慢消失  

```
    /// \</summary\>
    /// \<param name="boundNum"\>\</param\>
    /// \<param name="worldPos"\>\</param\>
    /// \<param name="text"\>\</param\>
    private void Init_BoundNum(BoundNum boundNum, Vector3 worldPos, string text)
    {
        //boundNum = Instantiate(BoundNum_Prefab, worldPos, Quaternion.identity);
        boundNum.transform.position = worldPos;
        boundNum.transform.SetParent(BoundNums);
        Vector3 screenPos = Camera.main.WorldToScreenPoint(worldPos + offset_Bound);
        RectTransformUtility.ScreenPointToLocalPointInRectangle(
            BoundNums,
            screenPos,
            null,
            out Vector2 localPoint
        );
        boundNum.GetComponent\<RectTransform\>().localPosition = localPoint;
        boundNum.GetComponent\<BoundNum\>().Init(text);
        // Add random movement and fading logic here
    }
    /// \<summary\>
    /// 
```

在指定世界坐标位置生成向上渐移的字符串 然后慢慢消失  
    `///` 若不足则生成`10`个新的`BoundNum`存储在对象池中

```
 
    /// \</summary\>
    /// \<param name="worldPos"\>\</param\>
    /// \<param name="text"\>\</param\>
    public void DeSequence_UpwardNum(Vector3 worldPos, string text)
    {
        if (upwardNums_Sequence.Count == 0)
        {
            for (int i = 0; i \< 10; i++)
            {
                UpwardNum newUpwardNum = Instantiate(UpwardNum_Prefab, UpwardNums).GetComponent\<UpwardNum\>();
                upwardNums_Sequence.Add(newUpwardNum);
                newUpwardNum.transform.gameObject.SetActive(false);
                newUpwardNum.transform.SetParent(UpwardNums);
            }
        }
        UpwardNum upwardNum = upwardNums_Sequence[0];
        upwardNums_Sequence.RemoveAt(0);
        upwardNum.transform.SetParent(UpwardNums);
        upwardNum.transform.gameObject.SetActive(true);
        Init_UpwardNum(upwardNum, worldPos, text);
    }
    /// \<summary\>
    /// 
```

将使用完毕的`UpwardNum`放回对象池  

```
    /// \</summary\>
    /// \<param name="upwardNum"\>\</param\>
    public void EnSuquence_UpwardNum(UpwardNum upwardNum)
    {
        upwardNum.Reset();
        upwardNums_Sequence.Add(upwardNum);
        upwardNum.transform.SetParent(UpwardNums);
        upwardNum.transform.gameObject.SetActive(false);
    }
    /// \<summary\>
    /// 
```

生成向上渐移的字符串 然后慢慢消失  

```
    /// \</summary\>
    /// \<param name="upwardNum"\>\</param\>
    /// \<param name="worldPos"\>\</param\>
    /// \<param name="text"\>\</param\>
    private void Init_UpwardNum(UpwardNum upwardNum, Vector3 worldPos, string text)
    {
        upwardNum.transform.position = worldPos;
        upwardNum.transform.SetParent(UpwardNums);
        Vector3 screenPos = Camera.main.WorldToScreenPoint(worldPos + offset_Upward);
        RectTransformUtility.ScreenPointToLocalPointInRectangle(
            UpwardNums,
            screenPos,
            null,
```

//覆盖模式则为Null 摄像机模式为Camera.Main  

```
            out Vector2 localPoint
        );
        upwardNum.GetComponent\<RectTransform\>().localPosition = localPoint;
        upwardNum.GetComponent\<UpwardNum\>().Init(text);
        // Add upward movement and fading logic here
    }
}
```
   
![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224654-1.png)   
```
using TMPro;
using UnityEngine;
public class BoundNum : MonoBehaviour
{
    [SerializeField] private Vector2 dir;
    [SerializeField] private float power;
    [SerializeField] private float duration = 1f;
    [SerializeField] private float timer = 0f;
    [SerializeField] private TextMeshProUGUI tmp;
    private void Awake()
    {
        tmp = GetComponent\<TextMeshProUGUI\>();
        Reset();
    }
    private void GetRandom()
    {
        //dir
```

中`y`必须为`1  x`可以为正负

```
1
        dir = new Vector2(Random.Range(-1f, 1f), 1f).normalized;
        power = Random.Range(100f, 120f) * 100f;
        duration = Random.Range(3f, 4f);
    }
    /// \<summary\>
    /// 
```

开始执行跳跃效果  

```
    /// \</summary\>
    /// \<param name="text"\>\</param\>
    public void Init(string text)
    {
        tmp.text = text;
        GetRandom();
        StartCoroutine(bound());
        IEnumerator bound()
        {
            
            float elapsed = 0f;
            Vector3 startPos = transform.localPosition;
            Color startColor = tmp.color;
            while (elapsed \< duration)
            {
                float t = elapsed / duration;
                timer = elapsed;
                // Move in the given direction with power, decelerating over time
                // Parabolic (projectile) motion: initial velocity in dir*power, affected by gravity
                // Assume gravity acts downward on y axis
                float gravity = -98f *2f;
                // Initial velocity
                Vector2 velocity = dir.normalized * power ;
                // Calculate new position
                float newX = startPos.x + velocity.x * t;
                //s = v₀t+½gt²
                float newY = startPos.y + velocity.y * t + 0.5f * gravity * t * t;
                transform.localPosition = new Vector3(newX, newY, startPos.z);
              
            
                // Fade out
                tmp.color = new Color(startColor.r, startColor.g, startColor.b, 1 - t);
                elapsed += Time.deltaTime;
                yield return null;
            }
            // Ensure fully transparent and optionally destroy
            tmp.color = new Color(startColor.r, startColor.g, startColor.b, 0);
            //Debug.Log("
```

消失

```
");
            //Destroy(gameObject);
        }
    }
    public void Reset()
    {
        StopAllCoroutines();
        dir = Vector2.zero;
        power = 0f;
        duration = 0f;
        timer = 0f;
        tmp.text = "
```

进入对象池

```
";
        tmp.color = new Color(tmp.color.r, tmp.color.g, tmp.color.b, 0);
    }
}
```

![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224700-2.png)   
```
using UnityEngine;
using TMPro;
public class UpwardNum : Num
{
    [SerializeField] private float power;
    [SerializeField] private float duration = 1f;
    [SerializeField] private float timer = 0f;
    [SerializeField] private TextMeshProUGUI tmp;
    private void Awake()
    {
        tmp = GetComponent\<TextMeshProUGUI\>();
        Reset();
    }
    private void GetRandom()
    {
        power = 800f;
        duration = 2f;
    }
    /// \<summary\>
    /// 
```

开始执行向上渐移并淡出的效果  

```
    /// \</summary\>
    /// \<param name="text"\>\</param\>
    public override void Init(string text)
    {
        tmp.text = text;
        GetRandom();
        StartCoroutine(bound());
        IEnumerator bound()
        {
            float elapsed = 0f;
            Vector3 startPos = transform.localPosition;
            Color startColor = tmp.color;
            float t = 0f;
            while (elapsed \< duration)
            {
                t = elapsed / duration;
                timer = elapsed;
                // Calculate new position
                float newX = startPos.x;
                // 
```

匀减速运动

```
: s = v0 * t - 0.5 * a * t^2
                float a = power / duration; // 
```

使末速度为

```
0
                float newY = startPos.y + power * t - 0.5f * a * t * t;
                transform.localPosition = new Vector3(newX, newY, startPos.z);
                // Fade out
                tmp.color = new Color(startColor.r, startColor.g, startColor.b, 1 - t);
                elapsed += Time.deltaTime;
                yield return null;
            }
            // Ensure fully transparent and optionally destroy
            tmp.color = new Color(startColor.r, startColor.g, startColor.b, 0);
            //
```

放入对象池  

```
            UIManager.Instance.floatNumber.EnSuquence_UpwardNum(this);
            //Debug.Log("
```

消失

```
");
            //Destroy(gameObject);
        }
    }
    public override void Reset()
    {
        power = 0;
        duration = 0;
        timer = 0;
        tmp.text = "
```

进入对象池

```
";
        tmp.color = new Color(tmp.color.r, tmp.color.g, tmp.color.b, 0f);
    }
}
```

![Exported image](%E9%99%84%E4%BB%B6/Exported%20image%2020251227224702-3.png)