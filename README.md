# Go-judge → LNCCOJ

## Description

​	This image is derived from [Go-Judge](https://github.com/criyle/go-judge) and includes 6 commonly used programming languages (listed below). The goal is to provide developers with a ready-to-use solution without the need for extensive language environment configuration.

```bash
Nodejs：v20.19.2
Python：3.13.5
Java：17.0.12
Go：1.24.8
gcc：14.2.0
g++：14.2.0
```

## Version Information

    `vX.1` is for standalone deployment, `vX.2` is for cluster deployment.

## Installation Guide

​	First, you need a basic understanding of Docker. The host machine can be win11 or Linux (tested successfully with WSL+Docker on win11). Second, you need access to efficient downloading resources. Finally, you need postman or other testing tools.

### Standalone Deployment

#### Loading the Image

Download the `vX.1` version, then load the image using the load command:

```bash
# Load the image
docker load -i filename.tar
```

You can also rename the image afterwards:

```bash
# Modify information
docker tag <Original Image Name>:<Original Tag> <New Image Name>:<New Tag>
```

#### Running the Container

```bash
docker run -d --privileged --shm-size=512m -p 5050:5050 lnccoj
# Explanation
docker run <parameters> <image name>
-d Run in the background, does not occupy the current terminal
--privileged Grants the container privileges almost equivalent to the root user on the host machine
--shm-size=512m Configuration parameter for shared memory size
-p Port mapping, host:container

# Alternatively, use
docker run -d --name lnccoj --privileged --cpus 2 --cpuset-cpus 0-1 -m 4G -p 5050:5050 -p 5052:5052 lnccoj:v1.1 -http-addr 0.0.0.0:5050 -enable-metrics -monitor-addr 0.0.0.0:5052

# Explanation
--cpus 2: Limits the container to use at most 2 CPU cores.
--cpuset-cpus 0-1: Restricts the container to run only on the specified CPU cores, here cores 0 and 1.
-m 4G: Limits the container's memory usage to 4GB.
-p 5050:5050: Maps the host's port 5050 to the container's port 5050.
-p 5052:5052: Maps the host's port 5052 to the container's port 5052.
-http-addr 0.0.0.0:5050: This parameter is passed to the application inside the container, specifying the HTTP service address as 0.0.0.0:5050.
-enable-metrics: Enables metrics collection (typically a feature of the application for collecting and exposing monitoring metrics).
-monitor-addr 0.0.0.0:5052: Specifies the address for exposing monitoring metrics as 0.0.0.0:5052.
```

#### Checking Container Status

```bash
docker ps -al
# If you see the following, it means success
CONTAINER ID   IMAGE             COMMAND           CREATED        STATUS        PORTS                    NAMES
7b9c6fecbbd0   lnccoj   "/opt/go-judge"   24 hours ago   Up 24 hours   0.0.0.0:5050->5050/tcp   lnccoj
# Success is indicated by successful port mapping.
```

### Cluster Deployment

Download the `vX.2` version, then similarly load the container:

```bash
# Load the image
docker load -i filename.tar
```

You can also rename the image afterwards:

```bash
# Modify information
docker tag <Original Image Name>:<Original Tag> <New Image Name>:<New Tag>
```

After loading the image, write a Docker Compose file for one-click deployment. Please refer to the following yaml file (for reference only, this example may not run directly):

```yaml
services:
  python:
    image: python:3.9  # Replace with the actual container image as needed
    container_name: Python # Container name
    privileged: true # Enable privileged mode
    shm_size: 512m # Shared memory size configuration parameter
    ports: # Port mapping can be customized, but must not be duplicated
      - "5050:5050"
      - "5052:5052"
    networks:
      - app-network

  java:
    image: openjdk:11  # Replace with the actual container image as needed
    container_name: Java
    privileged: true
    shm_size: 512m
    ports:
      - "5051:5050"
      - "5053:5052"
    networks:
      - app-network

  go:
    image: golang:1.17  # Replace with the actual container image as needed
    container_name: Go
    privileged: true
    shm_size: 512m
    ports:
      - "5054:5050"
      - "5055:5052"
    networks:
      - app-network
   ... # More containers can be added as needed.
networks:
  app-network:
    driver: bridge
```

After completion, run the Docker Compose file to deploy the cluster.

Now you can proceed with testing 🤓👍

## Request Examples (Remember to remove comments when testing)

### js

Compilation Request:

```json
{
    "cmd": [{
        "args": ["/usr/bin/nodejs", "a.js"], // Compilation command
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": ""
        }, {
            "name": "stdout", // Standard input
            "max": 10240 // Maximum standard input
        }, {
            "name": "stderr", // Standard output
            "max": 10240 // Maximum standard output
        }],
        "cpuLimit": 10000000000, // CPU limit
        "memoryLimit": 104857600, // Memory limit
        "procLimit": 50,
        "copyIn": {
            "a.js": { // Note: change accordingly
                "content": "#!/usr/bin/env node\nconsole.log(\"🎉 Hello, World!\");" // Code section, remember to escape
            }
        },
        "copyOut": ["stdout", "stderr"],
        "copyOutCached": ["a.js"]
    }]
}
```

Execution Request:

```json
{
    "cmd": [{
        "args": ["a.js"],
        "env": ["PATH=/usr/bin:/bin"],
        "files": [{
            "content": "" // Runtime arguments
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
                "fileId": "Z66NSY2T" // The compilation response will provide an ID, remember to replace it
            }
        }
    }]
}
```

### py

Compilation Request (Although there is no compilation step for Python, you still need to go through the compilation step to get the file ID):

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

Execution Request:

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

Compilation Request:

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
            "Main.java": { // Note: this is the Java file
                "content": "public class Main{ \n public static void main(String[] args) { \n System.out.println(\"Hello world\");\n }\n}"
            }
        },
        "copyOut": ["stdout", "stderr"],
        "copyOutCached": ["Main.class"] // This is the bytecode file
    }]
}
```

Execution Request:

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

Compilation Request:

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
        ], // This is different from most languages, don't ask, just more environment configuration
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
        "memoryLimit": 2516582400, // Note: memory greater than 100MB
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

Execution Request:

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
        "memoryLimit": 2516582400, // Note: memory greater than 100MB
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

Compilation Request:

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

Execution Request:

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
                "fileId": "5LWIZAA45JHX4Y4Z" // This cached file ID comes from the fileIds returned in the previous request
            }
        }
    }]
}
```

