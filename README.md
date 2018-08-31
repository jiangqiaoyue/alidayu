# alidayu
阿里大鱼发送短信API使用PHP版

短信可以用来发送验证码、订单信息、支付提醒等等，我们选择的是阿里大鱼

## 第一 注册阿里大鱼服务
  
  登录阿里云账号->产品与服务->云通信->短信服务->接口调用->获取AccessKey->点击继续使用AccessKey,获取到你的AccessKeyId 和 AccessKeySecret

## 第二 在控制台完成模板与签名的申请，获得调用接口必备的参数
  
  ### 短信签名
  
  根据用户属性来创建符合自身属性的签名信息。企业用户需要上传相关企业资质证明，个人用户需要上传证明个人身份的证明
  
  短信控制台->签名管理->添加签名   
  
  ### 短信模板
  
  即具体发送的短信内容。

  短信模板可以支持验证码、短信通知、推广短信、国际/港澳台消息四种模式。验证码和短信通知，通过变量替换实现个性短信定制。推广短信不支持在模板中添加变量。

  短信模板需要审核通过后才可以使用。
  
  短信控制台->模板管理->添加模板
  
## 第三 下载PHP版阿里大鱼SDK
  
  [下载地址](https://help.aliyun.com/document_detail/55359.html "阿里大鱼SDK PHP版")
## 第四 把阿里大鱼拷贝到你的第三方类库目录并引入
sdk里有个api_demo目录,里面有个SmsDemo.php，那个就是发短信的demo，傻瓜式的照着那个引入文件，然后把app_key和app_secret改成你自己的，把短信签名和模板code
改成你自己的就可以愉快的玩耍了
