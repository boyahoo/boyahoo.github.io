## Google Pixel 3 完美电信 4G 

十一前下单了一台 Google Pixel 3 手机，来回折腾了几遍，搞定了电信的电话、短信和上网，简单做个总结。

### 一、买手机时的考量因素

一直没敢下单 Google 系列手机，前几年还叫 Nexus 时是因为穷，到了 Pixel 则是因为电信号码的支持问题，懒得折腾，没有太大的兴趣。终于下单 Pixel 手机，是因为价格便宜。好吧，我承认，说到底还是一个穷字闹的。

至于具体的 Pixel 系列。或者说 Google 手机的优势，网上说的其实已经比较多，回头等使用一段时间之后，我再单开一篇说。最终吸引我下单，就是 Amazon 上 Pixel 3 居然只卖到只需要 12 张毛爷爷，这还要啥自行车，冲就是了。之所以不选择其他，一是因为不喜欢二郎神或者偏了的二郎神，也不喜欢西瓜太郎，因而 Pixel 3 就称为 Amazon 在售的 Google 手机最优选择。

### 二、为什么要折腾

在下单前就已经知道 Pixel 3 原封不动的话，是无法支持中国电信的手机卡的，连电话、短信都无法实现，简直就是个有 Sim 卡槽但又用不了 Sim 卡的触屏玩具。也知道至少系统版本是 Android 11 及之前的 9 、 10 ，都可以通过自己折腾，实现比较完美的电信支持。
这就是要折腾的由来，但显然这并不是我的风格。按我的想法，应该是通过对比 Google 和电信的双方参数，指出其中不同，从而得出为什么要折腾的结论。虽然在看攻略前，仅仅对比只能得出 Pixel 3 不支持电信的原因，并不一定能推论出折腾下就可以使用电信 4G 。
下面是我从各自官网查到的数据。

