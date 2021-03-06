# JVM

1. Java一次编译，到处允许，因为在不同的操作系统上有不同版本的JVM，所以同一份字节码文件，可以跨平台运行。

2. 而JVM是跨语言的平台，支持多种语言的字节码文件，例如JavaScript，Scala，Groovy等等

## 一、简介

### 虚拟机

虚拟机是一台虚拟的计算机，它是一款软件，用来执行一系列虚拟计算机指令，大体上可以分为系统虚拟机和程序虚拟机。

1. 系统虚拟机：VMware，Visual Box等，它们完全是对物理计算机的仿真，提供了一个可运行完整操作系统的软件平台。
2. 程序虚拟机：Java虚拟机，它专门为执行单个计算机程序而设计，在Java虚拟机中执行的指令我们称为Java字节码指令。

### Java虚拟机

1. 就是二进制字节码的运行环境，负责装载字节码到其内部，解释/编译为对应平台上的机器指令执行，
2. 每一条Java指令，Java虚拟机规范中都有详细定义，如怎么取操作数，怎么处理操作数，处理结果放在哪。
3. 特点：
   1. 一次编译，到处运行
   2. 自动内存管理
   3. 自动垃圾回收功能

### JVM的架构模型

1. 基于栈的指令集架构
   1. 设计和实现更简单，适用于资源受限的系统
   2. 避开了寄存器的分配难题，使用零地址指令方式分配
   3. 指令流中的指令大部分是零地址指令，执行过程中依赖于操作站，指令集更小，编译器容易实现
   4. 不需要硬件支持，可移植性更好，更好的实现跨平台
2. 基于寄存器的指令集架构
   1. 典型的应用是x86的二进制指令集，比如传统的PC以及Android的Davlik虚拟机
   2. 指令集架构则完全依赖硬件，可移植性差
   3. 性能优秀和执行更高效
   4. 花费更少的指令去完成一项操作
   5. 在大部分情况下，基于寄存器架构的指令集往往都以一地址指令，二地址指令和三地址指令为主，而基于栈式架构的指令集却是以零地址指令为主。

3. 总结：

   由于跨平台性的设计，Java的指令集都是根据栈来设计的，不同平台CPU架构不同，所以不能设计为基于寄存器的，优点是跨平台，指令集小，编译容易实现，缺点是性能下降，实现同样的功能需要更多的指令。

   （指令集小，实现相同的功能需要的指令多，执行性能比寄存器差）

### JVM的生命周期

1. 虚拟机的启动

   Java虚拟机的启动是通过引导类加载器（bootstrap class loader）创建一个初始类（initial class）来完成的，这个类是由虚拟机的具体实现指定的。

2. 虚拟机的执行

   一个运行中的Java虚拟机有着一个清晰的任务，执行Java程序。

   程序开始执行时它才运行，程序结束时他就停止。

   执行一个所谓的Java程序的时候，真正执行的是一个叫做Java虚拟机的进程。

3. 虚拟机的退出
   1. 程序的正常执行结束
   2. 程序在执行过程中遇到了异常或错误而异常终止
   3. 由于操作系统出现错误而导致Java虚拟机进程终止
   4. 某线程调用Runtime类或System类的exit方法，或Runtime类的halt方法，并且Java安全管理器也允许这次exit或halt操作
   5. JNI ( Java Native Interface ) 规范描述了用JNI Invocation API来加载或卸载 Java 虚拟机时，Java 虚拟机退出的情况。

### JVM 发展历程

1. Sun Classic VM

   世界上第一款商用Java虚拟机

   只提供解释器 (逐行解释字节码)，不提供JIT（即时编译器）

2. Exact VM

   自 jdk1.2

   准确式内存管理，可以知道内存中某个位置的数据具体是什么类型。 

   支持热点探测，编译器和解释器混合工作模式

3. HotSpot VM

   从服务器，桌面到移动端，嵌入式都有应用

   名称中的HotSpot指的就是它的热点代码探测技术

   通过计数器找到最具编译价值代码，触发即时编译或栈上替换

   通过编译器与解释器协同工作，在最优化的程序响应时间与最佳执行性能中取得平衡

4. BEA 的 JRockit
   1. 专注于服务器端应用。不关心程序的启动速度，因此内部不包含解析器的实现，全部代码都靠即时编译器编译后执行。
   2. 是世界上最快的JVM
   3. 优势：全面的Java运行时解决方案组合。

5. IBM 的 J9
   1. 市场定位与HotSpot接近，服务器端，桌面应用，嵌入式等多用途VM
   2. 广泛应用于IBM的各种Java产品

## 二、类加载子系统

### 类加载过程

1. 加载：
   1. 通过一个类的全限定名获取定义此类的二进制字节流
   2. 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构
   3. 在内存中生成一个代表这个类的 java.lang.Class 对象，作为方法区这个类的各种数据的访问入口

2. 验证
   1. 目的在于确保Class文件的字节流中包含信息符合当前虚拟机要求，保证被加载类的正确性，不会危害虚拟机的自身安全。
   2. 主要包括四种验证，文件格式验证，元数据验证，字节码验证，符号引用验证

3. 准备
   1. 为类变量分配内存并且设置该类变量的默认初始值，即零值。
   2. 这里不包含用final修饰的static，因为final在编译的时候就会分配了，准备阶段会显式初始化
   3. 这里不会为实例变量分配初始化，类变量会分配在方法区中，而实例变量是会随着对象一起分配到Java堆中。

4. 解析
   1. 将常量池内的符号引用转换为直接引用的过程
   2. 解析操作往往会伴随着JVM在执行完初始化之后再执行
   3. 符号引用就是一组符号来描述所引用的目标，符号引用的字面量形式明确定义再《java虚拟机规范》的class 文件格式中，直接引用就是直接指向目标的指针，相对偏移量或一个间接定位到目标的句柄。
   4. 解析动作主要针对类或接口，字段，类方法，接口方法，方法类型等，对应常量池中的 CONSTANT_Class_info，CONSTANT_Fieldref_info，CONSTANT_Methodref_info等。

5. 初始化
   1. 初始化阶段就是执行类构造器方法<clinit>() 的过程。
   2. 此方法不需要定义，是javac 编译器自动收集类中的所有类变量的赋值动作和静态代码块中的语句合并而来。
   3. 构造器方法中指令按语句在源文件中出现的顺序执行
   4. <clinit>() 不同于类的构造器
   5. 若该类具有父类，JVM会保证子类的<clinit>() 执行前，父类的<clinit>() 已经执行完毕。
   6. 虚拟机必须保证一个类的<clinit>() 方法在多线程下被同步加锁。

### 类加载器的分类

1. JVM支持两种类加载器，分别为引导类加载器（Bootstrap ClassLoader）和自定义类加载器（User-Defined ClassLoader）
2. 所有派生于抽象类ClassLoader的类加载器叫做自定义类加载器。
3. 常用类加载器：
   1. Bootstrap ClassLoader
   2. Extension ClassLoader
   3. System ClassLoader
   4. Optional （User Defined ClassLoader）

（四者为包含关系）

### 虚拟机自带的加载器

1. 启动类加载器（引导类加载器， Bootstrap ClassLoader）
   1. 这个类加载使用C/C++语言实现的，嵌套在JVM内部
   2. 它用来加载Java的核心库（JAVA_HOME/jre/lib/rt.jar、resources.jar或sun.boot.class.path路径下的内容）用于提供JVM自身需要的类
   3. 并不继承自java.lang.ClassLoader，没有父加载器
   4. 加载扩展类和应用程序类加载器，并指定为它们的父类加载器
   5. 出于安全考虑，Bootstrap启动类加载器只加载包名为java, javax, sun 等开头的类

