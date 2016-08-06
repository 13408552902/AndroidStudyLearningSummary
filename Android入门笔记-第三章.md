#Android 入门笔记-第三章
---

##3.1 该如何编写程序界面

使用可视化编辑工具并不利于去真正了解界面背后的实现原理，而且当需要编写较为复杂的界面时，可视化编辑工具将很难胜任。

---

##3.2 常见控件的使用方法 
###3.2.1 TextView 

主要用于在界面上显示一段文本信息

android:layout_width指定了控件的宽度，使用 android:layout_height指定了控件的高度

match_parent、fill_parent 表示让当前控件的大小和父布局的大小一样，也就是由父布局来决定当前控件的大小
wrap_content     表示让当前控件的大小能够刚好包含住里面的内容

使用 android:gravity来指定文字的对齐方式，可选值有 top、bottom、left、right、center 等 ， 可 以 用 “ | ” 来 同 时 指 定 多 个 值 

android:textSize属性可以指定文字的大小，android:textColor属性可以指定文字的颜色

###3.2.2 Button 

使用匿名类的方式来注册监听器
使用实现接口的方式来进行注册

附：界面设计与书中不符，需要手动调整。
android:layout_alignParentLeft="true"
android:layout_below="@+id/text_view"
android:layout_marginTop="28dp"

###3.2.3 EditText 

EditText 是程序用于和用户进行交互的另一个重要控件，它允许用户在控件里输入和编 辑内容，并可以在程序中对这些内容进行处理。

android:hint 属性来指定了一段提示性的文本
android:maxLines指定了 EditText的最大行数

Toast.makeText(MainActivity.this, inputText, Toast.LENGTH_SHORT).show()，使用 Toast将输入的内容显示出来

###3.2.4 ImageView 

ImageView是用于在界面上展示图片的一个控件
使用 android:src属性给 ImageView指定了一张图片

在程序中调用图片:
imageView  = (ImageView) findViewById(R.id.image_view);
imageView.setImageResource(R.drawable.jelly_bean); 

###3.2.5 ProgressBar
ProgressBar用于在界面上显示一个进度条

Android控件的可见属性:
visible 表示控件是可见的
invisible表示控件不可见，但是它仍 然占据着原来的位置和大小，可以理解成控件变成透明
gone则表示控件不仅不可见， 而且不再占用任何屏幕空间

使用setVisibility()方法，可以传入 View.VISIBLE、View.INVISIBLE和 View.GONE三种值
通过 style属性 可以将它指定成水平进度条.

通过 android:max 属性给进度条设置一个最大值，然 后在代码中动态地更改进度条的进度

###3.2.6 AlertDialog 
AlertDialog可以在当前的界面弹出一个对话框，这个对话框是置顶于所有界面元素之上 的，能够屏蔽掉其他控件的交互能力,用于提示一些非常重要的 内容或者警告信息

首先通过 AlertDialog.Builder创建出一个 AlertDialog的实例，然后可以为这个对话框设 置标题、内容、可否取消等属性，接下来调用 setPositiveButton()方法为对话框设置确定按钮 的点击事件，调用 setNegativeButton()方法设置取消按钮的点击事件，最后调用 show()方法 。

###3.2.7 ProgressDialog 
ProgressDialog 和 AlertDialog 有点类似，都可以在界面上弹出一个对话框，都能够屏蔽 掉其他控件的交互能力。不同的是，ProgressDialog 会在对话框中显示一个进度条，一般是 用于表示当前操作比较耗时，让用户耐心地等待。

如果在 setCancelable()中传入了 false，表示 ProgressDialog是不能通过 Back键取消 掉的，这时你就一定要在代码中做好控制，当数据加载完成后必须要调用 ProgressDialog的 dismiss()方法来关闭对话框，否则 ProgressDialog将会一直存在。 

---

##3.3 详解四种基本布局 

布局是一种可用于放置很 多控件的容器，它可以按照一定的规律调整内部控件的位置，从而编写出精美的界面。当然， 布局的内部除了放置控件外，也可以放置布局。

