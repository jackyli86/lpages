**HTTP代理**

- 启用代理,考虑到兼容性,同时使用大小写
```bash

#!/bin/bash

export HTTP_PROXY=http://your.proxy:port
export http_proxy=$HTTP_PROXY

export HTTPS_PROXY=http://your.proxy:port
export https_proxy=$HTTPS_PROXY

export NO_PROXY=localhost,127.0.0.1,192.168.49.2
export no_proxy=$NO_PROXY

```

- 关闭代理,执行 source 命令
```bash

#!/bin/bash

unset HTTP_PROXY http_proxy
unset HTTPS_PROXY https_proxy
unset NO_PROXY no_proxy
```