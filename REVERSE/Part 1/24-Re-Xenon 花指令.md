<h1 id="sI2CN">补充练习</h1>
<h2 id="WJlGb">Dirty_flowers--2024 NewStar Week2</h2>
<h3 id="RKzVn">step1:首先用ExeinfoPe查看文件，发现时32位且无壳</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737766394426-7911f5ce-79c3-4cb1-a7e0-a3dc95e1789b.png)

<h3 id="Sidie">step2:用ida打开，并找到主函数，发现存在花指令</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737766662445-3d2e339b-8563-4598-84e5-7cbd876b9834.png)

<h3 id="tZOuw">step3:通过字符串搜索发现提示</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737766755625-3d0abe07-d74c-4c97-98f5-df738f9225bf.png)

提取出具体的字符串信息

If you still don't understand how to remove it, you can change all the instructions in the 0x004012F1-0x00401302 section to nop instructions.Then press U and then P at the position of the function head.

给出了移除花指令的方法，就是对地址0x004012F1-0x00401302修改成nop指令，再在函数头处点击up。

<h3 id="E9GE5">step4:进行提示所给的操作</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737782549220-171924ec-c293-4c49-972a-8376e546ad9a.png)

随后便可以进入主函数

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737782596767-52954c4d-e574-461f-b2b8-bca53449ff89.png)

<h3 id="DFtLN">step5:发现在之前还有加密函数sub_401100();</h3>
这里同样利用上述方法移除花指令，最终发现

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737782752388-e2f22530-5729-432c-95ad-eeaf7d83cadb.png)

<h3 id="UDu8U">step6:梳理加密逻辑</h3>
首先输入字符串长度为36

然后进行异或加密（用字符串作为密钥）

点击sub_4011D0函数发现存在的密文

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737783139125-256b9b12-d1e0-4224-8d89-e659d31029d1.png)

转化成数组v3[36]={2,5,19,19,2,30,83,31,92,26,39,67,29,54,67，7,38,45,85,13,3,27,28,45,2,28,28,48,56,50,85,2,27,22,84,15};

<h3 id="cR471">step7:编写解密脚本</h3>
C语言

#include<stdio.h>

#include<string.h>

int main(){

	int v3[36]={2,5,19,19,2,30,83,31,92,26,39,67,29,54,67,7,38,45,85,13,3,27,28,45,2,28,28,48,56,50,85,2,27,22,84,15};//密文

	char key[]="dirty_flower";

	int i,v2=strlen(key);

	for(i=0;i<36;i++)

	{

		v3[i]^=key[i%v2];

	} 

	for(i=0;i<36;i++)

	{

		printf("%c",v3[i]); 

	}

	return 0;

} 

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737785244556-b897e8b7-f119-4092-871d-12b829de3171.png)

得到flag{A5s3mB1y_1s_r3ally_funDAm3nta1}![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737785288738-2f4183d4-0bd4-4741-93fc-ca33009a4244.png)

也可以用python

