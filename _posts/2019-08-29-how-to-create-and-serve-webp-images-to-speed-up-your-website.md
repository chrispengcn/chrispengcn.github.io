---
ID: 3236
post_title: >
  How To Create and Serve WebP Images to
  Speed Up Your Website
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/08/29/how-to-create-and-serve-webp-images-to-speed-up-your-website/
published: true
post_date: 2019-08-29 11:02:12
---
<div class="section-content section-content-growable content Tutorial-content">
<div class="tutorial-authors">
<div class="component-collaborators-container">
<ul class="component-collaborators-content">
 	<li class="collaborators-byline-data">
<p class="names">By <a href="https://www.digitalocean.com/community/users/abdullatif">Abdullatif Eymash</a></p>
<a class="advice" href="https://www.digitalocean.com/community/write-for-digitalocean">Become an author</a></li>
</ul>
</div>
</div>
<div class="content-body tutorial-content" data-growable-markdown="">

<em>The author selected the <a href="https://www.brightfunds.org/organizations/apache-software-foundation">Apache Software Foundation</a> to receive a donation as part of the <a href="https://do.co/w4do-cta">Write for DOnations</a> program.</em>
<h3 id="introduction">Introduction</h3>
<a href="https://developers.google.com/speed/webp">WebP</a> is an open image format developed by Google in 2010 based on the VP8 video format. Since then, the number of websites and mobile applications using the WebP format has grown at a fast pace. Both Google Chrome and Opera support the WebP format natively, and since these browsers account for about 74% of web traffic, users can access websites faster if these sites use WebP images. There are also <a href="https://bugzilla.mozilla.org/show_bug.cgi?id=1294490">plans for implementing WebP in Firefox</a>.

The WebP format supports both lossy and lossless image compression, including animation. Its main advantage over other image formats used on the web is its much smaller file size, which makes web pages load faster and reduces bandwidth usage. Using WebP images can lead to <a href="https://blog.chromium.org/2014/03/webp-improves-while-rolling-out-across.html">sizeable increases</a> in page speed. If your application or website is experiencing performance issues or increased traffic, converting your images may help optimize page performance.

In this tutorial, you will use the command-line tool <code>cwebp</code> to convert images to WebP format, creating scripts that will watch and convert images in a specific directory. Finally, you’ll explore two ways to serve WebP images to your visitors.
<div data-unique="prerequisites"></div>
<h2 id="prerequisites">Prerequisites</h2>
Working with WebP images does not require a particular distribution, but we will demonstrate how to work with relevant software on Ubuntu 16.04 and CentOS 7. To follow this tutorial you will need:
<ul>
 	<li>A server set up with a non-root sudo user. To set up an Ubuntu 16.04 server, you can follow our <a href="https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04">Ubuntu 16.04 initial server setup guide</a>. If you would like to use CentOS, you can set up a CentOS 7 server with our <a href="https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7">Initial Server Setup with CentOS 7 tutorial</a>.</li>
 	<li>Apache installed on your server. If you are using Ubuntu, you can follow step one of <a href="https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04">How To Install Linux, Apache, MySQL, PHP (LAMP) stack on Ubuntu 16.04</a>. If you are using CentOS, then you should follow step one of <a href="https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-centos-7">How To Install Linux, Apache, MySQL, PHP (LAMP) stack On CentOS 7</a>. Be sure to adjust your firewall settings to allow HTTP and HTTPS traffic.</li>
 	<li><code>mod_rewrite</code> installed on your server. If you are using Ubuntu, you can follow our guide on<a href="https://www.digitalocean.com/community/tutorials/how-to-rewrite-urls-with-mod_rewrite-for-apache-on-ubuntu-16-04">How To Rewrite URLs with mod_rewrite for Apache on Ubuntu 16.04</a>. On CentOS 7<code>mod_rewrite</code> is installed and activated by default.</li>
</ul>
<div data-unique="step-1-—-installing-cwebp-and-preparing-the-images-directory"></div>
<h2 id="step-1-—-installing-cwebp-and-preparing-the-images-directory">Step 1 — Installing cwebp and Preparing the Images Directory</h2>
In this section, we will install software to convert images and create a directory with images as a testing measure.

On Ubuntu 16.04, you can install <code>cwebp</code>, a utility that compresses images to the <code>.webp</code> format, by typing:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">sudo apt-get update</li>
 	<li class="line">sudo apt-get install webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
On CentOS 7, type:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">sudo yum install libwebp-tools</li>
</ul>
<pre class="code-pre command"><code></code></pre>
To create a new images directory called <code>webp</code> in the Apache web root (located by default at <code>/var/www/html</code>) type:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">sudo mkdir /var/www/html/webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
Change the ownership of this directory to your non-root user <strong>sammy</strong>:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">sudo chown <span class="highlight">sammy</span>: /var/www/html/webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
To test commands, you can download free JPEG and PNG images using <code>wget</code>. This tool is installed by default on Ubuntu 16.04; if you are using CentOS 7, you can install it by typing:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">sudo yum install wget</li>
</ul>
<pre class="code-pre command"><code></code></pre>
Next, download the test images using the following commands:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">wget -c "https://upload.wikimedia.org/wikipedia/commons/2/24/Junonia_orithya-Thekkady-2016-12-03-001.jpg?download" -O /var/www/html/webp/image1.jpg</li>
 	<li class="line">wget -c "https://upload.wikimedia.org/wikipedia/commons/5/54/Mycalesis_junonia-Thekkady.jpg" -O /var/www/html/webp/image2.jpg</li>
 	<li class="line">wget -c "https://cdn.pixabay.com/photo/2017/07/18/15/39/dental-care-2516133_640.png" -O /var/www/html/webp/logo.png</li>
