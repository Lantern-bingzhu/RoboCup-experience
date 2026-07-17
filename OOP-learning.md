# 面向对象编程

[视频教程源](https://www.bilibili.com/video/BV12owezFENo/?spm_id_from=333.337.search-card.all.click&vd_source=74181ce85e44eb3f3f001e6a7b8a9d1f)

一个面向对象的代码最少有三部分：

- 类的定义
- 成员函数的定义
- 创建对象然后调用方法

<details>
<summary></summary>

## 封装（类）

<details>
<summary></summary>

    所展示的代码大部分为例子，正规写法应将“方法”（函数等初始化或修改）全部放在public中，“属性”（变量）放在private中。

类的定义形如：
```C++
class student
{
    public:
        string name;
    private:
        int age;
}
```

使用如结构体：
```C++
student aa;
aa.age=20;
```

除去以下两点，其余与结构体相同:

### public 公有  
类的外部可以读写公有值。

        只有public的类相当于结构体。

### private 私有  
类的外部看不到。

<details>
<summary></summary>

#### 目的

保全“封装性”。

```C++

class student//省略了构造函数和属性
{
    public:
        void print_name();
    private:
        void print_age();
}
void student::print_name()
{
    cout<<"张三";
}
void student::print_age()
{
    cout<<20;
}
//下面是主函数。
student aa;
aa.print_name();
aa.print_age();//报错提示不存在print_age。

```

#### 调用

核心思想：通过公有的外壳调用私有。

```C++

class student//省略
{
    public:
        void print_age_public();
    private:
        void print_age_private();
}
void student::print_age_public()
{
    print_age_private();
}
void student::print_age_private()
{
    cout<<20;
}
//下面是主函数
student aa;
aa.print_age_public();//输出20。
aa.print_age_private();//报错。不存在print_age_private。

```

</details>

### 相关函数
<details>
<summary></summary>

#### 构造函数（对象初始化）

构造函数的名字必须和类的名字一样。

不带参数的版本：

```C++

class student
{
    public:
        int age;
        string name;
        student();//构造函数
}
student::student()
{
    age=20;
    name="张三";
    //为全部成员数据赋一个默认值。
}
//下面是主函数。
student aa;

```

带参数的版本（含函数的重载）：

```C++

class student
{
    public:
        int age;
        string name;
        student();
        student(int a,string b);
}
student::student()
{
    age=20;
    name="张三";
    //为全部成员数据赋一个默认值。
}
student::student(int a,string b)
{
    age=a;
    name=b;
}
//下面是主函数。
student aa;
student bb(25."李四");//重载向第二个同名函数。

```

#### 析构函数（销毁对象）

```C++

student::~student()

```
等效于

```C++

delete *student;

```

#### 常成员函数（只读不写）

若函数内含赋值（age=5）等改动操作会报错。

```C++

bool student::read() const
{
    cout<<age<<endl;
    cout<<name<<endl;
}

```

#### 静态成员函数

```C++

class student
{
    public:
        int age;
        string name;
        student();
        static int cnt;
        static int count();
}
student::student()
{
    age=20;
    name="张三";
    cnt++;
}
int student::count()
{
    return cnt;
}
//下面是主函数。
student aa;
student bb;
aa.count();//返回2。
bb.count();//同上。静态成员不依赖于某个对象。
student::count();//同上。只有不依赖于对象的静态函数可以这么写。

```

</details>

</details>

## 继承

<details>
<summary></summary>

若把学生细分成本科生和研究生，则后两者是前者的子类。学生是父类，也叫基类或者超类。

派生，是相对于父类而言的。继承，是相对于子类而言的。

子类自动继承父类的属性、方法，包括对象的创建。构造函数不可被继承，但每次创造子类对象时都会被调用。

```C++

class undergraduate:public student
{
    public:
        string course;//undergraduate类新增一个公开的字符串属性。
}
class postgraduate:public student
{
    public:
        string research;//postgraduate类新增一个公开的字符串属性。
}

```

类定义中的public：表示公有继承，仅继承父类公有部分，且继承的也会成为子类的公有部分。  

若将其替换为private，则继承父类公有部分，继承的会成为子类的私有部分。  

可见子类无法继承也无法访问父类的私有部分。  

——————那么怎么继承父类的属性？

#### protect关键字

该关键字与public和private平级，在没有派生继承的情况下与private作用相同。

将父类定义的private替换为protect，则保持封装功能不变的情况下，原私有部分可被继承。

```
- 总结
    - 公有继承
        - 父类公有内容A，私有内容B，子类公有继承，获得公有内容A。
        - 父类公有内容A，保护内容B，子类公有继承，获得公有内容A和保护内容B。
    - 私有继承和保护继承
        - 父类公有内容A，私有内容B，子类私有继承，获得私有内容A。
        - 父类公有内容A，保护内容B，子类保护继承，获得保护内容A和保护内容B。
```

### 子类的构造函数

<details>
<summary></summary>

```C++

postgraduate::postgraduate(int a,string b,string c):student(a,b)
{
    research=c;
}
//下面是主函数。
postgraduate bb(25,"李四","ASIC design");

```

程序会先调用父类student构造函数，把25（age）和李四（name）两个值传入其带参数的构造函数（初始化）。

随后将参数c=ASIC design传给bb自己的research属性，实现了全动态初始化。

</details>

</details>

## 多态

<details>
<summary></summary>

### 成员函数的重载（静态多态）

<details>
<summary></summary>

[教程源](https://zhuanlan.zhihu.com/p/623029910)

和普通函数的重载一致。基本规则：

- 函数名相同。
- 参数列表不同。（包括类型、个数、顺序差异）
- 返回值类型相同。

```C++

class student
{
    public:
        int age;
        string name;
        bool set(int a);
        bool set(string a);
}

```

编译器会根据实参类型决定运行哪个同名函数。

</details>

### 重写（隐藏、覆盖）

<details>
<summary></summary>

若父类与子类有一同名函数方法，则父类函数被隐藏。

```C++

class student//省略
{
    public:
        void study(string a)
        {
            cout<<"好好学习";
        }
};
class postgraduate:public student//省略
{
    public:
        void study(int b)
        {
            cout<<"芯片设计";
        }
};
//下面是主函数
postgraduate bb;
student aa;
bb.study(2);
aa.study("yes");
bb.study("yes");//出错，本应重载父类的study方法。但父类方法被隐藏。

```

若要访问父类同名函数：

```C++

bb.student::study("yes");

```
若要理解重载与重写的区别：[教程源](https://csguide.cn/cpp/object_oriented/overloading_overriding_and_hiding.html)
- 重载为单级多分支结构，重写为后来者居上。
- 重载为同一级别函数关系，重写为父类与子类函数关系。

</details>

### 类指针

<details>
<summary></summary>

不考虑继承，类指针与结构体指针类似。

```C++

student *p;
student aa;
p=&aa;
p->name;//相当于aa.name。
p->study();//相当于aa.study()，对aa执行成员函数。
//更高级写法
student *p=new student(20,'张三');
delete p;

```

只考虑公有继承：

```C++

student *p1;
postgraduate *p2;
student aa;
postgraduate bb;
p1=&aa;
p2=&bb;
p1=&bb;//通过，父类指向子类。
p2=&aa;//报错，子类指针指向父类对象。
p1->research//报错，不可调用子类属性和方法。

```

若想调用子类方法——————

</details>

### 虚函数（动态多态）

<details>
<summary></summary>

[教程源](https://zhuanlan.zhihu.com/p/629281871)

- 为什么需要？
    - 实现多态性，即同一个函数名可以在不同的子类中表现出不同的行为。
    - 避免静态绑定。若未使用虚函数，则只能调用父类的成员函数，无法调用子类特有的成员函数。
    - 通过定义纯虚函数实例化抽象类。

#### 动态绑定与静态绑定

<details>
<summary></summary>

##### 静态绑定：

在编译时就确定要调用哪个方法，也称为早期绑定。这种绑定方式通常出现在使用类或接口定义对象时，编译器根据对象的类型来决定具体要调用哪个方法。

```C++

class Shape
{
    public:
        void draw()
        { 
            cout<<"Drawing a shape."<<endl;
        }
};
class Circle:public Shape
{
    public:
        void draw()
        {
            cout<<"Drawing a circle."<<endl;
        }
};
int main()
{
    Shape *shapeObj=new Circle();
    shapeObj->draw();
    // 由于Shape类中的draw()方法不是虚函数，在编译时期就确定了方法调用，输出 "Drawing a shape."。
}

```

##### 动态绑定：

在程序运行时才能确定要调用哪个方法，也称为晚期绑定。这种绑定方式通常出现在使用子类对象调用父类方法时，程序会根据实际的对象类型来决定具体要调用哪个方法。

```C++

class Shape{
    public:
        virtual void draw() 
        { 
            cout<<"Drawing a shape."<<endl;
        }
};
class Circle:public Shape
{
    public:
        void draw() 
        { 
            cout<<"Drawing a circle."<<endl;
        }
};
int main()
{
    Shape *shapeObj=new Circle();
    shapeObj->draw();
    // Shape类中的draw()方法被声明为虚函数，在运行时期才确定方法调用，输出 "Drawing a circle."。
}

```

</details>

派生类可以重写它继承的虚函数，这被称为函数的覆盖。但在子类中重写虚函数时，其访问权限不能更严格（即不能由 public 变为 private 或 protected），否则编译器会报错。

```C++

class student
{
    public:
        virtual void study();
}
void student::study()
{
    cout<<"好好学习";
}
class postgraduate:public student
{
    public:
        virtual void study();
}
void post postgraduate::study()
{
    cout<<"芯片设计";
}
class undergraduate:public student
{
    public:
        virtual void study();
}
void undergraduate::study()
{
    cout<<"大学物理";
}
//下面是主函数
student  aa;
post graduate bb;
undergraduate cc;
student *p;
p=&aa;
p->study();
p=&bb;
p->study();//父类指针调用子类研究生的方法，打印出芯片设计，就是多态。
p=&cc;
p->study();//父类指针调用了子类本科生的方法，打印出大学物理，就是多态

```

当然，子类也可以选择不重写基类的虚函数，那么它将默认继承基类的实现，这就是虚函数的重载。

重载（静态多态）是编译时决定，动态多态是运行时调用。

前者编译时已经规定路径，后者会把所有情况储存入内存、即虚函数表中，用虚函数指针实现动态绑定，因此调用成本颇高。

在多级继承中，每个子类都需要维护自己的虚函数表及其对应的虚函数指针。

</details>

### 抽象类与纯虚函数

<details>
<summary></summary>

任何个体必然属于一个子类，不可能只属于父类，这种父类被称为抽象类。

构造抽象类依赖于纯虚函数，形式如下：

```C++

class student
{
    public:
        student();//初始化
        student(int a,string b);//重载
        virtual void study()=0;//virtual与=0表示此为纯虚函数，只有声明没有定义。
};

```

纯虚函数具体内容依靠子类的多态实现。如果一个类继承了抽象类，则必须实现（使该函数被定义）所有的纯虚函数，否则该类也会成为抽象类。

抽象类可创建指针，不可创建对象。

```C++

student aa;//报错，不可创建抽象类对象。
student *p;//可以创建抽象类指针。
postgraduate bb;
p=&bb;
p->study();//通过多态调用子类的study方法。

```
</details>

</details>

</details>