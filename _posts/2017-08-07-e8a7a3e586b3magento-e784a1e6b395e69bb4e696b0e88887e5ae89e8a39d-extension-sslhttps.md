---
ID: 883
post_title: >
  解决Magento 無法更新與安裝
  extension – SSL(https)
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2017/08/07/%e8%a7%a3%e5%86%b3magento-%e7%84%a1%e6%b3%95%e6%9b%b4%e6%96%b0%e8%88%87%e5%ae%89%e8%a3%9d-extension-sslhttps/'
published: true
post_date: 2017-08-07 08:28:39
---
Magento 無法更新與安裝 extension，這個問題會發生的情形，主要原因是，

Magento 在版本 1.9.2.0 之後，預設要求 HTTPS 連線。

這個問題多半會發生在開發環境上，本機(localhost)或是測試主機，因為通常這些環境下不會設定SSL。<span id="more-6740"></span>

所以如果在未設定SSL的環境下，想要更新或安裝extension時，就會遇到此訊息。

Unknown cipher in list: TLSv1

<img class="alignnone wp-image-6741" src="http://hss5.com/wp-content/uploads/2017/08/image001.jpg" alt='image00' width="750" height="578" />

所以我們現在就是要來修改設定檔了，在一般開發的環境下關閉SSL，或是沒有設定SSL的伺服器也關閉這個設定(只是不建議這麼做，不論如何增設SSL增加安全性都是好的)。

&nbsp;

在Magento資料夾裡，依此路徑找到這個檔案：<b>downloader/lib/Mage/HTTP/Client/Curl.php</b>

&nbsp;

在約第377行：
<pre class="prettyprint prettyprinted"><span class="pun">...</span><span class="pln">
$this</span><span class="pun">-&gt;</span><span class="pln">curlOption</span><span class="pun">(</span><span class="pln">CURLOPT_SSL_CIPHER_LIST</span><span class="pun">,</span> <span class="str">'TLSv1'</span><span class="pun">);</span>
<span class="pun">...</span></pre>
註解掉：
<pre class="prettyprint prettyprinted"><span class="pun">...</span>
<span class="com">// $this-&gt;curlOption(CURLOPT_SSL_CIPHER_LIST, 'TLSv1');</span>
<span class="pun">...</span></pre>
&nbsp;

然後再回到 Magento connect，安裝 extension 就可以進行了。

&nbsp;

還是額外提醒一下，假如您的正式網站沒有SSL，你可以在安裝extension時，先關閉(註解)這個設定，安裝後再恢復，來維持您的 Magento 網站安全性。

&nbsp;

来源： astralweb