</ul>
<pre class="code-pre command"><code></code></pre>
<span class="note"><strong>Note</strong>: These images are available for use and redistribution under the Creative Commons <a href="https://creativecommons.org/licenses/by-sa/4.0/deed.en">Attribution-ShareAlike license</a> and the <a href="https://creativecommons.org/publicdomain/zero/1.0/">Public Domain Dedication</a>.
</span>

Most of your work in the next step will be in the <code>/var/www/html/webp</code> directory, which you can move to by typing:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">cd /var/www/html/webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
With the test images in place, and the Apache web server, <code>mod_rewrite</code>, and <code>cwebp</code> installed, you are ready to move on to converting images.
<div data-unique="step-2-—-compressing-image-files-with-cwebp"></div>
<h2 id="step-2-—-compressing-image-files-with-cwebp">Step 2 — Compressing Image Files with cwebp</h2>
Serving <code>.webp</code> images to site visitors requires <code>.webp</code> versions of image files. In this step, you will convert JPEG and PNG images to the <code>.webp</code> format using <code>cwebp</code>. The <strong>general</strong> syntax of the command looks like this:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">cwebp image.jpg -o image.webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
The <code>-o</code> option specifies the path to the WebP file.

Since you are still in the <code>/var/www/html/webp</code> directory, you can run the following command to convert <code>image1.jpg</code> to <code>image1.webp</code> and <code>image2.jpg</code> to <code>image2.webp</code>:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">cwebp -q 100 image1.jpg -o image1.webp</li>
 	<li class="line">cwebp -q 100 image2.jpg -o image2.webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
Setting the quality factor <code>-q</code> to 100 retains 100% of the image quality; if not specified, the default value is 75.

Next, inspect the size of the JPEG and WebP images using the <code>ls</code> command. The <code>-l</code> option will show the long listing format, which includes the size of the file, and the <code>-h</code> option will make sure that <code>ls</code> prints human readable sizes:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">ls -lh image1.jpg image1.webp image2.jpg image2.webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
<pre class="code-pre "><code></code></pre>
<div class="secondary-code-label " title="Output">Output</div>
<pre class="code-pre "><code>-rw-r--r-- 1 sammy sammy 7.4M Oct 28 23:36 image1.jpg
-rw-r--r-- 1 sammy sammy 3.9M Feb 18 16:46 <span class="highlight">image1.webp</span>
-rw-r--r-- 1 sammy sammy  16M Dec 18  2016 image2.jpg
-rw-r--r-- 1 sammy sammy 7.0M Feb 18 16:59 <span class="highlight">image2.webp</span>
</code></pre>
The output of the <code>ls</code> command shows that the size of <code>image1.jpg</code> is 7.4M, while the size of <code>image1.webp</code> is 3.9M. The same goes for <code>image2.jpg</code> (16M) and <code>image2.webp</code> (7M). These files are almost half of their original size!

To save the complete, original data of images during compression, you can use the <code>-lossless</code>option in place of <code>-q</code>. This is the best option to maintain the quality of PNG images. To convert the downloaded PNG image from step 1, type:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">cwebp -lossless logo.png -o logo.webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
The following command shows that the lossless WebP image size (60K) is approximately half the size of the original PNG image (116K):
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">ls -lh logo.png logo.webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
<pre class="code-pre "><code></code></pre>
<div class="secondary-code-label " title="Output">Output</div>
<pre class="code-pre "><code>-rw-r--r-- 1 sammy sammy 116K Jul 18  2017 logo.png
-rw-r--r-- 1 sammy sammy  60K Feb 18 16:42 <span class="highlight">logo.webp</span>
</code></pre>
The converted WebP images in the <code>/var/www/html/webp</code> directory are about 50% smaller than their JPEG and PNG counterparts. In practice, compression rates can differ depending on certain factors: the compression rate of the original image, the file format, the type of conversion (lossy or lossless), the quality percentage, and your operating system. As you convert more images, you may see variations in conversion rates related to these factors.
<div data-unique="step-3-—-converting-jpeg-and-png-images-in-a-directory"></div>
<h2 id="step-3-—-converting-jpeg-and-png-images-in-a-directory">Step 3 — Converting JPEG and PNG Images in a Directory</h2>
Writing a script will simplify the conversion process by eliminating the work of manual conversion. We will now write a conversion script that finds JPEG files and converts them to WebP format with 90% quality, while also converting PNG files to lossless WebP images.

Using <code>nano</code> or your favorite editor, create the <code>webp-convert.sh</code> script in your user’s home directory:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">nano ~/webp-convert.sh</li>
</ul>
<pre class="code-pre command"><code></code></pre>
The first line of the script will look like this:
<div class="code-label " title="~/webp-convert.sh">~/webp-convert.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs ">find <span class="hljs-variable">$1</span> -type f -and \( -iname <span class="hljs-string">"*.jpg"</span> -o -iname <span class="hljs-string">"*.jpeg"</span> \)
</code></pre>
This line has the following components:
<ul>
 	<li><a href="https://www.digitalocean.com/community/tutorials/how-to-use-find-and-locate-to-search-for-files-on-a-linux-vps"><code>find</code></a>: This command will search for files within a specified directory.</li>
 	<li><code>$1</code>: This positional parameter specifies the path of the images directory, taken from the command line. Ultimately, it makes the location of the directory less dependent on the location of the script.</li>
 	<li><code>-type f</code>: This option tells <code>find</code> to look for regular files only.</li>
 	<li><code>-iname</code>: This test matches filenames against a specified pattern. The case-insensitive <code>-iname</code>test tells <code>find</code> to look for any filename that ends with <code>.jpg</code> (<code>*.jpg</code>) or <code>.jpeg</code> (<code>*.jpeg</code>).</li>
 	<li><code>-o</code>: This logical operator instructs the <code>find</code> command to list files that match the first <code>-iname</code>test (<code>-iname "*.jpg"</code>) <strong>or</strong> the second (<code>-iname "*.jpeg"</code>).</li>
 	<li><code>()</code>: Parentheses around these tests, along with the <code>-and</code> operator, ensure that the first test (i.e. <code>-type f</code>) is always executed.</li>
