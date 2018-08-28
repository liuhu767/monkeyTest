# monkeyTest
压力测试
Monkey总结（一）
Monkey是什么？

      Monkey是Google提供的一个命令行工具，可以运行在模拟器或者实际设备中。它向系统发送伪随机的用户事件（如按键、手势、触摸屏等输入），对软件进行稳定性与压力测试。Monkey程序是Android自带的。

      路径：/sdcard/system/framework/Monkey.jar

      启动脚本路径：/system/monkey

    

Monkey环境变量的配置

      Monkey是由adb命令启动，故只要配置adb环境（windows环境为例）

      1.下载Android SDK，解压
      2.将SDK目录下的platform-tools和tools目录配置到系统环境变量中
      3.打开cmd窗口，输入adb，adb帮助信息则配置成功



Monkey基本参数
      一个完整的Monkey命令：

         adb shell monkey -p cn.microinvestment.weitou --pct-touch 100 --ingore-crashes --throttle 1000 -s 100 -v -v 50

      以上是由Monkey基本参数组合而成。先看Monkey的基本参数：

      

分类	选项	说明
基本参数	--help	打印帮助信息
-v	Monkey向命令行打印输出的log信息级别
    默认级别是0:-v只打印启动信息，测试完成信息
    和最终结果信息
    级别2:-v -v 打印时执行的一些信息，如发送事件
    级别3:-v -v -v 打印最详细的信息


Monkey的约束参数


分类	选项	说明
约束条件	-p<允许执行的包名列表>	如果用词参数指定了一个或几个包，Monkey将只允许系
    统启动这些包里的Activity。如果你的应用程序还需要
      访问其它包里的Activity（如选择取一个联系人），那些包也需要在此同时指定。如果不指定任何包，Monkey将允许系统启动全部包里的Activity。要指定多个包，需要使用多个-p选项，每个-p只能用于一个包。
-c<意图的种类>	如果用此参数指定了一个或几个类别，Monkey将只允许系统启动被这些类别中的某个类别列出的Activity。如果不指定任何类别，Monkey将选择下列类别中列出的Activity:Intent.CATEGORY.LAUNCHER或Intent.CATEGORY.MONKEY。要指定多个类别，需要使用多个-c选项，每个-c选项只能用于一个类别。


Monkey发送的事件类型和频率

事件	-s<随机数种子>	伪随机数生成器的seed值。如果用相同的seed值再次运行Monkey，它将生成相同的事件序列
--throttle<意图的种类>	在事件之间插入固定延迟。通过这个选项可以减缓Monkey的执行速度。如果不指定该选项，Monkey将不会被延迟，事件将尽可能快地被执行完成
--pct-touch<percent>	调整触摸事件的百分比（触摸事件是一个down-up事件），它发生在屏幕上的某单一位置
--pct-motion<percent>	调整动作事件的百分比（动作事件是由屏幕上某处的一个down事件、一系列的伪随机事件和一个up事件组成）
--pct-trackball<percent>	调整轨迹事件的百分比（轨迹事件由一个或几个随机的移动组成，有时还伴随有点击）
--pct-nav<percent>	调整“基本”导航时间的百分比（导航事件由来自方向输入设备的up/down/left/right组成）
--pct-majornav<percent>	调整“主要”导航事件的百分比（这些导航事件通常引发图形界面中的动作，如5-way键盘的中间按键、回退按键、菜单按键）
--pct-syskeys<percent>	调整“系统”按钮事件的百分比（这些按键通常被保留，由系统使用，如Home、Back、StartCall、End   Call及音量控制键）
--pct-appswitch	调整启动Activity的百分比。在随机间隔里，Monkey执行一个startActivity()调用，作为最大程度覆盖包中全部Activity的一种方法
--pct-anyevent<percent>	调整其他类型事件的百分比。它包罗了所有其他类型的事件，如：按键、其他不常用的设备按钮等
--pct-flip PECENT	　
--pct-pinchzoom PERCENT	　


Monkey调试参数