2. 扩展类加载器（Extension ClassLoader）
   1. Java语言编写，由sun.misc.Launcher$ExtClassLoader实现
   2. 派生于ClassLoader类
   3. 父加载器为启动类加载器
   4. 从java.ext.dirs系统属性所指定的目录中加载类库，或从JDK的安装目录的jre/lib/ext子目录（扩展目录）下加载类库，如果用户创建的JAR放在此目录下，也会自动由扩展类加载器加载

3. 应用程序类加载器（系统类加载器，AppClassLoader）
   1. java语言编写，由sun.misc.Launcher$AppClassLoader实现
   2. 派生于ClassLoader类
   3. 父类加载器为扩展类加载器
   4. 它负责加载环境变量classpath或系统属性 java.class.path 指定路径下的类库
   5. 该类加载器是程序中默认的类加载器，一般来说，Java应用的类都是由它来完成加载
   6. 通过ClassLoader#getSystemClassLoader()方法可以获取到该类加载器

### 使用自定义类加载器的原因

1. 隔离加载类
2. 修改类加载的方式
3. 扩展加载源
4. 防止源码泄漏

### 用户自定义类加载器实现步骤

1. 开发人员可以通过继承抽象类 java.lang.ClassLoader 类的方式，实现自己的类加载器，以满足一些特殊需求
2. 在JDK1.2之前，在自定义类加载器时，总会去继承ClassLoader 类并重写loadClass() 方法，从而实现自定义的类加载类，但是在JDK1.2之后，不再建议用户去覆盖loadClass() 方法，而是建议把自定义的类加载逻辑写在findClass() 方法中。
3. 在编写自定义类加载器时，如果没有太过于复杂的需求，可以直接继承URLClassLoader类，这样就可以避免自己去编写findClass() 方法及其获取字节码流的方式，使自定义类加载器编写更加简洁。

### 双亲委派机制

1. 如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个请求委托给父类的加载器去执行

2. 如果父类加载器还存在其父类加载器，则进一步向上委托，依次递归，请求最终将到达顶层的启动类加载器。
3. 如果父类加载器可以完成类加载任务就，就成功返回，若父类加载器无法完成此加载任务，子加载器才会尝试自己去加载，这就是双亲委派模式。

### 优势

1. 避免类的重复加载
2. 保护程序安全，防止核心API被随意篡改

### 沙箱安全机制

自定义String类，但是在加载自定义String类的时候，会率先使用引导类加载器加载，而引导类加载器在加载的过程中，会先加载jdk自带的文件，（rt.jar包）报错信息说没有main方法，就是因为加载的是rt,jar包中的String类，这样可以保证堆java核心源代码的保护。

### 其他

在JVM中表示两个class对象是否为同一个类存在两个必要条件：

1. 类的完整类名必须一致，包括包名
2. 加载这个类的ClassLoader（指ClassLoader实例对象）必须相同

### 类的主动使用和被动使用

1. 主动使用：

   1. 创建类的实例

   2. 访问某个类或接口的静态变量，或者对该静态变量赋值

   3. 调用类的静态方法

   4. 反射

   5. 初始化一个类的子类

   6. Java虚拟机启动时被标明为启动类

   7. JDK 7 开始提供的动态语言支持

      java.lang.invoke.MethodHandle 实例的解析结果

      REF_getStatic、REF_putStatic、REF_invokeStatic句柄对应的类没有初始化，则初始化

2. 被动使用

   除了以上七种情况，其他使用Java类的方式都被看作是对类的被动使用，都不会导致类的初始化

## 三、运行时数据区

### 内存分配

1. 每个线程：独立包括程序计数器、栈、本地栈

2. 线程间共享：堆、堆外内存（永久代或元空间、代码缓存）

3. 每个JVM只有一个Runtime实例，即为运行时环境，相当于内存结构中的运行时环境

4. 线程是一个程序里的运行单元，JVM允许一个应用有多个线程并行执行。在Hotspot JVM里，每个线程都与操作系统的本地线程直接映射，当一个Java线程准备好执行以后，此时一个操作系统的本地线程也同时创建，Java线程执行终止后，本地线程也会回收。
5. 操作系统负责所有线程的安排调度到任何一个可用的CPU上，一旦本地线程初始化成功，它就会调用Java线程的run() 方法。

### 程序计数器（PC寄存器）

PC寄存器用来存储指向下一条指令的地址，也即将要执行的指令代码。由执行引擎读取下一条指令。

1. 特点：
   1. 他是一块很小的内存空间，几乎可以忽略不记，也是运行速度最快的存储区域。
   2. 在JVM规范中，每个线程都有它自己的程序计数器，是线程私有的，生命周期与线程的生命周期保持一致
   3. 任何时间一个线程都只有一个方法在执行，也就是所谓的当前方法，程序计数器会存储当前线程正在执行的Java方法的JVM指令地址，或者，如果是在执行Native方法，则是未指定值。
   4. 它是程序控制流的指示器，分支、循环、跳转、异常处理、线程回复等基础功能都需要依赖这个计数器来完成
   5. 字节码解释器工作时，就是通过改变这个计数器的值来选择下一条需要执行的字节码指令
   6. 它是唯一一个在Java虚拟机规范中没有规定任何OutOfMemoryError情况的区域

2. 作用：
   1. 因为CPU需要不停的切换各个线程，这时候切换回来以后，就需要知道接着从哪继续执行
   2. JVM的字节码解释器就需要通过改变PC寄存器的值来明确下一条应该执行什么样的字节码指令。

### 虚拟机栈

1. 背景

   由于跨平台性的设计，Java的指令都是根据栈来设计的，不同平台CPU架构不同，所以不能设计为基于寄存器的。

   优点是跨平台，指令集小，编译器容易实现，缺点是性能下降，实现同样的功能需要更多的指令。

2. 堆与栈

   栈是运行时单位，堆是存储的单位

   （栈解决程序的运行问题，即程序如何执行，或者说如何处理数据，堆解决的是数据存储的问题，即数据怎么放，放在哪）

3. 内容

   每个线程在创建时都会创建一个虚拟机栈，其内部保存一个个的栈帧，对应着一次次的Java方法调用。（是线程私有的）

4. 生命周期和线程一致

5. 作用：

   主管Java程序的运行，保存方法的局部变量（基本数据类型，对象的引用地址）、部分结果，并参与方法的调用和返回。

6. 特点：
   1. 栈是一种快速有效的分配方式，访问速度仅次于程序计数器
   2. JVM直接对Java栈的操作只有两个：
      1. 每个方法执行，伴随着进栈（入栈，压栈）
      2. 执行结束后出栈工作
   3. 对于栈来说不存在垃圾回收问题

### 栈相关的异常

Java虚拟机规范允许Java栈的大小是动态的或者是固定不变的

1. 如果是固定大小的虚拟机栈，每个线程的Java虚拟机栈容量可以再线程创建的时候独立选定，如果线程请求分配的栈容量超过Java虚拟机允许的最大容量，Java虚拟机将会抛出一个StackOverFlowError异常。
2. 如果Java虚拟机栈可以动态扩展，并且在尝试扩展的时候无法申请到足够的内存，或者在创建新的线程时没有足够的内存去创建对应的虚拟机栈，哪Java虚拟机将会抛出一个OutOfMemoryError异常。

