**一、基本概念**
**1.基本数据类型**
	七种基本的C++数据类型：`bool、char、int、float、double、void`
	类型修饰符：`signed、unsigned、short、long`
**2.经典算法程序：**
冒泡排序：
```
void BubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
            }
        }
    }
}
```
斐波那契数列：
```
int fib(int n) {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}
```
...
**3.指针和引用**
```
int a = 10, b = 20;

// 指针可以重新指向其他变量
int *ptr = &a;
ptr = &b;  // 现在指向b

// 引用一旦绑定就不能改变
int &ref = a;//当然，引用不能为空，而指针可以为空
ref = b;   // 这不是重新绑定，而是把b的值赋给a
// ref仍然引用a，现在a的值变成了20

cout << &ptr;  // 输出指针本身的地址
cout << &ref;  // 输出a的地址（与&a相同）
```
**4.类与对象**
类：
```
class Human {
    public:	//公有的，对外的 
    void eat(); //成员函数
    void sleep(); 
 
    string getName(); 
    int getAge(); 
    int getSalary();
 
private: //私有的，对象只能调用类的public方法
    string name; int age; int salary;
};
```
对象，是一个特定“类”的具体实例。即：
```
	Human h1;  //h1即为一个对象
	Human *p; p = &h1;
	
	// 合法使用
    h1.eat(); 
    h1.sleep();
    p->eat();
    p->sleep();
     
    // 非法使用
	cout << "年龄" << h1.age(p->age) << endl;	//直接访问私有成员，将无法通过编译
 
    //正确使用 
    cout << "年龄" << h1.getAge()(p->getAge()) << endl; //暴露问题，年龄值是一个很大的负数
```

**5.类的构造函数**
	作用：在创建一个新的对象时，自动调用的函数，用来进行“初始化”工作：对这个对象内部的数据成员进行初始化。
	特点：
		1. 自动调用（在创建新对象时，自动调用）
		2. 构造函数的函数名，和类名**相同**
		3. 构造函数没有返回类型
		4. 可以有多个构造函数（即函数重载形式）
```
class Human { 
public: //公有的，对外的
    Human(); //手动定义的“默认构造函数”
 
    void eat(); //方法， “成员函数”
    void sleep(); 
 
    string getName(); 
    int getAge(); 
    int getSalary();
private:
    string name = "Unknown"; 
    int age = 28;
    int salary;
};

//构造函数中的初始化，会覆盖对应的类内初始值
Human::Human() { 
    name = "无名氏"; 
    age = 18; 
    salary = 30000;
}

//自定义的重载构造函数
Human::Human(int age, int salary) {
    //cout << "调用自定义的构造函数" << endl; 
    this->age = age;	//this 是一个特殊的指针，指向这个对象本身 
    this->salary = salary;
    name = "无名";
}

//自定义的拷贝构造函数
Human::Human(const Human& man) {
	//cout << "调用自定义的拷贝构造函数" << endl;
	name = man.name; age = man.age;
	salary = man.salary;
}
```
**6.析构函数**
	作用：对象销毁前，做清理工作。对象销毁时，自动调用
		比如：如果在构造函数中，使用 new 分配了内存，就需在析构函数中用 delete 释放。如果构造函数中没有申请资源（主要是内存资源），那么很少使用析构函数。
	
```
...
	~Human();
...
Human::~Human() {
    cout << "调用析构函数-" << this << endl; //用于打印测试信息 delete addr;
}
```
**7.this指针**
```
...
const Human& Human::compare2(const Human& other) { 
    if (age > other.age) {
	    return *this;	//访问该对象本身的引用，而不是创建一个新的对象
    }else{
        return other;
    } 
}
...
cout << h1.compare2(h2).getAge() << endl;
```
 **8.Rule of Three/Five**
 - **三法则**：如果需要析构函数，通常也需要拷贝构造函数和拷贝赋值运算符
 - **五法则**：C++11后，加上移动构造函数和移动赋值运算符
