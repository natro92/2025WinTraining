<h1 id="m73YY">1.flower-1</h1>
是之前预备队训练的maze，具体WP参考[之前的WP](https://www.yuque.com/sdtvdp/hd8of8/kunibq4bp5uotpph#Nw14g)中的3.maze部分，考点为原版upx壳的脱壳及花指令的去除

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737809561418-b372235c-fc1a-4544-9598-a77eedb6f236.png)

flag为flag{ssaaasaassdddw}

<h1 id="pzh4F">2.flower-2</h1>
![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737809650726-dfb576fe-fe6c-48f1-8e82-3895c7de0a56.png)

照惯例用exeinfo看一下是否带壳，结论是不带壳，那么就可以直接用IDA分析，爆红非常明显的junk code

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737809780019-fa64421e-8a1c-4492-b307-9b3796b8b364.png)

从红色往上看即可看到一组jz和jnz同样到达loc_4010D4+1的位置，因此位于loc_4010D4的代码即为junk code（任意情况下程序正常执行均不会执行这段代码）

按d转换为数据再nop掉即可，最简单的junk code（所以为什么这个不是第一题

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737812519526-cb057969-dc7e-477e-aee0-ebed53d07e24.png)

正常代码就出来了，之后把标红部分按p声明函数即可正常反汇编，tab后观察反编译的伪代码

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737812498504-ee24c6af-ed4b-4679-babc-54b0ba4705cd.png)

经修改sub类识别错误的函数后结果如图，其中Arglist应为我们输入的内容

加密过程都写出来了，解密逆着写就行

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737813205552-64363bcb-26ce-43fc-b948-e927a0f98f6a.png)

运行程序即可得到

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737813236875-f43cd885-2af1-4a8e-abe6-13679e10a9b8.png)

flag即为 NSSCTF{Just_junk_Bytess}

<h1 id="vTNT4">3.flower-3</h1>
![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737814959083-0c20bec0-81eb-4014-8718-75909b33ff16.png)

exeinfo查查成分，无壳，直接IDA

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737815119630-d82740e4-6173-4400-b842-b25f35386d80.png)

按tab将（虚假的）伪代码转换成汇编看看情况，得知这里有两处标红

但是其实标红原因和上一处完全一致，都是由于jz和jnz导致的永真条件跳转

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737815199550-e8e521a0-9d06-4be3-86b5-432bcad71ef0.png)

转换为数据形式之后再把两个箭头指出的0E9h nop掉即可，都是与上一题同理的正常执行过程中永远不会执行到的数据

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737816075514-a783d15e-af79-4791-8dc3-df9bd5ec79d9.png)

观察代码，得知v4是我们输入的flag，因此需要对v4和unk_403000进行sub_401110验证，在上面v4会被000和080两个函数进行两次操作

先看看unk是什么东西

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737815833968-685de9ab-1713-465c-9970-fe4f3707a666.png)

shiftE提取一下

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737815913326-1947ad39-b8ca-4c07-bb15-6d868dedfd61.png)

随后回头看函数的操作

第一个000中

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737817345447-eb7bbc7f-259e-4670-8a62-67c643ad12bd.png)

使用a[i+1]+a[i]的方式加密，因此可知a[i]数列的最后一个元素并未被加密，用最后一个ai解密即可解密出之前所有的a[i]

然后再观察080

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737817542570-0fca4448-fcb1-4612-9cf3-8ab334282b16.png)



这个代码串核心在于break下的一句执行的操作，使用或连接的右侧将a中第i个元素右移4位，即将前四位与后四位互换；或的左侧则将a乘16，获得a的后四位

即 1001 1010在操作后变为1010 1001

因此直接复制这段代码即可完成解密

最后看110操作

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737819600525-08d836c7-f0c1-458e-87cb-24ad38a1cab5.png)

看着很唬人其实只是逐个比对前后元素，意思即为输入flag和前导出的字符串中的每个元素均相等

反向写出解密代码即可

![](https://cdn.nlark.com/yuque/0/2025/png/50154293/1737822797886-aa8e1487-d517-4c30-bf1b-2594c510e0e7.png)

最后得出flag为

moectf{p4tch_pr0gr4m_t0_d3c0mpi1e_it!}







