---
ID: 1221
post_title: 'Magento GDPR 插件如何安装  Magento GDPR Compliance Guide'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/18/magento-gdpr-%e6%8f%92%e4%bb%b6%e5%a6%82%e4%bd%95%e5%ae%89%e8%a3%85-magento-gdpr-compliance-guide/'
published: true
post_date: 2018-05-18 18:37:59
---
<header>
<h1 class="ft-ptitle entry-title">Magento 2 GDPR Compliance Guide</h1>
</header>
<div class="ft-bmeta"><span class="ft-time"><time class="updated" datetime="2018-02-19T11:50:30+00:00">https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html</time></span></div>
<div class="ft-boxct"><section class="ft-entry"><img class="aligncenter size-full wp-image-19083" src="https://firebearstudio.com/blog/wp-content/uploads/2018/02/Magento-2-GDPR-Compliance-Guide-1.jpg" sizes="(max-width: 2267px) 100vw, 2267px" srcset="https://firebearstudio.com/blog/wp-content/uploads/2018/02/Magento-2-GDPR-Compliance-Guide-1.jpg 2267w, https://firebearstudio.com/blog/wp-content/uploads/2018/02/Magento-2-GDPR-Compliance-Guide-1-300x175.jpg 300w, https://firebearstudio.com/blog/wp-content/uploads/2018/02/Magento-2-GDPR-Compliance-Guide-1-768x448.jpg 768w, https://firebearstudio.com/blog/wp-content/uploads/2018/02/Magento-2-GDPR-Compliance-Guide-1-1024x597.jpg 1024w" alt="Magento 2 GDPR Integration" width="2267" height="1322" />In the following post, we shed light on such an important topic as <strong>Magento 2 GDPR compliance</strong>. The article describes what <strong>GDPR</strong> is, how to make your <a href="https://firebearstudio.com/blog/magento-2-e-commerce-trends-2018.html">e-commerce</a> store suitable for the new legislation, what are the deadlines, and what <strong>Magento 2 GDPR extensions</strong> and <strong>Magento 1 GDPR modules</strong> are currently available in the market. <span id="more-19081"></span>

GDPR stands for General Data Protection Regulation. It’s the EU’s new data protection legislation developed after four years of hard work. For instance, data usage in the UK was based on the 1995 EU Data Protection Directive until now. Is it normal to control something on the Internet in 2018 by rules created more than 20 years ago? How many things are entirely different now? The whole Internet became an entirely new dimension since that time, but the legislation of 1995 is still used. Luckily, it will be replaced by the new data protection legislation. Is GDPR good or bad for your business? Let’s try to figure out.

<iframe src="https://www.youtube.com/embed/1xy_afgALSI?feature=oembed" width="720" height="405" frameborder="0" allowfullscreen="allowfullscreen" data-mce-fragment="1"></iframe>

GDPR is good because it introduces stricter fines for non-compliance and breaches. Perhaps, it requires a new control system that will discover non-compliance and breaches more efficiently, but the push towards toughening will make the market better from the perspective of end users who will receive the new treatment concerning personal data security. How many people want to have a more secure Internet? Plenty of EU citizens dream about this, and soon their dreams will come true.

GDPR gives people more say over what companies can do with their data. And that’s a massive jump into the new more user-oriented and personalized experience for each individual. Besides, the new legislation makes data protection rules more or less identical throughout the EU requiring the same standard to be adopted by the Union. So if your Magento website is accessible in several EU countries, you no longer have to follow the unique data protection requirements of each of them since the legislation is going to be standardized.
<div id="toc_container" class="no_bullets">
<p class="toc_title">Table of contents</p>

<ul class="toc_list">
 	<li style="list-style-type: none;">
