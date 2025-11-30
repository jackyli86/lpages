# Windows IOCP

## 核心函数及结构
```
// IOCP 核心句柄
HANDLE completion_port;


// OVERLAPPED 结构（基础）
typedef struct _OVERLAPPED {
    ULONG_PTR Internal;
    ULONG_PTR InternalHigh;
    union {
        struct {
            DWORD Offset;
            DWORD OffsetHigh;
        };
        PVOID Pointer;
    };
    HANDLE hEvent;
} OVERLAPPED, *LPOVERLAPPED;


// WSABUF 结构（用于分散/聚集I/O）
typedef struct _WSABUF {
    ULONG len;               // 缓冲区长度
    CHAR* buf;               // 缓冲区指针
} WSABUF, *LPWSABUF;


// 创建&关联完成端口
HANDLE CreateIoCompletionPort(
    HANDLE FileHandle,                 // 文件或socket句柄
    HANDLE ExistingCompletionPort,     // 现有完成端口
    ULONG_PTR CompletionKey,           // 完成键（用户数据）
    DWORD NumberOfConcurrentThreads    // 并发线程数
);


BOOL GetQueuedCompletionStatus(
    HANDLE CompletionPort,             // 完成端口句柄
    LPDWORD lpNumberOfBytesTransferred,// 传输字节数
    PULONG_PTR lpCompletionKey,        // 完成键
    LPOVERLAPPED* lpOverlapped,        // OVERLAPPED结构指针
    DWORD dwMilliseconds               // 超时时间
);

BOOL PostQueuedCompletionStatus(
    HANDLE CompletionPort,             // 完成端口句柄
    DWORD dwNumberOfBytesTransferred,  // 传输字节数
    ULONG_PTR dwCompletionKey,         // 完成键
    LPOVERLAPPED lpOverlapped          // OVERLAPPED结构
);

SOCKET WSASocket(
    int af,                            // 地址族
    int type,                          // socket类型
    int protocol,                      // 协议
    LPWSAPROTOCOL_INFO lpProtocolInfo, // 协议信息
    GROUP g,                           // 套接字组
    DWORD dwFlags                      // 标志位（必须包含WSA_FLAG_OVERLAPPED）
);

typedef BOOL (PASCAL FAR * LPFN_ACCEPTEX)(
    SOCKET sListenSocket,              // 监听socket
    SOCKET sAcceptSocket,              // 接受socket（必须预先创建）
    PVOID lpOutputBuffer,              // 输出缓冲区
    DWORD dwReceiveDataLength,         // 接收数据长度
    DWORD dwLocalAddressLength,        // 本地地址长度
    DWORD dwRemoteAddressLength,       // 远程地址长度
    LPDWORD lpdwBytesReceived,         // 接收的字节数
    LPOVERLAPPED lpOverlapped          // 重叠结构
);

typedef void (PASCAL FAR * LPFN_GETACCEPTEXSOCKADDRS)(
    PVOID lpOutputBuffer,              // AcceptEx的输出缓冲区
    DWORD dwReceiveDataLength,         // 接收数据长度
    DWORD dwLocalAddressLength,        // 本地地址长度
    DWORD dwRemoteAddressLength,       // 远程地址长度
    LPSOCKADDR *LocalSockaddr,         // 输出：本地地址
    LPINT LocalSockaddrLength,         // 输出：本地地址长度
    LPSOCKADDR *RemoteSockaddr,        // 输出：远程地址
    LPINT RemoteSockaddrLength         // 输出：远程地址长度
);

int WSARecv(
    SOCKET s,                          // socket句柄
    LPWSABUF lpBuffers,                // 缓冲区数组
    DWORD dwBufferCount,               // 缓冲区数量
    LPDWORD lpNumberOfBytesRecvd,      // 接收的字节数
    LPDWORD lpFlags,                   // 标志位
    LPWSAOVERLAPPED lpOverlapped,      // 重叠结构
    LPWSAOVERLAPPED_COMPLETION_ROUTINE lpCompletionRoutine // 完成例程
);


int WSASend(
    SOCKET s,                          // socket句柄
    LPWSABUF lpBuffers,                // 缓冲区数组
    DWORD dwBufferCount,               // 缓冲区数量
    LPDWORD lpNumberOfBytesSent,       // 发送的字节数
    DWORD dwFlags,                     // 标志位
    LPWSAOVERLAPPED lpOverlapped,      // 重叠结构
    LPWSAOVERLAPPED_COMPLETION_ROUTINE lpCompletionRoutine // 完成例程
);


// 从结构体成员指针获取整个结构体指针
#define CONTAINING_RECORD(addr, type, field) \
    ((type *)((char*)(addr) - offsetof(type, field)))

//
// Calculate the address of the base of the structure given its type, and an
// address of a field within the structure.
//

#define CONTAINING_RECORD(address, type, field) ((type *)( \
                                                  (PCHAR)(address) - \
                                                  (ULONG_PTR)(&((type *)0)->field)))
```


