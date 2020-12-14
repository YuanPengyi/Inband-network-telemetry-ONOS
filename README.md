# Inband-network-telemetry-ONOS

## 给Collector添加虚拟端口
```bash
sudo ip link add veth_1 type veth peer name veth_2 
sudo ip link set dev veth_1 up 
sudo ip link set dev veth_2 up 
```
## 运行控制器
```bash
ONOS_APPS=drivers,proxyarp,lldpprovider,hostprovider,fwd,gui,inbandtelemetry,bmv2 bazel run onos-local -- clean
```

## 运行mininet
```bash
$ONOS_ROOT/tools/test/topos/bmv2-demo.py --onos-ip=127.0.0.1 --pipeconf-id=org.onosproject.pipelines.int
```

## 运行Collector
```bash
sudo python BPFCollector/InDBClient.py veth_2
```

## 为sink节点添加端口镜像
```bash
simple_switch_CLI --thrift-port `cat /tmp/bmv2-s12-thrift-port`
mirroring_add 500 4
```

## iperf生成流量
```bash
h11 iperf -c h22 -u -l 1k -t 10000
```

## web前端没有INT Collector Configuration解决方法：

修改文件：$ONOS_ROOT/tools/package/config/network-cfg.json
```json
{
  "apps": {
    "org.onosproject.inbandtelemetry": {
      "report": {
        "collectorIp": "127.0.0.1",
        "collectorPort": 54321,
        "minFlowHopLatencyChangeNs": 300
      }
    }
  }
}
```
