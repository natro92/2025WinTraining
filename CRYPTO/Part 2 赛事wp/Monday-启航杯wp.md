## 个人信息
个人名称：Monday

排名：204

![](https://cdn.nlark.com/yuque/0/2025/png/42554774/1737972276453-4d9f7366-02d5-4458-98ea-c1b0c5e23ae0.png)



## 解题情况
|  类型  |  题目名称  |  分数  |
| --- | --- | --- |
| Crypto | Easy_RSA | 50 |
| Misc | PvzHE | 50 |
| Misc | QHCTF For Year 2025 | 50 |




## 解题过程
### 1.Easy_RSA
打开py文件后发现是用RSA库进行的RSA加密，要解密就也用标准库解密就行了，私钥在实例里给了（实测问ai就可以给你一个很棒的解密代码）

访问/encode得到私钥和密文

![](https://cdn.nlark.com/yuque/0/2025/png/42554774/1737795517639-90f1e3f1-6094-478d-8071-dc1681312af3.png)

在python里用解密代码解一下就可以得到flag

解密代码如下

```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP
import base64


def decrypt_message(encrypted_message, private_key_pem):
    # 从PEM格式的字符串中导入私钥
    private_key = RSA.import_key(private_key_pem)

    # 创建一个PKCS1_OAEP解密器实例
    cipher = PKCS1_OAEP.new(private_key)

    # 解码加密的消息（假设它是base64编码的）
    encrypted_bytes = base64.b64decode(encrypted_message)

    # 解密消息
    decrypted_bytes = cipher.decrypt(encrypted_bytes)

    # 将解密后的字节转换为字符串（根据加密时的编码方式）
    decrypted_message = decrypted_bytes.decode('utf-8')  # 假设加密时使用的是UTF-8编码

    return decrypted_message


# 示例用法
encrypted_message = 'U/NX//s14D4iq1amFSLgzP+yjggLaTB453OS/8z8Zr0MIC3DhaYwD4OveoA1mwMFnhNJu+yMx6MnXiosbRg/lbnSAytFRsZtTrCUwfrdGDPgV8y5LG2TIZtnFNE+qZXhlYvq0CT2lS58VgJCzLTDUOZIfmx375vC18g3Ad5JoLU='  # 你的加密消息（base64编码的字符串）
private_key_pem = '''-----BEGIN RSA PRIVATE KEY-----\nMIICXAIBAAKBgQDhYf9JK5XNbCLlQS1vwddgjV9QGcin4GxVz6QOa+dPgtDm1UKm\nDxBYc7mnbb4gZIRVZVSy3HKUwCMJWuzevsrj5Pj7p1OOko7ecx7FB5SwDirf0o96\n/ZUufr58WiLA99tu5zVoBNarDatDdXundo0nPKaXoVN5cBV1vEfHbXrbHQIDAQAB\nAoGARRYeA9bZZ4ujMrUE8YHwvEb5lXsh19viOXeZqVYIJIZL9MYgVPi/IO3wVdM8\n+X72VZrnGKCcet5enwqBG1JOrHNu+V59ip7FgKaSI8w2CtydY82WKoorXdDXkrPI\nvQA38SSMtQFsG2IEckl3bykjSEa0cSnSmPxEFNvVYI7VDX0CQQDnAMjL1Oe3PZ5r\nV0aCbg9fM6yHcwQaufypL+qkkgYR+ttlVTMzx5v5apvrejTPHj604QkxQ2R7Dmca\nqywE8mk7AkEA+cWFZwwz7weZ667YEBFLYlzMHHvfL4vrOJE9A0SxjsPMGBH2OE21\nGN4oXLTxH7lpv1yNqDM3snSsYbA6DAJHhwJBAOX7Gu4L3lHJcBIQBuvN5VHz4T3M\n3XY6WniacvI7Sv3VkV4Wb+6KORgc8nSC61aVFvr/3CYGoV/+G9oqNp4KNYcCQGoz\nnEd2ntZx6vaVf3VFhUIrpvYtjXaQDdIdn022dbD4e914NbM3B1utiofwv933Xolp\ndyofrP0KMwnOfsAAcB0CQAXkAGz1O4WOB6WFaUHGSuUlGGjmgxKjssncjTIfunBe\nqBjdLRTHRCtpWeR4PS3k6ho9GvVAyzMGSig7ZyXOJ3E=\n-----END RSA PRIVATE KEY-----'''

decrypted_message = decrypt_message(encrypted_message, private_key_pem)
print(decrypted_message)

# QHCTF{3164e62d-8b23-4f22-8b93-2dfb0b43f0be}
```



### 2.PvzHE
文件下载后是一个植物大战僵尸的游戏，本来想玩一玩的，结果程序打不开，那没办法，我就打开前面的文件一个一个看（想着可能在哪个data文件里，找到我都不用玩），于是就在image文件夹发现下面这个图了

![](https://cdn.nlark.com/yuque/0/2025/png/42554774/1737796190151-b879694d-9216-42e5-9779-878d03445272.png)

转文字一下就可以得到falg



### 3.QHCTF For Year 2025
给了一个日历，再根据hint给的数字连线，就可以得到flag了

![](https://cdn.nlark.com/yuque/0/2025/png/42554774/1737798870033-83d4ec3d-d19a-4e0b-bf85-ee792700d064.png)

QHCTF{FUN}

















