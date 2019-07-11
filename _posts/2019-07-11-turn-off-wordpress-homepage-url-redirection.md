---
ID: 2813
post_title: >
  Turn Off WordPress Homepage URL
  Redirection
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://www.hss5.com/2019/07/11/turn-off-wordpress-homepage-url-redirection/
published: true
post_date: 2019-07-11 23:41:03
---
If you are integrating WordPress into a preexisting site that already has its own homepage, or if you are developing a new WordPress website that is hidden behind a <em>Coming Soon</em> page, you will run into one frustrating problem. If you try to access the WordPress installation by visiting the <strong>index.php</strong> file, you won’t be able to see it. Instead, WordPress will automatically redirect you from the index.php page to the blog address url, as defined in your WordPress site settings.

There are, of course, several ways to get around this problem. Two common ways include:
<ol>
 	<li>developing the WordPress site in its own directory</li>
 	<li>integrating the preexisting site or <em>coming soon</em> page within the WordPress site</li>
</ol>
Unfortunately, these methods can result in quite a bit of extra work. Fortunately, it is easy to <strong>stop WordPress’ automatic redirects</strong>. But first, it is a good idea to know why WordPress employs these redirects.
<h2>Why WordPress Has Automatic <acronym title="Uniform Resource Locator">URL</acronym> Redirects</h2>
A page on any website, WordPress or not, can be accessed by multiple urls. For example, you can typically visit the home page of a WordPress web site by all of the following urls:
<ul>
 	<li>http://example.com/</li>
 	<li>http://www.example.com/</li>
 	<li>http://example.com/index.php</li>
 	<li>http://www.example.com/index.php</li>
</ul>
The problem with allowing all of these ways to access a single page is that it can potentially hurt your website’s overall <strong>search engine optimization</strong> (<acronym title="Search Engine Optimization">SEO</acronym>). Having multiple urls for a page means that search engines could index duplicate copies. So WordPress fixes this problem by employing <em>automatic redirects</em> known as <strong>Canonical <acronym title="Uniform Resource Locator">URL</acronym>Redirection</strong>, which only enables one url per page.<em>
</em>
<h2>How To Turn Off Canonical <acronym title="Uniform Resource Locator">URL</acronym> Redirection</h2>
To turn off Canonical <acronym title="Uniform Resource Locator">URL</acronym> Redirection, you can add the following code to your theme’s functions.php file.

<code>remove_filter('template_redirect','redirect_canonical');</code>

Not comfortable altering your theme files? <acronym title="WordPress">WP</acronym> developer Mark Jaquith has placed this code in his <a href="http://txfx.net/files/wordpress/disable-canonical-redirects.phps" target="_blank" rel="nofollow noopener noreferrer">Disable Canonical <acronym title="Uniform Resource Locator">URL</acronym> Redirection plugin</a>.

&nbsp;

To learn more about the introduction of Canonical URLs, see <a href="http://codex.wordpress.org/Migrating_Plugins_and_Themes_to_2.3#Canonical_URLs" target="_blank" rel="nofollow noopener noreferrer">Migrating Plugins and Themes to 2.3</a> in the WordPress Codex.