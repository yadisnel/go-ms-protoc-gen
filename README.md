[![Inline docs](http://inch-ci.org/github/yadisnel/protoc-gen-go-ms.svg?branch=master)](http://inch-ci.org/github/yadisnel/{protoc-gen-go-ms})

# protoc-gen-go-ms

This is protobuf code generation for go-ms. We use protoc-gen-go-ms to reduce boilerplate code.

## Install

```
go get github.com/yadisnel/protoc-gen-go-ms/v2
```

Also required: 

- [protoc](https://github.com/google/protobuf)
- [protoc-gen-go](https://github.com/golang/protobuf)

## Usage

Define your service as `greeter.proto`

```
syntax = "proto3";

service Greeter {
	rpc Hello(Request) returns (Response) {}
}

message Request {
	string name = 1;
}

message Response {
	string msg = 1;
}
```

Generate the code

```
protoc --proto_path=$GOPATH/src:. --go-ms_out=. --go_out=. greeter.proto
```

Your output result should be:

```
./
    greeter.proto	# original protobuf file
    greeter.pb.go	# auto-generated by protoc-gen-go
    greeter.pb.go.ms.go	# auto-generated by protoc-gen-go-ms
```

The go-ms generated code includes clients and handlers which reduce boiler plate code

### Server

Register the handler with your go-ms server

```go
type Greeter struct{}

func (g *Greeter) Hello(ctx context.Context, req *proto.Request, rsp *proto.Response) error {
	rsp.Msg = "Hello " + req.Name
	return nil
}

proto.RegisterGreeterHandler(service.Server(), &Greeter{})
```

### Client

Create a service client with your go-ms client

```go
client := proto.NewGreeterService("greeter", service.Client())
```

### Errors

If you see an error about `protoc-gen-go-ms` not being found or executable, it's likely your environment may not be 
configured correctly. If you've already installed `protoc`, `protoc-gen-go`, and `protoc-gen-go-ms` ensure you've included 
`$GOPATH/bin` in your `PATH`.

Alternative specify the Go plugin paths as arguments to the `protoc` command

```
protoc --plugin=protoc-gen-go=$GOPATH/bin/protoc-gen-go \
--plugin=protoc-gen-go-ms=$GOPATH/bin/protoc-gen-go-ms \
--proto_path=$GOPATH/src:. --go-ms_out=. --go_out=. greeter.proto
```

## LICENSE

protoc-gen-go-ms is a liberal reuse of protoc-gen-go hence we maintain the original license.
