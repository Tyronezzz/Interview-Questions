
## C++

- 重载(overload)、重写(overrride)、重定义(redefine)
https://www.cnblogs.com/tanky_woo/archive/2012/02/08/2343203.html





- lambda func
inline function which can be used for short snippets of code that are not going to be reuse and not worth naming




- lvalue, rvalue
an lvalue is something that points to a specific memory location
rvalues are temporary and short lived, while lvalues live a longer life since they exist as variables
int x = 5;  int* y = &x; 





- explicit 
The compiler is allowed to make one implicit conversion to resolve the parameters to a function.
 What this means is that the compiler can use constructors callable with a single parameter to convert from one type to another in order to get the right type for a parameter.



- 引用
不存在空引用。引用必须连接到一块合法的内存。
一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
引用必须在创建时被初始化。指针可以在任何时间被初始化。
引用实际是通过指针实现的。
引用是一个常量指针，在内存中占4个字节。




- 把引用作为返回值
通过使用引用来替代指针，会使 C++ 程序更容易阅读和维护。C++ 函数可以返回一个引用，方式与返回一个指针类似
在内存中不产生被返回值的副本。
```
int &changevalue()
{
    static int a_return =-29;
    return a_return;
}

int main()
{
    int &a_return = changevalue();
    a_return =20;
    cout<<changevalue()<<endl;
    system("pause");
}
```


- 不可返回一个临时变量的引用，函数返回时该变量已被销毁
```
string &func()
{
	string tmp = "gg";
	return tmp;
}
```





- static
Static variables in a Function: When a variable is declared as static, space for it gets allocated for the lifetime of the program. Even if the function is called multiple times, space for the static variable is allocated only once and the value of variable in the previous call gets carried through the next function call. 


	in a class : the static variables in a class are shared by the objects. There can not be multiple copies of same static variables for different objects. Also because of this reason static variables can not be initialized using constructors.



	static member functions also does not depend on object of class. Static member functions are allowed to access only the static data members or other static member functions, they can not access the non-static data members or member functions of the class.

	静态成员函数和静态数据成员一样，它们都属于类的静态成员，它们都不是对象成员。因此，对静态成员的引用不需要用对象名。
静态成员函数仅能访问静态的数据成员，不能访问非静态的数据成员，也不能访问非静态的成员函数





- const function
The idea of const functions is not to allow them to modify the object on which they are called.
When a function is declared as const, it can be called on any type of object. Non-const functions can only be called by non-const objects.


https://zh.coursera.org/lecture/cpp-chengxu-sheji/chang-liang-dui-xiang-chang-liang-cheng-yuan-han-shu-he-chang-yin-yong-wrcJr
常量成员函数不能修改成员变量的值（static除外），不能调用同类的非常量成员函数
常量对象不能执行非常量成员函数

不能通过常引用修改引用的变量
const int &a = b;
a = 4;    //error
b = 5;     //ok


- 常引用： 

对象作为参数，要调用 copy constructor，效率低  --》 引用
引用时不小心修改形参，实参也会变    -》常引用




- 模版
https://www.runoob.com/cplusplus/cpp-templates.html



模板类中可以使用虚函数。
模板成员函数不可以是虚函数。
编译器都期望在处理类的定义的时候就能确定这个类的虚函数表的大小，如果允许有类的虚成员模板函数，
那么就必须要求编译器提前知道程序中所有对该类的该虚成员模板函数的调用，而这是不可行的。 

```
template <class T> struct AA {
	template <class C> virtual void g(C); // error
	virtual void f(); // OK
};
```




- smart pointers

auto_ptr   所有权转移，两个auto_ptr指向一个，仍可访问，但null。
auto_ptr不能指向数组，因为auto_ptr在析构的时候只是调用delete,而数组应该要调用delete[]。
auto_ptr不能和标准容器（vector,list,map…)一起使用。 STL容器中的元素经常要支持拷贝，赋值等操作，在这过程中auto_ptr会传递所有权。
auto,shared_ptr  new分配内存
unique_ptr new， new[] 分配内存



