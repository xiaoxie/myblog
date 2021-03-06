title: Android NDK编程
date: 2015-12-30 13:59:08
categories: android
tags: android
---

## 官网

https://developer.android.com/ndk/index.html

## 配置

```java

ndk {
            moduleName "jlbopencv"
            ldLibs "log","android"
            abiFilters "armeabi","armeabi-v7a","x86"
            cFlags "-Im -lopencv_core -Iopencv_highgui -Iopencv_video -Iopencv_objdetect -Iopencv_imgproc -Iopencv_features2d -Iopencv_imgcodecs"
//            cFlags "-Im", "-lopencv_core", "-Iopencv_highgui", "-Iopencv_video", "-Iopencv_objdetect", "-Iopencv_imgproc", "-Iopencv_features2d", "-Iopencv_imgcodecs"
        }
```

## 记录

#### Cpp将功能封装到一个class，Java层持有cpp对象的引用

```java
public class Detector {

    static {
        System.loadLibrary("jlbopencv");
    }

    public native long nativeInit(String path);

    public native String nativeGetPhone(long self_ptr, long buf);

    public native void nativeEnd(long self_ptr);

    private long self_ptr;

    public void init(String path) {
        Log.d("Detector",path) ;
        if (0 != self_ptr) release();
        self_ptr = nativeInit(path);
    }

    public String getPhone(long buf) {
        if (self_ptr == 0) {
            return "";
        }
        Log.d("Detector","addr:"+ self_ptr) ;
        String result = nativeGetPhone(self_ptr, buf);
        nativeEnd(self_ptr);
        return result;
    }

    public void release() {
        self_ptr = 0;
    }
}
```

#### 静态库与动态库：

1、如果在android.mk文件中放置下面这一句：

	include$(BUILD_SHARED_LIBRARY)

	代表c代码最终会变成一个动态库 扩展名为.so

2、如果在android.mk文件中放置下面这一句：

	include $(BUILD_STATIC_LIBRARY)

	代表最终c代码会编译成一个静态库 扩展名为.a

3、动态库与静态库比较：

	一般来说 ,动态库的体积要比静态库的体积小很多.

	动态库: 动态的,代码的执行的时候 依赖的函数 依赖的c代码都是动态的加载执行的.

	静态库:　静态的，代码在执行之前，所有依赖的库函数　，所有依赖的代码，都必须预先加载编译到文件里面．

4、什么时候使用静态库：

	当你的公司,有一个需求:把某一个函数，某一个功能做成一个模块　供别的开发人员使用的时候

#### multiple target patterns

    删除 Obj 目录

## 教程

一、对照表
Java类型    本地类型         描述
boolean    jboolean       C/C++8位整型
byte       jbyte          C/C++带符号的8位整型
char       jchar          C/C++无符号的16位整型
short      jshort         C/C++带符号的16位整型
int        jint           C/C++带符号的32位整型
long       jlong          C/C++带符号的64位整型e
float      jfloat         C/C++32位浮点型
double     jdouble        C/C++64位浮点型
Object     jobject        任何Java对象，或者没有对应java类型的对象
Class      jclass         Class对象
String     jstring        字符串对象
Object[]   jobjectArray   任何对象的数组
boolean[]  jbooleanArray  布尔型数组
byte[]     jbyteArray     比特型数组
char[]     jcharArray     字符型数组
short[]    jshortArray    短整型数组
int[]      jintArray      整型数组
long[]     jlongArray     长整型数组
float[]    jfloatArray    浮点型数组
double[]   jdoubleArray   双浮点型数组
 
 二、JNI函数大全  
来自：http://blog.chinaunix.net/uid-22028680-id-3429721.html
 
1、AndroidJNI.AllocObject 分配对象


static function AllocObject (clazz : IntPtr) : IntPtr
Description描述
Allocates a new Java object without invoking any of the constructors for the object.
分配新 Java 对象而不调用该对象的任何构造函数。返回该对象的引用。
clazz 参数务必不要引用数组类。


2、AndroidJNI.AttachCurrentThread 附加当前线程


static function AttachCurrentThread () : int
Description描述
Attaches the current thread to a Java (Dalvik) VM.
附加当前线程到一个Java(Dalvik)虚拟机。
A thread must be attached to the VM before any other JNI calls can be made.
一个线程必须附加到虚拟机，在任何其他JNI可调用之前。
Returns 0 on success; returns a negative number on failure.
成功返回0，失败返回一个负数。


3、AndroidJNI.CallBooleanMethod 调用布尔方法


static function CallBooleanMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : bool
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。


4、AndroidJNI.CallByteMethod 调用字节方法
static function CallByteMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : Byte
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。


5、AndroidJNI.CallCharMethod 调用字符方法


static function CallCharMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : Char
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。


6、AndroidJNI.CallDoubleMethod 调用双精度浮点数方法


static function CallDoubleMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : double
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。


7、AndroidJNI.CallFloatMethod 调用浮点数方法


static function CallFloatMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : float
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。


8、AndroidJNI.CallIntMethod 调用整数方法


static function CallObjectMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : IntPtr
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。


9、AndroidJNI.CallLongMethod 调用长整数方法


static function CallLongMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : Int64
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。


10、AndroidJNI.CallObjectMethod 调用对象方法


static function CallObjectMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : IntPtr
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。
This method returns a reference to a java.lang.Object, or a subclass thereof.
这个方法返回一个引用到java.lang.Object，或者其子类。


11、AndroidJNI.CallShortMethod 调用短整数方法


static function CallShortMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : Int16
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。


12、AndroidJNI.CallStaticBooleanMethod 调用静态布尔方法


static function CallStaticBooleanMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : bool
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。


