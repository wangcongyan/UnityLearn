 # c#优化相关
    a、for、foreach IEnumerator // foreach 在循环过程中会有一个装箱的过程，造成一次GC
    b、string 和 StringBuilder String在10次以内直接用 “+” 链接不会造成GC 大量 “+” 号时会有GC出现，StringBuilder默认会初始化一个Buffer来存储字符串，缺损值默认为16，默认使用256进行初始化。

