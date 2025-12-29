---
tags:
  - 状态机
aliases:
  - FSM
time: 2025-12-25
---



# 状态机代码块
```
using UnityEngine;

[Serializable]

public class StateMachine

{

    //使用流程

    //0.写stateMachine上的状态字典   构造函数逻辑(初始化黑板  字典  获取root) 切换逻辑  添加状态逻辑  OnUpdate()逻辑

    //1.创建机子

    //StateMachine stateMachine = new StateMachine();

    //2.添加状态

    //stateMachine.AddState(E_State.defalt, new State_Enemy_Default());

    //3.Update上调用stateMachine.OnUpdate();

    //4.写所需状态的逻辑

    //public class State_Enemy_Default : StateLogic

    //{

    //    public override void StateStart(StateMachine _stateMachine)

    //    public override void StateUpdate(StateMachine _stateMachine)

    //    public override void StateEnd(StateMachine _stateMachine)

    //}

    //5.必要时 使用[Serializable] BlackBoard_gameObject :BlackBoard传递数据  或者 显示数据

    public BlackBoard blackBoard;

    //public BlackBoard blackBoard;

    //public Transform root;

    public E_State currentState;

    public Dictionary<E_State, StateLogic> statesDic;

    public StateMachine(Transform _root)

    {

        blackBoard = new BlackBoard();

        blackBoard.root = _root;

        statesDic = new Dictionary<E_State, StateLogic>();

    }

    public void AddState(E_State _state, StateLogic _stateLogic)

    {

        if (!statesDic.ContainsKey(_state))

        {

            statesDic.Add(_state, _stateLogic);

        }

    }

    public void OnUpdate()

    {

        statesDic[currentState].StateUpdate(this);

    }

    public void SwitchState(E_State _nState)

    {

        if (statesDic.ContainsKey(_nState))

        {

            if (currentState != _nState)

            {

                //Debug.Log("SwitchState: " + currentState + " -> " + _nState);

                statesDic[currentState].StateEnd(this);

                currentState = _nState;

                statesDic[currentState].StateStart(this);

            }

        }

    }

    public void DebugDicCount()

    {

        Debug.Log("statesDic.Count: " + statesDic.Count);

    }

}

public enum E_State

{

    defalt,

    aim,

    attack,

}

public class StateLogic

{

    public virtual void StateStart(StateMachine _stateMachine)

    {

    }

    public virtual void StateUpdate(StateMachine _stateMachine)

    {

    }

    public virtual void StateEnd(StateMachine _stateMachine)

    {

    }

}

[Serializable]

public class BlackBoard

{

    //public GameObject target;

    public Transform root;

}
```

---

# 个体使用代码块
```
using UnityEngine;

public class StateController_Enemy : MonoBehaviour

{

    public StateMachine stateMachine = null;

    private void Start()

    {

        stateMachine = new StateMachine(transform.parent);

        stateMachine.AddState(E_State.defalt, new State_Enemy_Default());

        stateMachine.AddState(E_State.aim, new State_Enemy_Aim());

        //stateMachine.DebugDicCount();

        stateMachine.SwitchState(E_State.defalt);

    }

}

public class State_Enemy_Default : StateLogic

{

    public override void StateStart(StateMachine _stateMachine)

    {

        Debug.Log("Enemy_Default");

    }

    public override void StateUpdate(StateMachine _stateMachine)

    {

    }

    public override void StateEnd(StateMachine _stateMachine)

    {

    }

}

public class State_Enemy_Aim : StateLogic

{

    public override void StateStart(StateMachine _stateMachine)

    {

        Debug.Log("Enemy_Aim");

    }

    public override void StateUpdate(StateMachine _stateMachine)

    {

    }

    public override void StateEnd(StateMachine _stateMachine)

    {

    }

}
```
