# Java 精简知识点

作者：牛客吴彦祖
链接：https://www.nowcoder.com/discuss/28656?type=0&order=0&pos=60&page=9
来源：牛客网

第一章
1. Java最大的特点就是可以在不同的平台环境上运行。
2. Java Write Once，Run Anywhere！
3. Java体系结构中立。
4. Java的版本分为:JSE 标准版；JEE 企业版；JME 小设备版。
5. Java与C++的区别：（-3+1）去掉指针，去掉多继承，去掉运算符重载；增加自动内存分配与回收机制。
6. Java面向对象的特点：封继多：封装、继承、多态。
7. Java分布式：Java的网络编程如同从文件发送和接收数据一样简单。
8. Java鲁棒性：异常处理机制、自动垃圾收集处理来进行内存管理。
9. Java安全性：不支持指针、沙箱运行模式（Java程序的代码和数据在一定的空间执行）
10. Java解释执行：代码被编译为JVM字节码，字节码的执行不依赖硬件配置。这是Java可以在不同平台上运行的基础。
11. javac name.java name.class java name
12. javac是编译命令  java是解释执行命令
13. package 一个程序最多只能有1句，且只能放在第一句。
14. import可有可无，有必须放在所有的类定义之前。
15. JVM是运行java字节码的一台虚拟机。
第二章
1.Java采用Unicode字符集，16位， ，支持大部分字符集。
2.Java标识符不能以数字开头，名称与大小写相关。
3.标识符的使用习惯：1名次首字母大写；2动名词动词首字母小写，名次首字母大写；3常量全大写；4变量首字母小写其他的大写。
4.Java数据类型：8个基本类型：byte、short、int、long、float、double、char、boolean；3个引申类型：class、interface、数组。
5.Java中逻辑类型和整数类型不能直接转换。
6.final修饰符号常量、其值在赋值之后不能改动。
7.类成员变量自动赋初值，局部变量不赋初值报错，引用作为类成员自动赋值为NULL。
8.Java中取消的操作符：->、*、&、sizeof、其中三个与指针相关。
9.基本类型赋值在栈里；引用类型赋值在堆里；引用本身位于栈。
10.引用直接用==比较的是地址，要比较对象的实际内容，用equal()方法。
11.Java中逻辑操作符&& || 有短路屏蔽，位操作符& |没有。
12.自动类型转换：低精度到高精度；高精度到低精度需要强制类型转换。
13.Java是结构化程序设计语言：顺序结构、选择结构、循环结构。
14.switch-case中只能用整型、字符型、枚举型、字符串。
15.break跳到最近的｝之后；continue跳出当前循环到下一次循环，当是while时直接判断条件，for还需要进行自加操作。
16.带有Lable的break和continue的区别：break跳到Lable之后跳过最近的｛｝；contine跳到Lable之后进入｛｝。
17.return返回结果、返回控制。

第三章
1.类变量 实例变量    类方法 实例方。
2.类变量、类方法既能用类名，也能用实例名访问。
3.实力变量、实例方法只能用实例名访问。
4.类方法中只能访问类变量，类方法中不能有this和super指针。
5.访问权限：public、protected、包（默认）、private。
6.protected：1自己用；2子类用；3同一包中用。
7.不同访问权限的访问个体有：1类；2包；3子类；4不同包中的非子类。
8.构造方法是一个特殊的方法，与类名相同，无返回类型，通过new()调用。
9.Java中没有析构函数，系统自动回收垃圾，对于不是通过new()申请的空间，用终结处理方法finalize()
10.对象的声明（标识符）；实例化（用new分配空间）；初始化（赋值）。
11.栈：基本类型和对象引用  堆：所有的Java对象，用new产生的数据。
12.类成员的初始化顺序：1静态成员；2组合对象成员；3类本身其他成员。
13.静态成员只初始化一次。
14.Java包有一定的层次，对应外存上的目录结构，同一包中不能有相同的类名。
15.程序中没有package语句时，放入默认包中。
16.一个编译单元只能有一个公共类public class，公共类名与文件名相同，还可以有多个支持类。
17.导入的两个包中有同名类时，使用时用全名来区分。
18.为导入包而用类库中的类时，用全名引用；自定义的包，java+类名。
19.java.lang  自动加载的默认包：基本数据的封装类、数学函数、字符串、线程等。
20.java.util  工具类库：日期、日历、堆栈、向量、集合、哈希集。
21.Object类：在lang包中，所有类的基类、定义所有类应有的公共函数。
22.System类：在lang包中，不能实例化，所有方法均可直接引用，与系统属性相关。
23.Math类：在lang包中，静态方法，不声明为实例，常用Math.PI、Math.E。
24.String类：在lang包中，字符常量，不用new创建时由栈中引用指向常量池（不重复）；用new创建时可重复。Substring截取中间一段，trim去掉两端空格。
25.StringBuilder、StringBuffer：对于经常要修改的字符串，StringBuilder效率高于StringBuffer，StringBuffer在线程上是安全的，在大多实现过程中，StringBuilder要比StringBuffer快。
26.String相当于字符中的常量，只要值变化，内存空间就变化。
27.StringBuffer中的方法：append to追加、insert插入、delete删除、reverse反转。

