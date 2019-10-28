# 				摩多cp通用sdk对接文档V1.1.4

## 客户端

### 1、引入js

​	客户端引地址入https://sdk.modo.cn/h5/v2.0.1/modosdk.js

​	所有操作均以modosdk对象进行操作；

备注：

非摩多内部游戏native（native只支持egret引擎）、或者webview（不限制）、需要引入额外一个封装接口js

​	https://sdk.modo.cn/native/v1.0.5/native.js  内部封装对象为mo.NT 需要在modosdk.js之前引入；

### 2、初始化接入

备注：客户端所有的接口必须以初始化完成才能进行下一步操作；

接口示例如下：

let obj:any = {};
obj.gameId = "1"//适用于微信小游戏gameid  非必须    游戏id
modosdk.init(obj,function (ret) {

​     	 if (ret.status == 1){

​       		//初始化成功、代表引入js成功、sdk的所有接口可以调用到

​      	 }

​    },this);

获取初始化参数

modosdk.getInitData(): IInitData获取初始化参数;同步接口 返回的对象如下所示

| 参数名    | 类型   | 是否必有 | 说明                                                    |
| --------- | ------ | -------- | ------------------------------------------------------- |
| gameid    | string | 是       | sdk分配给游戏的id                                       |
| channelid | string | 是       | sdk分配给平台的id                                       |
| maintain  | number | 否       | sdk是否为维护状态0和没有该字段为非维护状态，1位维护状态 |

### 3、游戏资源加载接入

下面方法都为mo.NT对象的function

备注：游戏执行流程 doJsReady(mo.NT)、init(modosdk)、runProgress(mo.NT)、login(modosdk)、closeLoading(mo.NT)

1、doJsReady();该接口表示js已经准备好开始加载js资源；

2、runProgress(opt: IProgressInfo);

IProgressInfo:对象说明

| 参数    | 类型   | 是否必传 | 说明                                                         |
| ------- | ------ | -------- | ------------------------------------------------------------ |
| tipText | string | 是       | 是指一些小文字提示标签，例如：资源加载中，请耐心等待...      |
| proText | string | 是       | proText是指progressText，就是加载进度的一些提示，但这个值可能为null，现在的项目都是为null的 |
| total   | number | 是       | 是指总共的进度需要多少                                       |
| cur     | number | 是       | 是指当前加载到了多少了                                       |

3、closeLoading();关闭加载页面；

4、reloadGame();重新加载游戏；

5、exitGame();退出游戏；

### 3、开关性质接口接入

判定sdk所有异步接口是否已经准备完毕、开关性质大部分为同步接口、部分用异步接口

modosdk.onHasReady(function(ret){

​		if (ret.status == 1){

​       		//1代表所有开关判定接口准备完毕

​      	 }

},this);

所有开关的同步接口返回均为boolean;

1、是否有客服系统 has_customerService()；
2、是否有分享功能 has_share()；
3、是否有关注功能has_subscribe();
4、是否有收藏功能has_favorite();
5、是否有绑定手机功能has_bindPhone();
6、是否有开始实名认证has_certification();
7、 是否有余额功能has_balance();
8、 是否有客服系统has_custService();（适用于h5）


### 4、sdk用户登录

cp调用sdk登录接口，登录接口会返回sdk的用户token，cp获取用户token后用于验证服务端登录

​	modosdk.login({},function (ret) {

​      	 if(ret.status == 1){

​				// ret.token 获取token

​			}

   },this);

### 5、前端统计接口

1、游戏账号注册

let obj:any = {};

obj.accountid :number;

modosdk.accountCreate(obj);

2、游戏账号创建

let obj:any = {};

obj.accountid:number;

modosdk.accountLogin(obj);

3、游戏角色创建

let obj:any = {};

obj.serverid:number;

obj.serverindex:number;

obj.servername:string;

obj. rolename:string;

obj.roleid:number;

obj.accountid:number;    

obj.createTime:Date;

modosdk. roleCreate ( obj);

4、游戏角色登录

var obj = {};

obj.serverid:number;

obj.serverindex:number;

obj.servername:string;

obj.rolename:string;

obj.roleid:number;

obj.accountid:number;

obj.rolelevel:number;

obj.vip:number;

obj.createTime:Date

