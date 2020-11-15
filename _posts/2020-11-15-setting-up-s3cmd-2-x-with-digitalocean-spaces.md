---
ID: 3798
post_title: >
  Setting Up s3cmd 2.x with DigitalOcean
  Spaces
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://vpseo.com/2020/11/15/setting-up-s3cmd-2-x-with-digitalocean-spaces/
published: true
post_date: 2020-11-15 04:23:15
---
<h1>Setting Up s3cmd 2.x with DigitalOcean Spaces</h1>
https://www.digitalocean.com/docs/spaces/resources/s3cmd/
<div class="article-meta article-meta--header">Validated on 19 April 2019 • Posted on 19 April 2019</div>
&nbsp;

Spaces is an S3-compatible object storage service that lets you store and serve large amounts of data. Each Space is a bucket for you to store and serve files. The free, built-in Spaces CDN minimizes page load times, improves performance, and reduces bandwidth and infrastructure costs.

&nbsp;

<nav class="section-nav">
<ul>
 	<li><a href="https://www.digitalocean.com/docs/spaces/">Overview</a></li>
 	<li><a href="https://www.digitalocean.com/docs/spaces/quickstart/">Quickstart</a></li>
 	<li><a href="https://www.digitalocean.com/docs/spaces/how-to/">How-To</a></li>
 	<li class="current"><a href="https://www.digitalocean.com/docs/spaces/resources/">Resources</a></li>
</ul>
</nav>s3cmd is a popular cross-platform command-line tool for managing S3 and S3-compatible object stores.

To use s3cmd with DigitalOcean Spaces, you need:
<ul>
 	<li><strong>s3cmd version 2.0.0+ or higher.</strong> You can check your version with <code>s3cmd --version</code>. Versions from package managers may be out of date, so we recommend using the <a href="http://s3tools.org/download">s3cmd download page</a>. <a href="https://brew.sh/">Homebrew</a> users can install the latest version with the command <code>brew install s3cmd</code>.</li>
 	<li><strong>An <a href="https://www.digitalocean.com/docs/spaces/how-to/manage-access/#access-keys">access key</a> pair for your Spaces.</strong> To generate these, visit the <a href="https://cloud.digitalocean.com/settings/api/tokens">API page</a> in the DigitalOcean Control Panel.</li>
</ul>
<h2 id="initialize-the-configuration-file">Initialize the Configuration File<i class="fa fa-link fa-lg"></i></h2>
By default, s3cmd stores its configuration file, <code>.s3cfg</code>, in the home directory of the user that ran the configuration command. <code>.s3cfg</code> is a plain text file of key/value pairs which can be edited directly once it has been created.

s3cmd uses the options set in its default configuration file when you run commands. You can specify a different configuration by appending <code>-c ~/path/to/config/file</code> to each command you run.

If DigitalOcean is the main or only provider you'll connect to with <code>s3cmd</code> and you don't want to specify its configuration file every time you use s3cmd, configure the default <code>~/.s3cfg</code> file with the following command:
<div class="code-toolbar">
<pre class=" language-text"><code class=" language-text">s3cmd --configure</code></pre>
<div class="toolbar">
<div class="toolbar-item"><a>Copy</a></div>
</div>
</div>
If you're already using s3cmd with another service, you can create an alternate configuration file by adding the <code>-c</code> flag and supplying a filename. The configuration file will be created in the directory where you issue the command, so specify the path if you want it created elsewhere.
<h2 id="enter-access-keys">Enter Access Keys<i class="fa fa-link fa-lg"></i></h2>
The script begins by asking for an Access Key and Secret Key. If you don't already have keys, you can generate a set for s3cmd by visiting the control panel's <a href="https://cloud.digitalocean.com/settings/api/tokens">API page</a>.