### 栈中存储的内容

1. 每个线程都有自己的栈，栈中的数据都是以栈帧的格式存在
2. 在这个线程上正在执行的每个方法都各自对应一个栈帧
3. 栈帧是一个内存区域，是一个数据集，维系着方法执行过程中的各种数据信息

### 栈运行原理

1. JVM直接对Java栈的操作只有两个，就是对栈帧的压栈和出战
2. 在一条活动线程中，一个时间点上，只会有一个活动的栈帧，即只有当前正在执行的方法的栈帧是有效的，这个栈帧称为当前栈帧，与当前栈帧相对应的方法就是当前方法，定义这个方法的类就是当前类。
3. 执行引擎运行的所有字节码指令只针对当前栈帧进行操作
4. 如果该方法中调用了其他方法，对应的新的栈帧会被创建出来，放在栈的顶端
5. 不同线程中所包含的栈帧是不允许存在相互引用的，即不可能在一个栈帧之中引用另外一个线程的栈帧
6. 如果当前方法调用了其他方法，方法返回之际，当前栈帧会返回此方法执行结果给前一个栈帧，接着，虚拟机会丢弃当前栈帧，使得前一个栈帧重新称为当前栈帧
7. Java方法有两种返回函数的方式，一种是正常的函数返回，使用return指令，另外一种是抛出异常，不管使用哪种方式，都会导致栈帧被弹出。

### 栈帧的内部结构

1. 局部变量表
2. 操作数栈（表达式栈）
3. 动态链接（指向运行时常量池的方法引用）
4. 方法返回地址（方法正常退出或者异常退出的定义）
5. 一些附加信息

### 局部变量表

1. 也称局部变量数组或本地变量表
2. 定义为一个数字数组，主要用于存储方法参数和定义在方法体内的局部变量，这些数据类型包括各种基本数据类型、对象引用，以及returnAddress类型。
3. 由于局部变量表是建立在线程的栈上，是线程私有数据，因此不存在数据安全问题
4. 局部变量表所需的容量大小是在编译期确定下来的，并保存在方法的Code属性的maximum local variables数据项中。在方法运行期间是不会改变局部变量表的大小的。

### Slot

1. 参数值的存放总是在局部变量数组的index0开始，到数组长度-1的索引结束。
2. 局部变量表，最基本的存储单元是slot（变量槽）
3. 局部变量表中存放编译期可知的各种基本数据类型（8种），引用类型（reference），returnAddress类型的变量。
4. 在局部变量表里，32位以内的类型只占用一个slot（包括returnAddress类型），64位的类型（long和double）占用两个slot。
5. byte、short、char、boolean在存储前被转换为int
6. JVM会为局部变量表中的每一个slot都分配一个访问索引，通过这个索引即可成功访问到局部变量表中指定的局部变量
7. 当一个实例方法被调用的时候，它的方法参数和方法体内部定义的局部变量将会按照顺序被复制到局部变量表中的每一个Slot上
8. 如果需要访问局部变量表中的一个64bit的局部变量值时，只需要使用前一个索引即可。（比如：访问long或double类型变量）
9. 如果当前帧是由构造方法或者实例方法创建的，那么该对象引用this将会存放在index的0的slot处，其余的参数按照参数表顺序排序。
10. 栈帧中的局部变量表中的槽位是可以重用的，如果一个局部变量过了其作用域，那么在其作用域之后申明的新的局部变量就很有可能重用过期局部变量的槽位，从而达到节省资源的目的。

### 静态变量和局部变量的对比

1. 参数表分配完毕后，再根据方法体内定义的变量的顺序和作用域分配
2. 类变量表（静态成员变量）有两次初始化的机会，第一次是在“准备阶段”，执行系统初始化，对类变量设置零值，另一次是在“初始化”阶段，赋予程序员在代码中定义的初始值。
3. 和类变量初始化不同的是，局部变量表不存在系统初始化的过程，这意味着一旦定义了局部变量则必须人为的初始化，否则无法使用。

### 操作数栈

1. 每一个独立的栈帧中除了包含局部变量表以外，还包含一个后进先出的操作数栈，也可以称为表达式栈。
2. 操作数栈，在方法执行过程中，根据字节码指令，往栈中写入数据或提取数据，即入栈/出栈
3. 某些字节码指令将值压入操作数栈，其余的字节码指令将操作数取出栈，使用它们后再把结果压入栈
4. 比如：执行复制，交换，求和等操作

### 操作数栈作用

1. 主要用于保存过程的中间结果，同时作为计算过程中变量临时的存储空间

2. 操作数栈就是JVM执行引擎的一个工作区，当一个方法刚开始执行的时候，一个新的栈帧也会随之被创建出来，这个方法的操作数栈是空的。

3. 每一个操作数栈都会拥有一个明确的栈深度用于存储数值，其所需的最大深度再编译器就定义好了，保存在方法的Code属性中，为max_stack的值

4. 栈中任何一个元素都是可以任意的Java数据类型

   （32bit类型占用一个栈单位深度，64bit类型占用两个栈单位深度）

5. 操作数栈并非采用访问索引的方式来进行数据访问的，而是只能通过标准的入栈和出栈操作来完成一次数据访问。

6. 如果被调用的方法带有返回值的话，其返回值将会被压入当前栈帧的操作数栈中，并更新PC寄存器中下一条需要执行的字节码指令。

7. 操作数栈中元素的数据类型必须与字节码指令的序列严格匹配，这由编译器在编译期间进行验证，同时在类加载过程中的类检验阶段的数据流分析阶段要再次验证。

5. 另外，我们说Java虚拟机的解释引擎是基于栈的执行引擎，其中的栈指的是操作数栈。

### 栈顶缓存技术

将栈顶元素全部缓存在物理CPU的寄存器中，一次降低对内存的读写次数，提升执行引擎的执行效率

### 动态链接（指向运行时常量池的方法引用）

1. 每一个栈帧内部都包含一个指向运行时常量池中该栈帧所属方法的引用。包含这个引用的目的就是为了支持当前方法的代码能够实现动态链接，比如：invokedynamic指令
2. 在Java源文件被编译到字节码文件时，所有的变量和方法引用都作为符号引用保存在class文件的常量池里。比如：描述一个方法调用了另外的其他方法时，就是通过常量池中指向方法的符号引用来表示的，那么动态链接的作用就是为了将这些符号引用转换为调用方法的直接引用。
3. 常量池的作用，就是为了提供一些符号和常量，便于指令的识别。

### 方法的调用

在JVM中，将符号引用转换为调用方法的直接引用与方法的绑定机制相关。

1. 静态链接

   当一个字节码文件被装载进JVM内部时，如果被调用的目标方法在编译期可知，且运行期保持不变时，这种情况下将调用方法的符号引用转换为直接引用的过程称之为静态链接。

2. 动态链接

   如果被调用的方法在编译期无法被确定下来，也就是说，只能够在程序运行期将调用方法的符号引用转换为直接引用，由于这种引用转换过程具备动态性，因此也就被称之为动态链接。

对应的方法绑定机制为：早期绑定和晚期绑定。绑定是一个字段、方法或者类在符号引用被替换为直接引用的过程，这仅仅发生一次。

1. 早期绑定：

   被调用的目标方法如果在编译期可知，且运行期保持不变时，即可将这个方法与所属的类型进行绑定，这样一来，由于明确了被调用的目标方法究竟是哪一个，因此也就可以使用静态链接的方式将符号引用转换为直接引用

