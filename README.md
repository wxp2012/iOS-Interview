# iOS-Interview

#### 注记：
     
       这是自己从一个收费的面试题应用上抓取到的数据，一共有两百多道，主要面向初级开发者，自己也做了相应的补充；虽然说自己掌握基本功是最重要的，但是多准备一下也不会有什么坏处，在此希望能给你们带来些许帮助。
       
#### 1、Object-C的类可以多重继承么？可以实现多个接口么？Category是什么？重写一个类的方式用继承好还是分类好？为什么？
* 不能够实现多继承，这里延伸出来就是C++的知识，C++是支持多重继承的
* 可以实现多个接口，通过实现多个接口可以完成类似于C++的多重继承功能
* Category是分类，或者说是类别
* 用分类好，因为对分类去扩展实现方法，并不会对其他类与原有类的关系

#### 2、"#import跟#include有什么区别，@class呢,#import<> 跟 #import”又有什么区别?"
* import是Objective-C导入头文件的关键字，#include是c/c++导入头文件的关键字，使用#import头文件会自动导入一次，不会重复导入，相当于#include和#pragma once；@class告诉编译器某个类的声明，当执行时，才去查看类的实现文件，可以解决头文件的相互包含；#import<>用来包含系统的头文件，#import“”用来包含用户头文件。

#### 3、属性readwrite、readonly、assign、retain、copy、nonatomic各是什么作用，在哪种情况下用？
* readwrite是可读可写属性，需要生成getter方法和setter方法时使用
* readonly是只读属性，只会生成getter方法，不会生成setter方法，当不希望属性在外部改变时使用
* assign是赋值属性，setter方法将传入参数赋值给实例再赋值，传入参数retaincount会+1
* copy表示赋值特性，setter方法将传入对象赋值一份，需要生成一份新的变量时使用
* nonatomic表示非原子操作，决定编译器生成的setter、getter是否是原子操作，atomic表示原子特性，表示多线程安全，通常在Mac开发中会使用，而nonatomic是在iOS开发中使用

#### 4、写一个setter方法用于完成@property（nonatomic，retain）NSString *name，写一个setter方法用于完成@property（nonatomic，copy）NSString *name
* —（void）setName:(NSString *)str 
  
  {  
     [str retain];
     
     [name release];
     
     name = str;
     
     }
* — (void) setName:(NSString *)str

  {
  
     id t = [str copy];
     
     [name release];
     
     name = t;
     
   }
   
#### 5、对于语句 NNString *obj = [[NSData alloc] init]; obj在编译时和运行时分别是什么类型的对象？
* 编译时是NSString的类型；运行时是NSData类型的对象

#### 6、常见的object-c的数据类型有哪些，和C的基本数据类型有什么区别？如：NSInteger和Int
* Object-C的数据类型有NSString，NSNumber，NSArray，NSMutableArray，NSData等等，这些都是类(class),创建后便是对象，而C语言的基本数据类型Int，只是拥有字节的内存空间，用于存放数值；NSInteger是基本数据类型，并不是NSNumber的子类，当然也不是NSObject的子类。NSInteger是基本数据类型Int或者long的别名(NSInteger的定义typedef long NSInteger)，它的区别在于，NSInteger会根据系统是32位还是64位来决定是本身Int还是long。

#### 7、id 声明的对象有什么特性？
* id声明的对象具有运行时的特性，即可以指向任意类型的objective-c的对象。

#### 8、Objective-C如何对内存管理的，说说你的看法和解决方法？
* Objective-C 的内存管理主要有三种方式ARC(自动引用计数)、手动内存计数、内存池。
* 1). ARC(自动引用计数)：这种方式和java类似，在你的程序的执行过程中。始终有一个高人在背后准确地帮你收拾垃圾，你不用考虑它什么时候开始工作，怎样工作。你只需要明白，我申请了一段内存空间，当我不再使用从而这段内存成为垃圾的时候，我就彻底的把它忘记掉，反正那个高人会帮我收拾垃圾。遗憾的是，那个高人需要消耗一定的资源，在携带设备里面，资源是紧俏商品所以iPhone不支持这个功能。那么在自动引用计数下就没有内存泄漏问题了？这是完全错误的观点，在ARC下也还是会出现内存泄漏的问题。     解决: 通过alloc – initial方式创建的, 创建后引用计数+1, 此后每retain一次引用计数+1, 那么在程序中做相应次数的release就好了.
* 2). MRC(手动内存计数)：就是说，从一段内存被申请之后，就存在一个变量用于保存这段内存被使用的次数，我们暂时把它称为计数器，当计数器变为0的时候，那么就是释放这段内存的时候。比如说，当在程序A里面一段内存被成功申请完成之后，那么这个计数器就从0变成1(我们把这个过程叫做alloc)，然后程序B也需要使用这个内存，那么计数器就从1变成了2(我们把这个过程叫做retain)。紧接着程序A不再需要这段内存了，那么程序A就把这个计数器减1(我们把这个过程叫做release);程序B也不再需要这段内存的时候，那么也把计数器减1(这个过程还是release)。当系统(也就是Foundation)发现这个计数器变 成员了0，那么就会调用内存回收程序把这段内存回收(我们把这个过程叫做dealloc)。顺便提一句，如果没有Foundation，那么维护计数器，释放内存等等工作需要你手工来完成。 
解决:一般是由类的静态方法创建的, 函数名中不会出现alloc或init字样, 如[NSString string]和[NSArray arrayWithObject:], 创建后引用计数+0, 在函数出栈后释放, 即相当于一个栈上的局部变量. 当然也可以通过retain延长对象的生存期.
* 3). (NSAutoRealeasePool)内存池：可以通过创建和释放内存池控制内存申请和回收的时机.解决:是由autorelease加入系统内存池, 内存池是可以嵌套的, 每个内存池都需要有一个创建释放对, 就像main函数中写的一样. 使用也很简单, 比如[[[NSString alloc]initialWithFormat:@”Hey you!”] autorelease], 即将一个NSString对象加入到最内层的系统内存池, 当我们释放这个内存池时, 其中的对象都会被释放."

#### 9、原子（atomic）和非原子（noatomic）属性有什么区别？
* 

