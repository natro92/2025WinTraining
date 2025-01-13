<h1 id="Ez1kQ"><font style="color:#000000;">一、PHP语言基础</font></h1>
<font style="color:#000000;">由于之前写过PHP笔记，这里的PHP知识作为补充</font>

<h2 id="JNFpO"><font style="color:#000000;">（一）php基础1</font></h2>
知识链接：[https://github.com/natro92/HnuSec-Training-Website/blob/main/docs/file/PHP1.pdf](https://github.com/natro92/HnuSec-Training-Website/blob/main/docs/file/PHP1.pdf)

+ <font style="color:#000000;">通过函数修改全局变量要在变量前加上global</font>
+ <font style="color:#000000;">常量名不加$修饰符</font>
+ <font style="color:#000000;">常量定义：</font>
    - <font style="color:#000000;">bool </font><font style="color:#000000;">define </font><font style="color:#000000;">( </font><font style="color:#000000;">string </font><font style="color:#000000;">$name </font><font style="color:#000000;">, </font><font style="color:#000000;">mixed </font><font style="color:#000000;">$value </font><font style="color:#000000;">[, </font><font style="color:#000000;">bool </font><font style="color:#000000;">$case_insensitive </font><font style="color:#000000;">= </font><font style="color:#000000;">false </font><font style="color:#000000;">] ) </font>
    - <font style="color:#000000;">name：必选参数，常量名称，即标志符。 </font>
    - <font style="color:#000000;">value：必选参数，常量的值。 </font>
    - <font style="color:#000000;">case_insensitive：可选参数，如果设置为 </font><font style="color:#000000;">TRUE</font><font style="color:#000000;">，该常量则大小写不敏感，默认是大小写敏感的</font>
+ <font style="color:#000000;">GLOBALS： </font>
    - <font style="color:#000000;">$GLOBALS 是PHP的一个超级全局变量组，在一个PHP脚本的全部作用域中都可以访问。 </font>
    - <font style="color:#000000;">$GLOBALS 是一个包含了全部变量的全局组合数组。变量的名字就是数组的键。</font>
+ <font style="color:#000000;">$_SERVER：</font><font style="color:#000000;"> </font>
    - <font style="color:#000000;">1$_SERVER 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目由 Web 服务器创建</font>
+ <font style="color:#000000;">$_POST： </font>

<font style="color:#000000;">$_POST 来收集表单中的 </font><font style="color:#000000;">input </font><font style="color:#000000;">字段数据</font>

+ <font style="color:#000000;">extract： </font>
    - <font style="color:#000000;">extract </font><font style="color:#000000;">函数可以将数组中的元素导入到当前作用域中作为变量。如果数组中的键与当前作用域中已有的变量名相同，可能会导致变量覆盖。</font>
+ <font style="color:#000000;">list</font><font style="color:#000000;">： </font>
    - <font style="color:#000000;">list() </font><font style="color:#000000;">函数用于将数组中的值赋给一组变量</font>
+ <font style="color:#000000;">parse_str</font><font style="color:#000000;">： </font>
    - <font style="color:#000000;">parse_str() </font><font style="color:#000000;">函数用于解析 URL 查询字符串，并将其中的参数赋值给相应的变量</font>
+ <font style="color:#000000;">compact</font><font style="color:#000000;">： </font>
    - <font style="color:#000000;">compact() </font><font style="color:#000000;">函数用于将多个变量转换为关联数组，其中变量名将成为数组的键，变量的值将成为数组的值</font>
+ <font style="color:#000000;">var_dump： </font>
    - <font style="color:#000000;"></font><font style="color:#000000;">var_dump是一个调试函数，用于输出变量的详细信息，包括类型和值。 </font>
    - <font style="color:#000000;"></font><font style="color:#000000;">它可以用于调试目的，帮助开发者了解变量的结构和内容</font>
+ <font style="color:#000000;">basename() 函数用于返回路径中的文件名部分。它可以用于确保文件名只包含合法的字符，并且不包含路径信息。</font>
+ <font style="color:#000000;">preg_replace():</font>
    - <font style="color:#000000;">preg_replace() 函数可以使用正则表达式替换文件名中的不合法字符。通过使用适当的正则表达式，可以从文件名中去除非法字符或将它们替换为其他字符。</font>
+ <font style="color:#000000;"></font><font style="color:#000000;">str_replace(): </font>
    - <font style="color:#000000;">str_replace() 函数可以用于简单的字符串替换。虽然不如正则表达式强大，但在某些情况下也可以用来去除特定字符</font>
+ <font style="color:#000000;">filter_var(): </font>
    - <font style="color:#000000;">filter_var() 函数可以用于过滤文件名中的非法字符。使用FILTER_SANITIZE_STRING 过滤器可以删除文件名中的非法字符。</font>
+ <font style="color:#000000;"></font><font style="color:#000000;">pathinfo() ： </font>
    - <font style="color:#000000;">pathinfo() 函数可以用于获取文件路径的信息，包括文件名、目录名和文件扩展名（后缀）等。通过 </font>
    - <font style="color:#000000;">pathinfo() 获取文件后缀，并与允许的后缀列表进行比较。</font>
+ <font style="color:#000000;">strtolower() ： </font>
    - <font style="color:#000000;">在检查文件后缀时，为了避免大小写问题，通常会将后缀转换为小写（或大写）后再进行比较。</font>
+ include()
    - <font style="color:rgb(17, 17, 17);">将外部文件引入到当前的 PHP脚本中</font>

<h2 id="FMVTQ"><font style="color:#000000;">（二）php基础2</font></h2>
芝士~链接：[https://github.com/natro92/HnuSec-Training-Website/blob/main/docs/file/PHP2.pdf](https://github.com/natro92/HnuSec-Training-Website/blob/main/docs/file/PHP2.pdf)

+ 类与对象等参考C语言
+ <font style="color:rgb(65,70,75);">__construct() </font>
    - <font style="color:rgb(65,70,75);">实例化类时自动调用 </font>
+ <font style="color:rgb(78,90,112);"></font><font style="color:rgb(65,70,75);">__destruct() </font>
    - <font style="color:rgb(78,90,112);"></font><font style="color:rgb(65,70,75);">类对象使用结束时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__set() </font>
    - <font style="color:rgb(65,70,75);">在给未定义的属性赋值时自动调用 </font><font style="color:rgb(78,90,112);"> </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__get() </font>
    - <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">调用未定义的属性时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__isset() </font>
    - <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">使用 isset() 或 empty() 函数时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__unset() </font>
    - <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">使用 unset() 时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__sleep() </font>
    - <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">使用 serialize 序列化时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__wakeup() </font>
    - <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">使用 unserialize 反序列化时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__call() </font>
    - <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">调用一个不存在的方法时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__callStatic() </font>
    - <font style="color:rgb(78,90,112);"></font><font style="color:rgb(65,70,75);">调用一个不存在的静态方法时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__toString() </font>
    - <font style="color:rgb(78,90,112);"></font><font style="color:rgb(65,70,75);">把对象转换成字符串时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__invoke() </font>
    - <font style="color:rgb(65,70,75);">当尝试把对象当方法调用时自动调用 </font>
+ <font style="color:rgb(78,90,112);"></font><font style="color:rgb(65,70,75);">__set_state() </font>
    - <font style="color:rgb(78,90,112);"></font><font style="color:rgb(65,70,75);">当使用 var_export() 函数时自动调用，接受一个数组参数 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__clone() </font>
    - <font style="color:rgb(65,70,75);">当使用 clone 复制一个对象时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__debugInfo() </font>
    - <font style="color:rgb(78,90,112);"></font><font style="color:rgb(65,70,75);">使用 var_dump() 打印对象信息时自动调用 </font>
+ <font style="color:rgb(78,90,112);"> </font><font style="color:rgb(65,70,75);">__autoload() </font>
    - <font style="color:rgb(78,90,112);"></font><font style="color:rgb(65,70,75);">尝试加载未定义的类 </font>

<h1 id="zCbbR"><font style="color:#000000;">二、写题</font></h1>
<h2 id="dUtaa"><font style="color:#000000;">（一）[极客大挑战 2019]BabySQL</font></h2>
<font style="color:#000000;">先尝试万能密码</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736519153630-3f9a91f6-5930-432d-843d-9c9180015499.png)

<font style="color:#000000;">不出所料，不是那道签到题，96dfc323c6b7d07e82ed08e6d01a894a试了用任何方法都解不开，都是乱码，万能密码用不了</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736519454073-4830fa0b-c9f3-425b-a36c-9bd766d4f375.png)

