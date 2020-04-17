# 获取openid，unionid 
[https://www.jianshu.com/p/d2faa62967cd](https://www.jianshu.com/p/d2faa62967cd)
## 方法一（前端获取）
注意：虽然前端能拿到openid，但是发布上线的时候会无法过审，因为出于安全考虑，前端代码不允许暴露小程序appId和app secret(秘钥)，所以此种方法不可取。

1. 登录凭证校验，通过 wx.login() 接口获得临时登录凭证 code 后传到开发者服务器调用此接口完成登录流程。更多使用方法详见 小程序登录。

2. 接着访问 https://api.weixin.qq.com/sns/jscode2session?

```
wx.login({
    success: function (res) {
        console.log(res)
         if (res.code) {
            console.log('通过login接口的code换取openid');
             wx.request({
               url: 'https://api.weixin.qq.com/sns/jscode2session',
               data: {
                  //填上自己的小程序唯一标识
                 appid: '',
                  //填上自己的小程序的 app secret
                 secret: '',
                 grant_type: 'authorization_code',
                 js_code: res.code
               },
               method: 'GET',
               header: { 'content-type': 'application/json'},
               success: function(openIdRes){
                    console.info("登录成功返回的openId：" + openIdRes.data.openid);
               },
               fail: function(error) {
                   console.info("获取用户openId失败");
                   console.info(error);
               }
            })
          }
        }
    })
```

方法二（后端获取）

前面我们说过前端获取openid的方法，项目上线是无法过审的。现在我们把小程序id和app secret给后台，让后台去请求，然后将返回值通过接口返回给我们，就可以了。另外，我们通过后台接口返回的参数unionid作为用户唯一标识

```
        wx.login({
            success: function (res) {
              console.log(res)
              wx.request({
                url: '后台通过获取前端传的code返回openid的接口地址',
                data: { code: code },
                method: 'POST',
                header: { 'content-type': 'application/json'},
                success: function (res) {
                 if (res.statusCode == 200) {
                   console.log(res.data.result.openid);
                   console.log(res.data.result.unionid);
                 } else {
                   console.log(res.errMsg)
                 }
              },
            })
          }
       })  
```


------
获取appid和appsecret 
[https://www.jianshu.com/p/497378676359](https://www.jianshu.com/p/497378676359)

1. 进入https://mp.weixin.qq.com 登录
2. 左侧菜单选择【开发】
3. 右侧tab选择【开发设置】
4. AppSecret栏右侧点击重置
会弹出一个二维码，需要开发者扫描二维码才可以重置AppSecret
。出现AppSecret后点击复制，并保存你的AppSecret。
在保存好之前不要进行任何操作。你退出了这个界面且没保存AppSecret，就只能再重新生成AppSecret了。
