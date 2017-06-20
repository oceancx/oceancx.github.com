---
---

2.3 复合类型 (Compund type)
	2.3.1 引用
	2.3.2 指针
	2.3.3 理解复合类型的声明

2.4 const限定符
	2.4.1 const引用
	2.4.2 指针和const

6.2 参数传递
	6.2.2 传引用参数
	
		void reset(int &i){i = 0;}

		int j=42;
		reset(j);
		cout<< "j = "<<j<<endl;  // j = 0


	引用可以作为参数 返回一些额外的信息


2.4.3 顶层const

top-level const(指针本身是个常量) , low-level const(指针所指对象是常量)
指针类型既可以是顶层常量也可以是底层常量

底层常量拷贝时要注意，一般来说非常量可以转换成常量，反之则不行

tip:尽量使用常量引用

6.2.5 main: 处理命令行选项

initializer_list形参：
initializer_list是一种标准库类型，用于表示某种特定类型的值的数组

6.3 返回类型和return语句

4.1 左值和右值

当一个对象被用作右值的时候，用的是对象的值（内容），当对象被用作左值的时候，用的是对象的身份。（在内存中的位置）

一个重要的原则：在需要右值的地方可以用左值代替，但是不能把右值当成左值。当一个左值被当成右值使用时，用到的是他的内容

6.1 函数
重载和const形参

Record lookup(Phone*);
Record lookup(Phone* const);		//重复声明了Record lookup(Phone)

Record lookup(Phone*);
Record lookup(const Phone*); 		//这两个可以重载

#### 常量转换

const_cast<string&>(r);
const_cast<const string&>(r);


#### 函数匹配

6.4.1 重载与作用域

6.5.1 默认实参
一旦某个形参被赋予了默认值，后面所有形参都必须有默认值

6.5.3 调试帮助

6.6 函数匹配
1. this指针在类中默认存在，对象调用成员函数的时候会将this指针绑定到对象上。

2. const成员函数，将成员函数设置成const提高了类的灵活性，使得const Class* this可以正常调用成员函数。（默认情况下，不能将this指针绑定到cosnt对象上）

3. 构造函数：

3.1 默认构造函数

3.1.1 对于普通的类，必须定义自己的默认构造函数，原因有三：

1.最容易理解是编译器只有在发现类不包含任何构造函数的情况下才会替我们生产一个默认的构造函数

2. 某些类，合成构造函数可能执行错误的操作，内置类型或符合类型（数组 指针）成员的类未定义的值

3. 编译器不能为某些类合成默认构造函数，类成员无默认构造函数

如果类中函数vector 或者 string 进行拷贝 或 赋值的话,vector string虽然是动态分配但是也能够work

mutable 用于在const 函数中改变值

7.5 委托构造函数

7.5.4 转换构造函数 只有一个参数的构造函数

10.1 

algorithm:

find(iter.begin() , iter.end() , val )
 accumulate(vec.cbegin(),vec.end(),0)
 fill
 copy 
 replace
 sort
 uniq
 begin end
 swap 
 move
 find_if() 只接受一元谓词

 lambda捕获采用值拷贝，值捕获跟引用捕获不同
 隐式捕获
 transform()


functional -> bind()函数
接受一个可调用对象，生成一个新的可调用对象

ref（）返回一个对象，包含给定的引用，此对象是可以拷贝的


再探迭代器：
1. 插入迭代器
2. 流迭代器 将对应的流当做一个特定类型的元素序列进行处理，可以用反省算法从流对象读取数据以及相弃吸入数据
3. 反向迭代器
4. 移动迭代器

接受谓词参数的算法都有附加的_if前缀

拷贝不拷贝
list merge sort uniq splice

11关联容器
pair->utility

对map而言 value_type是一个pair类型，
first保存const的关键字 second保存值

map下标操作的返回值：
map的下表运算符返回类型是mapped_type对象，
解引用的时候会得到value_type对象相同

hash<string>()(sd.isbn())''



	
