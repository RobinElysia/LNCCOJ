# Go-judge → LNCCOJ

Other Language: [CN](https://github.com/RobinElysia/LNCCOJ/blob/main/README_CN.md)

## Description

​	This image is derived from [Go-Judge](https://github.com/criyle/go-judge) and includes 6 commonly used programming languages (listed below) to facilitate developers to use it directly without excessive language environment configuration.

```bash
Nodejs: v20.19.2
Python: 3.13.5
Java: 17.0.12
Go: 1.24.8
gcc: 14.2.0
g++: 14.2.0
```

## Installation Tutorial

​	First, you need to have Docker basics. The host machine here can be win11 or Linux (win11 with WSL+Docker tested normally); second, you need to have a way to access international networks for more efficient downloading; finally, you need postman or other testing tools.

### Image Loading

​	After downloading the image file, directly use the load image command to load it:

```bash
# Load the image
docker load -i filename.tar
```

​	You can also specify the image name afterwards:

```bash
# Modify information
docker tag <original-image-name>:<original-tag> <new-image-name>:<new-tag>
```

### Run Container

```bash
docker run -d --privileged --shm-size=512m -p 5050:5050 lnccoj
# Comments
docker run <parameters> <image-name>
-d Run in the background without occupying the current port
--privileged Grant the container almost the same permissions as the host root user
--shm-size=512m Shared memory size configuration parameter
-p Port mapping, host:container

# Or use
docker run -d --name lnccoj --privileged --cpus 2 --cpuset-cpus 0-1 -m 4G -p 5050:5050 -p 5052:5052 lnccoj:v1.1 -http-addr 0.0.0.0:5050 -enable-metrics -monitor-addr 0.0.0.0:5052

# Comments
--cpus 2: Limit the container to use at most 2 CPU cores.
--cpuset-cpus 0-1: Restrict the container to run on specified CPU cores, here 0 and 1.
-m 4G: Limit the container's memory usage to 4GB.
-p 5050:5050: Map the host's 5050 port to the container's 5050 port.
-p 5052:5052: Map the host's 5052 port to the container's 5052 port.
-http-addr 0.0.0.0:5050: This is a parameter passed to the application inside the container, indicating that the application's HTTP service address is 0.0.0.0:5050.
-enable-metrics: Enable metrics collection (usually a feature of the application for collecting and exposing monitoring metrics).
-monitor-addr 0.0.0.0:5052: Specify the address for exposing monitoring metrics as 0.0.0.0:5052.
```

### Check Container Status

```bash
docker ps -al
# If you see the following, it means success
CONTAINER ID   IMAGE             COMMAND           CREATED        STATUS        PORTS                    NAMES
7b9c6fecbbd0   lnccoj   "/opt/go-judge"   24 hours ago   Up 24 hours   0.0.0.0:5050->5050/tcp   lnccoj
# The port mapping is successful
```

Now you can test it 🤓👍

## Request Examples (Remember to delete comments when testing)

### js

Compilation request:

```json
{
    "cmd": [{
        "args": ["/usr/bin/nodejs", "a.js"], // Compilation command
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": ""
        }, {
            "name": "stdout", // Standard input
            "max": 10240 // Maximum size of standard input
        }, {
            "name": "stderr", // Standard output
            "max": 10240 // Maximum size of standard output
        }],
        "cpuLimit": 10000000000, // CPU limit
        "memoryLimit": 104857600, // Memory limit
        "procLimit": 50,
        "copyIn": {
            "a.js": { // Note to modify
                "content": "#!/usr/bin/env node\nconsole.log(\"🎉 Hello, World!\");" // Code part, remember to escape
            }
        },
        "copyOut": ["stdout", "stderr"],
        "copyOutCached": ["a.js"]
    }]
}
```

Running request:

```json
{
    "cmd": [{
        "args": ["a.js"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": "" // Parameters passed during running
        }, {
            "name": "stdout",
            "max": 10240
        }, {
            "name": "stderr",
            "max": 10240
        }],
        "cpuLimit": 10000000000, // CPU limit
        "memoryLimit": 104857600, // Memory limit
        "procLimit": 50,
        "copyIn": {
            "a.js": {
                "fileId": "Z66NSY2T" // The compilation response will give the ID, remember to replace
            }
        }
    }]
}
```

### py

Compilation request (Although there is no actual compilation, you need to go through the compilation step to get the ID):

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

Running request:

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

Compilation request:

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
            "Main.java": { // Note this is the java file
                "content": "public class Main{ \n public static void main(String[] args) { \n System.out.println(\"Hello world\");\n }\n}"
            }
        },
        "copyOut": ["stdout", "stderr"],
        "copyOutCached": ["Main.class"] // This is the bytecode file
    }]
}
```

Running request:

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
            "Main.class": { // This is the bytecode file
                "fileId": "2O5QX3EB"
            }
        }
    }]
}
```

### go

Compilation request:

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
        ], // This is different from general languages. Don't ask, it's just more environment configurations
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
        "memoryLimit": 2516582400, // Note that memory is larger than 100MB
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

Running request:

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
        "memoryLimit": 2516582400, // Note that memory is larger than 100MB
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

Compilation request:

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

Running request:

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
                "fileId": "5LWIZAA45JHX4Y4Z" // The ID of this cached file comes from the fileIds returned by the previous request
            }
        }
    }]
}
```

### C

Compilation request:

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

Running request:

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

Other `API` references: [Go-Judge.API](https://docs.goj.ac/cn/api)

## Environment

### C/C++

​	In `/usr/bin`, you can see `gcc` and `g++`. You can manually uninstall this version and install other versions.

### Go

​	In `/usr/bin/go`. For the convenience of unified environment installation location, everything is managed under `/usr/bin/go`. In addition, the cache resources for the Go language are under `/tmp/`, including `go`, `go-cache`, and `go-tmp`. It is not recommended to modify these as they are carefully designed.

### Nodejs

​	In `/usr/bin`. You can install other versions of Nodejs or use `apt` to install `nvm` for version management. Detailed explanations are not provided here; please refer to the official `nvm` documentation.

### Java

​	In `/usr/bin/jdk-17.0.12`. Similarly, you can install `JEnv` for `Java` version management. Detailed explanations are not provided here; please refer to the [`JEnv` project address](https://github.com/jenv/jenv).

### Python

​	In `/usr/bin`. Note that `py` in Linux is `python3`, not `Python`—this is a potential pitfall. Also, Python does not natively support command-line parameters; you need to add relevant dependencies in the code yourself, such as "sys". The specific code is as follows:

```py
import sys

# sys.argv[0] is the script name
# sys.argv[1] is the first parameter, and so on

if len(sys.argv) > 1:
    print(f"Script name: {sys.argv[0]}")
    print(f"First parameter: {sys.argv[1]}")
    print(f"All parameters: {sys.argv[1:]}")
else:
    print("Please provide command-line parameters")
```

Other system-level settings reference: [Go-Judge.configuration](https://docs.goj.ac/cn/configuration)
