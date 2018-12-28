---
ID: 1540
post_title: >
  How To Crawl A Web Page with Scrapy and
  Python 3
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/11/29/how-to-crawl-a-web-page-with-scrapy-and-python-3/
published: true
post_date: 2018-11-29 18:53:12
---
<div class="tutorial-authors">
<div class="postable-info-bar-container">
<div class="author-and-date">
<div class="tutorial-author"><span style="color: inherit; font-size: 24px;">Introduction</span></div>
</div>
</div>
</div>
<div class="content-body tutorial-content" data-growable-markdown="">

Web scraping, often called web crawling or web spidering, or “programatically going over a collection of web pages and extracting data,” is a powerful tool for working with data on the web.

With a web scraper, you can mine data about a set of products, get a large corpus of text or quantitative data to play around with, get data from a site without an official API, or just satisfy your own personal curiosity.

In this tutorial, you’ll learn about the fundamentals of the scraping and spidering process as you explore a playful data set. We'll use <a href="http://brickset.com/">BrickSet</a>, a community-run site that contains information about LEGO sets. By the end of this tutorial, you’ll have a fully functional Python web scraper that walks through a series of pages on Brickset and extracts data about LEGO sets from each page, displaying the data to your screen.

The scraper will be easily expandable so you can tinker around with it and use it as a foundation for your own projects scraping data from the web.
<div data-unique="prerequisites"></div>
<h2 id="prerequisites">Prerequisites</h2>
To complete this tutorial, you'll need a local development environment for Python 3. You can follow <a href="https://www.digitalocean.com/community/tutorial_series/how-to-install-and-set-up-a-local-programming-environment-for-python-3">How To Install and Set Up a Local Programming Environment for Python 3 </a>to configure everything you need.
<div data-unique="step-1-—-creating-a-basic-scraper"></div>
<h2 id="step-1-—-creating-a-basic-scraper">Step 1 — Creating a Basic Scraper</h2>
Scraping is a two step process:
<ol>
 	<li>You systematically find and download web pages.</li>
 	<li>You take those web pages and extract information from them.</li>
</ol>
Both of those steps can be implemented in a number of ways in many languages.

You can build a scraper from scratch using <a href="https://www.digitalocean.com/community/tutorials/how-to-import-modules-in-python-3">modules</a> or libraries provided by your programming language, but then you have to deal with some potential headaches as your scraper grows more complex. For example, you'll need to handle concurrency so you can crawl more than one page at a time. You'll probably want to figure out how to transform your scraped data into different formats like CSV, XML, or JSON. And you'll sometimes have to deal with sites that require specific settings and access patterns.

You'll have better luck if you build your scraper on top of an existing library that handles those issues for you. For this tutorial, we're going to use Python and <a href="http://doc.scrapy.org/en/1.1/intro/overview.html">Scrapy</a> to build our scraper.

Scrapy is one of the most popular and powerful Python scraping libraries; it takes a "batteries included" approach to scraping, meaning that it handles a lot of the common functionality that all scrapers need so developers don't have to reinvent the wheel each time. It makes scraping a quick and fun process!

Scrapy, like most Python packages, is on PyPI (also known as <code>pip</code>). PyPI, the Python Package Index, is a community-owned repository of all published Python software.

If you have a Python installation like the one outlined in the prerequisite for this tutorial, you already have <code>pip</code> installed on your machine, so you can install Scrapy with the following command:

pip install scrapy

If you run into any issues with the installation, or you want to install Scrapy without using <code>pip</code>, check out the <a href="https://doc.scrapy.org/en/1.1/intro/install.html">official installation docs</a>.

&nbsp;

With Scrapy installed, let's create a new folder for our project. You can do this in the terminal by running:

mkdir brickset-scrape

&nbsp;

Now, navigate into the new directory you just created:

&nbsp;

cd brickset-scraper

&nbsp;

Then create a new Python file for our scraper called <code>scraper.py</code>. We'll place all of our code in this file for this tutorial. You can create this file in the terminal with the <code>touch</code> command, like this:

touch <span class="highlight">scraper.py</span>

Or you can create the file using your text editor or graphical file manager.

