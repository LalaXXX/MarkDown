# Home

> 迭代思维（先发1.0）

> D：Android图形系统涉及内容很多（Vsync等概念不清晰）

## 2021.7.29

### 环境变量

- 按先后次序检索（移动过后，重开 cmd 检验是否生效）
- 默认导航至文件所在的目录

### Cmd

- cd /d D:
  - `cd /d D:\wjx\YOLOv4`
  - C盘直接 `cd` 即可

- dir /b
- ipconfig
  - ipconfig /flushdns
- keytool（jdk 自带密钥工具）
- ping
- wget（需下载）
- findstr
- java -jar xxx（执行 jar 包，**需要配置 jre 环境变量**）

### ADB 进阶

- adb shell logcat -b all
- adb shell atrace
- adb shell ifconfig（板子 ip 相关）
- adb shell cat /proc/cpuinfo
- adb shell cat /proc/meminfo
- adb shell dumpsys cpuinfo
- adb shell dumpsys meminfo

### Linux 进阶

> GNU 编译器集合是一系列用于语言开发的编译器和库的集合，包括: C, C++, Objective-C, Fortran, Ada, Go, and D等编程语言，很多开源项目，包括 Linux kernel 和 GNU 工具，都是使用 GCC 进行编译的

- `/`：根目录
- `.`：此目录
  - adb pull xxx .
- sh
- bash（兼容 sh）
- vi xxx（:q! 退出）
- tar
- mv xxx xxx
- cp -r 
- `scp -r /mnt/d/wjx/AIOT/AI-EXPRESS/deploy sunrise@192.168.1.10:/tmp/`
- mount -o xx（挂载）
- Ubuntu：
  - sudo apt update
  - sudo apt install build-essential
    - build-essential 包含了 GNU 编辑器集合，GNU 调试器，和其他编译软件所必需的开发库和工具
    - 这个命令将会安装一系列软件包，包括`gcc`,`g++`,和`make`
  - sudo apt-get xxx
  - which xxx
  - wget [url]
- CentOS
  - sudo yum 

### VSCode 快捷键

- 自动格式化：Alt + Shift + F
- 全词替换：Ctrl + Shift + L

### Notepad++

- 不生成 .bak 文件：设置 -> 首选项 -> 备份（禁用）

### Git进阶

- git add "xxx"（中间有空格的file name）
- git push origin xxx（HEAD:xxx），新建分支提交
- 新建 .gitignore 添加需要忽略的文件
- Failed to connect to github.com port 443 after 21103 ms: Timed out（github.com ping 不通）
  - 修改 C:\Windows\System32\drivers\etc\hosts，`https://github.com.ipaddress.com/www.github.com`
  - e.g 140.82.112.4 github.com（有先后次序）
  - 刷新DNS，ipconfig /flushdns
- 设置代理：
  - git config --global http.proxy 'socks5://127.0.0.1:1080' 
  - git config --global https.proxy 'socks5://127.0.0.1:1080'
- 查看代理：
  - git config --global --get http.proxy
  - git config --global --get https.proxy
- 取消代理：
  - git config --global --unset http.proxy
  - git config --global --unset https.proxy
- gitk 打开中文乱码
  - git config --global gui.encoding utf-8
### Binder机制

- LMK（Loadable Kernel Module）：

动态内核可加载模块；模块是具有独立功能的程序，它可以被单独编译，但是不能独立运行。它在运行时被链接到内核作为内核的一部分运行。这样Android 系统就可以通过动态添加一个内核模块运行在内核空间，用户进程之间通过这个内核模块作为桥梁来实现通信；在 Android 系统中，这个运行在内核空间，负责各个用户进程通过 Binder 实现通信的内核模块就叫 Binder 驱动（Binder Dirver）

- 内存映射：

通过 mmap() 来实现，mmap() 是操作系统中一种内存映射的方法。内存映射简单的讲就是将用户空间的一块内存区域映射到内核空间。映射关系建立后，用户对这块内存区域的修改可以直接反应到内核空间；反之内核空间对这段区域的修改也能直接反应到用户空间。内存映射能减少数据拷贝次数，实现用户空间和内核空间的高效互动。两个空间各自的修改能直接反映在映射的内存区域，从而被对方空间及时感知。也正因为如此，内存映射能够提供对进程间通信的支持。

<img src="https://pic4.zhimg.com/80/v2-cbd7d2befbed12d4c8896f236df96dbf_720w.jpg" style="zoom:63%;" /> <img src="https://pic3.zhimg.com/80/v2-729b3444cd784d882215a24067893d0e_720w.jpg" alt="img" style="zoom:63%;" />

<img src="https://pic4.zhimg.com/80/v2-7c68928e26f5b96b8b3471ebb1927107_720w.jpg" alt="img" style="zoom:80%;" /> <img src="https://pic4.zhimg.com/80/v2-67854cdf14d07a6a4acf9d675354e1ff_720w.jpg" alt="img" style="zoom:80%;" />

- 代理模式：