###3.3.1 LinearLayout
LinearLayout 又称作线性布局，这个布局会将它所包含的控件在线性方向上依次排列。

android:orientation 属性定义排列方向。垂直方向 vertical，水平方向上horizontal。

LinearLayout的排列方向是 horizontal时，只有垂直方向上的对齐方式才会生效，竖直方向同理

android:layout_gravity是用于指定控件在布局中的对齐方式。android:gravity和android:layout_gravity的区别在于前者对控件内部操作，后者是对整个控件操作。

例如：android:gravity="center"是对textView中文字居中
      android:layout_gravity="center"是对textview控件在整个布局中居中
其实很容易理解，出现"layout"就是控件对整个布局的操作。

使用了 android:layout_weight属性，此时控件的宽度就不应该再由android:layout_width来决定，这里指定成0是一种比较规范的写法。系统会先把LinearLayout下所有控件指定的layout_weight值相加，得到一个总值。然后每个控件所占大小的比例就是用该控件的 layout_weight值除以刚才算出的总值。

###3.3.2 RelativeLayout
RelativeLayout 又称作相对布局，RelativeLayout显得更加随意一些，它可以通过相对定位的方式让控件出现在布局 的任何位置。

android:layout_alignParentLeft、 android:layout_alignParentTop、
android:layout_alignParentRight、android:layout_alignParentBottom、 
android:layout_centerInParent

 android:layout_below="@id/buttonx"         
 android:layout_toRightOf="@id/buttonx" 
 
### 3.3.3 FrameLayout 
这种布局没有任何的定位方式，所有的控件都会摆放在布局的左上角.

###3.3.4 TableLayout 
TableLayout 允许我们使用表格的方式来排列控件，TableRow 中的控件是不能指定宽度的.

在 TableLayout 中每加入一个 TableRow 就表示在表格中添加了一行，然后在 TableRow 中每加入一个控件，就表示在该行中加入了一列.

单元格进行合并,android:layout_span="x"让登录按钮占据x列的空间.

android:stretchColumns 允许将 TableLayout 中的某一列 进行拉伸，以达到自动适应屏幕宽度的作用. android:stretchColumns的值指定为1，表示如果表格不能完全占满屏幕宽度，就将第二列进行拉伸.指定成1就是拉伸第二列，指定成 0就是拉伸第一列

---

##3.4 自定义控件 

我们所用的所有控件都是直接或间接继承自 View 的，所用的所有布局都是 直接或间接继承自 ViewGroup.
View是 Android中一种最基本的UI组件，它可以在屏幕上绘制一块矩形区域，并能响应这块区域的各种事件，因此，我们使用的各种控件其实就是在View的基础之上又添加了各自特有的功能。而ViewGroup则是一种特殊的 View，它可以 包含很多的子 View和子 ViewGroup，是一个用于放置控件和布局的容器。 

###3.4.1 引入布局 

android:background 用于为布局或控件指定一个背景，可以使用颜色或图片来进行填充

android:layout_margin这个属 性，它可以指定控件在上下左右方向上偏移的距离，当然也可以使用 android:layout_marginLeft 或 android:layout_marginTop等属性来单独指定控件在某个方向上偏移的距离.

修改 activity_main.xml ,只需要通过一行 include语句将标题栏布局引入进来就可以了,<include layout="@layout/title" /> 。别忘了在 MainActivity中将系统自带的标题栏隐藏掉。

###3.4.2 创建自定义控件 
我们重写了 LinearLayout中的带有两个参数的构造函数，在布局中引入 TitleLayout 控件就会调用这个构造函数。然后在构造函数中需要对标题栏布局进行动态加载，这就要借 助 LayoutInflater 来实现了。通过 LayoutInflater 的 from()方法可以构建出一个 LayoutInflater 对象，然后调用 inflate()方法就可以动态加载一个布局文件，inflate()方法接收两个参数，第 一个参数是要加载的布局文件的 id，这里我们传入 R.layout.title，第二个参数是给加载好的 布局再添加一个父布局。

