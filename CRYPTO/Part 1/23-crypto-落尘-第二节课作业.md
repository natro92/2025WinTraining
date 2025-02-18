1.**安洵杯2020 easyAES**

  首先看到输出的key和hint的异或的结果，然后发现key和hint的长度不一样，而hint又是重复的字节，因此把key和hint的异或结果转为字符串就可以发现存在重复的序列，这个序列就是hint='}4$d'，知道了hint和key和hint异或的结果，根据异或的性质把hint和key和hint异或的结果进行异或就能得到key=b'd0g3{welcomeyou}'

   然后进行第二步的处理，AES_CBC所使用的加解密器和AES_ECB模式所使用的相同，由于我们知道了key还有明文，首先把明文分成每组16个字节，然后把列表翻转得到最后一组明文，又知道了最后一组密文，解密后就能得到上一组密文和这组明文异或的结果，把最后一组明文和最后一组密文解密得到的结果进行异或就能得到上一组密文，一直重复，知道遍历完整个明文列表，到最后把第一组密文块解密的结果和第一组明文进行异或就能得到初始化向量iv，也就能得到flag

完整脚本：

```python
import binascii
import libnum
from Crypto.Cipher import AES
from Crypto.Util.strxor import strxor

a=56631233292325412205528754798133970783633216936302049893130220461139160682777
print(libnum.n2s(a))
hint='}4$d'
key=a^libnum.s2n(hint * 8)
key=libnum.n2s(key)
print(key)

msg = b'Welcome to this competition, I hope you can have fun today!!!!!!'
msgs = [msg[i:i + 16] for i in range(0, len(msg), 16)]
msgs.reverse()  
enc=binascii.unhexlify('3c976c92aff4095a23e885b195077b66')

def decry(key, iv, ms):
    aes = AES.new(key, AES.MODE_ECB)  
    return strxor(aes.decrypt(iv), ms)  # 将参数进行异或操作

for ms in msgs:
    enc = decry(key, enc, ms)
print(b'd0g3{' + enc + b'}')

```

2.**[LitCTF 2023]babyLCG**

**lcg的内容**

![](https://cdn.nlark.com/yuque/0/2025/png/42557099/1737536509950-c035ba3d-bbe6-4074-a056-a8bd5a1f673a.png?x-oss-process=image%2Fformat%2Cwebp)

已知多个输出，a，b，m都不知道，那么首先要先求出m，这里的result就是xn的结果，先计算相邻元素的差值得到tn，在计算Tn=tn+1*tn-1-tn*2,去Tn和Tn+1最大公约数得到模数m，接着计算a和b，根据

 a=(xn+1 -xn)*(xn-xn-1)-1)modm

b=(xn+1 -a*xn) modm可以求出a和b的值，这里就可以直接使用result列表里的一两组数据就可以求出来了，然后就可以根据公式求出x0，也就是种子,然后就可以得到flag了

完整脚本：

```python
from functools import reduce
import gmpy2
import libnum

result = [699175025435513913222265085178805479192132631113784770123757454808149151697608216361550466652878,
          193316257467202036043918706856603526262215679149886976392930192639917920593706895122296071643390,
          1624937780477561769577140419364339298985292198464188802403816662221142156714021229977403603922943,
          659236391930254891621938248429619132720452597526316230221895367798170380093631947248925278766506,
          111407194162820942281872438978366964960570302720229611594374532025973998885554449685055172110829,
          1415787594624585063605356859393351333923892058922987749824214311091742328340293435914830175796909,
          655057648553921580727111809001898496375489870757705297406250204329094679858718932270475755075698,
          1683427135823894785654993254138434580152093609545092045940376086714124324274044014654085676620851,
          492953986125248558013838257810313149490245209968714980288031443714890115686764222999717055064509,
          70048773361068060773257074705619791938224397526269544533030294499007242937089146507674570192265]

list1 = [s1 - s0 for s0, s1 in zip(result, result[1:])]
#zip(result, result[1:]) 将相邻的两个元素配对，然后计算它们的差值
#也就是求tn，
list2 = [t2 * t0 - t1 * t1 for t0, t1, t2 in zip(list1, list1[1:], list1[2:])]
#求Tn=tn+1*tn-1-tn*tn
m = gmpy2.gcd(list2[1], list2[2])
# m = abs(reduce(gcd, list2))
print(m)

# 第二阶段，获得常数A, B
A_son = result[2] - result[1]
A_mother_ni = gmpy2.invert(result[1] - result[0], m)
A = pow(A_son * A_mother_ni, 1, m)
# print(A)
B = pow(result[1] - A * result[0], 1, m)
# print(B)

# 第三阶段，逆推回seed
A_ni = gmpy2.invert(A, m)
seed = (result[0] - B) * A_ni % m
# print(seed)也就是求x0
flag = libnum.n2s(int(seed))
print(flag)
```