unique_ptr 两个unique_ptr指向一个，编译时报错
我们无法通过复制构造函数或赋值运算符创建unique_ptr对象的副本
```
shared_ptr<A> sp1(new A(2)); //A(2)由sp1托管，
shared_ptr<A> sp2(sp1);       //A(2)同时交由sp2托管
shared_ptr<A> sp3 = sp2;
```

不可
```
A* p = new A(10);
shared_ptr <A> sp1(p), sp2(p);
```

如果程序要使用多个指向同一个对象的指针，应选择shared_ptr。这样的情况包括：有一个指针数组，并使用一些辅助指针来表示特定的元素，如最大的元素和最小的元素；
两个对象包含都指向第三个对象的指针；STL容器包含指针。
如果程序不需要多个指向同一个对象的指针，则可以使用unique_ptr。

release():释放对资源的所有权。把智能指针赋空。
reset():如果不传递参数（或者传递 NULL），则智能指针会释放当前管理的内存。如果传递一个对象，则智能指针会释放当前对象，来管理新传入的对象。





- C++ 四种强制转换类型函数      https://blog.csdn.net/ydar95/article/details/69822540

1. const_cast
常量指针被转化成非常量的指针，并且仍然指向原来的对象
// 通过const_cast<Ty> 去常量
int *ptr = const_cast<int*>(c_ptr);

2. static_cast
作用和C语言风格强制转换的效果基本一样,由于没有运行时类型检查来保证转换的安全性，所以这类型的强制转换和C语言风格的强制转换都有安全隐患
用于类层次结构中基类（父类）和派生类（子类）之间指针或引用的转换。注意：进行上行转换（把派生类的指针或引用转换成基类表示）是安全的；进行下行转换（把基类指针或引用转换成派生类表示）时，由于没有动态类型检查，所以是不安全的



3. dynamic_cast
用于动态类型转换。只能用于含有虚函数的类，用于类层次间的向上和向下转化。只能转指针或引用。向下转化时，如果是非法的对于指针返回NULL，对于引用抛异常。要深入了解内部转换的原理。


4. reinterpret_cast
任意的指针之间的转换，引用之间的转换，指针和足够大的int型之间的转换，整数到指针的转换


- 动态链接
动态链接是指在生成可执行文件时不将所有程序用到的函数链接到一个文件，因为有许多函数在操作系统带的dll文件中，当程序运行时直接从操作系统中找。
而静态链接就是把所有用到的函数全部链接到exe文件中。

动态链接是只建立一个引用的接口，而真正的代码和数据存放在另外的可执行模块中，在运行时再装入；
而静态链接是把所有的代码和数据都复制到本模块中，运行时就不再需要库了。


- 指针函数, 函数指针
指针函数 就是一个返回指针的函数，其本质是一个函数，而该函数的返回值是一个指针。
函数指针，其本质是一个指针变量，该指针指向这个函数
```
int (*fun)(int x,int y);
fun = &add;
(*fun)(2,3)
```

- new  malloc
https://www.cnblogs.com/ywliao/articles/8116622.html
https://www.cnblogs.com/qg-whz/p/5060894.html

1,malloc与free是C++/C语言的标准库函数，new/delete是C++的运算符。它们都可用于申请动态内存和释放内存。
2,对于非内部数据类型的对象而言，光用maloc/free无法满足动态对象的要求。对象在创建的同时要自动执行构造函数，对象在消亡之前要自动执行析构函数。由于malloc/free是库函数而不是运算符，不在编译器控制权限之内，不能够把执行构造函数和析构函数的任务强加于malloc/free。 
3,因此C++语言需要一个能完成动态内存分配和初始化工作的运算符new，以一个能完成清理与释放内存工作的运算符delete。注意new/delete不是库函数。
4,C++程序经常要调用C函数，而C程序只能用malloc/free管理动态内存。
5、new可以认为是malloc加构造函数的执行。new出来的指针是直接带类型信息的。而malloc返回的都是void指针。





