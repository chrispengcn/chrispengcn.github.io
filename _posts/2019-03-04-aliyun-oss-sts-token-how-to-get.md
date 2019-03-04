---
ID: 2504
post_title: 阿里云STS token浅析
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/03/04/aliyun-oss-sts-token-how-to-get/
published: true
post_date: 2019-03-04 18:16:11
---
<h1 class="title">阿里云STS token浅析</h1>
<div class="author"><img class="alignnone size-full wp-image-2508" src="http://hss5.com/wp-content/uploads/2019/03/8b724579399e927d549e7559d32e6999-2.jpg" width="96" height="96" alt="96" />
<div class="info"><span class="name"><a href="https://www.jianshu.com/u/e0a0d32748b8">阿呆少爷</a></span> <a class="btn btn-success follow"><i class="iconfont ic-follow"></i>关注</a>
<div class="meta"></div>
</div>
</div>
<div class="show-content" data-note-content="">
<div class="show-content-free">
<blockquote>非常想搞明白STS token在端上是如何使用的。因为OSS跟STS联系比较紧密，所以把这两个家伙一起研究了一下。里面有一些环节我现在也没搞太明白，只是记录一下，以后搞明白了再完善。</blockquote>
<h3>配置账号及角色</h3>
去RAM里面设置好子账号和角色，子账号需要<code>AliyunSTSAssumeRoleAccess</code>权限，角色需要<code>AliyunOSSFullAccess</code>权限。子账号需要有扮演OSS存取这个角色的能力，成功扮演该角色之后，就具有操作OSS资源的能力了。关于用户和角色关系，<a href="https://link.jianshu.com/?t=https://help.aliyun.com/document_detail/31931.html?spm=5176.doc32031.6.634.rZLN5u" target="_blank" rel="nofollow noopener">RAM和STS介绍</a>里面有比较详细的描述。

这里有一个坑是角色一定要选择<code>用户角色</code>。之前错误选择了<code>服务角色</code>，导致AssumeRole一直提示无权限。
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1384" data-height="522"><img class="" src="https://upload-images.jianshu.io/upload_images/1376176-cb2a68607435f780.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/1376176-cb2a68607435f780.png" data-original-width="1384" data-original-height="522" data-original-format="image/png" data-original-filesize="83525" /></div>
</div>
<div class="image-caption">screenshot.png</div>
</div>
<h3>获取STS token</h3>
<ul>
 	<li>安装好Python SDK。Python测试起来比较方便。</li>
</ul>
<pre class="hljs undefined"><code>pip install oss2
</code></pre>
<ul>
 	<li>运行脚本，获取STS token。</li>
</ul>
<pre class="hljs python"><code class="python"><span class="hljs-comment">#!/usr/bin/env python</span>
<span class="hljs-comment">#coding=utf-8</span>

<span class="hljs-keyword">from</span> aliyunsdkcore <span class="hljs-keyword">import</span> client
<span class="hljs-keyword">from</span> aliyunsdksts.request.v20150401 <span class="hljs-keyword">import</span> AssumeRoleRequest

clt = client.AcsClient(<span class="hljs-string">'access key id'</span>, <span class="hljs-string">'access key secret'</span>, <span class="hljs-string">'cn-shanghai'</span>)

<span class="hljs-comment"># 构造"AssumeRole"请求</span>
request = AssumeRoleRequest.AssumeRoleRequest()

<span class="hljs-comment"># 指定角色</span>
request.set_RoleArn(<span class="hljs-string">'acs:ram::1532770894211314:role/henshaoread2'</span>)

<span class="hljs-comment"># 设置会话名称，审计服务使用此名称区分调用者</span>
request.set_RoleSessionName(<span class="hljs-string">'henshao'</span>)

<span class="hljs-comment">#request.set_method('HMAC-SHA1')</span>