modosdk. roleLogin( obj);

5、角色升级

var obj = {};

obj.level:number;

modosdk.setRoleLevel(obj);

6、游戏事件上报

eventName的事件值：

1)、新手引导结束：guideComplete

2)、开始游戏界面：indexEnter

3)、点击选区按钮：clickServerBtn

4)、服务器列表界面：serverList

5)、展示创角界面：indexCreate

6)、点击创角按钮：clickCreateRole

7)、进入游戏主界面：enterGame

var eventName:string;

modosdk. tagEvent( eventName);

### 6、用户行为接口

打开客服系统

let obj:any = {};

modosdk.openCustomerService(obj,function(rs){

​		if(rs.status == 1){

​				//成功唤起客服

​			}

​	},this);

打开客服系统（适用于h5）

let obj:any = {};
	obj.isVip:boolean // 是否是vip
modosdk.openCustService(obj,function(rs){

​		if(rs.status == 1){

​				//成功唤起客服

​			}

​	},this);

### 7、sdk用户支付

cp调用sdk实现用户支付

let  obj = {};

   	obj.id = 1;   //道具ID，和后台配置一致

  	 obj.serverid = '1';    //服务器ID

   	obj.servername = 'server name'; //服务器名，不超过32个字

​		obj.orderno = '20122fsfsf'; //cp订单号，不超过32个字

   	obj.ext = ''; //额外参数，字符串，不超过128个字
   	obj.useBalance = '';//是否使用余额支付，如果不支持余额支付，则自动使用真实支付   非必须  余额比例为1:1

   	modosdk.pay(obj,function (rs) {

​      	 if (rs.status == 1){

​           	//充值成功

​      	 }else{

​           	//充值失败

​      	 }

   },this);

### 8、分享
分享功能
let obj:any = {};
  obj.ext = {};// key=>value形式拼接到query
modosdk.share({},function (rs) {

​      	 if (rs.status == 1){

​           	//分享成功

​      	 }else{

​           	//分享失败

​      	 }
   },this);

### 9、触发分享

触发分享功能
let  obj = {};
   obj.title = "传入标题";//传入标题
   obj.desc = "传入描述";//传入描述
   obj.img = "";//传入图片地址
   obj.ext = {};// key=>value形式拼接到query
modosdk.triggerSharing(obj,function (rs) {

​      	 if (rs.status == 1){

​           	//分享成功

​      	 }else{

​           	//分享失败

​           }

  },this);

### 10、邀请功能

邀请
modosdk.invite({},function (rs) {

​      	 if (rs.status == 1){

​           	//邀请成功

​      	 }else{

​           	//邀请失败

​           }

  },this);

### 11、收藏

收藏功能
modosdk.favorite({},function (rs) {

​      	 if (rs.status == 1){

​           	//收藏成功

​      	 }else{

​           	//收藏失败

​      	 }
   },this);

### 12、查询是否关注


modosdk.subscribe({},function (rs) {

​      	 if (rs.status == 0){

​           	//未关注

​      	 }else{

​           	//status == 2 不需要关注

​      	 }
   },this);

### 13、实名认证

实名验证：传入拥有show属性窗口对象，该窗口对象show属性方法的回调中有onOk 、onCancel两个方法。在show的回调方法中，当用户点击确定会调用onOk方法后,将认证信息传入并获得是否成功的回调,成功则会返回成功并执行close方法关闭认证窗口，失败则不会执行close方法且返回失败，当用户点击取消会调用onCannel方法并执行close方法关闭认证窗口。关闭窗口操作需游戏方在close方法中执行。

var onOk;
var onCancel;
var ICreator_CertificationDlg = {//窗口对象
​    show:function (cb) {
​        console.log("创建ICreator_CertificationDlg");
​        onOk = cb.onOk;//点击onOkClick方法
​        onCancel = cb.onCancel;//点击onCancelClick方法
​        return {
​            close:function () {//关闭实名认证窗口
​            	console.log("关闭ICreator_CertificationDlg");
​            }
​        }
​    }
};
var onOkClick = function () {
​    console.log("onOk点击执行");
​    onOk({"name":"张三","identity_card_id":"123123",cb:function (rs) {
​    	console.log("验证结果",rs);
​    }});
};
var onCancelClick = function () {
​   console.log("onCancel点击执行");
​    onCancel();
};
modosdk.registerCertificationDlg(ICreator_CertificationDlg);//传入认证窗口对象
modosdk.certification({},function (rs) {
​      	 if (rs.status == 1){
​           	//方法初始化成功后，点击绑定onOkClick方法即可发送实名认证信息，点击取消这则​                     关闭实名认证窗口
​      	 }else{
​           	//方法初始化失败
​      	 }
   },this);

