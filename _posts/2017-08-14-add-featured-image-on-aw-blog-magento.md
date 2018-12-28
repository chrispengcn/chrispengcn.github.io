---
ID: 967
post_title: Add Featured Image on AW-blog magento
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/08/14/add-featured-image-on-aw-blog-magento/
published: true
post_date: 2017-08-14 15:10:38
---
Bit of a late reply, but here's how to add a Featured Image field to a post:

In app/code/community/AW/Blog/Block/Manage/Blog/Edit/Form.php:

&nbsp;

Change:
<blockquote><code>$form = new Varien_Data_Form(array(
'id' =&gt; 'edit_form',
'action' =&gt; $this-&gt;getUrl('*/*/save', array('id' =&gt; $this-&gt;getRequest()-&gt;getParam('id'))),
'method' =&gt; 'post',
));</code></blockquote>
to:
<blockquote><code>$form = new Varien_Data_Form(array(
'id' =&gt; 'edit_form',
'action' =&gt; $this-&gt;getUrl('*/*/save', array('id' =&gt; $this-&gt;getRequest()-&gt;getParam('id'))),
'method' =&gt; 'post',
'enctype' =&gt; 'multipart/form-data'
));</code>

&nbsp;</blockquote>
In app/code/community/AW/Blog/Block/Manage/Blog/Edit/Tab/Form.php:

add:
<blockquote><code>$fieldset-&gt;addField('featured_image', 'image', array(
'name' =&gt; 'featured_image',
'label' =&gt; 'Featured Image'
));</code></blockquote>
Somewhere below the line:

&nbsp;

<code>$fieldset = $form-&gt;addFieldset('blog_form', array('legend' =&gt; Mage::helper('blog')-&gt;__('Post information')));</code>

In app/code/community/AW/Blog/controllers/Manage/BlogController.php:

add:
<blockquote><code>if(isset($_FILES['featured_image']['name']) and (file_exists($_FILES['featured_image']['tmp_name']))) {
try {
$uploader = new Varien_File_Uploader('featured_image');
$uploader-&gt;setAllowedExtensions(array('jpg','jpeg','gif','png'));
$uploader-&gt;setAllowRenameFiles(false);</code>

// setAllowRenameFiles(true) -&gt; move your file in a folder the magento way
// setAllowRenameFiles(true) -&gt; move your file directly in the $path folder
$uploader-&gt;setFilesDispersion(false);

$path = Mage::getBaseDir('media') . DS ;

$uploader-&gt;save($path, $_FILES['featured_image']['name']);

$data['featured_image'] = $_FILES['featured_image']['name'];
}catch(Exception $e) {

}
}

// handle delete image
else {
if(isset($data['featured_image']['delete']) &amp;&amp; $data['featured_image']['delete'] == 1)
$data['image_main'] = '';
else
unset($data['featured_image']);
}</blockquote>
below the line:
<blockquote><code>$model = Mage::getModel('blog/post');</code></blockquote>
&nbsp;

And finally, add a "featured_image" column to the aw_blog table in your database. Use the type VARCHAR(255) or something.

Now in your template files, you can access the featured image using the getFeaturedImage() method. For example in blog.phtml (the blog listing page) you could get the featured image for each post by putting the following code in the foreach loop:
<blockquote><code>&lt;?php echo Mage::getBaseUrl(Mage_Core_Model_Store::URL_TYPE_MEDIA).$post-&gt;getFeaturedImage() ?&gt;</code></blockquote>
Hi
Thanks for the code but I was also having issue with the featured image save in blog. I have checked and found that image is not being added to the blog data so its not added into database. I have made a small change and its fixed issue for me.

You just need to add below line
<blockquote>  $model-&gt;setFeaturedImage($_FILES['featured_image']['name']);</blockquote>
before
<blockquote>$model-&gt;save();</blockquote>
in app/code/community/AW/Blog/controllers/Manage/BlogController.php file.

Thanks

&nbsp;

https://forum.aheadworks.com/featured-image-t3821.html