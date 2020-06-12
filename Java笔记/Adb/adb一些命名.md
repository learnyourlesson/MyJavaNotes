关闭某个包的应用程序

adb shell am force-stop com.netease.beaverplan

从手机拷贝文件到电脑（好像不能直接拷到根目录下）

adb pull 手机文件路径  电脑路径

adb操作指定的手机（电脑连接多台手机时）

adb -s 设备ID（adb devices查看）