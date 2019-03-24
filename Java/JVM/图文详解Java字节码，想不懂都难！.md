##                      图文详解 Java 字节码，想不懂都难！

*2018-04-23* Java编程

**来自：开源中国 协作翻译**

**原文**：Introduction to Java Bytecode

**链接**：https://dzone.com/articles/introduction-to-java-bytecode

**译者**：dreamanzhao, 雪落无痕xdj, kevinlinkai, Tocy, 边城, 凉凉_, 无若, imqipan, Tot_ziens

即 便对那些有经验的Java开发人员来说，阅读已编译的Java字节码也很乏味。为什么我们首先需要了解这种底层的东西？这是上周发生在我身上的一个简单故 事：很久以前，我在机器上做了一些代码更改，编译了一个JAR，并将其部署到服务器上，以测试性能问题的一个潜在修复方案。不幸的是，代码从未被检入到版 本控制系统中，并且出于某种原因，本地更改被删除了而没有追踪。几个月后，我再次修改源代码，但是我找不到上一次更改的版本！

  

幸 运的是编译后的代码仍然存在于该远程服务器上。我于是松了一口气，我再次抓取JAR并使用反编译器编辑器打开它......只有一个问题：反编译器GUI 不是一个完美的工具，并且出于某种原因，在该JAR中的许多类中找到我想要反编译的特定类并在我打开它时会在UI中导致了一个错误，并且反编译器崩溃！

  

绝 望的时候需要采取孤注一掷的措施。幸运的是，我对原始字节码很熟悉，我宁愿花些时间手动地对一些代码进行反编译，而不是通过不断的更改和测试它们。因为我 仍然记得在哪里可以查看代码，所以阅读字节码帮助我精确地确定了具体的变化，并以源代码形式构建它们。（我一定要从我的错误中吸取教训，这次要珍惜好这些 教训！）

字节码的好处是，您可以只用学习它的语法一次，然 后它适用于所有Java支持的平台——因为它是代码的中间表示，而不是底层CPU的实际可执行代码。此外，字节码比本机代码更简单，因为JVM架构相当简 单，因此简化了指令集，另一件好事是，这个集合中的所有指令都是由Oracle提供完整的文档。

  

不过，在学习字节码指令集之前，让我们熟悉一下JVM的一些事情，这是进行下一步的先决条件。

  

## **JVM 数据类型**

  

Java 是静态类型的，它会影响字节码指令的设计，这样指令就会期望自己对特定类型的值进行操作。例如，就会有好几个add指令用于两个数字相加：iadd、 ladd、fadd、dadd。他们期望类型的操作数分别是int、long、float和double。大多数字节码都有这样的特性，它具有不同形式的 相同功能，这取决于操作数类型。

  

JVM定义的数据类型包括:

  

1.  基本类型:

*   数 值类型: byte (8位), short (16位), int (32 位), long (64-bit位), char (16位无符号 Unicode), float(32-bit IEEE 754 单精度浮点型), double (64-bit IEEE 754 双精度浮点型)

*   布尔类型

*   指针类型: 指令指针。

3.  引用类型:

*   类

*   数组

*   接口

  

在字节码中布尔类型的支持是受限的。举例来说，没有结构能直接操作布尔值。布尔值被替换转换成 int 是通过编译器来进行的，并且最终还是被转换成 int 结构。

  

Java 开发者应该熟悉所有上面的类型，除了 returnAddress，它没有等价的编程语言类型。

  

### **基于栈的架构**

  

字节码指令集的简单性很大程度上是由于 Sun 设计了基于堆栈的 VM 架构，而不是基于寄存器架构。有各种各样的进程使用基于JVM 的内存组件, 但基本上只有 JVM 堆需要详细检查字节码指令：

  

**PC寄存器**：对于Java程序中每个正在运行的线程，都有一个PC寄存器保存着当前执行的指令地址。

  

