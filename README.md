# Inband-network-telemetry-ONOS

给Collector添加虚拟端口

$ sudo ip link add veth_1 type veth peer name veth_2 
$ sudo ip link set dev veth_1 up 
$ sudo ip link set dev veth_2 up 