</ul>
The second line of the script will convert the images to WebP using the <code>-exec</code> parameter. The general syntax of this parameter is <code>-exec command {} \;</code>. The string <code>{}</code> is replaced by each file that the command iterates through, while the <code>;</code> tells <code>find</code> where the command ends:
<div class="code-label " title="~/webp-convert.sh">~/webp-convert.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs ">find <span class="hljs-variable">$1</span> -type f -and \( -iname <span class="hljs-string">"*.jpg"</span> -o -iname <span class="hljs-string">"*.jpeg"</span> \) \
-exec bash -c <span class="hljs-string">'commands'</span> {} \;
</code></pre>
In this case, the <code>-exec</code> parameter will require more than one command to search for and convert images:
<ul>
 	<li><code>bash</code>: This command will execute a small script that will make the <code>.webp</code> version of the file if it doesn’t exist. This script will get passed to <code>bash</code> as a string thanks to the <code>-c</code> option.</li>
 	<li><code>'commands'</code>: This placeholder is the script that will make <code>.webp</code> versions of your files.</li>
</ul>
The script inside <code>'commands'</code> will do the following things:
<ul>
 	<li>Create a <code>webp_path</code> variable.</li>
 	<li>Test whether or not the <code>.webp</code> version of the file exists.</li>
 	<li>Make the file if it does not exist.</li>
</ul>
The smaller script looks like this:
<div class="code-label " title="~/webp-convert.sh">~/webp-convert.sh</div>
<pre class="code-pre "><code>…
webp_path=$(sed 's/\.[^.]*$/.webp/' &lt;&lt;&lt; "$0");
if [ ! -f "$webp_path" ]; then 
  cwebp -quiet -q 90 "$0" -o "$webp_path";
fi;
</code></pre>
The elements in this smaller script include:
<ul>
 	<li><code>webp_path</code>: This variable will be generated using <a href="https://www.digitalocean.com/community/tutorials/the-basics-of-using-the-sed-stream-editor-to-manipulate-text-in-linux"><code>sed</code></a> and the matched file name from the <code>bash</code> command, denoted by the positional parameter <code>$0</code>. A <em>here string</em> (<code>&lt;&lt;&lt;</code>) will pass this name to <code>sed</code>.</li>
 	<li><code>if [ ! -f "$webp_path" ]</code>: This test will establish whether or not a file named <code>"$webp_path"</code>already exists, using the logical <code>not</code> operator (<code>!</code>).</li>
 	<li><code>cwebp</code>: This command will create the file if it doesn’t exist, using the <code>-q</code> option so as not to print output.</li>
</ul>
With this smaller script in place of the <code>'commands'</code> placeholder, the full script to convert JPEG images will now look like this:
<div class="code-label " title="~/webp-convert.sh">~/webp-convert.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs "><span class="hljs-comment"># converting JPEG images</span>
find <span class="hljs-variable">$1</span> -type f -and \( -iname <span class="hljs-string">"*.jpg"</span> -o -iname <span class="hljs-string">"*.jpeg"</span> \) \
-exec bash -c <span class="hljs-string">'
webp_path=$(sed '</span>s/\.[^.]*$/.webp/<span class="hljs-string">' &lt;&lt;&lt; "$0");
if [ ! -f "$webp_path" ]; then 
  cwebp -quiet -q 90 "$0" -o "$webp_path";
fi;'</span> {} \;
</code></pre>
To convert PNG images to WebP, we’ll take the same approach, with two differences: First, the <code>-iname</code> pattern in the <code>find</code> command will be <code>"*.png"</code>. Second, the conversion command will use the <code>-lossless</code> option instead of the quality <code>-q</code> option.

The completed script looks like this:
<div class="code-label " title="~/webp-convert.sh">~/webp-convert.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs "><span class="hljs-shebang">#!/bin/bash
</span>
<span class="hljs-comment"># converting JPEG images</span>
find <span class="hljs-variable">$1</span> -type f -and \( -iname <span class="hljs-string">"*.jpg"</span> -o -iname <span class="hljs-string">"*.jpeg"</span> \) \
-exec bash -c <span class="hljs-string">'
webp_path=$(sed '</span>s/\.[^.]*$/.webp/<span class="hljs-string">' &lt;&lt;&lt; "$0");
if [ ! -f "$webp_path" ]; then 
  cwebp -quiet -q 90 "$0" -o "$webp_path";
fi;'</span> {} \;

<span class="hljs-comment"># converting PNG images</span>
find <span class="hljs-variable">$1</span> -type f -and -iname <span class="hljs-string">"*.png"</span> \
-exec bash -c <span class="hljs-string">'
webp_path=$(sed '</span>s/\.[^.]*$/.webp/<span class="hljs-string">' &lt;&lt;&lt; "$0");
if [ ! -f "$webp_path" ]; then 
  cwebp -quiet -lossless "$0" -o "$webp_path";
fi;'</span> {} \;
</code></pre>
Save the file and exit the editor.

