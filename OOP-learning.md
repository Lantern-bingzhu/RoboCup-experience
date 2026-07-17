# 面向对象编程

一个面向对象的代码最少有三部分：

- 类的定义
- 成员函数的定义
- 创建对象然后调用方法

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
除去以下两点，其余与结构体相同:

<details>
<summary></summary>

- public 公有  
类的外部可以读写公有值。

        只有public的类相当于结构体。



- private 私有  
类的外部看不到。

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

### 类的使用

如结构体：
```C++
student aa;
aa.age=20;
```

### 成员函数的重载

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

### 构造函数（对象初始化）

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

带参数的版本：

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

### 析构函数（销毁对象）

```C++

student::~student()

```
等效于

```C++

delete *student;

```

### 常成员函数（只读不写）

若函数内含赋值（age=5）等改动操作会报错。

```C++

bool student::read() const
{
    cout<<age<<endl;
    cout<<name<<endl;
}

```

### 静态成员函数

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

## 派生和继承

若把学生细分成本科生和研究生，则后两者是前者的子类。学生是父类，也叫基类或者超类。
- 所谓派生，是相对于父类而言的。
- 所谓继承，是相对于子类而言的。

### 派生

自动继承父类的属性、方法，包括构造函数和对象的创建。

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

### 继承

```C++
class postgraduate:public student
```

中的public：表示公有继承，仅继承父类公有部分，且继承的也会成为子类的公有部分。  

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