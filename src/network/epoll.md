# Linux Epoll

## 核心API及结构
```

// epoll event 结构
typedef union epoll_data {
    void* ptr;        // 用户数据指针
    int fd;           // 文件描述符
    uint32_t u32;     // 32位整数
    uint64_t u64;     // 64位整数
} epoll_data_t;

struct epoll_event {
    uint32_t events;  // epoll 事件
    epoll_data_t data; // 用户数据
};

// 可读事件
#define EPOLLIN      0x001   // 数据可读

// 可写事件  
#define EPOLLOUT     0x004   // 数据可写

// 错误事件
#define EPOLLERR     0x008   // 错误发生
#define EPOLLHUP     0x010   // 挂起事件

// 边缘触发模式
#define EPOLLET      0x80000000  // 边缘触发(Edge Triggered)

// 一次性监听
#define EPOLLONESHOT 0x40000000  // 一次性事件

// 对端关闭连接
#define EPOLLRDHUP   0x2000      // 对端关闭连接(TCP半关闭)

// 唤醒功能（Linux 4.5+）
#define EPOLLWAKEUP  (1 << 29)   // 防止系统休眠

// 独占唤醒（Linux 4.5+）
#define EPOLLEXCLUSIVE (1 << 28) // 避免惊群效应

// 创建epoll实例
int epoll_create(int size);

// 创建epoll实例（扩展版本）
int epoll_create1(int flags);

// 控制epoll事件
// epfd: epoll实例的文件描述符
// op: 操作类型：
//     EPOLL_CTL_ADD: 添加监听的事件
//     EPOLL_CTL_MOD: 修改监听的事件
//     EPOLL_CTL_DEL: 删除监听的事件
// fd: 要操作的目标文件描述符
// event: 事件结构指针

int epoll_ctl(int epfd, int op, int fd, struct epoll_event* event);

// 等待事件发生
// epfd: epoll实例的文件描述符
// events: 事件数组，用于接收就绪的事件
// maxevents: 事件数组的大小
// timeout: 超时时间（毫秒）：
//     -1: 阻塞等待
//     0 : 立即返回
//     >0: 超时时间
int epoll_wait(int epfd, struct epoll_event* events, int maxevents, int timeout);



```