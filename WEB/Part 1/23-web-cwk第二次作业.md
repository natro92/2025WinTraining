# 课后作业2

### 第一题

分析代码，使file=hint.php

![](./assets/xibianshengqidetaiyang作业2/1.png)

看到flag位置

![](./assets/xibianshengqidetaiyang作业2/2.png)

查看文件得到flag

![](./assets/xibianshengqidetaiyang作业2/3.png)

### 第二题

分析代码

![](./assets/xibianshengqidetaiyang作业2/4.png)

 构造代码得到值

![](./assets/xibianshengqidetaiyang作业2/5.png)

带入值得到flag

![](./assets/xibianshengqidetaiyang作业2/6.png)

![](./assets/xibianshengqidetaiyang作业2/7.png)

### 第三题

分析代码传参，看到提示查看得到flag位置

![](./assets/xibianshengqidetaiyang作业2/8.png)

分析代码，构造带入值，其中因为函数PHP等的时候为空 所以吞噬后边的 所以可以构造逃逸

![](./assets/xibianshengqidetaiyang作业2/9.png)

![](./assets/xibianshengqidetaiyang作业2/10.png)

查看源代码看到flag位置

![](./assets/xibianshengqidetaiyang作业2/11.png)

把flag位置文件名64编码并修改逃逸得到flag值

![](./assets/xibianshengqidetaiyang作业2/12.png)

### 第四题

分析代码构造函数

![](./assets/xibianshengqidetaiyang作业2/13.png)


发现可以运行到pkshow函数继续构造

![](./assets/xibianshengqidetaiyang作业2/14.png)

可以运行到ace函数

![](./assets/xibianshengqidetaiyang作业2/15.png)

完整构造发现flag位置

![](./assets/xibianshengqidetaiyang作业2/16.png)

把filename改为flag位置的文件名 输入输出结果得到flag

![](./assets/xibianshengqidetaiyang作业2/17.png)


## 做题和学习笔记

![](./assets/xibianshengqidetaiyang作业2/1.HEIC)

![](./assets/xibianshengqidetaiyang作业2/2.HEIC)

![](./assets/xibianshengqidetaiyang作业3/3.HEIC)
