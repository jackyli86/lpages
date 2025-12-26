# Orleans 总结

### Grain Filter (筛选器)
- 分类:
1. 全局-筛选器
    通过 `AddIncomingGrainCallFilter` 或 `AddOutgoingGrainCallFilter` 方法实现
2. Grain-筛选器
    通过 Grain类 通过继承 `IIncomingGrainCallFilter` 或 `IOutgoingGrainCallFilter` 接口实现

- 用途:
1. 添加 **全局-筛选器** 打印Grain调用链,用于追踪死锁
2. 做一些过滤操作