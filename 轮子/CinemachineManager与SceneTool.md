---
tags:
  - 轮子
  - Camera
  - Scene
aliases:
time: 2025-12-27
---
[[Cinemachine]]

# 1.==CinamachineManager==脚本

```
public class CinemachineManager : BaseManager<CinemachineManager>

{

    [SerializeField] private CinemachineBrain brain;

    [SerializeField]private CinemachineVirtualCamera Main;

    [SerializeField]private CinemachineVirtualCamera RegularWater;

    [SerializeField]private CinemachineVirtualCamera Plant;

    [SerializeField]private CinemachineVirtualCamera Sacrifice;

    protected override void Init()

    {

    }

    /// <summary>

    /// 将指定的虚拟摄像机优先级拉高  将其他摄像机优先级设置为低

    /// </summary>

    /// <param name="_name"></param>

    public void OpenCamera(VirtualCameraName _name)

    {

        switch (_name)

        {

            case VirtualCameraName.Main:

                SetCameraActive(Main);

                break;

            case VirtualCameraName.RegularWater:

                SetCameraActive(RegularWater);

                break;

            case VirtualCameraName.Plant:

                SetCameraActive(Plant);

                break;

            case VirtualCameraName.Sacrifice:

                SetCameraActive(Sacrifice);

                break;

            default:

                Debug.LogError("Unknown virtual camera name: " + _name);

                break;

        }

    }

    private void SetCameraActive(CinemachineVirtualCamera camera)

    {

        if (camera != null)

        {

            // Reset all cameras to default priority

            if (Main != null) Main.Priority = 1;

            if (RegularWater != null) RegularWater.Priority = 1;

            if (Plant != null) Plant.Priority = 1;

            if (Sacrifice != null) Sacrifice.Priority = 1;

            // Set the selected camera to a higher priority to make it active

            camera.Priority = 10;

        }

        else

        {

            Debug.LogError("Attempted to activate a null camera.");

        }

    }

    public void RigistVirtualCamera(VirtualCameraName _name, CinemachineVirtualCamera _virtualCamera)

    {

        switch (_name)

        {

            case VirtualCameraName.Main:

                Main = _virtualCamera;

                break;

            case VirtualCameraName.RegularWater:

                RegularWater = _virtualCamera;

                break;

            case VirtualCameraName.Plant:

                Plant = _virtualCamera;

                break;

            case VirtualCameraName.Sacrifice:

                Sacrifice = _virtualCamera;

                break;

            default:

                Debug.LogError("Unknown virtual camera name: " + _name);

                break;

        }

    }

    public void SetBrain(CinemachineBrain _brain)

    {

        if (_brain != null)

        {

            brain = _brain;

        }

        else

        {

            Debug.LogError("CinemachineBrain is null, cannot set brain.");

        }

    }

    public CinemachineBrain GetBrain()

    {

        return brain;

    }

}

public enum VirtualCameraName

{

    Main,

    RegularWater,

    Plant,

    Sacrifice

}
```
---

# 2.==VirtualCamera==脚本
```
using System.Collections;

using System.Collections.Generic;

using UnityEngine;

using Cinemachine;

public class VirtualCameraController : MonoBehaviour

{

    [SerializeField] private CinemachineVirtualCamera virtualCamera;

    public VirtualCameraName VirtualCameraName;//虚拟摄像机名称

    private int priority = 1; // 默认优先级

    private void Awake()

    {

        virtualCamera = GetComponent<CinemachineVirtualCamera>();

        virtualCamera.Priority = priority;

        CinemachineManager.Instance.RigistVirtualCamera(VirtualCameraName, virtualCamera);

    }

}
```
---

**带有Brain组件的物件不能DontDestroyed  但是可以在Awake把引用注册到单例中**
# 3.==CinemachineBrain==脚本
```
using System.Collections;

using System.Collections.Generic;

using UnityEngine;

public class CinemachineBrain : MonoBehaviour

{

    [SerializeField] private CinemachineBrain brain;

    private void Awake()

    {

        brain = GetComponent<CinemachineBrain>();

        CinemachineManager.Instance.SetBrain(brain);

    }

}
```


---

**切换场景时 为了获取加入场景中的摄像机等资源**  

**可以通过以下方法实现**

**（以下内容 都是在VirtualCamera在Awake()中自动注册到单例中  详见下面代码）**

**1.直接在SceneLoad中挂载事件  因为SceneLoad是在当场景中物体完全加载后**

**再执行的回调**

**2.在加载场景的协程逻辑中  等待现实时间 然后执行**
# 4.**==SceneTool==**脚本

