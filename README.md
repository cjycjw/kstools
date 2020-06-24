#Android中自动爆破签名工具
<br>
##在使用该工具之前，一定要配置好JAVA_HOME和aapt工具目录！
##将需要操作的apk文件放在当前目录下，默认名称是src.apk
##如果要修改apk名称可以去kstools.bat中进行修改

<br>
#注意：

##第一种加固apk：如果是加固的apk,在脱壳之后进行修复之后的apk
##需要先将加固前的apk放到目录下，然后直接拖apk文件到apksign.bat中运行获取签名信息，运行结束之后保存在apksign.txt中;然后将修复之后的apk文件命名为src.apk，放在当前目录下，直接运行kstools.bat即可
##对于加固app有很多特殊情况，所以如果操作失败，可以自行编写代码获取加固app的签名信息，方法很多，自行网上搜索。
<br>
##第二种非加固apk：如果是非加固的app,直接将apk拷贝到当前目录下，命名为src.apk，直接运行kstools.bat即可，<font color='#FF00FF'>如果目录下还存在apksign.txt文件，需要手动删除该文件。以免使用错误签名!</font>

<br>
#操作签名失败
##在操作的过程中如果发现还是失败，第一反应先确定是否是签名获取错误，导致最终的hook失败。可以自行验证签名信息是否正确。

<br>
#作者：尼古拉斯.赵四(四哥)
##在使用过程中有任何问题，请联系我，不了解原理的同学可以查看文章说明:<a href="http://www.wjdiankong.cn">点击查看<a>
  
  
 #mac下运行
 作者只提供了windows下运行的bat批处理脚本，另外在内部java代码中也是通过执行shell命令（aapt, jarsigner, baksmali, smali）等来完成相关的工作，打开对应的bat脚本文件，可以看到执行的就是java命令行， 把java命令行直接拷贝出来在mac终端运行，会有各种错误，原因就是环境不同。但可以通过构造对应环境达到可执行的目的，具体如下：
 export JAVA_HOME=/usr/    内部java代码中会判断JAVA_HOME环境变量是否设置，没有设置直接退出(同时会附加/bin/java 在运行java程序等)
cp /usr/bin/jarsigner /usr/local/bin/jarsigner.exe   //伪造一个jarsigner.exe，因为java内部会执行这个命令程序进行签名等
chmod 777 /usr/local/bin/jarsigner.exe

然后就是把bat中对应的java命令拷贝出来，并把对应的参数等替换为争取的参数，如下：
cjydeMacBook-Pro% java -Xmx2048m -XX:-UseParallelGC -XX:MinHeapFreeRatio=15 -jar kstools.jar ++hook ./ src.apk '/Users/jay/Library/Android/sdk/build-tools/29.0.2/aapt' 1338303158
Unable to find a $JAVA_HOME at "/usr/", continuing with system-provided Java...
配置了签名值,开始读取进行转化...
获取签名配置信息成功：3082023d308201a6a00302010202044a7ef715300d06092a864886f70d01010505003063310b300906035504061302636e3111300f060355040813085368616e676861693111300f060355040713085368616e67686169310f300d060355040a1306436f6f54656b310f300d060355040b1306436f6f54656b310c300a060355040313034a696d301e170d3039303830393136313933335a170d3336313232353136313933335a3063310b300906035504061302636e3111300f060355040813085368616e676861693111300f060355040713085368616e67686169310f300d060355040a1306436f6f54656b310f300d060355040b1306436f6f54656b310c300a060355040313034a696d30819f300d06092a864886f70d010101050003818d00308189028181008ba744a30e426aa78b4ea7152f4f0c8e68ad7f415934a44da95fcdc1890e2162f0409c3dd9bc9230257c1c61a4ab49bda68dfd469e3508566e84a96e98cd1fbe359d483d5bfda4d3baed4c140450fffa9302eaf64965aa862971a456474f9f440a168aee7b592bcf179fd84a58e8ef32595d40e3697865642464858eddaff18f0203010001300d06092a864886f70d0101050500038181004bac5e473d001c0b9970603bf3192298c86af7226a439da77ca96792b536236c037323bf0d632cbe8041309add374f15c61692739026ce00b1d44854ad0ffffa610097b33275e0307e0a1f86901ca92be1cd6987c0afbd0c8a40c62f4f69b379b5b02f6536e3c1893435292c1a078804f8e6b047c23e5f0b5cd86b2cab065c59
第二步==> 获取apk文件入口信息
应用入口类:com.cootek.smartdialer.NovelApplication
获取apk入口类信息成功===耗时:0s


第三步==> 解压apk文件:/Users/jay/Downloads/kstools-master/src.apk
解压apk文件结束===耗时:0s


第四步==> 删除签名文件
删除签名文件命令:/Users/jay/Library/Android/sdk/build-tools/29.0.2/aapt remove /Users/jay/Downloads/kstools-master/unsigned.apk META-INF/MANIFEST.MF META-INF/CERT.SF META-INF/CERT.RSA
删除签名文件结束===耗时:0s


第五步==> 将dex转化成smali
dex转化smali成功===耗时:4s


第六步==> 代码中替换原始签名和包名信息
设置签名和包名成功===耗时:0s


第七步==> 添加hook代码
插入hook代码成功===耗时0s


第八步==> 将smali转化成dex
smali转化dex成功===耗时:9s


第九步==> 将dex文件添加到源apk中
cmd:/Users/jay/Library/Android/sdk/build-tools/29.0.2/aapt remove /Users/jay/Downloads/kstools-master/unsigned.apk classes.dex
cmd:/Users/jay/Library/Android/sdk/build-tools/29.0.2/aapt add /Users/jay/Downloads/kstools-master/unsigned.apk classes.dex
 'classes.dex'...
添加dex文件到apk中结束===耗时:1s


第十步==> 开始签名apk文件:unsigned.apk
cmd error:java.io.IOException: Cannot run program "jarsigner.exe": error=2, No such file or directory
签名apk文件结束===耗时:0s




