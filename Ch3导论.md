# 3.导论

​	这本书是一个使用现代硬件构建语言Chisel进行数字系统设计的入门书。在本书中，我们将视角聚焦在比平常的硬件设计更高的抽象层，使得在短时间内搭建更复杂、更具交互性的数字系统成为可能。

​	本书和Chisel一样，将目标对准了两类人：(1)硬件设计者和(2)软件工程师。能够熟练使用VHDL或者Verilog和使用类似Python、Java或者Tcl这样的语言生成硬件的硬件设计师可以移植到一个单独的硬件构建语言上，在这个语言中硬件生成是语言本身的一部分。对硬件设计有兴趣的软件工程师，例如，Intel未来的芯片会谈家可编程硬件来加速程序速度。使用Chisel作为你的第一门硬件描述语言是非常棒的选择。

​	Chisel将诸如面向对象和函数是语言之类的软件工程的优点引入到数字设计中。Chisel不仅允许表示硬件在寄存器传输的抽象层，而且允许你编写硬件生成器。

​	硬件现在普遍通过硬件描述语言进行描述。描绘硬件部分、甚至是使用CAD工具进行画图的时代已经过去了。一些高层次的草图可以给出一个系统的整体视图，但是并没法用于描述系统。

​	最常用的两个硬件描述语言是Verilog和VHDL。这两门语言都已经存在了较长的时间，含有大量的历史遗留特性，并且在关于什么样的语言结构能够综合到硬件上有一个不明确的规则。请不要误会我的意思，VHDL和Verilog仍然可以完美地描述综合到ASIC上的硬件模块。而使用Chisel进行硬件设计的过程中，Verilog充当了一个用于测试和综合的中间语言。

​	本书不是对硬件设计和基础知识的一般性介绍。对于有关数字设计基础知识，例如如何使用CMOS晶体管搭建一个门电路，你需要参考其他数字设计书。本书的目的是教你如何在一个抽象层次进行设计，能够描述ASIC或是设计FPGA。作为本书的前置知识，我们假设你有一些基本的布尔代数和二进制系统的知识。任意编程语言的编程经验也是必要的。你不必知晓如何使用Verilog或是VHDL。Chisel完全可以成为你的第一门硬件描述语言。因为构建过程基于sbt和make，基本的命令行界面知识会很有用。

​	Chisel本身不是一门大型语言。基本部分一页纸就能说完，你完全可以在几天之内掌握它。正因此，这本书也不会很厚。Chisel当然比VHDL和Verilog要小，同时继承了很多规则，Chisel的强大在于它嵌入在Scala语言中，Scala本身是一门很强的语言。Chisel继承了Scala“伴随你成长的语言”这一特性。但是这本书不会在Scala本身着墨过多。

​	这本书是一个数字设计和Chisel语言教程，要注意它不是Chisel语言的参考书也不是一本完整的芯片设计书。

​	书中的所有例子来自经过编译和测试的程序。理论上，代码应该不含任何语法问题。代码例子在本书的github repo里。除了Chisel代码外，我们还试着提供有用的设计以及一个好的硬件描述风格规则。

​	本书在笔记本电脑或者平板上经过了优化，我们在文本中提供了链接，大多数指向Wikipedia的文章。

## 3.1安装Chisel和FPGA工具

​	Chisel是一个Scala库，安装Chisel和Scala最简单的方法是通过sbt，Scala构建工具。Scala本身依赖于Java JDK 1.8。因为Oracle修改了Java的license，通过AdoptOpenJDK安装OpenJDK应当更容易一些。

### 3.1.1macOS

​	从AdoptOpenJDK安装Java OpenJDKL8。在macOS X中，利用包管理软件Homebrew，sbt可以通过如下命令安装：

```sh
$ brew install sbt git
```

​	安装GTKWave和IntelliJ。当你导入一个项目，选择JDK1.8(注意不是Java 11)。

### 3.1.2Linux/Ubuntu

​	在Ubuntu下安装Java和有用的工具，使用如下命令：

```sh
$ sudo apt install openjdk-8-jdk git make gtkwave
```