13、AndroidJNI.CallStaticByteMethod 调用静态字节方法


static function CallStaticByteMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : Byte
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。


14、AndroidJNI.CallStaticCharMethod 调用静态字符方法


static function CallStaticCharMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : Char
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。


15、AndroidJNI.CallStaticDoubleMethod 调用静态双精度浮点数方法


static function CallStaticDoubleMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : double
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。


16、AndroidJNI.CallStaticFloatMethod 调用静态浮点数方法


static function CallStaticFloatMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : float
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。


17、AndroidJNI.CallStaticIntMethod 调用静态整数方法


static function CallStaticIntMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : Int32
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。


18、AndroidJNI.CallStaticLongMethod 调用静态长整数方法


static function CallStaticLongMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : Int64
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。


19、AndroidJNI.CallStaticObjectMethod 调用静态对象方法


static function CallStaticObjectMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : IntPtr
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。
This method returns a reference to a java.lang.Object, or a subclass thereof.
这个方法返回一个java.lang.Object的引用，或者其子类。


20、AndroidJNI.CallStaticShortMethod 调用静态短整数方法


static function CallStaticShortMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : Int16
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。


21、AndroidJNI.CallStaticStringMethod 调用静态字符串方法


static function CallStaticStringMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : string
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。
This is a convenience function that calls CallStaticObjectMethod() with the same parameters, but creates a managed string from the result.
这是一个方便的函数，调用带有相同参数的CallStaticObjectMethod()，但从该结果创建一个托管字符串。


22、AndroidJNI.CallStaticVoidMethod 调用静态无类型方法