Enter your keys, then accept <code>US</code> for the <strong>Default Region</strong> because the region information isn't relevant to DigitalOcean. If you prefer, you can use the environment variables <code>AWS_ACCESS_KEY_ID</code> <code>AWS_SECRET_ACCESS_KEY</code> to store a set of keys.
<div class="code-toolbar">
<pre class=" language-text" data-line="4,5"><code class=" language-text">Enter new values or accept defaults in brackets with Enter.
Refer to user manual for detailed description of all options.
Access key and Secret key are your identifiers for Amazon S3. Leave them empty for using the env variables.
Access Key []: EXAMPLE7UQOTHDTF3GK4
Secret Key []: exampleb8e1ec97b97bff326955375c5
Default Region [US]:</code> Enter the DigitalOcean Endpoint</pre>
</div>
<h2 id="enter-the-digitalocean-endpoint"></h2>
Next, enter the DigitalOcean Spaces endpoint. The Spaces endpoint naming pattern is <code>&lt;region&gt;.digitaloceanspaces.com</code>, like <code>nyc3.digitaloceanspaces.com</code>. Use the endpoint for the region your Spaces are in.
<div class="code-toolbar">
<pre class=" language-text" data-line="2"><code class=" language-text">Use "s3.amazonaws.com" for S3 Endpoint and not modify it to the target Amazon S3.
S3 Endpoint [s3.amazonaws.com]: nyc3.digitaloceanspaces.com</code></pre>
</div>
The next prompt asks for a URL template to access your bucket, which is the S3 equivalent of a Space. Because Spaces supports <a href="https://www.digitalocean.com/docs/spaces/#features">DNS-based endpoint URLs</a>, you can use the variable <code>%(bucket)s</code> to stand in for the name of your space. Enter the following template format exactly as written: <code>%(bucket)s.nyc3.digitaloceanspaces.com</code>. Again, you will change this if your Space is in a different region.
<div class="code-toolbar">
<pre class=" language-text" data-line="3"><code class=" language-text">Use "%(bucket)s.s3.amazonaws.com" to the target Amazon S3. "%(bucket)s" and "%(location)s" vars c
an be used if the target S3 system supports dns based buckets.
DNS-style bucket+hostname:port template for accessing a bucket []: %(bucket)s.nyc3.digitaloceanspaces.com</code></pre>
</div>
<h2 id="optional-set-an-encryption-password">Optional: Set an Encryption Password<i class="fa fa-link fa-lg"></i></h2>
The next prompt is for an optional encryption password. Unlike HTTPS, which protects files only while in transit, GPG encryption prevents others from reading files both in transit and while they are stored on DigitalOcean. Setting a password now won't cause objects to be automatically encrypted; it just makes encryption available later.
<div class="code-toolbar">
<pre class=" language-text" data-line="3"><code class=" language-text">Encryption password is used to protect your files from reading
by unauthorized persons while in transfer to S3
Encryption password:</code></pre>
</div>
The next prompt asks for the path to the GPG program. On Linux, you can accept the default by pressing <code>ENTER</code>. If you're following these instructions on macOS, you may have to install GPG with a tool like <a href="https://brew.sh/">Homebrew</a> (<code>brew install gpg</code>). You can then find GPG's path with <code>which gpg</code>.
<div class="code-toolbar">
<pre class=" language-text" data-line="1"><code class=" language-text">Path to GPG program [/usr/bin/gpg]:</code></pre>
</div>
<h2 id="connect-via-https">Connect via HTTPS<i class="fa fa-link fa-lg"></i></h2>
The next prompt asks to use the HTTPS protocol. HTTPS protects data from being read while it's in transit.

DigitalOcean Spaces don't support unencrypted transfer, so you must use HTTPS. Press <code>ENTER</code> to accept the default.
<div class="code-toolbar">
<pre class=" language-text" data-line="4"><code class=" language-text">When using secure HTTPS protocol all communication with Amazon S3
servers is protected from 3rd party eavesdropping. This method is
slower than plain HTTP, and can only be proxied with Python 2.7 or newer
Use HTTPS protocol [Yes]: Yes</code></pre>
</div>
<h2 id="optional-set-a-proxy-server">Optional: Set a Proxy Server<i class="fa fa-link fa-lg"></i></h2>
The final prompt is for an HTTP proxy server. If your network requires one, enter its IP address or domain name without the protocol, e.g. <code>203.0.113.1 </code>or <code>proxy.example.com</code>. If you aren't using one, press <code>ENTER</code> to leave it blank.
<div class="code-toolbar">
<pre class=" language-text" data-line="3"><code class=" language-text">On some networks all internet access must go through a HTTP proxy.
Try setting it here if you can't connect to S3 directly
HTTP Proxy server name:</code></pre>
</div>
<h2 id="confirm-test-and-save-settings">Confirm, Test, and Save Settings<i class="fa fa-link fa-lg"></i></h2>
After the prompt for the HTTP Proxy server name, the configuration script presents a summary of the values it will use, followed by the opportunity to test them:
<div class="code-toolbar">
<pre class=" language-text" data-line="14"><code class=" language-text">New settings:
 Access Key: EXAMPLES7UQOTHDTF3GK4
 Secret Key: b8e1ec97b97bff326955375c5example
 Default Region: US
 S3 Endpoint: nyc3.digitaloceanspaces.com
 DNS-style bucket+hostname:port template for accessing a bucket: %(bucket)s.n
yc3.digitaloceanspaces.com
 Encryption password: secure_password
 Path to GPG program: /usr/bin/gpg
 Use HTTPS protocol: True
 HTTP Proxy server name:
 HTTP Proxy server port: 0

Test access with supplied credentials? [Y/n] Y</code></pre>
<div class=" line-highlight" aria-hidden="true" data-range="14" data-start="14"></div>
</div>
When the test completes successfully, enter <code>Y</code> to save the settings:
<div class="code-toolbar">
<pre class=" language-text" data-line="7"><code class=" language-text">Please wait, attempting to list all buckets...
Success. Your access key and secret key worked fine :-)

Now verifying that encryption works...
Success. Encryption and decryption worked fine :-)

Save settings? [y/N] Y</code></pre>
<div class=" line-highlight" aria-hidden="true" data-range="7" data-start="7"></div>
</div>
If the test fails or you choose <code>N</code> you'll have the opportunity to retry the configuration. Once you save the configuration, you'll receive confirmation of its location:
<div class="code-toolbar">
<pre class=" language-text"><code class=" language-text">Configuration saved to '/home/sammy/nyc3'</code></pre>
</div>
<h2 id="more-information">More Information<i class="fa fa-link fa-lg"></i></h2>
You can use our <a href="https://www.digitalocean.com/docs/spaces/resources/s3cmd-usage/">quick reference on s3cmd usage</a> to get started. For a comprehensive guide to s3cmd, see the <a href="http://s3tools.org/usage">s3cmd usage guide</a> or access the help file from the command line with <code>s3cmd --help</code>.