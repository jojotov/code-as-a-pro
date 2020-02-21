# UDP (User Datagram Protocol)

## 优势

- 更小的包大小
  - UDP header: 8 bytes
  - TCP header: 20 bytes
- 无需建立连接
- 更可控的发送时机（没有 Congestiont Control）

## 劣势

- 没有 flow control/contestion control

- 不可靠

  - 数据只会发送一次 ，如果丢失不会重发
  - 数据传输无序

- 没有错误恢复（尽管 UDP 可以通过 `checksum` 保证数据正确，但错误的数据不会尝试恢复而是直接丢弃）

  

# TCP (Transmission Control Protocol)

## 优势

- 必须建立连接
- 传输确认机制（Delivery acknowledgment)
- 重新传输机制
- Congestion Control
- 错误检测（相对于 UDP 没有什么区别，但 `checksum` 在 IPv4 也是强制的，而 UDP 只在 IPv6 强制）



## 劣势

- 比较大的 header
- 数据可能不能及时传输（由于 congestion control）
- 重传机制和确认机制可能会带来更多的冗余



# 消息传输 vs 流传输

- UDP 是面向消息的
- TCP 面向流传输



# 应用场景

## TCP

- 需要确保数据完整性的场景，例如文本通讯、远程访问（SSH）、文件传输
- 需要保证数据不会丢失的场景。



## UDP

- 相对于完整性和准确性，更加强调即时性的场景，例如即时视频/音频通讯等。
- 一些小的传输场景，例如 DNS 查找。