Next, let’s put the <code>webp-convert.sh</code> script into practice using the files in the <code>/var/www/html/webp</code>directory. Make sure that the script file is executable by running the following command:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">chmod a+x ~/webp-convert.sh</li>
</ul>
<pre class="code-pre command"><code></code></pre>
Run the script on the images directory:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">./webp-convert.sh /var/www/html/webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
Nothing happened! That’s because we already converted these images in step 2. Moving forward, the <code>webp-convert</code> script will convert images when we add new files or remove the <code>.webp</code> versions. To see how this works, delete the <code>.webp</code> files we created in step 2:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">rm /var/www/html/webp/*.webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
After deleting all of the <code>.webp</code> images, run the script again to make sure it works:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">./webp-convert.sh /var/www/html/webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
The <code>ls</code> command will confirm that the script has converted the images successfully:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">ls -lh /var/www/html/webp</li>
</ul>
<pre class="code-pre command"><code></code></pre>
<pre class="code-pre "><code></code></pre>
<div class="secondary-code-label " title="Output">Output</div>
<pre class="code-pre "><code>-rw-r--r-- 1 sammy sammy 7.4M Oct 28 23:36 image1.jpg
-rw-r--r-- 1 sammy sammy 3.9M Feb 18 16:46 <span class="highlight">image1.webp</span>
-rw-r--r-- 1 sammy sammy  16M Dec 18  2016 image2.jpg
-rw-r--r-- 1 sammy sammy 7.0M Feb 18 16:59 <span class="highlight">image2.webp</span>
-rw-r--r-- 1 sammy sammy 116K Jul 18  2017 logo.png
-rw-r--r-- 1 sammy sammy  60K Feb 18 16:42 <span class="highlight">logo.webp</span>
</code></pre>
The script in this step is the foundation of using WebP images in your site, as you will need a working version of all images in WebP format to serve to visitors. The next step will cover how to automate the conversion of new images.
<div data-unique="step-4-—-watching-image-files-in-a-directory"></div>
<h2 id="step-4-—-watching-image-files-in-a-directory">Step 4 — Watching Image Files in a Directory</h2>
In this step, we will create a new script to watch our images directory for changes and automatically convert newly created images.

Creating a script that watches our images directory can address certain issues with the <code>webp-convert.sh</code> script as it’s written. For example, this script will not identify if we have renamed an image. If we had an image called <code>foo.jpg</code>, ran <code>webp-convert.sh</code>, renamed that file <code>bar.jpg</code>, and then ran <code>webp-convert.sh</code> again, we would have duplicate <code>.webp</code> files (<code>foo.webp</code> and <code>bar.webp</code>). To solve this issue, and to avoid running the script manually, we will add <em>watchers</em> to another script. Watchers watch specified files or directories for changes and run commands in response to those changes.

The <code>inotifywait</code> command will set up watchers in our script. This command is part of the <code>inotify-tools</code> package, a set of command line tools that provide a simple interface to the inotify kernel subsystem. To install it on Ubuntu 16.04 type:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">sudo apt-get install inotify-tools</li>
</ul>
<pre class="code-pre command"><code></code></pre>
With CentOS 7, the <code>inotify-tools</code> package is available on the EPEL repository. Install the EPEL repository and <code>inotify-tools</code> package using the following commands:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">sudo yum install epel-release</li>
 	<li class="line">sudo yum install inotify-tools</li>
</ul>
<pre class="code-pre command"><code></code></pre>
Next, create the <code>webp-watchers.sh</code> script in your user’s home directory using <code>nano</code>:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">nano ~/webp-watchers.sh</li>
</ul>
<pre class="code-pre command"><code></code></pre>
The first line in the script will look like this:
<div class="code-label " title="~/webp-watchers.sh">~/webp-watchers.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs ">inotifywait -q -m -r --format <span class="hljs-string">'%e %w%f'</span> <span class="hljs-operator">-e</span> close_write <span class="hljs-operator">-e</span> moved_from <span class="hljs-operator">-e</span> moved_to <span class="hljs-operator">-e</span> delete <span class="hljs-variable">$1</span>
</code></pre>
This line includes the following elements:
<ul>
 	<li><code>inotifywait</code>: This command watches for changes to a certain directory.</li>
 	<li><code>-q</code>: This option will tell <code>inotifywait</code> to be quiet and not produce a lot of output.</li>
 	<li><code>-m</code>: This option will tell <code>inotifywait</code> to run indefinitely and not exit after receiving a single event.</li>
 	<li><code>-r</code>: This option will set up watchers recursively, watching a specified directory and all its sub-directories.</li>
 	<li><code>--format</code>: This option tells <code>inotifywait</code> to monitor changes using the event name followed by the file path. The events we want to monitor are <code>close_write</code> (triggered when a file is created and completely written to the disk), <code>moved_from</code> and <code>moved_to</code> (triggered when a file is moved), and <code>delete</code> (triggered when a file is deleted).</li>
 	<li><code>$1</code>: This positional parameter holds the path of the changed files.</li>
</ul>
Next, let’s add a <code>grep</code> command to establish whether or not our files are JPEG or PNG images. The <code>-i</code> option will tell <code>grep</code> to ignore case, <code>-E</code> will specify that <code>grep</code> should use extended regular expressions, and <code>--line-buffered</code> will tell <code>grep</code> to pass the matched lines to a <code>while</code> loop:
<div class="code-label " title="~/webp-watchers.sh">~/webp-watchers.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs ">inotifywait -q -m -r --format <span class="hljs-string">'%e %w%f'</span> <span class="hljs-operator">-e</span> close_write <span class="hljs-operator">-e</span> moved_from <span class="hljs-operator">-e</span> moved_to <span class="hljs-operator">-e</span> delete <span class="hljs-variable">$1</span> | grep -i -E <span class="hljs-string">'\.(jpe?g|png)$'</span> --line-buffered
</code></pre>
Next, we will build a <code>while</code> loop with the <code>read</code> command. <code>read</code> will process the event <code>inotifywait</code>has detected, assigning it to a variable called <code>$operation</code> and the processed file path to a variable named <code>$path</code>:
<div class="code-label " title="~/webp-watchers.sh">~/webp-watchers.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs ">…
| <span class="hljs-keyword">while</span> <span class="hljs-built_in">read</span> operation path; <span class="hljs-keyword">do</span>
  <span class="hljs-comment"># commands</span>