```plain
v3=[2,5,19,19,2,30,83,31,92,26,39,67,29,
    54,67,7,38,45,85,13,3,27,28,45,2,28,
    8,48,56,50,85,2,27,22,84,15]
key='dirty_flower'
flag=''
for i in range(len(v3)):
    v3[i]^=ord(key[i%len(key)])
    flag+=chr(v3[i])
print(flag)
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737785381289-58351e11-6bc9-4feb-b053-880b002c0695.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737785399032-1f96f44d-d01d-44bc-9bad-101d83f7fe2a.png)

<h3 id="kZMwz">最后补充一下花指令去除的原理</h3>
首先我们依次分析两次的花指令处理

<h4 id="Hq8iv">part1</h4>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737899243993-bd428f95-ef36-4828-9211-dc2f59aac917.png)

此处代码是有字符串提示删除的，是典型的push--pop花指令。

先对eax寄存器进行大量的操作，在利用pop指令使其恢复，从而干扰正常编译。

顺便一提，这里可以编写idc程序处理

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737900024344-6bbf267c-bfd0-4dac-972a-cd91b52c8c7a.png)

<h4 id="iKdsU">part2</h4>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737900115408-0761db9d-6ae5-45b2-8f4a-1fb2c0ae2c0a.png)

类似的花指令push--pop



<h2 id="sPmjL">Re-花指令</h2>
<h3 id="snuCQ">step1:用ExeinfoPe查看文件，同样32位且无壳</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737900476979-9c5012d2-fb48-4d13-9b31-42e26ecfd2e5.png)

<h3 id="NWlM9">step2:用ida打开，但并未发现主函数</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737900596262-1f452e47-7869-443f-bd1e-75ea619e9e04.png)

通过字符串索引发现主函数踪迹，然后利用交叉索引

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737900675612-161e40c5-4d79-4243-9556-7391925ee4f4.png)

发现被花指令处理过

<h3 id="ZlYb0">step3:处理花指令</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737900787685-fcefa7c9-870a-4aeb-8eb3-bd11015b7daa.png)

发现push--pop修饰，全部nop处理

处理过后重新分析主函数

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737901135818-9aa8e058-65b5-4f27-b34e-705ed0c70973.png)

进入主函数

<h3 id="p4Roh">step4:分析加密原理</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737901271168-0963b614-222f-4b2b-9c14-7acfabdeeb69.png)

输入32位字符串之后，分别由sub_401040和sub_401080函数进行处理

<h3 id="yIUew">step5:sub_401040分析</h3>
点击后发现还是有花指令

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737901548339-ce314c35-69a7-43a1-95c7-4fd22b649ae5.png)

这里是call--return修饰，call+地址，然后修改堆栈返回值，但retn跳过混淆，这里均nop掉

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737901886535-4ee0ad21-7628-46bd-be2e-dccf65b9a545.png)

得到具体的加密算法-异或



<h3 id="TQKJ8">step6:sub_401080分析</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737901994048-4e8222ce-7a6a-498b-9dc0-c1e5eb2192e8.png)

同样是call-return处理，nop后得到

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737902095250-da896e79-31c2-497e-bb56-3553ba4dd1f1.png)

左移后进行异或

<h3 id="h3oOn">step7:利用动态调试，查找函数的调用顺序</h3>
设置断点

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737903422172-c12d822d-c7ce-43c0-a732-aae84e19eb97.png)

得到调用顺序8448 4884 8844 4888 4448 4484 8888 4448（用8和4分别代指函数sub_401080和函数sub_401040）

<h3 id="tSA1h">step8:编写解密脚本</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737904916215-bd7e191e-f3f2-4a76-bb11-272bd0e42a45.png)

找出密文，利用异或处理恢复

利用异或逆向处理，这里注意要使用4位的16进制数

dword_B02120 dd定义双字节数组！在头部添加0x表示十六进制数据

int dword={0x4408, 0x68D8, 0x7AD8, 0x4308, 0x7BD8, 0x4608, 0x7B08, 0x70D8,0x3308, 0x7308, 0x76D8, 0x5CD8, 0x76D8, 0x6608, 0x6908, 0x6E08,0x4BD8, 0x76D8, 0x3FD8, 0x6F08, 0x5ED8, 0x76D8, 0x7408, 0x46D8 ,0x5F08, 0x6308, 0x3408, 0x7408, 0x76D8, 0x44D8, 0x4CD8, 0x7D08};

C语言示例

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737985148925-89910f8f-3227-4661-9397-65c41747594b.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737985201235-9b390923-fe63-4da7-8a03-9f626e1d8373.png)

python示例

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737985266902-5c6d2fbb-fcde-4f87-a34b-dbd1b81130b0.png)

最后得到字符串DASCTF{Y3s_u_find_how_to_c4t_me}

<h1 id="CY1m1">课后作业</h1>
<h2 id="RH9Cq">flower1</h2>
<h3 id="K855B">step1:首先用ExeinfoPe查看文件，发现时32位且无壳</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738021031132-91aa8cc3-d78b-46e9-9f48-4ebe5f817eed.png)

<h3 id="kP8FT">step2:用ida打开并处理花指令</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738021130545-6481681a-f917-4494-980e-e0ffc8e5c95a.png)

这里jnz失效，而且随后的call语言调用的地址不存在，是jnz--call修饰

将call的指令修改成数据

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738021504083-6e8b9562-6088-4b83-8e42-a686681a1557.png)

nop掉db oe8h，重新生成主函数

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738021598198-31522ba4-36af-4cc9-914a-116bfecfcb7e.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738021678565-597dcd92-e3a7-42fe-9dd9-b44eca502d13.png)

<h3 id="WHq8F">step3:分析加密原理</h3>
从adsw判断是迷宫题，sub_401140应该是printf

结合初始位置

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738021807467-db63d6b2-fb3a-49a5-95e0-86ff4318c44b.png)

就是要输入14个字符使得从（0，7）走到（-4，5）

//实际上任意一种走法都能判对（不唯一）

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738021954656-d632119b-838a-4db7-bc07-f390fff4bcf2.png)

找出打印的地图

<h3 id="XTLSu">step4:编写解密脚本</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738022729700-464bf7e9-92f0-4153-8782-d810a18ca01b.png)

打印迷宫，从+一直到F，a左d右w上s下

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738022834077-e0914b20-7a13-457c-b0ce-2d5105c6731b.png)ssaaasaassdddw一共14个字符

Python示例

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738025583037-b3d53c8a-18dc-4ea8-85df-09aaabe23779.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738025592620-3dacf5fe-03f4-4f86-80d3-ab30f77a48cf.png)

<h3 id="DaN6e">step5:得到flag</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738025792596-1e2fc1de-0500-494b-9849-c0d5996eb12e.png)

最后再在原文件中得到flag

<h2 id="xnBIz">flower2</h2>
<h3 id="BbnHk">step1:用ExeinfoPe查看文件，同样32位且无壳</h3>


![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738026109397-57f8502e-7103-4e24-9f3f-f44290077008.png)

<h3 id="gzwtW">step2:打开ida进行分析，处理花指令</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738026163732-ba5adac3-ec95-46bb-b82b-c700e9d3b32c.png)

同样的，先把jmp指令改成数据

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738026211200-ab90ebfc-e606-4b01-adcb-bcd3efdaa9a6.png)

再去除db 0E9h干扰数据，重新生成主函数

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738026306866-98ac7ec5-fa74-4a68-b8ad-d01ef040d1ff.png)

<h3 id="LAl5h">step3:分析加密算法并编写解密脚本</h3>
首先进行奇偶项交换再进行异或处理

C语言

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738026929859-59826226-a84e-4de1-87df-644688b1d2ac.png)

Python

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738027318991-61cebcfa-71b9-48a5-8d35-880607b4ee96.png)

最后得到flag NSSCTF{Just_junk_Bytess}

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738027342272-48e445bd-bcdd-48a2-a2da-1d667032d57a.png)

<h2 id="vqmJR">flower3</h2>
<h3 id="NiBqn">step1:用ExeinfoPe查看文件，同样32位且无壳</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738027498589-92ec1792-6047-402f-a323-1818090a5d60.png)

<h3 id="NUdt1">step2:用ida打开，编译主函数并处理花指令</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738027620654-ccbb2025-a610-4acd-b137-aa7e7983e74e.png)

但是发现对于loc_401000，有花指令存在

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738027674956-eeecfd95-a030-4fd4-970c-5aaff6207cae.png)

jump跳转到一个不存在的地址

还是先改成数据![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738027739147-23f3454f-ca1f-44c2-9e47-2240e9551b00.png)

然后去除db 0E9h 干扰数据

同理处理loc_40108D

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738027876889-e1098c16-433e-4d49-b704-ce1b555514e2.png)

<h3 id="DHlfa">step3:分析加密算法</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738028474633-ee4a20e5-6640-41a3-ae05-48a6dcebbd27.png)

首先输入38位的字符串，然后经过sub_401000和sub_401080两个函数加密

最后和密文byte_403000进行字符串比较

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738028608424-b5ca2923-f981-4eff-ae1a-320c41549e38.png)

提取密文

<h3 id="bJdKM">step4:sub_401000分析</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738028728104-e97f639e-89bc-46ac-a653-4369e2e12f84.png)

数组的后一位都向前一位相加，利用最后一位不变来处理

<h3 id="NxuQ6">step5:sub_401080分析</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738028832816-b9dbeacf-22de-444d-a7c6-835c9c3fbe5f.png)

|是位运算符

a1[i]左移4位，然后与a1[i]右移后的4位进行按位异或操作

后面4位与前面4位进行交换

<h3 id="u18nV">step6:编写解密脚本</h3>
Python示例

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738030641135-5b21ad4a-88c3-4993-b5f1-231ef207d66d.png)

C语言示例

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738031156691-b2834cc8-3bc2-4a47-b982-9e56b6a59464.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738031180592-1a1f82b7-d884-4e90-93bb-b0f78da9e920.png)

Python示例

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738031203695-5783eec6-1b64-4099-a498-c72417ff5fe3.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1738031219634-a04bba45-0f80-46c9-8646-549b814c8ccc.png)

最后得到flag moectf{p4tch_pr0gr4m_t0_d3c0mpi1e_it!}



<h1 id="ed57V">课后问题</h1>
由于在 C/C++ 程序被编译链接成 PE 文件的过程中，编译器和链接器会添加一些初始化代码。这些代码会在main函数之前执行，例如全局变量的初始化、C 运行时库（CRT）的初始化等。所以，PE 文件可选头中的 AddressOfEntryPoint 字段不是程序中main函数的地址。它是程序在内存中开始执行的起始点，这个起始点的代码会在执行过程中调用main函数。





