2. 晚期绑定

   如果被调用的方法在编译期无法确定虾类，只能在程序运行期根据实际的类型绑定相关的方法，这种绑定方式也就被称为晚期绑定。

面向对象的编程语言，因为具有多态的特性，因此也具备早期绑定和晚期绑定两种方式。

### 方法的调用

1. 虚方法

2. 非虚方法

   如果方法在编译期就确定了具体的调用版本，这个版本在运行时是不可变的，这样的方法称为非虚方法

   静态方法，私有方法，final方法，实例构造器，父类方法都是非虚方法

   其他方法称为虚方法

### 虚拟机的方法调用指令

1. 普通调用指令
   1. invokestatic: 调用静态方法，解析阶段确定唯一方法版本
   2. invokespecial: 调用<intit> 方法、私有及父类方法，解析阶段确定唯一方法版本
   3. invokevirtual: 调用所有虚方法
   4. invokeinterface: 调用接口方法
2. 动态调用指令
   5. invokedynamic: 动态解析处需要调用的方法，然后执行

前四条指令固化在虚拟机内部，方法的调用执行不可人为干预，而invokedynamic指令则支持由用户确定方法版本，其中invokestatic指令和invokespecial指令调用的方法称为非虚方法，其余的 (final修饰的除外) 称为虚方法。 

### 关于invokedynamic指令

1. JVM字节码指令集一直比较稳定，一直到Java7中才增加了一个invokedynamic指令，这是Java为了实现【动态类型语言】支持而作的一种改进。
2. 但是在Java7中并没有提供直接生成invokedynamic指令的方法，需要借助ASM这种底层字节码工具来产生invokedynamic指令。直到Java8的Lambda表达式的出现，invokedynamic指令的生成，在Java中才有了直接的生成方式。
3. Java7中增加的动态语言类型支持的本质是对Java虚拟机规范的修改，而不是对Java语言规则的修改，增加了虚拟机中的方法调用，最直接的受益者就是运行在Java平台的动态语言的编译器。

### 动态语言和静态语言

1. 区别在于对类型的检查是在编译器还是在运行期，满足前者的是静态类型语言，反之是动态类型语言
2. 静态类型语言是判断变量自身的类型信息；动态类型语言是判断变量值的类型信息，变量没有类型信息，变量值才有类型信息，这是动态语言的一个重要特征。

### 方法重写的本质

1. 找到操作数栈顶的第一个元素所执行的对象的实际类型，记作C
2. 如果在类型c中找到与常量中描述符合简单名称都相符的方法，则进行访问权限校验，如果通过则返回这个方法的直接引用，查找过程结束，如果不通过，则返回 java.lang.IllegalAccessError异常
3. 否则，按照继承关系从下往上依次对c的各个父类进行第2步的搜索和验证过程。
4. 如果始终没有找到合适的方法，则抛出 java.lang.AbstractMethodError异常。

### illegalAccessError介绍：

程序试图访问或修改一个属性或调用一个方法，这个属性或方法，你没有权限访问，一般会引起编译器异常，这个错误如果发生在运行时，就说明一个类发生了不兼容的改变。

### 虚方法表

1. 在面向对象的编程中，会很频繁的使用到动态分派，如果在每次动态分派的过程中都重新在类的方法元数据中搜索合适的目标的话，就可能影响到执行效率，因此为了提高性能，JVM采用在类的方法区建立一个虚方法表（virtual method table）（非虚方法不会出现在表中）来实现。使用索引表来代替查找。
2. 每个类中都拥有一个虚方法表，表中存放着各个方法的实际入口。
3. 虚方法会在类加载的链接阶段被创建并开始初始化，类的变量初始值准备完成之后，JVM会把该类的方法表也初始化完毕。

### 方法返回地址

1. 存放调用该方法的pc寄存器的值
2. 一个方法的结束，有两种方式：
   1. 正常执行完成
   2. 出现未处理的异常，非正常退出

3. 无论通过哪种方式退出，在方法退出后都返回到该方法被调用的位置。方法正常退出时，调用者的pc计数器的值作为返回地址，即调用该方法的指令下一条指令的地址。而通过异常退出的，返回地址是要通过异常表来确定，栈帧中一般不会保存这部分信息。
4. 区别在于：通过异常完成出口退出的不会给它的上层调用者产生任何的返回值。

## 本地方法

native method 就是一个 Java调用非Java代码的接口，本地方法的实现有非Java语言实现，比如C。这个特征并非Java所特有的，很多其他的编程语言都有这一机制，比如在C++中，你可以用extern “C” 告知C++编译器去调用一个C的函数。

在定义一个native method时，并不提供实现体（有些像定义一个Java interface)，因为其实现体是由非java语言在外面实现的。

本地接口的作用是融合不同的编程语言为Java所用，它的初衷是融合 C/C++程序。

### 使用Native Method的原因

1. 有时Java应用需要与Java外面的环境交互，这是本地方法存在的主要原因。Java需要与一些底层系统，如操作系统或某些硬件交换信息时的情况，本地方法正是这样一种交流机制：它为我们提供了一个非常简洁的接口，而且我们无需去了解Java应用之外的繁琐细节。
2. JVM支持着Java语言本身和运行时库，它是Java程序赖以生存的平台，它由一个解释器（解释字节码）和一些连接到本地代码的库组成，然而不管怎么样，他毕竟不是一个完整的系统，它经常依赖于一些底层系统的支持，这些底层系统常常是强大的操作系统。通过使用本地方法，我们得以用Java实现了jre的与底层系统的交互，甚至JVM的一些部分就是用C写的。还有，如果我们要使用一些Java语言本身没有提供封装的操作系统的特性时，我们也需要使用本地方法。
3. Sun的解释器是用C实现的，这使得它能像一些普通的C一样与外部交互。jre大部分是用Java实现的，它也通过一些本地方法与外界交互。例如：类java.lang.Thread的setPriority()方法使用Java实现的，但是它实现调用的是该类里的本地方法setPriority()。这个本地方法是用C实现的，并被织入Jvm内部，在Windows 95的平台上，这个本地方法最终将调用Win32 SetPriority() API.这是一个本地方法的具体实现由JVM直接提供，更多的情况是本地方法由外部的动态链接（external dynamic link library） 提供，然后被JVM调用。

### 本地方法栈

1. Java虚拟机用于管理Java方法的调用，而本地方法栈用于管理本地方法的调用。
2. 本地方法栈，也是线程私有的。
3. 允许被实现成固定或者是可动态扩展的内存大小。（在内存溢出方面是相同的）
   1. 如果线程请求分配的栈容量超过本地方法栈允许的最大容量，Java虚拟机会抛出一个StackOverflowError异常。
   2. 如果本地方法栈可以动态扩展，并且在尝试扩展的时候无法申请到足够的内存，或者在创建新的线程时没有足够的内存区创建对应的本地方法栈，那么Java虚拟机将会抛出一个OutOfMemoryError 异常。
4. 本地方法是使用C语言实现的。
5. 它的具体做法是Native Method Stack 中登记native 方法，在Execution Engine 执行时加载本地方法库。

6. 当某个线程调用一个本地方法时，它就进入了一个全新的并且不再受虚拟机限制的世界，它和虚拟机拥有同样的权限。
   1. 本地方法可以通过本地方法接口来访问虚拟机内部的运行时数据区。
   2. 它甚至可以直接使用本地处理器中的寄存器
   3. 直接从本地内存的堆中分配任意数量的内存。

