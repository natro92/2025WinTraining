# 我的新手作业

## 作业1
https://www.nssctf.cn/problem/2928

先按题目说的，nc连一下
![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962063720-5b9d461b-e285-467d-a46e-2b0b889e1d91.png)

ls查看目录

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962201978-6b9bf332-b9b3-48aa-81b8-dfa6efea0347.png)

bin、dev这些都是常见Linux文件，看到目录下有一个flag，la查看一下

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962294098-a2cf3be7-4b2d-4bd7-a39c-4ab5d57e42f8.png)

可以看到flag是一个文件,cat打印一下

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962434875-410c6411-62ce-45f0-bcb8-3583a1b4b932.png)

显示fakeflag，可恶被骗了
开始查找看起来可能有flag的地方

前往gift目录，好像看到了flag

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962486463-3e0df82f-9523-40e7-bf70-0f9eb5455c93.png)

打印一下两个文件

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962531197-6818cce6-aa45-4a9f-9705-2e3a0b14d015.png)

成功找到flag后半段,
以此类推，继续找

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962576948-b19c0419-e367-4c37-b404-82011b7f6109.png)

在nothing里找到前半段，提交

### 作业2

在终端连接一下题目

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962649025-24c030ec-c562-4f46-a5fe-eb2a6dc4bfc5.png)

输入ls指令

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962693742-aa5c245a-7885-4475-846a-f749aca682dd.png)

指令被禁用,静态分析，发现cat等指令被禁用

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962726867-52fa88e7-4200-4260-b1e2-7ca39162074b.png)

绕过，使用标准报错输出

![](https://cdn.nlark.com/yuque/0/2025/png/52509818/1736962788846-b24ffc94-080b-4d8c-a121-e5bd64d28853.png)

得到flag