We'll start by making a very basic scraper that uses Scrapy as its foundation. To do that, we'll create a <a href="https://www.digitalocean.com/community/tutorials/how-to-construct-classes-and-define-objects-in-python-3">Python class</a> that subclasses <code>scrapy.Spider</code>, a basic spider class provided by Scrapy. This class will have two required attributes:
<ul>
 	<li><code>name</code> — just a name for the spider.</li>
 	<li><code>start_urls</code> — a <a href="https://www.digitalocean.com/community/tutorials/understanding-lists-in-python-3">list</a> of URLs that you start to crawl from. We'll start with one URL.</li>
</ul>
Open the <code>scrapy.py</code> file in your text editor and add this code to create the basic spider:
<div class="code-label " title="scraper.py">scraper.py</div>
<pre class="code-pre "><code>import scrapy


class BrickSetSpider(scrapy.Spider):
    name = "brickset_spider"
    start_urls = ['<span class="highlight">http://brickset.com/sets/year-2016</span>']
</code></pre>
Let's break this down line by line:

First, we <a href="https://www.digitalocean.com/community/tutorials/how-to-import-modules-in-python-3">import</a> <code>scrapy</code> so that we can use the classes that the package provides.

Next, we take the <code>Spider</code> class provided by Scrapy and make a <em>subclass</em> out of it called <code>BrickSetSpider</code>. Think of a subclass as a more specialized form of its parent class. The <code>Spider</code> subclass has methods and behaviors that define how to follow URLs and extract data from the pages it finds, but it doesn't know where to look or what data to look for. By subclassing it, we can give it that information.

Then we give the spider the name <code>brickset_spider</code>.

Finally, we give our scraper a single URL to start from: <a href="http://brickset.com/sets/year-2016">http://brickset.com/sets/year-2016</a>. If you open that URL in your browser, it will take you to a search results page, showing the first of many pages containing LEGO sets.

Now let's test out the scraper. You typically run Python files by running a command like <code>python path/to/file.py</code>. However, Scrapy comes with <a href="https://doc.scrapy.org/en/latest/topics/commands.html">its own command line interface</a> to streamline the process of starting a scraper. Start your scraper with the following command:
<pre class="code-pre command"><code></code></pre>
scrapy runspider <span class="highlight">scraper.py</span>
<pre class="code-pre command"><code></code></pre>
You’ll see something like this:
 
<div class="secondary-code-label " title="Output">Output</div>
<pre class="code-pre "><code>2016-09-22 23:37:45 [scrapy] INFO: Scrapy 1.1.2 started (bot: scrapybot)
2016-09-22 23:37:45 [scrapy] INFO: Overridden settings: {}
2016-09-22 23:37:45 [scrapy] INFO: Enabled extensions:
['scrapy.extensions.logstats.LogStats',
 'scrapy.extensions.telnet.TelnetConsole',
 'scrapy.extensions.corestats.CoreStats']
2016-09-22 23:37:45 [scrapy] INFO: Enabled downloader middlewares:
['scrapy.downloadermiddlewares.httpauth.HttpAuthMiddleware',
 ...
 'scrapy.downloadermiddlewares.stats.DownloaderStats']
2016-09-22 23:37:45 [scrapy] INFO: Enabled spider middlewares:
['scrapy.spidermiddlewares.httperror.HttpErrorMiddleware',
 ...
 'scrapy.spidermiddlewares.depth.DepthMiddleware']
2016-09-22 23:37:45 [scrapy] INFO: Enabled item pipelines:
[]
2016-09-22 23:37:45 [scrapy] INFO: Spider opened
2016-09-22 23:37:45 [scrapy] INFO: Crawled 0 pages (at 0 pages/min), scraped 0 items (at 0 items/min)
2016-09-22 23:37:45 [scrapy] DEBUG: Telnet console listening on 127.0.0.1:6023
2016-09-22 23:37:47 [scrapy] DEBUG: Crawled (200) &lt;GET http://brickset.com/sets/year-2016&gt; (referer: None)
2016-09-22 23:37:47 [scrapy] INFO: Closing spider (finished)
2016-09-22 23:37:47 [scrapy] INFO: Dumping Scrapy stats:
{'downloader/request_bytes': 224,
 'downloader/request_count': 1,
 ...
 'scheduler/enqueued/memory': 1,
 'start_time': datetime.datetime(2016, 9, 23, 6, 37, 45, 995167)}