<font style="color:#000000;">这个万能密码方法只能放弃，毕竟这道题考的是sql注入，不可能这么简单</font>

<font style="color:#000000;">尝试随便输入几个</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736520392251-21c4c17d-fe43-46b9-8a79-3a867c9c6034.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736520505981-6ed42089-1ca3-4b02-ab1c-25d7387bfe5c.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736520608655-72edbb33-ca11-428d-bafa-647480e19912.png)

<font style="color:#000000;">结果都是这个</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736520392150-58da92eb-8bc6-4bbc-b908-ed8dcfe1de5d.png)

<font style="color:#000000;">到输入这个的时候出现了下面的报错信息，至少让我们知道了sql的闭合条件'1'''，去掉左右两边的单引号得到1''，再去掉1',得到闭合体条件就是一个单引号'</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736520524012-d7fb9d7e-c2bd-4d66-a0ad-f99b1764dd5c.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736520534822-8a7e9851-84e7-48f3-a970-2b4012c4d20d.png)

<font style="color:#000000;">经过测试，它把and省略掉了，说明这个网站会过滤掉某些字符</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736521103444-0e2e263c-056b-4a10-9954-72bf7e94432d.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736521277904-65d84254-63bd-425d-966a-934bc60964a4.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736521205065-c77c84b8-a7b3-480a-99e9-5776a5e7c6ca.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736521114846-b0a75543-dd8a-48cf-b2b2-ca60d4b56eed.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736521304292-3bee407e-af8f-4c53-9396-e57bf7bdb0ab.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736521220484-446bfc28-0ea6-40a5-b748-9c88b02985f8.png)