分类	选项	说明
调试	--dbg-no-events	设置此选项，Monkey将执行初始启动，进入到一个测试Activity，然后不会再进一步生成事件。最好将它与-v、一个或几个包约束。以及一个保持Monkey运行30秒或更长事件的非零值联合起来，从而提供一个环境，可以监视应用程序所调用的包之间的转换
--hprof	设置此选项，将在Monkey事件序列之前和之后立即生成profiling报告，这将在data/misc中生成大文件（~5MB），所以要小心使用
--ignore-crashes	通常，当应用程序崩溃或发生任何失控异常时，Monkey将停止运行。如果设置此选项，Monkey将继续向系统发送事件，直到计数完成
--ignore-timeouts	应用程序发生任何超时错误（如"Application   Not Responding"对话框）时，Monkey将停止运行。如果设置此选项，Monkey将继续向系统发送事件，直到计数完成
--ingore-security-exceptions	当应用程序发生权限许可错误时，Monkey将停止运行。如果设置了此选项，Monkey将继续向系统发送事件，直到计数完成
--ingnore-native-crashes	当应用程序发生底层C/C++代码引起的崩溃事件时，Monkey将停止运行。选择此项，Monkey将继续向系统发送事件，直到计数完成
--monitor-native-crashes	监视并报告Android系统中Android   C/C++引起的崩溃事件。如果设置了--kill-process-after-error，系统将停止运行
--kill-process-after-error	当Monkey由于一个错误而停止时，出错的应用程序将继续处于运行状态。当设置了此选项时，将会通知系统停止发生错误的进程。注意，当Monkey正常执行完毕，它不会关闭所有启动的应用，设备依然保留Monkey结束时的状态
--wait-dbq	启动Monkey后，先中断其运行，等待调试器附加上来


Monkey黑白名单

          黑名单：不测试的应用

          白名单：只测试这部分应用

          注意：不能同时设置黑名单和白名单

选项	说明
--pkg-blacklist-file
     PACKAGE_BlACKLIST_FILE	apk黑名单，屏蔽掉黑名单中的apk
--pkg-whitelist-file
    PACK_WHITELIST_FILE	apk白名单，只测试包含在白名单中的apk
           以黑名单为例，具体的步骤如下：

           1.查找系统的包，并输出到e盘的pkg文档里。adb shell pm list package>e:\pkg.txt

           2.将想要加入黑名单的apk的包名放到blacklist.txt里，最后push进设备。adb push e:\blacklist.txt /data/local/tmp/

           3.执行Monkey命令。adb shell monkey --pkg-blacklist-file /data/local/tmp/blacklist.txt --throttle 200 200


Monkey总结（三）
      众所周知，Monkey是一个压力测试工具，但是它可以用来做自动化测试，而且无需任何的工具，更不需要搭建环境，只需要一个文本文档编写好脚本运行，即可实现坐标、按键等基本操作。缺点是没有逻辑性。（http://t.leborn.me/blog/home/detail/1510190001579752374）
      
      一个简单的例子：

打开网页，输入www.baidu.com。

1.首先新建一个文档，命名为monkey.txt。

2.在里面输入：

      #头文件、控制monkey发送消息的参数

       type=raw events

       cont=10

       speed=1.0

       #以下为monkey命令

       start data >>

       #打开浏览器,并延时默认的时间

       LaunchActivity(com.android.browser,com.android.browser.BrowserActivity)

       ProfileWait()

       #清空网址

       Tap(500,150)

       DispatchPress(112)

       ProfileWait()

       #输入网址

       DispatchString(www.baidu.com)

       ProfileWait()

       #确认，载入网址

       DispatchPress(KEYCODE_ENTER)

       ProfileWait()

       #完成退出浏览器

       DispatchPress(3)

3.保存后push到手机里去

       adb push D:\monkey.txt /data/local/tmp/

4.执行monkey命令：     

       adb shell monkey -f /data/local/tmp/monkey.txt --throttle 500 -v -v 1



打开模拟器，顺利执行完毕。也可以用来做压力测试。
      