第四章
1.toString()函数，每个非基本类型中都有，未重写时默认输出为对象的内存地址。
2.组合：黑盒复用，依赖关系少，高效、动态。
3.继承：白盒复用，黏度高，易维护，破坏封装。
4.优先用组合。
5.super出现在构造方法中必须在第一行。
6.重写：子类修改父类已有的方法（覆盖），重写的方法名、返回类型、参数类型必须完全相同，且被重写的方法不能有更严格的访问权限，发生在多态中，无新受检异常。
7.重载：同一类中方法名相同参数列表不同，只能通过参数列表重载，不能通过访问权限、返回类型、抛出异常重载；重载可以有不同的返回类型、访问权限、异常，但是不作为重载的标志。
8.抽象类abstract抽象类不能实例化，类中有抽象方法，abstract class a1｛abstract void test()；｝函数声明之后有｛｝就算实现。
9.构造方法、静态方法、final方法不能使用abstract修饰。
10.接口默认为抽象类，实际上接口是抽象抽象类。
11.final类不能被继承，因此final类中的成员函数没有机会被覆盖，默认都是final的。
12.final方法：如果一个类不允许其子类覆盖某个方法，将这个方法声明为final。
13.将类方法声明为final更加高效，编译时绑定，静态绑定。
14.final变量常量，修饰的成员的值一旦给定就无法改变：静态变量、实例变量、局部变量。
15.static和final变量能够自动赋值为默认值。
16.this()本类的构造函数  this.本类的变量或方法
Super()超类的构造函数  super.超类的变量或方法
16. 子类和超类有重名变量时，用this和super区分。
17. 向上转型：子类转型为超类；超类的引用指向子类的对象。
18. 超类的引用不能调用：1子类中定义而超类中没有定义的方法；2子类重写的方法。
19. 只能将子类类型的引用赋值给超类类型的引用。
20. 每个类实例对象都自带一个虚函数表，这个表中存储的是指向虚函数的指针。
21. 超类和子类中都有的方法，才有多态的动态绑定问题。
22. 重写的重要特征：除了函数的实现以外其他部分都相同；覆盖：均相同才能覆盖。
23. Java中所有的成员方法都包含在虚表中，而C++中只有声明为virtual才是虚函数。
24. 动态绑定和静态绑定的适用条件：静态：private、static、final、构造函数。
25. 动态绑定的过程中，先在对象的实际类型的虚表中匹配，找不到的话再上溯到它的基类的虚表中找。
26. this指针始终指的都是实际类型。
27. 动态绑定指的是成员方法，成员变量始终都是静态绑定。
28. 想要动态的调用变量成员，将变量成员封装在方法中返回。
29. 重写是覆盖，同名的方法仅有一个；重载是并列，哪个参数匹配调用哪个。
30. 多态的三种实现方式：1继承实现；2抽象类实现；3接口实现。
31. 接口中只有常量和抽象方法。
32. Java接口中的成员变量默认都是public static final类型的，这些关键字都可以省略，但是必须初始化。
33. Java接口中的方法默认都是public abstract类型，不能有方法体，不能实现。
34. 抽象类和接口的区别：抽象类中可以有写实的方法，但至少要有一个方法是抽象的；接口中所有的方法都必须是抽象的。
35. Java接口中只能包含public static final成员变量和public abstract成员方法。
36. 接口继承接口，类实现接口，类继承类。
37. Java接口必须通过类来实现。
38. 类实现接口时，当所有的方法没有全部被写实时，类应该声明为抽象类。
39. 不允许创建接口实例，但是允许定义接口的引用指向实现了该接口的类实例对象。（类似于向上转型）
40. 抽象类只能在继承体系中实现，而接口可以任意实现。
41. 接口中的常量可以是不定常量，其值会在第一次访问是建立，然后保持不变。
42. 内部类的访问权限控制符有：public、default、protected、private，嵌套内部类static。
43. 创建非静态内部类时，一定要先创建其相应的外部类对象。
44. outerclass.innnerclass innerobject=outerobject.new innerclass()
45. 创建嵌套类对象时，不需要创建其外部类对象。
46. outerclass.innnerclass innerobject=outerclass.new innerclass()
47. 静态内部类不能访问外部类的非静态成员。
48. 局部内部类：定义在一个方法或一个语句块中。
49. 局部内部类访问外部类的局部变量时，只能访问final类型的。
50. 局部内部类的访问优先级：内部类>外部类>局部final变量。
51. 内部类的作用：允许protected和private权限，实现更小层次的封装。
52. 内部类一般与接口联合使用，更好的实现多重继承。
53. 匿名内部类：new 接口名(){}    new 超类名(){}
54. 内部类编译时生成的.class文件outerclassname$innerclassname.class   匿名内部类outerclassname$#.class  #为数字，从1开始，每生成一个，数字递增1。
55. 在内部类中调用this和new返回一个外部类时的不同，this得到的是创建该内部类的外部类对象的引用；而new创建一个新的引用。
56. 有内部类的实例作为外部类的成员时，构造该外部类时，先创建内部类的成员实例，再创建外部类实例的其他成员。（相当于组合关系）
57. 一个外部类的内部类被另一个类继承时，在该子类的构造方法中用外部类的一个实例调用超类的构造函数。Outerobject.super()。
58. 一般的super()调用默认的都是extends类名中的构造函数。
59. class son extends father{super();}
60. class son extends father.innerclass{fatherobject.super();}