<span class="hljs-keyword">done</span>;
</code></pre>
Let’s combine this loop with the rest of our script:
<div class="code-label " title="~/webp-watchers.sh">~/webp-watchers.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs ">inotifywait -q -m -r --format <span class="hljs-string">'%e %w%f'</span> <span class="hljs-operator">-e</span> close_write <span class="hljs-operator">-e</span> moved_from <span class="hljs-operator">-e</span> moved_to <span class="hljs-operator">-e</span> delete <span class="hljs-variable">$1</span> \
| grep -i -E <span class="hljs-string">'\.(jpe?g|png)$'</span> --line-buffered \
| <span class="hljs-keyword">while</span> <span class="hljs-built_in">read</span> operation path; <span class="hljs-keyword">do</span>
  <span class="hljs-comment"># commands</span>
<span class="hljs-keyword">done</span>;
</code></pre>
After the <code>while</code> loop has checked the event, the commands inside the loop will take the following actions, depending on the result:
<ul>
 	<li>Create a new WebP file if a new image file was created or moved to the target directory.</li>
 	<li>Delete the WebP file if the associated image file was deleted or moved from the target directory.</li>
</ul>
There are three main sections inside the loop. A variable called <code>webp_path</code> will hold the path to the <code>.webp</code> version of the subject image:
<div class="code-label " title="~/webp-watchers.sh">~/webp-watchers.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs ">…
webp_path=<span class="hljs-string">"<span class="hljs-variable">$(sed 's/\.[^.]*$/.webp/' &lt;&lt;&lt; "$path")</span>"</span>;
</code></pre>
Next, the script will test which event has happened:
<div class="code-label " title="~/webp-watchers.sh">~/webp-watchers.sh</div>
<pre class="code-pre "><code>…
if [ $operation = "MOVED_FROM" ] || [ $operation = "DELETE" ]; then
  # commands to be executed if the file is moved or deleted
elif [ $operation = "CLOSE_WRITE,CLOSE" ] || [ $operation = "MOVED_TO" ]; then
  # commands to be executed if a new file is created
fi;
</code></pre>
If the file has been moved or deleted, the script will check if the <code>.webp</code> version exists. If it does, the script will remove it using <code>rm</code>:
<div class="code-label " title="~/webp-watchers.sh">~/webp-watchers.sh</div>
<pre class="code-pre "><code>…
if [ -f "$webp_path" ]; then
  $(rm -f "$webp_path");
fi;
</code></pre>
For newly created files, compression will happen as follows:
<ul>
 	<li>If the matched file is a PNG image, the script will use lossless compression.</li>
 	<li>If it’s not, the script will use lossy compression with the <code>-quality</code> option.</li>
</ul>
Let’s add the <code>cwebp</code> commands that will do this work to the script:
<div class="code-label " title="~/webp-watchers.sh">~/webp-watchers.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs ">…
<span class="hljs-keyword">if</span> [ $(grep -i <span class="hljs-string">'\.png$'</span> &lt;&lt;&lt; <span class="hljs-string">"<span class="hljs-variable">$path</span>"</span>) ]; <span class="hljs-keyword">then</span>
  $(cwebp -quiet -lossless <span class="hljs-string">"<span class="hljs-variable">$path</span>"</span> -o <span class="hljs-string">"<span class="hljs-variable">$webp_path</span>"</span>);
<span class="hljs-keyword">else</span>
  $(cwebp -quiet -q <span class="hljs-number">90</span> <span class="hljs-string">"<span class="hljs-variable">$path</span>"</span> -o <span class="hljs-string">"<span class="hljs-variable">$webp_path</span>"</span>);
<span class="hljs-keyword">fi</span>;
</code></pre>
In full, the <code>webp-watchers.sh</code> file will look like this:
<div class="code-label " title="~/webp-watchers.sh">~/webp-watchers.sh</div>
<pre class="code-pre "><code class="code-highlight language-bash hljs "><span class="hljs-shebang">#!/bin/bash</span>
<span class="hljs-built_in">echo</span> <span class="hljs-string">"Setting up watches."</span>;

