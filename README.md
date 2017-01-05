# FKSDK
[![License MIT](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://raw.githubusercontent.com/seven/HGSDKDemo/master/LICENSE)&nbsp;
互冠聚合SDK

使用
==============
1.将fdsdk.jar包复制到游戏项目对应的目录下。

2.导入库文件、修改manifest
<img src="https://raw.githubusercontent.com/huguan/FKSDK/master/Snapshots/Androidmanifest.png"><br/>

注意	
==============
为防止横竖屏切换出现重载acvitity而导致短暂黑屏的问题，需要在主acvitity节点设置属性
```
android:configChanges="orientation|keyboardHidden|screenSize" 
```