第五章
1.AWT：重量级控件：靠本地方法实现其功能，不同的平台上不统一。
2.Swing：轻量级控件：提供了AWT的所有功能，并大幅扩充。
3.Swing中除了JFrame、JApplet、JDialog、JWindow之外都是轻量级控件。
4.Button在AWT中，JButton在Swing中；其他的名称类似。
5.Java采用向容器中添加组件的方式构建图形界面，通常用顶级容器作为所有组件的承载物。
6.容器之间可以完全嵌套。
7.GUI组件分为：容器类、控件类、辅助类。
8.常用的组件及相关函数
JFrame（窗口）：setsize、setvisible、setlocation、setdefaultcloseoperation、getcontentpane
JButton（按钮）：settext、seticon、setbounds、addactionlistener
JLable（标签）：settext、seticon、settooltiptext、sethorizontextposition
JTextField（文本框）：addactionlistener、setcolums、settext
JRadioButton（单选框）；JDialog（对话框）；JFileChooser（文件对话框）；JComboBox（组合框）：JTextField（文本域）、JMenu（菜单）。
9.布局管理：Container中有一个方法交setLayout()
10.不同的布局方式有：1BorderLayout；2FlowLayout；3GridLayout
11. BorderLayout是JFrame的默认布局，组件分为5个区域，东西南北中。
12.add(位置，组件)
13. FlowLayout顺序布局：从左到右、从上到下、
14.GridLayout网格布局：组件在网格中从左到右、从上到下。
15.事件处理：1事件；2源事件；3事件处理者。
16.事件处理的步骤：1注册事件监听器；2外部作用于事件源；3生成事件对象；4将事件对象传给事件处理器；5事件处理器进行处理。
17.定义监听器：actionPerformed、new创建监听器对象、注册监听器add***actionlistener()
18.监听器的三种实现方式：1实现注册写实；2内部类；3匿名内部类的实现。
19.GUI编程中要用的三个包：import java.awt.*；import javax.swing.*；import java.awt.event.*。
20.适配器：Adapter：适配器产生的必要性：接口中有很多抽象函数未写实，当要用到其中的某个功能时，需要全部写实，带来不便；适配器实现了所有方法却不做任何事情，可被继承然后只需写实要用到的那个方法而不用管其他方法。
21.适配器是处于接口和类之间的一个过渡性产物，省去很多实现的麻烦。