7. 并不是所有的JVM都支持本地方法。因为Java虚拟机规范中并没有明确要求本地方法栈的使用语言、具体实现方式、数据结构等。如果JVM产品不打算支持native方法，也可以无需实现本地方法栈。
8. 在Hotspot JVM中，直接将本地方法栈和虚拟机合二为一。

## 堆

1. 一个JVM实例只存在一个堆内存，堆也是Java内存管理的核心区域。
2. Java堆区在JVM启动的时候即被创建，其空间大小也就确定了。是JVM管理的最大一块内存空间。（大小可以调节）
3. 《Java虚拟机规范》规定，堆可以处于物理上不连续的内存空间中，但在逻辑上它应该被视为连续的。
4. 所有的线程共享Java堆，在这里还可以划分线程私有的缓存区（Thread Local Allocation Buffer, TLAB）
5. 所有的对象实例以及数组都应当在运行时分配在堆上。
6. 数组和对象可能永远不会存储在栈上，因为栈帧中保存引用，这个引用指向对象或者数组在堆中的位置。
7. 在方法结束后，堆中的对象不会马上被移除，仅仅在垃圾收集的时候才会被移除。
8. 堆，是GC执行垃圾回收的重点区域。

### 内存细分

现代垃圾收集器大部分都基于分代收集理论设计，堆空间细分为：

1. Java 7 之前逻辑上分为：新生区+养老区+永久区
2. Java 8之后：新生区+养老区+元空间

### 堆空间大小的设置

1. Java堆区用于存储Java对象实例，那么堆的大小在JVM启动时就已经设定好了，大家可以通过选项 "-Xmx" （堆区的最大内存）和 "-Xms" （堆区的起始内存）来进行设置。

2. 一旦堆区中的内存大小超过 "-Xmx" 所指定的最大内存时，将会抛出 OutOfMemoryError 异常。

3. 通常会将 -Xms 和 -Xmx两个参数配置相同的值，其目的是为了能够在 java 垃圾回收机制清理完堆区后不需要重新分隔计算堆区的大小，从而提高性能。

4. 默认情况下，初始内存大小：物理电脑内存大小 / 64

   ​                      最大内存大小：物理电脑内存大小 / 4

5. 查看设置的参数：方式一： jps  /   jstat  -gc  进程id

   ​                             方式二：-XX:+PrintGCDetails

### 年轻代和老年代

1. 存储在JVM中的Java对象可以划分为两类：
   1. 一类是生命周期较短的瞬时对象，这类对象的创建和消亡都非常迅速
   2. 另一类对象的生命周期却非常长，在某些极端的情况下还能够与JVM的生命周期保持一致。
2. Java堆区进一步细分的话，可以划分为年轻代和老年代
3. 其中年轻代又可以划分为Eden空间、Survivor0空间和Survivor1空间（有时也叫from区、to区）
4. 配置新生代与老年代在堆结构的占比：
   1. 默认 -XX：NewRatio=2，表示新生代占1，老年代占2，新生代占整个堆的1/3
   2. 可以修改-XX：NewRatio=4，表示新生代占1，老年代占4，新生代占整个堆的1/5
5. 在HotSpot中，Eden空间和另外两个Survivor空间缺省所占的比例是8：1：1
6. 开发人员可以通过选项 “-XX：SurvivorRatio” 调整这个空间比例。比如-XX：SurvivorRatio=8
7. 几乎所有的Java对象都是在Eden区被new出来的
8. 绝大部分的Java对象的销毁都在新生代进行了
9. 可以使用选项 “-Xmn” 设置新生代最大内存大小
9. Java堆分代的目的是优化GC性能。

### 对象分配过程

1. new的对象先放伊甸园区，此区域有大小限制

2. 当伊甸园的空间填满时，程序又需要拆功能键对象，JVM的垃圾回收器将堆伊甸园区进行垃圾回收，将伊甸园区中的不再被其他对象所引用的对象进行销毁，再加载新的对象放到伊甸园区

3. 然后将伊甸园中的剩余对象移动到幸存者0区。

4. 如果再次触发垃圾回收，此时上次幸存下来的放到幸存者0区，如果没有回收，就会放到幸存者1区

5. 如果再次经历垃圾回收，此时会重新放回幸存者0区，接着再去幸存者1区

6. 可设置次数，达到以后可以去养老区。

   （-XX:MaxTenuringThreshold=<N>）

7. 当养老区内存不足时，再次触发GC：Major GC，进行养老区的内存清理。
8. 若养老区执行了Major GC之后发现依然无法进行对象的保存，就会产生OOM异常。

### Minor GC、Major GC、Full GC

JVM在进行GC时，并非每次都对上面三个内存区域一起回收，大部分时候回收的都是指新生代。

针对HotSpot VM的实现，它里面的GC按照回收区域又分为两大种类型：一种是部分收集（Partial GC），一种是整堆收集（Full GC）

1. 部分收集：不是完整收集整个Java堆的垃圾收集。其中又分为：
   1. 新生代收集（Minor GC / Young GC）：只是新生代的垃圾收集
   2. 老年代收集（Major GC / Old GC）：只是老年代的垃圾收集。
      1. 目前，只有CMS GC会有单独收集老年代的行为。
      2. 注意，很多时候Major GC会和Full GC混淆使用，需要具体分辨是老年代回收还是整堆回收。
   3. 混合收集（Mixed GC）：收集整个新生代以及部分老年代的垃圾收集。
      1. 目前，只有G1 GC会有这种行为
2. 整堆收集（Full GC）：收集整个java堆和方法区的垃圾收集。

### Minor GC触发机制：

1. 当年轻代空间不足时，就会触发Minor GC，这里年轻代指的是Eden代满，Survivor满不会引发GC。（每次Minor GC会清理年轻代的内存）
2. 因为Java对象大多都具备朝生夕灭的特性，所以Minor GC非常频繁，一般回收速度也比较快，这一定义既清晰又易于理解。
3. Minor GC会引发STW，暂停其他用户的线程，等垃圾回收结束，用户线程才恢复运行。

### Major GC触发机制

1. 指发生在老年代的GC，对象从老年代消失时，我们说 ”Major GC" 或 “Full GC” 发生了。
2. 出现了Major GC，经常会伴随至少一次的Minor GC（但非绝对的，在Parallel Scavenge收集器的收集策略里就有直接进行Major GC的策略选择过程。
   1. 也就是在老年代空间不足时，会先尝试触发Minor GC。如果之后空间还不足，则触发Major GC
3. Major GC的速度一般会比Minor GC慢10倍以上，STW的时间更长。
4. 如果Major GC后，内存还不足，就报OOM了。

### Full GC触发机制

1. 调用System.gc()时，系统建议执行Full GC，但是不必然执行。

2. 老年代空间不足

3. 方法区空间不足

4. 通过Minor GC后进入老年代的平均大小大于老年代的可用内存。

5. 由Eden区、survivor space0（From Space)区向survivor space1（To Space）区复制时，对象大小大于To Space可用内存，则把该对象转存到老年代，且老年代的可用内存小于该对象大小。

   （full gc是开发或调优中尽量要避免的。这样暂时时间会短一些）

### 内存分配策略（对象提升规则）

1. 优先分配到Eden

2. 大对象直接分配到老年代

   （尽量避免程序中出现过多的大对象）

3. 长期存活的对象分配到老年代