<span class="hljs-comment"># 发起请求，并得到response</span>
response = clt.do_action_with_exception(request)
</code></pre>
实际发出来的请求如下所示。STS API和签名规则可以参考<a href="https://link.jianshu.com/?t=https://help.aliyun.com/document_detail/28761.html?spm=5176.doc28758.6.676.qKuJOx" target="_blank" rel="nofollow noopener">STS文档</a>。
<pre class="hljs undefined"><code>/?RoleSessionName=henshao
&amp;Format=json
&amp;Timestamp=2017-04-28T08%3A16%3A06Z
&amp;RoleArn=acs%3Aram%3A%3A1532770894211314%3Arole%2Fhenshaoread2
&amp;RegionId=cn-shanghai
&amp;SignatureVersion=1.0
&amp;AccessKeyId=LTAIAVqsmhvxNjGN
&amp;SignatureMethod=HMAC-SHA1
&amp;Version=2015-04-01
&amp;Signature=9geUPq00G2g1tInX3XSvk77HZhY%3D
&amp;Action=AssumeRole
&amp;SignatureNonce=cc636798-de57-4c11-beb5-567124635220
</code></pre>
<ul>
 	<li>返回的STS token如下所示，里面包含了AK id、AK secret）和security token。</li>
</ul>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1946" data-height="648"><img class="" src="https://upload-images.jianshu.io/upload_images/1376176-243ead08c94880fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/1376176-243ead08c94880fc.png" data-original-width="1946" data-original-height="648" data-original-format="image/png" data-original-filesize="299547" /></div>
</div>
<div class="image-caption">screenshot.png</div>
</div>
另外一种获取STS token的方式是使用<a href="https://link.jianshu.com/?t=https://github.com/aliyun/aliyun-cli" target="_blank" rel="nofollow noopener">aliyuncli</a>工具，也是非常方便的。
<pre class="hljs ruby"><code class="ruby">$ aliyuncli sts AssumeRole --AccessKeyId xxx --AccessKeySecret xxx --RoleArn xxx --RoleSessionName xxx
{
    <span class="hljs-string">"AssumedRoleUser"</span>: {
        <span class="hljs-string">"AssumedRoleId"</span>: <span class="hljs-string">"389053493353396514:henshao"</span>, 
        <span class="hljs-string">"Arn"</span>: <span class="hljs-string">"acs:ram::1532770894211314:role/henshaoread2/henshao"</span>
    }, 
    <span class="hljs-string">"Credentials"</span>: {
        <span class="hljs-string">"AccessKeySecret"</span>: <span class="hljs-string">"4s7GoYW7bbFLqe3XGbiMdjveaHEvU1bNBNxdXPtkgLK2"</span>, 
        <span class="hljs-string">"SecurityToken"</span>: <span class="hljs-string">"CAIS8gF1q6Ft5B2yfSjIrIXFet3ArJFY1vamUB/3jHcbdLtt16jJ2jz2IHBFfHRrBeEdtfk0n2FW6/8elq5zRpleRUXDNQG7TybIslHPWZHInuDox55m4cTXNAr+Ihr/29CoEIedZdjBe/CrRknZnytou9XTfimjWFrXWv/gy+QQDLItUxK/cCBNCfpPOwJms7V6D3bKMuu3OROY6Qi5TmgQ41cn0zwgsfTunpfFs0GH0GeXkLFF+97DRbG/dNRpMZtFVNO44fd7bKKp0lQLu0kSqfwq3fcfpGyc74/AWgVLhxWLNfHU+9p1IQZ1PLigomnpBFFI/xqAAazzStWsfFdf3btxxYkakt15g06RJeGi/uKqElMfEGIRJ+vjHtLzFqyjtJJV7gLWgkSeinqglonDuRjcfy/oTPcpQ4u3SCcOh1X/e13jVbc1SuaIGnWOCbDFqtxH68+BcTSMl7rZCd/JfDIKIpWmtWBcL1HFkpRTG3Hd04BmB0PY"</span>, 
        <span class="hljs-string">"Expiration"</span>: <span class="hljs-string">"2017-11-02T06:01:53Z"</span>, 
        <span class="hljs-string">"AccessKeyId"</span>: <span class="hljs-string">"STS.MFp1gtANya4MR9FhwNx4A8mb8"</span>
    }, 
    <span class="hljs-string">"RequestId"</span>: <span class="hljs-string">"14AA9E4A-EEF0-403A-9367-3255E836F382"</span>
}
</code></pre>
<h3><a href="https://link.jianshu.com/?t=https://github.com/aliyun/aliyun-oss-ios-sdk" target="_blank" rel="nofollow noopener">OSS客户端</a>使用STS token获取文件</h3>
拿到STS token，客户端上就可以操作OSS资源了。
<pre class="hljs objectivec"><code class="objectivec"><span class="hljs-keyword">id</span>&lt;OSSCredentialProvider&gt; credential = [[OSSStsTokenCredentialProvider alloc] initWithAccessKeyId:<span class="hljs-string">@"STS.DSVjYCefVuXKNP9CTyCfy83Uf"</span> secretKeyId:<span class="hljs-string">@"7raTtc4LK3ZtHrw8wWse7sbWAJypWdox3cpp5nYwxdDk"</span> securityToken:<span class="hljs-string">@"CAIS8gF1q6Ft5B2yfSjIpZDjIeP3iLl3wpqgTHaIp1QsT+lV1/b+hDz2IHBFfHRrBeEdtfk0n2FW6/8elq5zRpleRUXDNSGpOWeEslHPWZHInuDox55m4cTXNAr+Ihr/29CoEIedZdjBe/CrRknZnytou9XTfimjWFrXWv/gy+QQDLItUxK/cCBNCfpPOwJms7V6D3bKMuu3OROY6Qi5TmgQ41cn0zwgsfTunpfFs0GH0GeXkLFF+97DRbG/dNRpMZtFVNO44fd7bKKp0lQLu0kSqfwq3fcfpGyc74/AWgVLhxWLNfHU+9p1IQZ1PLigomnpBFFI/xqAAU0Ia5eMa0XreO/Uh0K6RXJmk9q8dEKrhhMuDEtCK/EUyb+v/8WSfeYez3z2wl9V5Pi7iUSFRciQKHnGyIVAVmBCGWvAZOad83dI8tft8Ks16/mER/wpysTKOcr+rrdn+JnPJDSFDKD5JBJSr0KXR1bbMU+9b0Nrej0P8kVwbhYd"</span>];

