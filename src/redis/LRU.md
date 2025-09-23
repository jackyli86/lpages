Redis 的 **LRU（Least Recently Used）** 策略是一种**内存淘汰策略**：
当实例内存达到 `maxmemory` 限制后，会**优先删除最近最少使用的键**，以便腾出空间给新数据。

---

## 1️⃣ 开启 LRU 的前提

在 `redis.conf` 中设置：

```conf
maxmemory 512mb               # 限制最大内存
maxmemory-policy allkeys-lru   # 或 volatile-lru
```

* `allkeys-lru`：所有 key 都可能被淘汰（最常见）。
* `volatile-lru`：只淘汰**设置了过期时间的 key**。

---

## 2️⃣ 工作原理（近似 LRU）

Redis 为每个 key 维护一个 **访问时间戳（LRU clock）**。
达到内存上限时：

1. 随机采样 N 个 key（默认 5，可调 `maxmemory-samples`）。
2. 从样本中挑选**最久未被访问**的 key 删除。
3. 循环直到内存低于限制。

> 这是 **近似 LRU**，用采样提高效率，避免全库扫描。

---

## 3️⃣ 举例说明

假设：

* `maxmemory = 100MB`
* `maxmemory-policy = allkeys-lru`
* 已存 3 个 key：`k1,k2,k3`，每个 30MB。
* 访问顺序：先 `GET k1`，过 10 秒 `GET k2`，`k3` 很久没访问。
* 现在要 `SET k4` (40MB)，超出 100MB。

**过程：**

1. Redis 采样若干 key（可能包括 k1,k2,k3）。
2. 根据访问时间，`k3` 最久未用，被删除。
3. 写入 `k4` 成功，最终 key 为 `k1,k2,k4`。

---

## 4️⃣ 变体策略一览

| 策略                             | 说明                             |
| ------------------------------ | ------------------------------ |
| `noeviction`                   | 默认，不删除，超内存直接返回错误               |
| `allkeys-lru`                  | 所有键参与 LRU                      |
| `volatile-lru`                 | 仅带 TTL 的键 LRU                  |
| `allkeys-random`               | 所有键随机淘汰                        |
| `volatile-random`              | 仅带 TTL 的键随机淘汰                  |
| `volatile-ttl`                 | TTL 最短的键先淘汰                    |
| `allkeys-lfu` / `volatile-lfu` | Redis 4.0+ 的 **LFU**（最少使用次数）策略 |

---

## 5️⃣ 实战小贴士

* **调节采样**：`maxmemory-samples 10` 提高准确度但增 CPU。
* **组合策略**：常见配置：

  ```conf
  maxmemory 1gb
  maxmemory-policy allkeys-lru
  maxmemory-samples 10
  ```
* **监控命中率**：`INFO stats` 关注 `keyspace_hits`/`keyspace_misses` 评估缓存效果。

---

### ✅ 总结

* **LRU 策略 = “删最久没用”**。
* Redis 实现是 **近似 LRU**，靠采样平衡性能和准确度。
* 实际使用需配置 `maxmemory` + `maxmemory-policy`，并根据业务调整采样率。
