<h3 id="v64HA">step1:使用exeinfore查看文档</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737427639665-eb05c7b2-7e67-428c-969c-be36a7e4e100.png)

判断出该文件是32位且是UPX加壳，并且进行里加密处理

<h3 id="w8bsl">step2:使用010 Editor进行解密处理</h3>
![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737427834139-981f5eec-2588-42b2-b2df-f5ee3b244d57.png)

发现此处有多个PXU加密，应修改成UPX

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737428178195-6eb7bc41-3264-47c8-a868-cf425820f189.png)

再用exeinfore查看

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737428234877-6c130348-a943-4d62-ab24-2c94d3beaf13.png)

发现有一层UPX加密

<h3 id="ycHiQ">step3:进行脱壳处理</h3>
首先打开命令提示符

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737428378205-e8fcf8ea-044b-493c-a08d-735830ed7e24.png)

利用cd修改E盘地址

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737428589092-72a7d999-7748-438a-b008-cb8b636d3603.png)

完成脱壳，再返回exeinfore查看

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737428654732-24715118-f6d4-4ec3-878b-5f1e7a248208.png)

现在无壳，是基本文本形式

<h3 id="M33fn">step4:放入ida进行程序分析</h3>
首先通过查询发现主函数 _main

反汇编后分析程序

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737428882926-f7e6693e-071d-4f60-b444-223d0e391881.png)

很明显使用了base64加密，很快找到密文

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737429042209-d329fe33-0fc1-4866-abe6-eab46fabac79.png)

8CJJ8z918CyC3HtzObOJcov3B2Sh8upqNu6ic/hxZjeJcotz8CkJcoY9

<h3 id="aj7Bg">step5:处理base64的换表问题</h3>
分析加密逻辑

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737435969390-e1dfec86-cf7c-4066-aa4d-7b6b5e3067b9.png)

此处进行了三层加密

1.首先是字符串均被0x10异或

2.是进行了base64加密

3.最后是str[12]和str[18]进行互换

点击base64加密函数，再点击base64en

找到被更换的密码表

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737436138859-38b6aad8-bc8f-44e8-aba0-0cc77883e03d.png)

<h3 id="mlp9p">step6:编写解码脚本</h3>
```plain
import base64
str1="8CJJ8z918CyC3HtzObOJcov3B2Sh8upqNu6ic/hxZjeJcotz8CkJcoY9"#加密字符串
string1="Fvm/RkQucZNVyYABpS2w6enjdtGPO8UalxrbD45Ci07MT9KLEJo1h3zHgfX+WqsI"#换表
string2="ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"#标准
str1_list=list(str1)
str1_list[12],str1_list[18]=str1_list[18],str1_list[12]#位数互换
str1="".join(str1_list)#修正
xor_str=base64.b64decode(str1.translate(str.maketrans(string1,string2)))#base64解码
flag=''
for i in range(len(xor_str)):
    flag+=chr(xor_str[i]^0x10)
print(flag)
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936975/1737436381869-c6465bcb-173d-46ac-95a9-aafdfd9795b4.png)

得到最后的flag



<h1 id="z7tMi"></h1>
