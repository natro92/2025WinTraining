<h3 id="xY0vR">java1</h3>
import java.util.Scanner;//导入了Scanner类，用于从中读取数据

public class java1 {//定义了一个公共类，注意：该类名称必须与文件名相同（大小写敏感）

    public static void main(String[] args) {//程序的主函数main

        Scanner sc=new Scanner(System.in);//创建了一个Scanner对象sc,从标准输入中读取数据

        int max=0;

        int i=0;//定义了两个变量，分别初始化为0

        while(sc.hasNext()){//while循环:只要输入还有下一个整数，就重新进入循环

            int cmp=sc.nextInt();//读入下一个整数，放入变量cmp中

            if(i==0){

                max=cmp;

                i++;

            }//如果是第一次读取，就把cmp的值赋给max

            if(cmp>max){

                max=cmp;

            }//如果新读取的数更大，就进行替换

            if(cmp==0){

                i=0;

                System.out.println(max);//如果读取的数为0，就输出当前最大值，并将i重置为0

                continue;//继续下一次循环

            }

        }

    }

}//最终该代码实现 记录数据最大值的功能

/*当不遇到0时，不断比较替换最大值，直到读取的数为0时，输出最大值并重新记录*/

<h3 id="DEOVZ">java2</h3>
//该代码是java2.java，不过私自对程序添加了打印功能

//注意：Java的命名规范,以public声明的类必须用完全一致的源文件名称，比如此处修改文件名为ChainMethodExample.java

class Person {//定义了一个名为Person的类

    private String name;

    private int age;//声明了两个私有数据

    public Person() {

    }//定义了一个无参数构造的析构函数

    public Person setName(String name) {

        this.name = name;

        return this;

    }//公有函数：用于设置name属性的值



    public Person setAge(int age) {

        this.age = age;

        return this;

    }//公有函数：用于设置age的值



    //添加获取name的方法

    public String getName(){

        return name;

    }



    //添加获取age的方法

    public int getAge(){

        return age;

    }

}

public class ChainMethodExample {//定义了一个名为ChainMethodExample的公共类

    public static void main(String[] args) {//定义主函数main

        Person person = new Person()//创建了一个名为person的对象

              .setName("Alice")

              .setAge(25);//根据对象进行设置

            //进行

            System.out.println("Name:"+person.getName());

            System.out.println("Age:"+person.getAge());

    }

}//注意：Java的命名规范,以public声明的类必须用完全一致的源文件名称，比如此处修改文件名为ChainMethodExample.java

<h3 id="Dy9xW">java3</h3>
//注意：Java的命名规范,以public声明的类必须用完全一致的源文件名称，比如此处修改文件名为Fish.java

abstract class Animal{//首先定义抽象类Animal

    protected int legs;//定义受保护的整型变量

    protected Animal(int legs){

        this.legs=legs;

    }//初始化，对legs进行赋值

    abstract void eat();//抽象的方法，会在继承的子类中实现

    public void walk(){

        System.out.println(legs+"条腿走路");

    }

}

interface Pet{//接口

    String getName();//声明一个函数

    void setName(String name);

    void play();//声明两个公有函数

}



class Fish extends Animal implements Pet {//继承类Fish

    private String name;//声明私有变量



    public Fish() {

        super(0);

    }//调用父类Animal的构造函数



    public void walk() {

        System.out.println("无腿，不能走路");

    }//重写父类Animal的walk方法



    @Override

    void eat() {

        System.out.println("通过嘴巴吞咽食物");

    }//进行重写



    @Override

    public String getName() {

        return name;

    }//具体实现pet接口的getName方法



    @Override

    public void setName(String name) {

        if (name == null || name.isEmpty()) {

            System.out.println("名字不合法");

            return;

        }

        this.name = name;

    }//具体实现pet接口的setName方法，检查名字是否为空



    @Override

    public void play() {

        System.out.println("在水中游动玩耍");

    }//实现play方法



    public static void main(String[] args) {//这是程序的入口

        Fish fish = new Fish();//创建一个Fish类的对象

        fish.setName("peach");

        System.out.println("Name:"+fish.getName());

        fish.eat();

        fish.play();

        fish.walk();//再依次调用相应函数

    }

}