<span class="hljs-comment"># watch for any created, moved, or deleted image files</span>
inotifywait -q -m -r --format <span class="hljs-string">'%e %w%f'</span> <span class="hljs-operator">-e</span> close_write <span class="hljs-operator">-e</span> moved_from <span class="hljs-operator">-e</span> moved_to <span class="hljs-operator">-e</span> delete <span class="hljs-variable">$1</span> \
| grep -i -E <span class="hljs-string">'\.(jpe?g|png)$'</span> --line-buffered \
| <span class="hljs-keyword">while</span> <span class="hljs-built_in">read</span> operation path; <span class="hljs-keyword">do</span>
  webp_path=<span class="hljs-string">"<span class="hljs-variable">$(sed 's/\.[^.]*$/.webp/' &lt;&lt;&lt; "$path")</span>"</span>;
  <span class="hljs-keyword">if</span> [ <span class="hljs-variable">$operation</span> = <span class="hljs-string">"MOVED_FROM"</span> ] || [ <span class="hljs-variable">$operation</span> = <span class="hljs-string">"DELETE"</span> ]; <span class="hljs-keyword">then</span> <span class="hljs-comment"># if the file is moved or deleted</span>
    <span class="hljs-keyword">if</span> [ <span class="hljs-operator">-f</span> <span class="hljs-string">"<span class="hljs-variable">$webp_path</span>"</span> ]; <span class="hljs-keyword">then</span>
      $(rm <span class="hljs-operator">-f</span> <span class="hljs-string">"<span class="hljs-variable">$webp_path</span>"</span>);
    <span class="hljs-keyword">fi</span>;
  <span class="hljs-keyword">elif</span> [ <span class="hljs-variable">$operation</span> = <span class="hljs-string">"CLOSE_WRITE,CLOSE"</span> ] || [ <span class="hljs-variable">$operation</span> = <span class="hljs-string">"MOVED_TO"</span> ]; <span class="hljs-keyword">then</span>  <span class="hljs-comment"># if new file is created</span>
     <span class="hljs-keyword">if</span> [ $(grep -i <span class="hljs-string">'\.png$'</span> &lt;&lt;&lt; <span class="hljs-string">"<span class="hljs-variable">$path</span>"</span>) ]; <span class="hljs-keyword">then</span>
       $(cwebp -quiet -lossless <span class="hljs-string">"<span class="hljs-variable">$path</span>"</span> -o <span class="hljs-string">"<span class="hljs-variable">$webp_path</span>"</span>);
     <span class="hljs-keyword">else</span>
       $(cwebp -quiet -q <span class="hljs-number">90</span> <span class="hljs-string">"<span class="hljs-variable">$path</span>"</span> -o <span class="hljs-string">"<span class="hljs-variable">$webp_path</span>"</span>);
     <span class="hljs-keyword">fi</span>;
  <span class="hljs-keyword">fi</span>;
<span class="hljs-keyword">done</span>;
</code></pre>
Save and close the file. Do not forget to make it executable:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">chmod a+x ~/webp-watchers.sh</li>
</ul>
<pre class="code-pre command"><code></code></pre>
Let’s run this script on the <code>/var/www/html/webp</code> directory in the background, using <code>&amp;</code>. Let’s also redirect standard output and standard error to an <code>~/output.log</code>, to store output in an readily available location:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">./webp-watchers.sh /var/www/html/webp &gt; output.log 2&gt;&amp;1 &amp;</li>
</ul>
<pre class="code-pre command"><code></code></pre>
At this point, you have converted the JPEG and PNG files in <code>/var/www/html/webp</code> to the WebP format, and set up watchers to do this work using the <code>webp-watchers.sh</code> script. It is now time to explore options to deliver WebP images to your website visitors.
<div data-unique="step-5-—-serving-webp-images-to-visitors-using-html-elements"></div>
<h2 id="step-5-—-serving-webp-images-to-visitors-using-html-elements">Step 5 — Serving WebP Images to Visitors Using HTML Elements</h2>
In this step, we will explain how to serve WebP images with HTML elements. At this point there should be <code>.webp</code> versions of each of the test JPEG and PNG images in the <code>/var/www/html/webp</code>directory. We can now serve them to supporting browsers using either HTML5 elements (<code>&lt;picture&gt;</code>) or the <code>mod_rewrite</code> Apache module. We’ll use HTML elements in this step.

The <code>&lt;picture&gt;</code> element allows you to include images directly in your web pages and to define more than one image source. If your browser supports the WebP format, it will download the <code>.webp</code> version of the file instead of the original one, resulting in web pages being served faster. It is worth mentioning that the <code>&lt;picture&gt;</code> element is well-supported in modern browsers that support the WebP format.

The <code>&lt;picture&gt;</code> element is a container with <code>&lt;source&gt;</code> and <code>&lt;img&gt;</code> elements that point to particular files. If we use <code>&lt;source&gt;</code> to point to a <code>.webp</code> image, the browser will see if it can handle it; otherwise, it will fall back to the image file specified in the <code>src</code> attribute in the <code>&lt;img&gt;</code> element.

Let’s use the <code>logo.png</code> file from our <code>/var/www/html/webp</code> directory, which we converted to <code>logo.webp</code>, as an example with <code>&lt;source&gt;</code>. We can use the following HTML code to display <code>logo.webp</code> to any browser that supports WebP format, and <code>logo.png</code> to any browser that does not support WebP or the <code>&lt;picture&gt;</code> element.

Create an HTML file located at <code>/var/www/html/webp/picture.html</code>:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">nano /var/www/html/webp/picture.html</li>
</ul>
<pre class="code-pre command"><code></code></pre>
Add the following code to the web page to display <code>logo.webp</code> to supporting browsers using the <code>&lt;picture&gt;</code> element:
<div class="code-label " title="/var/www/html/webp/picture.html">/var/www/html/webp/picture.html</div>
<pre class="code-pre "><code class="code-highlight language-html hljs "><span class="hljs-tag">&lt;<span class="hljs-title">picture</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-title">source</span> <span class="hljs-attribute">srcset</span>=<span class="hljs-value">"logo.webp"</span> <span class="hljs-attribute">type</span>=<span class="hljs-value">"image/webp"</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-title">img</span> <span class="hljs-attribute">src</span>=<span class="hljs-value">"logo.png"</span> <span class="hljs-attribute">alt</span>=<span class="hljs-value">"Site Logo"</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-title">picture</span>&gt;</span>
</code></pre>
Save and close the file.

To test that everything is working, navigate to <code>http://<span class="highlight">your_server_ip</span>/webp/picture.html</code>. You should see the test PNG image.