**JVM 栈**：对于每个线程，都会分配一个栈，其中存放本地变量、方法参数和返回值。下面是一个显示3个线程的堆栈示例。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**堆**：所有线程共享的内存和存储对象（类实例和数组）。对象回收是由垃圾收集器管理的。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

**方法区**：对于每个已加载的类，它储存方法的代码和一个符号表（例如对字段或方法的引用）和常量池。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

JVM堆栈是由帧组成的，当方法被调用时，每个帧都被推到堆栈上，当方法完成时从堆栈中弹出（通过正常返回或抛出异常）。每一帧还包括：

  

1.  本地变量数组，索引从0到它的长度-1。长度是由编译器计算的。一个局部变量可以保存任何类型的值，long和double类型的值占用两个局部变量。

2.  用来存储中间值的栈，它存储指令的操作数，或者方法调用的参数。

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

##   

## **字节码探索**

  

关于JVM内部的看法，我们能够从示例代码中看到一些被生成的基本字节码例子。Java类文件中的每个方法都有代码段，这些代码段包含了一系列的指令，格式如下：

 opcode (1 byte)      operand1 (optional)      operand2 (optional)      ...

 这个指令是由一个一字节的opcode和零个或若干个operand组成的，这个operand包含了要被操作的数据。

 在当前执行方法的栈帧里，一条指令可以将值在操作栈中入栈或出栈，可以在本地变量数组中悄悄地加载或者存储值。让我们来看一个例子：

  ![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
  

为了打印被编译的类中的结果字节码（假设在Test.class文件中），我们运行javap工具：

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

我们可以得到如下结果：

  ![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  

我 们可以看到main方法的方法声明，descriptor说明这个方法的参数是一个字符串数组([Ljava/lang/String; )，而且返回类型是void（V）。下面的flags这行说明该方法是公开的(ACC_PUBLIC)和静态的 (ACC_STATIC)。

  

Code 属性是最重要的部分，它包含了这个方法的一系列指令和信息，这些信息包含了操作栈的最大深度（本例中是2）和在这个方法的这一帧中被分配的本地变量的数量 （本例中是4）。所有的本地变量在上面的指令中都提到了，除了第一个变量（索引为0），这个变量保存的是args参数。其他三个本地变量就相当于源码中的 a，b和c。

  

从地址0到8的指令将执行以下操作：

 iconst_1:将整形常量1放入操作数栈。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

istore_1:在索引为1的位置将第一个操作数出栈（一个int值）并且将其存进本地变量，相当于变量a。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
  

iconst_2:将整形常量2放入操作数栈。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

istore_2:在索引为2的位置将第一个操作数出栈并且将其存进本地变量，相当于变量b。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
  

iload_1:从索引1的本地变量中加载一个int值，放入操作数栈。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
  

iload_2:从索引2的本地变量中加载一个int值，放入操作数栈。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

iadd:把操作数栈中的前两个int值出栈并相加，将相加的结果放入操作数栈。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
  

istore_3:在索引为3的位置将第一个操作数出栈并且将其存进本地变量，相当于变量c。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)
  

return:从这个void方法中返回。

  

上述指令只包含操作码，由JVM来精确执行。

  

### **方法调用**

  

上面的示例只有一个方法，即 main 方法。假如我们需要对变量 c 进行更复杂的计算，这些复杂的计算写在新方法 calc 中：

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  

看看生成的字节码：

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  

main  方法代码唯一的不同在于用 invokestatic 指令代替了 iadd 指令，invokestatic 指令用于调用静态方法 calc。注意，关键在于操作数栈中传递给 calc 方法的两个参数。也就是说，调用方法需要按正确的顺序为被调用方法准备好所有参数，交依次推入操作数栈。 iinvokestatic（还有后面提到的其它类似的调用指令）随后会从栈中取出这些参数，然后为被调用方法创建一个新的环境，将参数作为局域变量置于 其中。

  

我 们也注意到invokestatic指令在地址上看占据了3字节，由6跳转到9。不像其余指令那样那么远，这是因为invokestatic指令包含了两 个额外的字节来构造要调用的方法的引用（除了opcode外）。这引用由javap显示为#2，是一个引用calc方法的符号，解析于从前面描述的常量池 中。

  

其它的新信息显然是calc方法本身的代码。它首先将第一个整数参数加载到操作数堆栈上（iload_0）。下一条指令，i2d，通过应用扩展转换将其转换为double类型。由此产生的double类型取代了操作数堆栈的顶部。

  

再 下一条指令将一个double类型常量2.0d(从常量池中取出)推到操作数堆栈上。然后静态方法Math.pow调用目前为止准备好的两个操作数值（第 一个参数是calc和常量2.0d）。当Math.pow方法返回时，他的结果将会被存储在其调用程序的操作数堆栈上。在下面说明。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  

同样的程序应用于计算Math.pow(b,2):

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  

下 一条指令，dadd，会将栈顶的两个中间结果出栈，将它们相加，并将所得之和推入栈顶。最后，invokestatic 对这个和值调用 Math.sqrt，将结果从 double（双精度浮点型） 窄化转换（d2i）成 int（整型）。整型结果会返回到 main 方法中， 并在这里保存到 c（istore_3）。

  

### **创建实例**

  

现在修改这个示例，加入 Point 类来封装 XY 坐标。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  

编译后的 main 方法的字体码如下：

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  

这 里引入了 new、dup 和 invokespecial 几个新指令。new 指令与编程语言中的 new 运算符类似，它根据传入的操作数所指定类型来创建对象（这是对 Point 类的符号引用）。对象的内存是在堆上 分配，对象引用则是被推入到操作数栈上。

  

dup 指令会复制顶部操作数的栈值，这意味着现在我们在栈顶部有两个指向Point对象的引用。接下来的三条指令将构造函数的参数（用于初始化对象）压入操作数 堆栈中，然后调用与构造函数对应的特殊初始化方法。下一个方法中x和y字段将被初始化。该方法完成之后，前三个操作数的栈值将被销毁，剩下的就是已创建对 象的原始引用（到目前为止，已成功完成初始化了）。

  

接下来，astore_1将该Point引用出栈，并将其赋值到索引1所保存的本地变量(astore_1中的a表明这是一个引用值).

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  

通用的过程会被重复执行以创建并初始化第二个Point实例，此实例会被赋值给变量b。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  

最后一步是将本地变量中的两个Point对象的引用加载到索引1和2中（分别使用aload_1和aload_2），并使用invokevirtual调用area方法，该方法会根据实际的类型来调用适当的方法来完成分发。

  

例如，如果变量a包含一个扩展自Point类的SpecialPoint实例，并且该子类重写了area方法，则重写后的方法会被调用。在这种情况下，并不存在子类，因此仅有area方法是可用的。

  

![](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

  

请 注意，即使area方法接受单参数，堆栈顶部也有两个Point的引用。第一个（pointA，来自变量a）实际上是调用该方法的实例（在编程语言中被称 为this），对area方法来说，它将被传递到新栈帧的第一个局部变量中。另一个操作数（pointB）是area方法的参数。

  

### **另一种方式**  

  

你无需对每条指令的理解和执行的准确流程完全掌握，以根据手头的字节码了解程序的功能。例如，就我而言，我想检查代码是否驱动Java stream来读取文件，以及流是否被正确地关闭。现在以下面的字节码为例，确认以下情况是很简单的：一个流是否被使用并且很有可能是作为try-with-resources语句的一部分被关闭的。

  

> public static void main(java.lang.String[]) throws java.lang.Exception;
> 
>  descriptor: ([Ljava/lang/String;)V
> 
>  flags: (0x0009) ACC_PUBLIC, ACC_STATIC
> 
>  Code:
> 
>    stack=2, locals=8, args_size=1
> 
>       0: ldc           #2                  // class test/Test
> 
>       2: ldc           #3                  // String input.txt
> 
>       4: invokevirtual #4                  // Method java/lang/Class.getResource:(Ljava/lang/String;)Ljava/net/URL;
> 
>       7: invokevirtual #5                  // Method java/net/URL.toURI:()Ljava/net/URI;
> 
>      10: invokestatic  #6                  // Method java/nio/file/Paths.get:(Ljava/net/URI;)Ljava/nio/file/Path;
> 
>      13: astore_1
> 
>      14: new           #7                  // class java/lang/StringBuilder
> 
>      17: dup
> 
>      18: invokespecial #8                  // Method java/lang/StringBuilder."":()V
> 
>      21: astore_2
> 
>      22: aload_1
> 
>      23: invokestatic  #9                  // Method java/nio/file/Files.lines:(Ljava/nio/file/Path;)Ljava/util/stream/Stream;
> 
>      26: astore_3
> 
>      27: aconst_null
> 
>      28: astore        4
> 
>      30: aload_3
> 
>      31: aload_2
> 
>      32: invokedynamic #10,  0             // InvokeDynamic #0:accept:(Ljava/lang/StringBuilder;)Ljava/util/function/Consumer;
> 
>      37: invokeinterface #11,  2           // InterfaceMethod java/util/stream/Stream.forEach:(Ljava/util/function/Consumer;)V
> 
>      42: aload_3
> 
>      43: ifnull        131
> 
>      46: aload         4
> 
>      48: ifnull        72
> 
>      51: aload_3
> 
>      52: invokeinterface #12,  1           // InterfaceMethod java/util/stream/Stream.close:()V
> 
>      57: goto          131
> 
>      60: astore        5
> 
>      62: aload         4
> 
>      64: aload         5
> 
>      66: invokevirtual #14                 // Method java/lang/Throwable.addSuppressed:(Ljava/lang/Throwable;)V
> 
>      69: goto          131
> 
>      72: aload_3
> 
>      73: invokeinterface #12,  1           // InterfaceMethod java/util/stream/Stream.close:()V
> 
>      78: goto          131
> 
>      81: astore        5
> 
>      83: aload         5
> 
>      85: astore        4
> 
>      87: aload         5
> 
>      89: athrow
> 
>      90: astore        6
> 
>      92: aload_3
> 
>      93: ifnull        128
> 
>      96: aload         4
> 
>      98: ifnull        122
> 
>     101: aload_3
> 
>     102: invokeinterface #12,  1           // InterfaceMethod java/util/stream/Stream.close:()V
> 
>     107: goto          128
> 
>     110: astore        7
> 
>     112: aload         4
> 
>     114: aload         7
> 
>     116: invokevirtual #14                 // Method java/lang/Throwable.addSuppressed:(Ljava/lang/Throwable;)V
> 
>     119: goto          128
> 
>     122: aload_3
> 
>     123: invokeinterface #12,  1           // InterfaceMethod java/util/stream/Stream.close:()V
> 
>     128: aload         6
> 
>     130: athrow
> 
>     131: getstatic     #15                 // Field java/lang/System.out:Ljava/io/PrintStream;
> 
>     134: aload_2
> 
>     135: invokevirtual #16                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
> 
>     138: invokevirtual #17                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
> 
>     141: return
> 
>    ...

可以看到java/util/stream/Stream执行forEach之前，首先触发InvokeDynamic以引用Consumer。与此同时会发现大量调用Stream.close与Throwable.addSuppressed的字节码，这是编译器实现try-with-resources statement的基本代码。

  

这是完整的原始代码。

  

## **总结**

  

还好字节码指令集简洁，生成指令时几乎少有的编译器优化，反编译类文件可以在没有源码的情况下检查代码，当然如没有源码这也是一种需求！