<ul>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#What_are_the_consequences_of_the_reformation"><span class="toc_number toc_depth_2">0.1</span> What are the consequences of the reformation? </a></li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#What_is_the_GDPR_deadline"><span class="toc_number toc_depth_2">0.2</span> What is the GDPR deadline? </a></li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#Do_I_need_to_abide_by_the_GDPR"><span class="toc_number toc_depth_2">0.3</span> Do I need to abide by the GDPR?</a></li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#What_about_foreign_businesses"><span class="toc_number toc_depth_2">0.4</span> What about foreign businesses?</a></li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#What_is_consent_and_how_to_get_it"><span class="toc_number toc_depth_2">0.5</span> What is consent and how to get it? </a></li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#What_about_data_storing"><span class="toc_number toc_depth_2">0.6</span> What about data storing? </a></li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#What_new_rights_do_people_get"><span class="toc_number toc_depth_2">0.7</span> What new rights do people get? </a></li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#What_else_should_controllers_do"><span class="toc_number toc_depth_2">0.8</span> What else should controllers do?</a></li>
</ul>
</li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#Magento_GDPR_Compliance_Extensions"><span class="toc_number toc_depth_1">1</span> Magento GDPR Compliance Extensions</a>
<ul>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#Magento_GDPR_Support_by_ZERO-1"><span class="toc_number toc_depth_2">1.1</span> Magento GDPR Support by ZERO-1</a></li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#AdFabConnect_Magento_2_GDPR_Compliance"><span class="toc_number toc_depth_2">1.2</span> AdFabConnect Magento 2 GDPR Compliance</a></li>
</ul>
</li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#Final_Words"><span class="toc_number toc_depth_1">2</span> Final Words</a></li>
 	<li><a href="https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#GDPR_Compliance_Useful_Links"><span class="toc_number toc_depth_1">3</span> GDPR Compliance Useful Links</a></li>
</ul>
</div>
<h3><span id="What_are_the_consequences_of_the_reformation">What are the consequences of the reformation? </span></h3>
As we’ve just mentioned, the new legislation was developed with end users in mind. The EU gives people more rights regarding their personal information. As a Magento store visitor, you get all the necessary instruments for controlling how your personal data is used. Moreover, think of Google and Facebook: does everyone likes how these giants interact with data? This situation will be changed soon!

And it’s generally because the current legislation was created long before the appearance of cloud services and all these insane algorithms that exploit data. Do you still remember articles dedicated to Trump presidential campaign and the <a href="https://www.cnbc.com/2017/09/15/cambridge-analytica-darren-bolding-says-donald-trump-facebook.html" rel="nofollow">usage of Facebook</a>? Some specialists claim that the new US president won the elections due to the use of the latest data processing technologies that analyze personal data of Facebook users. In other words, microtargeting paid a crucial role in his campaign.

Thus, microtargeting becomes a powerful instrument that can be used by politics. And you might have seen the right-winged movement rapidly gains popular in many EU countries. Can we consider GDPR the new instrument of political and social stability for the EU?

Unfortunately, we are non-experts in this area, so let’s return to the e-commerce aspect of the topic. While by strengthening data protection legislation, the EU wants to improve trust in the emerging digital economy, it also gives businesses a new clearer environment to operate in. Since all the countries get the same data protection law, this will save companies a lot of money. According to some estimates – approximately €2.3 billion a year collectively.
<h3><span id="What_is_the_GDPR_deadline">What is the GDPR deadline? </span></h3>
Unfortunately, you don’t have much time to make your business ready for the new data protection legislation. It comes into force on 25 May 2018. Despite the GDPR deadline, there are still many companies which haven’t start the modernization of their websites. The majority of security professionals know about GDPR, but more than half of them are not preparing for its arrival. If you are in the majority, please, keep reading the article, below, we shed light on implementing GDPR for Magento 2 and 1.
<h3><span id="Do_I_need_to_abide_by_the_GDPR">Do I need to abide by the GDPR?</span></h3>
There are two types of businesses which need to abide by the GDPR. They are data controllers that must state how and why the data is processed and data processors that must run the actual data processing according to the new standards. Any organization could be a data controller. The range is extensive from a profit-seeking business to a charity organization. As for a data processor – it is any IT firm doing the actual data processing. As a Magento merchant or an e-commerce store owner, you need to abide by the GDPR.

There is also one VERY IMPORTANT thing we must mention here. Even if your company is a controller and no data processing takes place in your office, you have the responsibility to ensure your processor abides by GDPR.
<h3><span id="What_about_foreign_businesses">What about foreign businesses?</span></h3>
If the company is situated outside the EU but operates with the data of EU residents, it is still necessary to comply with the new data processing standards. So, non-EU merchants, welcome to the game and get your online store prepared for the new data processing epoch!
<h3><span id="What_is_consent_and_how_to_get_it">What is consent and how to get it? </span></h3>
Pre-ticked boxes or opt-outs are no longer useful. Now, you must make your website visitors more educated. Consent is <b>active</b>, affirmative action by the visitor (data subject). Therefore, passive acceptance is considered a GDPR violation.

