---
ID: 887
post_title: >
  Magento即時、便利、不影響編碼的Inline
  Translation，前台改文駕輕就熟！
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2017/08/07/magento%e5%8d%b3%e6%99%82%e3%80%81%e4%be%bf%e5%88%a9%e3%80%81%e4%b8%8d%e5%bd%b1%e9%9f%bf%e7%b7%a8%e7%a2%bc%e7%9a%84inline-translation%ef%bc%8c%e5%89%8d%e5%8f%b0%e6%94%b9%e6%96%87%e9%a7%95%e8%bc%95/'
published: true
post_date: 2017-08-07 09:01:50
---
<h1 class="single-title">Magento即時、便利、不影響編碼的Inline Translation，前台改文駕輕就熟！</h1>
&nbsp;
<p dir="ltr">繼上次介紹過如何在Magento後台編輯文字後，這次要和大家分享另一種更改文字的方法，
在<a href="http://www.astralweb.com.tw/magento/">Magento</a>裡面稱作Inline Translation，也就是直接於網頁修改文字！
也許大家會有疑問，既然都是修改文字，這兩者的分別差在哪呢？</p>
<p dir="ltr">這裡為大家稍作整理：</p>

<h4 dir="ltr">後台編輯器：</h4>
<h6 dir="ltr">主要功能為「編輯」，因此不只修改文字，還能像部落格一樣新增圖片、加入超連結等，進行文件編輯作業。畢竟後台才是中央處理中心，在這裡能修改的內容更多。</h6>
<h4>Inline Translation 網頁前台修改：</h4>
<h6>當您要修改網站選單、產品簡介等文字內容，或是翻譯語言介面等，由於只有碰觸文字部分，不必特意跑到後台修改，直接在網頁上進行修改即可。</h6>
<p dir="ltr">兩者比較後，可以知道後台編輯器能進行的作業更廣泛，適合做文字、圖片的全面編輯；
而Inline Translation提供的是便利、簡單的修改文字，無法進行圖片、超連結等編輯作業。
但是，在做檢查測試、細部微調時，Inline Translation會是相當省事的方法。</p>
<p dir="ltr">在預覽網頁時，突然發現一個錯字，這時不必跑到後台找對應的網頁修改，直接在網頁上進行即可，省去了前台、後台兩邊跑的程序，並能當下確認修改結果。</p>
<p dir="ltr">接下來，就讓我們進入Inline Translation的教學步驟，趕緊學起來應用在您的網站上吧！</p>
<p dir="ltr">由於Inline Translation必須登入IP才能進行修改，若您不曉得自己的IP位置，請先去google search 搜尋關鍵字「 My IP」。</p>
<a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-01.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-362" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-01.png" alt='Inline-Translation-01' width="650" height=" " /></a>
<p dir="ltr">點選google search後，會顯示您的IP位置，請複製這組數字。</p>
 <a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-02.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-363" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-02.png" alt='Inline-Translation-02' width="554" height="221" /></a>

接著，登入您的帳號、密碼並進入magento後台管理頁面；在上排選單的最右邊找尋System，點選最下方的configuartion。

<a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-03.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-364" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-03.png" alt='Inline-Translation-03' width="650" height=" " /></a>

&nbsp;

在configuartion的介面，於左邊列表的最下方會看到developer，請點選進入。

<a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-04.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-365" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-04.png" alt='Inline-Translation-04' width="650" height=" " /></a>

&nbsp;
<p dir="ltr">進入後，請點選Translate Inline展開欄位，在Enabled for Frontend的地方選擇Yes；
接著您會看到Allowed IPs的欄位，請在此處貼上您的IP位置，才能進行Inline Translation。</p>

<h4>註1：若同時有其他IP要進行此作業，請記得用「,」分隔每個IP位置，就可以一起修改囉！</h4>
<a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-05.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-366" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-05.png" alt='Inline-Translation-05' width="650" height=" " /></a>

&nbsp;
<p dir="ltr">設定好後請前往網站，您會發現大部分的網頁文字都有紅框，這樣就設定完成了；
當然，這樣的畫面，只有登入IP位置才能看到，因此不必擔心會被顧客發現喔！</p>
<p dir="ltr"><a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-06.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-367" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-06.png" alt='Inline-Translation-06' width="650" /></a></p>
<p dir="ltr">更改的方法相當簡單，只要將游標移到您想要更改的字，左側就會出現書本的圖案，此時請點選書本的圖示。</p>
<p dir="ltr"><a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-07.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-368" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-07.png" alt='Inline-Translation-07' width="650" height=" " /></a></p>
<p dir="ltr">點進去後會出現以下視窗，請在Custom的欄位輸入您要更改的字。</p>
<p dir="ltr"><a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-08.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-369" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-08.png" alt='Inline-Translation-08' width="650" height=" " /></a></p>
<p dir="ltr">假設我想將Category改成Category00，填好後按下submit送出。</p>
<p dir="ltr"><a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-09.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-370" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-09.png" alt='Inline-Translation-09' width="650" height=" " /></a></p>
<p dir="ltr">送出後先別急，請先重新整理網頁，才會出現修改後的字喔！
但在某些狀況下，重新整理也不見得能完成更改，此時，請再回去magento的後台管理，於上方選單中的system中，找尋cache management，並點選進去。</p>
 <a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-10.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-371" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-10.png" alt='Inline-Translation-10' width="650" height=" " /></a>
<p dir="ltr">進入畫面後，請點選右上方的flush Magento Cache；特別注意，請勿點選到其他按鈕，以免造成網站問題喔！</p>
 <a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-11.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-372" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-11.png" alt='Inline-Translation-11' width="650" height=" " /></a>

最後再請您回到前台網站重新整理，就會看到修改後的結果囉！

<a href="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-12.png" target="_blank" rel="noopener noreferrer"><img class="alignnone  wp-image-373" title="magento 修改文字" src="http://hss5.com/wp-content/uploads/2017/08/Inline-Translation-12.png" alt='Inline-Translation-12' width="650" /></a>
<p dir="ltr">相信使用過Inline Translation的朋友們，會發現不是每個字都有紅框可以修改；遇上這種情況時，還是得回去後台管理了！
畢竟Inline Translation提供的是簡單、方便的管道，能修復的部分有限；但我們還是得肯定Inline Translation的優點，即時修改的便利性，以及完全不影響編碼，讓初學者也能駕輕就熟，不怕改字改到網站出問題！</p>
<p dir="ltr">想了解更多Magento教學？
請點選<a href="http://www.astralweb.com.tw/magento-learning-guide-book/">Magento教學導覽索引</a>，幫助您快速找到需要資訊！</p>
<p dir="ltr">教學影片：
<iframe src="https://www.youtube.com/embed/hpV19CsGDPc" width="640" height="360" frameborder="0" allowfullscreen="allowfullscreen" data-mce-fragment="1"></iframe></p>
<p dir="ltr"><a href="http://www.astralweb.com.tw/">Astral Web</a> 製作編寫</p>