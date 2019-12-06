---
title: Django使用第三方存储服务器
date: 2018-05-20
updated: 2018-06-22
tags: Django
categories: 后端开发
keywords: 
description: 
---

随着服务器运作，难免有些时候需要上传文件。上传文件自然是在云服务器上，日积月累，慢慢地服务器存储空间越来越少。而且服务器迁移和备份都不方便，使用这些资源时也占用大量服务器流量。

较好的解决方案是使用第三方存储服务器，例如七牛、阿里云OSS、亚马逊S3等。将文件都放到这些存储服务器，可以减少服务器负担。服务器只剩下必要的静态文件和代码。



以阿里云OSS为例，讲解如何使用第三方存储服务器。（刚好最近用到这个，而且Django有其他人写好的第三方库）

首先，需要拥有OSS。这个去阿里云购买即可。购买之后可得到密钥等一系列信息。

接着，安装oss2库，该库是Python对应oss的操作库。

```shell
pip install oss2
```



再安装或下载Django OSS的Storage库。这些库是继承Django的Storage类，并重写相关方法。Django的Stroage是管理上传文件的存储。如何自定义Storage可参考[Django官方文档](https://docs.djangoproject.com/en/1.11/howto/custom-file-storage/)。

执行如下命令，安装Django-Aliyun-OSS2-Storage：

```shell
pip install django-aliyun-oss2-storage
```



也可以不用pip安装，打开[该第三方库的Github](https://github.com/xiewenya/django-aliyun-oss2-storage)，下载源码到本地。这里我需要修改部分代码，所以直接下载把整个包放到Django项目的根目录（也可其他位置）。

安装下载完成之后，配置Django的Settings，添加如下设置：

```python
# 使用OSS存储文件
DEFAULT_FILE_STORAGE = 'aliyun_oss2_storage.backends.AliyunMediaStorage'
 
# 配置OSS信息
ACCESS_KEY_ID = "xxxxxxxxxxx"
ACCESS_KEY_SECRET = "xxxxxxxxxxx"
END_POINT = "oss-cn-shenzhen.aliyuncs.com"  # OSS存储节点
BUCKET_NAME = "xxx"
BUCKET_ACL_TYPE = "public-read"  # private, public-read, public-read-write
```



另外，还有两个对应参数需要注意一下，MEDIA_ROOT和MEDIA_URL。

MEDIA_ROOT是媒体文件的上传位置根目录，由于设置了BUCKET_NAME，一般在这个bucket中。可以设置为空字符串。

文件自然上传到Django模版的FileField字段设置的upload_to位置。

MEDIA_URL是获取媒体文件的链接前缀，可根据自己的oss文件链接位置添加。



由于上传的文件需要开放被用户下载，BUCKET_ACL_TYPE设置为公共的。若你的静态文件也需要上传到OSS中，设置如下：

```python
# 设置上传的静态文件
STATICFILES_STORAGE = 'aliyun_oss2_storage.backends.AliyunStaticStorage'
```



设置无误后，重启Django即可使用。上传文件将自动上传到OSS中。

上面提到我要修改里面的源码。因为发现上传的文件在下载时的文件名是一串乱码，不是上传时的文件名。这个需要设置一些header信息，可参考[OSS的SDK文档](https://help.aliyun.com/document_detail/31978.html?spm=5176.doc31859.2.1.2jOIPWhttps://github.com/xiewenya/django-aliyun-oss2-storage)。该header需要在上传文件时就提交，而上面的django-aliyun-oss2-storage在上传文件时没有写入header信息。

打开该包的源码文件backends.py，找到AliyunBaseStorage类的_save方法。修改如下：

```python
def _save(self, name, content):
    # 获得文件名
    filename = str(content)
 
    # 设置header
    headers = {}
    headers['Content-Type'] = 'application/octet-stream'
    headers['Content-Disposition'] = 'attachment; filename=%s' % filename
 
    # get oss' target name
    name = self._get_target_name(name)
 
    content.open()
    content_str = b''.join(chunk for chunk in content.chunks())
    self.bucket.put_object(name, content_str, headers=headers)  # add headers
    content.close()
 
    return self._clean_name(name)
```



这样设置，点击文件链接，即可下载并且下载文件名是上传的文件名。若你不是什么类型文件都需要这么处理，可以判断filename的后缀名加以处理。