2016-09-22 23:37:47 [scrapy] INFO: Spider closed (finished)
</code></pre>
That's a lot of output, so let's break it down.
<ul>
 	<li>The scraper initialized and loaded additional components and extensions it needed to handle reading data from URLs.</li>
 	<li>It used the URL we provided in the <code>start_urls</code> list and grabbed the HTML, just like your web browser would do.</li>
 	<li>It passed that HTML to the <code>parse</code> method, which doesn't do anything by default. Since we never wrote our own <code>parse</code> method, the spider just finishes without doing any work.</li>
</ul>
Now let's pull some data from the page.
<div data-unique="step-2-—-extracting-data-from-a-page"></div>
<h2 id="step-2-—-extracting-data-from-a-page">Step 2 — Extracting Data from a Page</h2>
We've created a very basic program that pulls down a page, but it doesn't do any scraping or spidering yet. Let’s give it some data to extract.

If you look at <a href="http://brickset.com/sets/year-2016">the page we want to scrape</a>, you'll see it has the following structure:
<ul>
 	<li>There's a header that’s present on every page.</li>
 	<li>There's some top-level search data, including the number of matches, what we’re searching for, and the breadcrumbs for the site.</li>
 	<li>Then there are the sets themselves, displayed in what looks like a table or ordered list. Each set has a similar format.</li>
</ul>
When writing a scraper, it's a good idea to look at the source of the HTML file and familiarize yourself with the structure. So here it is, with some things removed for readability:
 
<div class="secondary-code-label " title="brickset.com/sets/year-2016">brickset.com/sets/year-2016</div>
<pre class="code-pre "><code>&lt;body&gt;
  &lt;section class="setlist"&gt;
    <span class="highlight">&lt;article class='set'&gt;</span>
      &lt;a class="highslide plain mainimg" href=
      "http://images.brickset.com/sets/images/10251-1.jpg?201510121127"
      onclick="return hs.expand(this)"&gt;&lt;img src=
      "http://images.brickset.com/sets/small/10251-1.jpg?201510121127"
      title="10251-1: Brick Bank"&gt;&lt;/a&gt;
      &lt;div class="highslide-caption"&gt;
        &lt;h1&gt;&lt;a href='/sets/10251-1/Brick-Bank'&gt;Brick Bank&lt;/a&gt;&lt;/h1&gt;
        &lt;div class='tags floatleft'&gt;
          &lt;a href='/sets/10251-1/Brick-Bank'&gt;10251-1&lt;/a&gt; &lt;a href=
          '/sets/theme-Advanced-Models'&gt;Advanced Models&lt;/a&gt; &lt;a class=
          'subtheme' href=
          '/sets/theme-Advanced-Models/subtheme-Modular-Buildings'&gt;Modular
          Buildings&lt;/a&gt; &lt;a class='year' href=
          '/sets/theme-Advanced-Models/year-2016'&gt;2016&lt;/a&gt;
        &lt;/div&gt;
        &lt;div class='floatright'&gt;
          &amp;copy;2016 LEGO Group
        &lt;/div&gt;
        &lt;div class="pn"&gt;
          &lt;a href="#" onclick="return hs.previous(this)" title=
          "Previous (left arrow key)"&gt;&amp;#171; Previous&lt;/a&gt; &lt;a href="#"
          onclick="return hs.next(this)" title=
          "Next (right arrow key)"&gt;Next &amp;#187;&lt;/a&gt;
        &lt;/div&gt;
      &lt;/div&gt;
      ...
    <span class="highlight">&lt;/article&gt;</span>
    <span class="highlight">&lt;article class='set'&gt;</span>

      ...

    <span class="highlight">&lt;/article&gt;</span>
&lt;/section&gt;
&lt;/body&gt;
</code></pre>
Scraping this page is a two step process:
<ol>
 	<li>First, grab each LEGO set by looking for the parts of the page that have the data we want.</li>
 	<li>Then, for each set, grab the data we want from it by pulling the data out of the HTML tags.</li>
</ol>
<code>scrapy</code> grabs data based on <em>selectors</em> that you provide. Selectors are patterns we can use to find one or more elements on a page so we can then work with the data within the element. <code>scrapy</code> supports either CSS selectors or <a href="https://en.wikipedia.org/wiki/XPath">XPath</a> selectors.

