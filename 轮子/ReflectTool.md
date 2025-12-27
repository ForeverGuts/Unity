---
tags:
  - 轮子
  - Csharp
aliases:
time: 2025-12-27
---
# ==反射 工具==  

```
public class ReflectTool : BaseTool\<ReflectTool\>
{
//打印按钮的监听事件数 (代码挂载的)  
    public int PrintButtonListeners(Button button)
    {
        
        UnityEventBase unityEventBase = button.onClick;
        FieldInfo callsField = typeof(UnityEventBase).GetField("m_Calls", BindingFlags.Instance | BindingFlags.NonPublic);
        object calls = callsField.GetValue(unityEventBase);
        FieldInfo runtimeCallsField = calls.GetType().GetField("m_RuntimeCalls", BindingFlags.Instance | BindingFlags.NonPublic);
        object runtimeCalls = runtimeCallsField.GetValue(calls);
        var runtimeCallsList = runtimeCalls as System.Collections.IList;
        if (runtimeCallsList != null)
        {
            //Debug.Log(runtimeCallsList.Count);
            for (int i = 0; i \< runtimeCallsList.Count; i++)
            {
                var call = runtimeCallsList[i];
                FieldInfo targetField = call.GetType().GetField("m_Target", BindingFlags.Instance | BindingFlags.NonPublic);
                FieldInfo methodField = call.GetType().GetField("m_MethodName", BindingFlags.Instance | BindingFlags.NonPublic);
                //object target = targetField.GetValue(call);
                //string methodName = methodField.GetValue(call) as string;
                //Debug.Log($"Listener {i}: {target.GetType().Name}.{methodName}");
            }
            return runtimeCallsList.Count;
        }
        else
        {
            Debug.LogError("Failed to get runtime calls.");
        }
        return 0;
    }
}
```