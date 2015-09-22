##Android---菜单却换与底部菜单栏##

大家好，底部菜单栏很古老了，不管是用户习惯和还是体验都好，现在都还在使用。
实现菜单的效果其实很简单，xml 配置就可以完全实现。

 我们先来一个QQ中 消息和电话的切换菜单
 
 ![这里写图片描述](http://img.blog.csdn.net/20150922230446389)
 
###第一步:在activity_main.xml中添加 RadioGroup ###

```java
<RadioGroup
        android:id="@+id/radioGroup1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_centerHorizontal="true"
        android:layout_marginTop="58dp"
        android:orientation="horizontal" >

        <RadioButton
            android:id="@+id/message"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="@drawable/left_button_select"
            android:button="@null"
            android:checked="true"
            android:gravity="center"
            android:text="消息"
            android:textColor="@color/text_color" />

        <RadioButton
            android:id="@+id/phone"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:button="@null"
            android:gravity="center"
            android:text="电话"
            android:background="@drawable/right_button_select"
            android:textColor="@color/text_color"
            />
    </RadioGroup>
```
比较重要的是  首先设置 RadioButton 的 button 为null --> android:button="@null"
RadioGroup 是linearLayou 的子类 默认是 垂直的...我们设置成 android:orientation="horizontal"的

### 第二步: 设置RadioButton背景的圆角####

设置圆角我们可使用 Shape :

```
<shape android:shape="rectangle"
  xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="#D0E5C6" />
    <corners android:radius="8.0dip" />
</shape>

<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android" >
    
   <!-- 设置圆角半径 
     同时设置五个属性，则Radius属性无效
     radius ==> 四角的半径
     topLeftRadius=>左上角
     topRightRadius=>右上角
     bottomLeftRadius=>左下角
     bottomRightRadius=>右下角
     -->
    <corners
        android:radius="10dp"
        android:topLeftRadius="10dp"
        android:topRightRadius="10dp"
        android:bottomLeftRadius="10dp"
        android:bottomRightRadius="10dp"/>
    
    <!-- 渐变 -->
    <gradient
        android:startColor="@android:color/white"
        android:centerColor="@android:color/white"
        android:endColor="@android:color/white"
        android:useLevel="true"
        android:angle="45"
        android:type="radial"
        android:centerX="0"
        android:centerY="0"
        android:gradientRadius="90"/>
    
   <!-- 设置四个方向上的间隔 -->
    <padding
        android:left="2dp"
        android:top="2dp"
        android:right="2dp"
        android:bottom="2dp"/>
    

    <!-- 大小>>>宽度和高度 -->
    <size
        android:width="50dp"
        android:height="50dp"/>
    
   <!-- 填充的颜色 -->
    <solid
        android:color="@android:color/white"/>
    
    <!-- 边框 
      android:width=>>  边框的宽度
      android:color=>>  边框的颜色
      android:dashWidth==> 边框的虚线的宽度
      android:dashGap ==> 边框虚线的间隔宽度
      说明: 如果  dashWidth 或者 dashGap  其中有一个为0 则边框为 实线边框
      -->
    <stroke
        android:width="1dp"  
        android:color="@android:color/black"
        android:dashWidth="0dp"
        android:dashGap="0dp"/>
    
</shape>
```

了解的 Shape 的用法以后，我们根据需要对应的设置一下，就可以得到我们的需要的效果


以下为左RadioButton (消息)的Shape.. 右 (RadioButton) 电话的 shape 类似只不过换个方向就是
```
<?xml version="1.0" encoding="utf-8"?>
<shape
    xmlns:android="http://schemas.android.com/apk/res/android"
    >
    <!--  圆角-->
    <corners 
        android:topLeftRadius="10dp"
        android:bottomLeftRadius="10dp"
        />
    
    <!-- 大小 -->
    <size
        android:width="50dp"
        android:height="20dp"/>
    
    <!--填充的颜色  -->
    <solid
        android:color="#D0EFFA"/>
    
    <!--  边  -->
    <stroke
        android:width="0dp"
        android:color="@android:color/white"
        android:dashWidth="1dp"
        android:dashGap="0dp"/> 
    
</shape>
```
RadioButton 使用 android:background="@drawable/XXXX"  shape的文件名
圆角边框已经有了。

### 第三步 ： 状态的切换，改变背景颜色和文字颜色   selector ###

背景状态颜色的改变我们知道，都是使用 selector
我们简单的使用就继续在创建一个 shape 文件 其他都一样 就修改其 填充颜色

```
 <!--填充的颜色  -->
    <solid android:color="#18B4ED"/>
```
然后在Res/drawable 目录中创建 selector 文件

```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android" >
    <!--RadioButton checked状态-->
    <item android:state_checked="true" android:drawable="@drawable/left_rounded_checked"/>
    <!--RadioButton 选中按下状态-->
    <item android:state_pressed="true" android:drawable="@drawable/left_rounded_checked"/>
    <!--RadioButton 默认状态(无选择,checked为false)-->
	<item android:drawable="@drawable/left_rounded"></item>
    
</selector>
```

selector效果也可以了....我们在 RadioButton 使用 android:background 属性使用我们的 selector xml就可以了。

####文字效果呢，也一样可以的，首先我们在 Res 目录下创建一个 color 文件夹，创建以后使用方法和 selector 一样. 只不过用的是 android:color 而已如下:####

```
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    
    <item android:state_checked="true" android:color="@color/text_selected"/>
    <item android:state_pressed="true" android:color="@color/text_selected"/>
    <item android:color="@color/text_normal" />
    
</selector>
```
而后 RadioButton android:textColor 引用其color 文件 -->android:textColor="@color/text_color"
至此我们就实现看这个类似QQ消息和电话的切换菜单。

##底部菜单栏实现###

菜单都已经实现了，底部菜单栏更简单的可以实现了，贴上XML文件一看就能明白了。

```
<RadioGroup
        android:id="@+id/radioGroup2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:orientation="horizontal"
        android:background="#ffffff"
        >

        <RadioButton
            style="@style/bottom_radiobutton"
            android:drawableTop="@drawable/fka"
            android:text="广发"
            />

        <RadioButton
            style="@style/bottom_radiobutton"
            android:drawableTop="@drawable/fkl"
            android:text="工商"
            />

        <RadioButton
            style="@style/bottom_radiobutton"
            android:drawableTop="@drawable/fkn"
            android:text="农业"
            />

        <RadioButton
            style="@style/bottom_radiobutton"
            android:drawableTop="@drawable/fkq"
            android:text="招商"
            />
    </RadioGroup>
```

我们实现一个底部4个的导航栏，其RadioButton 大多属性一样，这里我们引用一个style 可简单的复用

```
<style name="bottom_radiobutton">
        <item name="android:background">@drawable/bottom_button_select</item>
        <item name="android:textColor">@color/bottom_text_color</item>
        <item name="android:layout_width">wrap_content</item>
        <item name="android:layout_height">wrap_content</item>
        <item name="android:button">@null</item>
        <item name="android:gravity">center</item>
        <item name="android:layout_weight">1</item>
    </style>
```

效果和上面的类似，添加了图片,使用父类 TextView 的  android:drawableTop 属性 在文字的上方添加图片

仅仅XML配置 就可以完成效果

![这里写图片描述](http://img.blog.csdn.net/20150922234842103)