- c++内存分区
堆 malloc,new
栈 编译器自动申请和释放的  局部变量，函数参数，局部常量
静态存储区 静态变量，全局变量，以及虚函数表
代码区
常量区  全局常量，函数指针



- 常量指针和指针常量
https://blog.csdn.net/jackystudio/article/details/11519817
const int  *p
int *const p




-  拷贝构造函数
对象作为值，初始化新对象
复制对象，作为函数形参
复制对象，作为函数返回值


- 初始化列表
https://www.cnblogs.com/graphics/archive/2010/07/04/1770900.html

	常量成员，因为常量只能初始化不能赋值，所以必须放在初始化列表里面
引用类型，引用必须在定义的时候初始化，并且不能重新赋值，所以也要写在初始化列表里面
没有默认构造函数的类类型，因为使用初始化列表可以不必调用默认构造函数来初始化，而是直接调用拷贝构造函数初始化。


- 类的6个默认的成员函数包括：
构造函数、析构函数、拷贝构造函数、赋值运算符重载函数、取地址操作符重载、const修饰的取地址操作符重载。


- 继承方式
若继承方式是public，基类成员在派生类中的访问权限保持不变，也就是说，基类中的成员访问权限，在派生类中仍然保持原来的访问权限；

若继承方式是private，基类所有成员在派生类中的访问权限都会变为私有(private)权限；

若继承方式是protected，基类的共有成员和保护成员在派生类中的访问权限都会变为保护(protected)权限，私有成员在派生类中的访问权限仍然是私有(private)权限。


- 面向对象
面向对象是首先抽象出各种对象（各种类），把数据和方法都封装在对象中（类），然后各个对象之间发生相互作用。
面向过程是将问题分解成若干步骤（动作），每个步骤（动作）用一个函数来实现，在使用的时候，将数据传递给这些函数。





- vector  
resize    改变容器大小，这也可能导致容量的增加
std::vector<int> values {1,2,3};
values.resize (5);
values.resize (7, 99);
values.resize (6);
capacity   reserve() 来增加容器的容量




迭代器失效
-  vector,deque
```
for(auto it=values.begin();it!=values.end();)
    {
        if(*it==5)
            it = values.erase(it);
        else
            it++;
        cout<<*it<<" ";
    }
```

- map,set
```
map<int, string>::iterator tmpIter = iter;
iter++;
dataMap.erase(tmpIter);
//dataMap.erase(iter++) 这样也行
```
数组型数据结构：该数据结构的元素是分配在连续的内存中，insert和erase操作，都会使得删除点和插入点之后的元素挪位置，所以，插入点和删除掉之后的迭代器全部失效，也就是说insert(*iter)(或erase(*iter))，然后在iter++，是没有意义的。解决方法：erase(*iter)的返回值是下一个有效迭代器的值。 iter =cont.erase(iter);

链表型数据结构：对于list型的数据结构，使用了不连续分配的内存，删除运算使指向删除位置的迭代器失效，但是不会失效其他迭代器.解决办法两种，erase(*iter)会返回下一个有效迭代器的值，或者erase(iter++).

树形数据结构： 使用红黑树来存储数据，插入不会使得任何迭代器失效；删除运算使指向删除位置的迭代器失效，但是不会失效其他迭代器.erase迭代器只是被删元素的迭代器失效，但是返回值为void，所以要采用erase(iter++)的方式删除迭代器。

原文链接：https://blog.csdn.net/lujiandong1/java/article/details/49872763




// deque???????



## os

- 线程同步的方式
https://www.cnblogs.com/raichen/p/5768752.html
互斥量
信号量
条件变量



