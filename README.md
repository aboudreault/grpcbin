# grpcbin
httpbin like for gRPC

[![CircleCI](https://circleci.com/gh/moul/grpcbin.svg?style=svg)](https://circleci.com/gh/moul/grpcbin)
[![Docker Build Status](https://img.shields.io/docker/build/moul/grpcbin.svg)](https://hub.docker.com/r/moul/grpcbin/)
[![Go Report Card](https://goreportcard.com/badge/github.com/moul/grpcbin)](https://goreportcard.com/report/github.com/moul/grpcbin)
[![GoDoc](https://godoc.org/github.com/moul/grpcbin?status.svg)](https://godoc.org/github.com/moul/grpcbin)
[![License](https://img.shields.io/github/license/moul/grpcbin.svg)](https://github.com/moul/grpcbin/blob/master/LICENSE)

## Demo server

* endpoint (insecure): http://grpcb.in:9000
* [grpcbin.proto](https://github.com/moul/grpcbin/blob/master/grpcbin/grpcbin.proto)

## Run server locally

```console
$ docker run -it --rm -p 9000:9000 moul/grpcbin
2017/12/18 14:48:01 listening on :9000
```

## Example

Example with [grpcc](https://github.com/njpatel/grpcc):

```console
# fetch proto and install tool
$ wget -qN https://github.com/moul/grpcbin/raw/master/grpcbin/grpcbin.proto
$ npm install -g grpcc

# interactive client
$ grpcc -i -p ./grpcbin.proto --address grpcb.in:9000
Connecting to grpcbin.GRPCBin on grpcb.in:9000. Available globals:

  client - the client connection to GRPCBin
    index (EmptyMessage, callback) returns IndexReply
    dummyUnary (DummyMessage, callback) returns DummyMessage
    dummyServerStream (DummyMessage, callback) returns DummyMessage
    dummyClientStream (DummyMessage, callback) returns DummyMessage
    dummyBidirectionalStreamStream (DummyMessage, callback) returns DummyMessage

  printReply - function to easily print a unary call reply (alias: pr)
  streamReply - function to easily print stream call replies (alias: sr)
  createMetadata - convert JS objects into grpc metadata instances (alias: cm)

GRPCBin@grpcb.in:9000> ^C

# call index endpoint
$ grpcc -i -p ./grpcbin.proto --address grpcb.in:9000 --eval 'client.index({}, printReply)'
{
  "description": "gRPC testing server",
  "endpoints": [
    {
      "path": "index",
      "description": "This endpoint."
    },
    {
      "path": "dummyUnary",
      "description": "Unary endpoint that replies a received DummyMessage."
    },
    [...]
  ]
}

# call dummyUnary with arguments
$ grpcc -i -p ./grpcbin.proto --address grpcb.in:9000 --eval 'client.dummyUnary({f_string:"hello",f_int32:42}, printReply)'
{
  "f_string": "hello",
  "f_strings": [],
  "f_int32": 42,
  "f_int32s": [],
  "f_enum": "ENUM_0",
  "f_enums": [],
  "f_sub": null,
  "f_subs": [],
  "f_bool": false,
  "f_bools": [],
  "f_int64": "0",
  "f_int64s": [],
  "f_bytes": {
    "type": "Buffer",
    "data": []
  },
  "f_bytess": [],
  "f_float": 0,
  "f_floats": []
}
```
