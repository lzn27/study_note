# 1. lua variables 没有类型，value有类型。
```lua
a=1
b="asd"

print(type(a))
print(type(b))
--number string
```

# 2. _ 下划线一般作为特殊变量的开头
```lua
_VERSION
```
_也常常单独使用，作为一个假变量

# 3. lua是大小写敏感的

# 4. 关键字
and, break, do, else, elseif, end, false, for, function, if, in, local, nil, not, or, repeat, return, then, true, until, while

# 4. string 输出
```lua 
-- Example 8   -- Strings.

a="single 'quoted' string and double \"quoted\" string inside"
b='single \'quoted\' string and double "quoted" string inside'
c= [[ multiple line
with 'single'
and "double" quoted strings inside.]]

print(a)
print(b)
print(c)


-------- Output ------

single 'quoted' string and double "quoted" string inside
single 'quoted' string and double "quoted" string inside
 multiple line
with 'single'
and "double" quoted strings inside.

Press 'Enter' key for next example
```

# 5. 多重赋值
```lua
-- Example 9   -- Assignments.
-- Multiple assignments are valid.
--  var1,var2=var3,var4

a,b,c,d,e = 1, 2, "three", "four", 5

print(a,b,c,d,e)

-------- Output ------

1       2       three   four    5

-- Example 10   -- More Assignments.
-- Multiple assignments allows one line to swap two variables.
print(a,b)
a,b=b,a
print(a,b)

-------- Output ------
1       2
2       1

-- Example 11   -- Numbers.
-- Multiple assignment showing different number formats.可以有不同的数据表示格式
-- Two dots (..) are used to concatenate strings (or a
-- string and a number).使用..来连接字符串，..的最前面不一定是字符串，也可以 2..str

a,b,c,d,e = 1, 1.123, 1E9, -123, .0008
print("a="..a, "b="..b, "c="..c, "d="..d, "e="..e)



-------- Output ------

a=1     b=1.123 c=1000000000    d=-123  e=0.0008
```

# 6. 打印字符串
- print字符串时加不加（）都可以
- io.write字符串最后不会有空行，
- print()打印一个空行

# 7. if语句
```lua
-- Example 16   -- if statement.
-- Simple if.
a=1
if a==1 then
    print ("a is one")
end
-------- Output ------
a is one

-- Example 17   -- if else statement.
b="happy"
if b=="sad" then
    print("b is sad")
else
    print("b is not sad")
end
-------- Output ------
b is not sad

-- Example 18   -- if elseif else statement
c=3
if c==1 then
    print("c is 1")
elseif c==2 then
    print("c is 2")
else
    print("c isn't 1 or 2, c is "..tostring(c))
end
-------- Output ------
c isn't 1 or 2, c is 3
```

# 8. 条件语句
```lua
-- Example 19   -- Conditional assignment.
-- value = test and x or y
a=1
b=(a==1) and "one" or "not one"
print(b)

-- is equivalent to
a=1
if a==1 then
    b = "one"
else
    b = "not one"
end
print(b)

-------- Output ------
one
one
```

# 9. while语句
```lua
-- Example 20   -- while statement.

a=1
while a~=5 do -- Lua uses ~= to mean not equal使用 ~= 表示不等于
    a=a+1
    io.write(a.." ")
end


-------- Output ------

2 3 4 5
```

# 10. repeat until 语句
```lua
-- Example 21   -- repeat until statement.

a=0
repeat
    a=a+1
    print(a)
until a==5


-------- Output ------

1
2
3
4
5
```

# 11. for语句
```lua
-- Example 22   -- for statement.
-- Numeric iteration form.

-- Count from 1 to 4 by 1.
for a=1,4 do io.write(a) end

print()-- 打印空行

-- Count from 1 to 6 by 3.
for a=1,6,3 do io.write(a) end


-------- Output ------

1234
14



-- Example 23   -- More for statement.
-- Sequential iteration form.

for key,value in pairs({1,2,3,4}) do print(key, value) end


-------- Output ------

1       1
2       2
3       3
4       4
```

# 12. 打印 tables
```lua
-- Example 24   -- Printing tables.
-- Simple way to print tables.

a={1,2,3,4,"five","elephant", "mouse"}

for i,v in pairs(a) do print(i,v) end


-------- Output ------

1       1--table的索引从1开始，而不是0
2       2
3       3
4       4
5       five
6       elephant
7       mouse
```

# 13. 定义一个函数
```lua
-- Example 26   -- Functions.

-- Define a function without parameters or return value.
function myFirstLuaFunction()
    print("My first lua function was called")
end

-- Call myFirstLuaFunction.
myFirstLuaFunction()


-------- Output ------

My first lua function was called




-- Example 27   -- More functions.

-- Define a function with a return value.
function mySecondLuaFunction()
    return "string from my second function"
end

-- Call function returning a value.
a=mySecondLuaFunction("string")
print(a)


-------- Output ------

string from my second function



-- Example 28   -- More functions.

-- Define function with multiple parameters and multiple return values.
function myFirstLuaFunctionWithMultipleReturnValues(a,b,c)
    return a,b,c,"My first lua function with multiple return values", 1, true
end

a,b,c,d,e,f = myFirstLuaFunctionWithMultipleReturnValues(1,2,"three")
print(a,b,c,d,e,f)


-------- Output ------

1       2       three   My first lua function with multiple return values       1       true
```