We’ll use CSS selectors for now since CSS is the easier option and a perfect fit for finding all the sets on the page. If you look at the HTML for the page, you'll see that each set is specified with the class <code>set</code>. Since we're looking for a class, we'd use <code>.set</code> for our CSS selector. All we have to do is pass that selector into the <code>response</code> object, like this:
<div class="code-label " title="scraper.py">scraper.py</div>
<pre class="code-pre "><code>class BrickSetSpider(scrapy.Spider):
    name = "brickset_spider"
    start_urls = ['http://brickset.com/sets/year-2016']

    def parse(self, response):
        <span class="highlight">SET_SELECTOR = '.set'</span>
        <span class="highlight">for brickset in response.css(SET_SELECTOR):</span>
            <span class="highlight">pass</span>
</code></pre>
This code grabs all the sets on the page and loops over them to extract the data. Now let’s extract the data from those sets so we can display it.

Another look at the [source](view-source:brickset.com/sets/year-2016) of the page we're parsing tells us that the name of each set is stored within an <code>a</code> tag inside an <code>h1</code> tag for each set:

<div class="secondary-code-label " title="brickset.com/sets/year-2016">brickset.com/sets/year-2016</div>
<pre class="code-pre "><code>&lt;h1&gt;&lt;a href='/sets/10251-1/Brick-Bank'&gt;<span class="highlight">Brick Bank</span>&lt;/a&gt;&lt;/h1&gt;
</code></pre>
The <code>brickset</code> object we’re looping over has its own <code>css</code> method, so we can pass in a selector to locate child elements. Modify your code as follows to locate the name of the set and display it:
<div class="code-label " title="scraper.py">scraper.py</div>
<pre class="code-pre "><code>class BrickSetSpider(scrapy.Spider):
    name = "brickset_spider"
    start_urls = ['http://brickset.com/sets/year-2016']

    def parse(self, response):
        SET_SELECTOR = '.set'
        for brickset in response.css(SET_SELECTOR):

            <span class="highlight">NAME_SELECTOR = 'h1 a ::text'</span>
            <span class="highlight">yield {</span>
                <span class="highlight">'name': brickset.css(NAME_SELECTOR).extract_first(),</span>
            <span class="highlight">}</span>
</code></pre>
<span class="note"><strong>Note</strong>: The trailing comma after <code>extract_first()</code> isn't a typo. We're going to add more to this section soon, so we've left the comma there to make adding to this section easier later.
</span>

You’ll notice two things going on in this code:
<ul>
 	<li>We append <code>::text</code> to our selector for the name. That’s a CSS <em>pseudo-selector</em> that fetches the text <em>inside</em> of the <code>a</code> tag rather than the tag itself.</li>
 	<li>We call <code>extract_first()</code> on the object returned by <code>brickset.css(NAME_SELECTOR)</code> because we just want the first element that matches the selector. This gives us a <a href="https://www.digitalocean.com/community/tutorial_series/working-with-strings-in-python-3">string</a>, rather than a list of elements.</li>
</ul>
Save the file and run the scraper again:
<pre class="code-pre command"><code></code></pre>
scrapy runspider <span class="highlight">scraper.py</span>
<pre class="code-pre command"><code></code></pre>
This time you'll see the names of the sets appear in the output:
 
<div class="secondary-code-label " title="Output">Output</div>
<pre class="code-pre "><code>...
[scrapy] DEBUG: Scraped from &lt;200 http://brickset.com/sets/year-2016&gt;
<span class="highlight">{'name': 'Brick Bank'}</span>
[scrapy] DEBUG: Scraped from &lt;200 http://brickset.com/sets/year-2016&gt;
<span class="highlight">{'name': 'Volkswagen Beetle'}</span>
[scrapy] DEBUG: Scraped from &lt;200 http://brickset.com/sets/year-2016&gt;
<span class="highlight">{'name': 'Big Ben'}</span>
[scrapy] DEBUG: Scraped from &lt;200 http://brickset.com/sets/year-2016&gt;
<span class="highlight">{'name': 'Winter Holiday Train'}</span>
...

</code></pre>
Let's keep expanding on this by adding new selectors for images, pieces, and miniature figures, or <em>minifigs</em>that come with a set.

Take another look at the HTML for a specific set:
 
