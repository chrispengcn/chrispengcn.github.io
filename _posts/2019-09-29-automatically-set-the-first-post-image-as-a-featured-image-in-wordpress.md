---
ID: 3252
post_title: >
  Automatically Set the First Post Image
  as a Featured Image in WordPress
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/09/29/automatically-set-the-first-post-image-as-a-featured-image-in-wordpress/
published: true
post_date: 2019-09-29 16:39:13
---
Have you ever run into the situation where you have a lot of posts without a featured image and you need to change that to be able to use <a href="https://www.gavick.com/wordpress-themes">WordPress Themes</a> which depend upon featured images in their layout? We’ve got a simple solution to this problem.

<img class="alignnone size-full wp-image-3253" src="https://www.hss5.com/wp-content/uploads/2019/09/function.png" width="804" height="266" alt="WordPress functions php file" />

Everyone likes to change up their WordPress site every once in a while, whether its by adding new plugins to expand the functionality, or getting a brand-new look with a fresh theme. Such changes don’t always go smoothly though; implementing a new plugin or theme can bring its own set of issues; for example, what if you make the jump to a new theme, but its layout relies upon featured images from posts to deliver its content while your previous theme did not? You could be left with hundreds of posts that don’t have a featured image assigned, which leaves you with the frustrating task of going through each post one-by-one and assigning a Featured Image via the Edit Post screen, wasting several hours in the process.

So what can you do if you’ve got a lot of posts without a featured image, and you need to change that ASAP so you can use your new featured-image-based theme? If you have a thing for tedium then you could of course do things manually, but we’ve got a much simpler solution that will take a lot of the sting out of this task.
<h2>The Featured Image generator function</h2>
The script below will be able to take care of your image problem, and all you’ll need to do is copy the code and add it into your theme’s <b>functions.php</b> file:
<div>
<div id="highlighter_444356" class="syntaxhighlighter gk-highligt plain">
<pre>function auto_featured_image() {
    global $post;
 
    if (!has_post_thumbnail($post-&gt;ID)) {
        $attached_image = get_children( "post_parent=$post-&gt;ID&amp;amp;post_type=attachment&amp;amp;post_mime_type=image&amp;amp;numberposts=1" );
         
      if ($attached_image) {
              foreach ($attached_image as $attachment_id =&gt; $attachment) {
                   set_post_thumbnail($post-&gt;ID, $attachment_id);
              }
         }
    }
}
// Use it temporary to generate all featured images
add_action('the_post', 'auto_featured_image');
// Used for new posts
add_action('save_post', 'auto_featured_image');
add_action('draft_to_publish', 'auto_featured_image');
add_action('new_to_publish', 'auto_featured_image');
add_action('pending_to_publish', 'auto_featured_image');
add_action('future_to_publish', 'auto_featured_image');</pre>
</div>
</div>
<h2>Inserting the function into your theme</h2>
Don’t worry if you’re not familiar with adding scripts to your WordPress theme’s files as it’s a very simple process. There are two main options for adding the script to the functions.php file; first, you can use FTP to connect to your server, download the functions file, make your changes and then reupload the file, replacing the original, or you can utilize the editor that is built into WordPress to access and change the file directly in the WordPress dashboard.
<h2>Using FTP</h2>
The FTP method might seem long-winded, but if you’re familiar with the concept and how to use it then it’s actually an extremely fast and accurate solution. If you’re not sure where to start with FTP, you can our <a href="https://www.gavick.com/blog/beginners-intro-ftp">Beginner’s Guide to FTP</a> blog article to get the lowdown on what it does and how. Once you’re ready to go, then just follow these basic steps:
<ol>
 	<li>Connect to your server via FTP and navigate to your current theme’s folder, which you’ll find in <b>wp-content → themes</b> in your WordPress installation. Make sure to open the correct folder for your theme, as every theme has its own functions.php file and adding the script to the wrong, inactive theme won’t do you any favours!</li>
 	<li>Download the existing functions.php file from the theme folder, and open it in the code/text editor of your choice (we like to use the <a href="https://www.gavick.com/blog/best-sublime-extensions-front-end-back-end-joomla-wordpress-development">Sublime Text code editor</a>, but whatever you’re comfortable with will do).</li>
 	<li>Copy the entire code provided above and paste it at the end of the functions.php file’s code. Save the changes.</li>
 	<li>With the file now modified accordingly, connect to your server via FTP once again and upload the updated file to the theme folder, replacing the original. You’re now ready to go!</li>
</ol>
<h2>Using the WordPress Editor</h2>
The WordPress Editor allows an admin to modify the core theme files in the dashboard without the need for FTP access; an elegant solution if you’re not comfortable with FTP and just need a quick solution. Making the changes with this method is very easy:
<ol>
 	<li>In the WordPress Dashboard, click on <b>Appearance → Editor</b> in the left menu to open the editor screen.</li>
 	<li>You’ll now be on the <b>Edit Themes</b> page for your current active theme; a list of theme files is on the right, and the code to be modified will appear in the main editor window. Under the <b>Templates</b>heading of the theme file list, look for the <b>functions.php</b> file and click on it to open it in the editor.</li>
 	<li>You can now see the code of the functions.php file in the main window of the editor; scroll down to the end of the file and then copy and paste the script above at the end of the existing code.</li>
 	<li>Now click on the <b>Update File</b> button under the editor to save changes; the code is now ready!</li>
</ol>
<h2>How the script works</h2>
So you’ve added the script to your functions file; now what is it actually going to <i>do</i>? It’s really very simple; whenever a post is viewed or a new post saved, the script checks the specific post to see if it has a featured image set. If not, then the script checks for others images in the post, grabs the first one it find, and sets this image as the featured image.

Something to be aware of here is that this instruction will be executed every time a post is displayed, which can have a slight performance hit on your website. For this reason, we recommend removing the following lines of code from the function once all of the featured images have been generated for existing articles:
<div>
<div id="highlighter_787973" class="syntaxhighlighter gk-highligt plain">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="plain plain">// Use it temporary to generate all featured images</code></div>
<div class="line number2 index1 alt1"><code class="plain plain">add_action('the_post', 'auto_featured_image');</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<h2>There are two other important takeaways from this:</h2>
The featured image must be removed from the media manager, and each image may only be automatically added as a featured image once. That means if you’ve already used a particular image as a featured image in a post, any future posts that use the same image will not be able to automatically use it as the featured image. For this reason we recommend using this solution only to migrate your older posts across to the updated format; once this is done and all earlier articles have been updated, the featured image can be manually set for any new posts as this method allows for duplicates if needed.