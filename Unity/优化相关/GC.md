A. Device.Present: 
      1.GPU的presentdevice确实非常耗时，一般出现在使用了非常复杂的shader. 
      2.GPU运行的非常快，而由于Vsync的原因，使得它需要等待较长的时间. 
      3.同样是Vsync的原因，但其他线程非常耗时，所以导致该等待时间很长，比如：过量AssetBundle加载时容易出现该问题. 
      4.Shader.CreateGPUProgram:Shader在runtime阶段（非预加载）会出现卡顿(华为K3V2芯片). 
   B. StackTraceUtility.PostprocessStacktrace()和StackTraceUtility.ExtractStackTrace(): 
      1.一般是由Debug.Log或类似API造成. 
      2.游戏发布后需将Debug API进行屏蔽.

   C. Overhead: 
      1.一般情况为Vsync所致. 
      2.通常出现在Android设备上. 
   D. GC.Collect: 
      原因: 1.代码分配内存过量(恶性的) 2.一定时间间隔由系统调用(良性的). 
      占用时间：1.与现有Garbage size相关 2.与剩余内存使用颗粒相关（比如场景物件过多，利用率低的情况下，GC释放后需要做内存重排) 
   E. GarbageCollectAssetsProfile: 
      1.引擎在执行UnloadUnusedAssets操作(该操作是比较耗时的,建议在切场景的时候进行). 
      2.尽可能地避免使用Unity内建GUI，避免GUI.Repaint过渡GC Allow. 
      3.if(other.tag == GearParent.MogoPlayerTag)改为other.CompareTag(GearParent.MogoPlayerTag).因为other.tag为产生180B的GC Allow. 
   F. 少用foreach，因为每次foreach为产生一个enumerator(约16B的内存分配)，尽量改为for. 
   G. Lambda表达式，使用不当会产生内存泄漏. 
   H. 尽量少用LINQ： 
      1.部分功能无法在某些平台使用. 
      2.会分配大量GC Allow. 
   I. 控制StartCoroutine的次数： 
      1.开启一个Coroutine(协程)，至少分配37B的内存. 
      2.Coroutine类的实例 — 21B. 
      3.Enumerator — 16B. 
   J. 使用StringBuilder替代字符串直接连接. 
   K. 缓存组件: 
      1.每次GetComponent均会分配一定的GC Allow. 
      2.每次Object.name都会分配39B的堆内存.