### 14、手机号绑定

手机号绑定：传入拥有show属性窗口对象，该窗口对象show属性方法的回调中有send 、bind和cancel三个方法。在show的回调方法中，当用户点击发送验证码调用send方法后,回调成功则做为参数传入的手机号会接收到验证码。用户输入验证码并点击确定调用bind方法后，回调验证成功会执行close方法进行关闭窗口，反之，失败则不会。当用户点击取消调用cannel方法并执行close方法关闭认证窗口。关闭窗口操作需游戏方在close方法中执行。

var send;
var bind;
var cancel;
var ICreator_bindPhoneDlg = {//窗口对象
​    show:function (cb) {
​        console.log("创建ICreator_bindPhoneDlg");
​        send = cb.send;//点击sendClick方法
​        bind  = cb.bind;//点击bindClick方法
​        cancel = cb.cancel;//点击cancelClick方法
​        return {
​            close:function () {//关闭手机号绑定窗口
​            	console.log("关闭ICreator_bindPhoneDlg");
​            }
​        }
​    }
};
var sendClick = function () {
​    console.log("send点击执行");
​    send({"phoneNumber":"15000000000",cb:function (rs) {//发送验证码
​    	console.log("发送验证码结果回调",rs);
​    }});
};
var bindClick = function () {
​   console.log("bind点击执行");
​    bind({"verificationCode":"999999",cb:function (rs) {//验证验证码
​    	console.log("验证验证码是否正确回调",rs);
​    }});
};
var cancelClick = function () {
​   console.log("cancel点击执行");
​    cancel();//取消绑定手机号
};
modosdk.registerBindPhoneDlg(ICreator_bindPhoneDlg);//传入手机号绑定窗口对象
modosdk.bindPhone({},function (rs) {
​      	 if (rs.status == 1){
​           	//方法初始化成功后，点击绑定sendClick方法按钮即可发送验证码，点击绑定             bindClick方法按钮即可验证验证码是否正确，点击绑定cancelClick方法按钮即可取消验证
​      	 }else{
​           	//方法初始化失败
​      	 }
   },this);

### 15、查询余额（微信小游戏）

获取游戏币余额。开通了虚拟支付的小游戏，可以通过本接口查看某个用户的游戏币余额
modosdk.query_balance({},function (rs) {
​      	 if (rs.status == 1){
​                   //查询成功  rs.balance 游戏币余额
​      	 }else{
​           	//查询失败
​      	 }
   },this);

### 16、复制剪贴板

复制到剪切板
let obj:any = {};
obj.data = ""//string类型 需要复制到剪切板的内容
modosdk.setClipboardData(obj,function (rs) {
​      	 if (rs.status == 1){
​                   //复制到剪切板成功
​      	 }else{
​           	//复制到剪切板失败
​      	 }
   },this); 

### 17、查询绑定手机号的结果

modosdk.query_bindPhone（{},function (rs) {
​      	 if (rs.status == 1){
​                   //rs.phoneNumber 玩家手机号
​                   //已绑定手机号
​      	 }else{
​           	//未绑定手机号
​      	 }
   },this);

### 18、查询实名认证结果

modosdk.query_certification（{},function (rs) {
​      	 if (rs.status == 1){
​                   //已实名认证
​      	 }else{
​           	//未实名认证
​      	 }
   },this);

## 服务端

### 1、验证用户token，获取用户信息



请求的方式：post/get

请求路由：query/user_info

线上正式域名：https://api.modo.cn/        http://api.modo.cn

| 参数     | 类型   | 是否必定传 | 说明                         |
| -------- | ------ | ---------- | ---------------------------- |
| token    | string | 是         | 前端获取的用户token          |
| new_time | string | 是         | 秒级时间戳                   |
| key      | string | 否         | 登录秘钥不参传输，只参与签名 |
| game_id  | number | 是         | sdk分配给cp的游戏id          |
| sign     | string | 是         | 签名                         |