​	对于基于Debian的Ubutu系统，软件通常通过(.deb)文件进行安装。但是编写本书时，sbt还不能通过这种方式进行安装，安装过程会比较麻烦一些：

```sh
echo "deb https://dl.bintray.com/sbt/debian /" | \
	sudo tee -a /etc/apt/sources.list.d/sbt.list
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 \
	--recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823
sudo apt-get update
sudo apt-get install sbt
```

### 3.1.3Windows

​	从AdoptOpenJDK安装Java OpenJDK。Chisel和Scala也支持Windows操作系统。安装GTKWave和IntelliJ。同样的，导入项目时选择JDK1.8，sbt可以使用Windows安装器按轧辊，参照:Installing sbt on Windows。 Install a git client。

### 3.1.4FPGA工具

​	为了构建FPGA硬件，你需要一个综合的工具。两大主要的FPGA供应商，Intel和Xilinx都提供了免费的工具，涵盖了小容量到中等容量大小的FPGA。其中，中等规模的FPGA已经足够容纳多核RISC架构处理器。Intel提供了Quartus Prime Lite版本和Vivado Design Suite， WebPACK版本。

## 3.2Hello World

​	每一本编程书都会从Hello World的最小程序开始。下面代码是第一种实现方法：

```scala
object HelloScala extends App{
  println("Hello Chisel World!")
}
```

​	通过sbt编译并执行这个程序：

```sh
$ sbt "runMain HelloScala"
```

​	随后，Hello World程序输出预期内容：

```sh
[info] Running HelloScala
Hello Chisel World!
```

​	然而，这真的是Chisel程序吗？这个生成硬件是用于输出字符串吗？并不是这样，这里实际上是Scala代码，并不能表示硬件设计的Hello World程序。

## 3.3Chisel Hello World

​	那么硬件设计中等价于Hello World的程序是什么呢？是最小的实用可视化设计？闪烁的LED是硬件(或者说嵌入式软件)版本的Hello World。如果成功让一个LED闪烁，那我们就能够迎接更大的挑战！

```scala
class Hello extends Module{
  val io = IO(new Bundle{
    val led = Output(UInt(1.W))
  })
  val CNT_MAX = (50000000 / 2 - 1).U;
  
  val cntReg = RegInit(0.U(32.W))
  val blkReg = RegInit(0.U(1.W))
  
  cntReg := cntReg + 1.U
  when(cntReg == CNT_MAX){
    cntReg := 0.U
    blkReg := -blkReg
  }
  io.led := blkReg
}
```

<center style="color:#c0c0c0">程序3.1 hello</center>

​	hello展示的是一个Chisel描述的闪烁LED程序。你现在不必追求理解代码案例的细节，我们会在之后的内容中进行介绍。现在只需要注意这个电路通常有着较高频率的时钟，例如50MHz，我们需要一个计数器分频得到Hz级别的频率来实现可视闪烁。在这个例子中，我们从0计数到25000000-1，然后触发闪烁信号(blkReg:=-blkReg)，然后复位计数器(cntReg:=0.U)。这个硬件以1Hz的频率闪烁LED。

## 3.4Chisel的IDE

​	本书不对读者的编程环境和编辑器做出假设。在命令行中使用sbt并选择一个你喜欢的编辑器对于入门学习来说已经足够。在其他的传统书籍中，所有你想要输入终端的命令前会有一个$字符，这个字符你不需要输入，如下例子会让Unix列出当前文件夹下的文件：

```sh
$ ls
```

​	也就是说，一个编译器在后台运行的集成开发环境(IDE)，可以加速编程。因为Chisel是一个Scala库，所有支持Scala的IDE都可以支持Chisel。在IntelliJ和Eclipse中从build.sbt中的sbt项目配置生成一个项目也是可行的。

​	在IntelliJ中，你可以通过File-New-Project form Existing Sources...选择项目的build.sbt文件。

​	在Eclipse中，你可以通过

```sh
$sbt eclipse
```

​	然后在Eclipse 中导入一个工程。

​	VSCode是另一个Chisel IDE选择。Scala Metals拓展提供了Scala支持。在左侧栏中选择Extensions，搜索Metals，安装Scala(Metals)，为了引入sbt为基础的项目，通过File-Open打开文件夹。