4. 动态对象年龄判断

   （如果Survivor区中相同年龄的所有对象大小的总和大于Survivor空间的一半，年龄大于或等于该年龄的对象可以直接进入老年代，无需等到MaxTenuringThreshold中要求的年龄。

5. 空间分配担保

   （-XX：HandlePromotionFailure）

### TLAB (Thread Local Allocation Buffer)

堆区是线程共享区域，是非线程安全的，为避免多个线程操作同一地址，需要使用加锁等机制，进而影响分配速度。

1. 从内存模型而不是垃圾收集的角度，对Eden区域继续进行划分，JVM为每个线程分配了一个私有缓存区域，它包含在Eden空间内。
2. 多线程同时分配内存时，使用TLAB可以避免一系列的非线程安全问题。同时还能够提升内存分配的吞吐量，因此我们可以将这种内存分配方式称之为快速分配策略。
3. 所有OpenJDK衍生出来的JVM都提供了TLAB的设计。

### TLAB说明

1. 尽管不是所有对象实例都能够在TLAB中成功分配内存，但JVM确实是将TLAB作为内存分配的首选。
2. 在程序中，开发人员可以通过选项 “-XX：UseTLAB” 设置是否开TLAB空间。
3. 默认情况下，TLAB空间的内存非常小，仅占有整个Eden空间的1%，当然我们可以通过选项 “-XX：TLABWasteTargetPercent” 设置TLAB空间所占用Eden空间的百分比大小。
4. 一旦对象在TLAB空间分配内存失败时，JVM就会尝试着通过使用加锁机制确保数据操作的原子性，从而直接在Eden空间中分配内存。

### 堆空间的参数设置

1. -XX:+PrintFlagsInitial：查看所有参数的默认初始值

2. -XX:+PrintFlagsFinal：查看所有参数的最终值

   具体查看某个参数的指令：jps：查看当前运行中的进程

   ​                                           jinfo -flag SurvivorRatio 进程id

3. -Xms: 初始堆空间内存（默认为物理内存的1/64）

4. -Xmx: 最大堆空间内存（默认为物理内存的1/4）

5. -Xmn: 设置新生代的大小（初始值及最大值）

6. -XX:NewRatio： 配置新生代与老年代在堆结构的占比

7. -XX: SurvivorRatio设置新生代中Eden和S0/S1空间的比例

8. -XX:MaxTenuringThreshold: 设置新生代垃圾的最大年龄

9. -XX:+PrintGCDetails: 输出详细的GC处理日志

10. 打印gc简要信息：①-XX:+PrintGC ②-verbose:gc

11. -XX:HandlePromotionFailure: 是否设置空间分配担保

### 空间分配担保

在发生Minor GC之前，虚拟机会检查老年代最大可用的连续空间是否大于新生代所有对象的总空间。

1. 如果大于，则此次Minor GC是安全的
2. 如果小于，则虚拟机会查看-XX：HandlePromotionFailure设置值是否允许担保失败。
   1. 如果HandlePromotionFailure=true,那么会继续检查老年代最大可用连续空间是否大于历次晋升到老年代的对象的平均大小。
      1. 如果大于，则尝试进行一次Minor GC，但这次Minor GC依然是有风险的
      2. 如果小于，则改为进行一次FullGC
   2. 如果HandlePromotionFailure=false，则改为进行一次Full GC

在JDK6 Update24之后（JDK7）,HandlePromotionFailure参数不会再影响到虚拟机的空间分配担保策略，观察OpenJDK中源码变化，虽然源码中还定义了HandlePromotionFailure参数，但是在代码中已经不会再使用它。JDK6 Update24之后的规则变为只要老年代的连续空间大于新生代对象总大小或者历次晋升的平均大小就会进行Minor GC，否则将进行Full GC。

### 除堆外分配对象存储

1. 在Java虚拟机中，对象是在Java堆中分配内存的，这是一个普遍的常识，但是，有一种特殊情况，那就是如果经过逃逸分析（Escape Analysis）后发现，一个对象并没有逃逸出方法的话，那么就可能被优化成栈上分配，这样就无需在堆上分配内存，也无需进行垃圾回收了，这也是最常见的堆外存储技术。
2. 此外，基于OpenJDK深度定制的TaoBaoVM，其中创新的GCIH（GC invisible heap）技术实现off-heap，将生命周期较长的Java对象从heap中移至heap外，并且GC不能管理GCIH内部的Java对象，以此达到降低GC的回收频率和提升GC的回收效率的目的。

### 逃逸分析

1. 如何将堆上的对象分配到栈，需要使用逃逸分析手段。
2. 这是一种可以有效减少Java程序中同步负载和内存堆分配压力的跨函数全局数据流分析算法。
3. 通过逃逸分析，Java Hotspot编译器能够分析出一个新的对象的引用的使用范围从而决定是否要将这个对象分配到堆上。
4. 逃逸分析的基本行为就是分析对象动态作用域：
   1. 当一个对象在方法中被定义后，对象只在方法内部使用，则认为没有发生逃逸。
   2. 当一个对象在方法中被定义后，它被外部方法所引用，则认为发生逃逸。例如作为调用参数传递到其他地方中。

### 参数设置

1. 在JDK 6u23版本之后，HotSpot中默认就已经开启了逃逸分析。
2. 如果使用的是较早版本，开发人员可以通过：
   1. 选项 “-XX：+DoEscapeAnalysis” 显式开启逃逸分析
   2. 通过选项 “-XX：+PrintEscapeAnalysis” 查看逃逸分析的筛选结果。

### 逃逸分析：代码优化

使用逃逸分析，编译器可以对代码做如下优化：

一、栈上分配。将堆分配转化为栈分配，如果一个对象在子程序中被分配，要使指向该对象的指针永远不会逃逸，对象可能是栈分配的候选，而不是堆分配。

二、同步省略。如果一个对象被发现只能从一个线程被访问到，那么对于这个对象的操作可以不考虑同步。

三、分离对象或标量替换。有的对象可能不需要作为一个连续的内存结构存在也可以被访问到，那么对象的部分（或全部）可以不存储在内存（java堆），而是存储在CPU寄存器中（java栈）。

### 代码优化之标量替换

1. 标量（Scalar）是指一个无法再分解成更小的数据的数据。Java中的原始数据类型就是标量。
2. 相对的，那些还可以分解的数据叫做聚合量（Aggregate），Java中的对象就是聚合量，因为他可以分解成其他聚合量和标量。
3. 在JIT阶段，如果经过逃逸分析，发现一个对象不会被外界访问的话，那么经过JIT优化，就会把这个对象拆解成若干个其中包含的若干个成员变量代替，这个过程就是标量替换。

## 方法区内存

1. 方法区与Java堆一样，是各个线程共享的内存区域。

2. 方法区在JVM启动的时候被创建，并且它的实际物理内存空间中和Java堆区一样都是可以是不连续的。

3. 方法区的大小，跟堆空间一样，可以选择固定大小或者可扩展。

4. 方法区的大小决定了系统可以保存多少个类，如果系统定义了太多的类，导致方法区溢出，虚拟机同样会抛出内存溢出错误：java.lang.OutOfMemoryError: PermGen space 或者 java.lang.OutOfMemoryError: Metaspace

   （加载大量的第三方的jar包；Tomcat部署的工程过多，30-50个；大量动态的生成反射类）

5. 关闭JVM就会释放这个区域的内存。

