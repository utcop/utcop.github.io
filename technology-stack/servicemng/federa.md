
在云计算环境中，服务的作用距离范围从近到远一般可以有：同主机（Host，Node）、跨主机同可用区（Available Zone）、跨可用区同地区（Region）、跨地区同服务商（Cloud Service Provider）、跨云平台。K8s 的设计定位是单一集群在同一个地域内，因为同一个地区的网络性能才能满足 K8s 的调度和计算存储连接要求。而集群联邦（Federation）就是为提供跨 Region 跨服务商 K8s 集群服务而设计的。

每个 Federation 有自己的分布式存储、API Server 和 Controller Manager。用户可以通过 Federation 的 API Server 注册该 Federation 的成员 K8s Cluster。当用户通过 Federation 的 API Server 创建、更改 API 对象时，Federation API Server 会在自己所有注册的子 K8s Cluster 都创建一份对应的 API 对象。

在提供业务请求服务时，K8s Federation 会先在自己的各个子 Cluster 之间做负载均衡，而对于发送到某个具体 K8s Cluster 的业务请求，会依照这个 K8s Cluster 独立提供服务时一样的调度模式去做 K8s Cluster 内部的负载均衡。而 Cluster 之间的负载均衡是通过域名服务的负载均衡来实现的。

![federation](../images/federation-api.png)


[fedaration](https://kubernetes.feisky.xyz/he-xin-yuan-li/index-1/federation)