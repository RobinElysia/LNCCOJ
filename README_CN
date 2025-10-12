# Go-judge → LNCCOJ

## 描述

​	本镜像由[Go-Judge](https://github.com/criyle/go-judge)衍生而来，加入了常用的 6 种编程语言（如下），目的是为了方便开发者拿来直接使用，不需要配置过多的语言环境。

```bash
Nodejs：v20.19.2
Python：3.13.5
Java：17.0.12
Go：1.24.8
gcc：14.2.0
g++：14.2.0
```

## 安装教程

​	首先，你要有Docker基础，这里宿主机可以是win11或者Linux（win11下WSL+Docker测试正常）；其次，你需要有魔法，这样才能更加高效的下载；最后，需要你有postman或者其他测试工具

### 镜像装载

下载完该镜像文件后，直接使用加载镜像命令进行加载：

```bash
# 加载镜像
docker load -i 文件名.tar
```

你也可以事后指定镜像名称：

```bash
# 修改信息
docker tag <原镜像名>:<原标签> <新镜像名>:<新标签>
```

### 运行容器

```bash
docker run -d --privileged --shm-size=512m -p 5050:5050 lnccoj
# 注释
docker run <参数> <镜像名称>
-d 后台运行，不占用当前端口
--privileged 赋予容器几乎与宿主机 root 用户相同的权限
--shm-size=512m 共享内存大小配置参数
-p 端口映射，宿主机:容器

# 或者使用
docker run -d --name lnccoj --privileged --cpus 2 --cpuset-cpus 0-1 -m 4G -p 5050:5050 -p 5052:5052 lnccoj:v1.1 -http-addr 0.0.0.0:5050 -enable-metrics -monitor-addr 0.0.0.0:5052

# 注释
--cpus 2：限制容器最多使用2个CPU核心。
--cpuset-cpus 0-1：限制容器只能运行在指定的CPU核心上，这里是指定使用0和1号CPU核心。
-m 4G：限制容器的内存使用为4GB。
-p 5050:5050：将主机的5050端口映射到容器的5050端口。
-p 5052:5052：将主机的5052端口映射到容器的5052端口。
-http-addr 0.0.0.0:5050：这是传递给容器内应用程序的参数，表示应用程序的HTTP服务地址为0.0.0.0:5050。
-enable-metrics：启用指标收集（通常是应用程序的功能，用于收集和暴露监控指标）。
-monitor-addr 0.0.0.0:5052：指定监控指标的暴露地址为0.0.0.0:5052。
```

### 查看容器状态

```bash
docker ps -al
# 如果看到如下就代表成功了
CONTAINER ID   IMAGE             COMMAND           CREATED        STATUS        PORTS                    NAMES
7b9c6fecbbd0   lnccoj   "/opt/go-judge"   24 hours ago   Up 24 hours   0.0.0.0:5050->5050/tcp   lnccoj
# 端口映射成功就算成功了
```

接下来就可以测试了🤓👍

## 请求样例（记得删除注释测试）

### js

编译请求：

```json
{
    "cmd": [{
        "args": ["/usr/bin/nodejs", "a.js"], //编译命令
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": ""
        }, {
            "name": "stdout", // 标准输入
            "max": 10240 // 标准输入最大
        }, {
            "name": "stderr", // 标准输出
            "max": 10240 // 标准输出最大
        }],
        "cpuLimit": 10000000000, // cpu限制
        "memoryLimit": 104857600, // 内存限制
        "procLimit": 50,
        "copyIn": {
            "a.js": { // 注意修改
                "content": "#!/usr/bin/env node\nconsole.log(\"🎉 Hello, World!\");" // 代码部分，记得转译
            }
        },
        "copyOut": ["stdout", "stderr"],
        "copyOutCached": ["a.js"]
    }]
}
```

运行请求：

```json
{
    "cmd": [{
        "args": ["a.js"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": "" // 运行传参
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000, // cpu限制
        "memoryLimit": 104857600, // 内存限制
        "procLimit": 50,
        "copyIn": {
            "a.js": {
                "fileId": "Z66NSY2T" // 编译的响应会给出ID，记得换
            }
        }
    }]
}
```

### py

编译请求（虽然没有编译部分，需要走编译部分，不然你拿不到ID）：

```json
{
    "cmd": [{
        "args": ["/usr/bin/python3", "a.py"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": ""
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000,
        "memoryLimit": 104857600,
        "procLimit": 50,
        "copyIn": {
            "a.py": {
                "content": "#!/usr/bin/env python3\nprint('a')\n"
            }
        },
        "copyOut": ["stdout", "stderr"],
        "copyOutCached": ["a.py"]
    }]
}
```

编译请求：

```json
{
    "cmd": [{
        "args": ["a.py"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": ""
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000,
        "memoryLimit": 104857600,
        "procLimit": 50,
        "copyIn": {
            "a.py": {
                "fileId": "U54EWMTY"
            }
        }
    }]
}
```

### java

编译请求：

```json
{
    "cmd": [{
        "args": ["/usr/bin/jdk-17.0.12/bin/javac", "Main.java"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": ""
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000,
        "memoryLimit": 104857600,
        "procLimit": 50,
        "copyIn": {
            "Main.java": { // 注意这里是java文件
                "content": "public class Main{ \n public static void main(String[] args) { \n System.out.println(\"Hello world\");\n }\n}"
            }
        },
        "copyOut": ["stdout", "stderr"],
        "copyOutCached": ["Main.class"] // 这里是字节码文件
    }]
}
```

运行请求：

```json
{
    "cmd": [{
        "args": ["/usr/bin/jdk-17.0.12/bin/java","Main"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": ""
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000,
        "memoryLimit": 104857600,
        "procLimit": 50,
        "copyIn": {
            "Main.class": { // 这里是字节码文件
                "fileId": "2O5QX3EB"
            }
        }
    }]
}
```

### go

编译请求：

```json
{
    "cmd": [{
        "args": ["/usr/bin/go/bin/go", "run","main.go"],
        "env": [
            "PATH=/usr/bin:/bin:/usr/bin/go/bin",
            "HOME=/tmp", 
            "GOCACHE=/tmp/go-cache",
            "GOROOT=/usr/bin/go",
            "GO111MODULE=off",
            "GOOS=linux",
            "GOARCH=amd64"
        ], // 这里和一般的语言不太一样，别问，问就是环境配置多
        "files": [{
            "content": ""
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000,
        "memoryLimit": 2516582400, // 注意内存大于100MB
        "procLimit": 50,
        "copyIn": {
            "main.go": {
                "content": "package main\nfunc main() {\nprintln(\"Hello, World!\")\n}\n"
            }
        },
        "copyOut": ["stdout", "stderr"],
        "copyOutCached": ["main.go"]
    }]
}
```

运行请求：

```json
{
    "cmd": [{
        "args": ["/usr/bin/go/bin/go", "run", "main.go"],
        "env": [
            "PATH=/usr/bin:/bin:/usr/bin/go/bin",
            "HOME=/tmp", 
            "GOCACHE=/tmp/go-cache",
            "GOROOT=/usr/bin/go",
            "GO111MODULE=off",
            "GOOS=linux",
            "GOARCH=amd64"
        ],
        "files": [{
            "content": ""
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000,
        "memoryLimit": 2516582400, // 注意内存大于100MB
        "procLimit": 50,
        "copyIn": {
            "main.go": {
                "fileId": "XFRTPULW"
            }
        }
    }]
}
```

### C++

编译请求：

```json
{
    "cmd": [{
        "args": ["/usr/bin/g++", "a.cc", "-o", "a"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": ""
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000,
        "memoryLimit": 104857600,
        "procLimit": 50,
        "copyIn": {
            "a.cc": {
                "content": "#include <iostream>\nusing namespace std;\nint main() {\nint a, b;\ncin >> a >> b;\ncout << a + b << endl;\n}"
            }
        },
        "copyOut": ["stdout", "stderr"],
        "copyOutCached": ["a"]
    }]
}
```

运行请求：

```json
{
    "cmd": [{
        "args": ["a"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": "1 1"
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000,
        "memoryLimit": 104857600,
        "procLimit": 50,
        "copyIn": {
            "a": {
                "fileId": "5LWIZAA45JHX4Y4Z" // 这个缓存文件的 ID 来自上一个请求返回的 fileIds
            }
        }
    }]
}
```

### C

编译请求：

```json
{
    "cmd": [{
        "args": ["/usr/bin/gcc", "a.c", "-o", "a"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": ""
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000,
        "memoryLimit": 104857600,
        "procLimit": 50,
        "copyIn": {
            "a.c": {
                "content": "#include <stdio.h>\nint main()\n{\nprintf(\"Hello, World!\");\nreturn 0;\n}\n"
            }
        },
        "copyOut": ["stdout", "stderr"],
        "copyOutCached": ["a"]
    }]
}
```

运行请求：

```json
{
    "cmd": [{
        "args": ["a"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": ""
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000,
        "memoryLimit": 104857600,
        "procLimit": 50,
        "copyIn": {
            "a": {
                "fileId": "ZNDTKSMX"
            }
        }
    }]
}
```

其他`API`参考：[Go-Judge.API](https://docs.goj.ac/cn/api)

## 环境

### C/C++

​	在`/usr/bin`下，你可以看到`gcc`和`g++`，你可以动手卸载这个版本、安装其他版本。

### Go

​	在`/usr/bin/go`下，这里为了统一环境安装位置，全部管理在`/usr/bin/go`下。除了这些，go语言的缓存资源在`/tmp/`下，有`go`、`go-cache`和`go-tmp`，这里不建议修改，因为是精心设计的（

### Nodejs

​	在`/usr/bin`下。你可以安装其他版本的Nodejs，或者使用`apt`安装`nvm`进行版本管理，这里不做详细的解释了，详情见`nvm`的官网文档。

### Java

​	在`/usr/bin/jdk-17.0.12`下。同样，你可以安装`JEnv`进行`Java`版本管理，这里不再进行详解，进请参考[`JEnv`的项目地址](https://github.com/jenv/jenv)。

### Python

​	在`/usr/bin`下，注意`py`在 Linux 下为`python3`，不是`Python`，算是一个坑，并且`Py`原生不支持命令行传参，需要自行在代码中添加相关依赖。如：“sys”，具体代码如下：

```py
import sys

# sys.argv[0] 是脚本名称
# sys.argv[1] 是第一个参数，依此类推

if len(sys.argv) > 1:
    print(f"脚本名称: {sys.argv[0]}")
    print(f"第一个参数: {sys.argv[1]}")
    print(f"所有参数: {sys.argv[1:]}")
else:
    print("请提供命令行参数")
```

其他系统级别设置参考：[Go-Judge.configuration](https://docs.goj.ac/cn/configuration)

