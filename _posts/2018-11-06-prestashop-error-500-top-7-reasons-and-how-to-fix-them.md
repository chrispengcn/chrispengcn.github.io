---
ID: 1406
post_title: >
  Prestashop Error 500 – Top 7 reasons,
  and how to fix them
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/11/06/prestashop-error-500-top-7-reasons-and-how-to-fix-them/
published: true
post_date: 2018-11-06 02:19:31
---
<h1 class="blog-header-title"><strong>Prestashop Error 500 – Top 7 reasons, and how to fix them</strong></h1>
<p class="post-meta">by <span class="author vcard">Visakh S</span> | <span class="published">28 May , 2018</span></p>

<div class="entry-content">

“500 Internal Server Error” or plain “Internal Server Error” is common in Prestashop installations.

As a Server Administration Service provider, we’ve seen a wide range of reasons for this error in Nginx servers, Apache, IIS, LiteSpeed and more.

It can range from Memory limits and File permission issues to obscrure Deadlock errors and Cache issues.
<div id="attachment_98918" class="wp-caption aligncenter">

<img class="lazy size-full wp-image-98918 loaded lazy-loaded" src="https://bobcares.com/wp-content/uploads/prestashop-error-500.png" sizes="(max-width: 575px) 100vw, 575px" srcset="https://bobcares.com/wp-content/uploads/prestashop-error-500.png 575w, https://bobcares.com/wp-content/uploads/prestashop-error-500-300x88.png 300w" alt="Prestashop error 500" width="575" height="168" data-lazy-type="image" data-lazy-src="https://bobcares.com/wp-content/uploads/prestashop-error-500.png" data-lazy-srcset="https://bobcares.com/wp-content/uploads/prestashop-error-500.png 575w, https://bobcares.com/wp-content/uploads/prestashop-error-500-300x88.png 300w" data-lazy-sizes="(max-width: 575px) 100vw, 575px" data-was-processed="true" />
<p class="wp-caption-text">Here’s what an HTTP error 500 looked like in a Prestashop hosted in Nginx.</p>

</div>
&nbsp;
<h2>What is HTTP 500 Internal Sever Error?</h2>
“<strong>500</strong>” is an universal error code used by all Web Servers to say, “<em>Some error happened. I have no clue what!</em>”

Yeah, not very helpful.

But, the HTTP error logs can (and often will) show you what has gone wrong.

&nbsp;
<h2>How to troubleshoot Error 500</h2>
Every web server will have an error log.

For Apache servers, it is usually located at <code>/var/log/apache/error.log</code>. It will show you something like:

<code>[Wed Apr 18 12:59:12.862338 2018] [php7:error] [pid 3538] [client XX.XX.XX.XX:31504] PHP Fatal error: Out of memory (allocated 2097152) (tried to allocate 59462712 bytes) in /home/myuser/public_html/classes/Configuration.php on line 206</code>

Again, not very helpful unless you know what these numbers mean.

Now, let’s look at another option.

&nbsp;
<h4><strong>Troubleshooting Error 500 in Prestashop</strong></h4>
In Prestashop, you can also get some additional information by turning on error reporting in the file “<strong>config/defines.inc.php</strong>”
<ul>
 	<li>For Prestashop v1.5.2 and lower, change <code>@ini_set('display_errors', '<strong>off</strong>');</code> to <code>@ini_set('display_errors', '<strong>on</strong>');</code>.</li>
 	<li>For Prestashop v1.5.3 and higher, change <code>define('_PS_MODE_DEV_', <strong>false</strong>);</code>to <code>define('_PS_MODE_DEV_', <strong>true</strong>);</code>.</li>
</ul>
Then take the page that gave you the error once more, and you’ll see something like this:

<code>[PrestaShopDatabaseException]</code>
<code>Db-&gt;executeS() must be used only with select, show, explain or describe queries at line 471 in file classes/db/Db.php</code>
<code>465.</code>
<code>466. // This method must be used only with queries which display results</code>
<code>467. if (!preg_match('#^\s*\(?\s*(select|show|explain|describe|desc)\s#i', $sql))</code>

OK, that can look scary too.

&nbsp;
<h2>Just tell me how to fix this</h2>
Now, if you are like most website owners, you’ll want to know what are the common reasons for this error, so that you can try out some solutions yourself.

Don’t worry, we have you covered.

Here we have the top 7 reasons why you’ll get this error, and what you can do to fix them.

&nbsp;
<h2>1. Low PHP Memory Limit</h2>
The most common reason we’ve seen for error 500 is low memory limit for PHP.

Many web hosts set the default memory allocation for PHP as 32 MB, 64 MB or 128 MB.

However, Prestashop requires at least 128 MB to function, and throw in some more for add-ons if you need them.

So, a sensible memory allocation is “256 MB”. If you have a VPS, set it to “512 MB”.

&nbsp;
<h4>How to increase PHP Memory Limit</h4>
There are many ways to increase PHP memory size.

Some hosts provide web administration interface. Some allow command line access.

Here’s one way that we’ve seen to be working for many:
<ul>
 	<li>Login to your FTP account.</li>
 	<li>Download .htaccess file.</li>
 	<li>Add the line <code>php_value memory_limit 512M</code> in the file, and save</li>
 	<li>Re-upload the file.</li>
</ul>
If you are not sure how to do this, or if editing is disallowed in your account, our Prestashop experts can help you fix this. Click here to submit a support request. We are online 24/7.