- 进程的状态与转换
就绪  waiting for cpu
运行
阻塞  waiting for resource


- PV操作的意义：
我们用信号量及PV操作来实现进程的同步和互斥。PV操作属于进程的低级通信。
P waiting   while s<=0   ;   s--
V releasing  s++



- 进程通信
共享存储
消息传递
管道通信
https://juejin.im/entry/592257b62f301e006b183b95





- 常用的系统调度算法
先来先服务和短作业(进程)优先调度算法
高优先权优先调度算法(抢占式，非。。。)
基于时间片的轮转调度算法（时间片轮转法，多级反馈队列调度算法）






- 虚拟内存
为每个进程提供了一个一致的、私有的地址空间，它让每个进程产生了一种自己在独享主存的错觉（每个进程拥有一片连续完整的内存空间）


- 死锁条件



- 进程线程
进程是系统进行资源分配和调度的一个独立单位.
线程是进程的一个实体,是CPU调度和分派的基本单位,它可与同属一个进程的其他的线程共享进程所拥有的全部资源.

	每个进程在操作系统内部用 进程控制块（process control block，PCB） 来表示，它包含了与一个特定进程相关的信息。每个进程控制块包含：
进程状态（process state） ：可为上述五种状态
程序计数器（program counter） ：指定进程要执行的下条指令地址
CPU 寄存器 ：根据计算机体系结构的不同，寄存器数量、类型也不同，多数包括累加器、索引寄存器、堆栈指针、通用寄存器等
CPU 调度信息（CPU-scheduling information） ：包括进程优先级、调度队列的指针和其它调度参数
内存管理信息（memory-management information） ：根据内存系统，通常包括基址、界限寄存器值、页表或段表
统计信息（accounting information） ：包括 CPU 时间、实际使用时间、时间界限、统计数据、作业/进程数量等
I/O 状态信息 ：包括分配给进程的 I/O 设备列表、打开的文件列表等




- 外部碎片
在内存上，外部碎片是位于任何两个操作系统分配的用于装载进程的内存区域或页面之间的空闲区域，内部碎片是位于一个操作系统分配的用于装载进程的内存区域或页面内部的空闲区域。




- 分页
把主存空间划分为大小相等且固定的块，块相对较小，作为主存的基本单位。每个进程也以块为单位进行划分，进程在执行时，以块为单位逐个申请主存中的块空间。
因为程序数据存储在不同的页面中，而页面又离散的分布在内存中，因此需要一个页表来记录逻辑地址和实际存储地址之间的映射关系，以实现从页号到物理块号的映射。
例子，有内部碎片
逻辑地址            页表                 物理地址
页？偏移量        页号，块号            块号 * 页大小 + 偏移量





- 分段
分段是为了满足程序员在编写代码的时候的一些逻辑需求
分段内存管理当中，地址是二维的，一维是段号，一维是段内地址；其中每个段的长度是不一样的，而且每个段内部都是从0开始编址的。
外部碎片
https://blog.csdn.net/zhaohong_bo/article/details/90166135?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task
https://juejin.im/entry/592257b62f301e006b183b95
https://www.yiibai.com/os/os-segmentation.html




- 内核态
内核态：cpu可以访问内存的所有数据，包括外围设备，例如硬盘，网卡，cpu也可以将自己从一个程序切换到另一个程序。
用户态：只能受限的访问内存，且不允许访问外围设备，占用cpu的能力被剥夺，cpu资源可以被其他程序获取。

用户态：上层应用程序的活动空间，应用程序的执行必须依托于内核提供的资源。
系统调用：为了使上层应用能够访问到这些资源，内核为上层应用提供访问的接口。










## network


app layer: DNS, FTP, HTTP

transport layer: TCP/ UDP

network layer: IP

datalink

- ARP 
ARP 协议的工作原理: 即地址解析协议， 用于实现从 IP 地址到 MAC 地址的映射，即询问目标IP对应的MAC地址。 https://zhuanlan.zhihu.com/p/28771785