OSSClient *client = [[OSSClient alloc] initWithEndpoint:<span class="hljs-string">@"oss-cn-shanghai.aliyuncs.com"</span> credentialProvider:credential];

OSSGetObjectRequest * request = [OSSGetObjectRequest new];

request.bucketName = <span class="hljs-string">@"wla-test2"</span>;
request.objectKey = <span class="hljs-string">@"DragonMedium.png"</span>;

request.downloadProgress = ^(int64_t bytesWritten, int64_t totalBytesWritten, int64_t totalBytesExpectedToWrite) {
    <span class="hljs-built_in">NSLog</span>(<span class="hljs-string">@"%lld, %lld, %lld"</span>, bytesWritten, totalBytesWritten, totalBytesExpectedToWrite);
};

OSSTask * getTask = [client getObject:request];
[getTask continueWithBlock:^<span class="hljs-keyword">id</span>(OSSTask *task) {
    <span class="hljs-keyword">if</span> (!task.error) {
        <span class="hljs-built_in">NSLog</span>(<span class="hljs-string">@"download object success!"</span>);
        OSSGetObjectResult * getResult = task.result;
        <span class="hljs-built_in">NSLog</span>(<span class="hljs-string">@"download result: %@"</span>, getResult.downloadedData);

        <span class="hljs-built_in">UIImage</span> *image = [[<span class="hljs-built_in">UIImage</span> alloc] initWithData: getResult.downloadedData];
        image = <span class="hljs-literal">nil</span>;
    } <span class="hljs-keyword">else</span> {
        <span class="hljs-built_in">NSLog</span>(<span class="hljs-string">@"download object failed, error: %@"</span> ,task.error);
    }
    <span class="hljs-keyword">return</span> <span class="hljs-literal">nil</span>;
}];
</code></pre>
成功获取到DragonMedium.png这张图片。
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="2328" data-height="1212"><img class="" src="https://upload-images.jianshu.io/upload_images/1376176-82eb862def7e1b36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/1376176-82eb862def7e1b36.png" data-original-width="2328" data-original-height="1212" data-original-format="image/png" data-original-filesize="720796" /></div>
</div>
<div class="image-caption">screenshot.png</div>
</div>
稍微分析了一下OSS SDK里面的细节。在header里面有两个重要的字段，<code>Authorization = "OSS " + AK.Id + ":" + sign</code>，x-oss-security-token则是security token。
<pre class="hljs bash"><code class="bash">(lldb) po requestMessage.headerParams
{
    Authorization = <span class="hljs-string">"OSS STS.DSVjYCefVuXKNP9CTyCfy83Uf:spZmloXZZJZFBCE8st9fvRxKLag="</span>;
    <span class="hljs-string">"User-Agent"</span> = <span class="hljs-string">"aliyun-sdk-ios/2.6.0/iOS/10.3/en_US"</span>;
    <span class="hljs-string">"x-oss-security-token"</span> = <span class="hljs-string">"CAIS8gF1q6Ft5B2yfSjIpZDjIeP3iLl3wpqgTHaIp1QsT+lV1/b+hDz2IHBFfHRrBeEdtfk0n2FW6/8elq5zRpleRUXDNSGpOWeEslHPWZHInuDox55m4cTXNAr+Ihr/29CoEIedZdjBe/CrRknZnytou9XTfimjWFrXWv/gy+QQDLItUxK/cCBNCfpPOwJms7V6D3bKMuu3OROY6Qi5TmgQ41cn0zwgsfTunpfFs0GH0GeXkLFF+97DRbG/dNRpMZtFVNO44fd7bKKp0lQLu0kSqfwq3fcfpGyc74/AWgVLhxWLNfHU+9p1IQZ1PLigomnpBFFI/xqAAU0Ia5eMa0XreO/Uh0K6RXJmk9q8dEKrhhMuDEtCK/EUyb+v/8WSfeYez3z2wl9V5Pi7iUSFRciQKHnGyIVAVmBCGWvAZOad83dI8tft8Ks16/mER/wpysTKOcr+rrdn+JnPJDSFDKD5JBJSr0KXR1bbMU+9b0Nrej0P8kVwbhYd"</span>;
}
</code></pre>
<h3>参考资料</h3>
<ol>
 	<li><a href="https://link.jianshu.com/?t=https://help.aliyun.com/document_detail/31867.html?spm=5176.doc32122.6.603.OxVuL8" target="_blank" rel="nofollow noopener">OSS访问控制</a></li>
 	<li><a href="https://link.jianshu.com/?t=https://help.aliyun.com/document_detail/32059.html?spm=5176.doc32062.6.704.p0JHb1" target="_blank" rel="nofollow noopener">OSS访问控制-iOS端</a></li>
 	<li><a href="https://link.jianshu.com/?t=http://www.zhenchao.org/2017/03/11/oauth-v2-token/" target="_blank" rel="nofollow noopener">OAuth2.0协议原理与实现：TOKEN生成算法</a></li>
 	<li><a href="https://link.jianshu.com/?t=http://dev.xiaomi.com/doc/wp-content/uploads/2013/10/%E5%B0%8F%E7%B1%B3%E5%B8%90%E6%88%B7-OAuth2.0-%E6%96%87%E6%A1%A3-1.pdf" target="_blank" rel="nofollow noopener">小米帐户 OAuth 2.0 文档</a></li>
 	<li><a href="https://link.jianshu.com/?t=https://cloud.baidu.com/doc/BOS/API/15.5CSTS.E7.AE.80.E4.BB.8B.html" target="_blank" rel="nofollow noopener">百度云访问控制</a></li>
</ol>
最后非常感谢 @周卓 大神的鼎力支持😀

</div>
</div>