&nbsp;
<h2>2. Wrong file &amp; folder permissions</h2>
Some servers that have SuPHP or Fast-CGI enabled can be especially sensitive to file and folder permissions.

The right permissions for files is <code>644</code> and folders is <code>755</code>.

If you see from the server logs or Prestashop error report that file permissions maybe the issue, try resetting the permissions using the commands:

<code>find /home/USERNAME/public_html -type d -exec chmod 755 {} \;</code>
<code>find /home/USERNAME/public_html -type f -exec chmod 644 {} \;</code>

Replace <code>/home/USERNAME/public_html</code> with the path to your web directory.

You might also do this from an admin front-end (if you are provided with one).

<strong>Warning</strong> : Take a backup of your site before you execute any command.

If you are not sure how to reset the file and folder permissions, click here to request support. Our Prestashop experts are online 24/7.

&nbsp;
<h2>3. Corrupted files or wrong encoding</h2>
Many Prestashop owners come to us saying the site broke after they tried to change something.

We’ve seen that often these issues are caused by incorrect file encoding or stray spaces that’s inserted while editing the files.

If you are not a web hosting expert, the best way to solve this is to download a fresh copy of Prestashop, and replace recently changed files.

If you’ve made extensive changes to your files, you’ll need to manually check the file formats and comb for syntax errors.

If you find that difficult, don’t worry. We can do it for you in a few minutes. Click here to open a support request to our Prestashop experts. We’re online 24/7.

&nbsp;
<h2>4. Coding error in recently installed/updated module or theme</h2>
Prestashop addons (themes and modules) help you setup a shop just like you imagined.

But the downside is that some of these themes and modules might not be well maintained.

Many times we’ve seen some module using deprecated functions, or incorrect code, causing Prestashop to display the white screen.

So, if error 500 was shown after you recently installed or upgraded a new addon, try deleting that addon folder from backend. It should be located in the <code>/themes/</code> or <code>/modules/</code> folder in your Prestashop’s <code>public_html</code>.

If you are unable to find out which addon caused the error, we can fix that for you. Click here to open a support request to our 24/7 Prestashop support team.

&nbsp;
<h2>5. Missing PHP modules</h2>
A common issue we’ve seen with Prestashop sites in VPSs is that they lack all the needed PHP modules.

For Prestashop to function, it needs:
<ul>
 	<li>Mcrypt</li>
 	<li>OpenSSL</li>
 	<li>Zip</li>
 	<li>Curl</li>
 	<li>GD</li>
 	<li>PDO</li>
</ul>
To check if these modules are enabled for your website, copy the below code into a file called <code>phpinfo.php</code> and upload it to your site. Then take it in a browser, and check for these extensions.
<code>&lt;?</code>
<code>phpinfo();</code>
<code>?&gt;</code>
If any of those modules are missing, check your server’s <code>php.ini</code> to see if it is enabled. If not, you’ll need to install them.

Installing modules can get a bit technical. If you suspect PHP modules are missing in your server, click here to request support from our Prestashop experts. We can fix that for you in a few minutes.

Note: Remember to delete the phpinfo.php file once you are done. Hackers could use that to target specific vulnerabilities.

&nbsp;
<h2>6. Database connection limit</h2>
Some users have reported the error : “<strong>PrestaShop Fatal error: no utf-8 support. Please check your server configuration</strong>“.

This might look like UTF-8 support isn’t enabled in the server.

But in reality, it is just Prestashop unable to run a database query.

Some servers have database query limits (eg. 10,000 queries per hour). Any query above that limit wouldn’t be sent to the MySQL database, and it’ll show the UTF-8 error.

Of course it is also possible that the UTF-8 encoding in your database might be changed due to some reason.

If you have a VPS, you can fix this by changing the <code>max_questions</code> variable in the MySQL configuration file to <code>0</code> (means unlimited).

If you need help fixing the database limits, our Prestashop experts are online 24/7. Click here to open a support request.

&nbsp;
<h2>7. Stale cache, incorrect .htaccess and many others</h2>
OK, this is not exactly ONE reason. This point exists to say there could be a lot more other reasons why Error 500 can happen.

Some of them are:
<ul>
 	<li><strong>.htaccess errors</strong> : Syntax errors in .htaccess files, especially those used to pass PHP variables can cause compilation errors.</li>
 	<li><strong>Security settings</strong> : Some security settings such as mod_security limits can cause the execution to fail. It’ll need to be found out by looking at the web server logs.</li>
 	<li><strong>Many more</strong> : Basically anything that blocks the proper execution of Prestashop files can cause this error. If none of these seem to fit your issue, you’ll need to enable Error Reporting.</li>
</ul>
&nbsp;
<h2>None of these fixes worked. What now?</h2>
There’s good news and bad news.

Bad news is that you’ll really have to geek out the cryptic log files.

Good news is that, you really don’t have to.

Here at Bobcares, our Prestashop experts do this kind of troubleshooting all day long.

It’ll take us only a few minutes to go through the logs and zero in on the solution.

So, if you need help, click here to get a Prestashop expert to look into your site. Yes, we are online 24/7.

&nbsp;
<h2>Conclusion</h2>
Prestashop error 500 can happen due to PHP memory limits, file permission issues, database connection errors, and more. Here we have listed the top 7 reasons for this error, and how you can resolve it yourself.

</div>
https://bobcares.com/blog/prestashop-error-500/