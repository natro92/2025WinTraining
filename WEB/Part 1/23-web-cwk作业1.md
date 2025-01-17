# 课后作业

## Ctfhub-RCE模块完成

![](./assets/xibianshengqidetaiyang作业1/1.png)

### RCE-labs[1]

看页面代码发现flag,直接写

![](./assets/xibianshengqidetaiyang作业1/3.png)

### RCE-labs[2]

分析代码输入a值

![](./assets/xibianshengqidetaiyang作业1/4.png)

 直接../看文件发现flag直接cat

![](./assets/xibianshengqidetaiyang作业1/5.png)

### RCE-labs[3]

分析代码输入action值，获得新函数

![](./assets/xibianshengqidetaiyang作业1/6.png)

使用新函数传参，得到flag

![](./assets/xibianshengqidetaiyang作业1/7.png)

### RCE-labs[4]

分析代码传参，看到flag

![](./assets/xibianshengqidetaiyang作业1/8.png)

cat看flag

![](./assets/xibianshengqidetaiyang作业1/9.png)

### RCE-labs[5]

看代码直接传ip循环加通道符加指令

![](./assets/xibianshengqidetaiyang作业1/10.png)

看到flag文件直接cat

![](./assets/xibianshengqidetaiyang作业1/11.png)

### RCE-labs[5]

分析代码输入cmd值，发现flag

![](./assets/xibianshengqidetaiyang作业1/12.png)

但是flag被过滤直接加‘ ，cat文件得到flag

![](./assets/xibianshengqidetaiyang作业1/13.png)

### RCE-labs[6]

分析代码发现大部分被过滤发现开始使用8进制，ls命令发现flag

![](./assets/xibianshengqidetaiyang作业1/14.png)

再次8进制cat文件

![](./assets/xibianshengqidetaiyang作业1/15.png)

### RCE-labs[7]

分析代码发现空格被过滤使用${IFS}替代输入命令

![](./assets/xibianshengqidetaiyang作业1/16.png)

cat flag

![](./assets/xibianshengqidetaiyang作业1/17.png)

### RCE-labs[8]

重新规定一个朝向，并输出
![](./assets/xibianshengqidetaiyang作业1/18.png)

## linux的搭建和学习

![](./assets/xibianshengqidetaiyang作业1/linux1.HEIC)

![](./assets/xibianshengqidetaiyang作业1/linux2.HEIC)

## PHP的学习

![](./assets/xibianshengqidetaiyang作业1/php1.HEIC)

![](./assets/xibianshengqidetaiyang作业1/php2.HEIC)

![](./assets/xibianshengqidetaiyang作业1/php3.HEIC)

![](./assets/xibianshengqidetaiyang作业1/php4.HEIC)

![](./assets/xibianshengqidetaiyang作业1/php5.HEIC)