static function CallStaticVoidMethod (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : void
Description描述
Invokes a static method on a Java object, according to the specified methodID, optionally passing an array of arguments (args) to the method.
在一个Java对象调用一个静态方法，根据指定的methodID，可选传递参数（args）的数组到该方法。


23、AndroidJNI.CallStringMethod 调用字符串方法


static function CallStringMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : string
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。
This is a convenience function that calls CallObjectMethod() with the same parameters, but creates a managed string from the result.
这是一个方便的函数，调用带有相同参数的CallObjectMethod()，但从整个结果创建一个托管字符串。


24、AndroidJNI.CallVoidMethod 调用无类型方法


static function CallVoidMethod (obj : IntPtr, methodID : IntPtr, args : jvalue[]) : void
Description描述
Calls an instance (nonstatic) Java method defined by methodID, optionally passing an array of arguments (args) to the method.
调用一个由methodID定义的实例的Java方法，可选择传递参数（args）的数组到这个方法。


25、AndroidJNI.DeleteGlobalRef 删除全局引用


static function DeleteGlobalRef (obj : IntPtr) : void
Description描述
Deletes the global reference pointed to by obj.
删除 obj 所指向的全局引用。


26、AndroidJNI.DeleteLocalRef 删除局部引用


static function DeleteLocalRef (obj : IntPtr) : void
Description描述
Deletes the local reference pointed to by obj.
删除 obj 所指向的局部引用。


27、AndroidJNI.DetachCurrentThread 分离当前线程


static function DetachCurrentThread () : int
Description描述
Detaches the current thread from a Java (Dalvik) VM.
从一个Java（Dalvik）虚拟机，分类当前线程。
A thread must be detached from the VM before exiting.
从退出虚拟机之前，一个线程必须被分类。


28、AndroidJNI.EnsureLocalCapacity 确保局部性能


static function EnsureLocalCapacity (capacity : int) : int
Description描述
Ensures that at least a given number of local references can be created in the current thread.
在当前线程确保至少一个可以被创建的给定局部引用数。


29、AndroidJNI.ExceptionClear 清除异常


static function ExceptionClear () : void
Description描述
Clears any exception that is currently being thrown.
清除当前抛出的任何异常。如果当前无异常，则此例程不产生任何效果。


30、AndroidJNI.ExceptionDescribe 描述异常


static function ExceptionDescribe () : void
Description描述
Prints an exception and a backtrace of the stack to the logcat
将异常及堆栈的回溯输出到系统错误报告信道。该例程可便利调试操作。


31、AndroidJNI.ExceptionOccurred 发生异常


static function ExceptionOccurred () : IntPtr
Description描述
Determines if an exception is being thrown
确定是否某个异常正被抛出。


32、AndroidJNI.FatalError 致命错误


static function FatalError (message : string) : void
Description描述
Raises a fatal error and does not expect the VM to recover. This function does not return.
抛出致命错误并且不希望虚拟机进行修复。该函数无返回值。


33、AndroidJNI.FindClass 查找类


static function FindClass (name : string) : IntPtr
Description描述
This function loads a locally-defined class.
这个函数加载一个本地定义的类。


34、AndroidJNI.FromBooleanArray 从布尔数组


static function FromBooleanArray (array : IntPtr) : Boolean[]
Description描述
Convert a Java array of boolean to a managed array of System.Boolean.
转换一个Java布尔数组到一个托管System.Boolean数组。


35、AndroidJNI.FromByteArray 从字节数组


static function FromByteArray (array : IntPtr) : Byte[]
Description描述
Convert a Java array of byte to a managed array of System.Byte.
转换一个Java字节数组到一个托管的System.Byte数组。


36、AndroidJNI.FromCharArray 从字符数组


static function FromCharArray (array : IntPtr) : Char[]
Description描述
Convert a Java array of char to a managed array of System.Char.
转换一个Java字符数组到一个托管的System.Char数组。


37、AndroidJNI.FromDoubleArray 从双精度浮点数数组


static function FromDoubleArray (array : IntPtr) : double[]
Description描述
Convert a Java array of double to a managed array of System.Double.
转换一个Java双精度浮点数数组到一个托管的System.Double数组。


38、AndroidJNI.FromFloatArray 从浮点数数组


static function FromFloatArray (array : IntPtr) : float[]
Description描述
Convert a Java array of float to a managed array of System.Single.
转换一个Java浮点数数组到一个托管的System.Single数组。


39、AndroidJNI.FromIntArray 从整数数组


static function FromIntArray (array : IntPtr) : Int32[]
Description描述
Convert a Java array of int to a managed array of System.Int32.
转换一个Java整数数组到一个托管的System.Int32数组。


40、AndroidJNI.FromLongArray 从整长数数组


static function FromLongArray (array : IntPtr) : Int64[]
Description描述
Convert a Java array of long to a managed array of System.Int64.
转换一个Java长整数数组到一个托管的System.Int64数组。


41、AndroidJNI.FromObjectArray 从对象数组


static function FromObjectArray (array : IntPtr) : IntPtr[]
Description描述
Convert a Java array of java.lang.Object to a managed array of System.IntPtr, representing Java objects
转换一个Java的java.lang.Object数组到一个托管的System.IntPtr数组，表示Java对象。


42、AndroidJNI.FromReflectedField 来自反射的域


static function FromReflectedField (refField : IntPtr) : IntPtr
Description描述
Converts a java.lang.reflect.Field to a field ID.
转换一个java.lang.reflect.Field到一个域ID。


43、AndroidJNI.FromReflectedMethod 来自反射的方法


static function FromReflectedMethod (refMethod : IntPtr) : IntPtr
Description描述
Converts a java.lang.reflect.Method or java.lang.reflect.Constructor object to a method ID.
转换一个java.lang.reflect.Method或java.lang.reflect.Constructor对象到一个方法ID。


44、AndroidJNI.FromShortArray 从短整数数组


static function FromShortArray (array : IntPtr) : Int16[]
Description描述
Convert a Java array of short to a managed array of System.Int16.
转换一个Java短整数数组到一个托管的System.Int16数组。


45、AndroidJNI.GetArrayLength 获取数组长度


static function GetArrayLength (array : IntPtr) : int
Description描述
Returns the number of elements in the array.
返回数组的元素数。


46、AndroidJNI.GetBooleanArrayElement 获取布尔数组元素


static function GetBooleanArrayElement (array : IntPtr, index : int) : bool
Description描述
Returns the value of one element of a primitive array.
返回一个基本数组一个元素的值。
This function is a special case of GetBooleanArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的GetBooleanArrayRegion()，就是region大小设置为1时。


47、AndroidJNI.GetBooleanField 获取布尔域


static function GetBooleanField (obj : IntPtr, fieldID : IntPtr) : bool
Description描述
This function returns the value of an instance (nonstatic) field of an object.
这个函数返回一个对象实例（非静态）域的值。


48、AndroidJNI.GetByteArrayElement 获取字节数组元素


static function GetByteArrayElement (array : IntPtr, index : int) : Byte
Description描述
Returns the value of one element of a primitive array.
返回一个基本数组一个元素的值。
This function is a special case of GetByteArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的GetByteArrayRegion()，就是region大小设置为1时。


49、AndroidJNI.GetByteField 获取字节域


static function GetByteField (obj : IntPtr, fieldID : IntPtr) : Byte
Description描述
This function returns the value of an instance (nonstatic) field of an object.
这个函数返回一个对象实例（非静态）域的值。


50、AndroidJNI.GetCharArrayElement 获取字符数组元素


static function GetCharArrayElement (array : IntPtr, index : int) : Char
Description描述
Returns the value of one element of a primitive array.
返回一个基本数组一个元素的值。
This function is a special case of GetCharArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的GetCharArrayRegion()，就是region大小设置为1时。


51、AndroidJNI.GetCharField 获取字符域


static function GetCharField (obj : IntPtr, fieldID : IntPtr) : Char
Description描述
This function returns the value of an instance (nonstatic) field of an object.
这个函数返回一个对象实例（非静态）域的值。


52、AndroidJNI.GetDoubleArrayElement 获取双精度浮点数数组元素


static function GetDoubleArrayElement (array : IntPtr, index : int) : double
Description描述
Returns the value of one element of a primitive array.
返回一个基本数组一个元素的值。
This function is a special case of GetDoubleArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的GetDoubleArrayRegion()，就是region大小设置为1时。


53、AndroidJNI.GetDoubleField 获取双精度浮点数域


static function GetDoubleField (obj : IntPtr, fieldID : IntPtr) : double
Description描述
This function returns the value of an instance (nonstatic) field of an object.
这个函数返回一个对象实例（非静态）域的值。


54、AndroidJNI.GetFieldID 获取域ID


static function GetFieldID (clazz : IntPtr, name : string, sig : string) : IntPtr
Description描述
Returns the field ID for an instance (nonstatic) field of a class.
返回类的实例（非静态）域的域 ID。该域由其名称及签名指定。
GetFieldID() 将未初始化的类初始化。


55、AndroidJNI.GetFloatArrayElement 获取浮点数数组元素


static function GetFloatArrayElement (array : IntPtr, index : int) : float
Description描述
Returns the value of one element of a primitive array.
返回一个基本数组一个元素的值。
This function is a special case of GetFloatArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的GetFloatArrayRegion()，就是region大小设置为1时。


56、AndroidJNI.GetFloatField 获取浮点数域


static function GetFloatField (obj : IntPtr, fieldID : IntPtr) : float
Description描述
This function returns the value of an instance (nonstatic) field of an object.
这个函数返回一个对象实例（非静态）域的值。


57、AndroidJNI.GetIntArrayElement 获取整数数组元素


static function GetIntArrayElement (array : IntPtr, index : int) : Int32
Description描述
Returns the value of one element of a primitive array.
返回一个基本数组一个元素的值。
This function is a special case of GetIntArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的GetIntArrayRegion()，就是region大小设置为1时。


58、AndroidJNI.GetIntField 获取整数域


static function GetIntField (obj : IntPtr, fieldID : IntPtr) : Int32
Description描述
This function returns the value of an instance (nonstatic) field of an object.
这个函数返回一个对象实例（非静态）域的值。


59、AndroidJNI.GetLongArrayElement 获取长整数数组元素


static function GetLongArrayElement (array : IntPtr, index : int) : Int64
Description描述
Returns the value of one element of a primitive array.
返回一个基本数组一个元素的值。
This function is a special case of GetLongArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的GetLongArrayRegion()，就是region大小设置为1时。


60、AndroidJNI.GetLongField 获取长整数域


static function GetLongField (obj : IntPtr, fieldID : IntPtr) : Int64
Description描述
This function returns the value of an instance (nonstatic) field of an object.
这个函数返回一个对象实例（非静态）域的值。


61、AndroidJNI.GetMethodID 获取方法ID


static function GetMethodID (clazz : IntPtr, name : string, sig : string) : IntPtr
Description描述
Returns the method ID for an instance (nonstatic) method of a class or interface.
返回类或接口实例（非静态）方法的方法 ID。方法可在某个 clazz 的超类中定义，也可从 clazz 继承。该方法由其名称和签名决定。
GetMethodID() 可使未初始化的类初始化。


62、AndroidJNI.GetObjectArrayElement 获取对象数组元素


static function GetObjectArrayElement (array : IntPtr, index : int) : IntPtr
Description描述
Returns an element of an Object array.
返回一个对象数组的一个元素。


63、AndroidJNI.GetObjectClass 获取对象类


static function GetObjectClass (obj : IntPtr) : IntPtr
Description描述
Returns the class of an object.
返回一个对象的类。


64、AndroidJNI.GetObjectField 获取对象域


static function GetObjectField (obj : IntPtr, fieldID : IntPtr) : IntPtr
Description描述
This function returns the value of an instance (nonstatic) field of an object.
这个函数返回一个对象实例（非静态）域的值。
The result is a reference to a java.lang.Object, or a subclass thereof.
其结果是对一个java.lang.Object的引用，或者其子类。


65、AndroidJNI.GetShortArrayElement 获取短整数数组元素


static function GetShortArrayElement (array : IntPtr, index : int) : Int16
Description描述
Returns the value of one element of a primitive array.
返回一个基本数组一个元素的值。
This function is a special case of GetShortArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的GetShortArrayRegion()，就是region大小设置为1时。


66、AndroidJNI.GetShortField 获取短整数域


static function GetShortField (obj : IntPtr, fieldID : IntPtr) : Int16
Description描述
This function returns the value of an instance (nonstatic) field of an object.
这个函数返回一个对象实例（非静态）域的值。


67、AndroidJNI.GetStaticBooleanField 获取静态布尔域


static function GetStaticBooleanField (clazz : IntPtr, fieldID : IntPtr) : bool
Description描述
This function returns the value of a static field of an object.
这个函数返回一个对象静态域的值。


68、AndroidJNI.GetStaticByteField 获取静态字节域


static function GetStaticByteField (clazz : IntPtr, fieldID : IntPtr) : Byte
Description描述
This function returns the value of a static field of an object.
这个函数返回一个对象静态域的值。


69、AndroidJNI.GetStaticCharField 获取静态字符域


static function GetStaticCharField (clazz : IntPtr, fieldID : IntPtr) : Char
Description描述
This function returns the value of a static field of an object.
这个函数返回一个对象静态域的值。


70、AndroidJNI.GetStaticDoubleField 获取静态双精度浮点数域


static function GetStaticDoubleField (clazz : IntPtr, fieldID : IntPtr) : double
Description描述
This function returns the value of a static field of an object.
这个函数返回一个对象静态域的值。


71、AndroidJNI.GetStaticFieldID 获取静态域ID


static function GetStaticFieldID (clazz : IntPtr, name : string, sig : string) : IntPtr
Description描述
Returns the field ID for a static field of a class.
返回类的静态域的域 ID。域由其名称和签名指定。
GetStaticFieldID() 将未初始化的类初始化。


72、AndroidJNI.GetStaticFloatField 获取静态浮点数域


static function GetStaticFloatField (clazz : IntPtr, fieldID : IntPtr) : float
Description描述
This function returns the value of a static field of an object.
这个函数返回一个对象静态域的值。


73、AndroidJNI.GetStaticIntField 获取静态整数域


static function GetStaticIntField (clazz : IntPtr, fieldID : IntPtr) : Int64
Description描述
This function returns the value of a static field of an object.
这个函数返回一个对象静态域的值。


74、AndroidJNI.GetStaticLongField 获取静态长整数域


static function GetStaticLongField (clazz : IntPtr, fieldID : IntPtr) : Int64
Description描述
This function returns the value of a static field of an object.
这个函数返回一个对象静态域的值。


75、AndroidJNI.GetStaticMethodID 获取静态方法ID


static function GetStaticMethodID (clazz : IntPtr, name : string, sig : string) : IntPtr
Description描述
Returns the method ID for a static method of a class.
返回类的静态方法的方法 ID。方法由其名称和签名指定。
GetStaticMethodID() 将未初始化的类初始化。


76、AndroidJNI.GetStaticObjectField 获取静态对象域


static function GetStaticObjectField (clazz : IntPtr, fieldID : IntPtr) : IntPtr
Description描述
This function returns the value of a static field of an object.
这个函数返回一个对象静态域的值。
The result is a reference to a java.lang.Object, or a subclass thereof.
该结果是一个java.lang.Object的引用，或者其子类。


77、AndroidJNI.GetStaticShortField 获取静态短整数域


static function GetStaticShortField (clazz : IntPtr, fieldID : IntPtr) : Int16
Description描述
This function returns the value of a static field of an object.
这个函数返回一个对象静态域的值。


78、AndroidJNI.GetStaticStringField 获取静态字符串域


static function GetStaticStringField (clazz : IntPtr, fieldID : IntPtr) : string
Description描述
This function returns the value of a static field of an object.
这个函数返回一个对象静态域的值。
This is a convenience function that calls GetStaticObjectField() with the same parameters, but creates a managed string from the result.
这是一个方便的函数，调用带有相同参数的GetStaticObjectField()，但从该结果创建一个托管字符串。


79、AndroidJNI.GetStringField 获取字符串域


static function GetStringField (obj : IntPtr, fieldID : IntPtr) : string
Description描述
This function returns the value of an instance (nonstatic) field of an object.
这个函数返回一个对象实例（非静态）域的值。
This is a convenience function that calls GetObjectField() with the same parameters, but creates a managed string from the result.
这是一个方便的函数，调用带有相同参数的GetObjectField()，但从结果创建一个托管字符串。


80、AndroidJNI.GetStringUTFChars 获取字符串UTF的字符


static function GetStringUTFChars (str : IntPtr) : string
Description描述
Returns a managed string object representing the string in modified UTF-8 encoding.
返回由UTF-8修改的托管的字符串对象。
另解，返回指向字符串的 UTF-8 字符数组的指针。
This method is a modification of the original GetStringUTFChars, which returns a pointer to an array of bytes.
这个方法是原始GetStringUTFChars的修改，返回指向字节的数组。


81、AndroidJNI.GetStringUTFLength 获取字符串UTF的长度


static function GetStringUTFLength (str : IntPtr) : int
Description描述
Returns the length in bytes of the modified UTF-8 representation of a string.
以字节为单位返回字符串的 UTF-8 长度。


82、AndroidJNI.GetSuperclass 获取超类


static function GetSuperclass (clazz : IntPtr) : IntPtr
Description描述
If clazz represents any class other than the class Object, then this function returns the object that represents the superclass of the class specified by clazz.
如果 clazz 代表类而非类 object，则该函数返回由 clazz 所指定的类的超类。
If clazz specifies the class Object, or clazz represents an interface, this function returns NULL.
如果 clazz 指定类 Object，或代表某个接口，则该函数返回 NULL。


83、AndroidJNI.GetVersion 获取版本


static function GetVersion () : int
Description描述
Returns the version of the native method interface.
返回本地方法接口的版本。


84、AndroidJNI.IsAssignableFrom 是否可赋值


static function IsAssignableFrom (clazz1 : IntPtr, clazz2 : IntPtr) : bool
Description描述
Determines whether an object of clazz1 can be safely cast to clazz2.
确定 clazz1 的对象是否可安全地强制转换为 clazz2。


85、AndroidJNI.IsInstanceOf 是否某类的实例


static function IsInstanceOf (obj : IntPtr, clazz : IntPtr) : bool
Description描述
Tests whether an object is an instance of a class.
测试对象是否为某个类的实例。


86、AndroidJNI.IsSameObject 是否同一对象


static function IsSameObject (obj1 : IntPtr, obj2 : IntPtr) : bool
Description描述
Tests whether two references refer to the same Java object.
测试两个引用是否引用同一 Java 对象。


87、AndroidJNI.NewBooleanArray 新建布尔数组


static function NewBooleanArray (size : int) : IntPtr
Description描述
Construct a new primitive array object.
构造一个新的基本数组对象。


88、AndroidJNI.NewByteArray 新建字节数组


static function NewByteArray (size : int) : IntPtr
Description描述
Construct a new primitive array object.
构造一个新的基本数组对象。


89、AndroidJNI.NewCharArray 新建字符数组


static function NewCharArray (size : int) : IntPtr
Description描述
Construct a new primitive array object.
构造一个新的基本数组对象。


90、AndroidJNI.NewDoubleArray 新建双精度浮点数数组


static function NewDoubleArray (size : int) : IntPtr
Description描述
Construct a new primitive array object.
构造一个新的基本数组对象。


91、AndroidJNI.NewFloatArray 新建浮点数数组


static function NewFloatArray (size : int) : IntPtr
Description描述
Construct a new primitive array object.
构造一个新的基本数组对象。


92、AndroidJNI.NewGlobalRef 新建全局引用


static function NewGlobalRef (obj : IntPtr) : IntPtr
Description描述
Creates a new global reference to the object referred to by the obj argument.
创建 obj 参数所引用对象的新全局引用。全局引用通过调用 DeleteGlobalRef() 来显式撤消。
返回全局引用。如果系统内存不足则返回 NULL。


93、AndroidJNI.NewIntArray 新建整数数组


static function NewIntArray (size : int) : IntPtr
Description描述
Construct a new primitive array object.
构造一个新的基本数组对象。


94、AndroidJNI.NewLocalRef 新建局部引用


static function NewLocalRef (obj : IntPtr) : IntPtr
Description描述
Creates a new local reference that refers to the same object as obj.
创建 obj 参数对象的新局部引用。


95、AndroidJNI.NewLongArray 新建长整数数组


static function NewLongArray (size : int) : IntPtr
Description描述
Construct a new primitive array object.
构造一个新的基本数组对象。


96、AndroidJNI.NewObjectArray 新建对象数组
static function NewObjectArray (size : int, clazz : IntPtr, obj : IntPtr) : IntPtr
Description描述
Constructs a new array holding objects in class clazz. All elements are initially set to obj.
构造新的数组，它将保存在类 clazz 中的对象。所有元素初始值均设为 obj。


97、AndroidJNI.NewObject 新建对象
static function NewObject (clazz : IntPtr, methodID : IntPtr, args : jvalue[]) : IntPtr
Description描述
Constructs a new Java object. The method ID indicates which constructor method to invoke. This ID must be obtained by calling GetMethodID() with as the method name and void (V) as the return type.
构造新的 Java 对象。方法 ID指示应调用的构造函数方法。该 ID 必须通过调用 GetMethodID() 获得，且调用时的方法名必须为 ，而返回类型必须为 void (V)。
clazz 参数务必不要引用数组类。


98、AndroidJNI.NewShortArray 新建短整数数组
static function NewShortArray (size : int) : IntPtr
Description描述
Construct a new primitive array object.
构造一个新的基本数组对象。


99、AndroidJNI.NewStringUTF 新建UTF字符串
static function NewStringUTF (bytes : string) : IntPtr
Description描述
Constructs a new java.lang.String object from an array of characters in modified UTF-8 encoding.
利用 UTF-8 字符数组构造新的 java.lang.String 对象。


100、AndroidJNI.PopLocalFrame 弹出局部帧
static function PopLocalFrame (result : IntPtr) : IntPtr
Description描述
Pops off the current local reference frame, frees all the local references, and returns a local reference in the previous local reference frame for the given result object.
弹出关闭当前局部引用帧，释放所有的本地引用，并返回一个局部引用，在前一个局部引用帧，用于给定的结果对象。
    PushLocalFrame为一定数量的局部引用创建了一个使用堆栈，而PopLocalFrame负责销毁堆栈顶端的引用。
    Push/PopLocalFrame函数对提供了对局部引用的生命周期更方便的管理。
    在管理局部引用的生命周期中，Push/PopLocalFrame是非常方便的。你可以在本地函数的入口处调用PushLocalFrame，然后在出 口处调用PopLocalFrame，这样的话，在函数对中间任何位置创建的局部引用都会被释放。而且，这两个函数是非常高效的。
    如果你在函数的入口处调用了PushLocalFrame，记住在所有的出口（有return出现的地方）调用PopLocalFrame。
    大量的局部引用创建会浪费不必要的内存。一个局部引用会导致它本身和它所指向的对象都得不到回收。尤其要注意那些长时间运行的方法、创建局部引用的循环和工具函数，充分得利用Pus/PopLocalFrame来高效地管理局部引用。


101、AndroidJNI.PushLocalFrame 压入局部帧
static function PushLocalFrame (capacity : int) : int
Description描述
Creates a new local reference frame, in which at least a given number of local references can be created.
创建一个新的局部引入帧，至少一个给定的局部引用可以被创建的数。
    PushLocalFrame为一定数量的局部引用创建了一个使用堆栈，而PopLocalFrame负责销毁堆栈顶端的引用。
    Push/PopLocalFrame函数对提供了对局部引用的生命周期更方便的管理。
    在管理局部引用的生命周期中，Push/PopLocalFrame是非常方便的。你可以在本地函数的入口处调用PushLocalFrame，然后在出 口处调用PopLocalFrame，这样的话，在函数对中间任何位置创建的局部引用都会被释放。而且，这两个函数是非常高效的。
    如果你在函数的入口处调用了PushLocalFrame，记住在所有的出口（有return出现的地方）调用PopLocalFrame。
    大量的局部引用创建会浪费不必要的内存。一个局部引用会导致它本身和它所指向的对象都得不到回收。尤其要注意那些长时间运行的方法、创建局部引用的循环和工具函数，充分得利用Pus/PopLocalFrame来高效地管理局部引用。


102、AndroidJNI.SetBooleanArrayElement 设置布尔数组数组元素
static function SetBooleanArrayElement (array : IntPtr, index : int, val : byte) : void
Description描述
Sets the value of one element in a primitive array.
设置一个基本数组一个元素的值。
This function is a special case of SetBooleanArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的SetBooleanArrayRegion()，就是region大小设置为1时。


103、AndroidJNI.SetBooleanField 设置布尔域
static function SetBooleanField (obj : IntPtr, fieldID : IntPtr, val : bool) : void
Description描述
This function sets the value of an instance (nonstatic) field of an object.
这个函数设置一个对象实例（非静态）域的值。


104、AndroidJNI.SetByteArrayElement 设置字节数组元素
static function SetByteArrayElement (array : IntPtr, index : int, val : sbyte) : void
Description描述
Sets the value of one element in a primitive array.
设置一个基本数组一个元素的值。
This function is a special case of SetByteArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的SetByteArrayRegion()，就是region大小设置为1时。


106、AndroidJNI.SetByteField 设置字节域
static function SetByteField (obj : IntPtr, fieldID : IntPtr, val : Byte) : void
Description描述
This function sets the value of an instance (nonstatic) field of an object.
这个函数设置一个对象实例（非静态）域的值。


107、AndroidJNI.SetCharArrayElement 设置字符数组元素
static function SetCharArrayElement (array : IntPtr, index : int, val : Char) : void
Description描述
Sets the value of one element in a primitive array.
设置一个基本数组一个元素的值。
This function is a special case of SetCharArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的SetCharArrayRegion()，就是region大小设置为1时。


108、AndroidJNI.SetCharField 设置字符域
static function SetCharField (obj : IntPtr, fieldID : IntPtr, val : Char) : void
Description描述
This function sets the value of an instance (nonstatic) field of an object.
这个函数设置一个对象实例（非静态）域的值。


109、AndroidJNI.SetDoubleArrayElement 设置双精度浮点数数组元素
static function SetDoubleArrayElement (array : IntPtr, index : int, val : double) : void
Description描述
Sets the value of one element in a primitive array.
设置一个基本数组一个元素的值。
This function is a special case of SetDoubleArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的SetDoubleArrayRegion()，就是region大小设置为1时。


110、AndroidJNI.SetDoubleField 设置双精度浮点数域
static function SetDoubleField (obj : IntPtr, fieldID : IntPtr, val : double) : void
Description描述
This function sets the value of an instance (nonstatic) field of an object.
这个函数设置一个对象实例（非静态）域的值。


111、AndroidJNI.SetFloatArrayElement 设置浮点数数组元素
static function SetFloatArrayElement (array : IntPtr, index : int, val : float) : void
Description描述
Sets the value of one element in a primitive array.
设置一个基本数组一个元素的值。
This function is a special case of SetFloatArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的SetFloatArrayRegion()，就是region大小设置为1时。


112、AndroidJNI.SetFloatField 设置浮点数域
static function SetFloatField (obj : IntPtr, fieldID : IntPtr, val : float) : void
Description描述
This function sets the value of an instance (nonstatic) field of an object.
这个函数设置一个对象实例（非静态）域的值。


113、AndroidJNI.SetIntArrayElement 设置整数数组元素
static function SetIntArrayElement (array : IntPtr, index : int, val : Int32) : void
Description描述
Sets the value of one element in a primitive array.
设置一个基本数组一个元素的值。
This function is a special case of SetIntArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的SetIntArrayRegion()，就是region大小设置为1时。


114、AndroidJNI.SetIntField 设置整数域
static function SetIntField (obj : IntPtr, fieldID : IntPtr, val : Int32) : void
Description描述
This function sets the value of an instance (nonstatic) field of an object.
这个函数设置一个对象实例（非静态）域的值。


115、AndroidJNI.SetLongArrayElement 设置长整数数组元素
static function SetLongArrayElement (array : IntPtr, index : int, val : Int64) : void
Description描述
Sets the value of one element in a primitive array.
设置一个基本数组一个元素的值。
This function is a special case of SetLongArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的SetLongArrayRegion()，就是region大小设置为1时。


116、AndroidJNI.SetLongField 设置长整数域
static function SetLongField (obj : IntPtr, fieldID : IntPtr, val : Int64) : void
Description描述
This function sets the value of an instance (nonstatic) field of an object.
这个函数设置一个对象实例（非静态）域的值。


117、AndroidJNI.SetObjectArrayElement 设置对象数组元素
static function SetObjectArrayElement (array : IntPtr, index : int, obj : IntPtr) : void
Description描述
Sets an element of an Object array.
设置一个对象数组的一个元素。


118、AndroidJNI.SetObjectField 设置对象域
static function SetObjectField (obj : IntPtr, fieldID : IntPtr, val : IntPtr) : void
Description描述
This function sets the value of an instance (nonstatic) field of an object.
这个函数设置一个对象实例（非静态）域的值。
The value to set is a reference to either a java.lang.Object, or a subclass thereof.
要设置的值无论是一个java.lang.Object的引用，或者其子类。


119、AndroidJNI.SetShortArrayElement 设置短整数数组元素
static function SetShortArrayElement (array : IntPtr, index : int, val : Int16) : void
Description描述
Sets the value of one element in a primitive array.
设置一个基本数组一个元素的值。
This function is a special case of SetShortArrayRegion(), called with region size set to 1.
这个函数是一个特殊情况的SetShortArrayRegion()，就是region大小设置为1时。


120、AndroidJNI.SetShortField 设置短整数域
static function SetShortField (obj : IntPtr, fieldID : IntPtr, val : Int16) : void
Description描述
This function sets the value of an instance (nonstatic) field of an object.
这个函数设置一个对象实例（非静态）域的值。


121、AndroidJNI.SetStaticBooleanField 设置静态布尔域
static function SetStaticBooleanField (clazz : IntPtr, fieldID : IntPtr, val : bool) : void
Description描述
This function ets the value of a static field of an object.
这个函数设置一个对象的静态域的值。


122、AndroidJNI.SetStaticByteField 设置静态字节域
static function SetStaticByteField (clazz : IntPtr, fieldID : IntPtr, val : Byte) : void
Description描述
This function ets the value of a static field of an object.
这个函数设置一个对象的静态域的值。


123、AndroidJNI.SetStaticCharField 设置静态字符域
static function SetStaticCharField (clazz : IntPtr, fieldID : IntPtr, val : Char) : void
Description描述
This function ets the value of a static field of an object.
这个函数设置一个对象的静态域的值。


124、AndroidJNI.SetStaticDoubleField 设置静态双精度浮点数域
static function SetStaticDoubleField (clazz : IntPtr, fieldID : IntPtr, val : double) : void
Description描述
This function ets the value of a static field of an object.
这个函数设置一个对象的静态域的值。


125、AndroidJNI.SetStaticFloatField 设置静态浮点数域
static function SetStaticFloatField (clazz : IntPtr, fieldID : IntPtr, val : float) : void
Description描述
This function ets the value of a static field of an object.
这个函数设置一个对象的静态域的值。


126、AndroidJNI.SetStaticIntField 设置静态整数域
static function SetStaticIntField (clazz : IntPtr, fieldID : IntPtr, val : Int32) : void
Description描述
This function ets the value of a static field of an object.
这个函数设置一个对象的静态域的值。


127、AndroidJNI.SetStaticLongField 设置静态长整数域
static function SetStaticLongField (clazz : IntPtr, fieldID : IntPtr, val : Int64) : void
Description描述
This function ets the value of a static field of an object.
这个函数设置一个对象的静态域的值。


128、AndroidJNI.SetStaticObjectField 设置静态对象域
static function SetStaticObjectField (clazz : IntPtr, fieldID : IntPtr, val : IntPtr) : void
Description描述
This function ets the value of a static field of an object.
这个函数设置一个对象的静态域的值。
The value to set is a reference to either a java.lang.Object, or a subclass thereof.
设置该值不管是一个java.lang.Object的引用，或是其子类。


129、AndroidJNI.SetStaticShortField 设置静态短整数域
static function SetStaticShortField (clazz : IntPtr, fieldID : IntPtr, val : Int16) : void
Description描述
This function ets the value of a static field of an object.
这个函数设置一个对象的静态域的值。


130、AndroidJNI.SetStaticStringField 设置静态字符串域
static function SetStaticStringField (clazz : IntPtr, fieldID : IntPtr, val : string) : void
Description描述
This function ets the value of a static field of an object.
这个函数设置一个对象的静态域的值。
This is a convenience function that calls SetStaticObjectField() with the same parameters, but performs the necessary marshalling of the string value.
这是一个方便的函数，调用带有相同参数的SetStaticObjectField()，但执行需要字符串的值编组。


131、AndroidJNI.SetStringField 设置字符串域
static function SetStringField (obj : IntPtr, fieldID : IntPtr, val : string) : void
Description描述
This function sets the value of an instance (nonstatic) field of an object.
这个函数设置一个对象实例（非静态）域的值。
This is a convenience function that calls SetObjectField() with the same parameters, but performs the necessary marshalling of the string value.
这是一个方便的函数，调用带有相同参数的SetObjectField()，但执行字符串的值需要编组。


132、AndroidJNI.ThrowNew 新的抛出
static function ThrowNew (clazz : IntPtr, message : string) : int
Description描述
Constructs an exception object from the specified class with the message specified by message and causes that exception to be thrown.
利用指定类的消息（由 message 指定）构造异常对象并抛出该异常。


133、AndroidJNI.Throw 抛出
static function Throw (obj : IntPtr) : int
Description描述
Causes a java.lang.Throwable object to be thrown.
导致一个java.lang.Throwable的对象被抛出。


134、AndroidJNI.ToBooleanArray 到布尔数组
static function ToBooleanArray (array : Boolean[]) : IntPtr
Description描述
Convert a managed array of System.Boolean to a Java array of boolean.
转换一个System.Boolean的托管数组到一个Java布尔数组。


135、AndroidJNI.ToByteArray 到字节数组
static function ToByteArray (array : Byte[]) : IntPtr
Description描述
Convert a managed array of System.Byte to a Java array of byte.
转换一个System.Byte的托管数组到一个Java的字节数组。


136、AndroidJNI.ToCharArray 到字符数组
static function ToCharArray (array : Char[]) : IntPtr
Description描述
Convert a managed array of System.Char to a Java array of char.
转换一个System.Char的托管数组到一个Java的字符数组。


137、AndroidJNI.ToDoubleArray 到双精度浮点数数组
static function ToDoubleArray (array : double[]) : IntPtr
Description描述
Convert a managed array of System.Double to a Java array of double.
转换一个System.Double的托管数组到一个Java的双精度浮点数数组。


138、AndroidJNI.ToFloatArray 到浮点数数组
static function ToFloatArray (array : float[]) : IntPtr
Description描述
Convert a managed array of System.Single to a Java array of float.
转换一个System.Single的托管数组到一个Java的浮点数数组。


139、AndroidJNI.ToIntArray 到整数数组
static function ToIntArray (array : Int32[]) : IntPtr
Description描述
Convert a managed array of System.Int32 to a Java array of int.
转换一个System.Int32的托管数组到一个Java的整数数组。


140、AndroidJNI.ToLongArray 到长整数数组
static function ToLongArray (array : Int64[]) : IntPtr
Description描述
Convert a managed array of System.Int64 to a Java array of long.
转换一个System.Int64的托管数组到一个Java的长整数数组。


141、AndroidJNI.ToObjectArray 到对象数组
static function ToObjectArray (array : IntPtr[]) : IntPtr
Description描述
Convert a managed array of System.IntPtr, representing Java objects, to a Java array of java.lang.Object.
转换一个System.IntPtr的托管数组，表示Java对象，到一个Java的java.lang.Object数组。


142、AndroidJNI.ToReflectedField 到反射的域
static function ToReflectedField (clazz : IntPtr, fieldID : IntPtr, isStatic : bool) : IntPtr
Description描述
Converts a field ID derived from cls to a java.lang.reflect.Field object.
转换一个取自clazz的域ID到一个java.lang.reflect.Field对象。


143、AndroidJNI.ToReflectedMethod 到反射的方法
static function ToReflectedMethod (clazz : IntPtr, methodID : IntPtr, isStatic : bool) : IntPtr
Description描述
Converts a method ID derived from clazz to a java.lang.reflect.Method or java.lang.reflect.Constructor object.
转换一个取自clazz的方法ID到一个java.lang.reflect.Method 或 java.lang.reflect.Constructor对象。


144、AndroidJNI.ToShortArray 到短整数数组
static function ToShortArray (array : Int16[]) : IntPtr
Description描述
Convert a managed array of System.Int16 to a Java array of short.
转换一个System.Int16的托管数组到一个Java的短整数数组。