[ Google 官方](https://support.google.com/pixelphone/answer/7158570?hl=zh-Hans#zippy=%2Cpixel) 对于的 Pixel 3 网络支持描述：

```markdown

支持全球网络/运营商：8
	• GSM/EDGE：四频段（850、900、1800、1900 MHz）
	• UMTS/HSPA+/HSDPA：频段 1/2/4/5/8
	• CDMA EVDO Rev A：BC0/BC1/BC10（日本 SKU 除外）
	• WCDMA：W1/W2
	• FDD-LTE：频段 1/2/3/4/5/7/8/12/13/17/18/19/20/25/26/28/29/32/66/71
	• TD-LTE：频段 38/39/40/41/42/46
	• 最高支持 CAT 16（1Gbps DL / 75Mbps UL)、5x DL CA、4x4 MIMO、LAA、256-QAM DL 和 64-QAM UL，具体取决于运营商支持情况
日本 SKU 也支持 FeliCa。

```

电信在官网我未查到相关数据，暂且引用 [Wikipedia](https://zh.wikipedia.org/zh-cn/%E4%B8%AD%E5%9B%BD%E7%94%B5%E4%BF%A1) 的数据：

```markdown

• FDD-LTE：频段 1/3/5/26
• TD-LTE：频段 40/41

```

因为电信 3G 和 2G 在陆续退网中，所以其实我只需要关心 4G 频段支持就行：手机支持的频段覆盖电信，那理论上没问题。在现实世界中，理论上可行、实际行不通的比比皆是，占据了绝大多数。不过，感谢那些先行者，他们已经帮我们趟出了一条路：解锁 bootloader 、刷入 Magisk 、写 Chinese Sim Supporter 模块、重启，电话、短信、 4G 上网完美的出现在你的手机上。

### 三、准备工作

好了，经过前面的铺垫，我们开始解锁手机的 bootloader 吧。且慢，完事备份先行，然后才是准备我们需要的东西： adb 调试工具、 Magisk 的 apk 文件、系统的 boot.img 镜像、 Chinese Sim Supporter 文件；此外还需要科学上网工具，不管是路由器还是手机 apk 文件，尤其是手机 apk 文件，要注意需与手机系统版本匹配。

3.1 Android SDK 工具包，即 adb 的安装和配置，网上已经有比较多的教程，我参考的是[这篇](https://sspai.com/post/57427) 。回头有时间我自己总结一下。

3.2 Magisk 的 apk 文件直接在官网下载即可。

3.3 系统的 boot.img 镜像也建议在[官网](https://developers.google.com/android/images)下载， 下载跟自己手机版本 (Settings - About phone - Build number) 一致的。要注意的是 Google 对自己的亲儿子比较亲，每月必更新，所以在做本文的工作之前，建议将手机更新到不能再新再做，避免更新后相关更改失效。而且版本号一定要一模一样，一个字都不能差。下载后解压zip包，在解压后的文件里找到类似名为 image-blueline-rq3a.211001.001.zip 的文件，继续解压，然后就能看到我们需要的、名为 boot.img 的文件了。

3.4 Chinese Sim Supporter 文件建议使用 apporc 和 TadiT7 在 Github 发布的[包](https://github.com/apporc/china_telecom_supporter)，自己打包就行，具体原理说得很清楚；我开始偷懒，直接使用了[别人打包](https://storage.isteed.cc/Pixel3/crack_ct/Chinese_SIM_Supporter.zip)好的，这里也能找到 Pixel 3 的修补后镜像，版本号一致的话可以直接用，可以免去单独安装 Magisk 这步。多说一句，这个网盘的正主是一个自称[Lufs](https://blog.isteed.cc/)的博主。

3.5 科学上网工具，要么路由器可翻，要么待解锁手机可用的 apk 程序包，不多说。注意一定是可用，能安装不代表能正常运行。

### 四、开始安装

在开始之前，照例还是要强调一遍备份的重要性：尽一切可能，把所有可能的资料，从手机上备份到其他地方。每个人的情况都不尽相同，这里仅说我自己的情况：一般我备份的都是照片、视频，以及各种 app 的 apk 文件。其他比如通讯录等等，要么是通过同步实现，要么本身就是在线的云端存储，或者根本就在其他地方有多个副本，比如铃声等。
再说一遍：一定要完成备份，确认手机里的东西都可以抹掉；因为一旦开始，就没有回头的可能。其实在进行这次操作时，我并没有备份，因为是全新手机。万一变砖，需要的东西也都准备好了？拜托，不要咒我好吗？我哪里那么差的运气，根本就不用准备。好在确实运气好。。。


4.1 解锁手机

首先操作手机： Settings - About phone - Build number ，连击七下，确认打开 Developer options ；然后 Settings - System - Advanced - Developer options ，找到 USB debugging 和 OEM unlocking ，将这两个都打开，关机；
待手机关机之后，将其用数据线与电脑连接，电脑上打开终端或者命令行，输入 fastboot flashing unlock ，但不要点回车，停留在当前窗口；
将手机进入 fastboot 模式（电源键 + 音量减），直到手机亮屏，松开并在电脑上摁下回车键；
待电脑上执行完毕之后，留意手机画面变化，通过音量的+ - 键进行操作选择，选中 Unlock the bootloader ，并确认。
手机会重新启动，所有数据都会被抹掉，就像手机刚刚才被快递员送到手一样。开机后进行相应设置，进入手机。


4.2 刷入 Magisk 

接下来这一段我要信口开河，信马由缰。因为我直接刷入了跟我一样版本号的 boot.img ，重启后就带有 Magisk ，导致我并没有亲自操作刷入 Magisk ，因而只好根据我的直觉，瞎写。其实也算不上瞎写，只是根据 Magisk 官网的步骤，做个搬运。
从官网用手机自带浏览器下载最新版本的 Magisk App 安装包文件，此时最新版本是 Magisk-v23.0.apk ；完成下载后点击文件进行安装，会提醒 For your security, your phone is not allowed to install unknown apps from this source. 点击右边的 SETTINGS ，然后将 Allow from this source 选项按钮启用，完成安装。此时的 Magisk 只是临时的，并不具有 root 权限。

4.3 修补 boot.img 

将从官网下载的系统 boot.img 镜像传输至手机，可以通过 adb 命令，也可以直接复制到手机内。打开 Magisk ，点击右上角的 Install ，然后只能看到一个选项： Select and Patch a File 选择该选项并点击开始，选择刚刚传输至手机的 boot.img ，进行修补。
完成后，将对应的修补后的 img 文件拷贝到电脑上，至此完成 boot.img 的修补。
将手机关机。

4.4 安装 Magisk 

手机通过数据线连接至电脑，打开 adb ，输入 fastboot boot /path/boot.img ，暂时不要点回车将其执行； /path/boot.img 是 boot.img 的完整位置。
手机再次进入 fastboot 模式（电源键 + 音量减），此时将电脑按下回车，执行 fastboot boot 命令，手机会自动开机，偶尔会时间稍长，还请耐心等待。
开机进入系统，打开 Magisk ，再次点击右上角的 Install ，会发现除了刚才的： Select and Patch a File 选项外，还多了一个 Direct Install (Recommended) 选项。
这次我们选择新增的 Direct Install (Recommended) ，然后点击开始，选择已经在手机里的、修补后的 boot.img 文件，完成后会有个蓝色的 Reboot 选项，点击后手机重启。

4.5 刷入 Chinese Sim Supporter

手机完成重新启动，进入系统，打开 Magisk ，可以在 Magisk 的 Home 页面能看到红色的 Uninstall Magisk , 两个 Install 区域都能看到版本号，在页面下方能看到一排四个可点击按钮。
将下载好的 Chinese_SIM_Supporter.zip 拷到手机，点击 Magisk Home 页面下方能看到一排四个可点击按钮中的 Modules ，选择 Install from storage ，找到拷进来的 Chinese_SIM_Supporter.zip ，安装。完成后根据提示将手机重启。
如果重启前已经将中国电信的 4G Sim 卡放到手机卡槽，那么在开机进入系统后，应该是能看到右上角表示 Mobile network 的三角符号是一个完整的实心等腰直角三角形，不再有前面看到的叉叉。

怀着激动的心情，用颤抖的手，触响你最想听到的声音吧。

### 五、总结

中间其实走了弯路，而且 Magisk 已经支持 OTA 升级（即升级前先将 Magisk ，然后进行常规升级操作，但在最后一步的重启之前停下来，把 Magisk 再装回去且一定要选择 Install to Inactive Slot (After OTA) ，但我操作时并未看到，一狠心一跺脚重启了。再进来就看到 Magisk 失去 root 权限。可能是我哪儿操作的不对，等回头再升级时继续研究。