6. 元空间的本质和永久代类似，都是对JVM规范中方法区的**实现**。不过元空间与永久代最大的区别在于：元空间不在虚拟机设置的内存中，而是使用本地内存。

### 设置方法区内存大小

1. 方法区的大小不必是固定的，jvm可以根据应用的需要动态调整。
2. jdk7及以前：
   1. 通过-XX：PermSize来设置永久代初始分配空间。默认值是20.75M
   2. -XX：MaxPermSize来设定永久代最大可分配空间。32位机器默认是64M，64位机器模式是82M
   3. 当JVM加载的类信息容量超过了这个值，会报异常OutOfMemoryError: PermGenspace。
3. 演进：
   1. 到了JDK8，完全放弃了永久代的概念，变为了在本地内存中实现的元空间来代替。
   2. 元空间的本质和永久代类似，都是对JVM规范中方法区的实现，不过元空间与永久代最大的区别是：元空间不在虚拟机设置的内存中，而是使用本地内存。
   3. 永久代、元空间并不是名字变了，内部结构也调整了
   4. 当方法区无法满足新的内存分配需求时，将抛出OOM异常。
4. Jdk8及以后
   1. 元数据区大小可以使用参数-XX：MetaspaceSize和-XX：MaxMetaspaceSize指定，代替上述原有的两个参数。
   2. 默认值依赖于平台。windows下，-XX：MetaspaceSize是21M，-XX:MaxMetaspaceSize 的值是-1，即没有限制
   3. 与永久代不同，如果不指定大小，默认情况下，虚拟机会耗尽所有的可用系统内存。如果元空间数据区发生溢出，虚拟机一样会抛出异常OutOfMemoryError: Metaspace
   4. -XX：MetaspaceSize: 设置初始的元空间大小，对于一个64位的服务器端JVM来说，其默认的-XX：MetaspaceSize值位21MB。这就是初始的高水位线，一旦触及这个水位线，Full GC将会被触发并卸载没用的类（即这些类对应的类加载器不再存活），然后这个高水位线将会重置。新的高水位线的值取决于GC后释放了多少元空间。如果释放的空间不足，那么在不超过MaxMetaspaceSize时，适当提高该值，如果释放空间过多，则适当降低该值。
   4. 如果初始化的高水位线设置过低，上述高水位线调整情况会发生多次，通过垃圾回收其的日志可以观察到Full GC 多次调用。为了避免频繁的GC，建议将-XX: MetaspaceSize设置为一个相对较高的值。

### 如何解决OOM

1. 要解决OOM异常或heap space的异常，一般的手段是首先通过内存印象分析工具（如Eclipse Memory Analyzer）对dump出来的堆转储快照进行分析，重点是确认内存中的对象是否是必要的，也就是要先分清楚到底是出现了内存泄漏（Memory Leak）还是内存溢出（Memory Overflow)。
2. 如果是内存泄漏，可进一步通过工具查看泄漏对象到GC Roots的引用链。于是就能找到泄漏对象是通过怎样的路径与GC Roots相关连并导致垃圾收集器无法自动回收它们的。掌握了泄漏对象的类型信息，以及GC Roots引用链的信息，就可以比较准确的定位出泄漏代码的位置。
3. 如果不存在内存泄漏，换句话说就是内存中的对象确实都还必须存活着，那就应当检查虚拟机的堆参数（-Xmx与-Xms），与机器物理内存对比看是否还可以调大，从代码检查是否存在某些对象生命周期过长、持有状态时间过长的情况，尝试减少程序运行期的内存消耗。

### 方法区的内部结构

用于存储已被虚拟机加载的类型信息、常量、静态变量、即时编译器编译后的代码缓存等。

1. 类型信息

   对每个加载的类型（类class、接口interface、枚举enum、注解annotation），JVM必须在方法区中存储以下类型信息：

   1. 这个类型的完整有效名称（全名=包名.类名）
   2. 这个类型直接父类的完整有效名（对于interface或是java.lang.Object，都没有父类）
   3. 这个类型的修饰符（public，abstract，final的某个子集）
   4. 这个类型直接接口的一个有序列表

2. 域（Field）信息

   1. JVM必须在方法区中保存类型的所有域的相关信息以及域的声明顺序。
   2. 域的相关信息包括：域名称、域类型、域修饰符（public，private，protected, static, final, volatile, transient 的某个子集）

3. 方法信息

   JVM必须保存所有方法的以下信息，同域信息一样包括声明顺序：

   1. 方法名称
   2. 方法的返回类型（或 void）
   3. 方法参数的数量和类型（按顺序）
   4. 方法的修饰符（public，private，protected, static, final, synchronized, native, abstract的一个子集）
   5. 方法的字节码（bytecodes)、操作数栈、局部变量表及大小（abstract和native方法除外)
   6. 异常表（abstract 和native方法除外）（每个异常处理的开始位置、结束位置、代码处理在程序计数器中的偏移地址、被捕获的异常类的常量池索引）


### 运行时常量池vs常量池

1. 方法区，内部包含了运行时常量池
2. 字节码文件，内部包含了常量池

### 为什么需要常量池

一个Java源文件中的类、接口，编译后产生一个字节码文件，而Java中的字节码需要数据支持，通常这种数据会很大以至于不能直接存到字节码里，换一种方式，可以存到常量池，这个字节码包含了指向常量池的引用。在动态连接时会用到运行时常量池。

### 常量池中的数据类型

1. 数量值
2. 字符串值
3. 类引用
4. 字段引用
5. 方法引用

常量池，可以看作一张表，虚拟机指令根据这张常量表找到要执行的类名、方法名、参数类型、字面量等类型。

### 运行时常量池

1. 运行时常量池是方法区的一部分

2. 常量池表是Class文件的一部分，用于存放编译期生成的各种字面量与符号引用，这部分内容将在类加载后存放到方法区的运行时常量池中。

3. 运行时常量池，在加载类和接口到虚拟机后，就会创建对应的运行时常量池。

4. JVM为每个已加载的类型（类或接口）都维护一个常量池，池中的数据项像数组项一样，是通过索引访问的。

5. 运行时常量池中包含多种不同的常量，包括编译期就已经明确的数值字面量，也包括到运行期解析后才能够获得的方法或者字段引用，此时不再是常量池中的符号地址了，这里换为真实地址。

   （运行时常量池，相对于Class文件常量池的另一重要特征是：具备动态性）

6. 运行时常量池类似于传统编程语言中的符号表（symbol table），但是它所包含的数据却比符号表要更加丰富一些。

7. 当创建类或接口的运行时常量池时，如果构造运行时常量池所需的内存空间超过了方法区所能提供的最大值，则JVM会抛OutOfMemoryError异常。

### 方法区的演进细节

1. 只有HotSpot才有永久代，BEA JRockit、IBM J9等，是不存在永久代的概念的。原则上如何实现方法区属于虚拟机实现细节，不受《Java虚拟机规范》管束，并不要求统一。
2. Hotspot中方法区的变化如下：
   1. jdk1.6及之前，有永久代，静态变量存放在永久代上
   2. jdk1.7，有永久代，但已经逐步“去永久化”，字符串常量池、静态变量移除，保存在堆中
   3. jdk1.8及之后，无永久代，类型信息、字段、方法、常量保存在本地内存的元空间，但**字符串常量池、静态变量仍在堆**

### 元空间替换永久代的原因

由于类的元数据分配在本地内存中，元空间的最大可分配空间就是系统可用内存空间