第六章
1.异常的分类：1Exception经过处理能恢复正常，不中断现有程序；2Error无法恢复必须中断现行程序退出。
2.常见的异常：ArithmeticExption数学异常、OutofMemory内存溢出异常、NullPointerExption空指针异常、IndexOutofMemoryExption下标边界异常。
3.错误由JVM抛出。
4.异常分为受检异常和不受检异常：1不受检异常（编译器可处理）：不要求程序员处理，最终可从main()抛出并用printStackTrace()一般是逻辑错误，包括RuntimeException和Error及其子类；2受检异常：强制程序员检测和处理。
5.异常的处理方法：1用try-catch-finally结构捕获和处理；2将异常层层向上抛直至转交给JVM处理。
6.多个catch块时只会匹配其中一个异常类并执行catch的代码，catch匹配的顺序是从上到下。
7.无论try语句块是否发生异常，finally语句块都是要执行的。
8.try-catch-finally三个语句块不能单独使用，可组成try-catch-finally、try-catch、try-finally三种结构，catch块可以有多个，finally块只能有一个。
9.try-catch-finally块中的变量的作用域为语句块内部，分别独立不能相互访问。
10.catch块的参数有父子关系时，子类在前，父类在后。
11.返回类型+函数名称+形参列表+throws Exception{}
12.异常传递链：不处理不执行，方法在抛出异常的地方退出，不想终止方法就用try来捕获异常。
13.自定义异常在构造函数中super()来调用Exception类的构造函数。
14.throws方法声明时放在方法中，throw在方法体中，在一个具体的动作。
15.如果抛出的异常一直未被处理，沿函数一直往上抛，从主函数中抛出之后由JVM处理，此时可以调用printStackTrace()打印这一异常所在的位置、类型等信息。

第七章
1.Java中数组的下标从0开始编号，数组中除了元素之外还存在唯一的可被访问的属性length。
2.数组声明：类型[]数组名   类型 数组名[]    []表示数组类型
3. 数组的初始化：静态初始化、动态初始化。
4.静态初始化：[]中没有数字，各个元素的值显性给出。
5.动态初始化：[]中有数字或变量，用关键字new，数组中各个元素可以自动赋值。
6.new申请空间时在堆上，在堆上均可自动赋值。
7.对象数组的初始化：类型[]数组名=new 类型{new 构造方法()…}
8.对于大型的数组或对象数组，一般结合循环语句来赋值。
9.多维数组：n维数组中存放着n-1维数组的引用。
10.不规则数组：数组元素引用的数组长度不等：a.length指的是第一维数组长度；a[i].length指的是对应维数数组的长度。
11.数组是一个特殊的对象，数组名存放一个对象的引用。
12.数组名实质是一个引用，所以可以作为参数。
13.数组工具类：Arrays，类函数：copyof数组中的值拷贝；sort升序排序；binarysearch查找特定值；equals判断两个数组是否相等。
14.对象比较接口：java.lang.comparable  int compareTo(Object o)
java.util.comparator  int compare(Object o1, Object o2)
15.一个类用sort比较大小时，自动调用compareTo方法比较大小。
16.枚举：有限的、不变的值的集合。
17.enum类型变量只能保存声明时给定的枚举值。
18.数组：静态容量，容量的一开始就定了。
19.类集Collection，是动态容量：有两个子集：1List对象有顺序，允许重复；2Set无序，不能重复。
20.Map映射：键值对，键是唯一的，值可以重复。
21.List：(有顺序，靠下标索引)ArrayList  LinkedList
22.List l=new ArrayList()，改变集合类型时代码不用变化，类似于接口引用一个实现了它的类，当使用类特有的方法时，不能用接口定义，而应该使用具体的类。
23.ArrayList数组内部维护了一个Object类型的数组；LinkedList内部维护了一个有头结点的双向链表。
24.使用LinkedList可以很简单的构造堆栈、队列，addFirst、addLast、getFirst、getLast、removeFirst。
25.泛型，C++中的模版，声明泛型时，使用尖括号指定类型参数，应用时用具体的类型填入类型参数。
26.泛型：类模版、函数模版。
27.声明 class 类名<类型参数列表>{}
28.应用 类名<具体的类型列表>变量名=new类名<具体的类型列表>(构造函数的参数列表)。
29.类、接口、方法都能使用泛型。
30.集合Set：不允许重复，无下标索引，常用的集合HashSet、TreeSet、LinkedHashSet。
31.system.out.println(set)用结合作为参数能直接输出结合中的元素，输出形式为[…,…,…]
32.迭代器：Iterator设计模式、对象，特别为遍历无索引的集合而准备。
33.Iterator中的函数：next()首次使用时，返回序列的第一个元素，之后获得序列中的下一个元素并向后移；hasnext()判断是否有下一个。
34.迭代器的使用：判断it.hasnext();取出类型=it.next()
35.for-each遍历，对迭代器进行了包装，代码更加简洁，只能遍历两种类型：1数组；2实现了Iterator接口的实例。
36.哈希集：HashSet：用哈希表和哈希算法实现快速查找，折中了数组和链表，内部维护了一个链表数组，拥有常数查找时间，要求对象重写hashCode、和equal方法。
37.哈希表类似于邻接表，散列、Cache映射寻址中的组相连。
38.哈希表工作的两大步骤：1获得哈希码，算出该插入哪个链表；2在链表上比较。
39.容量：链表数组的大小，载入因子，重构的条件；当元素数目>容量*载入因子时，重构翻倍。
40.HashSet中没有相同的元素，所以内容equals的对象，hashcode一定也相等，hashcode不相等的两个对象一定不equals。
41.hashcode默认返回的是内存地址，equals默认比较的也是内存地址。
42.哈希集中一般选一个不重复的关键词作为哈希码。
43.TreeSet元素间有序但不能重复，，自平衡二叉树，必须实现comparable和compator接口来比较，写实equal保证唯一性，查找效率为 。
44.LinkedHashSet哈希集+链表，在HashSet基础上将元素串起来，元素有序。
45.输入同一组数据，用不同的集输出，HashSet元素存放无序，LinkedHashSet保持添加顺序，TreeSet按排序存放。
46.key-value关系对的无序容器，键值均为对象，键值对也为对象。
47.一个映射Map中不能有相同的key，每个key只能有一个value
48.Map不能用迭代器，无序，无法索引。
49.遍历Map时，通过将映射转换为键或值来进行遍历。
50.List Set Map为接口，不能实例化。
51.泛型<>的位置，函数：在访问控制符之后，类，接口在名称之后。理解：必须在可能出现类型的地方之前
52.collection是list和set的父类，collection工具类，静态类，有常用的操作。


