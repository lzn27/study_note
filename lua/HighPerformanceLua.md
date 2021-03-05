## 1 local变量
lua读取local变量效率很高，尽量使用local。

## 2 table 重新哈希rehash
当需要创建数量很多的小table时尽量提前填充好每个table到预计的大小，避免rehash。单独创建一个很大的table并不会产生太多rehash。

## 3 字符串
lua的字符串都是内化的，默认重用。
lua一个字符串频繁进行连接时候会不断的拷贝这个字符串的副本连接完毕再拷贝回来，很慢。可以先把需要连接的字符串片段先放到一个table里面，然后用concat函数连接。
```lua
local t = {}
for line in io.lines() do
  t[#t + 1] = line
end
s = table.concat(t,"\n")
```

## 4 减少table的新建，尽可能重用table，根据具体试验适当调教垃圾收集器