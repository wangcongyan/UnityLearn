# SerializeField，Serialzable

## 1. public 变量
    在没有加入任何Atrribute的前提下，public变量默认被视为可以被Serialize，所以在Inspector面板中是可见的
---
##  2、[SerialzeField] Attribute
    强制unity去序列化一个私有域
    这是一个内部的unity序列化功能，有时候我们需要serialize一个private或者protected属性，这个时候可以用[SerialzeField]这个Atrribute进行序列化
## 3、单独的class或struct 
    Serializable是.Net自带的序列化
    有时候我们会自定义一些单独的Class/struct,由于这些类没有从MonoBehavior派生所以默认并不被Unity3D识别为可以Serialize的结构。自然也就不会再Inspector中显示。我们可以通过添加[Serializable这个Attribute使Unity3D检测并注册这些类可Serialize的类型
    Example:
        [Serializable]
        public Class Boo
        {
            public int foo = 5;
            public int bar = 5;
        }
        注意：Serializable只可以对Class,Struct,enum,delegate进行序列化不可以对属性进行序列化
## 4、ScriptableObject
    ScriptableObject 类型经常用于存储一些Unity3D本身不可以打包的object，比如字符串，一些类对象等。用这个类型的子类型，则可以用BuildPipline打包成AssetBundle包供后续使用，具体参考[ScriptableObject 序列化](http://docs.unity3d.com/Manual/class-ScriptableObject.html)
### 4.1 projected、private、internal变量
    默认情况下protected、private、internal变量不会被serialize;
### 4.2 [System.NonSerialized]Attribute
    有时候我们需要定义一些public变量方便操作，但又不希望这些变量保留。这个时候可以利用[System.NonSerialized]来完成这个操作。
### 4.3 readonly、const、static 修饰符
    如果变量加入了readonly，const、static等修饰符，无论他的serialize设置如何都不会进行serialize。
### 4.4. Dictionary<T,K>
    Unity3D可以对List<T>进行序列化显示，如果需要 序列化
    serialize Dictionary<T,K>，则需要通过Serialize一份List<T>,然后在Awake中初始化Dictionary的方法来完成Dictionary的Serialize操作，Example:
        [System.Serializable]
    public class NameToID {
        public string name = "";
        public int ID = -1;
    }
    public List<NameToID> nameToIDList = new List<NameToID>();  
    Dictionary<string,int> nameToID = new Dictionary<string,int>();  
    //
        void Awake () 
        {
            foreach ( NameToID info in nameToIDList ) {
                nameToID[info.name] = info.ID;
            }
            nameToIDList = null; // put it null make garbage collect it (I wish)
    }

    