sign生成方式：

MD5(token= token &new_time= new_time&key= key &game_id= game_id);

签名示例：

MD5前：

token=58460ab8d70217fky32fjtghb3zrngys&new_time=1481007107&key=eYFcxx6tJrenDCmr&game_id=1 

MD5后：

sign   1d38a2d492e9f9a1259d61e139477cc3

CP拉起用户信息,失败返回JSON

{

​	"status":"fail",

​	"msg":"fail message"

}

成功返回JSON

{

​	“status”:"success",

​	"data":{

​		"channel_id":渠道id,

​		"name":用户名,

​        "img":"用户头像",

​		"open_id":"sdk用户的唯一标识"
​               "register_time":"玩家注册时间"

​	}

}

### 2、验证支付流程发放道具

请求的方式：post

备注：需要提供给sdk一个支付回调通知地址

| 参数         | 类型   | 是否必传 | 说明                |
| ------------ | ------ | -------- | ------------------- |
| channel_id   | number | 是       | sdk的平台id         |
| ext          | string | 否       | cp附带参数          |
| fee          | floalt | 是       | 金额                |
| game_id      | number | 是       | sdk提供给cp的游戏id |
| key          | string | 否       | 支付秘钥            |
| new_time     | number | 是       | 时间戳              |
| open_id      | string | 是       | 用户的唯一open_id   |
| out_trade_no | string | 是       | sdk订单号           |
| trade_no     | string | 是       | cp订单号            |
| sign         | string | 是       | 签名                |

sign生成方式：

MD5(channel_id= channel_id&fee= fee &game_id= game_id &key=key&new_time= new_time &open_id= open_id &out_trade_no= out_trade_no &trade_no= trade_no);

签名示例：

MD5前：

channel_id=1 &fee=1&game_id=gameidstring&key=daa3352db58a0d&new_time=1481009400&open_id=openidstring&out_trade_no=20123123123&trade_no=20123123123 

MD5后：

sign   
 4b9b9f9cec144237f858a639037de3ec



失败返回JSON

{

​	"status":"fail",

​	"msg":"failmessage"

}

成功返回

{

​	"status":"success",

​	"msg":"推送成功"

}

### 3、服务端上报数据接口

###### 1、账号创建

```
接口路由 /report/accountCreate   请求方式post/get
```

| 参数        | 类型   | 是否必传 | 说明                 |
| ----------- | ------ | -------- | -------------------- |
| mdgid       | number | 是       | 游戏id               |
| mdcl        | number | 是       | 渠道id               |
| open_id     | number | 是       | sdk用户唯一用户id    |
| account_id  | number | 是       | cp账号id             |
| create_time | number | 是       | 创建时间，秒级时间戳 |

###### 2、账号登录

```
接口路由 /report/accountLogin	请求方式 post/get
```

| 参数       | 类型   | 是否必传 | 说明                 |
| ---------- | ------ | -------- | -------------------- |
| mdgid      | number | 是       | 游戏id               |
| mdcl       | number | 是       | 渠道id               |
| open_id    | number | 是       | sdk用户唯一用户id    |
| account_id | number | 是       | cp账号id             |
| login_time | number | 是       | 登录时间，秒级时间戳 |



###### 3、角色创建

```
接口路由 /report/roleCreate		请求方式 post/get
```

| 参数         | 类型   | 是否必传 | 说明                       |
| ------------ | ------ | -------- | -------------------------- |
| mdgid        | number | 是       | 游戏id                     |
| mdcl         | number | 是       | 渠道id                     |
| open_id      | number | 是       | sdk用户唯一用户id          |
| account_id   | number | 是       | cp账号id                   |
| role_id      | number | 是       | cp角色id                   |
| role_name    | string | 是       | cp角色昵称                 |
| server_id    | number | 是       | 区服id（合服会变）         |
| server_index | number | 是       | 区服唯一标识（合服不会变） |
| create_time  | number | 是       | 创建时间                   |



###### 4、角色登录

```
接口路由  /report/roleLogin		请求方式 post/get
```

