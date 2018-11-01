# alidayu
阿里大鱼发送短信PHP版

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

```php
<?php
namespace app\index\controller;
use think\Session;
use think\Request;
use think\Db;
use think\Cache;
use think\cache\driver\Redis;

require_once EXTEND_PATH . 'AliDayu/api_sdk/vendor/autoload.php';

use Aliyun\Core\Config;
use Aliyun\Core\Profile\DefaultProfile;
use Aliyun\Core\DefaultAcsClient;
use Aliyun\Api\Sms\Request\V20170525\SendSmsRequest;
use Aliyun\Api\Sms\Request\V20170525\SendBatchSmsRequest;
use Aliyun\Api\Sms\Request\V20170525\QuerySendDetailsRequest;

// 加载区域结点配置
Config::load();

class Index
{
    $phone = '13333333333';
    $code = mt_rand(111111,999999);
    
    //发送短信验证码
    $msgRes = $this->sendSms($phone,$code);
    	
   	static $acsClient = null;

   	/**
   	 * 取得AcsClient
   	 *
   	 * @return DefaultAcsClient
   	 */
   	public static function getAcsClient() 
   	{
   	    //产品名称:云通信流量服务API产品,开发者无需替换
   	    $product = "Dysmsapi";

   	    //产品域名,开发者无需替换
   	    $domain = "dysmsapi.aliyuncs.com";

   	    // TODO 此处需要替换成开发者自己的AK (https://ak-console.aliyun.com/)
   	    $accessKeyId = "aaaaaaaaaaaaa"; // AccessKeyId

   	    $accessKeySecret = "bbbbbbbbbbb"; // AccessKeySecret

   	    // 暂时不支持多Region
   	    $region = "cn-hangzhou";

   	    // 服务结点
   	    $endPointName = "cn-hangzhou";


   	    if(static::$acsClient == null) {

   	        //初始化acsClient,暂不支持region化
   	        $profile = DefaultProfile::getProfile($region, $accessKeyId, $accessKeySecret);

   	        // 增加服务结点
   	        DefaultProfile::addEndpoint($endPointName, $region, $product, $domain);

   	        // 初始化AcsClient用于发起请求
   	        static::$acsClient = new DefaultAcsClient($profile);
   	    }
   	    return static::$acsClient;
   	}

   	/**
   	 * 阿里大鱼发送短信
   	 * @return stdClass
   	 */
   	private function sendSms($phone,$code) 
   	{

   	    // 初始化SendSmsRequest实例用于设置发送短信的参数
   	    $request = new SendSmsRequest();

   	    //可选-启用https协议
   	    //$request->setProtocol("https");

   	    // 必填，设置短信接收号码
   	    $request->setPhoneNumbers($phone);

   	    // 必填，设置签名名称，应严格按"签名名称"填写，请参考: https://dysms.console.aliyun.com/dysms.htm#/develop/sign
   	    $request->setSignName("你申请的签名");

   	    // 必填，设置模板CODE，应严格按"模板CODE"填写, 请参考: https://dysms.console.aliyun.com/dysms.htm#/develop/template
   	    $request->setTemplateCode("你申请的模版code");

   	    // 可选，设置模板参数, 假如模板中存在变量需要替换则为必填项
   	    $request->setTemplateParam(json_encode(array(  // 短信模板中字段的值
   	        "code"=>$code,
   	        "product"=>"dsd"
   	    ), JSON_UNESCAPED_UNICODE));

   	    // 可选，设置流水号
   	    // $request->setOutId("yourOutId");

   	    // 选填，上行短信扩展码（扩展码字段控制在7位或以下，无特殊需求用户请忽略此字段）
   	    // $request->setSmsUpExtendCode("1234567");

   	    // 发起访问请求
   	    $acsResponse = static::getAcsClient()->getAcsResponse($request);

   	    return $acsResponse;
   	}
}
```