# 14.所有变量都是全局变量
定义本地变量，要显式在前面加上 “lcoal” 
```lua
-- Example 29   -- Variable scoping and functions.
-- All variables are global in scope by default.

b="global"

-- To make local variables you must put the keyword 'local' in front.
function myfunc()
    local b=" local variable"
    a="global variable"
    print(a,b)
end

myfunc()
print(a,b)


-------- Output ------

global variable  local variable
global variable global
```

# 15. 格式化输出
```lua
-- Example 30   -- Formatted printing.
-- An implementation of printf.

function printf(fmt, ...)
    io.write(string.format(fmt, ...))--string.format返回格式化的字符串
end

printf("Hello %s from %s on %s\n",
       os.getenv"USER" or "there", _VERSION, os.date())


-------- Output ------

Hello there from Lua 5.1 on 05/07/20 11:14:46
```

# 16. lua library
```lua
 --[[

 Standard Libraries

  Lua has standard built-in libraries for common operations in
  math, string, table, input/output & operating system facilities.

 External Libraries

  Numerous other libraries have been created: sockets, XML, profiling,
  logging, unittests, GUI toolkits, web frameworks, and many more.

]]
```
1. internal library
```lua
-- Example 32   -- Standard Libraries - math.

-- Math functions:
-- math.abs, math.acos, math.asin, math.atan, math.atan2,
-- math.ceil, math.cos, math.cosh, math.deg, math.exp, math.floor,
-- math.fmod, math.frexp, math.huge, math.ldexp, math.log, math.log10,
-- math.max, math.min, math.modf, math.pi, math.pow, math.rad,
-- math.random, math.randomseed, math.sin, math.sinh, math.sqrt,
-- math.tan, math.tanh

print(math.sqrt(9), math.pi)
-------- Output ------
3       3.1415926535898



-- Example 33   -- Standard Libraries - string.

-- String functions:
-- string.byte, string.char, string.dump, string.find, string.format,
-- string.gfind, string.gsub, string.len, string.lower, string.match,
-- string.rep, string.reverse, string.sub, string.upper

print(string.upper("lower"),string.rep("a",5),string.find("abcde", "cd"))
-------- Output ------
LOWER   aaaaa   3       4


-- Example 34   -- Standard Libraries - table.

-- Table functions:
-- table.concat, table.insert, table.maxn, table.remove, table.sort

a={2}
table.insert(a,3);
table.insert(a,4);
table.sort(a,function(v1,v2) return v1 > v2 end)
for i,v in ipairs(a) do print(i,v) end
-------- Output ------
1       4
2       3
3       2


-- Example 35   -- Standard Libraries - input/output.

-- IO functions:
-- io.close , io.flush, io.input, io.lines, io.open, io.output, io.popen,
-- io.read, io.stderr, io.stdin, io.stdout, io.tmpfile, io.type, io.write,
-- file:close, file:flush, file:lines ,file:read,
-- file:seek, file:setvbuf, file:write

       print(io.open("file doesn't exist", "r"))


-------- Output ------
-- nil     file doesn't exist: No such file or directory   2


-- Example 36   -- Standard Libraries - operating system facilities.

-- OS functions:
-- os.clock, os.date, os.difftime, os.execute, os.exit, os.getenv,
-- os.remove, os.rename, os.setlocale, os.time, os.tmpname

print(os.date())


-------- Output ------

05/07/20 11:37:08
```

2. external libraries
```lua
-- Example 37   -- External Libraries.
-- Lua has support for external modules using the 'require' function
-- INFO: A dialog will popup but it could get hidden behind the console.

require( "iuplua" )
ml = iup.multiline
    {
    expand="YES",
    value="Quit this multiline edit app to continue Tutorial!",
    border="YES"
    }
dlg = iup.dialog{ml; title="IupMultiline", size="QUARTERxQUARTER",}
dlg:show()
print("Exit GUI app to continue!")
iup.MainLoop()


-------- Output ------

failed to load & run sample code
error loading module 'iuplua' from file 'C:\Program Files (x86)\Lua\5.1\clibs\iuplua51.dll':
        找不到指定的模块。
```


# 附录
1. pairs和ipairs，ipairs只能按照索引1，2，3...遍历，第一次遇到空的索引就会停止。pairs可以遍历所有。

2. lua closures闭包：closure is a function plus all it needs to aceess its **upvalue** correctly. Technically speaking, what is a value in Lua is the closure, not the function. The function itself is just a prototype for closures. Nevertheless, we will continue to use the term "function" to refer to a closure whenever there is no possibility of confusion.
```lua
    do
      local oldOpen = io.open
      io.open = function (filename, mode)
        if access_OK(filename, mode) then
          return oldOpen(filename, mode)
        else
          return nil, "access denied"
        end
      end
    end
    -- use closure to create secure environments, also called sandboxes. What makes this example nice is that, after that redefinition, there is no way for the program to call the unrestricted open.
```