| 参数         | 类型   | 是否必传 | 说明                       |
| ------------ | ------ | -------- | -------------------------- |
| mdgid        | number | 是       | 游戏id                     |
| mdcl         | number | 是       | 渠道id                     |
| open_id      | number | 是       | sdk用户唯一用户id          |
| account_id   | number | 是       | cp账号id                   |
| role_id      | number | 是       | cp角色id                   |
| role_name    | string | 是       | cp角色昵称                 |
| server_id    | number | 是       | 区服id（合服会变）         |
| server_index | number | 是       | 区服唯一标识（合服不会变） |
| login_time   | number | 是       | 登录时间                   |

###### 5、发货通知

```
接口路由 /report/orderNotify	请求方式 post/get
```

| 参数         | 类型   | 是否必传 | 说明                       |
| ------------ | ------ | -------- | -------------------------- |
| mdgid        | number | 是       | 游戏id                     |
| mdcl         | number | 是       | 渠道id                     |
| open_id      | number | 是       | sdk用户唯一用户id          |
| subject_id   | number | 是       | 充值项id                   |
| sdk_order_no | string | 是       | sdk订单id                  |
| game_order_no| string | 是       | 游戏订单号id               |
| account_id   | string | 是       | cp账号id                   |
| role_id      | number | 是       | cp角色id                   |
| role_name    | string | 是       | cp角色名字                 |
| server_id    | number | 是       | cp区服id                   |
| server_index | number | 是       | 区服index、唯一标识        |
| create_time  | number | 是       | 订单创建时间（秒级时间戳） |
| success_time | number | 是       | 订单成功时间（秒级时间戳） |

###### 6、微信的文本信息安全检查接口（微信小游戏接口，建议提审服使用，微信接口有使用次数限制）

```
接口路由 /game/msgSecCheck/mdcl/cXXX(渠道id)/mdgid/XX(游戏id)/ 	请求方式 post/get
```

| 参数         | 类型   | 是否必传 | 说明                       |
| ------------ | ------ | -------- | -------------------------- |
| content      | string | 是       | 检查文本内容                 |

失败返回JSON

{

​	"status":"fail",

}

成功返回

{

​	"status":"success",

​	"result":"请求微信结果"

}

###### 7、微信的图片信息安全检查接口（微信小游戏接口，微信接口有使用次数限制）

```
接口路由 /game/imgSecCheck/mdcl/cXXX(渠道id)/mdgid/XX(游戏id)/ 	请求方式 post/get
```

| 参数 | 类型   | 是否必传 | 说明     |
| ---- | ------ | -------- | -------- |
| media_url  | string | 是       | 图片或音频地址 |
| media_type  | number | 是       | 1:音频;2:图片|

失败返回JSON

{

​	"status":"fail",

}

成功返回

{

​	"status":"success",

​	"result":"请求微信结果"

}

###### 8、微信的音频或图片信息安全检查接口（微信小游戏异步接口，微信接口有使用次数限制）

```
接口路由 /game/mediaCheckAsync/mdcl/cXXX(渠道id)/mdgid/XX(游戏id)/ 	请求方式 post/get
```

| 参数 | 类型   | 是否必传 | 说明     |
| ---- | ------ | -------- | -------- |
| url  | string | 是       | 图片地址 |

失败返回JSON

{

​	"status":"fail",

}

成功返回

{

​	"status":"success",

​	"result":"请求微信结果"

}

### 4.官网直充接口说明（由游戏方提供）

###### 1、查询用户

```
	请求方式 post/get
```
| 参数         | 类型   | 是否必传 | 说明                       |
| ------------ | ------ | -------- | -------------------------- |
| userId      | number | 是       | 游戏内玩家id                 |
| ms          | number | 是       | 请求发起的时间戳（秒级别）      |
| sign        | string | 是       | 签名                     |

sign生成方式：

MD5(game_id= game_id&key=key&ms= ms &userId= userId );

签名示例：

MD5前：

game_id=gameidstring&key=daa3352db58a0d&ms=1481009400&userId=11111

MD5后：

sign   
  59fb77d3cac7410b9d549ba19bcf3aca

失败返回JSON

{

​	"status":"0",

​	"msg":"failmessage"

​     "ms":"" //number 请求发起的时间戳（秒级别）
}

成功返回

