#Android 入门笔记-第二章
---

##2.1 活动是什么

活动（Activity）是最容易吸引到用户的地方了，它是一种可以包含用户界面的组件， 主要用于和用户进行交互。一个应用程序中可以包含零个或多个活动

---

##2.2 活动的基本用法

###2.2.1 手动创建活动

选择Blank Activity,你应该在 src目录下先添加一个包,新建类继承自 Activity.

在FirstActivity中重写onCreate() 方法:
```
@Override                  //@Override表示这个方法是覆盖了父类的同名方法
protected void onCreate(Bundle savedInstanceState) 
{   
      super.onCreate(savedInstanceState);  
}
```
###2.2.2 创建和加载布局 

Android 程序的设计讲究逻辑和视图分离，最好每一个活动都能对应一 个布局，布局就是用来显示界面内容的

Graphical Layout是当前的可视化布局编辑器， first_layout.xml 则是通过 XML 文件的方式来编辑布局。

android:id是给 当前的元素定义一个唯一标识符。如果你需要在XML中引用一个id，就使用@id/id_name这种语法，而如果你需要在 XML中 定义一个 id，则要使用@+id/id_name 这种语法

android:layout_width 指定了当前元素的宽度，这里使用match_parent表示让当前元素和父元素一样宽。android:layout_height指定 了当前元素的高度，这里使用wrap_content，表示当前元素的高度只要能刚好包含里面的内 容就行。android:text指定了元素中显示的文字内容

setContentView()方法来给当前的活动加载一个布局，而在setContentView()方法中，我们一般都会传入一个布局文件的id。

###2.2.3 在 AndroidManifest 文件中注册 

所有的活动都要在 AndroidManifest.xml中进行注册才能生效

活动的注册声明要放在<application>标签内，这里是通过<activity>标签来对活动进行注册.

标签里添加了<action android:name= "android.intent.action.MAIN" />和<category android:name="android.intent.category.LAUNCHER"/>这两句声明,作用为点击桌面应用程序图标时首先打开的就是这个活动。

###2.2.4 隐藏标题栏

requestWindowFeature(Window.FEATURE_NO_TITLE)不在活动中显示标题栏，注意这句代码一定要在 setContentView()之前执行，

###2.2.5 在活动中使用 Toast 

Toast是 Android系统提供的一种非常好的提醒方式，在程序中可以使用它将一些短小的信息通知给用户。

在活动中，可以通过 findViewById()方法获取到在布局文件中定义的元素，。findViewById()方法返回的是一个 View 对象，我们需要向下转型。

调用 setOnClickListener()方法为按钮注册一个监听 器，点击按钮时就会执行监听器中的 onClick()方法。

通过静态方法 makeText()创建出一个Toast对象，然后调用show()将Toast显示出来，makeText()方法需要传入三个参数。第一个参数是 Context，也就是Toast要求的上下文，由于活动本身就是一个 Context对象，因此 这里直接传入*.this即可。第二个参数是Toast显示的文本内容，第三个参数是Toast显示的时长，有两个内置常量可以选择 Toast.LENGTH_SHORT和 Toast.LENGTH_LONG

```
@Override   
public void onClick(View v) 
{   
    Toast.makeText(FirstActivity.this, "You clicked Button 1",      
    Toast.LENGTH_SHORT).show(); 
}
```

###2.2.6 在活动中使用 Menu 

<item>标签就是用来创建具体的某一个菜单项，然后通过android:id给这个菜单项指定一个唯一标识符，通过android:title给这个菜单项指定一个名称

打开 FirstActivity，重写onCreateOptionsMenu()方法.

```
public boolean onCreateOptionsMenu(Menu menu) 
{  getMenuInflater().inflate(R.menu.main, menu);  
return true; 
} 
```
通过 getMenuInflater()方法能够得到 MenuInflater对象，再调用它的 inflate()方法就可以给 当前活动创建菜单了。inflate()方法接收两个参数，第一个参数用于指定我们通过哪一个资源 文件来创建菜单，第二个参数用于指定我们的菜单项将添加到哪 一个 Menu对象当中.

再定义菜单响应事件。在 FirstActivity中重写onOptionsItemSelected()方法

###2.2.7 销毁一个活动 

Activity 类提供了一 个 finish()方法，我们在活动中调用一下这个方法就可以销毁当前活动了.

---

##2.3 使用 Intent 在活动之间穿梭

