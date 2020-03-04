# 1. 哈希冲突解决方法
1. 开放地址法
    - 线行探查法：冲突时以以1为步长向后寻找空单元，会发生聚集现象
    - 平方探查法：冲突时对原位置加上1^2、2^2、3^2等步长值，直到找到空闲单元
    - 双散列函数探查法：使用另一个哈希函数产生步长值。
2. 链地址法（也叫拉链法）
3. 再哈希法：增加了计算时间