### C

Compilation Request:

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

Execution Request:

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

For other `APIs`, refer to: [Go-Judge.API](https://docs.goj.ac/cn/api)

## Environment

### C/C++

​	Under `/usr/bin`, you can find `gcc` and `g++`. You can uninstall this version and install other versions manually.

### Go

​	Under `/usr/bin/go`. To unify the environment installation location, everything is managed under `/usr/bin/go`. Besides this, Go language cache resources are under `/tmp/`, including `go`, `go-cache`, and `go-tmp`. It is not recommended to modify this, as it is carefully designed (.

### Nodejs

​	Under `/usr/bin`. You can install other versions of Nodejs, or use `apt` to install `nvm` for version management. This will not be explained in detail here; please refer to the official `nvm` documentation.

### Java

​	Under `/usr/bin/jdk-17.0.12`. Similarly, you can install `JEnv` for `Java` version management. This will not be explained in detail here; please refer to the [JEnv project address](https://github.com/jenv/jenv).

### Python

​	Under `/usr/bin`. Note that `py` in Linux is `python3`, not `Python`, which is a pitfall. Additionally, `Py` does not natively support command-line arguments; you need to add relevant dependencies in the code yourself. For example: "sys". Specific code is as follows:

```py
import sys

# sys.argv[0] is the script name
# sys.argv[1] is the first parameter, and so on

if len(sys.argv) > 1:
    print(f"Script name: {sys.argv[0]}")
    print(f"First parameter: {sys.argv[1]}")
    print(f"All parameters: {sys.argv[1:]}")
else:
    print("Please provide command line parameters")
```

For other system-level settings, refer to: [Go-Judge.configuration](https://docs.goj.ac/cn/configuration)