As a controller, you must keep a record of how and when an individual gave consent. Furthermore, any individual may withdraw their consent anytime. Seems like a nightmare especially for e-commerce stores with thousands of daily visitors, doesn’t it?

Also, note that the term ‘personal data’ has the new definition under the GDPR. Even IP addresses are considered personal data. Besides, the personally identifiable information includes economic, cultural, or mental health information.

Even pseudonymized personal data is a subject to the new legislation. And it is not a joke. Here at Firebear, we are also surprised, but we understand the importance of the new definition since it makes individual digital space safer allowing people feel more comfortable both online and offline.
<h3><span id="What_about_data_storing">What about data storing? </span></h3>
If your website stores personal data, you must be ready to provide customers who ask for access to their data with a response within a month. It is also necessary to be transparent about many new aspects of your business.
<ul>
 	<li>Firstly, you must inform visitors about how you collect the data.</li>
 	<li>Secondly, it is necessary to provide a description of what you do with it.</li>
 	<li>Thirdly, tell visitors how you process information about them.</li>
</ul>
Don’t forget to use plain language and avoid the usage of too complicated terms while describing the aspects mentioned above. Your clients should understand what you are talking about. If they don’t, it seems to be your problem according to GDPR.
<h3><span id="What_new_rights_do_people_get">What new rights do people get? </span></h3>
Below, you can see a list of new rights individuals get after GDPR comes into force.
<ul>
 	<li>Access to any information a company holds on individuals;</li>
 	<li>Right to know why that data is stored and processed;</li>
 	<li>Right to know how long it’s stored;</li>
 	<li>Right to know who has access to it;</li>
 	<li>Right to get direct access to review the stored data;</li>
 	<li>Right to ask for data correction if it is incorrect or incomplete;</li>
 	<li>Right to be forgotten: an individual can ask you to delete the data if it’s no longer necessary to the purpose for which it was initially collected.</li>
</ul>
According to the last condition, you, as a controller, is responsible for telling other organizations to delete any links to copies of that data. The copies must be deleted as well.
<h3><span id="What_else_should_controllers_do">What else should controllers do?</span></h3>
Two important aspects should be mentioned in our article. First of all, the data must be stored in common formats. For instance, you can entirely rely on CSV. Why is it necessary to do so? Because of the second aspect.

If an individual asks you to move data to another organization, you should do that. There is only one month to provide the specified organization with the data in commonly used format!

<iframe src="https://www.youtube.com/embed/9hh5EDUWPGo?feature=oembed" width="720" height="405" frameborder="0" allowfullscreen="allowfullscreen" data-mce-fragment="1"></iframe>
<h2><span id="Magento_GDPR_Compliance_Extensions">Magento GDPR Compliance Extensions</span></h2>
Now when you know some core aspects of GDPR and understand the influence of the new personal data protection legislation on your business, we’d like to draw your attention to several Magento GDPR extensions designed for the implementation of the new standards. Note that neither of the following modules provides the full integration.
<h3><span id="Magento_GDPR_Support_by_ZERO-1">Magento GDPR Support by ZERO-1</span></h3>
<img class="aligncenter size-full wp-image-19084" src="https://firebearstudio.com/blog/wp-content/uploads/2018/02/Zero-1-GDPR.jpg" alt="Magento 2 GDPR Compliance" width="270" height="260" />

If you still use Magento 1, then this is a must-have module for implementing GDPR on your website. The extension is totally free, and it is designed to provide such services as Cookie Compliance and Customer Data Anonymisation.

Besides, it adds multiple vital features to make Magento GDPR-compliant.  Unfortunately, the Magento platform doesn’t support the removal of customer data on request. But you will get this opportunity with the Magento GDPR extension by Zero-1.

Besides, you will be able to delete customer cart data quotes and customer order data for failed orders – the new laws require both procedures. And you will quickly avoid ALL non-essential cookies from operating UNTIL express consent has been granted.

Below, you can see an image of a customer account with new features. Under the Delete Account tab, a customer can view, edit, or delete his/her information following GDPR:

<img class="aligncenter size-full wp-image-19085" src="https://firebearstudio.com/blog/wp-content/uploads/2018/02/Zero-1-GDPR-Magento-account.jpg" sizes="(max-width: 918px) 100vw, 918px" srcset="https://firebearstudio.com/blog/wp-content/uploads/2018/02/Zero-1-GDPR-Magento-account.jpg 918w, https://firebearstudio.com/blog/wp-content/uploads/2018/02/Zero-1-GDPR-Magento-account-300x153.jpg 300w, https://firebearstudio.com/blog/wp-content/uploads/2018/02/Zero-1-GDPR-Magento-account-768x392.jpg 768w" alt="GDPR Magento Integration" width="918" height="468" />

For any further information, follow this link:

<strong><a href="https://marketplace.magento.com/zero1-zero1-gdpr.html" rel="nofollow">Download Magento GDPR Support Extension by ZERO-1</a></strong>
<h3><span id="AdFabConnect_Magento_2_GDPR_Compliance">AdFabConnect Magento 2 GDPR Compliance</span></h3>
<img class="aligncenter size-full wp-image-19086" src="https://firebearstudio.com/blog/wp-content/uploads/2018/02/AdFabConnect-Magento-2-GDPR-Compliance.jpg" sizes="(max-width: 813px) 100vw, 813px" srcset="https://firebearstudio.com/blog/wp-content/uploads/2018/02/AdFabConnect-Magento-2-GDPR-Compliance.jpg 813w, https://firebearstudio.com/blog/wp-content/uploads/2018/02/AdFabConnect-Magento-2-GDPR-Compliance-300x140.jpg 300w, https://firebearstudio.com/blog/wp-content/uploads/2018/02/AdFabConnect-Magento-2-GDPR-Compliance-768x358.jpg 768w" alt="" width="813" height="379" />

If you already use the Magento 2 store to run your e-commerce business, then it is necessary to install the Magento 2 GDPR compliance module by AdFabConnect. The extension will add all the essential tools and features that provide successful GDPR implementation for Magento 2.

It adds a new huge backend section under Configuration -&gt; Customers -&gt; Customer Configuration -&gt; Privacy (GDPR). Here, you can enable/disable all provided features, making your Magento 2 store GDPR-compliant. It is also necessary to mention that the Magento 2 GDPR extension does not affect any third-party modules, which can store personal data. Thus, the integration becomes more complex, since you should ask module providers for custom improvements.

Magento 2 GDPR Compliance by AdFabConnect is totally free, and you can get it here:

<strong><a href="https://github.com/AdFabConnect/magento2gdpr" rel="nofollow">Download Magento 2 GDPR Compliance Extension by AdFabConnect</a></strong>
<h2><span id="Final_Words">Final Words</span></h2>
Let’s summarise everything we’ve just said in the following steps:
<ul>
 	<li>Inform everyone in your organization that the law is changing to the GDPR. People should understand the impact of the new law.</li>
 	<li>Get ready to organize an information audit: everything should be documented and appropriately stored.</li>
 	<li>Review your privacy notices and adopt the Magento store to the new requirements.</li>
 	<li>Beware of the new rights individuals now have (we’ve just described them above).</li>
 	<li>Create a mechanism for providing data on a request within the new timescales.</li>
 	<li>Identify the lawful basis for processing activity and explain it via the updated privacy notice.</li>
 	<li>Don’t forget about the new way of getting consent (and the new meaning of consent).</li>
 	<li>Verify individuals’ ages and to obtain parental or guardian consent for data processing.</li>
 	<li>Provide procedures for detecting, reporting, and investigating personal data breaches.</li>
 	<li>Remember all your responsibilities related to third parties: data processing services and extensions you use in your e-commerce business.</li>
</ul>
<h2><span id="GDPR_Compliance_Useful_Links">GDPR Compliance Useful Links</span></h2>
For any further information about GDPR, check the following links:
<ul>
 	<li><a href="http://www.itpro.co.uk/it-legislation/27814/what-is-gdpr-everything-you-need-to-know" rel="nofollow">What is GDPR? Everything you need to know before the 2018 deadline</a></li>
 	<li><a href="http://www.wired.co.uk/article/what-is-gdpr-uk-eu-legislation-compliance-summary-fines-2018" rel="nofollow">What is GDPR? by WIRED</a></li>
 	<li><a href="https://www.thepixel.com/gdpr-magento-merchants/" rel="nofollow">What does GDPR mean for Magento merchants?</a></li>
</ul>
</section></div>