{

​	"status":"1",

​	"data":{
​                   "openId":"",          //string     平台账号openId
​                   "accountId":"",     //number 游戏账号id
​                   "userId":"",           //number 游戏分服用户id
​                   "userName":"",    //string     游戏分服用户名称
​                   "serverId":"",        //number 游戏分服服务器id
​                   "serverIndex":"", //number 游戏分服用户所在服务器下标
​                   "serverName":"", //string     游戏分服名称
​                   "lvl":"",                  //number 玩家当前角色等级
​                   "vip":"",                 //number  玩家当前vip等级
​                   "isIOS":"",             //number  是否是ios，如果传1就表示是，0表示否
​       }
​    "ms":"" //number 请求发起的时间戳（秒级别）
}

###### 2、获取充值页面信息

```
	请求方式 post/get
```
| 参数         | 类型   | 是否必传 | 说明                       |
| ------------ | ------ | -------- | -------------------------- |
| userId       | number | 是       | 游戏内玩家id                 |
| ms           | number | 是       | 请求发起的时间戳（秒级别）      |
| sign         | string | 是       | 签名                     |

sign生成方式：

MD5(game_id= game_id &key=key&ms= ms &userId= userId );

签名示例：

MD5前：

game_id=gameidstring&key=daa3352db58a0d&ms=1481009400&userId=11111

MD5后：

sign   
  59fb77d3cac7410b9d549ba19bcf3aca

失败返回JSON

{

​	"status":"0",

​	"msg":"failmessage"

​       "ms":"" //number 请求发起的时间戳（秒级别）
}

成功返回

{

​	"status":"1",

​	"data":{
​	    "descInfo":DescInfo, // 非必须  描述项
​		"tabs":[{
​		    "descInfo":DescInfo,         //     描述项
​            "type":"",//number 类型，0表示"充值"，1表示"特权卡"，2表示"活动"
​            "groups":[{
​                "descInfo":DescInfo,// 描述项
​                "list":[{
​                    "descInfo":DescInfo,//描述项      
​                    "rechargeId":"",//number 充值项id
​                    "type":"",//number 充值类型
​                    "value":"",//number|string 非必须 充值类型对应的值
​                },...],
​                "descList":[{DescInfo},...],// 非必须 描述列表
​            },...],// 官网直充数据组列表
​        },...]
   }
​    "ms":"" //number 请求发起的时间戳（秒级别）
}

"DescInfo":{
    "name":"",         //string 非必须 描述项名称
    "desc":"",          //string 非必须 描述内容，平台需要支持ubb的富文本显示模式，例如设置字号、颜色等。 
    "img":"",          //string 非必须 图片 
    "disabled":"",     //number 非必须 1表示禁用，目前用于按钮的禁用状态
    "msg_disabled":"",//string 非必须 当按钮处于禁用状态的时候，点击的反馈信息
}

###### 3、拉起充值请求

```
	请求方式 post/get
```
| 参数         | 类型   | 是否必传 | 说明                       |
| ------------ | ------ | -------- | -------------------------- |
| itemStr      | string | 是       | 充值内容项信息                |
| ms           | number | 是       | 请求发起的时间戳（秒级别）      |
| sign         | string | 是       | 签名                        |
| userId       | number | 是       | 游戏内玩家id                 |

"itemStr":{
    "rechargeId":"",    //number 充值项id
    "type":"",          //number 类型，0表示"充值"，1表示"特权卡"，2表示"活动"
    "value":"",         //number|string 非必须 充值类型对应的值
}

sign生成方式：

MD5(game_id= game_id&itemStr=itemStr&key=key&ms=ms&userId=userId);

签名示例：

MD5前：

game_id=gameidstring&itemStr=itemStrString&key=daa3352db58a0d&ms=1481009400&userId= 1111

MD5后：

sign   
   f05681e60a7ec0819fb6490726909261

失败返回JSON

{

​	"status":"0",

​	"msg":"failmessage"

​    "ms":"" //number 请求发起的时间戳（秒级别）
}

成功返回

{

​	"status":"1",

​	"data":{
​		"orderNo":"", //string 游戏方订单号
​		"ext":"",     //string 游戏方拓展参数
​       },
​    "ms":"" //number 请求发起的时间戳（秒级别）
}