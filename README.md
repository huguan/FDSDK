# FKSDK
[![License MIT](https://img.shields.io/badge/license-MIT-green.svg?style=flat)](https://raw.githubusercontent.com/seven/HGSDKDemo/master/LICENSE)&nbsp;
互冠聚合SDK

使用
==============
1.将fdsdk.jar包复制到游戏项目对应的目录下。

2.导入库文件、修改manifest<br/>
<img src="https://raw.githubusercontent.com/huguan/FKSDK/master/Snapshots/Androidmanifest.png"><br/>

注意	
==============
为防止横竖屏切换出现重载acvitity而导致短暂黑屏的问题，需要在主acvitity节点设置属性
```
android:configChanges="orientation|keyboardHidden|screenSize" 
```

如果是unity开发的游戏，需要在AndroidManifest.xml 的主activity入口处添加如下meta-data
```
<meta-data android:name="unityplayer.ForwardNativeEventsToDalvik" android:value="true" />
```


API具体应用说明
==============
1.初始化SDK（必须在onCreate里调用）

```java
public void onCreate(Bundle savedInstanceState) 
{
	FDSDK.getInstance().init(MainActivity.this);           
    FDSDK.getInstance().onCreate();
    if(FDSDK.getInstance().getOrientation().equals("landscape"))
    {
    	setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);
    }
}
```

2.登录（必接）
```java
FDUser.getInstance().login();
```

3.切换账号
```java
if (!FDUser.getInstance().isSupport("switchLogin")) {
	return;
}
FDUser.getInstance().switchLogin();
```

4.注销
```java
if (!FDUser.getInstance().isSupport("logout")) {
	return;
}
FDUser.getInstance().logout();
```

5.支付（必接）

`如果游戏需要校验支付结果，可以由游戏服务器生成唯一订单号，复制给extension参数，在调用支付回调地址时，FDSDK服务器会把extension参数原封不动的传回给游戏服务器`

<table>
    <thead>
        <tr>
            <th>字段名称</th>
            <th>类型</th>
            <th>说明</th>
            <th>备注</th> 
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>productId</td>
            <td>String</td>
            <td>商品id</td>
            <td></td>
        </tr>
        <tr>
            <td>productName</td>
            <td>String</td>
            <td>商品名称</td>
            <td></td>
        </tr>
        <tr>
            <td>productDesc</td>
            <td>String</td>
            <td>商品描述</td>
            <td></td>
        </tr>
        <tr>
            <td>price</td>
            <td>int</td>
            <td>单个商品价格</td>
            <td>单位：分</td>
        </tr>
        <tr>
            <td>buyNum</td>
            <td>int</td>
            <td>购买数量</td>
            <td>默认传1即可</td>
        </tr>
        <tr>
            <td>serverId</td>
            <td>String</td>
            <td>服务器id</td>
            <td></td>
        </tr>
        <tr>
            <td>serverName</td>
            <td>String</td>
            <td>服务器名称</td>
            <td></td>
        </tr>
        <tr>
            <td>roleId</td>
            <td>String</td>
            <td>角色id</td>
            <td></td>
        </tr>
        <tr>
            <td>roleName</td>
            <td>String</td>
            <td>角色名称</td>
            <td></td>
        </tr>
        <tr>
            <td>roleLevel</td>
            <td>int</td>
            <td>角色等级</td>
            <td></td>
        </tr>
        <tr>
            <td>payNotifyUrl</td>
            <td>String</td>
            <td>支付回调通知地址</td>
            <td>可不填写。需告知我们配置</td>
        </tr>
        <tr>
            <td>vip</td>
            <td>String</td>
            <td>vip等级</td>
            <td></td>
        </tr>
        <tr>
            <td>extension</td>
            <td>String</td>
            <td>扩展参数</td>
            <td></td>
        </tr>
        <tr>
            <td>union</td>
            <td>String</td>
            <td>公会名称</td>
            <td></td>
        </tr>
    </tbody>
</table>

```java
PayParams params = new PayParams();
params.setBuyNum(1);
params.setCoinNum(100);
params.setExtension(System.currentTimeMillis()+"");
params.setPrice(100);
params.setProductId("1");
params.setProductName("元宝");
params.setProductDesc("购买100元宝");
params.setRoleId("1");
params.setRoleLevel(1);
params.setRoleName("角色名");
params.setServerId("10");
params.setServerName("测试");
params.setVip("vip1");
FDPay.getInstance().orderAndPay(params);
```