当 A 进程想要获取 B 进程中的 object 时，驱动并不会真的把 object 返回给 A，而是返回了一个跟 object 看起来一模一样的代理对象 objectProxy，这个 objectProxy 具有和 object 一摸一样的方法，但是这些方法并没有 B 进程中 object 对象那些方法的能力，这些方法只需要把把请求参数交给驱动即可。对于 A 进程来说和直接调用 object 中的方法是一样的。当 Binder 驱动接收到 A 进程的消息后，发现这是个 objectProxy 就去查询自己维护的表单，一查发现这是 B 进程 object 的代理对象。于是就会去通知 B 进程调用 object 的方法，并要求 B 进程把返回结果发给自己。当驱动拿到 B 进程的返回结果后就会转发给 A 进程，一次通信就完成了

<img src="https://pic2.zhimg.com/80/v2-13361906ecda16e36a3b9cbe3d38cbc1_720w.jpg" alt="img" style="zoom:67%;" /> 

- Binder 的完整定义：
  - 从进程间通信的角度看，Binder 是一种进程间通信的机制
  - 从 Server 进程的角度看，Binder 指的是 Server 中的 Binder 实体对象
  - 从 Client 进程的角度看，Binder 指的是对 Binder 代理对象，是 Binder 实体对象的一个远程代理
  - 从传输过程的角度看，Binder 是一个可以跨进程传输的对象；Binder 驱动会对这个跨越进程边界的对象对一点点特殊处理，自动完成代理对象和本地对象之间的转换
- AIDL：在APP里绑定一个其他APP的service，与其他APP交互；当我们定义好 AIDL 文件，在编译时编译器会帮我们生成代码实现 IPC 通信
  - IBinder：一个接口，代表了一种跨进程通信的能力。只要实现了这个借口，这个对象就能跨进程传输
  - IInterface：IInterface 代表的就是 Server 进程对象具备什么样的能力（能提供哪些方法，其实对应的就是 AIDL 文件中定义的接口）
  - Binder：Java 层的 Binder 类，代表的其实就是 Binder 本地对象。BinderProxy 类是 Binder 类的一个内部类，它代表远程进程的 Binder 对象的本地代理；这两个类都实现了IBinder接口, 因而都具有跨进程传输的能力；实际上，在跨越进程的时候，Binder 驱动会自动完成这两个对象的转换。
  - Stub：AIDL 的时候，编译工具会给我们生成一个名为 Stub 的静态内部类；这个类继承了 Binder, 说明它是一个 Binder 本地对象，它实现了 IInterface 接口，表明它具有 Server 承诺给 Client 的能力；Stub 是一个抽象类，具体的 IInterface 的相关实现需要开发者自己实现

### 事件分发

- 事件定义：点击事件（Touch事件）

<img src="https://upload-images.jianshu.io/upload_images/944365-89fdef77be149d47.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" alt="img" style="zoom:60%;" /> 

- 事件列：

<img src="https://upload-images.jianshu.io/upload_images/944365-365bf4be7d949fe7.png?imageMogr2/auto-orient/strip|imageView2/2/w/1018/format/webp" alt="img" style="zoom:70%;" /> 

- 本质：将点击事件（MotionEvent）传递到某个具体的View处理的整个过程，事件传递的过程即分发过程（Activity -> ViewGroup -> View）

