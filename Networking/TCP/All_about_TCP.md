

# Note

- TCP guarantees the reliable, in-order delivery of a stream of bytes.
- At the heart of TCP is the sliding window algorithm


# TCP Segment

![tcp-header](rsc/tcp-header.png)

## 报文头字段
-  **SrcPort:** Source port
-  **DstPort:** Destination port
-  **SequenceNum:** Sequence number of first byte
-  **Acknowledgment:** Carry information about the flow of data going in the other direction
-  **AdvetisedWindow:** Carry information about the flow of data going in the other direction
-  **Flags:** Relay control information (`SYN`, `FIN`, `RESET`, `PUSH`, `URG`, `ACK`)
-  **Checksum:** Compute
-  **Hdrlen:** Offset

> 一句话总结：TCP 报文头总共包含 24 个字节：前 4个字节（`SrcPort`, `DstPort`）定义源端口和目标端口；11个字节（`SequenceNum`, `Acknowledgment` 和 `AdvertisedWindow`）用于 TCP 的 Sliding window algorithm。`Flags` 字段用于传输一些相应信息（例如建立和断开连接时用到的 `SYN`, `FIN` 和 `ACK`）。


# 建立和断开 TCP 连接 

> TCP 连接原则是时对称的，即两个端都有发起连接和断开的能力，但通常情况下会有一个端进行 Active open 主动开启连接而另一端进行 Passive open 被动接受连接

## 三次握手 

![Three-way handshake](rsc/three_way_handshake.png)

### 为什么需要三次握手：

- 由于 SequenceNum 是随机生成的数字，并不是从 0 开始，因此双方都需要确认对方的 sequenceNum 

- 从算法层面来说，至少需要三次的沟通才能让双方都确认并建立连接

> 注意：
> 如果客户端向服务端发出的 `ACK`，即第三次握手发送失败了，那客户端依然会成功进入 ESTABNLISHED 状态，也就是说本地应用可以正常传输数据。这些传输的数据都会带有一个 `ACK` 标识，以及正确的 `Acknowledgment` 信息，所以服务端在接收到这些数据后仍然可以正确建连接。

## 四次挥手 

![Four-way handshake](rsc/four_way_handshake.png)

### 为什么需要四次挥手：

TCP 连接的断开需要两端都进行中断，如果其中有一方关闭连接，那代表它已经没有需要传输的数据了，但它仍然可以接受数据。同时，考虑到有可能两端在同一时刻触发关闭操作，或者一方主动关闭后，另一方也立刻主动关闭，因此两端都分别需要 3 个状态转换才能进入终止状态。

