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

#### 9、原子（atomic）和非原子（nonatomic）属性有什么区别？
* 1). atomic提供多线程安全。是防止在写未完成的时候被另外一个线程读取，造成数据错误
* 2). nonatomic:在自己管理内存的环境中，解析的访问器保留并自动释放返回的值，如果指定了nonatomic ，那么访问器只是简单地返回这个值。

#### 10、看下面的程序,第一个NSLog会输出什么?这时str的retainCount是多少?第二个和第三个呢? 为什么?
* NSMutableArray* ary = [[NSMutableArray array] retain];
 NSString *str = [NSString stringWithFormat:@\"test\"];
  [str retain];
 
[aryaddObject:str]; [str retain];
 [str release];
 [str release];  [aryremoveAllObjects];  str的retainCount创建+1，retain+1，加入数组自动+1 3 retain+1，release-1，release-1 2数组删除所有对象，所有数组内的对象自动-1 1
#### 11、内存管理的几条原则时什么?按照默认法则。哪些关键字生成的对象需要手动释放?在和property结合的时候怎样有效的避免内存泄露?
* 谁申请，谁释放遵循Cocoa Touch的使用原则;内存管理主要要避免“过早释放”和“内存泄漏”，对于“过早释放”需要注意@property设置特性时，一定要用对特性关键字，对于“内存泄漏”，一定要申请了要负责释放，要细心。关键字alloc 或new 生成的对象需要手动释放;设置正确的property属性，对于retain需要在合适的地方释放

#### 12、如何对iOS设备进行性能测试?
* Profile-> Instruments ->Time Profiler

#### 13、Object-C中创建线程的方法有什么?如果在主线程中执行代码，方法是什么?如果想延时执行代码的方法又是什么?
* 线程创建有三种方法：使用NSThread创建、使用GCD的dispatch、使用子类化的NSOperation,然后将其加入NSOperationQueue;在主线程执行代码，方法是performSelectorOnMainThread，如果想延时执行代码可以用performSelector:onThread:withObject:waitUntilDone:

#### 14、MVC设计模式是什么？你还熟悉什么设计模式？
* 设计模式：并不是一种新技术，而是一种编码经验，使用比如java中的接口，iphone中的协议，继承关系等基本手段，用比较成熟的逻辑去处理某一种类型的事情，总结为所谓设计模式。面向对象编程中，java已经归纳了23种设计模式。
* mvc设计模式 ：模型，视图，控制器，可以将整个应用程序在思想上分成三大块，对应是的数据的存储或处理，前台的显示，业务逻辑的控制。 Iphone本身的设计思想就是遵循mvc设计模式。其不属于23种设计模式范畴。
* 代理模式：代理模式给某一个对象提供一个代理对象，并由代理对象控制对源对象的引用.比如一个工厂生产了产品，并不想直接卖给用户，而是搞了很多代理商，用户可以直接找代理商买东西，代理商从工厂进货.常见的如QQ的自动回复就属于代理拦截，代理模式在iphone中得到广泛应用.
* 单例模式：说白了就是一个类不通过alloc方式创建对象，而是用一个静态方法返回这个类的对象。系统只需要拥有一个的全局对象，这样有利于我们协调系统整体的行为，比如想获得[UIApplication sharedApplication];任何地方调用都可以得到 UIApplication的对象，这个对象是全局唯一的。
* 观察者模式： 当一个物体发生变化时，会通知所有观察这个物体的观察者让其做出反应。实现起来无非就是把所有观察者的对象给这个物体，当这个物体的发生改变，就会调用遍历所有观察者的对象调用观察者的方法从而达到通知观察者的目的。
* 工厂模式：public class Factory{

  public static Sample creator(int which)
  
  {
  
   if (which==1)return new SampleA();
   
   else if (which==2)return new SampleB();
   
   }
   
   }
   
#### 15、浅复制和深复制的区别?
* 浅层复制：只复制指向对象的指针，而不复制引用对象本身。深层复制：复制引用对象本身。意思就是说我有个A对象，复制一份后得到A_copy对象后，对于浅复制来说，A和A_copy指向的是同一个内存资源，复制的只不过是是一个指针，对象本身资源还是只有一份，那如果我们对A_copy执行了修改操作,那么发现A引用的对象同样被修改，这其实违背了我们复制拷贝的一个思想。深复制就好理解了,内存中存在了两份独立对象本身。用网上一哥们通俗的话将就是：浅复制好比你和你的影子，你完蛋，你的影子也完蛋;深复制好比你和你的克隆人，你完蛋，你的克隆人还活着。

#### 16、类别的作用?继承和类别在实现中有何区别?
* category 可以在不获悉，不改变原来代码的情况下往里面添加新的方法，只能添加，不能删除修改，并且如果类别和原来类中的方法产生名称冲突，则类别将覆盖原来的方法，因为类别具有更高的优先级。类别主要有3个作用： 
* 1).将类的实现分散到多个不同文件或多个不同框架中。
* 2).创建对私有方法的前向引用。
* 3).向对象添加非正式协议。继承可以增加，修改或者删除方法，并且可以增加属性。