Now that you know how to serve <code>.webp</code> images directly from HTML code, let’s look at how to automate this process using Apache’s <code>mod_rewrite</code> module.
<div data-unique="step-6-—-serving-webp-images-using-mod_rewrite"></div>
<h2 id="step-6-—-serving-webp-images-using-mod_rewrite">Step 6 — Serving WebP Images Using mod_rewrite</h2>
If we want to optimize the speed of our site, but have a large number of pages or too little time to edit HTML code, then Apache’s <code>mod_rewrite</code> module can help us automate the process of serving <code>.webp</code> images to supporting browsers.

First, create an <code>.htaccess</code> file in the <code>/var/www/html/webp</code> directory using the following command:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">nano /var/www/html/webp/.htaccess</li>
</ul>
<pre class="code-pre command"><code></code></pre>
The <code>ifModule</code> directive will test if <code>mod_rewrite</code> is available; if it is, it can be activated by using <code>RewriteEngine On</code>. Add these directives to the <code>.htaccess</code>:
<div class="code-label " title="/var/www/html/webp/.htaccess">/var/www/html/webp/.htaccess</div>
<pre class="code-pre "><code class="code-highlight language-apache hljs "><span class="hljs-tag">&lt;ifModule mod_rewrite.c&gt;</span>
  <span class="hljs-keyword"><span class="hljs-common">RewriteEngine</span></span> <span class="hljs-literal">On</span> 
  <span class="hljs-comment"># further directives</span>
<span class="hljs-tag">&lt;/IfModule&gt;</span>
</code></pre>
The web server will make several tests to establish when to serve <code>.webp</code> images to the user. When a browser makes a request, it includes a header to indicate to the server what the browser is capable of handling. In the case of WebP, the browser will send an <code>Accept</code> header containing <code>image/webp</code>. We will check if the browser sent that header using <code>RewriteCond</code>, which specifies the criteria that should be matched in order to carry out the <code>RewriteRule</code>:
<div class="code-label " title="/var/www/html/webp/.htaccess">/var/www/html/webp/.htaccess</div>
<pre class="code-pre "><code class="code-highlight language-apache hljs ">…
<span class="hljs-keyword"><span class="hljs-common">RewriteCond</span></span> <span class="hljs-cbracket">%{HTTP_ACCEPT}</span> image/webp
</code></pre>
Everything should be filtered out but JPEG and PNG images. Using <code>RewriteCond</code> again, add a regular expression (similar to what we used in the previous sections) to match the requested URI:
<div class="code-label " title="/var/www/html/webp/.htaccess">/var/www/html/webp/.htaccess</div>
<pre class="code-pre "><code class="code-highlight language-apache hljs ">…
<span class="hljs-keyword"><span class="hljs-common">RewriteCond</span></span> <span class="hljs-cbracket">%{REQUEST_URI}</span>  (?i)(.*)(\.jpe?g|\.png)$ 
</code></pre>
The <code>(?i)</code> modifier will make the match case-insensitive.

To check if the <code>.webp</code> version of the file exists, use <code>RewriteCond</code> again as follows:
<div class="code-label " title="/var/www/html/webp/.htaccess">/var/www/html/webp/.htaccess</div>
<pre class="code-pre "><code class="code-highlight language-apache hljs ">…
<span class="hljs-keyword"><span class="hljs-common">RewriteCond</span></span> <span class="hljs-cbracket">%{DOCUMENT_ROOT}</span><span class="hljs-number">%1</span>.webp -f
</code></pre>
Finally, if all previous conditions were met, <code>RewriteRule</code> will redirect the requested JPEG or PNG file to its associated WebP file. Notice that this will <em>redirect</em> using the <code>-R</code> flag, rather than <em>rewrite</em>the URI. The difference between rewriting and redirecting is that the server will serve the rewritten URI without telling the browser. For example, the URI will show that the file extension is <code>.png</code>, but it will actually be a <code>.webp</code> file. Add <code>RewriteRule</code> to the file:
<div class="code-label " title="/var/www/html/webp/.htaccess">/var/www/html/webp/.htaccess</div>
<pre class="code-pre "><code class="code-highlight language-apache hljs ">…
<span class="hljs-keyword"><span class="hljs-common">RewriteRule</span></span> (?i)(.*)(\.jpe?g|\.png)$ <span class="hljs-number">%1</span>\.webp<span class="hljs-sqbracket"> [L,T=image/webp,R] 
</span></code></pre>
At this point, the <code>mod_rewrite</code> section in the <code>.htaccess</code> file is complete. But what will happen if there is an intermediate caching server between your server and the client? It could serve the wrong version to the end user. That is why it is worth checking to see if <code>mod_headers</code> is enabled, in order to send the <code>Vary: Accept</code> header. The <code>Vary</code> header indicates to caching servers (like proxy servers) that the content type of the document varies depending on the capabilities of the browser which requests the document. Moreover, the response will be generated based on the <code>Accept</code> header in the request. A request with a different <code>Accept</code> header might get a different response. This header is important because it prevents cached WebP images from being served to non-supporting browsers:
<div class="code-label " title="/var/www/html/webp/.htaccess">/var/www/html/webp/.htaccess</div>
<pre class="code-pre "><code class="code-highlight language-apache hljs ">…
<span class="hljs-tag">&lt;IfModule mod_headers.c&gt;</span>
  <span class="hljs-keyword"><span class="hljs-common">Header</span></span> append Vary Accept env=REDIRECT_accept
