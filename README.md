# BDT

<p align="center">
  <a href="https://buckycloud.com/">
	<img alt="BDT" src='https://raw.githubusercontent.com/buckyos/bdt/master/logo.jpg'/>
  </a>
</p>

BDT 一个基于NAT穿透的高性能P2P协议栈，使用该协议可以简单快捷地实现一个分布式系统中节点之间的可靠数据传输。

BDT协议由“GeekChain基金会”赞助，[“中国深圳巴克云网络科技有限公司”][organization]开发；采用开源的策略和各参与志愿者们共同维护、管理和更新。

[organization]: https://buckycloud.com/

## 授权协议

[BSD-3-Clause][LICENSE]

[LICENSE]: BDT_LICENSE

## 开发环境

BDT协议目前只有JS版本，在[Node.js 8.9][]版本上开发实现。

### 第三方依赖：

msgpack-lite

日志版额外依赖fs-extra

[Node.js 8.9]: https://nodejs.org/en/blog/release/v8.9.0/

## 快速入门

要快速了解项目提供组件及内容，可以先阅读接口文档[`P2P_interface.js`][]

[`P2P_interface.js`]: /doc/P2P_interface.js

### 环境准备

1. 一台公网服务器，作为穿透辅助服务器(SN)

2. 两台普通计算机，作为测试连接的服务端和客户端；当然，在项目初期，穿透服务器和测试机都可以使用同一台开发机进行单机测试

3. 在所有计算机上安装[Node.js 8.9][]

4. Clone BDT工程到开发环境

5. 安装依赖包：

```
	npm i msgpack-lite
```

### 第一个BDT分布式程序

1. 	启动自己的SN，一般没什么定制逻辑，直接启动[`start_simple_sn.js`][]就好:

```
	node ./sample/start_simple_sn.js -peerid peerid\_sn -udp port1 -tcp port2
```

2. 	编写自己的服务端程序，参考[`start_simple_server.js`][]:

```
	node ./sample/start_simple_server.js -peerid peerid\_server -udp port3 -tcp port4 -vport vport1 -sn peerid=peerid\_sn,ip=sn-ip,udp=port1,tcp=port2
```

3. 	编写自己的客户端程序，参考[`start_simple_client.js`][]:

```
	node ./sample/start_simple_client.js -peerid peerid\_client -udp port5 -tcp port6 -sn ip=sn-ip,udp=port1,tcp=port2 -server peerid=peerid\_server,vport=vport1
```

4. 	行文说明(大神略过):
	* 	peerid: 每个节点（包括SN）的唯一标识；以下peerid\_sn、peerid\_server和peerid\_client分别代表SN、服务端和客户端的peerid，可以自行更改指定

	*	udp/tcp:每个节点（包括SN）都可以监听UDP和TCP各一个端口，也可以只指定其中一个，但各节点监听的端口类型必须一致，否则会出现连通障碍；以下port1-port6分别代表自己的监听端口，用户根据需要自行指定，不同机器上运行的节点实例可以监听相同的端口，但运行在相同机器上的节点实例必须监听不同的端口，否则会有端口冲突；

		端口一致: 以下标号相同的portn必须指定相同，如启动SN时的udp监听端口port1，在启动服务端时指定的SN参数port1，它们应该相同；

	*	IP: 每台机器可能有多个IP，BDT协议建议用户指定使用特定IP，但例子程序只简单的监听了'0.0.0.0'地址，在生产环境上，这样做可能会因为发现不了本地的局域网IP，导致同局域网节点无法连通，因为测试发现部分路由器不支持通过公网IP访问同局域网机器；

		SN作为所有节点发生关联的种子节点，任何节点上线都必须准确指定SN的peerid和IP地址及监听端口；

	*	vport:类似TCP协议的端口，是一个16位整数，作为BDT服务端提供服务的标识，BDT客户端通过服务端的peerid和vport即可发起连接；

[`start_simple_sn.js`]: /sample/start_simple_sn.js
[`start_simple_server.js`]: /sample/start_simple_server.js
[`start_simple_client.js`]: /sample/start_simple_client.js