1. 永久代设置空间大小是很难确定的。

   在某些场景下，如果动态加载类过多，容易产生Perm区的OOM，比如某个实际Web工程中，因为功能比较多，在运行过程中，要不断动态加载很多类，经常出现致命错误。

   而元空间和永久代之间最大的区别在于：元空间并不在虚拟机中，而是使用本地内存。因此，默认情况下，元空间的大小仅受本地内存限制。

2. 堆永久代进行调优是很困难的

### StringTable (字符串常量池) 为什么要调整

jdk7中将StringTable放到了堆空间中，因为永久代的回收效率很低，在full gc的时候才会触发，而full gc是老年代的空间不足、永久代不足时才会触发。这就导致StringTable回收效率不高，而开发中会有大量的字符串被创建，回收效率低，导致永久代内存不足。放到堆里，能及时回收内存。

### 方法区的垃圾回收

主要回收：常量池中废弃的常量和不再使用的类型。

1. 常量池中主要存放的两大类常量：字面量和符号引用。
2. HotSpot虚拟机对常量池的回收策略：只要常量池中的常量没有被任何地方引用，就可以被回收。
3. 判定一个类型是否允许被回收，需要同时满足三个条件：
   1. 该类所有的实例都已经被回收，也就是Java堆中不存在该类极其任何派生子类的实例。
   2. 加载该类的类加载器已经被回收，这个条件除非是经过精心设计的可以换类加载器的场景，如OSGi、JSP的重加载等，否则通常是很难达成的。
   3. 该类对应的java.lang.Class对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。
4. Java虚拟机被允许对满足上述三个条件的无用类进行回收，这里说的仅仅是“被允许”，而并不是和对象一样，没有引用了就必然会被回收。关于是否要对类型进行回收，HotSpot虚拟机提供了-Xnoclassgc参数进行控制，还可以使用-verbose:class以及-XX:+TraceClass-Loading、-XX:+TraceClassUnLoading查看类加载和卸载信息
5. 在大量使用反射、动态代理、CGLib等字节码框架，动态生成JSP以及OSGi这类频繁自定义类加载器的场景中，通常都需要Java虚拟机具备类型卸载的能力，以保证不会对方法区造成过大的内存压力。

## 对象的实例化

### 创建对象的方式

1. new
2. Class的newInstance(): 反射的方式，只能调用空参的构造器，权限必须是public
3. Constructor的newInstance(Xxx): 反射的方式，可以调用空参、带参的构造器，权限没有要求
4. 使用clone()：不调用任何构造器，当前类需要实现Cloneable接口
5. 使用反序列化：从文件中、从网络中获取一个对象的二进制流
6. 第三方库Objenesis

### 创建对象的步骤

1. 判断对象对应的类是否加载、链接、初始化

2. 为对象分配内存

   1. 如果内存规整（指针碰撞）
   2. 如果内存不规整（虚拟机需要维护一个列表，空闲列表分配）
   3. 说明

3. 处理并发安全问题

   1. 采用CAS配上失败重试保证更新的原子性
   2. 每个线程预先分配一块TLAB

4. 初始化分配到的空间

   所有属性设置默认值，保证对象实例字段在不赋值时可以直接使用

5. 设置对象的对象头

6. 执行init方法进行初始化

### 步骤详细说明

1. 虚拟机遇到一条new指令，首先去检查这个指令的参数能否在Metaspace的常量池中定位到一个类的符号引用，并且检查这个符号引用代表的类是否已经被加载、解析和初始化。（即判断类元信息是否存在）。如果没有，那么在双亲委派模式下，使用当前类加载器以ClassLoader+包名+类名为Key进行查找对应的.Class文件。如果没有找到文件，则抛出ClassNotFoundException异常，如果找到，则进行类加载，并生成对应的Class类对象。
2. 首先计算对象占用空间大小，接着在堆中划分一块内存给新对象。如果实例成员变量是引用变量，仅分配引用变量空间即可，即4个字节大小。
   1. 如果内存是规整的，那么虚拟机将采用的是指针碰撞法（Bump the Pointer) 来为对象分配内存。意思是所有用过的内存在一边，空闲的内存在另外一边，中间放着一个指针作为分界点的指示器，分配内存就仅仅是把指针向空闲的那边挪动一段与对象大小相等的距离罢了。如果垃圾收集器选择的是Serial、ParNew这种基于压缩算法的，虚拟机采用这种分配方式。一般使用带有compact（整理）过程的收集器时，使用指针碰撞。
   2. 如果内存是不规整的，那么虚拟机将采用的是空闲列表法来为对象分配内存。意思是虚拟机维护了一个列表，记录上哪些内存块是可用的，再分配的时候从列表中找到一块足够大的空间划分给对象实例，并更新列表上的内容，这种分配方式称为“空闲列表（Free List)”。
   3. 选择哪种分配方式由Java堆是否规整决定，而Java堆是否规整又由所采用的垃圾收集器是否带有压缩整理功能决定。

5. 给对象的属性赋值操作：
   1. 属性的默认初始化
   2. 显式初始化
   3. 代码块中初始化
   4. 构造器中初始化
6. 将对象的所属类（即类的元数据类型）、对象的HashCode和对象的GC信息、锁信息等数据存储在对象的对象头中。这个过程的具体设置方式取决于JVM实现。

7. 在Java程序的视角看来，初始化才正式开始。初始化成员变量，执行实例化代码块，调用类的构造方法，并把堆内对象的首地址赋值给引用变量。因此一般来说（由字节码中是否跟随由invokespecial指令所决定），new指令之后会接着就是执行方法，把对象按照程序员的意愿进行初始化，这样一个真正可用的对象才算完全创建出来。

### 对象的内存布局

1. 对象头（Header）：
   1. 运行时元数据（Mark Word）
      1. 哈希值
      2. GC分代年龄
      3. 锁状态标志
      4. 线程持有的锁
      5. 偏向线程ID
      6. 偏向时间戳
   2. 类型指针：指向类元数据InstanceKlass，确定该对象所属的类型
   3. 如果时数组，还需记录数组的长度
2. 实例数据（Instance Data）：
   1. 说明：它是对象真正存储的有效信息，包括程序代码中定义的各种类型的字段（包括从父类继承下来的和本身拥有的字段）
   2. 规则：
      1. 相同宽度的字段总是被分配在一起
      2. 父类中定义的变量会出现在子类之前
      3. 如果CompactFields参数为true（默认为true）：子类的窄变量可能插入到父类变量的空隙
3. 对齐填充（Padding）：不是必须的，也没有特别含义，仅仅起到占位符的作用

### 对象访问定位

1. 句柄访问

   1. 堆内存分为句柄池和实例池

   2. 对象引用指向句柄，
   3. 句柄分为到对象实例数据的指针（指向实例池）和到对象类型数据的指针（指向方法区）

2. 直接指针（Hotspot采用）

   对象引用指向对象实例，对象头中存有到对象类型数据的指针。

### 直接内存

1. 不是虚拟机运行时数据区的一部分，也不是《Java虚拟机规范》中定义的内存区域。
2. 直接内存是在Java堆外的、直接向系统申请的内存空间。
3. 来源于NIO，通过存在堆中的DirectByteBuffer操作Native内存
4. 通常，访问直接内存的速度会优于Java堆。即读写性能高。
   1. 因此出于性能考虑，读写频繁的场合可能会考虑使用直接内存
   2. Java的NIO库允许Java程序使用直接内存，用于数据缓冲区