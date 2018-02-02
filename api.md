
一．账户接入步骤
步骤1：申请接入，获取app_id和app_secret，等待审核；

步骤2：开发应用/网站；

步骤3：根据实际情况选择client或者server。

步骤4：通过用户登录验证和授权，获取Access Token；

步骤5：通过Access Token获取用户的校验信息；

步骤6：调用OpenApi相关接口，来请求访问或修改用户授权的资源。

调用方式
client
client是OAuth2.0认证的一种模式；
适用于需要通过客户端访问的方式，例如需要通过浏览器的javascript代码，或者电脑/移动终端上的客户端访问时。
请求方法： 
https://xmax.io/oauth/authorize
请求参数
参数	必选	字段说明
response_type	是	授权类型，此值固定为“token”。
app_id	是	分配给授权方的的app_id
redirect_uri	是	成功授权后的回调地址，必须是注册client_id时填写的主域名下的地址
scope	是	请求用户授权时向用户显示的可进行授权的列表
state	是	client端的状态值。用于第三方应用防止CSRF攻击，成功授权后回调时会原样带回。
返回说明 
1. 如果用户成功登录并授权，则会跳转到指定的回调地址，并在URL后加“#”号，带上Access Token以及expires_in,app_id等参数。如果请求参数中传入了state，这里会带上原始的state值。如果redirect_uri地址后已经有“#”号，则加“&”号，带上相应的返回参数。 
如：#app_id=xxx123445&access_token=********&expires_in=360000&state=callbackstate
server
构造请求授权地址,获取Authorization Code：
获取code
请求参数:
参数	必须	字段说明
response_type	是	授权类型，此值固定为“code”。
app_id	是	分配的app_id
redirect_uri	是	成功授权后的回调地址，必须是注册client_id时填写的主域名下的地址。
scope	是	请求用户授权时向用户显示的可进行授权的列表。
state	是	client端的状态值。用于第三方应用防止CSRF攻击，成功授权后回调时会原样带回。
示例：
https://xmax.io/oauth/authorize?response_type=code&app_id= [YOUR APPID]&redirect_uri=[YOUR_REDIRECT_URI]&scope=[THE_SCOPE] 
如果用户授权后，则成功跳转到指定的redirect_uri，并跟上Authorization Code。 redirect_url?code=cb42e130d1471239a27fca6228094f0e&state=state1

通过code获取Access Token 请求：POST https://xmax.io/oauth/token 
请求参数:
参数	必须	字段说明
grant_type	是	此处固定为：auth_code
app_id	是	分配的app_id。
app_secret	是	分配的app_secret。
code	是	前一步骤获得code。(注：一个code只可以使用一次对access_token的获取，不管成功与否)
redirect_uri	是	此步骤的redirect_uri一定要和获取code时所传递的redirect_uri保持一致
state	true	client端的状态值。用于第三方应用防止CSRF攻击，成功授权后回调时会原样带回。

返回:
参数	字段说明
access_token	受信访问令牌
token_type	授权类型
refresh_token	刷新token,用于在access_token过期的情况下再取到一个access_token
expires_in	token有效果时长（单位：秒）
scope	token访问能力
open_id	xmax用户对应的唯一ID

校验access_token
GET/POST https://xmax.io/token/check
请求参数：
参数	必须	字段说明
access_token	是	受信访问令牌
返回:
{
  ret_code: 200/500/1001   正确返回/系统内部错误/access_token过期
  ret_msg:
  ret_data:{
       expired_at:access_token的过期时间戳 
       open_id:授权用户对应的唯一key
       scope:权限
       app_id:授权的app_id
}
redirect:保留字段
}



2.账户接入

其他平台或者应用需要在xmax平台上注册，或取应用标识openid以及公私钥。其用户便可以接如到xmax平台。

参数	是否必须	说明
access_token	是	认证信息
appid	是	应用标识
scope	否	授权编号（如果不填则系统会开放默认权限给用户）

返回说明 请求结果将以json的方式返回
{
    "appid": "xmax_xxxxxxxx", 
    "nickname": "张三", 
    "sex": 1, 
    "headurl":"http://xmax.io/698d51a19d8a121ce581499d7b701668.jpg",
    "create_time": 1382694957,
    "uid": "xmax_xxxxxxxx_oqsjkfio"
    "remark": "",
   }


二.投票
  
  
参数	是否必须	说明
nonce_str	是	随机字符串用户签名
sign	是	签名字符串
appid	是	应用标识
uid	是	投票用户
vote_id	是	投票模板id
vote_param	是	投票必要参，数结构为json字符串


返回说明 请求结果将以json的方式返回
{
  err_code:0
  error_msg:”success”
  data：{
    //交易hash
    txhash:0x44a8b80fdf2d27877152c118a2315b9710e8fc74e64f876bfe139714d2bbb5d4
}
}








三.竞猜
参数	是否必须	说明
nonce_str	是	随机字符串用户签名
sign	是	签名字符串
appid	是	应用标识
uid	是	用户
guess_id	是	竞猜模板id
guess_param	是	竞猜必要参数，结构为json字符串
redirect_uri	否	竞猜结束后通知接入应用结果

返回说明 请求结果将以json的方式返回
{
  err_code:0
  error_msg:”success”
  data：{
    //交易hash
    txhash:0x44a8b80fdf2d27877152c118a2315b9710e8fc74e64f876bfe139714d2bbb5d4
}
}

