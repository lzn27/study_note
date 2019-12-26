# 1. select poll epoll
1. select连接数比epoll少
2. select需要从内核空间拷贝数据到用户空间，epoll是共享内存更快
3. select轮询复杂度O(N)，epoll内核进行通知，时间复杂度O(1)。