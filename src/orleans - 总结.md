# Orleans 总结

### Cluster Management (集群管理)
- 实例代码
```csharp
using IHost host = Host.CreateDefaultBuilder(args)
    .UseOrleans(builder =>
    {
        builder.Configure<ClusterOptions>(options =>
        {
            options.ClusterId = "my-first-cluster";
            options.ServiceId = "SampleApp";
        });
        builder.UseAdoNetClustering(options => {
            options.Invariant = "Mysql.Data.MysqlClient";
            options.ConnectString = "connectionString";
        });
    })
    .UseConsoleLifetime()
    .Build();
```
- 实现详解
1. 通过指定 ClusterId 和 ServiceId
2. silo 发现 通过 **Use*Cluster** 等扩展方法定义的 将必要信息 IP/Port/ClusterId 写入 membership 实现
3. silo 启动后，会从membership表中拉取集群相关silo节点IP/Port 从而建立连接 **做 服务发现**

- 关于Membership
[MySQL-Clustering](https://github.com/dotnet/orleans/blob/main/src/AdoNet/Orleans.Clustering.AdoNet/MySQL-Clustering.sql)

### Grain Filter (筛选器)
- 分类:
1. 全局-筛选器
    通过 `AddIncomingGrainCallFilter` 或 `AddOutgoingGrainCallFilter` 方法实现
2. Grain-筛选器
    通过 Grain类 通过继承 `IIncomingGrainCallFilter` 或 `IOutgoingGrainCallFilter` 接口实现

- 用途:
1. 添加 **全局-筛选器** 打印Grain调用链,用于追踪死锁
2. 做一些过滤操作

### Grain Stream(流)
