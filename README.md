# TheAnswerForLeChengBao
第一题：
using System.Collections;
using System.Xml;
using UniRx;
using UnityEngine;
public static class MoveUtil
{

    public static void easeIn(this GameObject gameObject,Vector3 begin, Vector3 end, float time)
    {
        MainThreadDispatcher.StartCoroutine(
            move(gameObject,begin,end,time,false));
    }
    public static void easeOut(this GameObject gameObject,Vector3 begin, Vector3 end, float time)
    {
        MainThreadDispatcher.StartCoroutine(
            move(gameObject,end,begin,time,false));
    }
    public static void easeInOut(this GameObject gameObject,Vector3 begin, Vector3 end, float time)
    {
        MainThreadDispatcher.StartCoroutine(
            move(gameObject,begin,end,time,true));
    }
    static IEnumerator move(GameObject gameObject,
        Vector3 begin, Vector3 end, float time,
        bool pingpang)
    {
        gameObject.transform.position = begin;
        float timeAcc = 0;
        Vector3 endValue = end;
        bool isBegin = true;
        while (true)
        {
            yield return new WaitForEndOfFrame();
            timeAcc += Time.deltaTime;
            gameObject.transform.position = Vector3.Lerp(gameObject.transform.position,endValue,1/time * timeAcc);
            if (timeAcc >= time)
            {
                if (pingpang)
                {
                    timeAcc = 0;
                    isBegin = !isBegin;
                    endValue = isBegin ? end : begin;
                    gameObject.transform
                        .position = isBegin
                        ? begin
                        : end;
                }
                else
                {
                    break;
                }
                
            }
           

        }

    }    
}

第二题：

1、网络模块常驻内存：
设置为DontDestroyOnLoad()

2、网络模块初始化的设置缓存大小以以及初始化线程安全的消息队列
SetByteBufferSize()
InitThreadSafeMsgQueue()

3、建立并启动多线程Socket连接
CreateThreadConnect()
StartConnectThread()
4、异步接受Socket消息，并设置消息监听回调
SocketReceiveAsync()
5、设置socket连接状态委托，成功或失败状态
是否被踢，是否token过期
6、、接受Socket消息，并将消息包加入消息队列
GetNetPacket()
EnqueueMsg()

7、网络模块回调设置（包括正常回调，网络断开回调）
将网络回调存储在列表中，在主线程的Update中处理回调和消息队列
Update()
{
  HandlerNetCallbackList()处理回调
  HandleNetMessage()处理消息
}

8、根据消息包类型处理消息
连接成功开始发送握手消息
NetShake()
收到服务器握手消息
再次发送确认消息
初始化心跳机制
NetShakeACKAndInitializeNetHeart()
处理数据体并广播给所有的网络监听实体
HandleDataAndBroadCastNetDataReceive()

9、发送消息给服务器
   SendMessageToServer()

10、

在网络模块销毁时主动断开网络连接：
OnDestroy()
{
    NetDisConnect()
}

第三题：


对于这个问题，我有些疑惑，说的不好，还请面试官海涵

1、如果有这么大量的gameobject 确实可能会造成渲染批次过多
造成帧率下降，影响性能，如果gameObject材质相同，可以选择合批优化
2、如果有大量物体在摄像机的可视范围内，对于极远处的物体在不影响用户体验和满足产品需求的情况下，可以选择剔除不显示
3、有可能是用较少的gameObject通过对象池来模拟10万+的gameObject
根据是否在摄像机可视范围内，动态复用对象池里面的gameObject
可能没有理解面试官的出这道题真正意图，抱歉。