<div class="secondary-code-label " title="brickset.com/sets/year-2016">brickset.com/sets/year-2016</div>
<pre class="code-pre "><code>&lt;article class="set"&gt;
  &lt;a class="highslide plain mainimg" href="http://images.brickset.com/sets/images/10251-1.jpg?201510121127" onclick="return hs.expand(this)"&gt;
    <span class="highlight">&lt;img src="http://images.brickset.com/sets/small/10251-1.jpg?201510121127" title="10251-1: Brick Bank"</span>&gt;&lt;/a&gt;
  ...
  &lt;div class="meta"&gt;
    &lt;h1&gt;&lt;a href="/sets/10251-1/Brick-Bank"&gt;&lt;span&gt;10251:&lt;/span&gt; Brick Bank&lt;/a&gt; &lt;/h1&gt;
    ...
    &lt;div class="col"&gt;
      &lt;dl&gt;
        <span class="highlight">&lt;dt&gt;Pieces&lt;/dt&gt;</span>
        <span class="highlight">&lt;dd&gt;&lt;a class="plain" href="/inventories/10251-1"&gt;2380&lt;/a&gt;&lt;/dd&gt;</span>
        <span class="highlight">&lt;dt&gt;Minifigs&lt;/dt&gt;</span>
        <span class="highlight">&lt;dd&gt;&lt;a class="plain" href="/minifigs/inset-10251-1"&gt;5&lt;/a&gt;&lt;/dd&gt;</span>
        ...
      &lt;/dl&gt;
    &lt;/div&gt;
    ...
  &lt;/div&gt;
&lt;/article&gt;
</code></pre>
We can see a few things by examining this code:
<ul>
 	<li>The image for the set is stored in the <code>src</code> attribute of an <code>img</code> tag inside an <code>a</code> tag at the start of the set. We can use another CSS selector to fetch this value just like we did when we grabbed the name of each set.</li>
 	<li>Getting the number of pieces is a little trickier. There's a <code>dt</code> tag that contains the text <code>Pieces</code>, and then a <code>dd</code> tag that follows it which contains the actual number of pieces. We'll use <a href="https://en.wikipedia.org/wiki/XPath">XPath</a>, a query language for traversing XML, to grab this, because it's too complex to be represented using CSS selectors.</li>
 	<li>Getting the number of minifigs in a set is similar to getting the number of pieces. There's a <code>dt</code> tag that contains the text <code>Minifigs</code>, followed by a <code>dd</code> tag right after that with the number.</li>
</ul>
So, let's modify the scraper to get this new information:
<div class="code-label " title="scraper.py">scraper.py</div>
<pre class="code-pre "><code>class BrickSetSpider(scrapy.Spider):
    name = 'brick_spider'
    start_urls = ['http://brickset.com/sets/year-2016']

    def parse(self, response):
        SET_SELECTOR = '.set'
        for brickset in response.css(SET_SELECTOR):

            NAME_SELECTOR = 'h1 a ::text'
            <span class="highlight">PIECES_SELECTOR = './/dl[dt/text() = "Pieces"]/dd/a/text()'</span>
            <span class="highlight">MINIFIGS_SELECTOR = './/dl[dt/text() = "Minifigs"]/dd[2]/a/text()'</span>
            <span class="highlight">IMAGE_SELECTOR = 'img ::attr(src)'</span>
            yield {
                'name': brickset.css(NAME_SELECTOR).extract_first(),
                <span class="highlight">'pieces': brickset.xpath(PIECES_SELECTOR).extract_first(),</span>
                <span class="highlight">'minifigs': brickset.xpath(MINIFIGS_SELECTOR).extract_first(),</span>
                <span class="highlight">'image': brickset.css(IMAGE_SELECTOR).extract_first(),</span>
            }
</code></pre>
Save your changes and run the scraper again:

scrapy runspider <span class="highlight">scraper.py</span>