<font style="color:#000000;">试了一下，还把or过滤掉了</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736567900542-4a4d10ff-c5a6-4de3-abd4-fd43ca40c705.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736567932240-cc1ff621-6f17-4b6b-8258-2be8bbe9b59b.png)

<font style="color:#000000;">尝试用一下burp看一下还有那些字符被过滤了</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736570755954-87553241-8e6b-4ea9-9749-cfc715cb6b72.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736570742161-55d79597-14f5-459f-a314-5c7bb6dc3a19.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736570732309-014ac547-ae41-48d9-b00f-28fda3811180.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736570728596-5f58e1bb-312d-49aa-8f00-b4830df71556.png)<font style="color:#000000;">and、And、AND、ANd、by、BY、or、Or、select、SELECT、insert、insERT、INSERT、where、+、sleep、SLEEp、union、UNION、drop、DROP、mid、from、count……等字符被过滤了，且字符不分大小写但是啊，双写如anandd、selselectect、ununionion、whwhereere等就绕过去了，</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736571025057-d8e7e060-5e5f-485f-8d99-aeeb43b0d5db.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736571026890-59355c8d-c555-43e4-bcd4-58757470bf55.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736571090148-63ec76b1-357e-4277-a39a-9d1cb7976abf.png)

<font style="color:#000000;">接下来看一下,要判断字位数</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736571733360-af3bdb3b-44af-4820-964f-f6aecc6b903c.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736571741059-cf7f7486-c194-4765-9e75-f94acdea1077.png)

<font style="color:#000000;">4出现unknown，和之前显示的不一样，说明字段数为3</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736571769135-9ab9c1f0-8841-4c7e-87ac-3c58c6409f96.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736571776630-95fdea75-c3cc-44ff-966f-0a07e7e2cf17.png)