- 三次握手，四次挥手
https://juejin.im/post/5d9c284b518825095879e7a5

	半连接队列
半断开队列

	SYN, ACK

	2MSL


	closewait
	timewait





- TCP 协议是如何保证可靠传输的
1、确认和重传：接收方收到报文就会确认，发送方发送一段时间后没有收到确认就重传。
2、数据校验: 若校验出包有错，则丢弃报文段并且不给出响应，这时 TCP 发送数据端超时后会重发数据
3、数据合理分片和排序：tcp会按MTU合理分片，接收方会缓存未按序到达的数据，重新排序后再交给应用层。
4、流量控制：当接收方来不及处理发送方的数据，能提示发送方降低发送的速率，防止包丢失。  滑动窗口
5、拥塞控制：当网络拥塞时，减少数据的发送。




- ARQ
https://blog.csdn.net/guoweimelon/article/details/50879588

	自动重传请求 ARQ 协议
停止等待协议中超时重传是指只要超过一段时间仍然没有收到确认，就重传前面发送过的分组（认为刚才发送过的分组丢失了）。因此每发送完一个分组需要设置一个超时计时器，其重传时间应比数据在分组传输的平均往返时间更长一些。这种自动重传方式常称为自动重传请求 ARQ。

	连续 ARQ 协议
连续 ARQ 协议可提高信道利用率。发送方维持一个发送窗口，凡位于发送窗口内的分组可以连续发送出去，而不需要等待对方确认。接收方一般采用累计确认，对按序到达的最后一个分组发送确认，表明到这个分组为止的所有分组都已经正确收到了。




- 滑动窗口
https://coolshell.cn/articles/11609.html

	发送窗口中有四个概念：：已发送并收到确认的数据（不在发送窗口和发送缓冲区之内）、已发送但未收到确认的数据（位于发送窗口之内）、允许发送但尚未发送的数据（位于发送窗口之内）、发送窗口之外的缓冲区内暂时不允许发送的数据。

	接收窗口中也有四个概念：已发送确认并交付主机的数据（不在接收窗口和接收缓冲区之内）、未按序收到的数据（位于接收窗口之内）、允许的数据（位于接收窗口之内）、不允许接收的数据（位于发送窗口之内）。






RTT——Round Trip Time，也就是一个数据包从发出去到回来的时间

- 拥塞控制

	慢开始算法的思路是当主机开始发送数据时，如果立即把大量数据字节注入到网络，那么可能会引起网络阻塞，因为现在还不知道网络的符合情况。经验表明，较好的方法是先探测一下，即由小到大逐渐增大发送窗口，也就是由小到大逐渐增大拥塞窗口数值。cwnd 初始值为 1，每经过一个传播轮次，cwnd 加倍。


	拥塞避免算法的思路是让拥塞窗口 cwnd 缓慢增大，即每经过一个往返时间 RTT 就把发送方的 cwnd 加 1。



	快重传与快恢复： 没有 FRR，如果数据包丢失了，TCP 将会使用定时器来要求传输暂停。在暂停的这段时间内，没有新的或复制的数据包被发送。有了 FRR，如果接收机接收到一个不按顺序的数据段，它会立即给发送机发送一个重复确认。如果发送机接收到三个重复确认，它会假定确认件指出的数据段丢失了，并立即重传这些丢失的数据段。





- 粘包
基于上面两点，在使用 TCP 传输数据时，才有粘包或者拆包现象发生的可能。一个数据包中包含了发送端发送的两个数据包的信息，这种现象即为粘包。
UDP不会


	发送方产生粘包：但当发送的数据包过于的小时，那么 TCP 协议默认的会启用 Nagle 算法，将这些较小的数据包进行合并发送（缓冲区数据发送是一个堆压的过程）；这个合并过程就是在发送缓冲区中进行的，也就是说数据发送出来它已经是粘包的状态了。

	接收方产生粘包：放>读



	分包机制一般有两个通用的解决方法：