Intent 是 Android 程序中各组件之间进行交互的一种重要方式，它不仅可以指明当前组 件想要执行的动作，还可以在不同组件之间传递数据。Intent 一般可被用于启动活动、启动 服务、以及发送广播等场景.Intent的用法大致可以分为两种，显式 Intent和隐式 Intent

###2.3.1 使用显式 Intent 

Intent 是 Android 程序中各组件之间进行交互的一种重要方式，它不仅可以指明当前组 件想要执行的动作，还可以在不同组件之间传递数据。Intent 一般可被用于启动活动、启动 服务、以及发送广播等场景

Intent有多个构造函数的重载，其中一个是 Intent(Context packageContext, Class<?> cls)。 这个构造函数接收两个参数，第一个参数 Context要求提供一个启动活动的上下文，第二个 参数 Class则是指定想要启动的目标活动，通过这个构造函数就可以构建出 Intent的“意图”。 

Activity类中提供了一个 startActivity()方法，这个方法是专门用于启动活动的，它接收一个Intent参数，这里我们将构建好的Intent传入startActivity() 方法就可以启动目标活动了.

```
Intent intent = new Intent(FirstActivity.this, SecondActivity.class); 
startActivity(intent); 
```

###2.3.2 使用隐式 Intent 

使用隐式 Intent，我们不仅可以启动自己程序内的活动，还可以启动其他程序的活动，使Android多个应用程序之间的功能共享成为了可能。隐式Intent不明确指定要启动哪个活动, 而是指定抽象的action category等信息, 让系统去分析该启动哪个活动。

在<action>标签中我们指明了当前活动可以响应 com.example.activitytest.ACTION_ START 这个 action，而<category>标签则包含了一些附加信息，更精确地指明了当前的活动 能够响应的Intent中还可能带有的 category。只有<action>和<category>中的内容同时能够匹 配上 Intent中指定的 action和 category时，这个活动才能响应该 Intent。

每个 Intent中只能指定一个 action，但却能指定多个 category。