Now you’ll see that new data in the program's output:
<div class="secondary-code-label " title="Output">Output</div>
<pre class="code-pre "><code>2016-09-22 23:52:37 [scrapy] DEBUG: Scraped from &lt;200 http://brickset.com/sets/year-2016&gt;
<span class="highlight">{'minifigs': '5', 'pieces': '2380', 'name': 'Brick Bank', 'image': 'http://images.brickset.com/sets/small/10251-1.jpg?201510121127'}</span>
2016-09-22 23:52:37 [scrapy] DEBUG: Scraped from &lt;200 http://brickset.com/sets/year-2016&gt;
<span class="highlight">{'minifigs': None, 'pieces': '1167', 'name': 'Volkswagen Beetle', 'image': 'http://images.brickset.com/sets/small/10252-1.jpg?201606140214'}</span>
2016-09-22 23:52:37 [scrapy] DEBUG: Scraped from &lt;200 http://brickset.com/sets/year-2016&gt;
<span class="highlight">{'minifigs': None, 'pieces': '4163', 'name': 'Big Ben', 'image': 'http://images.brickset.com/sets/small/10253-1.jpg?201605190256'}</span>
2016-09-22 23:52:37 [scrapy] DEBUG: Scraped from &lt;200 http://brickset.com/sets/year-2016&gt;
<span class="highlight">{'minifigs': None, 'pieces': None, 'name': 'Winter Holiday Train', 'image': 'http://images.brickset.com/sets/small/10254-1.jpg?201608110306'}</span>
2016-09-22 23:52:37 [scrapy] DEBUG: Scraped from &lt;200 http://brickset.com/sets/year-2016&gt;
<span class="highlight">{'minifigs': None, 'pieces': None, 'name': 'XL Creative Brick Box', 'image': '/assets/images/misc/blankbox.gif'}</span>
2016-09-22 23:52:37 [scrapy] DEBUG: Scraped from &lt;200 http://brickset.com/sets/year-2016&gt;
<span class="highlight">{'minifigs': None, 'pieces': '583', 'name': 'Creative Building Set', 'image': 'http://images.brickset.com/sets/small/10702-1.jpg?201511230710'}</span>
</code></pre>
Now let's turn this scraper into a spider that follows links.
<div data-unique="step-3-—-crawling-multiple-pages"></div>
<h2 id="step-3-—-crawling-multiple-pages">Step 3 — Crawling Multiple Pages</h2>
We’ve successfully extracted data from that initial page, but we’re not progressing past it to see the rest of the results. The whole point of a spider is to detect and traverse links to other pages and grab data from those pages too.

You’ll notice that the top and bottom of each page has a little right carat (<code>&gt;</code>) that links to the next page of results. Here's the HTML for that:
 
<div class="secondary-code-label " title="brickset.com/sets/year-2016">brickset.com/sets/year-2016</div>
<pre class="code-pre "><code>&lt;ul class="pagelength"&gt;

  ...

  <span class="highlight">&lt;li class="next"&gt;</span>
    <span class="highlight">&lt;a href="http://brickset.com/sets/year-2017/page-2"&gt;&amp;#8250;&lt;/a&gt;</span>
  <span class="highlight">&lt;/li&gt;</span>
  &lt;li class="last"&gt;
    &lt;a href="http://brickset.com/sets/year-2016/page-32"&gt;&amp;#187;&lt;/a&gt;
  &lt;/li&gt;
&lt;/ul&gt;
</code></pre>
As you can see, there's a <code>li</code> tag with the class of <code>next</code>, and inside that tag, there's an <code>a</code> tag with a link to the next page. All we have to do is tell the scraper to follow that link if it exists.

Modify your code as follows:
<div class="code-label " title="scraper.py">scraper.py</div>
<pre class="code-pre "><code>class BrickSetSpider(scrapy.Spider):
    name = 'brick_spider'
    start_urls = ['http://brickset.com/sets/year-2016']

    def parse(self, response):
        SET_SELECTOR = '.set'
        for brickset in response.css(SET_SELECTOR):

            NAME_SELECTOR = 'h1 a ::text'
            PIECES_SELECTOR = './/dl[dt/text() = "Pieces"]/dd/a/text()'
            MINIFIGS_SELECTOR = './/dl[dt/text() = "Minifigs"]/dd[2]/a/text()'
            IMAGE_SELECTOR = 'img ::attr(src)'
            yield {
                'name': brickset.css(NAME_SELECTOR).extract_first(),
                'pieces': brickset.xpath(PIECES_SELECTOR).extract_first(),
                'minifigs': brickset.xpath(MINIFIGS_SELECTOR).extract_first(),
                'image': brickset.css(IMAGE_SELECTOR).extract_first(),
            }

        <span class="highlight">NEXT_PAGE_SELECTOR = '.next a ::attr(href)'</span>
        <span class="highlight">next_page = response.css(NEXT_PAGE_SELECTOR).extract_first()</span>
        <span class="highlight">if next_page:</span>
            <span class="highlight">yield scrapy.Request(</span>
                <span class="highlight">response.urljoin(next_page),</span>
                <span class="highlight">callback=self.parse</span>
            <span class="highlight">)</span>
