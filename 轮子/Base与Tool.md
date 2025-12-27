---
tags:
  - 轮子
aliases:
time: 2025-12-25
---

# 泛型 单例

---

using System.Collections;
using System.Collections.Generic;
using UnityEngine;
//BaseManager
public class BaseManager<T> : MonoBehaviour where T : MonoBehaviour
{
    private static T _instance;
    public static T Instance
    {
        //被调用时执行
        get
        {
            if (_instance == null)
            {
                //寻找场景中的单例对象
                _instance = FindObjectOfType<T>();
                if (_instance == null)
                {
                    //找不到就创建一个新的
                    GameObject singletonObject = new GameObject(typeof(T).Name);
                    _instance = singletonObject.AddComponent<T>();
                }
            }
            return _instance;
        }
    }
    //Instance有可能在Awake()执行前被调用导致空引用  所以在Instance的Set中实例更加安全
    protected virtual void Awake()
    {
        if (_instance == null)
        {
            _instance = this as T;
            DontDestroyOnLoad(gameObject);
            Init();
        }
        else
        {
            Destroy(gameObject);
        }
    }
    protected virtual void Init() { }
}
//BaseTool
public abstract class BaseTool<T> where T : BaseTool<T>, new()
{
    private static T _instance;
    public static T Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new T();
                _instance.Init();
            }
            return _instance;
        }
    }
    //析构函数中调用OnDestroy()
    protected abstract void Init();
    protected abstract void OnDestroy();
    ~BaseTool()
    {
        OnDestroy();
    }
}