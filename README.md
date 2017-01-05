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

参数说明
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
            <td>ratio</td>
            <td>int</td>
            <td>兑换比例</td>
            <td></td>
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

代码示例
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

5.提交扩展数据（必接）

用户扩展数据，已经登录的角色相关数据，有的渠道需要统计角色相关数据

参数说明


DataType 参数类型

<table>
    <thead>
        <tr>
            <th>类型</th>
            <th>说明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>TYPE_CREATE_ROLE = 2</td>
            <td>创建角色</td>
        </tr>
        <tr>
            <td>TYPE_ENTER_GAME = 3</td>
            <td>进入游戏</td>
        </tr>
        <tr>
            <td>TYPE_LEVEL_UP = 4</td>
            <td>等级提升</td>
        </tr>
    </tbody>
</table>

扩展参数 extraData 说明

<table>
    <thead>
        <tr>
        	<th>参数</th>
            <th>类型</th>
            <th>说明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>dataType</td>
            <td>DataType枚举</td>
            <td>数据类型，当前处于阶段</td>
        </tr>
        <tr>
            <td>roleID</td>
            <td>int</td>
            <td>角色id</td>
        </tr>
        <tr>
            <td>roleName</td>
            <td>String</td>
            <td>角色名称</td>
        </tr>
        <tr>
            <td>roleLevel</td>
            <td>String</td>
            <td>角色等级</td>
        </tr>
        <tr>
            <td>serverID</td>
            <td>int</td>
            <td>服务器id</td>
        </tr>
        <tr>
            <td>serverName</td>
            <td>String</td>
            <td>服务器名称</td>
        </tr>
        <tr>
            <td>moneyNum</td>
            <td>int</td>
            <td>余额</td>
        </tr>
        <tr>
            <td>vipLv</td>
            <td>int</td>
            <td>vip等级</td>
        </tr>
         <tr>
            <td>unionName</td>
            <td>String</td>
            <td>公会名称</td>
        </tr>
    </tbody>
</table>

代码示例
```java
if(FDUser.getInstance().isSupport("submitExtraData")) {
	UserExtraData params = new UserExtraData();
	params.setRoleID("12345");
	params.setDataType(1);
	params.setMoneyNum(1);
	params.setRoleLevel("11");
	params.setRoleName("角色名");
	params.setServerID(1);
	params.setServerName("服务器名称");
	params.setUnionName("公会名称");
	params.setVipLv(10);
	params.setCreateTime(123456789); //角色创建时间 	
	FDUser.getInstance().submitExtraData(params); 
}
```

6.退出SDK（必接）

代码示例
```java
if (!FDUser.getInstance().isSupport("exit")) 
{
	//某些sdk没有指定退出逻辑，可以在这里调用自己的退出逻辑
	return;
}
FDUser.getInstance().exit();
```

7.添加生命周期方法（必接）

代码示例

```java
public void onActivityResult(int requestCode, int resultCode, Intent data){
	FDSDK.getInstance().onActivityResult(requestCode, resultCode, data);
	super.onActivityResult(requestCode, resultCode, data);
}
	
public void onStart(){
	FDSDK.getInstance().onStart();
	super.onStart();
}

public void onPause(){
	FDSDK.getInstance().onPause();
	super.onPause();
}
public void onResume(){
	FDSDK.getInstance().onResume();
	super.onResume();
}
public void onNewIntent(Intent newIntent){
	FDSDK.getInstance().onNewIntent(newIntent);
	super.onNewIntent(newIntent);
}
public void onStop(){
	FDSDK.getInstance().onStop();
	super.onStop();
}
public void onDestroy(){
	FDSDK.getInstance().onDestroy();
	super.onDestroy();
}
public void onRestart(){
	FDSDK.getInstance().onRestart();
	super.onRestart();
}

public void onConfigurationChanged(Configuration newConfig){
	super.onConfigurationChanged(newConfig);
}

public boolean dispatchKeyEvent(KeyEvent event) {
	FDSDK.getInstance().onBackPressed();
	return super.dispatchKeyEvent(event);
}

//如果是unity开发的游戏，需要把onBackPressed和onKeyDown注释掉
public void onBackPressed()
{
	FDSDK.getInstance().onBackPressed();
	super.onBackPressed();
	if(FDUser.getInstance().isSupport("exit"))
	{
		FDUser.getInstance().exit();
	}
	else
	{
		this.finish();
		System.exit(0);
	}
}

public boolean onKeyDown(int keyCode, KeyEvent event)
{
	if(keyCode == KeyEvent.KEYCODE_BACK){
		if(FDUser.getInstance().isSupport("exit"))
		{
			FDUser.getInstance().exit();
		}
		else
		{
			this.finish();
			System.exit(0);
		}
	}
	return true;
}
```