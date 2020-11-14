---
ID: 3785
post_title: >
  How to upload the file on S3 bucket via
  terminal?
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://vpseo.com/2020/11/15/how-to-upload-the-file-on-s3-bucket-via-terminal/
published: true
post_date: 2020-11-15 00:11:18
---
<p id="1542" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">For the ones who are not introduced to the world of S3 bucket, this article will guide you on how to upload the file on s3bucket through the terminal in Linux and assigning permission to it.</p>
<p id="35c9" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">But before that let’s get a small overview on what S3 bucket is and in what ways the file can be uploaded on s3bucket.</p>
<p id="7cc9" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">S3 bucket is one of the many services pr<span id="rmm">o</span>vided by AWS. It is nothing but public cloud storage where you can store your files, kind of a folder in your pc but unlike pc, you can access it anywhere you want. S3 is an abbreviation of <strong class="gv co">Simple Storage Service</strong>. AWS provides many ways to upload a file on the s3 bucket, which are given below:</p>

<ol class="">
 	<li id="f1f6" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq hr hs ht fc" data-selectable-paragraph="">Upload file using Drag and Drop</li>
 	<li id="9eae" class="gt gu eg gv b gw hu gy gz ha hv hc hd he hw hg hh hi hx hk hl hm hy ho hp hq hr hs ht fc" data-selectable-paragraph="">Upload file using click</li>
 	<li id="f67f" class="gt gu eg gv b gw hu gy gz ha hv hc hd he hw hg hh hi hx hk hl hm hy ho hp hq hr hs ht fc" data-selectable-paragraph="">Upload file using aws CLI in the terminal.</li>
</ol>
<p id="16bc" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">From the above-mentioned ways, we are going to look into the 3rd way.</p>
<p id="cce7" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">As a first step, you need to install aws CLI on your local machine. Here is the command to install it.</p>

<pre class="hz ia ib ic id ie if ig"><span id="efd7" class="fc ih ii eg ij b dd ik il s im" data-selectable-paragraph="">sudo apt install aws</span></pre>
<p id="d706" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">Once the installation is done, please configure the aws with this command:</p>

<pre class="hz ia ib ic id ie if ig"><span id="e20b" class="fc ih ii eg ij b dd ik il s im" data-selectable-paragraph="">aws configure</span></pre>
<p id="d471" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">As soon as you run the above command you will be asked the following details one by one in the terminal as you keep entering the correct values and press enter.</p>

<pre>AWS Access Key ID: &lt;S3_BUCKET_NAME&gt;
AWS Secret Access Key: &lt;AWS_SERVER_PUBLIC_KEY&gt;
Default region name: &lt;S3_BUCKET_REGION&gt;
Default output format: json</pre>
<p id="784e" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">After you enter the relevant information, your aws is configured.</p>
<p id="f08d" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">Now onto uploading the file and to do that following command will help you achieve it.</p>

<pre>aws s3 cp &lt;path-to-file-from-local&gt; s3://&lt;S3_BUCKET_NAME&gt;/&lt;folder-name&gt; --acl public-read
</pre>
<p id="2216" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">eg: <strong class="gv co">aws s3 cp /home/nehalk/Desktop/file.txt s3://&lt;S3_BUCKET_NAME&gt;/resources — acl public-read</strong></p>
<p id="cafc" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">And voila! Your file, file.txt has been uploaded from Desktop folder of your local machine to resources folder in S3 bucket.</p>
<p id="5c27" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">Another useful command is to permit the given file to download it.</p>

<pre>aws s3api put-object-acl --bucket &lt;S3_BUCKET_NAME&gt; --key &lt;path-to-file-on-s3-bucket&gt; --acl public-read
</pre>
<p id="516f" class="gt gu eg gv b gw gx gy gz ha hb hc hd he hf hg hh hi hj hk hl hm hn ho hp hq db fc" data-selectable-paragraph="">This command will give the read permission to that and after that, you can download the file.</p>
<p data-selectable-paragraph=""></p>
<p data-selectable-paragraph=""></p>
In order to use the AWS CLI with object storage service that provide an S3-compatible API like DigitalOcean Spaces, you must also configure a custom endpoint.

This can be done on the command line using the <code>--endpoint</code> flag:
<pre><code>aws s3 ls --endpoint=https://nyc3.digitaloceanspaces.com</code>


<span class="comment-copy">Worth pointing out that there's no way currently to have a default endpoint. You gotta specify it every time. I usually alias it for convenience: <code>alias awsdo='aws --endpoint=https://nyc3.digitaloceanspaces.com'</code></span> – <a class="comment-user" title="8,778 reputation" href="https://stackoverflow.com/users/2062384/freedom-ben">Freedom_Ben</a> <span class="comment-date" dir="ltr"><a class="comment-link" href="https://stackoverflow.com/questions/53712255/how-to-use-aws-cli-with-digital-ocean-spaces#comment99722036_53713795"><span class="relativetime-clean" title="2019-06-12 23:47:38Z, License: CC BY-SA 4.0">Jun 12 '19 at 23:47</span></a></span></pre>