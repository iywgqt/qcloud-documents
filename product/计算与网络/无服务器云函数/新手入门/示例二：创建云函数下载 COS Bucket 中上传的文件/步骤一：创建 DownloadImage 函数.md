1) 登录腾讯云控制台，选择【对象存储服务】。

2) 单击【Bucket列表】选项卡下的【创建Bucket】按钮，新建一个COS Bucket。

3) 设置COS Bucket的名称如`testbucket`，选择地域为`华南`，设置访问权限为默认值`公有读私有写`并设置CDN加速为默认值`关闭`，单击【确定】按钮新建一个COS Bucket。

4) 在控制台上导航至【无服务器云函数】，在`广州`地域下单击【创建函数】按钮，进入新建函数页面。

5) 填写函数名称`DownloadImage`，其他配置项保持默认选项即可。

6) 单击【下一步】按钮，进入函数代码编辑页，默认选择【在线编辑】，并在【模版】中选择 `COS Put Object` 模版。此时，执行方法和代码将填入模版的默认值：

执行方法显示`index.main_handler`。表示无服务器云函数控制台会将此段代码自动保存为`index.py`文件，并压缩该文件上传至 SCF 平台以创建云函数。

函数代码显示以下代码片段，修改其中 `appid, secret_id, secret_key, region`参数字段，改为您的实际数据。其中：

- appid可在控制台【账户信息】中获得
![](//mc.qcloudimg.com/static/img/8149e0d15b64340c2a2dca5569854af8/image.png)
- secret_id 和 secret_key可在控制台【云API密钥】中获得
![](//mc.qcloudimg.com/static/img/e1eecfe7459069d0f453083ff459e30e/image.png)
- region 为 函数 和 COS Bucket 所在地域，支持 `sh、gz、bj` 三个值。请注意，必须保持和 COS Bucket 在同一个地域。此处第一步中创建的存储桶位于华南（广州），因此代码中的 region 值必须为 `gz`

```
import json
import urllib
import commands
import logging
from qcloud_cos import CosClient
from qcloud_cos import DownloadFileRequest
print('Loading function')
appid = 1251111111  #please change to your appid. Find it in Account Info
secret_id = u'AKIDYDh085xQp48161uOn2CKKVbeebvDu9Ib'   #please change to your API secret id. Find it in API secret key pair
secret_key = 'lLkxx40kIfuyqW0IOI0WqyueCYjlgzqE'  #please change to your API secret key. Find it in API secret key pair
region = u'gz'  
def main_handler(event,context):
    logger = logging.getLogger()
    bucket = event['Records'][0]['cos']['cosBucket']['name']
    key = urllib.unquote_plus(event['Records'][0]['cos']['cosObject']['key'].encode('utf8'))
    try:
        cos_client = CosClient(appid, secret_id, secret_key, region)
        request = DownloadFileRequest(bucket, key, '/tmp'+key)
        download_file_ret = cos_client.download_file(request) 
        if download_file_ret['code'] == 0:
            logger.info("Download file [%s] Success" % key)
            logger.info("find local file:" + commands.getoutput('ls /tmp'))
            return "download success"
        else:
            logger.error("Download file [%s] Failed, err: %s" % (key, download_file_ret['message']))
            return -1
    except Exception as e:
        print(e)
        print('Error getting object {} from bucket {}. Make sure the object exists and your bucket is in the same region as this function.'.format(key, bucket))
        raise e


```

7) 单击【下一步】按钮，进入触发方式页面。单击【新建触发方式】按钮为函数添加一个新的触发器，选择触发方式为`COS触发`，并选择刚刚创建的`testbucket`存储桶，事件类型选择为`文件上传`，单击【保存】按钮。

8) 单击底部【完成】按钮，此时控制台会自动生成代码程序包并上传至 SCF 平台以创建云函数。您可以单击云函数列表页中刚刚创建的`DownloadImage`函数进入云函数详情页。