第八章
1.创建好的输入输出系统是艰难的工作：1与不同的源和目的端进行交互；2以不同的方式进行通信；3大多数I/O需要异常处理。
2.Java通过各种封装的流类完成输入输出工作。
3.流：想象中无限长的数据序列1输入流输出流；2字符流字节流；3节点流处理流。
4.字节输入：InputStream、字节输出OutputStream、字符输入Reader、字符输出Writer。
5.ByteArrayInputStream：将内存的缓冲区来当作InputStream使用。
6.StringBufferInputStream：将String转换成InputStream。
7.FileInputStream：从文件中读取信息。
8.PipedInputStream：产生用于写入相关数据，实现管道化。
9.ObjectInputStream：对象序列化输入流。
10.SequenceInputStream：将两个或多个InputStream对象转换成单一的InputStream。
11.DataInputStream：从流中读取基本的数据类型(int char long…)
12.BufferedInputStream：使用缓冲区读入，防止每次读取都要进行操作破坏硬盘。
13.LineNumberInputStream：跟踪输入流的行号。
14.节点流：从特殊位置读写的基本功能。
15.处理流：结合其他流的基础上，修改管理数据并提供额外功能。
16.常见的处理流：1缓冲Buffered；2过滤：Filter；3转换InputStreamReader；4对象序列化ObjectInputStream；5打印PrintStream。
17.缓冲流：不直接读写，对硬盘损害小，嵌套在其他节点流之上，提高效率。
18.缓冲流在构造时，一般有两个参数：1要包装的流类；2要缓冲的数据的大小。
19.mark标记流中当前的位置；reset定位到最近的标记点；readline读一行；flush刷新。
20.打印流，不抛异常，自动刷新。
21.数据流：支持基本类型的输入输出，继承自InputStream、OutputStream。
22.流的难点在于：搞清格流之间的关系，流之间如何嵌套，如何用一个流若为参数来构造另一个流。
23.处理流需要在节点流的基础上封装构造，所以识别基本的节点流和处理流是一项基本功。
24.处理流在使用完毕之后要调用close函数，处理流是动态的，不关一直起作用。
25.标准I/O流，System.in键盘输入；System.out屏幕输出；System.err错误输出。
26.各种流类的嵌套定义和初始化时遵循一个原则，简单的被复杂的嵌套，简单的作为复杂的参数。
27.Scanner类：扫描输入文本，除了可用正则表达式之外，还可分析字符串和基本类型。
28.Scanner的使用： Scanner s=new Scanner(System.in)  int ss=s.nextInt()
29.使用输入输出时导入一个包import java.io.*;
30.使用Scanner是import java.util.*;
31.文件输入输出流：FileInputStream、FileOutputStream 、FileReader、FileWriter。
32.File类的构造：public File(String pathname)相对路径、绝对路径、父路径、子路径。
33.mkdir创建文件夹 mkdirs在不存在为文件中创建文件夹
34.有关文件的输入输出：int b=0，while((b=in.reader()!=-1)){}
35.随机访问文件：适用于大小已知的记录组成的文件，seek到指定位置，getFilePointer得到当前指针位置。
36.length判断文件大小。
37.RandomAccessFile生成随机访问文件。

