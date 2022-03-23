静态包、动态包有什么区别？何时使用增量更新？Addressables 更新流程大梳理

Unity技术博客

关注
阅读 1431
2021年6月9日
在平时与开发小伙伴的交流过程中，我们发现会遇到不少关于 Addressables 更新流程的新问题，比如什么时候用静态包？什么时候用动态包？客户端该如何判断是否有资源更新等等。因此，以本文再帮大家梳理下更新流程。
大家也可以查看 Addressables 的手册，以及之前的视频教程。
Addressables 手册：
https://docs.unity3d.com/Packages/com.unity.addressables@1.18/manual/ContentUpdateWorkflow.html
视频教程：
https://space.bilibili.com/386224375/channel/detail?cid=119328
基本更新流程
01  更新包发布流程
我们首先来看一下常见的资源更新流程，如下图所示：

一般来说，为了减小 App 的大小，除了保留一些必要的数据，我们会将大部分的数据剥离出 App，并上传到 CDN 上，这部分数据，我们称之为首包。当然后续还可以对首包的数据进行更新，我们称之为局部更新包。
02  静态包与动态包
作为首包里的 Asset Bundle，我们期望这个包是一直存在于缓存中的，因此也被称作静态包，对应 Group 属性里的 Update Restriction 设置为 Cannot Change Post Release。静态包中如果有文件被修改了，需要发布新的更新包，通过 Tools 菜单中的 Check For Content Update 命令可以检测出变化的文件，并放到新的 Group 中，该 Group 属性里的 Update Restriction 设置为 Can Change Post Release，也就是我们常说的动态包。
03  何时使用增量更新？
上面我们看到动态包实际上是基于静态包的一个增量包，那再一次进行更新的时候是否继续使用增量更新的方式呢？我们先假设当前的初始静态包中有两个资源：

初始静态包更新后，如果 Asset A 有修改，我们可以利用 CheckFor Content Update 命令，将 Asset A 识别出来，并放入新的动态包中，当然我们也可以加入一些新资源到该动态包中。

到目前为止，我们可以说 Group 1 是一个增量更新包。但是当我们再次修改 Asset A，由于 Asset A 此时已位于 Content Update Group 1，而该 Group 设置为 Can Change Post Release。也就是说使用 Check For Content Update 命令不会再检测出 Asset A，而且客户端之前下载的 Group 1 会被整体下载和替换。
这时，有小伙伴可能会想到，Group1 里只有 Asset A 变化了，但是整包都进行了下载更新，下载了多余的资源。那是否可以将 Group1 也设置为 Cannot Change Post Release, 检测出变化的 AssetA，并将其放入一个新的 Content Update Group 2，从而只下载增量资源？

理论上每次都进行增量更新这是可以的，但是会明显带来两个问题：
Group 1 中的 Asset A 会变成冗余资源一直保存在缓存中
增量包逐层依赖，依赖链条变长后会难于维护且容易出错
所以，相对来说比较简单的方案就是除首包外的动态包都设置为 Can Change Post Release。Asset A 修改后，仍然位于 Content Update Group 1 中。但是由于该包内容已发生变化，客户端会将最新版本的包同步到本地。

至于缓存中的旧版本如何处理，可以在 Group 的设置中选择相应清除策略：

通过以上的分析，我们可以看到，首包之后的动态包更新采用整包更新的方式更加简单直接。
客户端更新逻辑
01  基本流程
接下来，我们再继续说一下大家经常问到的客户端更新逻辑。还是先来看一下常规的一个更新流程：

通常来说客户端的更新包含上面三步：
Catalog 更新，也就是资源清单的更新
资源下载到客户端缓存
资源加载到内容
虽然每一步都提供了单独的一套 API 调用，但如果直接调用后面的步骤，系统也会自动帮你完成之前的步骤。比如当你直接调用 LoadAssetsAsync，系统会自动进行 Catalog 的更新，并且下载对应的资源到本地缓存中。
如果你希望系统不要自动调用，比如希望在开机启动时先加载本地缓存中的资源，可以在 Catalog 的设置中勾选 Disable Catalog Update on Startup。

02  如何判断是否有新的资源
通常，我们使用 GetDownloadSizeAsync(key) 来获取新资源包的大小，如果包大小大于 0，就说明有新的资源，并调用 DownloadDependenciesAsync(key) 来进行下载。具体使用方法，可以参考手册。
一般大家都会用到 Label 来作为参数 key，这样可以进行批量资源的下载。但经常被问到的问题就是，Label 如果写死在 App 中，如果 App 发布了，是否就没有机会传入新增的 Label 了？对于这个问题，从实际使用情况来看，在规划资源更新的时候，一般都在项目中后期，资源格局基本已经定好，对应的 Label 也设定好了，所以后期的新增或者修改资源也在现有的 Label 下进行增改。当然，如果实在动态获取新增的 Labels，可以采用如下方法。
在介绍方法前，需要提前说明的是，Labels，资源地址都统一作为资源的 keys 进行保存，所以 Addressables 可以返回所有 keys，但不能单独获取到所有 Labels。

这种方式虽然可以动态获取 keys，但由于要查询所有 keys，不如指定 keys 的效率高，所以一般情况下，直接使用指定 keys 的方式即可。
希望以上介绍能够帮助大家对 Addressables 系统更新策略的理解和使用。也欢迎参阅 Addressable Asset System 说明文档来了解更多最佳实践、入门系统使用、学习新扩增的 API。
Addressable Asset System 说明文档：
https://docs.unity3d.com/Packages/com.unity.addressables@1.17/manual/index.html