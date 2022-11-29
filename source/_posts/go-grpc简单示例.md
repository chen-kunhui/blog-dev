---
title: grpc-go-demo
mathjax: false
date: 2019-11-10 21:04:31
updated: 2019-11-10 21:04:31
tags:
categories:
---

# 第一步安装 go

go 安装这里不做介绍，需要注意的是 go 版本需要 1.9 以上
安装完 go 后记得设置 GOPATH 环境变量，并把 $GOPATH/bin 添加到环境变量 PATH 中

# 第二步安装 Protocol Buffers 编译工具

> 源码地址：https://github.com/protocolbuffers/protobuf

1. 下载安装包

> https://github.com/protocolbuffers/protobuf/releases

可选择 protobuf-all-xxx.tar.gz 也可选择 protobuf-xxx-linux-x86_64.zip
区别在于一个是包含全部语言的，一个是仅包含 go 语言的

2. 解压后在bin目录下有 `protoc` 可执行文件，可用来编译 Protocol Buffer 格式文件，但还不能编译 service 部分

将解压后的bin 目录加到环境变量 PATH 中，比如：

```
# ~/.bashrc

export PATH=$PATH:/解压protocbuf的目录/bin
```

```
>_$ source ~/.bashrc
```

# 第三部安装 go 的 grpc 包及 protoc-gen-go

> 源码地址：https://github.com/grpc/grpc-go

通过 go 命令安装
```
go get -u goole.golang.org/grpc
go get -u github.com/golang/protobuf/protoc-gen-go
```

通过离线安装
```
git clone https://github.com/grpc/grpc-go.git $GOPATH/src/google.golang.org/grpc
git clone https://github.com/golang/net.git $GOPATH/src/golang.org/x/net
git clone https://github.com/golang/text.git $GOPATH/src/golang.org/x/text
git clone https://github.com/golang/protobuf.git $GOPATH/src/github.com/golang/protobuf
git clone https://github.com/google/go-genproto.git $GOPATH/src/google.golang.org/genproto
cd $GOPATH/src/
go install google.golang.org/grpc
cd $GOPATH/src/github.com/golang/protobuf/protoc-gen-go
go build
go install
```

# 第三部 编写demo 代码

```
cd $GOPATH/src
mkdir go-grpc-demo && cd go-grpc-demo
mkdir protocs
```

在 protocs 下创建一个helloworld.proto 文件，内容如下
```
// Copyright 2015 gRPC authors.
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at
//
//     http://www.apache.org/licenses/LICENSE-2.0
//
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

syntax = "proto3";

option java_multiple_files = true;
option java_package = "io.grpc.examples.helloworld";
option java_outer_classname = "HelloWorldProto";

package helloworld;

// The greeting service definition.
service Greeter {
  // Sends a greeting
  rpc SayHello (HelloRequest) returns (HelloReply) {}
}

// The request message containing the user's name.
message HelloRequest {
  string name = 1;
}

// The response message containing the greetings
message HelloReply {
  string message = 1;
}
```

编译proto 文件
```
mkdir helloworld
protoc -I protocs/ protocs/helloworld.proto --go_out=plugins=grpc:helloworld
```

创建服务端，创建 greeter_server/main.go 文件，内容如下

```
/*
 *
 * Copyright 2015 gRPC authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 *
 */

//go:generate protoc -I ../helloworld --go_out=plugins=grpc:../helloworld ../helloworld/helloworld.proto

// Package main implements a server for Greeter service.
package main

import (
	"context"
	"log"
	"net"

	"google.golang.org/grpc"
	pb "../helloworld"
)

const (
	port = ":50051"
)

// server is used to implement helloworld.GreeterServer.
type server struct {
	pb.UnimplementedGreeterServer
}

// SayHello implements helloworld.GreeterServer
func (s *server) SayHello(ctx context.Context, in *pb.HelloRequest) (*pb.HelloReply, error) {
	log.Printf("Received: %v", in.GetName())
	return &pb.HelloReply{Message: "Hello " + in.GetName()}, nil
}

func main() {
	lis, err := net.Listen("tcp", port)
	if err != nil {
		log.Fatalf("failed to listen: %v", err)
	}
	s := grpc.NewServer()
	pb.RegisterGreeterServer(s, &server{})
	if err := s.Serve(lis); err != nil {
		log.Fatalf("failed to serve: %v", err)
	}
}

```

启动服务端
```
go run greeter_server/main.go
```

创建客户端，创建 greeter_client/main.go 文件，内容如下
```
/*
 *
 * Copyright 2015 gRPC authors.
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 *
 */

// Package main implements a client for Greeter service.
package main

import (
	"context"
	"log"
	"os"
	"time"

	"google.golang.org/grpc"
	pb "../helloworld"
)

const (
	address     = "localhost:50051"
	defaultName = "world"
)

func main() {
	// Set up a connection to the server.
	conn, err := grpc.Dial(address, grpc.WithInsecure(), grpc.WithBlock())
	if err != nil {
		log.Fatalf("did not connect: %v", err)
	}
	defer conn.Close()
	c := pb.NewGreeterClient(conn)

	// Contact the server and print out its response.
	name := defaultName
	if len(os.Args) > 1 {
		name = os.Args[1]
	}
	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	defer cancel()
	r, err := c.SayHello(ctx, &pb.HelloRequest{Name: name})
	if err != nil {
		log.Fatalf("could not greet: %v", err)
	}
	log.Printf("Greeting: %s", r.GetMessage())
}

```

启动客户端，可看到拿到了数据
```
>_$ go run greeter_client/main.go
2019/11/10 23:57:50 Greeting: Hello world
```

客户端或服务端可以是其他语言的，比如 ruby 等，只要将 protoc 编译成ruby的即可，grpc 是跨语言的

> 以上代码均引用自官方demo: https://github.com/grpc/grpc-go/tree/master/examples

# 其他相关资料

- [protocol buffers doc](https://developers.google.com/protocol-buffers/docs/gotutorial)
- [grpc doc](https://www.grpc.io/docs/)