```
using System.Collections;

using System.Collections.Generic;

using Unity.VisualScripting;

using UnityEngine;

using UnityEngine.SceneManagement;

public class SceneTool : BaseTool<SceneTool>

{

    protected override void Init()

    {

        SceneManager.sceneLoaded += OnSceneLoaded;

    }

    protected void OnDestroy()

    {

        SceneManager.sceneLoaded -= OnSceneLoaded; // 取消注册事件

    }

    /// <summary>

    /// 加载场景 并且关闭不必要场景

    /// </summary>

    /// <param name="_sceneName"></param>

    public void LoadScene(SceneName _sceneName)

    {

        CoroutineRunner.Instance.StartCoroutine(IE_LoadScene(_sceneName));

    }

    public IEnumerator IE_LoadScene(SceneName _sceneName)

    {

        if (SceneManager.GetActiveScene().name == _sceneName.ToString())

        {

            //Debug.Log("当前场景已是 " + _sceneName.ToString() + "，无需加载。");

            yield break; // 如果当前场景已经是目标场景，则不进行加载

        }

if(SceneManager.GetSceneByName(_sceneName.ToString()).isLoaded)

        {

           yield break; // 如果已经加载了目标场景，则不进行加载

        }

        AsyncOperation _asynicLoad = SceneManager.LoadSceneAsync(_sceneName.ToString(), LoadSceneMode.Additive);

        VirtualCameraName virtualCameraName;

        switch (_sceneName)

        {

            case SceneName.Main:

                UnLoadScene(SceneName.RegularWater);

                UnLoadScene(SceneName.Sacrifice);

                UnLoadScene(SceneName.Plant);

                virtualCameraName = VirtualCameraName.Main;

                break;

            case SceneName.RegularWater:

                UnLoadScene(SceneName.Sacrifice);

                UnLoadScene(SceneName.Plant);

                virtualCameraName = VirtualCameraName.RegularWater;

                break;

            case SceneName.Sacrifice:

                UnLoadScene(SceneName.RegularWater);

                UnLoadScene(SceneName.Plant);

                virtualCameraName = VirtualCameraName.Sacrifice;

                break;

            case SceneName.Plant:

                UnLoadScene(SceneName.RegularWater);

                UnLoadScene(SceneName.Sacrifice);

                virtualCameraName = VirtualCameraName.Plant;

                break;

            default:

                Debug.LogError("未处理的场景名称: " + _sceneName.ToString());

                virtualCameraName = VirtualCameraName.Main; // 默认使用Main摄像机

                break;

        }

        //AsyncOperation _asynicLoad = SceneManager.LoadSceneAsync(_sceneName.ToString(), LoadSceneMode.Additive);

        //Debug.Log("开始加载场景 : " + _sceneName.ToString());

        _asynicLoad.allowSceneActivation = false;

        while (_asynicLoad.progress < 0.9f)

        {

            //Debug.Log(_asynicLoad.progress);

            yield return null;

        }

        _asynicLoad.allowSceneActivation = true;

        yield return new WaitForSeconds(0.01f);

        //yield return null;

        //CinemachineManager.Instance.OpenCamera(virtualCameraName);

        yield break;

        // Debug.Log("成功加载场景 : " + _sceneName.ToString());

    }

    /// <summary>

    /// 卸载指定场景 若场景未加载则不进行操作

    /// </summary>

    /// <param name="_sceneName"></param>

    private void UnLoadScene(SceneName _sceneName)

    {

        Scene targetScene = SceneManager.GetSceneByName(_sceneName.ToString());

        if (targetScene.isLoaded)

        {

            AsyncOperation unloadOp = SceneManager.UnloadSceneAsync(targetScene);

            // 可监听 unloadOp.completed  事件

        }

        else

        {

            Debug.LogWarning("场景未加载，无需卸载");

        }

    }

    /// <summary>

    /// 检查场景是否添加到Build Settings中

    /// </summary>

    /// <param name="_sceneName"></param>

    public void CheckAsset(SceneName _sceneName)

    {

        if (SceneUtility.GetBuildIndexByScenePath("Assets/Scenes/RegularWater" + _sceneName) == -1)

        {

            Debug.LogError(_sceneName + "  场景未添加到Build Settings！");

        }

    }

    private void OnSceneLoaded(Scene scene, LoadSceneMode mode)

    {

        switch (scene.name)

        {

            case "Main":

                CinemachineManager.Instance.OpenCamera(VirtualCameraName.Main);

                break;

            case "RegularWater":

                CinemachineManager.Instance.OpenCamera(VirtualCameraName.RegularWater);

                break;

            case "Sacrifice":

                CinemachineManager.Instance.OpenCamera(VirtualCameraName.Sacrifice);

                break;

            case "Plant":

                CinemachineManager.Instance.OpenCamera(VirtualCameraName.Plant);

                break;

            default:

                Debug.LogWarning("未知场景加载: " + scene.name);

                break;

        }

        //Debug.Log("场景加载完成: " + scene.name);

    }

}

public enum SceneName

{

    Main,

    RegularWater,

    Sacrifice,

    Plant

}
```
