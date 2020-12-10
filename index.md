## Welcome to the go-ms community

Here you will find documentation about the proto-gen-go-ms plugin. This is protobuf code generation for go-ms. We use protoc-gen-go-ms to reduce boilerplate code.

### Install

Use the go get command to install the protoc-gen-go-ms binary:

```markdown
go get github.com/yadisnel/protoc-gen-go-ms/v2
```

### Usage

Define your service as `greeter.proto`:

```markdown
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