<span class="hljs-tag">&lt;/IfModule&gt;</span>
</code></pre>
Finally, at the end of the <code>.htaccess</code> file, set the MIME type of the <code>.webp</code> images to <code>image/webp</code> by using the <code>AddType</code> directive. This will serve the images using the right MIME type:
<div class="code-label " title="/var/www/html/webp/.htaccess">/var/www/html/webp/.htaccess</div>
<pre class="code-pre "><code class="code-highlight language-apache hljs ">…
<span class="hljs-keyword">AddType</span> image/webp .webp
</code></pre>
This is the final version of our <code>.htaccess</code> file:
<div class="code-label " title="/var/www/html/webp/.htaccess">/var/www/html/webp/.htaccess</div>
<pre class="code-pre "><code class="code-highlight language-apache hljs "><span class="hljs-tag">&lt;ifModule mod_rewrite.c&gt;</span>
  <span class="hljs-keyword"><span class="hljs-common">RewriteEngine</span></span> <span class="hljs-literal">On</span> 
  <span class="hljs-keyword"><span class="hljs-common">RewriteCond</span></span> <span class="hljs-cbracket">%{HTTP_ACCEPT}</span> image/webp
  <span class="hljs-keyword"><span class="hljs-common">RewriteCond</span></span> <span class="hljs-cbracket">%{REQUEST_URI}</span>  (?i)(.*)(\.jpe?g|\.png)$ 
  <span class="hljs-keyword"><span class="hljs-common">RewriteCond</span></span> <span class="hljs-cbracket">%{DOCUMENT_ROOT}</span><span class="hljs-number">%1</span>.webp -f
  <span class="hljs-keyword"><span class="hljs-common">RewriteRule</span></span> (?i)(.*)(\.jpe?g|\.png)$ <span class="hljs-number">%1</span>\.webp<span class="hljs-sqbracket"> [L,T=image/webp,R] 
&lt;/IfModule&gt;

&lt;IfModule mod_headers.c&gt;
  Header append Vary Accept env=REDIRECT_accept
&lt;/IfModule&gt;

AddType image/webp .webp
</span></code></pre>
<span class="note"><strong>Note</strong>: You can merge this <code>.htaccess</code> with another <code>.htaccess</code> file, if it exists. If you are using WordPress, for instance, you should copy this <code>.htaccess</code> file and paste it at the <strong>top</strong> of the existing file.
</span>

Let’s put what we have done in this step into practice. If you have followed the instructions in the previous steps, you should have <code>logo.png</code> and <code>logo.webp</code> images in <code>/var/www/html/webp</code>. Let’s use a simple <code>&lt;img&gt;</code> tag to include <code>logo.png</code> in our web page. Create a new HTML file to test the setup:
<pre class="code-pre command"><code></code></pre>
<ul class="prefixed">
 	<li class="line">nano /var/www/html/webp/img.html</li>
</ul>
<pre class="code-pre command"><code></code></pre>
Enter the following HTML code in the file:
<div class="code-label " title="/var/www/html/webp/img.html">/var/www/html/webp/img.html</div>
<pre class="code-pre "><code class="code-highlight language-html hljs "><span class="hljs-tag">&lt;<span class="hljs-title">img</span> <span class="hljs-attribute">src</span>=<span class="hljs-value">"logo.png"</span> <span class="hljs-attribute">alt</span>=<span class="hljs-value">"Site Logo"</span>&gt;</span>
</code></pre>
Save and close the file.

When you visit the web page using Chrome by visiting <code>http://<span class="highlight">your_server_ip</span>/webp/img.html</code>, you will notice that the served image is the <code>.webp</code> version (try opening the image in a new tab). If you use Firefox, you will get a <code>.png</code> image automatically.
<div data-unique="conclusion"></div>
<h2 id="conclusion">Conclusion</h2>
In this tutorial, we have covered basic techniques for working with WebP images. We have explained how to use <code>cwebp</code> to convert files, as well as two options to serve these images to users: HTML5’s <code>&lt;picture&gt;</code> element and Apache’s <code>mod_rewrite</code>.

In order to customize the scripts from this tutorial, you can look at some of these resources:
<ul>
 	<li>To learn more about the features of the WebP format and how to use the conversion tools, see the <a href="https://developers.google.com/speed/webp/">WebP documentation</a>.</li>
 	<li>To see more details about the usage of <code>&lt;picture&gt;</code> element, see its <a href="https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture">documentation on MDN</a>.</li>
 	<li>To fully understand how to use <code>mod_rewrite</code>, see its <a href="https://httpd.apache.org/docs/current/mod/mod_rewrite.html">documentation</a>.</li>
</ul>
Using the WebP format for your images will reduce file sizes by a considerable amount. This can lower bandwidth usage and make page loads faster, particularly if your web site uses a lot of images.

</div>
</div>
<div class="tutorial-footer">
<div class="tutorial-footer-details">
<div class="postable-info-bar-container">
<div class="postable-info-bar">
<div class="author-byline">
<div class="component-collaborators-container">
<ul class="component-collaborators-content">
 	<li class="collaborator-byline-avatar"><img class="alignnone size-full wp-image-3237" src="https://www.hss5.com/wp-content/uploads/2019/08/823ff3dade93c4461dc1b049558058d4.jpg" width="80" height="80" alt="abdullatif" /></li>
 	<li class="collaborators-byline-data">
<p class="names">By <a href="https://www.digitalocean.com/community/users/abdullatif">Abdullatif Eymash</a></p>
</li>
</ul>
</div>
https://www.digitalocean.com/community/tutorials/how-to-create-and-serve-webp-images-to-speed-up-your-website

<a href="https://m.do.co/c/3b013a1ebf2a">Buy Digital Ocean VPS</a>

&nbsp;

</div>
</div>
</div>
</div>
</div>