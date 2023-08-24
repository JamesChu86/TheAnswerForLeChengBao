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