</code></pre>
First, we define a selector for the "next page" link, extract the first match, and check if it exists. The <code>scrapy.Request</code> is a value that we return saying “Hey, crawl this page”, and <code>callback=self.parse</code> says “once you’ve gotten the HTML from this page, pass it back to this method so we can parse it, extract the data, and find the next page."

This means that once we go to the next page, we’ll look for a link to the next page there, and on that page we’ll look for a link to the next page, and so on, until we don't find a link for the next page. This is the key piece of web scraping: finding and following links. In this example, it’s very linear; one page has a link to the next page until we’ve hit the last page, But you could follow links to tags, or other search results, or any other URL you'd like.

Now, if you save your code and run the spider again you’ll see that it doesn't just stop once it iterates through the first page of sets. It keeps on going through all 779 matches on 23 pages! In the grand scheme of things it’s not a huge chunk of data, but now you know the process by which you automatically find new pages to scrape.

Here’s our completed code for this tutorial, using Python-specific highlighting:
<div class="code-label " title="scraper.py">scraper.py</div>
<pre class="code-pre "><code class="code-highlight language-python hljs "><span class="hljs-keyword">import</span> scrapy


<span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">BrickSetSpider</span><span class="hljs-params">(scrapy.Spider)</span>:</span>
    name = <span class="hljs-string">'brick_spider'</span>
    start_urls = [<span class="hljs-string">'http://brickset.com/sets/year-2016'</span>]

    <span class="hljs-function"><span class="hljs-keyword">def</span> <span class="hljs-title">parse</span><span class="hljs-params">(self, response)</span>:</span>
        SET_SELECTOR = <span class="hljs-string">'.set'</span>
        <span class="hljs-keyword">for</span> brickset <span class="hljs-keyword">in</span> response.css(SET_SELECTOR):

            NAME_SELECTOR = <span class="hljs-string">'h1 a ::text'</span>
            PIECES_SELECTOR = <span class="hljs-string">'.//dl[dt/text() = "Pieces"]/dd/a/text()'</span>
            MINIFIGS_SELECTOR = <span class="hljs-string">'.//dl[dt/text() = "Minifigs"]/dd[2]/a/text()'</span>
            IMAGE_SELECTOR = <span class="hljs-string">'img ::attr(src)'</span>
            <span class="hljs-keyword">yield</span> {
                <span class="hljs-string">'name'</span>: brickset.css(NAME_SELECTOR).extract_first(),
                <span class="hljs-string">'pieces'</span>: brickset.xpath(PIECES_SELECTOR).extract_first(),
                <span class="hljs-string">'minifigs'</span>: brickset.xpath(MINIFIGS_SELECTOR).extract_first(),
                <span class="hljs-string">'image'</span>: brickset.css(IMAGE_SELECTOR).extract_first(),
            }

        NEXT_PAGE_SELECTOR = <span class="hljs-string">'.next a ::attr(href)'</span>
        next_page = response.css(NEXT_PAGE_SELECTOR).extract_first()
        <span class="hljs-keyword">if</span> next_page:
            <span class="hljs-keyword">yield</span> scrapy.Request(
                response.urljoin(next_page),
                callback=self.parse
            )
</code></pre>
<div data-unique="conclusion"></div>
<h2 id="conclusion">Conclusion</h2>
In this tutorial you built a fully-functional spider that extracts data from web pages in less than thirty lines of code. That's a great start, but there’s a lot of fun things you can do with this spider. Here are some ways you could expand the code you've written. They'll give you some practice scraping data.
<ol>
 	<li>Right now we’re only parsing results from 2016, as you might have guessed from the <code>2016</code> part of <code>http://brickset.com/sets/year-2016</code> — how would you crawl results from other years?</li>
 	<li>There's a retail price included on most sets. How do you extract the data from that cell? How would you get a raw number out of it? <strong>Hint</strong>: you'll find the data in a <code>dt</code> just like the number of pieces and minifigs.</li>
 	<li>Most of the results have tags that specify semantic data about the sets or their context. How do we crawl these, given that there are multiple tags for a single set?</li>
</ol>
That should be enough to get you thinking and experimenting. If you need more information on Scrapy, check out <a href="https://scrapy.org/doc/">Scrapy’s official docs</a>. For more information on working with data from the web, see our tutorial on <a href="https://www.digitalocean.com/community/tutorials/how-to-scrape-web-pages-with-beautiful-soup-and-python-3">"How To Scrape Web Pages with Beautiful Soup and Python 3"</a>.

</div>