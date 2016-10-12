

# UnityResourceMgr
项目说明：主要实现基于UNITY资源管理以及更新打包的统一流程机制。有针对资源做Cache并带有一定的资源清理机制。

### 主要接口：ResourcMgr

### 切换场景：
1. ResourceMgr.Instance.CloseScene当前场景
2. ResourceMgr.Instance.LoadScenex新场景
3. OnLevelWasLoaded中，调用
   AssetCacheManager.Instance.ClearUnUsed();
   ResourceMgr.Instance.UnloadUnUsed();

压缩AB, LZ4 AB，非压缩AB，Resources读取都支持。5.3版本支持同步和异步读取所有类型的AB。5.3之前版本，同步函数只支持Resources和非压缩AB, LZ4（5.x才有），异步加载均全支持。
外部使用接口，并不用关心具体文件是在Resources里还是在StreamAssets还是在下载目录，均使用一种方式读取（只有选择同步和非同步的区别）。

--》》重大更新：
   
   1.已经支持LZ4了。另：已经支持同步和异步读取Sprite[]。异步读取LZO压缩，采用5.3版本的LoadFromAsync读取AB。
   
   2.LZMA的读取，在5.3版本采用LoadFromFile和LoadFromFileAsync,在5.3之前版本使用WWW读取（所以5.3可以支持同步和异步，而5.3之前只支持异步）
   
   3.增加了NGUIResLoader, 方便NGUI内部UI框架调用资源加载，简化繁琐的引用计数管理（针对NGUI）
   
   4.增加BaseResLoader, 方便在MonoBehaviour中使用，简化繁琐的引用计数管理
   
   5.依赖文件配置读取，最新采用二进制读取.
   
   6.ResourceMgr的Destroy接口已经支持对原始资源进行Resources.UnloadAsset（第二个参数设置为true）,但注意，使用要非常小心，只有非常确认外面没有使用的情况下（Sprite要保证它对应的纹理都没有被其他地方使用），否则其他地方会丢失资源。并且不能针对GameObject进行Resources.UnloadAsset,因为Resources.UnloadAsset只能删除非可见资源。
   
具体说明请看WIKI: https://github.com/billwillman/UnityResourceMgr/wiki/%E7%9B%AE%E5%BD%95