3.**hgame-week2 notRC4**

看见题目的rc4就猜测应该是rc4的题目，去详细了解了一下rc4的加密过程

首先是初始化算法(KSA)

首先生成一个key，也就是密钥，相当于一个盒子，有256个元素，如果key是678，那么就把678扩展到256位，也就是'678678.....'长度为256，

然后生成一个s盒，也有256个元素，分别为0,1,2，....，255

然后用key初始化s盒

```python
j = 0
key_length = len(key)
for i in range(256):
    j = (j + S[i] + key[i % key_length]) % 256
    S[i], S[j] = S[j], S[i]  # 交换S[i]和S[j]
```

  由于未初始化的S盒都是确定的，所以j可以看作只受key的值的影响   打乱后的s盒称为初始化后的s盒

接下来就是**伪随机子密码生成算法（PRGA)、加密阶段**

这个阶段的目标就是生成与明文等长的密钥流，用于异或加密  

```python
i=j=0
for i in range(256)
i = (i + 1) % 256
j = (j + S[i]) % 256
S[i], S[j] = S[j], S[i]  # 交换
t = (S[i] + S[j]) % 256
key_byte = S[t]
```

然后就是使用这个密钥流和明文进行异或得到密文，题目里是变量特别相似，非常容易弄混，让人看不下去。

知道这些我们就可以来解题了

首先，我们知道flag的最后一个字符肯定是'}',而且我们又知道密文的最后一位是'~'，那么我们就可以得到最后一个密钥也就是s[t],也就可以知道t，又因为最后一个i的值也就是明文/密文的长度，根据**t = (s[i] + s[j])%256**就可以知道最后一个sj，sj知道后就能得到j吗，然后根据**j = (j + s[i])%256**也就能得到上一个j，i的值，进而确定t，s[t],也就能得到每一个s[t],和密文异或后就能得到明文，把明文反转就是flag

完整脚本：

```python
s = [166, 163, 208, 147, 181, 152, 69, 90, 253, 114, 150, 255, 218, 220, 34, 74, 63, 201, 70, 115, 233, 96, 43, 169, 103, 191, 14, 149, 143, 25, 105, 93, 199, 246, 51, 75, 20, 5, 107, 3, 52, 135, 111, 139, 113, 47, 184, 76, 161, 174, 23, 30, 173, 72, 198, 56, 85, 55, 106, 126, 244, 223, 104, 29, 112, 148, 219, 118, 165, 59, 98, 175, 125, 100, 17, 16, 108, 6, 214, 140, 130, 206, 89, 62, 4, 236, 251, 91, 28, 45, 18, 53, 1, 144, 193, 133, 73, 44, 57, 88, 250, 204, 177, 67, 120, 21, 79, 71, 2, 185, 211, 200, 65, 234, 117, 9, 226, 142, 230, 209, 132, 248, 242, 196, 101, 81, 238, 247, 119, 179, 131, 229, 94, 50, 22, 183, 24, 158, 190, 222, 155, 172, 19, 12, 225, 10, 189, 232, 146, 227, 212, 210, 31, 164, 138, 78, 122, 176, 121, 0, 194, 186, 162, 160, 188, 168, 216, 153, 37, 252, 127, 145, 33, 82, 58, 170, 61, 254, 136, 207, 8, 237, 159, 36, 239, 95, 178, 167, 182, 102, 27, 123, 60, 129, 42, 26, 99, 39, 97, 40, 235, 205, 7, 49, 202, 197, 109, 195, 245, 171, 180, 15, 46, 83, 48, 156, 92, 249, 38, 32, 203, 41, 124, 54, 217, 134, 154, 35, 157, 128, 137, 221, 84, 86, 215, 213, 80, 11, 116, 141, 241, 64, 224, 87, 77, 187, 68, 192, 151, 240, 13, 228, 231, 66, 110, 243]
a = b"]7\xab\xc9\xd8\x90\x1f\xd2OP\xad\x87'0\xe1\xff=~"

t=s.index(ord('}')^ord('~'))
i=len(a) #i的值即为明文/密文长度
sj=(t-s[i])%256 ##求出最后一个sj
j=s.index(sj) #找出sj对应的索引
s[i],s[j]=s[j],s[i]#交换s[i]和s[j]
j=(j-s[i])%256 #求出上一个j
i=i-1#上一个i
t=(s[i]+s[j])%256 #确定t,进而确定s[t]
flag='}'
while i>0:
    flag+=(chr((a[i-1])^s[t]))
    s[i], s[j] = s[j], s[i]
    j = (j - s[i]) % 256
    i = i - 1
    t = (s[i] + s[j]) % 256
flag=flag[::-1]
print(flag)
```

