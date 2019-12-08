# 1.redis 数据类型
- string 最多512MB
    - set：set mykey myvalue (nx(检查若已存在则不可进行),xx(默认强制))
    - get: get mykey
    - mset：批量设置
    ```
        mset a 10 b 20 c 30 返回‘OK’
    ```
    - mget：批量获取
    ```
        mget a b c 依次返回a b c 三个key的value
    ```
    - incr：对数字类型的字符串加1 
        ```
        set counter 50
        incr counter
        ```
        - incrby:设置加多少
        ```
        incrby counter 100 表示给counter的数据加上100
        ```
    - decr：减法，同上
        - decrby
- list 最多2^31个数据
    - lpush、rpush直接使用可创建list，可一次加入多个数据
    - lpop、rpop 弹出数据，只能一次一个
    - brpop、blpop 
    ```
        brpop mylist 10 若mylist为空，则最多等待10秒钟，10s后依然为空则返回
    ```
    - lrange mylist 0 -1 根据两个索引（这里是0，-1）返回mylist的[0,-1]范围的数据。
    
