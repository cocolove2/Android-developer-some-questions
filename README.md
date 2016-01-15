
# Android开发过程中遇到的问题

标签（空格分隔）： 技术问题
#目录
[TOC]

---
######1.使用Intent传递数据量大时(`尤其是bitmap对象`)，app没反应？
```
   android四大组件之间Intent传递数据，数据不能过大，基本要小于1M,不然会导致程序黑屏，ANR.
```
---
######2.`this.requestWindowFeature(Window.FEATURE_NO_TITLE);`代码中去掉标题栏使用时报错？
```
    当我们的Activity是继承自Activity或者是FragmentActivity时不会有问题;但当我们继承的是AppCompatActivity时就会报错，
解决方法是 getSupportActionBar().hide();
```
---
######3.在androidAPI16中NoScrollGridView的item布局的根布局背景色设置为白色时候，在代码中调用setTopBarBgColor()设置topBar的背景色时，item的背景会和topBar背景一样而在API16以上不会出现。
```
可以在xml直接设置，不用代码设置来避免
```
---
######4.在使用第三方库经常报有一些v4,v7包冲突问题？
```
   在封装library的时候，尽量不要引入第三方包，v4,v7等自带的包也是一样尽量不要引入，避免以后工程依赖的时候，包或者包内文件产生冲突。
   对于冲突的包，改成一样的就可以解决了。
```
---
######5.手机屏幕的高度包括哪些?
```
android手机的屏幕的高度不包括[状态栏]，包括[标题栏(actionBar)]和[显示区]；方法getWindowVisibleDisplayFrame(rect)获取的是显示区的范围
```
---
######6.static修饰的class、代码块、变量什么时候执行?
```
static 修饰的代码块，变量和方法在编译的时候就已经执行了，（在程序开始执行前）
```
---
######7.ContentProvider的onCreate()什么时候执行?
```
contentProvider的oncreate()方法在程序Application的oncreate()方法前执行。
```
---
######8.Can't load native library.CPU arch invalid for this build？
```
 CPU_ABI : armeabi, armeabi-v7a, arm64-v8a, x86, x86_64, mips, mips64.
 
 复制你的.so文件到<project>/libs/(armeabi|armeabi-v7a|x86|...)
 Android Studio复制到jniLibs文件夹下,eclipse 复制到libs文件夹下。
 
注意当你使用多个第三方库的时候时，而且这些库有使用.so文件，创建文件夹时应保证【最少原则】。比如：一个库中.so文件支持armeabi，armeabi-v7a,x86,另外一个库却只支持armeabi-v7a这样也会造成该问题的产生，因为支持x86的机器会在另一个库中找不到.so文件而保错。

```
---
######9.Parcelable接口使用时，数据传递错误原因
```
//实体类的属性的写入和读取顺序不一致，造成的。例如：
  public class ImageItem implements Parcelable {
    public String imageId;
    public String imagePath;//原图的路径
    public boolean isSelected;
    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
    //写入的顺序
        dest.writeString(imageId);
        dest.writeString(imagePath);
        dest.writeByte((byte) (isSelected ? 1 : 0));
    }

    public static final Parcelable.Creator<ImageItem> CREATOR = new Parcelable.Creator<ImageItem>() {
        @Override
        public ImageItem createFromParcel(Parcel source) {
            //读取要和写入的顺序一致，否则会造成数据读取错乱
            ImageItem item = new ImageItem();
            item.imageId = source.readString();
            item.imagePath = source.readString();
            item.isSelected = (source.readByte() != 0);
            return item;
        }

        @Override
        public ImageItem[] newArray(int size) {
            return new ImageItem[size];
        }
    };
}
```
######10，如何不让任务栈显示在最近任务栈列表里面？

*  将任务栈的rootActivity的excludeFromRecents设置为true
*  官网对**excludeFromRecents**的解释如下：
```
android:excludeFromRecents
Whether or not the task initiated by this activity should be excluded from the list of recently used applications, the overview screen. That is, when this activity is the root activity of a new task, this attribute determines whether the task should not appear in the list of recent apps. Set "true" if the task should be excluded from the list; set "false" if it should be included. The default value is "false".
```


   