**9.静态数据成员与静态成员函数**
- 静态数据成员：类的“全局”变量
```
class Human { 
public:
    ......
    int getCount();
private:
    string name = "Unknown"; 
    int age = 28;
    ......
    // 类的静态成员
    static int count;//通常在构造函数里面count++，静态成员不是private
};
```
- 静态成员函数：当前没有可用对象。如果为了访问总的人数，而特意去创建一个对象，就很不方便，而且得到的总人数还不真实（包含了一个没有实际用处的人）
```
class Human { 
public:
    static int getCount();//没有对象的建立即可调用
};
...
//静态方法的实现，不能加static
int Human::getCount() {
// 静态方法中，不能访问实例成员（普通的数据成员）
// cout << age;
// 静态方法中，不能访问this指针
// 因为this指针是属于实例对象的
// cout << this;
//静态方法中，只能访问静态数据成员
return count;
}
```
**10.const数据成员和const成员函数**
- const数据成员：不能在构造函数或其他成员函数内，对 const 成员赋值
```
	const string bloodType;
	
// 使用初始化列表，对const数据成员初始化
Human::Human():bloodType("未知") { ......
    //在成员函数内，不能对const数据成员赋值
    //bloodType = "未知血型";
    count++;
}
void Human::description() const { 
    cout << "age:" << age
         << " name:" << name
         << " salary:" << salary
         << " addr:" << addr
         << " bloodType:" << bloodType << endl; //其他成员函数可以“读”const变量
}
```
- const成员函数：不会修改任何数据成员，一般用于打印数据
```
//Human.h class Human { 
public:
    ......
    void description() const; //注意，const的位置
    ......
};
 
//Human.cpp 
void Human::description ()const { 
     cout << "age:" << age
     << " name:" << name
     << " salary:" << salary
     << " addr:" << addr
     << " bloodType:" << bloodType << endl;
}
 
//main.cpp 
int main(void) { 
    const Human h1; 
    h1.description();
 
    system("pause"); 
    return 0;
}
```
**二、继承与派生**
**1.继承与派生的概念**
	父亲“派生”出儿子，儿子“继承”自父亲，本质是相同的，只是从不同的角度来描述。
	除了“构造函数”和“析构函数”，父类的所有成员函数，以及数据成员，都会被子类继承！
```
//父类
class Father
{
public:
    Father(const char*name, int age);
    ~Father();
 
    string getName(); 
    int getAge(); 
    string description();
private:
    int age; 
    string name;
};

Father::Father(const char*name, int age)
{ 
    cout << __FUNCTION__ << endl; 
    this->name = name; 
    this->age = age;
}
 
Father::~Father()
{ 
 
}
 
string Father::getName() { 
    return name;
}
 
int Father::getAge() { 
    return age;
}
 
string Father::description() { 
    stringstream ret; ret << "name:" << name << " age:" << age;
 
    return ret.str();
}

//子类
class Son : public Father { 
public:
    Son(const char *name, int age, const char *game);
    ~Son(); 
 
    string getGame(); 
    string description();
private:
    string game;
};

// 创建Son对象时, 会调用构造函数!
// 会先调用父类的构造函数, 用来初始化从父类继承的数据
// 再调用自己的构造函数, 用来初始化自己定义的数据
Son::Son(const char *name, int age, const char *game) : Father(name, age) { 
    cout << __FUNCTION__ << endl;
// 没有体现父类的构造函数, 那就会自动调用父类的默认构造函数!!! this->game = game;
}
 
Son::~Son() {
 
}
 
string Son::getGame() { 
    return game;
}
 
string Son::description() { 
    stringstream ret;
    // 子类的成员函数中, 不能访问从父类继承的private成员
 
    ret << "name:" << getName() << " age:" << getAge()
        << " game:" << game; return ret.str();
}

int main(void) {
    Father wjl("王健林", 68);
    Son wsc("王思聪", 32, "电竞");
 
    cout << wjl.description() << endl;
    // 子类对象调用方法时, 先在自己定义的方法中去寻找, 如果有, 就调用自己定义的方法
    // 如果找不到, 就到父类的方法中去找, 如果有, 就调用父类的这个同名方法 // 如果还是找不到, 就是发生错误!
    cout << wsc.description() << endl;
 
    system("pause"); 
    return 0;
}
```