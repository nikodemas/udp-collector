### CMSSW UDP collector service

[![Go CI build](https://github.com/dmwm/udp-collector/actions/workflows/go-ci.yml/badge.svg)](https://github.com/dmwm/udp-collector/actions/workflows/go-ci.yml)

The new CMSSW UDP collector service consist of UDP server `udp_server`
and `udp_server_monitor` monitor application. To compile them you
need a [Go-lang](http://golang.org/) to be installed on your system.
Then use the following instructions:
```
# build executables
go build udp_server.go
go build udp_server_monitor.go
go build udp_client.go
```

### Service maintenance
To start the service please compile `udp_server` and `udp_server_monitor`
executables and put it on your node, then
```
# create your udp_server.json file, and provide proper credentials
cat > udp_server.json << EOF
{
    "port": 9331,
    "bufSize": 2048,
    "monitorInterval": 10,
    "monitorPort": 9330,
    "stompLogin": "xxx",
    "stompPassword": "yyy",
    "stompURI": "zzz:port"
    "endpoint": "/abc/xyz",
    "contentType": "application/json",
    "verbose": false
}
EOF
# make sure that PATH contains path to location of your executable, e.g.
export PATH=$PATH:$PWD
# start udp_server_monitor process which will take care of udp_server
nohup ./udp_server_monitor 2>&1 1>& log < /dev/null &
```

### Testing procedure
We can test our udp server with provided udp client code.
```
# start server as following
udp_server -config udp_server.json

# start client as following
udp_client
```
The `udp_client` provides options to specify host, port and number of
documents to be used.