![img](https://upload-images.jianshu.io/upload_images/944365-02c588300f6ad741.png?imageMogr2/auto-orient/strip|imageView2/2/w/590/format/webp) 

- 方法协作：

![img](https://upload-images.jianshu.io/upload_images/944365-7c6642f518ffa3d2.png?imageMogr2/auto-orient/strip|imageView2/2/w/675/format/webp) 

## 2021.7.30

> Q：IntentService和Thread的区别？

> D：App启动流程看的比较吃力

### Service

> 计算型组件，提供需在后台长期运行的服务（复杂计算、音乐播放、下载等）；无用户界面、在后台运行、生命周期长

- Service通常运行在应用进程的主线程中，进行耗时操作需要在Service内部开工作线程处理
- 三种启动方式：
  - startService()
  - bindService()
  - 同时调用

- 区别Service与Thread
- IntentService：处理耗时操作不用再单独开线程，因为其内部已经开了工作线程进行处理。并且在处理完成后会自动停止服务。体现出优势，service需要调用stopSelf()或者stopService()停止服务，会出现有时候忘记停止的情况，但是使用IntentService就不用担心会出现这个问题

### APP启动流程

- 两大重要进程zygote、SystemServer（Socket跨进程通信）
- C/S模式（Binder机制进行跨进程通信）
  - 服务器端：SystemServer提供ActivityManagerService、PackageManagerService、WindowManagerService（AMS、PMS、WMS工作在SystemServer进程）
  - 客户端：各App进程 

### 图片的三级缓存

- 三级缓存：
  - 网络缓存, 不优先加载, 速度慢,浪费流量
  - 本地缓存, 次优先加载, 速度快
  - 内存缓存, 优先加载, 速度最快
- 原理：
  - 首次加载 Android App 时，肯定要通过网络交互来获取图片，之后我们可以将图片保存至本地SD卡和内存中
  - 之后运行 App 时，优先访问内存中的图片缓存，若内存中没有，则加载本地SD卡中的图片
  - 总之，只在初次访问新内容时，才通过网络获取图片资源

## 2021.8.02

> D：性能优化内容比较杂乱，没有厘清

### ANR

- ANR(Application Not responding)是指应用程序未响应，主线程如果在规定时间内没有处理完相应的工作，就会出现ANR，ANR本质上是性能问题，ANR机制实际上是对应用程序的主线程起到了限制作用。
- 主线程（UI线程）如果在规定时间内没有处理完相应的工作，就会出现ANR
  - 输入事件（按键和触摸事件）5s内没有被处理：Input event dispatching timed out
  - BroadcaseReceiver的事件（onReceive方法）在规定时间内没处理完（前台广播10s,后台广播60s）：Timeout of broadcast BroadcastRecord
  - ContentProvider的publish在10s内没进行完：Timeout publishing content providers
  - service前台20s，后台200s没有完成启动 ：Timeout executing service
- 原因：
  - 主线程在做一些耗时任务
  - 主线程被其他线程锁
  - CPU被其他进程占用，该进程没被分配到足够的cpu资源
- 分析方法：
  - traces.txt文件：应用ANR产生的时候，ActivityManagerService的appNotResponding方法就会被调用，然后在/data/anr/traces.txt文件中写入ANR相关信息。最新的ANR信息在最开始部分
  - log分析：通常发生了ANR，ActivityManager会打印报错信息：ANR in com.xxx.xxxx // ANR出现的进程包名，从log中搜索“ANR in”或“am_anr”
- traces文件有哪些信息：
  - ANR的进程id、时间和进程名称
  - 线程的基本信息
  - 线程的优先级（默认5）、线程锁id和线程状态
  - 线程的调用栈信息(这里可查看导致ANR的代码调用流程，分析ANR最重要的信息)

### ThreadPoolExecutor

- 工作流程：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200730114223412.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhbnllMjAyMA==,size_16,color_FFFFFF,t_70)

## 2021.8.03

### 动画

> 类型：
>
> - 视图动画（View Animation）
>   - 补间动画（Tween Animation）
>   - 逐帧动画（Frame Animation）
> - 属性动画（Property Animation）

- 两种动画的区别：是否改变动画本身的属性
- 补间动画：

![示意图](https://github.com/LalaXXX/Memory/raw/main/imgs202207192340394.jpeg) 

```java
Button mButton = (Button) findViewById(R.id.Button);
        // 创建 需要设置动画的 视图View

        // 组合动画设置
        AnimationSet setAnimation = new AnimationSet(true);
        // 步骤1:创建组合动画对象(设置为true)

        // 步骤2:设置组合动画的属性
        // 特别说明
        // 因为在下面的旋转动画设置了无限循环(RepeatCount = INFINITE)
        // 所以动画不会结束，而是无限循环
        // 所以组合动画的下面两行设置是无效的
        setAnimation.setRepeatMode(Animation.RESTART);
        setAnimation.setRepeatCount(1);// 设置了循环一次,但无效

        // 步骤3:逐个创建子动画(方式同单个动画创建方式)

        // 子动画1:旋转动画
        Animation rotate = new RotateAnimation(0,360,Animation.RELATIVE_TO_SELF,0.5f,Animation.RELATIVE_TO_SELF,0.5f);
        rotate.setDuration(1000);
        rotate.setRepeatMode(Animation.RESTART);
        rotate.setRepeatCount(Animation.INFINITE);

        // 子动画2:平移动画
        Animation translate = new TranslateAnimation(TranslateAnimation.RELATIVE_TO_PARENT,-0.5f,
                TranslateAnimation.RELATIVE_TO_PARENT,0.5f,
                TranslateAnimation.RELATIVE_TO_SELF,0
                ,TranslateAnimation.RELATIVE_TO_SELF,0);
        translate.setDuration(10000);

        // 子动画3:透明度动画
        Animation alpha = new AlphaAnimation(1,0);
        alpha.setDuration(3000);
        alpha.setStartOffset(7000);

        // 子动画4:缩放动画
        Animation scale1 = new ScaleAnimation(1,0.5f,1,0.5f,Animation.RELATIVE_TO_SELF,0.5f,Animation.RELATIVE_TO_SELF,0.5f);
        scale1.setDuration(1000);
        scale1.setStartOffset(4000);

        // 步骤4:将创建的子动画添加到组合动画里
        setAnimation.addAnimation(alpha);
        setAnimation.addAnimation(rotate);
        setAnimation.addAnimation(translate);
        setAnimation.addAnimation(scale1);

        mButton.startAnimation(setAnimation);
        // 步骤5:播放动画
```

- 逐帧动画：

![示意图](https://github.com/LalaXXX/Memory/raw/main/imgs202207192340312.png) 

```java
   <-- 直接从drawable文件夹获取动画资源（图片） -->
        animationDrawable = new AnimationDrawable();
        for (int i = 0; i <= 25; i++) {
            int id = getResources().getIdentifier("a" + i, "drawable", getPackageName());
            Drawable drawable = getResources().getDrawable(id);
            animationDrawable.addFrame(drawable, 100);
        }

        <-- 开始动画 -->
        btn_startFrame.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                animationDrawable.setOneShot(true);
                iv.setImageDrawable(animationDrawable);
                // 获取资源对象
                animationDrawable.stop();
                 // 特别注意：在动画start()之前要先stop()，不然在第一次动画之后会停在最后一帧，这样动画就只会触发一次
                animationDrawable.start();
                // 启动动画
               
            }
        });

         <-- 停止动画 -->
        btn_stopFrame.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                animationDrawable.setOneShot(true);
                iv.setImageDrawable(animationDrawable);
                animationDrawable.stop();
            }
        });
```

- 属性动画：

![示意图](https://github.com/LalaXXX/Memory/raw/main/imgs202207192340092.jpeg)

<img src="https://github.com/LalaXXX/Memory/raw/main/imgs202207192339080.png" style="zoom: 80%;" />  

### 屏幕适配

- 概念：

  - 屏幕尺寸：手机对角线的物理尺寸，英寸（inch），1英寸=2.54cm；Android手机常见的尺寸有5寸、5.5寸、6寸等等
  - 屏幕分辨率：手机在横向、纵向上的像素点数总和，px（pixel），1px=1像素点；Android手机常见的分辨率320x480、480x800、720x1280、1080x1920
  - 屏幕像素密度：每英寸的像素点数，dpi（dots per ich）

  | 密度类型             | 代表的分辨率（px） | 屏幕像素密度（dpi） |
  | -------------------- | :----------------: | ------------------: |
  | 低密度（ldpi）       |      240x320       |                 120 |
  | 中密度（mdpi）       |      320x480       |                 160 |
  | 高密度（hdpi）       |      480x800       |                 240 |
  | 超高密度（xhdpi）    |      720x1280      |                 320 |
  | 超超高密度（xxhdpi） |     1080x1920      |                 480 |

  - 密度无关像素（density-independent pixel，叫dp或dip）：与终端上的实际物理像素点无关，dp，可以保证在不同屏幕像素密度的设备上显示相同的效果
  - 独立比例像素（scale-independent pixel，叫sp或sip）：sp，Android开发时用此单位设置文字大小，可根据字体大小首选项进行缩放

- 解决方案：

![img](https://github.com/LalaXXX/Memory/raw/main/imgs202207192339839.webp) 

![img](https://github.com/LalaXXX/Memory/raw/main/imgs202207192339242.webp) 

### RecyclerView

## 2021.8.04

### TCP/IP三次握手四次挥手

### HTTP与HTTPS

## 2021.12.4

### 算法

> 算法是为一个问题或一类问题给出的解决方法与具体步骤，是对问题求解过程的一种准确而完整的**逻辑描述**。**程序**则是为了用计算机解题或控制某一过程而编排的一系列**指令的集合**。程序不等于算法，但是，通过程序设计可以在计算机上实现算法。

- 给定两个字符串s1，s2，将s1中的包含在s2中的字符翻转

```java
import java.util.*;

public class Solution {
    /**
     * 代码中的类名、方法名、参数名已经指定，请勿修改，直接返回方法规定的值即可
     *
     * 
     * @param s1 string字符串 
     * @param s2 string字符串 
     * @return string字符串
     */
    public String reverse (String s1, String s2) {
        String str = "";
        Stack<Character> stk = new Stack<>();
        int start = 0;
        int end = s1.length() - 1 ;
        char[] charS1 = s1.toCharArray();
        while(start < end){
            if(s2.contains(String.valueOf(charS1[start]))){
                stk.add(charS1[start]);
            }else{
                start++;
                continue;
            }
            if(s2.contains(String.valueOf(charS1[end]))){
                if(!stk.isEmpty()){
                    char temp = charS1[end];
                    charS1[end] = stk.pop();
                    charS1[start] = temp;
                    start++;
                    end--;
                }else{
                    end--;
                }
            }
        }
        str = new String(charS1);
        return str;
    }
    
}
```

- 

### Object.wait

- Object的wait方法：wait指线程处于进入阻塞状态，**不占用任何资源**，不增加时间限制。

- Thread的sleep方法： sleep指线程被调用时，**占着CPU不工作**，其他线程无法进入，会增加时间限制。
- Thread的yield方法：yield()的作用是让步。它能让当前线程由**“运行状态”**进入到**“就绪状态”**，从而让其它具有相同优先级的等待线程获取执行权；但是，并不能保证在当前线程调用yield()之后，其它具有相同优先级的线程就一定能获得执行权；也有可能是当前线程又进入到“运行状态”继续运行！

- Thread的join方法：join()方法将挂起调用线程的执行，**直到**被调用的对象完成它的执行。故而**是阻塞状态**。比如系统目前运行线程A，在线程A里面调用了线程B.join方法，则接下来线程B会抢先在线程A面前执行，等到线程B全部执行完后才继续执行线程A。

- Thread的interrupt方法：其作用是**中断**此线程（此线程不一定是当前线程，而是指调用该方法的Thread实例所代表的线程），但实际上只是给线程**设置一个中断标志，线程仍会继续运行**。

### 平衡二叉树

- 最小节点数：min(n) = F(n+2) - 1，其中，F为斐波那契数列， 1 1 2 3 5 8 13 ...，所以 min(5) = F(7) - 1 = 13 - 1 = 12
- 最多节点数：max(n) = 2^n - 1

### 数据库特性

-  原子性
- 一致性
- 隔离性
- 持久性

## 2022.1.3

### Handler实践

- 需要在子线程请求网络获得数据，之后再切换回主线程更新UI

### final关键字

### this关键字

- `this`关键字可用来引用当前类的实例变量
- `this`关键字可用来调用当前类的方法(隐式)
- `this()`可以用来调用当前类的构造函数ss

### 变量类型

- 类变量：独立于方法之外的变量，用 static 修饰（静态成员变量）
- 实例变量：独立于方法之外的变量，没有 static 修饰（非静态成员变量）
- 局部变量：类的方法中的变量

## 2022.1.23

### AS快捷键

- Shift  + 1
- Shift  + 7
- Shift  + 2
- Shift  + 8

### 接口（interface）

- 接口只能继承接口，类可以实现接口

### ContentProvider

> Android的数据存储方式：
>
> - Shared Preferences
> - 网络存储
> - 文件存储
> - 外储存储
> - SQLite

- 中间人，真正操作数据源可能是数据库，也可以是文件、xml或网络等其他存储方式

## 2022.3.6

### 冷启动

> Starting Window显示的就是你启动Activity的`android:windowBackground`属性，所以才会出现白屏或者黑屏的情况。

### Android内核

- 用户空间
- 内核空间

### CPU

- 用户态
- 内核态

## 2022.4.17

### Wifi

> - 基础网（Infra）：由AP创建，众多STA加入所组成的无线网络，这种类型的网络的特点是AP是整个网络的中心，网络中所有的通信都通过AP来转发完成
>   - AP无线接入点，是一个无线网络的创建者，是网络的中心节点，一般家庭或办公室使用的无线路由器就是一个AP 
>   - STA站点，每一个连接到无线网络中的终端（如笔记本电脑、PDA及其它可以联网的用户设备）都可称为一个站点
>   - WLAN（Wireless Local Area Network）
>
> - 自组网（Adhoc）：仅由两个及以上STA自己组成，网络中不存在AP，这种类型的网络是一种松散的结构，网络中所有的STA都可以直接通信

![img](https://github.com/LalaXXX/Memory/raw/main/imgs202207192338079.png) 

![img](https://raw.githubusercontent.com/LalaXXX/Memory/main/imgs202207192334378.webp)

- WifiStateTracker 会创建 WifiMonitor 接收来自底层的事件，WifiService 和 WifiMonitor 是整个模块的核心。
- WifiService 负责启动关闭 wpa_supplicant、启动关闭 WifiMonitor 监视线程和把命令下发给 wpa_supplicant，WifiMonitor 负责从 wpa_supplicant 接收事件通知
- WifiService负责wifi整个流程的控制，而WifiMonitor负责监视底层的事件

## 2022.4.18

### 蓝牙

### NFC

### 移动网络

### 人脸

### 指纹

## 2022.4.19

### HAL

### 工作配置必备

- AS（adb、数据线、测试机）
- Git（ssh）
- Typora（.md）
- Notepad++（.txt）
- MobaXterm
- Google Chrome（Ghelper、沙拉查词）
  - 使用其它浏览器下载Ghelper（crx），安装插件
  - chrome://extensions/shortcuts（Alt + w），修改快捷键
- WPS（Excel、PPT）

### git配置

> git config core.autocrlf false
>
> git config --get core.autocrlf

- ```shell
  git init
  ```

- ```shell
  ssh-keygen -t rsa -C "lalaxiang66@gmail.com"
  ```

- Github（Git应用：Gerrit、GitLib）添加SSH keys

- new Repositories

- ```shell
  git remote add origin git@github.com:LalaXXX/MarkDown.git(xxx)(SSH、HTTPS)
  ```

- git status

- git add .

- git commit -m "xxx"/git commit

- ```shell
  //[new branch]      master -> master
  git push origin master
  ```

### MIUI调试

- adb pair 192.168.0.102:xxx / code
- adb connect 192.168.0.102:42121（IP地址和端口）
- 关闭MIUI优化
- adb install -r -t -d xxx

### APK

- AndroidManifest.xml：应用的全局配置文件
- assets文件夹：原始资源文件夹，对应着Android工程的assets文件夹，一般用于存放原始的网页、音频等等，与res文件夹的区别这里不再赘述，可以参考上面介绍的两篇文章
- classes.dex：源代码编译成class后转成jar，再压缩成dex文件，dex是可以直接在Android虚拟机上运行的文件
- lib文件夹：引用的第三方sdk的so文件
- META-INF文件夹：Apk签名文件
- res文件夹：资源文件，包括了布局、图片等等
- resources.arsc：记录资源文件和资源id的映射关系

### 性能优化

> chrome://tracing/

- 布局优化

<img src="https://github.com/LalaXXX/Memory/raw/main/imgs202207192328595.png" alt="2" style="zoom:67%;" /> 

- 可观测性技术
  - Log
  - Metric
  - Trace

## 2022.4.21

### Logcat key

- ActivityTaskManager：启动时间

### Perfetto（替代systrace）
## 2022.6.24

### npm

> npm 是 node.js 的包管理器（node package manager）
>
> - Node.js 是一个 Javascript 运行环境（runtime environment），不是一个 js 文件
>
> - Node.js 是一个基于 Chrome V8 引擎的 Javascript 运行环境，是用 C++ 写的
>
> - Node.js 不是库，是一个运行环境，或者说是一个 JS 语言解释器
>
>   - ```shell
>     //进node 环境
>     > node
>     ```
>
>   - ```shell
>     //node 命令执行，类似 python 
>     > node xxx.js
>     ```

> ```
> npm install packageName //本地安装，安装到项目目录下，不在package.json中写入依赖
> npm install -g packageName //全局安装，安装在Node安装目录下的node_modules下
> npm install --save packageName //安装到项目目录下，并在package.json文件的dependencies中写入依赖，简写为-S
> npm install --save-dev packageName //安装到项目目录下，并在package.json文件的devDependencies中写入依赖，简写为-D
> ```

- npm config set python python2.7

- npm config list

- npm install -> node_modules

  - 需要 Python2.0 环境
  - 提示 added xxx packages, and audited xxx packages in xxx

- npm run build -> dist

  - ERROR in build.js from UglifyJs

    - npm install --save-dev babel-preset-es2015
    - 修改【webpack.config.js】配置文件：找到 `/\.js$/`的rules，进行修改

    ```css
     {
            test: /\.js$/,
            use: [{
              loader: 'babel-loader',
              options: {
                 presets: ['es2015']
              }
            }]
          }
    ```

    - 根目录下添加【.babelrc】文件

    ```prolog
    {
      "presets": ["es2015"]
    }
    ```

- npm run dev

  - Chrome -> 开发者工具 -> Toggle device toolbar

### DApp开发环境

> PATH配置：默认进入文件夹路径
>
> 比特币：Bitcoin
>
> 以太坊：Ethereum（ETH货币）

- npm install -g @vue/cli
- vue create crowdfunding（工程文件夹）
- npm install -g truffle
- cd crowdfunding
- truffle init
- truffle compile
  - 合约中，字串不能为中文，编译不通过
  - 根据 Error 信息修改
- 在migrations下创建部署脚本，2_crowfunding.js

```js
const crowd = artifacts.require("Crowdfunding");

module.exports = function (deployer) {
  deployer.deploy(crowd);
};
```

- 配置 truffle-config.js

```js
ganache: {
      host: "127.0.0.1",     // Localhost (default: none)
      port: 8545,            // Standard Ethereum port (default: none)
      network_id: "*",       // Any network (default: none)
    },
```

- truffle develop（develop，不联网）

- truffle console --network ganache（ganache，联网）

  - Cannot find module '@truffle/hdwallet-provider
    - npm install -g @truffle/hdwallet-provider
    - npm install @truffle/hdwallet-provider
  - Error: ENOENT: no such file or directory, open '.secret'
  - Error: EISDIR: illegal operation on a directory, read
    - 根目录下新建，隐藏的可读文件（file） .secret
  - Could not connect to your Ethereum client with the following parameters:
    - 下载 Ganache 客户端，修改端口号 8545

- truffle migrate

- npm install --save truffle-contract web3

- npm run serve

  - webpack ＜ 5 used to include polyfills for node.js core modules by default.

    - 配置 vue.config.js

    ```js
    const { defineConfig } = require('@vue/cli-service')
    //added
    const NodePolyfillPlugin = require('node-polyfill-webpack-plugin')
    
    module.exports = defineConfig({
      transpileDependencies: true,
      //added
      configureWebpack: {
    	plugins: [ new NodePolyfillPlugin() ]
      }
    })
    ```


- truffle migrate

### webapp

> html、xml、js 区别？
>
> html 与 js 怎么交互？
>
> 带空格的字串属性，需要使用""包围
>
> ```html
> <!-- 文本 -->
>  <a-text
>     id= start-text 
>     value= Start 
>     color= #BBB 
>     position= "-3 6 -18 "
>     scale= "10 10 10" 
>     font= mozillavr 
> ></a-text>
> ```

- 直接运行本地**文件系统中**的文件

```
file:///yourFile.html
```

- 访问运行在**本地服务器上**的文件

```
http://localhost/yourFile.html
```

- 通过 localhost 访问 html 文件进行开发
  - npm install -g http-server
  - http-server -p ${your-port} ./
  - 本地环境：访问  `http://localhost:${your-port}`
  - 局域网环境：
    - 访问 `http://ip:${your-port}`
    - npm install -g **anywhere**
    - anywhere -p xxx
    - 访问 `https://ip:${your-port}`（webxr api 要求必须使用 https 协议）

- 部署 Tomcat 本地局域网访问 webapp
  - 启动 C:\Users\Lenovo\Desktop\apache-tomcat-10.0.21\bin\startup.bat
  - C:\Users\Lenovo\Desktop\apache-tomcat-10.0.21\webapps 目录下新建工程文件夹内容
  - 关闭系统防火墙
  - 访问 `http://ip:port/ProjectName`

## 2022.7.07

### 地平线 X3M sdb

- windows 环境安装 wget，`http://downloads.sourceforge.net/gnuwin32/wget-1.11.4-1-setup.exe`，添加环境变量
- WSL 环境安装 wget：复合命令，需要分开调试，安装环境
  - tar -zxvf cmake-3.17.2.tar.gz
    - 若 ./bootstrap 多次失败，删除 cmake-3.17.2 文件夹，在 Windows 环境下解压后，再次尝试

  - ./bootstrap
    - Cannot find appropriate C compiler on this system 需要安装 gcc、g++
      - sudo apt update
      - sudo apt install build-essential

    - Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the system variable OPENSSL_ROOT_DIR
      - sudo apt-get install openssl
      - sudo apt-get install libssl-dev

    - CMake has bootstrapped.  Now run make.（成功）


```bash
wget https://github.com/Kitware/CMake/releases/download/v3.17.2/cmake-3.17.2.tar.gz \
    && tar -zxvf cmake-3.17.2.tar.gz \
    && cd cmake-3.17.2 \
    && ./bootstrap \
    && make \
    && sudo make install \
    && cd .. \
    && rm -rf cmake-3.17* 
```

- 安装串口驱动，`https://developer.horizon.ai/resource`，设备管理器识别串口，MobaXterm 创建 Serial会话，串口登录 root
- 查看串口打印的 boot 信息
- balenaEtcher 导入 system_sdcard.img 制作系统盘到sd卡，断电插入sd卡，上电后，再重复一次

![image-20220708112315166](https://github.com/LalaXXX/Memory/raw/main/imgs202207191632798.png)

- 串口登录 sunrise（sd卡不能拔出）
- （可选）创建 SSH 会话
  - 网络设置 - 更改适配器选项 - 以太网 - 属性 - IPv4，修改 IP 地址（192.168.0.107 -> 192.168.1.100）和开发板（192.168.1.10）处于同一网段
  - `hrut_ipfull s 192.168.0.10 255.255.255.0 192.168.1.1`，可修改开发板 IP 地址
  - Remote Host：192.168.1.10，用户名：sunrise，Password：sunrise
- （可选）hbupdate 刷系统 img
  - could not open port 'com3'，重新 link 串口
  - can not start tftp，下载方式选择 uart

### AI-EXPRESS

- bash build.sh x3（生成 build output 文件夹）
- bash deploy.sh（生成 deploy 文件夹）
- WSL 环境执行，`sudo scp -r /mnt/d/wjx/AIOT/AI-EXPRESS/deploy sunrise@192.168.1.10:/tmp/`，复制 deploy 文件夹到开发板
- bash run.sh w
  - 连接摄像头后，SSH session 退出，拔插一次网线重连
- sh run.sh w
  - camera init fail，更换 MIPI 线（E488937 ...）

### WSL

> Windows Subsystem for Linux

- 控制面板 - 程序 - 启动或关闭Windows 功能 - 适用于 Linux 的 ....
- 重启
- Store 下载 Ubuntu
  - 用户名：lalax
  - Password：root
- cmd - bash

### SSH

> Secure Shell（安全外壳协议，简称SSH）是一种加密的网络传输协议，可在不安全的网络中为网络服务提供安全的传输环境

### SSL、TLS、HTTPS

> Secure Socket Layer（安全套接字层）
>
> Transport Layer Security（传输层安全）

## 2022.7.16

### Framework 学习

> 基础知识、经验、方法技巧

## 2022.7.18

### GDPR

> General Data Protection Regulation

### App自启动

- 进程启动
  - App = 进程（必选）+ Activity（可选）+ BroadcastReceiver（可选）+ Service（可选）+ ContentProvider（可选) + 子进程（可选）
  - 四大组件可单独启动
  - 四大组件启动之前, 系统都会检查对应的进程是否存在, 如果进程不存在 , 那么就需要先启动进程, 再启动这个组件
    - 在桌面上点击一个应用图标, 其实启动的就是他的 Activity , 系统会先创建进程, 然后再启动 Activity , 我们才可以看到对应的界面
  - 一般自启动和关联启动，一般不会直接启动 Activity , 因为 Activity 是用户可感知的，
  - 所以自启动和关联启动都是在 BroadcastReceiver（可选） + Service（可选）+ ContentProvider（可选）三个上面做文章

- 自启动
  - 自启动指的是不借助其他的应用，通过监听系统的一些事件，或者文件变化，通过系统的机制，把自己的进程拉起来处理事情
- 关联启动
  - 关联启动指的是借助其他应用来启动自己，比如起点读书启动作家助手\电信营业厅\百词斩

### APM

> Application Performance Management

### crtc_commit

### 卡顿丢帧

- 系统

  - SurfaceFlinger 主线程耗时

    - 主要工作就是合成

    ```cpp
    void SurfaceFlinger::onMessageReceived(int32_t what, int64_t vsyncId, nsecs_t expectedVSyncTime) {
        switch (what) {
            case MessageQueue::INVALIDATE: {
                onMessageInvalidate(vsyncId, expectedVSyncTime);
                break;
            }
            case MessageQueue::REFRESH: {
                onMessageRefresh();
                break;
            }
        }
    }
    ```

    - handleMessageRefresh
      - 准备工作
        1. preComposition();
        2. rebuildLayerStacks();
        3. calculateWorkingSet();
      - 合成工作
        1. begiFrame(display);
        2. prepareFrame(display);
        3. doDebugFlashRegions(display, repaintEverything);
        4. doComposition(display, repaintEverything);
      - 收尾工作
        1. logLayerStats();
        2. postFrame();
        3. postComposition();

  - SurfaceFlinger Vsync 不均匀

## 2022.7.19

### PicGo 图桌配置

![image-20220719160111938](https://github.com/LalaXXX/Memory/raw/main/imgs202207191601982.png) 

- 流程

  - github 生成 token，给予 repo 权限
  - 配置 github 信息到 PicGo

  ![image-20220719160009674](https://github.com/LalaXXX/Memory/raw/main/imgs202207191600720.png) 

  - PicGo 设置，开启 `时间戳重命名`  及 `上传后复制`
  - Typora -> 设置 -> 图片，将 `插入图片时` 配置为上传图片，`上传服务` 选定为 PicGo.app，选择 PicGo 路径
  - 点击 `验证图片上传选项`

- 找不到 git 配置
  
  - 点击确定
- 服务端未响应
  
  - 设定分支名为 main
- 图片上传后不显示
  - 确保本地网络环境 ping 通 github.com
    - 参考 `Git进阶` 第四点
  - `https://github.com/LalaXXX/Memory/blob/main/imgs202207191555286.png`，blob 改为 raw
  - `https://github.com/lalaxxx/memory/imgs202207191558825.png`（不可用），必须使用规定的自定义域名格式
  - `https://github.com/LalaXXX/Memory/raw/main/imgs202207191600720.png`（可用）
  - `https://raw.githubusercontent.com/LalaXXX/Memory/main/`（可用）
  - 开启 VPN

### xxx-core

> 使用 command line，区别于 软件应用

- v2rayN-Core
- PicGo-Core

### Android 源码编译
### Android 图形数据流向

> VSYNC-app 先于 VSYNC-sf
>
> surfaceflinger（HWC）：FramebufferSurface
>
> android.hardware.graphics.composer：FrameQueue、EventThread、VSyncThread
>
> UI Thread -> Render Thread（渲染） -> surfaceflinger（合成）
>
> OpenGL（Open Graphics Library）

![image-20220725160958535](https://github.com/LalaXXX/Memory/raw/main/imgs202207251609596.png) 

- 第一阶段：App 在收到 Vsync-App 的时候，在主线程进行 measure、layout、draw(构建 DisplayList , 里面包含 OpenGL 渲染需要的命令及数据) 。这里对应的 Systrace 中的主线程 **doFrame** 操作
- 第二阶段：CPU 将数据上传（共享或者拷贝）给 GPU,　这里 ARM 设备 内存一般是 GPU 和 CPU 共享内存。这里对应的 Systrace 中的渲染线程的 **flush drawing commands** 操作
- 第三阶段：通知 GPU 渲染，真机一般不会阻塞等待 GPU 渲染结束，CPU 通知结束后就返回继续执行其他任务，使用 Fence 机制辅助 GPU CPU 进行同步操作
- 第四 阶段：swapBuffers，并通知 SurfaceFlinger 图层合成。这里对应的 Systrace 中的渲染线程的 **eglSwapBuffersWithDamageKHR** 操作
- 第五阶段：SurfaceFlinger 开始合成图层，如果之前提交的 GPU 渲染任务没结束，则等待 GPU 渲染完成，再合成（Fence 机制），合成依然是依赖 GPU，不过这就是下一个任务了，这里对应的 Systrace 中的 SurfaceFlinger 主线程的 onMessageReceived 操作（包括 handleTransaction、handleMessageInvalidate、handleMessageRefresh）SurfaceFlinger 在合成的时候，会将一些合成工作委托给 Hardware Composer,从而降低来自 OpenGL 和 GPU 的负载，只有 Hardware Composer 无法处理的图层，或者指定用 OpenGL 处理的图层，其他的 图层偶会使用 Hardware Composer 进行合成
- 第六阶段 ：最终合成好的数据放到屏幕对应的 Frame Buffer 中，固定刷新的时候就可以看到了

## 2022.7.26

### CUDA

> Compute Unified Device Architecture，统一计算架构

### Python

- 字符串 `''` （单引号）包裹
- 不能有多余空格

### YOLOv4

- conda create --name torch36 python=3.6（指定 python 版本，创建虚拟环境）
  
- C:\Users\Lenovo\anaconda3\envs\torch36
  
- conda activate torch36

- conda install pytorch torchvision torchaudio cpuonly -c pytorch（安装 pytorch，CPU）

  ```python
  device = torch.device('cuda' if torch.cuda.is_available() else 'cpu')
  
  # model.cuda()
  model.to(device)
    
  # prediction = model([image.cuda()])
  prediction = model([image.to(device)])
  ```

- 安装 opencv-pytorch，`https://www.lfd.uci.edu/~gohlke/pythonlibs/#opencv`，将其放到前面创建的 torch36 的 site-packages 文件夹中

- pip install opencv_python-4.4.0-cp36-cp36m-win_amd64.whl（根据 python 版本选择）

  - C:\Users\Lenovo\anaconda3\envs\torch36\Lib\site-packages

- python demo.py -cfgfile cfg/yolov4.cfg -weightfile weights/yolov4.weights -imgfile data/dog.jpg

  - pytorch-YOLOv4 目录下 demo.py 文件

- 生成 predictions.jpg 文件

<img src="https://github.com/LalaXXX/Memory/raw/main/imgs202207262139316.jpg" alt="predictions" style="zoom: 80%;" /> 