## 3.5访问源码和电子书功能

​	本书开源在Github:chisel-book。书中所有的Chisel例子都包含在该仓库中。这些代码通过最近的Chisel版本编译，很多例子还包含了测试程序。我们收集了很多Chisel的例子在chisel-examples仓库里。如果你发现了错误或者笔误，可以提交pull reques你可以在Github记录一个issue来提供反馈或者按照老派做法发一封邮件给我们。

​	本书有免费的PDF电子书，电子书版提供了更多的资源和Wikipedia入口。我们使用Wikiped来说明与本书直接相关的背景知识(如二进制数字系统)。电子书排版已经过优化，方便如ipad这样的便携式设备阅读。

## 3.6更多读物

​	以下列出了关于数字电路设计和Chisel更深层次的读物：

- Digital Design: A Systems Approach，作者是William J.Dally和R. Curtis Hating。这是一本现代数字设计教科书。它有两个版本分别使用Verilog和VHDL作为硬件描述语言。

​	Chisel官方文档和之后的文档都会更新到网上：

- Chisel主页是开始安装和学习Chisel的地方。
- Chisel Tutorial提供了一个含有测试和答案的小练习的热身项目。
- Chisel Wiki包含了简短的Chisel用户手册和更多信息的链接。
- Chisel Testers放在含有一个Wiki文档的仓库里。
- Generator Bootcamp是一个基于Jupyter Notebook写的Chisel课程，专注于硬件生成器。
- Chisel Style Guide，作者是Christopher Celio。
- Chisel-lab涵盖了丹麦技术大学“Digital Electronics 2”课程的Chisel练习。

## 3.7练习

​	每个章节结尾都有实践的练习题。对于入门练习，我们通常使用一个FPGA板来点亮一个LED使其闪烁。第一步先将chisel-examples仓库从Github克隆下来。Hello World例子在hello-world文件夹里，是最小的一个项目。你可以在目录src/main/scala/Hello.scala下查看闪烁LED的Chisel代码。按照如下步骤编译此工程：

```sh
$ git clone https://github.com/schoeberl/chisel-examples.git
$ cd chisel-examples/hello-world/
$ make
```

​	在Chisel部分下载完成之后，程序会生成一个Verilog文件Hello.v。浏览此Verilog文件，你会看到电路包含两个输入clock和reset以及一个输出io_led。当你对比Verilog文件和Chisel模块时，你会发现在Chisel文件的声明中不含clock和reset。这些信号是隐式生成的，不需要处理这些底层细节在大部分设计中都相当方便。Chisel提供的寄存器如果需要，会自动连接clock和reset。

​	下一步是建立FPGA工程文件，分配引脚然后编译Verilog代码，最后利用生成的bit文件烧录进FPGA。我们无法提供这些步骤的详细过程，请参考你的EDA工具手册。不过，例程仓库里包含了一些针对几个流行的FPGA的Quartus可用的工程在quartus文件夹中。如果仓库支持的板子包括你的FPGA，启动Quartus，打开工程，编译，烧录，然后你的LED会开始闪烁。

​	恭喜！你已经成功地在FPGA上运行了你第一个Chisel电路！

​	如果LED没有闪烁，检查reset的状态。在DE2-115配置中，reset的输入连接到SW0上。

​	现在改变闪烁的频率然后重新运行一遍构建工程。闪烁的频率和闪烁的方式可以预示不同的状态。例如一个缓慢的闪烁信号代表一切正常，一个快速的闪烁信号代表报警。探索一下哪个频率能更好地表示这两种状态。

​	如果你还没有一个FPGA板，你仍然可以运行LED闪烁的例子。你将会用到Chisel的模拟。为了防止太长的模拟实践，我们需要改变Chisel代码的时钟频率，从50000000改为50000。执行以下指令，模拟一个闪烁的LED。

```sh
$ sbt test
```

​	这会执行测试程序，运行时钟一百万次。闪烁的频率取决于模拟速度，也就是你的计算机速度。因此你看可能需要根据假设的时钟频率测试几次才能看到模拟的闪烁LED。

# 