[Android程序调试–LogCat](http://my.oschina.net/liuher/blog/293594)

###2.3.3 更多隐式 Intent 的用法

可以在<intent-filter>标签中再配置一个<data>标签，用于更精确地指定当前活动能够响应什么类型的数据。

1. android:scheme 用于指定数据的协议部分，如上例中的 http部分。 
2. android:host 用于指定数据的主机名部分，如上例中的 www.baidu.com部分。
3. android:port 用于指定数据的端口部分，一般紧随在主机名之后。 
4. android:path 用于指定主机名和端口之后的部分，如一段网址中跟在域名之后的内容。 
5. android:mimeType 用于指定可以处理的数据类型，允许使用通配符的方式进行指定。 

###2.3.4 向下一个活动传递数据 

Intent中提供了一系列 putExtra()方法的重载，可以把我们想要传递的数据暂存在 Intent 中，启动了另一个活动后，只需要把这些数据再从 Intent 中取出就可以了。

```
String data = "Hello SecondActivity";  
Intent intent = new Intent(FirstActivity.this, SecondActivity.class);   intent.putExtra("extra_data", data);   
startActivity(intent); 
```

```
Intent intent = getIntent();   
String data = intent.getStringExtra("extra_data");   
Log.d("SecondActivity", data); 
```

###2.3.5 返回数据给上一个活动 

Activity中还有一个 startActivityForResult()方法也是用于启动活动的，但这个方法期望在活动销毁的时候能够返回一个结果给上一个活动.

startActivityForResult()方法接收两个参数，第一个参数还是 Intent，第二个参数是请求 码，用于在之后的回调中判断数据的来源.

setResult()方法是专门用于向上一个活动返回数据的。接收两个参数，第一个参数用于向上一个活动返回处理结果，一般只使用 RESULT_OK或RESULT_CANCELED这两个值，第二个参数则是把带有数据的Intent传递回去，然后调用了finish()方法来销毁当前活动。

---

##2.4 活动的生命周期 

###2.4.1 返回栈 
 Android使用任务（Task）来管理活动，一个任务就是一组存放在栈里的活动的集合，这个栈也被称作返回栈（Back Stack）。栈是一种后进先出的数据结构。按下Back键或调用finish()方法去销毁一个活动时，处于栈顶的活动会出栈，这时前一个入栈的活动就会重新处于栈顶的位置。系统总是会显示处于栈顶的活动给用户。 

###2.4.2 活动状态

每个活动在其生命周期中最多可能会有四种状态。 
1. 运行状态 
  当一个活动位于返回栈的栈顶时，这时活动就处于运行状态。
2. 暂停状态
 当一个活动不再处于栈顶位置，但仍然可见
3. 停止状态 
 当一个活动不再处于栈顶位置，并且完全不可见的时候
4. 销毁状态 
 当一个活动从返回栈中移除后就变成了销毁状态

###2.4.3 活动的生存期 

Activity 类中定义了七个回调方法，覆盖了活动生命周期的每一个环节。
1. onCreate() 
  它会在活动 第一次被创建的时候调用，在这个方法中完成活动的初始化操作，比如说加载布 局、绑定事件等。
2. onStart() 
  这个方法在活动由不可见变为可见的时候调用。 
3. onResume() 
  这个方法在活动准备好和用户进行交互的时候调用
4. onPause()
 这个方法在系统准备去启动或者恢复另一个活动的时候调用。
5. onStop() 
 这个方法在活动完全不可见的时候调用
6. onDestroy() 
 这个方法在活动被销毁之前调用，之后活动的状态将变为销毁状态
7. onRestart() 
 这个方法在活动由停止状态变为运行状态之前调用，也就是活动被重新启动了

1. 完整生存期 
2. 可见生存期 
3. 前台生存期 

###2.4.4 体验活动的生命周期



###2.4.5 活动被回收了怎么办

onSaveInstanceState()方法会携带一个Bundle类型的参数，Bundle提供了一系列的方法用于保存数据，比如可以使用 putString()方法保存字符串，使用putInt()方法保存整型数据，以此类推。每个保存方法需要传入两个参数，第一个参数是键，用于后面从 Bundle中取值， 第二个参数是真正要保存的内容

Intent 还可以结合 Bundle 一起用于传递数据的，首先可以把需要传递的数据都保存在 Bundle 对象中，然后再 将 Bundle 对象存放在 Intent 里。
```
         Intent intent = new Intent();
         Bundle bundle = new Bundle();
         bundle.putString("key_height", fieldHeight.getText().toString());
         bundle.putString("key_weight", fieldWeight.getText().toString());
         intent.setClass(ActivityMain.this,Report.class);
         intent.putExtras(bundle);
         startActivity(intent);  
```

```
         Bundle bundle = new Bundle();
         bundle = this.getIntent().getExtras();
         double height = Double.parseDouble(bundle.getString("key_height"))/100;
         double weight = Double.parseDouble(bundle.getString("key_weight"));
```

---

##2.5 活动的启动模式 

启动模式一共有四种，分别是 standard、singleTop、 singleTask 和 singleInstance，可以在 AndroidManifest.xml 中通过给<activity>标签指定 android:launchMode属性来选择启动模式

###2.5.1 standard
standard 是活动默认的启动模式，在不进行显式指定的情况下，所有活动都会自动使用这种启动模式。
每当启动一个新的活动，它就会在返回栈中入栈，并处于栈顶的位置，对于使用standard模式的活动，系统不会在乎这个活动是否已经在返回栈中存在，每次启动都会创建 该活动的一个新的实例。

###2.5.2 singleTop
当活动的启动模式 指定为singleTop，在启动活动时如果发现返回栈的栈顶已经是该活动，则认为可以直接使用它，不会再创建新的活动实例。

###2.5.3 singleTask
当活动的启动模式指定为 singleTask，每次启动该活动时系统首先会在返回栈中检查是否存在该活动的实例，如果发现已经存在则直接使用该实例，并把在这个活动之上的所有活动统统出栈，如果没有发现就会创建一个新的活动实例。

###2.5.4 singleInstance 
不同于以上三种启动模式，指定为singleInstance模式的活动会启用一个新的返回栈来管理这个活动（如果 singleTask模式指定了不同的 taskAffinity，也会启动一个新的返回栈）

---

##2.6 活动的最佳实践

###2.6.1 知晓当前是在哪一个活动 
修改 FirstActivity、SecondActivity和 ThirdActivity的继承结构，让它们不再继承自 Activity，而是 继承自 BaseActivity。通过这种方法可以看出运行的活动名称。

###2.6.2 随时随地退出程序 

在活动管理器中，我们通过一个 List来暂存活动，然后提供了一个 addActivity()方法用 于向 List 中添加一个活动，提供了一个 removeActivity()方法用于从 List 中移除活动，最后 提供了一个 finishAll()方法用于将 List中存储的活动全部都销毁掉。
