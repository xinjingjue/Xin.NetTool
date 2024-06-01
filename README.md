## Xin.NetTool

### SnowFlake

为了解决Guid 128位过长占据数据库的存储空间，并且无法再分布式系统中使用的原因。

使用c#重写了Twitter的雪花算法,线程安全，并且适合于分布式系统的使用（正确设置工区Id和作业Id）

```
SnowflakeIdGenerator generator = new SnowflakeIdGenerator(workerId,datacenterId,timeCallBackHandler);
string id = generator.nextId();
```

### JobManager

  仅适用于windows平台
  作业对象，主要用于子进程管理。
  目前版本是只支持作业销毁拥有的子进程退出,仅支持了一个Job对象，需要使用可以修改Job句柄为可变参数
  通常可以定义一个全局静态变量使用

```
Job job = new Job();
job.AddProcess(xxxx);
支持使用Using代码块包裹


//环境变量清理功能：实现对主机变量名为Path的内容清理
    /// 1.清除所有无效路径
    /// 2.清除路径文件夹中包含包含某文件（在代码中修改，没有提供修改方法 ）"HHTech.CSM2018.Starter.exe"或"HHTech.CSM2018.Client.exe"
    /// 3.重写path的系统环境变量
EnviornmentCleanr cleaner = new EnviornmentCleanr();
cleaner.ResetkeyPathInEnvironment();
```

### EventBus

事件总线是对发布-订阅模式的一种实现。它是一种集中式事件处理机制，允许不同的组件之间进行彼此通信而又不需要相互依赖，达到一种解耦的目的。

不依赖于高耦合的delegate和Event,实现了一个线程安全类型的事件总线形式。

```
//组件已经实现了自动注册功能
//发布，及触发
EventBus.Default.publish();
//订阅
EventBus.Default.Subscribe();
//取消订阅
EventBus.Default.UnSubscribe();
```

###EasyLog
一个跨平台的轻量化的简单Log日志，需要创建LogConfig.json。

```
//json格式
{
  "LogFileName": "MYlOG",
  "SuccessLogName": "SuccessLog",
  "ErrorLogName": "ErrorLog",
  "ExpectionLogName": "ExpectionLog",
  "WarningLogName": "WarningLog",
  "SuccessSaveDays": 10,
  "ErrorSaveDays": 20,
  "ExpectionSaveDays": 20,
  "WarningSaveDays": 20

}
/*
* 解释说明：LogFileName：根文件下的Log文件夹
* "Success|Error|Expection|Warning LogName":成功，错误，异常，警告，四种日志文件的名字（不需* * 要后缀名）
* SaveDays：日志会根据设置进行刷新，以日为单位进行刷新
*/

使用
 static void Main(string[] args)
 {
 	// 可以使用绝对路径，也可以使用相对路径，使用相对路径，将LogConfig.json属性设置如果较新则复制
     LogSaver saver = new LogSaver("./LogConfig.json");
     saver.LogWarning("This is a warning log");
 }
```



<<<<<<< HEAD
###IniParser
>>>>>>> refs/remotes/origin/main

Ini文件解析器，提供Ini文件解析，添加，修改等功能。

```
IniFile iniFile = new IniFile(path);
//读取指定得Value
iniFile.ReadValue(section,key);
//读取KeyValuePairs
iniFile.ReadKeyValuePairsInSection(section);
//修改节点名字
iniFile.EditSection(oldSection, newSection);
//修改节点下key名字
iniFile.EditKey(Section,oldKey,newKey);
//修改节点下对应Key得Value名字
iniFile.EditValue(section,key,value);
//添加keyValue
iniFile.AddKeyValueInSection("xxxx", "xxxx", "xxxx");
//添加节点
iniFile.AddSection("pilipala");
//保存 会覆盖源文件，导致注释消失
iniFile.Save();

//支持Idispose释放资源
using(IniFile inif = new IniFile(Path))
{
    //ToDo
}
```

### SysInfo

**仅Windows平台可用**：通过WMI获取设备硬件信息，轻量化无另外插件集成。
