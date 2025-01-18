# 课前作业
## 1、nc连一下
nc给定的靶机，ls显示所有文件，发现flag，cat查看文件内容，发现是假的flag
![屏幕截图 2025-01-17 105920](https://github.com/user-attachments/assets/51ade96c-a5ce-4a2e-8d29-305fb25f2381)

发现目录中还有gift和nothing两个文件，
cd至gift，ls显示，分别打开2galf和flag2

![屏幕截图 2025-01-17 110017](https://github.com/user-attachments/assets/60565983-680f-4447-bc04-f83bef47bc65)

发现了一段flag
cd至nothing，ls显示，cat查看文件内容

![屏幕截图 2025-01-17 110136](https://github.com/user-attachments/assets/3f347737-c364-4c02-b152-5d253b08d253)

发现另一段flag
## 2、test nc
（对新手确实有难度，看这个答案研究了好久）
下载附件，checksec查看文件保护设置

![屏幕截图 2025-01-18 174341](https://github.com/user-attachments/assets/b6d03b40-349f-423b-8854-32557bba9ee8)

发现是64位，用64位IDA打开，F5将main函数转化为c语言形式，观察函数

![屏幕截图 2025-01-17 111939](https://github.com/user-attachments/assets/393fa9fc-3af4-4503-a530-344b48aab24e)

发现cat、flag被strstr函数禁用，s、h也因strpbrk禁止出现所以ls也没法使用
继续观察函数发现条件语句不成立，则关闭标准输出执行backdoor函数，在backdoor函数中发现system函数

![屏幕截图 2025-01-17 170937](https://github.com/user-attachments/assets/3aa93e89-9041-4088-b5b1-01958319a91f)

目标为执行backdoor函数，nc靶机并输入

![屏幕截图 2025-01-18 174451](https://github.com/user-attachments/assets/0d3bb12b-2068-4835-b5b6-6072c32ed772)

**tac为cat的反写，不会被禁用，功能为按行倒序查看文件内容**\
**fla * 是通配符模式，可以模糊搜索文件，表示所有以fla开头的文件，避免了flag被禁用**
**>&2 表示将标准输出重定向到标准错误输出，即使有错误也可以直接输出到终端，便于观察（对重定向错误输出的理解不知道对不对）**\
通过此绕过处理，成功得到flag