<font style="color:#000000;">使用联合注入</font>

<font style="color:#000000;">用户名：1'ununionion seselectlect 1,2,3#       密码是：1</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736572113901-36411377-8a30-4941-8ee8-905bc02edbe5.png)

<font style="color:#000000;">成功了，之后就是找他的数据库，发现2和3这两个位置可以注入，回显点位是2，3</font>

<font style="color:#000000;">接着找它的数据库，还是联合注入</font>

<font style="color:#000000;">用户名：1'ununionion seselectlect 1,database(),3#   密码是：1</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736572258483-0c5b5411-32b5-4219-9ffc-5b2c5c2c0039.png)

<font style="color:#000000;">geek应该就是数据库名，接下来就是找表名了，还是联合注入，from和information中的or要双写</font>

<font style="color:#000000;">用户名为1' ununionion seselectlect 1,2,group_concat(table_name) frfromom infoorrmation_schema.tables whwhereere table_schema='geek'#</font>

<font style="color:#000000;">密码为1</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736590234712-160235b2-a75f-492c-b189-bf1afc6962d5.png)

<font style="color:#000000;">所以就找到表了，表名为：b4bsql,geekuser</font>

<font style="color:#000000;">接着就是查询表中的内容了，还是联合注入 </font>

<font style="color:#000000;">用户名：1' uniunionon selselectect 1,2,group_concat(column_name) frfromom infoorrmation_schema.columns whwhereere table_name='b4bsql'# </font>

<font style="color:#000000;">密码：1</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736593552367-1e0e30ff-88ce-49ec-9d4e-d609bf518bd6.png)

<font style="color:#000000;">得到该表中的字段信息，查询username和password的内容，注意password中间or需要双写</font>

<font style="color:#000000;">用户名：1' uniunionon selselectect 1,group_concat(username),group_concat(passwoorrd) frfromom geek.b4bsql #</font>

<font style="color:#000000;">密码：1</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736594191337-c114b125-5926-4d37-ac5b-fda45e0a78b5.png)

<font style="color:#000000;">拿到flag，结束游戏</font>

<h2 id="y6Muo"><font style="color:#000000;">（二）[SWPUCTF 2021 新生赛]finalrce</font></h2>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736597845911-dd49e667-6fd9-4238-a911-164066019025.png)

<font style="color:#000000;">题目是一段PHP代码，需要绕过正则表达式。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736597837872-ce7eb4e7-b15e-4013-ad39-3ab3560043b2.png)<font style="color:#000000;">cat命令最常见的用法查看文本文件的内容。例如，查看一个名为 file.txt的文件，可以运行以下命令：</font>

<font style="color:#000000;">+cat file.txt</font>

<font style="color:#000000;">先看if里的内容，preg_match 函数用于执行一个正则表达式匹配，末尾的/i说明</font><font style="color:#000000;"> 匹配不分大小写。读了代码后发现反斜杠和单引号都没有被注释掉，那么我们就可以利用这一点来绕过被禁止掉的命令当正则表达式没有过滤掉反斜杠或者单引号或者双引号，那么可以使用单引号或者双引号包裹命令（如：ls cat等）任意一个字符，只要他们不是一个整体就行，或者使用反斜杠插入到命令里面（如：ca\t  c\at等）即可实现绕过可以看到的是没有过滤”|“这个符号，然后exec执行是没有</font><font style="color:#000000;">回显</font><font style="color:#000000;">的.</font>

<font style="color:#000000;">这个题目是需要用linux的一个命令，”tee“将想要执行的命令写入到一个文件里面，然后再去访问这个文件，以此来执行这个命令</font><font style="color:#000000;">，简单来说，就是</font><font style="color:#000000;">从标准输入读取，再写入标准输出和文件。</font>

<font style="color:#000000;">所以：tee 1.txt：将输出复制一份并输出到终端窗口以及写入到名为1.txt的文件中</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736599306986-1873fd03-6d89-4152-9ca0-aaa34e757fc6.png)

