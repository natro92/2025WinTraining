1.rsa用python代码实现

```python
# 求两个数的最大公约数（欧几里得算法）
def gcd(a, b):
   if b == 0:
       return a
   else:
       return gcd(b, a % b)

# 获取公钥和私钥
def get_key(p, q):
   n = p * q
   phi= (p - 1) * (q - 1)
   e = 2
   while gcd(e, phi) != 1:
       e = e + 1#获得最小的与phi互质的e
   d = 2
   while (e*d) % phi != 1:
       d = d + 1#找到满足条件的d
   return (n, e), (n, d)

# 加密
def encryption(x, pubkey):
   n = pubkey[0]
   e = pubkey[1]
   y = x ** e % n   # 加密
   return y

# 解密
def decryption(y, prikey):
   n = prikey[0]
   d = prikey[1]
   x = y ** d % n      # 解密
   return x


if __name__ == '__main__':
   p = int(input("请输入第一个质数p："))
   q = int(input("请输入第二个质数q："))
   x = int(input("请输入要加密的消息x："))
   # 生成公钥私钥
   pubkey, prikey = get_key(p, q)
   print("加密前的消息是：", x)
   y = encryption(x, pubkey)
   print("加密后的消息是：", y)
   after_x = decryption(y, prikey)
   print("解密后的消息是：", after_x)
```

```python
import random
from math import gcd

# 生成大素数
def generate_large_prime(bits):
    while True:
        num = random.getrandbits(bits)
        if is_prime(num):
            return num

def is_prime(n, k=128):
    if n <= 1:
        return False
    if n <= 3:
        return True
    if n % 2 == 0 or n % 3 == 0:
        return False
    r, s = 0, n - 1
    while s % 2 == 0:
        r += 1
        s //= 2
    for _ in range(k):
        a = random.randrange(2, n - 1)
        x = pow(a, s, n)
        if x == 1 or x == n - 1:
            continue
        for _ in range(r - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break
        else:
            return False
    return True


# 计算最大公约数
def gcd(a, b):
    while b != 0:
        a, b = b, a % b
    return a

# 扩展欧几里得算法求逆元
def modinv(a, m):
    m0, x0, x1 = m, 0, 1
    if m == 1:
        return 0
    while a > 1:
        q = a // m
        m, a = a % m, m
        x0, x1 = x1 - q * x0, x0
    if x1 < 0:
        x1 += m0
    return x1

# 生成RSA密钥对
def generate_keypair(bits):
    p = generate_large_prime(bits)
    q = generate_large_prime(bits)
    n = p * q
    phi = (p - 1) * (q - 1)
    e = random.randrange(2, phi)
    while gcd(e, phi) != 1:
        e = random.randrange(2, phi)
    d = modinv(e, phi)
    return ((e, n), (d, n))

# 加密消息
def encrypt(pk, plaintext):
    key, n = pk
    cipher = [pow(ord(char), key, n) for char in plaintext]
    return cipher

# 解密消息
def decrypt(pk, ciphertext):
    key, n = pk
    plain = [chr(pow(char, key, n)) for char in ciphertext]
    return ''.join(plain)


if __name__ == '__main__':
    print("Generating your public/private keypairs now...")
    public, private = generate_keypair(1024)
    print("Your public key is ", public)
    print("Your private key is ", private)

    message = input("请输入要加密的内容")
    print("Original message:", message)
    encrypted_msg = encrypt(public, message)
    print("Encrypted message:", encrypted_msg)
    decrypted_msg = decrypt(private, encrypted_msg)
    print("Decrypted message:", decrypted_msg)

```

**2.rabin算法学习**

 Rabin算法是RSA的算法一个特例，e取固定值2  

其中，取两个大素数p，q，满足 p≡q≡3mod4  ，n=p*q，n为公钥，p，q为私钥

加密 c≡m**2mod n，也就是m![image](https://cdn.nlark.com/yuque/__latex/84c2bded7c3cb91f4dd4825288c6f21a.svg)≡c mod n

根据中国剩余定理又可以得到

m![image](https://cdn.nlark.com/yuque/__latex/84c2bded7c3cb91f4dd4825288c6f21a.svg)≡c mod p

m![image](https://cdn.nlark.com/yuque/__latex/84c2bded7c3cb91f4dd4825288c6f21a.svg)≡c mod q  

![](https://cdn.nlark.com/yuque/0/2025/png/42557099/1737706172770-53efbb67-a552-49d7-82a9-a453a953c251.png)

具体脚本：

```python
import gmpy2
import libnum
 
p = 13934102561950901579
q = 14450452739004884887
n = 201354090531918389422241515534761536573
c = 20442989381348880630046435751193745753
e = 2
inv_p = gmpy2.invert(p, q)#p在模q下的逆元
inv_q = gmpy2.invert(q, p)#q在模p下的逆元
mp = pow(c, (p + 1) // 4, p)
mq = pow(c, (q + 1) // 4, q)
a = (inv_p * p * mq + inv_q * q * mp) % n
b = n - int(a)
c = (inv_p * p * mq - inv_q * q * mp) % n
d = n - int(c)#中国剩余定理
# 因为rabin 加密有四种结果，全部列出。
aa = [a, b, c, d]
 
for i in aa:
    print(i)
    print(libnum.n2s(int(i)))
```

**3.rsa共模攻击**

 共模是指：就是明文m,相同。用两个公钥e1,e2加密得到两个私钥d1,d2 和两个密文c1,c2  
**共模攻击**，即当m不变的情况下，知道n,e1,e2,c1,c2, 在不知道d1,d2的情况下，解出m  

已知有密文：

c1 = pow(m, e1, n)

c2 = pow(m, e2, n)

条件：

当e1，e2互质，则有gcd(e1,e2)=1

根据扩展欧几里德算法，对于不完全为 0 的整数 a，b，gcd（a，b）表示 a，b 的最大公约数。那么一定存在整数 x，y 使得 gcd（a，b）=ax+by

所以得到：

e1*s1+e2*s2 = 1

因为e1和e2为正整数，所以s1、s2皆为整数，但一正一负，此时假设s1为正数,s2为负数

脚本：

```python
import gmpy2
from Crypto.Util.number import *

e1= 3247473589
e2= 3698409173
c1 = 100156221476910922393504870369139942732039899485715044553913743347065883159136513788649486841774544271396690778274591792200052614669235485675534653358596366535073802301361391007325520975043321423979924560272762579823233787671688669418622502663507796640233829689484044539829008058686075845762979657345727814280
c2 = 86203582128388484129915298832227259690596162850520078142152482846864345432564143608324463705492416009896246993950991615005717737886323630334871790740288140033046061512799892371429864110237909925611745163785768204802056985016447086450491884472899152778839120484475953828199840871689380584162839244393022471075
n = 103606706829811720151309965777670519601112877713318435398103278099344725459597221064867089950867125892545997503531556048610968847926307322033117328614701432100084574953706259773711412853364463950703468142791390129671097834871371125741564434710151190962389213898270025272913761067078391308880995594218009110313

g,x,y = gmpy2.gcdext(e1, e2)     由扩展欧几里得算法可得，xe1 + ye2 = 1 解出x , y
print(x,y)
m = pow(c1, x , n) * pow(c2, y, n) % n 
print(long_to_bytes(m))
```

数学推导：![](https://cdn.nlark.com/yuque/0/2025/jpeg/42557099/1737704052608-91acf35a-1fd1-4ebd-aa26-d7786da93c3f.jpeg)

