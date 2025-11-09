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