1. 特殊字符控制；
2. 在包头首都添加数据包的长度。



- HTTP  HTTPS
https://juejin.im/entry/58d7635e5c497d0057fae036

	HTTP协议以明文方式发送内容，不提供任何方式的数据加密，如果攻击者截取了Web浏览器和网站服务器之间的传输报文，就可以直接读懂其中的信息，因此，HTTP协议不适合传输一些敏感信息，比如：信用卡号、密码等支付信息。

	为了解决HTTP协议的这一缺陷，需要使用另一种协议：安全套接字层超文本传输协议HTTPS，为了数据传输的安全，HTTPS在HTTP的基础上加入了SSL协议，SSL依靠证书来验证服务器的身份，并为浏览器和服务器之间的通信加密。



- socket

```
client                           server

create socket                 create socket

                               bind

                               listen

connect 
(three-way hs)               半连接队列，全连接队列
 			
 							  accept   阻塞     返回一个新的对应于此次连接的套接字（accept()）;

read/write                     read/write

 
close 						   close
```






- udp客户端使用connect()函数
先指明目的地址/端口，然后就可以使用send()函数向目的地址发送数据了(取代 sendto())


- udp客户端程序使用bind()函数
如果服务器程序就绪后一上来就要发送数据给客户端，那么服务器就需要知道客户端的地址信息和端口，那么就不能让客户端的地址信息和端口号由客户端所在操作系统分配，而是要在客户端程序指定了。怎么指定，那就是用bind()函数


- udp服务器程序使用connect()函数
connect()函数可以用来指明套接字的目的地址/端口号，那么若udp服务器可以使用connect，将导致服务器只接受这特定一个主机的请求。



//https://www.cnblogs.com/haichun/p/3519232.html
recv和send函数提供了和read和write差不多的功能.但是他们提供 了第四个参数来控制读写操作.
int recv(int sockfd,void *buf,int len,int flags)
int send(int sockfd,void *buf,int len,int flags)







## database

- 1NF 
1NF:字段不可分 强调的是列的原子性，即列不能够再分成其他几列
2NF: 一是表必须有一个主键；二是没有包含在主键中的列必须完全依赖于主键。      学号, 课程名称,   姓名, 成绩, 学分
3NF:非主键字段不能相互依赖  //首先是 2NF，另外非主键列必须直接依赖于主键，不能存在传递依赖  
(选课人 课程)  上课老师 老师职称
BCNF：主键列之间，不存在依赖。





- 聚集索引： 数据行的物理顺序与列值（一般是主键的那一列）的逻辑顺序相同    叶节点是数据节点
非聚集


	二次查询
复合索引
最左匹配(最左前缀)原则: https://juejin.im/post/5da53e04e51d45782f663c04





- 事务
并发控制的基本单位。所谓的事务是指逻辑上的一组操作，组成这组操作的各个功能，要么全部成功，要么全部都不成功。事务可以是一条SQL语句，一组SQL语句或者整个程序。事物的提出是为了解决并发情况下保持数据一致性问题。



	Atomicity 原子性
指事务是一个不可分割的操作单位，事务中的操作要么全部成功，要么全部不成功
undo log(回滚日志)来保证原子性操作。当事务执行失败或者调用了 rollback 方法时，就会触发回滚事件，利用 undo log 中记录将数据回滚到修改之前的样子


	Consistency 一致性
必须使数据库从一个一致性状态装换到另一个一致性状态
如转账前A+B为2000，A像B转账100，完成后，A和B总和仍然为2000元
利用数据库的一些特性来保证部分一致性需求
绝大部分还是需要我们程序员在编写业务代码得时候来保证。



	Isolation 隔离性
多个用户并发访问数据库时，数据库为每一个用户开启的事务，不能被其他事务的操作数据所影响


	Durability 持久性