自定义控件已经创建好了，然后我们需要在布局文件中添加这个自定义控件，添加自定义控件和添加普通控件的方式基本是一样的，只不过在添加自定义控件的时候我们需要指明控件的完整类名，包名在这里是不可以省略的。

---

##3.5 最常用和最难用的控件——ListView 

###3.5.1 ListView 的简单用法 

在布局中加入 ListView 控件还算非常简单，先为 ListView 指定了一个 id，然后将宽度 和高度都设置为 match_parent，这样 ListView也就占据了整个布局的空间

ListView是用于展示大量数据的，这些数据可以是从网上下载的，也可以是从数据库中读取的。数组中的数据是无法直接传递给 ListView 的，我们还需要借助适配器来完成，最好用的实现类就是 ArrayAdapter。

此将 ArrayAdapter的泛型指定为合适的数据类型，然后在ArrayAdapter的构造函数中依次传入当前上下文、ListView 子项布局的 id，以及要适配的数据。，最后，还需要调用ListView的setAdapter()方法，将构建好的适配器对象传递进去。

###3.5.2 定制 ListView 的界面

定义一个实体类，作为 ListView适配器的适配类型，然后需要为 ListView 的子项指定一个我们自定义的布局。

重写了父类的一组构造函数，用于将上下文、ListView 子项布局的 id 和数 据都传递进来。另外又重写了 getView()方法，这个方法在每个子项被滚动到屏幕内的时候 会被调用。在 getView 方法中，首先通过 getItem()方法得到当前项的Fruit实例，然后使用LayoutInflater来为这个子项加载我们传入的布局，接着调用 View的 findViewById()方法分别 获取到 ImageView和 TextView的实例，并分别调用它们的 setImageResource()和 setText()方 法来设置显示的图片和文字，最后将布局返回。

###3.5.3 提升 ListView 的运行效率

getView()方法中还有一个 convertView参数，这个参数用于将之前加载好的布局进行缓存，以便之后可以进行重用。

新增了一个内部类 ViewHolder，用于对控件的实例进行缓存。

###3.5.4 ListView的点击事件

使用了 setOnItemClickListener()方法来为 ListView注册了一个监听器， 当用户点击了 ListView中的任何一个子项时就会回调 onItemClick()方法，在这个方法中可以 通过 position 参数判断出用户点击的是哪一个子项

---

##3.6 单位和尺寸

在布局文件中指定宽高的固定大小有以下常用单位可供 选择：px、pt、dp和 sp。

###3.6.1 px和 pt 的窘境 

px是像素的意思，即屏幕中可以显示的最小元素单元。

pt是磅数的意思，1磅等于 1/72英寸，一般 pt都会作为字体的单位。

dp是密度无关像素的意思，也被称作 dip，和 px 相比，它在不同密度的屏幕中的显示比 例将保持一致。

sp是可伸缩像素的意思，它采用了和 dp 同样的设计理念，解决了文字大小的适配问题。

Android 中的密度就是屏幕每英 寸所包含的像素数，通常以 dpi为单位。

---

##3.7 编写界面的最佳实践 

###3.7.1 制作 Nine-Patch 图片

Nine-Patch图片是一种被特殊处理过的 png 图片，能够指定哪些区域可以被拉伸而哪些区域不可以。

###3.7.2 编写精美的聊天界面

首先在主界面中放置了一个 ListView用于显示聊天的消息内容，又放置了一个EditText用于输入消息，还放置了一个 Button 用于发送消息。

然后定义消息的实体类Msg。

接着来编写 ListView子项的布局。这里我们让收到的消息居左对齐，发出的消息居右对齐，并且分别使用 message_left.9.png 和 message_right.9.png作为背景图

接下来需要创建 ListView的适配器类，让它继承自 ArrayAdapter，并将泛型指定为 Msg 类。

最后修改 MainActivity中的代码，来为 ListView初始化一些数据，并给发送按钮加入事件响应。

