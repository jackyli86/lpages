# VCPKG 基础

# 下载安装
- 下载地址 :  https://github.com/microsoft/vcpkg.git
- 注意 ： 要稳定版本安装

```
# 下载
git clone https://github.com/microsoft/vcpkg.git

# 选择稳定版本
git tags

// 安装vcpkg
bootstrap-vcpkg.bat

// 集成到Visual Studio 
.\vcpkg.exe integrate install

// 设置环境变量
VCPKG_ROOT=C:\work

// TRIPLET默认是x64-windows
// 注意当不是默认值时,需要在VS中指定TRIPLET
VCPKG_DEFAULT_TRIPLET=x64-windows-static

```

# 使用清单模式配置

- 清单文件模板
```vcpkg.json

{
    "dependencies":[
        "fmt",
        {
            "name": "gtest"
            "version>=": "1.17.0"
        }
    ],
    "builtin-baseline": "74e6536215718009aae747d86d84b78376bf9e09"
}

```

- 从清单文件安装
```

```