#### 17、类别和类扩展的区别？
* category和extensions的不同在于后者可以添加属性。另外后者添加的方法是必须要实现的。extensions可以认为是一个私有的Category。

#### 18、OC中的协议和java中的接口概念有何不同?
* OC中的代理有2层含义，官方定义为formal和informal protocol。前者和Java接口一样。informal protocol中的方法属于设计模式考虑范畴，不是必须实现的，但是如果有实现，就会改变类的属性。其实关于正式协议，类别和非正式协议我很早前学习的时候大致看过，也写在了学习教程里 “非正式协议概念其实就是类别的另一种表达方式“这里有一些你可能希望实现的方法，你可以使用他们更好的完成工作”。这个意思是，这些是可选的。比如我门要一个更好的方法，我们就会申明一个这样的类别去实现。然后你在后期可以直接使用这些更好的方法。这么看，总觉得类别这玩意儿有点像协议的可选协议。”现在来看，其实protocal已经开始对两者都统一和规范起来操作，因为资料中说“非正式协议使用interface修饰“，现在我们看到协议中两个修饰词：“必须实现(@requied)”和“可选实现(@optional)”。

#### 19、什么是KVO和KVC?
* KVC:键 – 值编码是一种间接访问对象的属性使用字符串来标识属性，而不是通过调用存取方法，直接或通过实例变量访问的机制。很多情况下可以简化程序代码。apple文档其实给了一个很好的例子。
* KVO:键值观察机制，他提供了观察某一属性变化的方法，极大的简化了代码。具体用看到嗯哼用到过的一个地方是对于按钮点击变化状态的的监控。比如我自定义的一个button [self addObserver:self forKeyPath:@"highlighted" options:0 context:nil];#pragma mark KVO - (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary *)change context:(void *)context{if ([keyPath isEqualToString:@"highlighted"] ) {[self setNeedsDisplay];}}对于系统是根据keypath去取的到相应的值发生改变，理论上来说是和kvc机制的道理是一样的。对于kvc机制如何通过key寻找到value:“当通过KVC调用对象时，比如：[self valueForKey:@”someKey”]时，程序会自动试图通过几种不同的方式解析这个调用。首先查找对象是否带有 someKey 这个方法，如果没找到，会继续查找对象是否带有someKey这个实例变量(iVar)，如果还没有找到，程序会继续试图调用 -(id) valueForUndefinedKey:这个方法。
* 如果这个方法还是没有被实现的话，程序会抛出一个NSUndefinedKeyException异常错误。 (cocoachina.com注：Key-Value Coding查找方法的时候，不仅仅会查找someKey这个方法，还会查找getsomeKey这个方法，前面加一个get，或者_someKey以及_getsomeKey这几种形式。同时，查找实例变量的时候也会不仅仅查找someKey这个变量，也会查找_someKey这个变量是否存在。)设计valueForUndefinedKey:方法的主要目的是当你使用-(id)valueForKey方法从对象中请求值时，对象能够在错误发生前，有最后的机会响应这个请求。
* 这样做有很多好处，下面的两个例子说明了这样做的好处。“来至cocoa，这个说法应该挺有道理。因为我们知道button却是存在一个highlighted实例变量.因此为何上面我们只是add一个相关的keypath就行了，可以按照kvc查找的逻辑理解，就说的过去了。

#### 20、代理的作用？
* 代理的目的是改变或传递控制链。允许一个类在某些特定时刻通知到其他类，而不需要获取到那些类的指针。可以减少框架复杂度。另外一点，代理可以理解为java中的回调监听机制的一种类似。

#### 21、OC中可修改和不可以修改类型
* 可修改不可修改的集合类。这个我个人简单理解就是可动态添加修改和不可动态添加修改一样。比如NSArray和NSMutableArray。前者在初始化后的内存控件就是固定不可变的，后者可以添加等，可以动态申请新的内存空间。