一个事务一旦被提交。对数据库的改变就是永久性的，即使数据库发生故障也不会造成影响
InnoDB 引擎引入了一个中间层来解决这个持久性的问题，我们把这个叫做 redo log(归档日志)。
当有一条记录需要更新的时候，InnoDB 引擎就会先把记录写到 redo log 里面，并更新内存，这个时候更新就算完成了。





	脏读：读取了另一个事务未提交的数据

	不可重复读：多次读取同一行，多次读取结果不同，读取了另一个事务提交的数据

	幻读：其他事物插入了新记录，导致前后读取不一致、


	读取未提交 READ UNCOMMITED   最低级别，无法解决问题。完全不隔离
	读取已提交 READ COMMITED    大多数数据库默认级别   解决脏读
	可重读     REPEATABLE READ  Mysql默认级别    解决脏读，不可重复读
	可串行化   SERIALIZABLE      全部解决，但效率极低，一般不用 带来超时和锁竞争现象


- 数据库索引的作用和优点缺点
https://blog.csdn.net/zhanghaotian2011/article/details/8904333


- EXISTS 和 IN 的区别
IN   n^2    子表较小，内存
EXISTS   n  子表较大，db






- 存储过程
一些预编译的SQL语句。
然而存储过程是一个编译过的代码块，所以执行效率要比T-SQL语句高。
在网络中交互时可以替代大堆的T-SQL语句，所以也能降低网络的通信量，提高通信速率







## sorting algorithms
选择排序、快速排序、希尔排序、堆排序不是稳定的排序算法，


- heap sort   （选择排序）
数组代表堆， parent = (cur-1)/2, lchild = cur*2+1, rchild = cur*2+2
构建最大堆，
swap(arr[0], arr[i-1])对非叶结点最大堆处理  // 每次将最大值放到最后

nlogn, nlogn, nlogn, 1  //best worst, avg, space
//9, 5a, 7, 5b unstable
https://www.runoob.com/w3cnote/heap-sort.html



- bubble sort
n, n^2 , n^2, 1


- insert sort
n, n^2, n^2, 1


- select sort
n^2, n^2, n^2, 1    // 5,5,2 unstable


- quick sort
nlogn, n^2, nlogn, logn(递归树的深度)
5  2 3   9 8 9   4

- merge sort
nlogn, nlogn, nlogn, n


- 桶排序
https://blog.csdn.net/liaoshengshi/article/details/47320023
N






## 设计模式
https://www.jianshu.com/p/5efb80654a1c

- 简单工厂:

- 工厂方法:  factory 细分为 concretfactory, 负责不同的产品 

- 抽象工厂： factory 细分为 concretfactory, 同时product1, product2也细分为A1, A2, B1, B2.

- 建造者模式： builder 分为 fatBuilder,thinBuilder. director 传入一个fatBuilder指针，再create()里build part

- 单例模式： private: m_instance, 构造函数，拷贝构造。 public : static sinleton* getInstance
线程安全
https://www.cnblogs.com/qiaoconglovelife/p/5851163.html



- 观察者模式：https://www.jianshu.com/p/4b0aee15cdb8




```
水塘抽样
https://blog.csdn.net/javastart/article/details/50610868


n个抽k个

reserve k element,
for i = k+1,...

p = random(0,i)
if(p<k) result[p] = stream[i]


return result
```



```
quicksort(vector<> vec)
{

	if(low < high)
	{
		int pivot = findp();

		quicksort(low, pivot-1);

		quicksort(pivot+1, high);
	}


}

findp(vec, low, high)
{

	pivot = vec[high];
	int i = low-1;

	for(int j=low;j<high;j++)
	{
		if(vec[j]<pivot)
		{
  			i++;
  			swap([i], [j]);
		}
	}

    i++;
    swap([i], [high]);
    return i;
}
```