第九章
1.系统级并发，单个的一段程序执行控制流称为线程。
2.多线程：一个程序内有多个相同或不同的线程或执行不同的任务。
3.多线程的作用：1提高UI的响应速度；2提高硬件资源的利用率；3隔离高速硬件和低速硬件；4提供程序上的隔离不同的运行模块。
4.实现多线程的程序设计关键：获得Thread实例，重写run方法，调用start方法开始线程并分配资源。
5.从run方法中返回是退出线程唯一正确的方法。
6.start是线程从系统外分配资源，run是线程获得资源后要进行的工作。
7.线程类Thread是Object的直接子类，成员属性：PRIORITY优先级。
8.Thread的成员方法：sleep休眠指定的秒数，start启动线程，yield挂起暂停，run执行，notify唤醒。
9.isLive判断线程是否处于活动状态，getName返回线程名称，currentThread返回正在执行的线程的引用。
10.继承Thread：1继承Thread类；2重写run方法；3new创建新对象；4调用start启动线程。
11.线程的不确定性：同一线程内执行的顺序是一定的，多个线程执行顺序有多种组合。
12.用继承Thread类的方式实现多线程，被Java多继承的限制不能再继承别的类。
13.用实现Runnable方法实现多线程：（更加通用的多线程方法），1实现Runnable方法；2重写public void run(){}；3用new来实例新对象；4调用start启动线程。
14.通过继承和实现两种方法形成的线程类在使用时有区别：1继承实现的线程类能直接实例化；2通过实现的线程类实例化之后作为new Thread()的参数。
15.线程的生命周期：出生、就绪、运行、休眠、等待、阻塞、死亡。
16.两个线程的依赖关系：某格线程在另一个线程终止时才能继续执行，可调用另一个join的方法当另一个线程死亡时，该线程从等待态进入就绪态。
17.线程被创建时，从父线程中继承优先级，创建之后也能改变优先级。
18.优先级定义分为10级。
19.线程优先级的作用：方便操作系统的调度，优先级相同且不实用分时技术时，一个运行完才运行下一个，除非他自己变为其他状态。
20两个线程同时访问一个成员，造成对共享数据的冲突，造成一致破坏，为保证数据的一致性，引入同步锁sunchronized
21.同步分为方法同步和语句同步，方法同步在函数类型之前加sunchronized；语句同步在{}之前加sunchronized。
22.协作机制：生产者消费者模型。
23.死锁的解决：1逻辑隔离：每个同步区使用各自的锁；2尽快释放：使用完尽快释放锁对象；3按顺序申请：用到多格锁对象的申请一致。

第十章
1.数据库的组成：数据库、数据库管理系统、应用系统、数据库管理员、数据库用户。
2.数据类型：网状模型、层次模型、关系模型。
3.关系模型数据库是目前最流行的数据库。
4.数据库的数据有最小的冗余度和较高的数据和程序独立。
5.SQL语言，常见的SQL查询命令
查询：selsct…from…where
操纵：insert、updata、delete
定义：creat、alter、drop
6.JDBC是为了JAVA专门和数据库交互的一套接口，用于执行SQL语句的JavaAPI。
7.JDBC的基本功能：1加载JDBC驱动程序；2建立与数据库的联系；3使用SQL语言进行数据库操作和处理结果；4关闭相关链接。
8.写数据库程序要导入的包：import java.sql.*。