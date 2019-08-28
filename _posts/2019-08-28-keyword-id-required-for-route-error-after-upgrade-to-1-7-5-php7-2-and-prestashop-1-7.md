---
ID: 3233
post_title: 'Keyword &#8220;{id}&#8221; required for route error after upgrade to 1.7.5 &#8211; php7.2 and prestashop 1.7'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/08/28/keyword-id-required-for-route-error-after-upgrade-to-1-7-5-php7-2-and-prestashop-1-7/
published: true
post_date: 2019-08-28 16:34:15
---
After upgrading from 1.7.3.1 to 1.7.5.0 (latest) i cant save the SEO & URLs page (to recreate .htaccess), i always get this errors:

Keyword "{id}" required for route "xipblog-archive-wid-module" (rule: "blog/category/{rewrite}.html")
Keyword "{id}" required for route "xipblog-tag-wid-module" (rule: "blog/tag/{rewrite}.html")
Keyword "{id}" required for route "xipblog-single-wid-module" (rule: "blog/post/{rewrite}.html")

See atached screenshoot's.

Any ideas how to solve this?

<img class="alignnone size-full wp-image-3234" src="https://www.hss5.com/wp-content/uploads/2019/08/50688966-1cd57c00-1028-11e9-96cb-a2d4c23fbc98.jpg" width="1280" height="481" />

Ok i think i found the solution.
And yes is an theme error/bug/feature.
What i did:
Went to www.domain.com\modules\xipblog (our theme blog module).
Edited xipblog.php.
For example i searched for xipblog-single-wid-module.
and in that array i changed:
'rule' =>$main_slug.'/'.$single_slug.'/{rewrite}'.$postfix_slug,
to
'rule' =>$main_slug.'/'.$single_slug.'/{id}-{rewrite}'.$postfix_slug,
No more errors on SEO & URLs page and it get saved ok.
Thanks to all for helping. Thanks @khouloudbelguith , thanks @rdy4ever
If anyone nedds more info don't hesitate to contact me.
Can be marked as solved.

https://github.com/PrestaShop/PrestaShop/issues/12026