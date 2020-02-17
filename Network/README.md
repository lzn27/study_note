# 1. select poll epoll
1. select连接数比epoll少
2. select需要从内核空间拷贝数据到用户空间，epoll是共享内存更快
3. select轮询复杂度O(N)，epoll内核进行通知，时间复杂度O(1)。

# 2. tcp与udp
OSI七层模型：物理层、数据链路层、网络层、传输层、会话层、表示层、应用层

1. tcp需要建立连接，udp无连接
2. TCP保证数据正确性，UDP可能丢包（tcp有超时重传）
3. TCP保证数据顺序，UDP不保证 （tcp使用序号，滑动窗口保证数据顺序）
4. TCP是流模式，UDP是数据报模式 
5. tcp消耗的系统资源更多。（TCP包头20字节，udp包头8字节。tcp包头比udp更长）

### UDP应用场景特点：
  1. 面向数据报方式
  2. 网络数据大多为短消息 
  3. 拥有大量Client
  4. 对数据安全性无特殊要求
  5. 网络负担非常重，但对响应速度要求高
  6. 例：ping命令

## tcp三次握手状态
1. CLOSED：起始点，在超时或者连接关闭时候进入此状态，这并不是一个真正的状态，而是这个状态图的假想起点和终点。
2. LISTEN：服务器端等待连接的状态。服务器经过 socket，bind，listen 函数之后进入此状态，开始监听客户端发过来的连接请求。此称为应用程序被动打开（等到客户端连接请求）。
3. SYN_SENT：第一次握手发生阶段，客户端发起连接。客户端调用 connect，发送 SYN 给服务器端，然后进入 SYN_SENT 状态，等待服务器端确认（三次握手中的第二个报文）。如果服务器端不能连接，则直接进入CLOSED状态。
4. SYN_RCVD：第二次握手发生阶段，跟 3 对应，这里是服务器端接收到了客户端的 SYN，此时服务器由 LISTEN 进入 SYN_RCVD状态，同时服务器端回应一个 ACK，然后再发送一个 SYN 即 SYN+ACK 给客户端。状态图中还描绘了这样一种情况，当客户端在发送 SYN 的同时也收到服务器端的 SYN请求，即两个同时发起连接请求，那么客户端就会从 SYN_SENT 转换到 SYN_REVD 状态。
5. ESTABLISHED：第三次握手发生阶段，客户端接收到服务器端的 ACK 包（ACK，SYN）之后，也会发送一个 ACK 确认包，客户端进入 ESTABLISHED 状态，表明客户端这边已经准备好，但TCP 需要两端都准备好才可以进行数据传输。服务器端收到客户端的 ACK 之后会从 SYN_RCVD 状态转移到ESTABLISHED 状态，表明服务器端也准备好进行数据传输了。这样客户端和服务器端都是 ESTABLISHED 状态，就可以进行后面的数据传输了。所以 ESTABLISHED 也可以说是一个数据传送状态。

## 四次挥手状态
1. FIN_WAIT_1：第一次挥手。主动关闭的一方（执行主动关闭的一方既可以是客户端，也可以是服务器端，这里以客户端执行主动关闭为例），终止连接时，发送 FIN 给对方，然后等待对方返回 ACK 。调用 close() 第一次挥手就进入此状态。
2. CLOSE_WAIT：接收到FIN 之后，被动关闭的一方进入此状态。具体动作是接收到 FIN，同时发送 ACK。之所以叫 CLOSE_WAIT 可以理解为被动关闭的一方此时正在等待上层应用程序发出关闭连接指令。前面已经说过，TCP关闭是全双工过程，这里客户端执行了主动关闭，被动方服务器端接收到FIN 后也需要调用 close 关闭，这个 CLOSE_WAIT 就是处于这个状态，等待发送 FIN，发送了FIN 则进入 LAST_ACK 状态。
3. FIN_WAIT_2：主动端（这里是客户端）先执行主动关闭发送FIN，然后接收到被动方返回的 ACK 后进入此状态。
4. LAST_ACK：被动方（服务器端）发起关闭请求，由状态2 进入此状态，具体动作是发送 FIN给对方，同时在接收到ACK 时进入CLOSED状态。
5. CLOSING：两边同时发起关闭请求时（即主动方发送FIN，等待被动方返回ACK，同时被动方也发送了FIN，主动方接收到了FIN之后，发送ACK给被动方），主动方会由FIN_WAIT_1 进入此状态，等待被动方返回ACK。
6. TIME_WAIT：从状态变迁图会看到，四次挥手操作最后都会经过这样一个状态然后进入CLOSED状态。共有三个状态会进入该状态

time_wait状态存在的意义：
- 可靠地实现TCP全双工连接的终止；
- 允许老的重复分节（数据报）在网络中消逝。

# 3. http、https
1. HTTP uses port number 80 for communication and HTTPS uses 443
2. HTTP is considered to be unsecure and HTTPS is secure
4. In HTTP, Encryption is absent and Encryption is present in HTTPS as discussed above
5. HTTP does not require any certificates and HTTPS needs SSL Certificates

# 4. epoll ET边缘触发模式下的epollin和epollout
ET模式称为边缘触发模式，顾名思义，不到边缘情况，是死都不会触发的。

EPOLLOUT事件：
EPOLLOUT事件只有在连接时触发一次，表示可写，其他时候想要触发，那你要先准备好下面条件：
1.某次write，写满了发送缓冲区，返回错误码为EAGAIN。
2.对端读取了一些数据，又重新可写了，此时会触发EPOLLOUT。
简单地说：EPOLLOUT事件只有在不可写到可写的转变时刻，才会触发一次，所以叫边缘触发，这叫法没错的！

其实，如果你真的想强制触发一次，也是有办法的，直接调用epoll_ctl重新设置一下event就可以了，event跟原来的设置一模一样都行（但必须包含EPOLLOUT），关键是重新设置，就会马上触发一次EPOLLOUT事件。

EPOLLIN事件：
EPOLLIN事件则只有当对端有数据写入时才会触发，所以触发一次后需要不断读取所有数据直到读完EAGAIN为止。否则剩下的数据只有在下次对端有写入时才能一起取出来了。

现在明白为什么说epoll必须要求异步socket了吧？如果同步socket，而且要求读完所有数据，那么最终就会在堵死在阻塞里。