<font style="color:#000000;">输出重定向之后，看到有flag</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736599882646-79831275-e2c2-446f-9877-14b9a24d6972.png)

<font style="color:#000000;">之后使用类似的命令，将flllllaaaaaaggggggg文件中的内容进行查询，</font><font style="color:#000000;">然后访问2.txt就可以拿到flag了</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736600117584-aceb6f22-110a-46ee-9f7d-06bc76a24846.png)

<h2 id="JPgCw"><font style="color:#000000;">（三）[SWPUCTF 2021 新生赛]babyrce</font></h2>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736677463696-0a3fec3d-3233-4ede-b18a-6701e83d99d7.png)

<font style="color:#000000;">进来之后发现有一串PHP代码</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736677471281-cc005a30-3a75-4bdf-b6c4-bd85e94edc59.png)

<font style="color:#000000;">header查了资料</font><font style="color:#000000;">header() 函数向客户端发送原始的 HTTP 报头。</font>

<font style="color:#000000;">在这里的header（）的含义是：</font><font style="color:#000000;">header("Content-Type:text/html;charset=utf-8");` 设置内容类型为 HTML ，并指定字符类型为 UTF-8。</font>

<font style="color:#000000;">接着看代码</font>

<font style="color:#000000;"> 如果'admin' 的 cookie 值是否为1。如果是，它将包含（include）"../next.php" 文件，所以把请求中Cookie的值设置为1就可以拿到flag了</font>

<font style="color:#000000;">还有，小饼干(Small Cookies)，直接burp你</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736677880412-e08481dd-b47b-4ffa-a2fc-b2151d34ad25.png)

<font style="color:#000000;">设置admin的cookie值为1</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736678140585-a5abd1e1-c69d-4974-80d1-4aa09bdb10c3.png)

<font style="color:#000000;">出现了一个rasalghul.php文件</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736678203998-f7a4fa6d-da2e-4aa3-8e32-8b9b77771715.png)

<font style="color:#000000;">访问一下又来了一串PHP代码，显然可见，这个PHP代码过滤掉了空格 </font>

<font style="color:#000000;">shell_exec($ip); 的作用是将用户输入直接传递给shell_exec()，执行系统命令，并将输出返回给用户。  </font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736678266831-d9b81fc2-d3a2-4b7c-b008-08427a812d61.png)

<font style="color:#000000;">先用</font><font style="color:#000000;">Linux ls命令试试，输出当前目录下的文件列表  </font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736678408604-54185343-7288-4d2f-96e0-8a95231022ea.png)

<font style="color:#000000;">试了下打开index.php文件，又回到开始了。</font>

<font style="color:#000000;"></font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736679462086-cc038d95-6155-4148-be06-75a648a1c23f.png)

<font style="color:#000000;">可以看到，ls命令是有作用的，</font><font style="color:#000000;">我们可以看到很显眼的flag文件，,</font><font style="color:#000000;">所以我们只要能绕过空格限制，</font><font style="color:#000000;">使用cat命令查看,</font><font style="color:#000000;">就能够进行下一步了</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736678755188-25b63fdc-95a2-4405-b71d-f3f8dfdf6241.png)

<font style="color:#000000;">查了资料，绕过空格的常用的方法为： </font>

<font style="color:#000000;">cat flag.txt </font>

<font style="color:#000000;">cat${IFS}flag.txt </font>

<font style="color:#000000;">cat$IFS$9flag.txt </font>

<font style="color:#000000;">cat<flag.txt</font>

<font style="color:#000000;">cat<>flag.txt</font>

<font style="color:#000000;">所以传入?url=cat${IFS}/flllllaaaaaaggggggg得到flag</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736679016602-55b270e4-d284-462d-82be-bfaf809c448d.png)

<h2 id="dbv7v"><font style="color:#000000;">（四）[ACTF2020 新生赛]Exec</font></h2>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736600651502-1dc7ff11-6cde-4def-aaec-df98566db4e3.png)

<font style="color:#000000;">先尝试ping自己，没有flag，</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736600922279-794f0a33-4595-4c53-898c-fb15629a083d.png)

<font style="color:#000000;">随便试了一下错的，丢包率100%，hhhhhh</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736601054735-9b580b60-22dc-47d1-b46e-fdb9b33f7fd5.png)

<font style="color:#000000;">试试看是否过滤了分号：127.0.0.1;ls;</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736601142389-bb0bd8ea-4da3-436f-8bf9-f8aa615a508a.png)

<font style="color:#000000;">发现没有过滤分号，但是呢，相比传入127.0.0.1，多了一个文件index.php，所以尝试切换到上一级目录看一下</font><font style="color:#000000;">接着我们就</font><font style="color:#000000;">遍历</font><font style="color:#000000;">目录了,通过cd …/达到访问上一个目录的目的，再通过ls 达到查看该目录有哪些文件</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736601320447-7b5f977b-f5bc-4a0d-8645-1d977b20f1a8.png)

<font style="color:#000000;">没有什么信息，在往上看</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736601397802-7559a93a-9a6d-4bfe-9cc6-2918882a265e.png)

~~<font style="color:#000000;">没有什么信息，在往上看 </font>~~<font style="color:#000000;"> 还往上看神马，flag不久在这里？抓住它不久行了吗？</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736601426809-1f75dd15-ea2e-4f32-8ba7-15e502c97688.png)

<font style="color:#000000;">找到flag：flag{9002c8ac-2096-4a00-b0a7-99b6d4c71fab}</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736601700499-2f2662cf-d7ed-42c6-b0e1-6138829ac0dc.png)

<h2 id="J4csD"><font style="color:#000000;">（五）[SWPUCTF 2022 新生赛]webdog1__start</font></h2>
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736604220579-3227a982-36ea-41d7-84a2-34742e380d36.png)

<font style="color:#000000;">一进去灵魂提问，好好好</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736604234982-d64e3451-66a6-4876-b5fc-3e1afe8d2915.png)

<font style="color:#000000;">查找网页源代码，发现这一段PHP代码，这个考的应该是md5绕过</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736604429725-619e999d-87e6-4b9a-8f38-0e1017e069c0.png)

<font style="color:#000000;">想着用之前写题的思路，使用数组绕过，下面有一行小字发生了变化，拷问我How do you think?应该是错了，</font>

<font style="color:#000000;">MD5之后的数组返回值是</font><font style="color:#000000;">null</font><font style="color:#000000;">，肯定不等于</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736604613851-0b8949e7-559f-4f1e-b14a-9174d108402d.png)

<font style="color:#000000;">变量等于md5的变量这里的思路是让他们都为0e来触发0e计数法接着相等</font>

<font style="color:#000000;">查了资料，自</font><font style="color:#000000;">身与md5相等</font><font style="color:#000000;">对于</font><font style="color:#000000;">0e215962017</font><font style="color:#000000;">，md5后也是以0e开头，可以尝试用它绕过</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736605472358-1d64ea8b-ed44-470f-a72b-df6d83109517.png)

<font style="color:#000000;">看，成功了，出现一下的东西：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736605450990-344208fa-af2d-4f35-8abd-ca6f5e690f9c.png)

<font style="color:#000000;">这里出现了很多链接，除了第一个链接是一篇文章，其余都是404</font>

<font style="color:#000000;">"Next"do you know the power of bot? go go go!!</font>

<font style="color:#000000;">“下一个”你知道机器人的力量吗？去吧，去吧！！</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736606576471-7f33ee31-d1f6-4a0b-827b-272963376f55.png)

<font style="color:#000000;">去robots.txt的路径看一看，发现是一堆乱码</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736606663833-1e12b57c-6489-4d2c-af22-0e53360cce27.png)

<font style="color:#000000;">乱码中夹杂着f14g.php</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736606776525-945c5e85-1fd9-496c-a2cf-4cdea7a1d98c.png)

<font style="color:#000000;">有一个网址，看看</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736606809767-104bef09-9bc6-48ba-8e22-3aa891a94f0d.png)

<font style="color:#000000;">好好好，弔图一张</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736606835633-5221608c-ff45-4729-a75e-4688520c2561.png)

<font style="color:#000000;">那别怪我burp你了</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736607267463-e1dc1a6b-ad17-44ff-bd9f-c5e7e6be72b7.png)

<font style="color:#000000;">找到F1l1l1l1l1lag.php文件，打开看一下还是PHP代码</font><font style="color:#000000;">是一段代码审计</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736607257872-f5efcdd0-f063-4a66-9d9c-c79ebd462700.png)

<font style="color:#000000;">这里主要限制是过滤了flag和空格以及有18个字符的限制 </font>

<font style="color:#000000;">if(!strstr($get," "))</font>

<font style="color:#000000;">检查 $get 中是否包含空格，如果包含，则直接拒绝。  </font>

<font style="color:#000000;">$get = str_ireplace("flag", " ", $get);</font>

<font style="color:#000000;"> 把输入中的字符串 flag（大小写不敏感）替换为空格。  </font>

<font style="color:#000000;">if (strlen($get) > 18){</font>

<font style="color:#000000;">    die("This is too long.");}</font>

<font style="color:#000000;">如果输入的长度超过 18 个字符，终止代码。  </font>

<font style="color:#000000;">既然空格被过滤了，那我们可以尝试使用同为空白字符的制表符%09，代替空格</font>

<font style="color:#000000;">所以传入system("cat%09/f*");</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736608441905-4811645c-ef74-479c-983b-2aa0cdd7f972.png)

<font style="color:#000000;">拿到flag：NSSCTF{0cca4d05-2ad2-4a90-9897-dbe875454267}</font>

<font style="color:#000000;">这道题这个文章讲的很详细，有很多芝士~点可以学习：</font>[<font style="color:#000000;">http://www.coreui.cn/news/171523.html</font>](http://www.coreui.cn/news/171523.html)

<h2 id="ZKRD6"><font style="color:#000000;">（六）[SWPUCTF 2022 新生赛]奇妙的MD5</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736686453403-0cb6e2c2-3683-48f1-bb6d-019632b5d049.png)</h2>
<font style="color:#000000;">奇妙的字符串，又跟 MD5 有关，查了资料，找到了两个：</font>

<font style="color:#000000;">一个是</font><font style="color:#000000;"> </font>[<font style="color:#000000;">MD5 加密</font>](https://so.csdn.net/so/search?q=MD5%20%E5%8A%A0%E5%AF%86&spm=1001.2101.3001.7020)<font style="color:#000000;">后弱比较等于自身，这个字符串是 0e215962017 ：</font>

<font style="color:#000000;">另一个是 MD5 加密后变成万能密码，这个字符串是 ffifdyop</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736686622285-e564fb2b-6728-480b-98e7-718022292450.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736686600148-eb6b0c70-9732-465a-bc42-ddae4118640e.png)

<font style="color:#000000;">0e215962017不行，ffifdyop才出现这个页面</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736686671383-9696db48-e3a7-4744-9e65-334339dfb219.png)

<font style="color:#000000;">这不在源代码这里找吗？</font>![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736686747911-4aa0c775-fa2a-4279-87f3-89ab3141e5c7.png)

<font style="color:#000000;">md5数组绕过</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736686850333-4801d61e-d4af-465a-b069-b09edea0a415.png)

<font style="color:#000000;">url出现了奇怪的东西</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736686875495-cf956c22-70f2-466e-a2ea-1c55a0df8c15.png)

<font style="color:#000000;">访问一下</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736686915904-8d7cac2c-cfd0-4fb0-b595-bec766a3fba7.png)

<font style="color:#000000;">同样数组绕过，拿到flag：</font><font style="color:#000000;">NSSCTF{d2e1d7ef-32ab-4d1c-a1e2-0ea1349d610d}</font>

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1736686988086-efe48298-c369-4689-a3a6-cb8780a02532.png)

