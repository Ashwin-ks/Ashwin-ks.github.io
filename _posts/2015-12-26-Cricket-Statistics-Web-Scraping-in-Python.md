---
layout: post
title: Python Web Scraping - Cricket Player Stats
subtitle: Using Python to scrape web page for information on cricket player statistics in 2015 World Cup event and storing the fetched information in a database for further querying/analysis.
bigimg: /img/cricket.png
---

## Cricket Statistics WebScraping using Python


```python
import requests
from bs4 import BeautifulSoup
import re
import csv
import sqlite3
from IPython.display import display,HTML
```


```python
url='http://www.espncricinfo.com/icc-cricket-world-cup-2015/content/current/series/509587.html'
```


```python
page=requests.get(url)
```


```python
page.status_code
```




    200




```python
soup=BeautifulSoup(page.text,"html.parser")
```


```python
print(soup.prettify())
```

    <!DOCTYPE html>
    <!-- hostname: web005, edition-view: espncricinfo-en-in, country: in, cluster: ind, created: 2017-12-24 19:04:52 -->
    <!--[if IE 8]><html class="lt-ie9" lang="en" xmlns="http://www.w3.org/1999/xhtml" xmlns:fb="http://www.facebook.com/2008/fbml"> <![endif]-->
    <!--[if IE 9]><html class="ie9" lang="en" xmlns="http://www.w3.org/1999/xhtml" xmlns:fb="http://www.facebook.com/2008/fbml"> <![endif]-->
    <!--[if gt IE 9]><!-->
    <html lang="en">
     <!--<![endif]-->
     <head>
      <meta content="text/html;charset=utf-8" http-equiv="Content-Type"/>
      <meta content="IE=edge,chrome=1" http-equiv="X-UA-Compatible"/>
      <script type="text/javascript">
       var _sf_startpt=(new Date()).getTime()
      </script>
      <meta content="ZxdgH3XglRg0Bsy-Ho2RnO3EE4nRs53FloLS6fkt_nc" name="google-site-verification">
       <title>
        ICC Cricket World Cup 2015 | Cricket news, live scores, fixtures, features and statistics on ESPN Cricinfo
       </title>
       <meta content="width=device-width,initial-scale=1" name="viewport"/>
       <meta content="ICC Cricket World Cup 2015, Live scores, Ball by ball update, Live scores, Live statistics, Fantasy Cricket, Player profile, Squads, Ground profile, Latest news, Latest features." name="keywords">
        <meta content="ICC Cricket World Cup 2015 with live cricket scores and the latest news and features throughout the series." name="description"/>
        <!-- Include isMobile script inline -->
        <script>
         /**
     * isMobile.js v0.3.6
     *
     * @author: Kai Mallea (kmallea@gmail.com)
     *
     * @license: http://creativecommons.org/publicdomain/zero/1.0/
     */
    !function(a){var b=/iPhone/i,c=/iPod/i,d=/iPad/i,e=/(?=.*\bAndroid\b)(?=.*\bMobile\b)/i,f=/Android/i,g=/IEMobile/i,h=/(?=.*\bWindows\b)(?=.*\bARM\b)/i,i=/BlackBerry/i,j=/BB10/i,k=/Opera Mini/i,l=/(?=.*\bFirefox\b)(?=.*\bMobile\b)/i,m=new RegExp("(?:Nexus 7|BNTV250|Kindle Fire|Silk|GT-P1000)","i"),n=function(a,b){return a.test(b)},o=function(a){var o=a||navigator.userAgent;return this.apple={phone:n(b,o),ipod:n(c,o),tablet:n(d,o),device:n(b,o)||n(c,o)||n(d,o)},this.android={phone:n(e,o),tablet:!n(e,o)&&n(f,o),device:n(e,o)||n(f,o)},this.windows={phone:n(g,o),tablet:n(h,o),device:n(g,o)||n(h,o)},this.other={blackberry:n(i,o),blackberry10:n(j,o),opera:n(k,o),firefox:n(l,o),device:n(i,o)||n(j,o)||n(k,o)||n(l,o)},this.seven_inch=n(m,o),this.any=this.apple.device||this.android.device||this.windows.device||this.other.device||this.seven_inch,this.phone=this.apple.phone||this.android.phone||this.windows.phone,this.tablet=this.apple.tablet||this.android.tablet||this.windows.tablet,"undefined"==typeof window?this:void 0},p=function(){var a=new o;return a.Class=o,a};"undefined"!=typeof module&&module.exports&&"undefined"==typeof window?module.exports=o:"undefined"!=typeof module&&module.exports&&"undefined"!=typeof window?module.exports=p():"function"==typeof define&&define.amd?define("isMobile",[],a.isMobile=p()):a.isMobile=p()}(this);
        </script>
        <!--[if IE 9]>
    <script language="javascript" type="text/javascript">
    function fnCreateJumpList(iScenario) {
    fnClearJumpList();
    window.external.msSiteModeCreateJumpList("Quick Links")
    window.external.msSiteModeAddJumpListItem('TCM', 'http://www.espncricinfo.com/ci/content/current/url/784439.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('Both Ends', 'http://www.espncricinfo.com/ci/content/current/url/1094910.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('MATCH DAY', 'http://www.espncricinfo.com/ci/content/current/url/1065560.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('CRICIQ', 'http://www.espncricinfo.com/ci/content/current/url/1095890.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('Insights', 'http://www.espncricinfo.com/ci/content/current/url/834953.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('WI v IND', 'http://www.espncricinfo.com/west-indies-v-india-2017/content/current/series/1098203.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('ENG v SA', 'http://www.espncricinfo.com/england-v-south-africa-2017/content/current/series/1031417.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('WWC', 'http://www.espncricinfo.com/icc-womens-world-cup-2017/content/current/series/1085935.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('Match Highlights', 'http://www.espncricinfo.com/ci/content/current/url/1107251.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeShowJumpList();
    }
    function fnClearJumpList() {
    window.external.msSiteModeClearJumplist();
    }
    </script>
    
    <meta name="msapplication-task" content="name=Live Scores;action-uri=http://www.espncricinfo.com/ci/engine/current/match/scores/live.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
    <meta name="msapplication-task" content="name=Latest News;action-uri=http://www.espncricinfo.com/ci/content/current/story/news.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
    <meta name="msapplication-task" content="name=Fixtures;action-uri=http://www.espncricinfo.com/ci/content/current/match/fixtures/index.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
    <meta name="msapplication-task" content="name=Results;action-uri=http://www.espncricinfo.com/ci/engine/current/match/scores/recent.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
    <meta name="msapplication-task" content="name=Photos;action-uri=http://www.espncricinfo.com/ci/content/current/image/index.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
    <meta name="msapplication-task" content="name=Audio/Video;action-uri=http://www.espncricinfo.com/ci/content/video_audio/index.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
    <script language="javascript" type="text/javascript">
            fnCreateJumpList(2);
    </script>
    <![endif]-->
        <meta content="index, follow" name="robots"/>
        <meta content="index, follow" name="googlebot"/>
        <meta content="260890547115" property="fb:app_id"/>
        <meta content="Cricinfo" property="og:site_name"/>
        <link href="http://a.espncdn.com/espncricinfo/favicon.ico" rel="shortcut icon"/>
        <link href="http://a.espncdn.com/espncricinfo/favicon.png" rel="icon" type="image/png"/>
        <link href="http://a.espncdn.com/espncricinfo/favicon.gif" rel="icon" type="image/gif"/>
        <link href="http://a.espncdn.com/wireless/mw5/r1/images/bookmark-icons/espncricinfo_icon-57x57.min.png" rel="apple-touch-icon"/>
        <link href="http://a.espncdn.com/wireless/mw5/r1/images/bookmark-icons/espncricinfo_icon-57x57.min.png" rel="apple-touch-icon-precomposed"/>
        <link href="http://a.espncdn.com/wireless/mw5/r1/images/bookmark-icons/espncricinfo_icon-72x72.min.png" rel="apple-touch-icon-precomposed" sizes="72x72"/>
        <link href="http://a.espncdn.com/wireless/mw5/r1/images/bookmark-icons/espncricinfo_icon-114x114.min.png" rel="apple-touch-icon-precomposed" sizes="114x114"/>
        <link href="http://a.espncdn.com/wireless/mw5/r1/images/bookmark-icons/espncricinfo_icon-152x152.min.png" rel="apple-touch-icon-precomposed" sizes="152x152"/>
        <meta content="ESPNcricinfo" name="application-name"/>
        <meta content="#266ab4" name="msapplication-TileColor"/>
        <meta content="http://i.imgci.com/espncricinfo/6b245241-3938-499c-8c79-9b80f97bed96.png" name="msapplication-TileImage"/>
        <meta content="ICC Cricket World Cup" property="og:title"/>
        <meta content="sport" property="og:type"/>
        <meta content="http://i.imgci.com/espncricinfo/facebook/2.jpg" property="og:image"/>
        <link href="http://i.imgci.com/espncricinfo/facebook/2.jpg" rel="image_src">
         <meta content="ICC Cricket World Cup 2015 with live cricket scores and the latest news and features throughout the series." property="og:description"/>
         <link href="http://www.espncricinfo.com/icc-cricket-world-cup-2015/content/series/509587.html" rel="canonical">
          <meta content="http://www.espncricinfo.com/icc-cricket-world-cup-2015/content/series/509587.html" property="og:url"/>
          <!-- Load GPT JavaScript library used by DFP -->
          <script type="text/javascript">
           var googletag = googletag || {};
      googletag.cmd = googletag.cmd || [];
      (function() {
        var gads = document.createElement("script");
        gads.async = true;
        gads.type = "text/javascript";
        var useSSL = true;
        // var useSSL = "https:" == document.location.protocol;
        gads.src = (useSSL ? "https:" : "http:") + "//www.googletagservices.com/tag/js/gpt.js";
        var node =document.getElementsByTagName("script")[0];
        node.parentNode.insertBefore(gads, node);
       })();
          </script>
          <script src="http://a.espncdn.com/combiner/c?js=swfobject/2.2/swfobject.js?minify=false" type="text/javascript">
          </script>
          <style>
           .espni-ad-slot{line-height: 0;}
    pre.gpt-debug{padding: 10px; background: burlywood;}
          </style>
          <script type="text/javascript">
           var ad_setting = {"adtar":"","kvbrand":"icccricketworldcup","kvcluster":"ind","kvnavtype":"serieshome","kvpt":"serieshome","kvseriesid":"509587","kvsite":"icccricketworldcup","networkid":"6444","path":"/6444/espn.cricinfo.com/series/icccricketworldcup/homepage","sp":"cricinfo","template":"desktop","template_type":"responsive"}; var __GPTenabled = true;
          </script>
          <link charset="utf-8" href="http://a.espncdn.com/combiner/c?css=fonts/bentonsans.css,fonts/bentonsansbold.css,fonts/bentonsanslight.css,fonts/bentonsansmedium.css,fonts/bentonsanscond.css,fonts/bentonsanscondbold.css,fonts/bentonsanscondmedium.css" media="screen" rel="stylesheet" type="text/css"/>
          <link charset="utf-8" href="http://i.imgci.com/navigation/cricinfo/ci/assets/css/sub_nav.css?v=1500525949" media="screen" rel="stylesheet" type="text/css"/>
          <link href="http://i.imgci.com/navigation/cricinfo/ci/assets/css/cilogin.1.0.4.css?v=1443427381" media="all" rel="stylesheet" type="text/css">
           <link href="http://i.imgci.com/navigation/cricinfo/ci/redesign/css/homepage/homepage.css?v=1479985888" rel="stylesheet"/>
           <link href="http://i.imgci.com/navigation/cricinfo/ci/assets/css/cilogin.1.0.4.css?v=1" rel="stylesheet"/>
           <link href="http://i.imgci.com/navigation/cricinfo/ci/redesign/css/oneid/forms.css?v=" media="all" rel="stylesheet" type="text/css">
            <link href="http://i.imgci.com/navigation/cricinfo/ci/vod_player/css/espn-web-player-bundle.1.0.1.css?v=1510209320" rel="stylesheet">
             <link href="http://i.imgci.com/navigation/cricinfo/ci/redesign/css/common/print.css?v=1506494239" media="print" rel="stylesheet"/>
             <!--[if lt IE 9]>
            <script src="/navigation/cricinfo/ci/assets/js/plugins/html5.js"></script>
            <link rel="stylesheet" href="/navigation/cricinfo/ci/redesign/css/common/ie8.css?v=1432640863">
        <![endif]-->
             <script src="http://i.imgci.com/navigation/cricinfo/ci/assets/js/libs/modernizr-2.8.3.min.js">
             </script>
             <script language="javascript" type="text/javascript">
              cqanswer = 'in';
                        location_country = 'in';
                location_cluster = 'ind';
            ord=Math.random()*10000000000000000;
            var __hpad300_100Web = 1;
            var ad_counter = 1;
            var ad_counter_mobile = 1;
            var devicenames = ['iPhone','BB10','Android','Windows Phone','Fennec','GoBrowser','NokiaBrowser','S40OviBrowser','SymbianOS','NokiaN73','Symbian','Opera Mini','Opera Mobi','Minimo','IEMobile','Mobile Safari','BlackBerry','sonyericsson'];
             </script>
             <style type="text/css">
              #adform-container { width: auto!important; }
             </style>
            </link>
           </link>
          </link>
         </link>
        </link>
       </meta>
      </meta>
     </head>
     <body class="country-home pagetype-home">
      <div id="viewport">
       <div class="espni-ad-slot ad0" data-kvpos="outofpage" data-out-of-page="true" data-slot-type="overlay">
       </div>
       <div class="espni-ad-slot ad5" data-exclude-bp="s,m,l" data-kvpos="top" data-slot-type="wallpaper">
       </div>
       <div id="fb-root">
       </div>
       <!-- Win8 Prompt (existing code) -->
       <!-- Win8 Prompt (existing code) -->
       <div class="hide-for-mob">
       </div>
       <script type="text/javascript">
        window.espn = window.espn || {};
      espn.isOneSite = true;
       </script>
       <script src="http://cdn.espn.com/core/format/modules/head/i18n?edition-host=espncricinfo.com&amp;type=ext&amp;site=espncricinfo®ion=in&amp;lang=en&amp;build=0.371.5">
       </script>
       <link crossorigin="" href="http://a.espncdn.com" rel="preconnect"/>
       <!--<link rel="stylesheet" href="http://local.espncricinfo.com:8080/static/css/transitional-header-cricinfo.css" />
    <link rel="stylesheet" href="/navigation/cricinfo/ci/redesign/css/transitional-header/transitional-header-cricinfo.css">-->
       <link href="http://a.espncdn.com/redesign/0.371.5/css/transitional-header-cricinfo.css" rel="stylesheet">
        <style>
         #global-viewport #header-wrapper{
        position: relative !important;
      }
      #global-viewport{    
        font-size: 16px !important;
        font-family: sans-serif !important;
      }
      body {
        background: #edeef0 !important;
      }
      #global-viewport .footer{
        background: transparent;
        text-transform: initial !important;
      }
      .global-search .search-results ul a {
        font-size: 12px;
        line-height: 16px;    
      }
      .global-search .search-results ul a b {
        font-family: BentonSans, -apple-system, Roboto, Helvetica, Arial, sans-serif !important;
      }
      #global-nav .pillar.editions > div{
        left:auto !important;
      }
      .feed-title {
        line-height: 24px !important;
        text-transform: uppercase;
        font-family: "BentonSans",-apple-system,"Roboto",Helvetica,Arial,sans-serif !important;
      }
      .feed-title.favorites span {
        color: #06c;
        cursor: pointer;
        display: block;
        font-weight: 400;
        line-height: 13px;
        position: absolute;
        right: 10px;
        text-transform: none;
        top: 5px;
      }
      #global-scoreboard-trigger+.alert{
        padding: 12px 35px 12px 18px;
        font-size: 12px;
      }
      .hidden{
        display: none !important;
      }
      @media screen and (min-width: 768px) {
        .scores-link-alert{
          display: none !important;
        }
      }
      @media screen and (min-width: 1024px) {
        .global-user .current-favorites {
          padding: 0 10px 34px 20px !important;
        }
      }
        </style>
        <script src="http://a.espncdn.com/redesign/0.371.5/js/espn-head.js">
        </script>
        <div class="espncricinfo transitional-header legacy-pages" data-behavior="global_nav_condensed global_nav_full" id="global-viewport">
         <nav data-loadtype="client" id="global-nav-mobile">
         </nav>
         <div class="menu-overlay-primary">
         </div>
         <div class="hidden-print" data-behavior="global_header" id="header-wrapper">
          <header class="espncricinfo-en user-account-management has-search" id="global-header">
           <div class="menu-overlay-secondary">
           </div>
           <div class="container">
            <a data-route="false" href="#" id="global-nav-mobile-trigger">
             <span>
              Menu
             </span>
            </a>
            <h1>
             <a href="/" name="&amp;lpos=sitenavdefault&amp;lid=sitenav_main-logo">
              ESPN
             </a>
            </h1>
            <ul class="tools">
             <li class="search">
              <a class="icon-font-after icon-search-solid-after" href="#" id="global-search-trigger">
              </a>
              <div class="global-search" id="global-search">
               <input class="search-box" placeholder="Search Sports, Teams or Players..." type="text"/>
               <input class="btn-search" type="submit"/>
              </div>
             </li>
             <li class="user" data-behavior="favorites_mgmt">
             </li>
             <li>
              <a data-route="false" href="#" id="global-scoreboard-trigger">
               Scores
              </a>
              <div class="hidden scores-link-alert scores-link-alert alert alert_button--close icon-font-after icon-close-solid-after" data-behavior="scores_link_alert">
               All cricket scores, fixtures and results here.
              </div>
             </li>
            </ul>
           </div>
          </header>
         </div>
        </div>
       </link>
      </div>
     </body>
    </html>
    <nav data-loadtype="server" id="global-nav">
     <ul itemscope="">
     </ul>
    </nav>
    <nav data-loadtype="server" id="global-nav-secondary">
     <div class="global-nav-container">
     </div>
    </nav>
    <div class="subnav-wrap sub-nav icc-cricket-world-cup-2015">
     <div class="row">
      <div class="large-20 sub-nav-wrap" id="sub-nav-wrap">
       <div class="icc-home">
        <a href="/icc-cricket-world-cup-2015/content/series/509587.html">
         ICC Cricket World Cup 2015
        </a>
       </div>
       <ul class="subnav_tier1 subnav-item-wrap" data-role="none" id="subnav_tier1">
        <li>
         <a href="/icc-cricket-world-cup-2015/content/story/news.html?object=509587" name="&amp;lpos=quicklink_News">
          News
         </a>
        </li>
        <li>
         <a href="/icc-cricket-world-cup-2015/content/story/features.html?object=509587" name="&amp;lpos=quicklink_Features">
          Features
         </a>
        </li>
        <li>
         <a href="/icc-cricket-world-cup-2015/content/image/index.html?object=509587" name="&amp;lpos=quicklink_Photos">
          Photos
         </a>
        </li>
        <li>
         <a href="/icc-cricket-world-cup-2015/content/series/509587.html?template=fixtures" name="&amp;lpos=quicklink_Fixtures">
          Fixtures
         </a>
         <div class="dd_wrap">
          <ul class="subnav_grp">
           <li class="subnav_grpitm">
            <ul class="subnav_tire2">
             <li class="sub_nav_item">
              <a href="/ci/content/page/817521.html" name="&amp;lpos=quicklink_Groups">
               Groups
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/series/509587.html?template=fixtures" name="&amp;lpos=quicklink_Fixtures">
               Fixtures
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/series/509587.html?template=download_fixtures" name="&amp;lpos=quicklink_Download the fixtures">
               Download the fixtures
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/series/806105.html?template=fixtures" name="&amp;lpos=quicklink_Warm-up matches">
               Warm-up matches
              </a>
             </li>
            </ul>
           </li>
          </ul>
         </div>
        </li>
        <li>
         <a href="/icc-cricket-world-cup-2015/engine/series/509587.html" name="&amp;lpos=quicklink_Results">
          Results
         </a>
         <div class="dd_wrap">
          <ul class="subnav_grp">
           <li class="subnav_grpitm">
            <ul class="subnav_tire2">
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/engine/series/509587.html" name="&amp;lpos=quicklink_Tournament">
               Tournament
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/engine/series/806105.html" name="&amp;lpos=quicklink_Warm-up matches">
               Warm-up matches
              </a>
             </li>
            </ul>
           </li>
          </ul>
         </div>
        </li>
        <li>
         <a href="/icc-cricket-world-cup-2015/engine/series/509587.html?view=pointstable" name="&amp;lpos=quicklink_Table">
          Table
         </a>
        </li>
        <li>
         <a href="/icc-cricket-world-cup-2015/content/squad?object=509587" name="&amp;lpos=quicklink_Squads">
          Squads
         </a>
         <div class="dd_wrap">
          <ul class="subnav_grp">
           <li class="subnav_grpitm">
            <ul class="subnav_tire2">
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/814789.html" name="&amp;lpos=quicklink_Afghanistan">
               Afghanistan
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/819741.html" name="&amp;lpos=quicklink_Australia">
               Australia
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/816431.html" name="&amp;lpos=quicklink_Bangladesh">
               Bangladesh
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/812413.html" name="&amp;lpos=quicklink_England">
               England
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/817409.html" name="&amp;lpos=quicklink_India">
               India
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/816765.html" name="&amp;lpos=quicklink_Ireland">
               Ireland
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/818117.html" name="&amp;lpos=quicklink_New Zealand">
               New Zealand
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/817901.html" name="&amp;lpos=quicklink_Pakistan">
               Pakistan
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/818887.html" name="&amp;lpos=quicklink_Scotland">
               Scotland
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/817889.html" name="&amp;lpos=quicklink_South Africa">
               South Africa
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/817899.html" name="&amp;lpos=quicklink_Sri Lanka">
               Sri Lanka
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/819825.html" name="&amp;lpos=quicklink_United Arab Emirates">
               United Arab Emirates
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/819743.html" name="&amp;lpos=quicklink_West Indies">
               West Indies
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/squad/817903.html" name="&amp;lpos=quicklink_Zimbabwe">
               Zimbabwe
              </a>
             </li>
            </ul>
           </li>
          </ul>
         </div>
        </li>
        <li>
         <a href="/icc-cricket-world-cup-2015/content/series/509587.html?template=ground" name="&amp;lpos=quicklink_Grounds">
          Grounds
         </a>
         <div class="dd_wrap">
          <ul class="subnav_grp">
           <li class="subnav_grpitm">
            <ul class="subnav_tire2">
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/56293.html" name="&amp;lpos=quicklink_Adelaide">
               Adelaide
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/58792.html" name="&amp;lpos=quicklink_Auckland">
               Auckland
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/56336.html" name="&amp;lpos=quicklink_Brisbane">
               Brisbane
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/58810.html" name="&amp;lpos=quicklink_Christchurch">
               Christchurch
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/56370.html" name="&amp;lpos=quicklink_Canberra">
               Canberra
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/58827.html" name="&amp;lpos=quicklink_Dunedin">
               Dunedin
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/58831.html" name="&amp;lpos=quicklink_Hamilton">
               Hamilton
              </a>
             </li>
            </ul>
           </li>
           <li class="subnav_grpitm">
            <ul class="subnav_tire2">
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/56407.html" name="&amp;lpos=quicklink_Hobart">
               Hobart
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/56441.html" name="&amp;lpos=quicklink_Melbourne">
               Melbourne
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/58857.html" name="&amp;lpos=quicklink_Napier">
               Napier
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/424917.html" name="&amp;lpos=quicklink_Nelson">
               Nelson
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/56490.html" name="&amp;lpos=quicklink_Perth">
               Perth
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/56544.html" name="&amp;lpos=quicklink_Sydney">
               Sydney
              </a>
             </li>
             <li class="sub_nav_item">
              <a href="/icc-cricket-world-cup-2015/content/ground/58899.html" name="&amp;lpos=quicklink_Wellington">
               Wellington
              </a>
             </li>
            </ul>
           </li>
          </ul>
         </div>
        </li>
        <li>
         <a href="/icc-cricket-world-cup-2015/engine/series/509587.html?view=records" name="&amp;lpos=quicklink_Stats">
          Stats
         </a>
        </li>
        <li>
         <a href="/icc-cricket-world-cup-2015/engine/records/index.html?id=12;type=trophy" name="&amp;lpos=quicklink_Records">
          Records
         </a>
        </li>
        <li>
         <a href="/wctimeline/content/site/wctimeline/index.html" name="&amp;lpos=quicklink_Timeline">
          Timeline
         </a>
        </li>
        <li>
         <a href="/wctimeline/content/site/wctimeline/genre.html?id=639" name="&amp;lpos=quicklink_Vignettes">
          Vignettes
         </a>
        </li>
        <li>
         <a href="/ci/content/video_audio/index.html?genre=111" name="&amp;lpos=quicklink_Men of the final">
          Men of the final
         </a>
        </li>
       </ul>
      </div>
     </div>
    </div>
    <div class="site-container ad-banner-1280">
     <div class="leaderboard-ad">
      <div class="espni-ad-slot ad-banner" data-kvpos="top" data-slot-type="banner">
      </div>
     </div>
    </div>
    <!-- Homepage wrapper Start -->
    <div class="hp-wrapper">
     <div class="site-container">
      <div class="pencil-ad">
       <div class="slot-1 hide-for-mob">
        <div class="espni-ad-slot ad-panel" data-exclude-bp="s" data-kvpos="left" data-slot-type="longstrip">
        </div>
       </div>
       <div class="slot-2 hide-for-mob">
        <div class="espni-ad-slot" data-exclude-bp="s" data-kvpos="top" data-slot-type="incontentstrip">
        </div>
       </div>
      </div>
     </div>
     <div class="hp-container">
      <div class="secondary">
       <section class="livescores module" data-seriesid="509587">
        <div class="tabs-web-below" data-iscounty="0" data-lscount="5" data-query="">
         <ul class="tabs" data-contentparent="#livescores-full">
          <li class="selected">
           <a class="ci-tabs" data-tabname="results" href="#results">
            Results
           </a>
          </li>
         </ul>
        </div>
        <div class="tabs-wrapper slider-wrapper" id="livescores-full">
         <div class="tabs-content" id="fixtures" style="display:none;">
          <ul class="scoreline-list">
           <li>
            No Live Matches at the moment
           </li>
          </ul>
         </div>
         <div class="tabs-content" id="results" style="display:block;">
          <ul>
           <li class="others">
            <ul class="scoreline-list">
             <li>
              <p>
               <span class="result-text">
                <a href="http://www.espncricinfo.com/series/509587/scorecard/656495/Australia-vs-New-Zealand-Final-ICC-Cricket-World-Cup">
                 New Zealand v Australia 
    					 at Melbourne
    					 on Mar 29, 2015
                </a>
               </span>
              </p>
              <p>
               Australia won by 7 wickets (with 101 balls remaining)
              </p>
             </li>
             <li>
              <p>
               <span class="result-text">
                <a href="http://www.espncricinfo.com/series/509587/scorecard/656493/Australia-vs-India-2nd Semi-Final-ICC-Cricket-World-Cup">
                 Australia v India 
    					 at Sydney
    					 on Mar 26, 2015
                </a>
               </span>
              </p>
              <p>
               Australia won by 95 runs
              </p>
             </li>
             <li>
              <p>
               <span class="result-text">
                <a href="http://www.espncricinfo.com/series/509587/scorecard/656491/New-Zealand-vs-South-Africa-1st Semi-Final-ICC-Cricket-World-Cup">
                 South Africa v New Zealand 
    					 at Auckland
    					 on Mar 24, 2015
                </a>
               </span>
              </p>
              <p>
               New Zealand won by 4 wickets (with 1 ball remaining) (D/L method)
              </p>
             </li>
             <li>
              <p>
               <span class="result-text">
                <a href="http://www.espncricinfo.com/series/509587/scorecard/656489/New-Zealand-vs-West-Indies-4th Quarter-Final-ICC-Cricket-World-Cup">
                 New Zealand v West Indies 
    					 at Wellington
    					 on Mar 21, 2015
                </a>
               </span>
              </p>
              <p>
               New Zealand won by 143 runs
              </p>
             </li>
             <li>
              <p>
               <span class="result-text">
                <a href="http://www.espncricinfo.com/series/509587/scorecard/656487/Australia-vs-Pakistan-3rd Quarter-Final-ICC-Cricket-World-Cup">
                 Pakistan v Australia 
    					 at Adelaide
    					 on Mar 20, 2015
                </a>
               </span>
              </p>
              <p>
               Australia won by 6 wickets (with 97 balls remaining)
              </p>
             </li>
            </ul>
           </li>
          </ul>
          <ul class="explore-links">
           <li>
            <a href="/icc-cricket-world-cup-2015/engine/series/509587.html">
             Results page
            </a>
           </li>
          </ul>
         </div>
        </div>
        <div class="tabs-wrapper slider-wrapper" id="livescores-compact">
         <div class="tabs-content" id="fixtures-compact" style="display:none;">
          <ul class="scoreline-list">
           <li>
            No Live Matches at the moment
           </li>
          </ul>
          <ul class="explore-links results-link">
           <li>
            <a href="/icc-cricket-world-cup-2015/content/series/509587.html?template=fixtures">
             All fixtures
            </a>
           </li>
          </ul>
         </div>
         <div class="tabs-content" id="results-compact" style="display:block;">
          <ul class="scoreline-list international">
           <li>
            <p>
             <span class="result-text">
              <a href="http://www.espncricinfo.com/series/509587/scorecard/656495/Australia-vs-New-Zealand-Final-ICC-Cricket-World-Cup">
               New Zealand v Australia 
    				 at Melbourne
    				 on Mar 29, 2015
              </a>
             </span>
            </p>
            <p>
             Australia won by 7 wickets (with 101 balls remaining)
            </p>
           </li>
           <li>
            <p>
             <span class="result-text">
              <a href="http://www.espncricinfo.com/series/509587/scorecard/656493/Australia-vs-India-2nd Semi-Final-ICC-Cricket-World-Cup">
               Australia v India 
    				 at Sydney
    				 on Mar 26, 2015
              </a>
             </span>
            </p>
            <p>
             Australia won by 95 runs
            </p>
           </li>
           <li>
            <p>
             <span class="result-text">
              <a href="http://www.espncricinfo.com/series/509587/scorecard/656491/New-Zealand-vs-South-Africa-1st Semi-Final-ICC-Cricket-World-Cup">
               South Africa v New Zealand 
    				 at Auckland
    				 on Mar 24, 2015
              </a>
             </span>
            </p>
            <p>
             New Zealand won by 4 wickets (with 1 ball remaining) (D/L method)
            </p>
           </li>
           <li>
            <p>
             <span class="result-text">
              <a href="http://www.espncricinfo.com/series/509587/scorecard/656489/New-Zealand-vs-West-Indies-4th Quarter-Final-ICC-Cricket-World-Cup">
               New Zealand v West Indies 
    				 at Wellington
    				 on Mar 21, 2015
              </a>
             </span>
            </p>
            <p>
             New Zealand won by 143 runs
            </p>
           </li>
           <li>
            <p>
             <span class="result-text">
              <a href="http://www.espncricinfo.com/series/509587/scorecard/656487/Australia-vs-Pakistan-3rd Quarter-Final-ICC-Cricket-World-Cup">
               Pakistan v Australia 
    				 at Adelaide
    				 on Mar 20, 2015
              </a>
             </span>
            </p>
            <p>
             Australia won by 6 wickets (with 97 balls remaining)
            </p>
           </li>
          </ul>
          <ul class="explore-links results-link">
           <li>
            <a href="/icc-cricket-world-cup-2015/engine/series/509587.html">
             All Results
            </a>
           </li>
          </ul>
         </div>
        </div>
       </section>
       <!-- 1st MPU start: Excluding from small, medium profiles and excluding the story pages -->
       <section class="ad-container">
        <div class="espni-ad-slot ad-incontent" data-exclude-bp="s,m" data-kvpos="top" data-slot-type="incontent">
        </div>
       </section>
       <!-- 1st MPU end -->
       <!-- Panel Ad start -->
       <section class="ad-container ad-140">
        <div class="espni-ad-slot ad-panel" data-kvpos="" data-slot-type="panel">
        </div>
       </section>
       <!-- Panel Ad end -->
       <!-- Cricket on Twitter start -->
       <section class="reader_rec hide-for-mob hide-landscape hide-portrait" style="border-top: 8px solid #dddddd;margin-bottom: 10px;">
        <a class="twitter-timeline" data-widget-id="586402209283227648" href="/ESPNcricinfo/timelines/585813293161406464">
         Readers recommend
        </a>
        <script>
         !function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");
        </script>
       </section>
       <!-- Cricket on Twitter end -->
       <!-- 2nd MPU start -->
       <section class="ad-container">
        <div class="espni-ad-slot ad-incontent" data-exclude-bp="s,m" data-kvpos="bottom" data-slot-type="incontent">
        </div>
       </section>
       <!-- MPU end -->
       <section class="ad-container">
        <div class="espni-ad-slot ad-panel" data-exclude-bp="s,m" data-kvpos="top" data-slot-type="panel">
        </div>
       </section>
       <!-- Facebook start -->
       <section class="cric-facebook module">
        <div class="content-padding">
         <h5 class="section-name">
          Facebook
         </h5>
        </div>
        <iframe allowtransparency="true" frameborder="0" scrolling="no" src="http://www.facebook.com/plugins/likebox.php?href=http%3A%2F%2Fwww.facebook.com%2Fcricinfo&amp;width=300&amp;height=245&amp;show_faces=true&amp;colorscheme=light&amp;stream=false&amp;border_color=0&amp;header=false&amp;show_border=false" style="border:none; overflow:hidden; width:300px; height:245px;  background:#FFFFFF;">
        </iframe>
       </section>
       <!-- Facebook end -->
      </div>
      <div class="primary">
       <!-- Multimedia module start -->
       <!-- use col-3 for option 1, option 2 and col-2 for option 3, option4 | connected class would connect col-2 with headline,
    remove it when a gap needed-->
       <section class="multimedia col-2 module">
        <div class="featured headline-before content">
         <div class="headers">
          <h6 class="sub-headline">
           <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&amp;lpos=headline_row0area0">
            ICC news
           </a>
          </h6>
          <h1 class="featured-link">
           <a href="/ci-icc/content/story/857811.html" name="&amp;lpos=headline_row0area0">
            Mustafa Kamal quits as ICC president
           </a>
          </h1>
         </div>
        </div>
        <div class="option-1" style="">
        </div>
        <div class="option-2" style="">
        </div>
        <div class="option-3">
        </div>
        <div class="option-4">
         <!-- Option 4 - Medium Image Start-->
         <div class="medium-image slider-wrapper" style="">
          <ul class="site-photo">
           <li data-pcaption="Golden glory: The Australia team celebrate their fifth World Cup win under a shower of confetti" data-pid="857245">
            <div class="img-wrap">
             <picture>
              <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209800/209891.3.jpg" title=""/>
             </picture>
             <div class="img-caption ">
              <p>
               Golden glory: The Australia team celebrate their fifth World Cup win under a shower of confetti
               <span class="copyright">
                © ICC
               </span>
              </p>
             </div>
            </div>
           </li>
          </ul>
         </div>
         <!-- Medium Image End-->
        </div>
       </section>
       <!-- Multimedia module end -->
       <div class="tabs-mobile">
        <ul class="tabs">
         <li class="selected">
          <a data-tname="news" href="#">
           News
          </a>
         </li>
         <li>
          <a data-tname="epicks" href="#">
           Editor's Picks
          </a>
         </li>
        </ul>
       </div>
       <section class="intermediate">
        <!-- Editor Picks module start -->
        <section class="editor-picks hide-portrait module">
         <div class="content-padding">
          <h5 class="section-name">
           <a href="/ci/content/story/editors_pick.html">
            Editor's Picks
           </a>
          </h5>
         </div>
         <!-- Editor Picks Items start-->
         <ul>
          <li>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/magazine/content/story/857201.html" name="&amp;lpos=editorspick_1">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209800/209873.5.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="content">
            <h2>
             <a href="/magazine/content/story/857201.html" name="&amp;lpos=editorspick_1">
              What did we learn from this World Cup?
             </a>
            </h2>
           </div>
           <ul class="link-cluster">
            <li>
             <span class="">
             </span>
             That there is a place for proper batsmanship in ODIs, that New Zealand punch above their weight, and that wickets win you matches. By
             <b>
              Ed Smith
             </b>
            </li>
           </ul>
          </li>
          <li>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/magazine/content/story/856297.html" name="&amp;lpos=editorspick_2">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209700/209725.6.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="content">
            <h2>
             <a href="/magazine/content/story/856297.html" name="&amp;lpos=editorspick_2">
              The greatest time of our lives
             </a>
            </h2>
           </div>
           <ul class="link-cluster">
            <li>
             <span class="">
             </span>
             <b>
              Martin Crowe:
             </b>
             Whatever happens, the Australia-New Zealand World Cup final at the MCG will be the most divine fun
            </li>
           </ul>
          </li>
          <li>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/magazine/content/story/852755.html" name="&amp;lpos=editorspick_3">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/207100/207187.4.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="content">
            <h2>
             <a href="/magazine/content/story/852755.html" name="&amp;lpos=editorspick_3">
              Left-arm quicks rule
             </a>
            </h2>
           </div>
           <ul class="link-cluster">
            <li>
             <span class="">
             </span>
             <b>
              Numbers Game:
             </b>
             Mitchell Starc and Trent Boult have led the way, but other left-arm fast bowlers have also shone in this World Cup
            </li>
           </ul>
          </li>
          <li>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/magazine/content/story/852631.html" name="&amp;lpos=editorspick_4">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/208900/208993.5.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="content">
            <h2>
             <a href="/magazine/content/story/852631.html" name="&amp;lpos=editorspick_4">
              The serenity of Kane Williamson
             </a>
            </h2>
           </div>
           <ul class="link-cluster">
            <li>
             <span class="">
             </span>
             <b>
              Martin Crowe:
             </b>
             He is assured, humble and intelligent: a player opposition captains and bowlers find hard to combat
            </li>
           </ul>
          </li>
          <li>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/icc-cricket-world-cup-2015/content/story/851397.html" name="&amp;lpos=editorspick_5">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/207100/207117.4.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="content">
            <h2>
             <a href="/icc-cricket-world-cup-2015/content/story/851397.html" name="&amp;lpos=editorspick_5">
              Build, build, blast off
             </a>
            </h2>
           </div>
           <ul class="link-cluster">
            <li>
             <span class="">
             </span>
             <b>
              Sharda Ugra:
             </b>
             This World Cup,most teams seem to have followed a strategy of constructing an innings till about the last quarter and then gone full pelt
            </li>
           </ul>
          </li>
         </ul>
         <!-- Editor Picks Items end-->
         <!-- Ad center column start -->
         <div class="ad-container ad-center-column ad-140">
          <div class="espni-ad-slot ad-panel" data-exclude-bp="s,m" data-kvpos="top" data-slot-type="panel">
          </div>
         </div>
         <!-- Ad center column end -->
         <!-- mpu Ad -->
         <div class="ad-container ad-center-column hide show-landscape">
          <div class="espni-ad-slot ad-incontent" data-exclude-bp="s,m,xl" data-kvpos="top" data-slot-type="incontent">
          </div>
         </div>
         <!-- mpu Ad End -->
        </section>
        <!-- Editor Picks module end -->
        <section class="ch-points-table module hide-portrait hide-for-mob">
         <div class="content-padding all-link">
          <h5 class="section-name">
           Points Table
          </h5>
          <a href="/icc-cricket-world-cup-2015/engine/series/509587.html?view=pointstable">
           All →
          </a>
         </div>
         <div class="group">
          <p class="heading">
           Pool A
          </p>
          <table>
           <tbody>
            <tr>
             <th class="bold">
              T
              <span>
               eams
              </span>
             </th>
             <th class="bold">
              M
              <span>
               at
              </span>
             </th>
             <th class="bold">
              W
              <span>
               on
              </span>
             </th>
             <th class="bold">
              L
              <span>
               ost
              </span>
             </th>
             <th class="bold">
              T
              <span>
               ied
              </span>
             </th>
             <th class="bold">
              N/R
             </th>
             <th class="bold last">
              P
              <span>
               ts
              </span>
             </th>
            </tr>
            <tr>
             <td>
              NZ
             </td>
             <td>
              6
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              12
             </td>
            </tr>
            <tr>
             <td>
              AUS
             </td>
             <td>
              6
             </td>
             <td>
              4
             </td>
             <td>
              1
             </td>
             <td>
              0
             </td>
             <td>
              1
             </td>
             <td class="last">
              9
             </td>
            </tr>
            <tr>
             <td>
              SL
             </td>
             <td>
              6
             </td>
             <td>
              4
             </td>
             <td>
              2
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              8
             </td>
            </tr>
            <tr>
             <td>
              BDESH
             </td>
             <td>
              6
             </td>
             <td>
              3
             </td>
             <td>
              2
             </td>
             <td>
              0
             </td>
             <td>
              1
             </td>
             <td class="last">
              7
             </td>
            </tr>
            <tr>
             <td>
              ENG
             </td>
             <td>
              6
             </td>
             <td>
              2
             </td>
             <td>
              4
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              4
             </td>
            </tr>
            <tr>
             <td>
              AFG
             </td>
             <td>
              6
             </td>
             <td>
              1
             </td>
             <td>
              5
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              2
             </td>
            </tr>
            <tr>
             <td>
              SCOT
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              0
             </td>
            </tr>
           </tbody>
          </table>
          <p class="heading">
           Pool B
          </p>
          <table>
           <tbody>
            <tr>
             <th class="bold">
              T
              <span>
               eams
              </span>
             </th>
             <th class="bold">
              M
              <span>
               at
              </span>
             </th>
             <th class="bold">
              W
              <span>
               on
              </span>
             </th>
             <th class="bold">
              L
              <span>
               ost
              </span>
             </th>
             <th class="bold">
              T
              <span>
               ied
              </span>
             </th>
             <th class="bold">
              N/R
             </th>
             <th class="bold last">
              P
              <span>
               ts
              </span>
             </th>
            </tr>
            <tr>
             <td>
              INDIA
             </td>
             <td>
              6
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              12
             </td>
            </tr>
            <tr>
             <td>
              SA
             </td>
             <td>
              6
             </td>
             <td>
              4
             </td>
             <td>
              2
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              8
             </td>
            </tr>
            <tr>
             <td>
              PAK
             </td>
             <td>
              6
             </td>
             <td>
              4
             </td>
             <td>
              2
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              8
             </td>
            </tr>
            <tr>
             <td>
              WI
             </td>
             <td>
              6
             </td>
             <td>
              3
             </td>
             <td>
              3
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              6
             </td>
            </tr>
            <tr>
             <td>
              IRE
             </td>
             <td>
              6
             </td>
             <td>
              3
             </td>
             <td>
              3
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              6
             </td>
            </tr>
            <tr>
             <td>
              ZIM
             </td>
             <td>
              6
             </td>
             <td>
              1
             </td>
             <td>
              5
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              2
             </td>
            </tr>
            <tr>
             <td>
              UAE
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              0
             </td>
            </tr>
           </tbody>
          </table>
         </div>
        </section>
        <section class="ch-statistics module hide-portrait hide-for-mob">
         <div class="content-padding">
          <h5 class="section-name">
           Statistics
          </h5>
         </div>
         <div class="content">
          <ul>
           <li>
            <h2>
             ICC Cricket World Cup, 2014/15
            </h2>
            <p>
             (in Australia/New Zealand ODIs)
            </p>
            <span>
             <a href="/icc-cricket-world-cup-2015/engine/records/batting/most_runs_career.html?id=6537;type=tournament">
              Most runs
             </a>
             |
            </span>
            <span>
             <a href="/icc-cricket-world-cup-2015/engine/records/bowling/most_wickets_career.html?id=6537;type=tournament">
              Most wickets
             </a>
             |
            </span>
            <span>
             <a href="/icc-cricket-world-cup-2015/engine/records/batting/most_runs_innings.html?id=6537;type=tournament">
              High scores
             </a>
             |
            </span>
            <span>
             <a href="/icc-cricket-world-cup-2015/engine/records/bowling/best_figures_innings.html?id=6537;type=tournament">
              Best bowling
             </a>
             |
            </span>
            <span>
             <a href="/icc-cricket-world-cup-2015/engine/records/index.html?id=6537;type=tournament">
              More
             </a>
            </span>
           </li>
          </ul>
          <ul>
           <li>
            <h2>
             Chappell-Hadlee Trophy, 2014/15
            </h2>
            <p>
             (Australia in New Zealand ODIs)
            </p>
            <span>
             <a href="/ci/engine/records/batting/most_runs_career.html?id=9897;type=series">
              Most runs
             </a>
             |
            </span>
            <span>
             <a href="/ci/engine/records/bowling/most_wickets_career.html?id=9897;type=series">
              Most wickets
             </a>
             |
            </span>
            <span>
             <a href="/ci/engine/records/batting/most_runs_innings.html?id=9897;type=series">
              High scores
             </a>
             |
            </span>
            <span>
             <a href="/ci/engine/records/bowling/best_figures_innings.html?id=9897;type=series">
              Best bowling
             </a>
             |
            </span>
            <span>
             <a href="/ci/engine/records/index.html?id=9897;type=series">
              More
             </a>
            </span>
           </li>
          </ul>
          <h2>
           One-Day Internationals
          </h2>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2">
            Overall |
           </a>
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=40;type=team">
            Afghanistan
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2;type=team">
            Australia
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=25;type=team">
            Bangladesh
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=1;type=team">
            England
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=6;type=team">
            India
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=29;type=team">
            Ireland
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=5;type=team">
            New Zealand
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=7;type=team">
            Pakistan
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=30;type=team">
            Scotland
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=3;type=team">
            South Africa
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=8;type=team">
            Sri Lanka
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=27;type=team">
            United Arab Emirates
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=4;type=team">
            West Indies
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=9;type=team">
            Zimbabwe
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2;type=host">
            Australia
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=5;type=host">
            New Zealand
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2017;type=year">
            Year 2017
           </a>
          </span>
         </div>
        </section>
        <section class="ch-squad module hide-portrait hide-for-mob">
         <div class="content-padding">
          <h5 class="section-name">
           Squad
          </h5>
         </div>
         <div class="squad-dd">
          <dl class="dropdown">
           <dt>
            <a>
             <span>
              Bangladesh Squad
             </span>
            </a>
           </dt>
           <dd>
            <ul>
             <li>
              <a class="default" data-value="10723">
               Bangladesh Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10737">
               South Africa Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10749">
               Zimbabwe Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10753">
               Scotland Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10771">
               Australia Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10775">
               United Arab Emirates Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10729">
               Ireland Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10733">
               India Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10747">
               Pakistan Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10681">
               England Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10699">
               Afghanistan Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10773">
               West Indies Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10745">
               Sri Lanka Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10751">
               New Zealand Squad
              </a>
             </li>
            </ul>
           </dd>
          </dl>
         </div>
         <div class="content">
          <ul class="list-block" id="squad_list_w">
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56007.html">
             Mashrafe Mortaza (c)
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/380354.html">
             Anamul Haque
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56268.html">
             Arafat Sunny
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/280734.html">
             Imrul Kayes
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56025.html">
             Mahmudullah
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/373696.html">
             Mominul Haque
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56029.html">
             Mushfiqur Rahim (wk)
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/300618.html">
             Nasir Hossain
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/300619.html">
             Rubel Hossain
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/373538.html">
             Sabbir Rahman
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56143.html">
             Shakib Al Hasan
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/436677.html">
             Soumya Sarkar
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/401057.html">
             Taijul Islam
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56194.html">
             Tamim Iqbal
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/538506.html">
             Taskin Ahmed
            </a>
           </li>
          </ul>
         </div>
        </section>
       </section>
       <!-- Headlines module start -->
       <section class="headlines module">
        <ul class="blocks">
         <li>
          <div class="featured">
           <div class="headers ">
            <h6 class="sub-headline">
             <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&amp;lpos=headline_row0area0">
              ICC news
             </a>
            </h6>
            <h1 class="featured-link">
             <a href="/ci-icc/content/story/857811.html" name="&amp;lpos=headline_row0area0">
              Mustafa Kamal quits as ICC president
             </a>
            </h1>
           </div>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/ci-icc/content/story/857811.html" name="&amp;lpos=headline_row0area0">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/150800/150825.4.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="link-list-wrapper">
            <ul class="link-list">
             <li>
              <span class="">
              </span>
              <a href="/ci-icc/content/story/857811.html" name="&amp;lpos=headline_row0area0">
               Steps down two days after threatening to expose ICC actions
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857327.html" name="&amp;lpos=headline_row0area0">
               Kamal rails at World Cup trophy snub
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/ci-icc/content/story/857977.html" name="&amp;lpos=headline_row0area0">
               'Whole episode is unfortunate' - Nazmul Hassan
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/magazine/content/story/858223.html" name="&amp;lpos=headline_row0area0">
               Kalra: Mustafa Kamal's own goal
              </a>
             </li>
            </ul>
            <p class="related related-mobile">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               4 Related
              </span>
              <span class="more-news-close">
               Close
              </span>
             </a>
            </p>
           </div>
          </div>
         </li>
         <li>
          <div class="others">
           <div class="headers">
            <h6 class="sub-headline">
             <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&amp;lpos=headline_row1area1">
              World Cup 2015 review
             </a>
            </h6>
            <h2 class="featured-link">
             <a href="/magazine/content/story/857425.html" name="&amp;lpos=headline_row1area1">
              Advantage batsmen, game to bowlers
             </a>
            </h2>
           </div>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/magazine/content/story/857425.html" name="&amp;lpos=headline_row1area1">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209900/209935.4.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="link-list-wrapper">
            <ul class="link-list">
             <li>
              <span class="">
              </span>
              <a href="/magazine/content/story/857425.html" name="&amp;lpos=headline_row1area1">
               Bal: Bowlers had the last word despite string of high scores
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857187.html" name="&amp;lpos=headline_row1area1">
               NZ 5, Australia 4 in our World Cup team
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857203.html" name="&amp;lpos=headline_row1area1">
               Best bowling  - Wahab, Southee, Starc
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857261.html" name="&amp;lpos=headline_row1area1">
               Best innings - Guptill, Smith, Shenwari
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857339.html" name="&amp;lpos=headline_row1area1">
               Stats - Blazing Brendon, Sensational Starc
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857341.html" name="&amp;lpos=headline_row1area1">
               Controversies - Mooney's catch, Rohit's no-ball
              </a>
             </li>
            </ul>
            <ul class="related-list">
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857249.html" name="&amp;lpos=headline_row1area1">
               Memories - The nice guys from New Zealand
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/856295.html" name="&amp;lpos=headline_row1area1">
               Best quotes - Dhoni's citizenship quip
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/gallery/856445.html" name="&amp;lpos=headline_row1area1">
               World Cup in pictures
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857333.html" name="&amp;lpos=headline_row1area1">
               World Cup in tweets - Less boring than 2007
              </a>
             </li>
            </ul>
            <!-- Related items for web Start -->
            <p class="related">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               4 Related
              </span>
              <span class="more-news-close">
               Close
              </span>
             </a>
            </p>
            <!-- Related items for web End -->
            <!-- Related items for mobile Start -->
            <p class="related related-mobile">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               10 Related
              </span>
              <span class="more-news-close">
               Close
              </span>
             </a>
            </p>
            <!-- Related items for mobile End -->
           </div>
          </div>
         </li>
         <li>
          <div class="others">
           <div class="headers">
            <h6 class="sub-headline">
             <a href="/icc-cricket-world-cup-2015/engine/match/656495.html" name="&amp;lpos=headline_row1area2">
              Aus v NZ, Final, Melbourne
             </a>
            </h6>
            <h2 class="featured-link">
             <a href="/icc-cricket-world-cup-2015/content/story/857209.html" name="&amp;lpos=headline_row1area2">
              Australia in a World Cup final, business as usual
             </a>
            </h2>
           </div>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/icc-cricket-world-cup-2015/content/story/857209.html" name="&amp;lpos=headline_row1area2">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209700/209773.4.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="link-list-wrapper">
            <ul class="link-list">
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857209.html" name="&amp;lpos=headline_row1area2">
               Ugra: Their A-game gene overrides all obstacles
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/engine/match/656495.html" name="&amp;lpos=headline_row1area2">
               SCORECARD
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/856581.html" name="&amp;lpos=headline_row1area2">
               Report - Majestic Australia claim magical fifer
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857229.html" name="&amp;lpos=headline_row1area2">
               Zaltzman: NZ drown in duck nightmare
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/856849.html" name="&amp;lpos=headline_row1area2">
               Stats - Oldest Australian in a final
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/thestands/content/story/856789.html" name="&amp;lpos=headline_row1area2">
               #report - Ruining finals since 1999
              </a>
             </li>
            </ul>
            <ul class="related-list">
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857045.html" name="&amp;lpos=headline_row1area2">
               Reactions - 'I'll try to have a beer with everyone in the place'
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/856811.html" name="&amp;lpos=headline_row1area2">
               Plays - McCullum's dream turns to nightmare
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/856599.html" name="&amp;lpos=headline_row1area2">
               Moments - Johnson owns Spartacus
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/856837.html" name="&amp;lpos=headline_row1area2">
               Photo report
              </a>
             </li>
             <li>
              <span class="icon-font icon-play-solid">
              </span>
              <a href="/ci/content/video_audio/856645.html" name="&amp;lpos=headline_row1area2">
               Holding: How could anyone think this is the best World Cup ever?
              </a>
             </li>
             <li>
              <span class="icon-font icon-play-solid">
              </span>
              <a href="/ci/content/video_audio/856623.html" name="&amp;lpos=headline_row1area2">
               On the road - Rugby nation's cricket moment
              </a>
             </li>
             <li>
              <span class="icon-font icon-play-solid">
              </span>
              <a href="/ci/content/video_audio/856649.html" name="&amp;lpos=headline_row1area2">
               Match Point - Behind the scenes
              </a>
             </li>
            </ul>
            <!-- Related items for web Start -->
            <p class="related">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               7 Related
              </span>
              <span class="more-news-close">
               Close
              </span>
             </a>
            </p>
            <!-- Related items for web End -->
            <!-- Related items for mobile Start -->
            <p class="related related-mobile">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               13 Related
              </span>
              <span class="more-news-close">
               Close
              </span>
             </a>
            </p>
            <!-- Related items for mobile End -->
           </div>
          </div>
         </li>
         <li class="hr">
          <hr/>
         </li>
         <li class="hide-all">
         </li>
         <li>
          <div class="others">
           <div class="headers">
            <h6 class="sub-headline">
             <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&amp;lpos=headline_row2area1">
              Aus v NZ, final, Melbourne
             </a>
            </h6>
            <h2 class="featured-link">
             <a href="/icc-cricket-world-cup-2015/content/story/856971.html" name="&amp;lpos=headline_row2area1">
              Young Australia suggest years of dominance
             </a>
            </h2>
           </div>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/icc-cricket-world-cup-2015/content/story/856971.html" name="&amp;lpos=headline_row2area1">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209800/209813.4.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="link-list-wrapper">
            <ul class="link-list">
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/856971.html" name="&amp;lpos=headline_row2area1">
               Brettig: Top performers will be around for 2019
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857181.html" name="&amp;lpos=headline_row2area1">
               Plans fall in place for Australia's A-Team
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857177.html" name="&amp;lpos=headline_row2area1">
               Clarke speaks of emotional toll
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857167.html" name="&amp;lpos=headline_row2area1">
               Gallery - Glee and glory on a golden evening
              </a>
             </li>
             <li>
              <span class="icon-font icon-play-solid">
              </span>
              <a href="/ci/content/video_audio/856401.html" name="&amp;lpos=headline_row2area1">
               Ponting: Smith makes batting look easier
              </a>
             </li>
            </ul>
            <!-- Related items for mobile Start -->
            <p class="related related-mobile">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               5 Related
              </span>
              <span class="more-news-close">
               Close
              </span>
             </a>
            </p>
            <!-- Related items for mobile End -->
           </div>
          </div>
         </li>
         <li>
          <div class="others">
           <div class="headers">
            <h6 class="sub-headline">
             <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&amp;lpos=headline_row2area2">
              Aus v NZ, final, Melbourne
             </a>
            </h6>
            <h2 class="featured-link">
             <a href="/icc-cricket-world-cup-2015/content/story/856963.html" name="&amp;lpos=headline_row2area2">
              New Zealand lost final by abandoning aggression
             </a>
            </h2>
           </div>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/icc-cricket-world-cup-2015/content/story/856963.html" name="&amp;lpos=headline_row2area2">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209700/209767.4.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="link-list-wrapper">
            <ul class="link-list">
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/856963.html" name="&amp;lpos=headline_row2area2">
               Coverdale: Team found wanting for lack of plan B
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/magazine/content/story/857205.html" name="&amp;lpos=headline_row2area2">
               Kimber: NZ's greatest almost
              </a>
             </li>
             <li>
              <span class="">
              </span>
              <a href="/icc-cricket-world-cup-2015/content/story/857165.html" name="&amp;lpos=headline_row2area2">
               Gracious McCullum hopes run will leave legacy
              </a>
             </li>
             <li>
              <span class="icon-font icon-play-solid">
              </span>
              <a href="/ci/content/video_audio/856375.html" name="&amp;lpos=headline_row2area2">
               <b>
                Two Men Out
               </b>
               - NZ's match-winners
              </a>
             </li>
            </ul>
            <!-- Related items for mobile Start -->
            <p class="related related-mobile">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               4 Related
              </span>
              <span class="more-news-close">
               Close
              </span>
             </a>
            </p>
            <!-- Related items for mobile End -->
           </div>
          </div>
         </li>
         <li class="hr">
          <hr/>
         </li>
         <li class="hide-all">
         </li>
        </ul>
       </section>
       <!-- Headlines module end -->
       <!-- More news module start -->
       <section class="more-news module">
        <div class="content-padding all-link">
         <h5 class="section-name">
          <a href="/icc-cricket-world-cup-2015/content/story/news.html?object=509587">
           More News
          </a>
         </h5>
         <a href="/icc-cricket-world-cup-2015/content/story/news.html?object=509587" name="&amp;lpos=morenews_all">
          All →
         </a>
        </div>
        <div id="espni-moreNews">
         <ul class="blocks">
          <li>
           <div class="wrapper">
            <div class="headers">
             <h6 class="sub-headline">
              <a href="/ci/content/site/297120.html" name="&amp;lpos=morenews_news">
               ICC news
              </a>
             </h6>
             <h2 class="featured-link">
              <a href="/ci-icc/content/story/857265.html" name="&amp;lpos=morenews_news">
               ICC chiefs voice support for Associates
              </a>
             </h2>
            </div>
           </div>
          </li>
          <li>
           <div class="wrapper">
            <div class="headers">
             <h6 class="sub-headline">
              <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&amp;lpos=morenews_news">
               Aus v NZ, Final, Melbourne
              </a>
             </h6>
             <h2 class="featured-link">
              <a href="/icc-cricket-world-cup-2015/content/story/856521.html" name="&amp;lpos=morenews_news">
               Australia look to heal an old wound
              </a>
             </h2>
            </div>
            <div class="related">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               5
              </span>
              <span class="more-news-close icon-font icon-boxed-close-outline">
              </span>
             </a>
            </div>
            <div class="link-list-wrapper">
             <ul class="link-list">
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856521.html">
                Brettig: Team still bears scars of 1992
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856301.html">
                Australia's journey to final began in 2013
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856169.html">
                NZ loss 'kick up the backside' - Clarke
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856347.html">
                Time to have the neighbours over
               </a>
              </li>
              <li>
               <span class="icon-font icon-play-solid">
               </span>
               <a href="/ci/content/video_audio/856355.html">
                Will MCG be a full house for final?
               </a>
              </li>
             </ul>
            </div>
           </div>
          </li>
          <li>
           <div class="wrapper">
            <div class="headers">
             <h6 class="sub-headline">
              <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&amp;lpos=morenews_news">
               Aus v India, 2nd semi-final, SCG
              </a>
             </h6>
             <h2 class="featured-link">
              <a href="/icc-cricket-world-cup-2015/content/story/856171.html" name="&amp;lpos=morenews_news">
               Johnson drowns out tumult to down India
              </a>
             </h2>
            </div>
            <div class="related">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               3
              </span>
              <span class="more-news-close icon-font icon-boxed-close-outline">
              </span>
             </a>
            </div>
            <div class="link-list-wrapper">
             <ul class="link-list">
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856171.html">
                Brettig: Overcomes a hostile crowd to strike telling blows
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856209.html">
                Zaltzman: Smith, Starc the talk of SCG
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/855999.html">
                Australia arrives at its own party
               </a>
              </li>
             </ul>
            </div>
           </div>
          </li>
          <li>
           <div class="wrapper">
            <div class="headers">
             <h6 class="sub-headline">
              <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&amp;lpos=morenews_news">
               Aus v Ind, 2nd semi-final, Sydney
              </a>
             </h6>
             <h2 class="featured-link">
              <a href="/icc-cricket-world-cup-2015/content/story/856195.html" name="&amp;lpos=morenews_news">
               India swept away by Australia's depth
              </a>
             </h2>
            </div>
            <div class="related">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               4
              </span>
              <span class="more-news-close icon-font icon-boxed-close-outline">
              </span>
             </a>
            </div>
            <div class="link-list-wrapper">
             <ul class="link-list">
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856195.html">
                Ugra: Team's set patterns went awry
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856045.html">
                Pressure makes you do things you don't want to - Dhoni
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856099.html">
                Kimber: No helicopter ride to glory
               </a>
              </li>
              <li>
               <span class="icon-font icon-play-solid">
               </span>
               <a href="/ci/content/video_audio/856145.html">
                Let's Talk About: A message for India
               </a>
              </li>
             </ul>
            </div>
           </div>
          </li>
          <li>
           <div class="wrapper">
            <div class="headers">
             <h6 class="sub-headline">
              <a href="/icc-cricket-world-cup-2015/engine/match/656493.html" name="&amp;lpos=morenews_news">
               Aus v Ind, 2nd semi-final, Sydney
              </a>
             </h6>
             <h2 class="featured-link">
              <a href="/icc-cricket-world-cup-2015/content/story/855539.html" name="&amp;lpos=morenews_news">
               Australia win big as India run out of gas
              </a>
             </h2>
            </div>
            <div class="related">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               8
              </span>
              <span class="more-news-close icon-font icon-boxed-close-outline">
              </span>
             </a>
            </div>
            <div class="link-list-wrapper">
             <ul class="link-list">
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/engine/match/656493.html">
                SCORECARD
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/855539.html">
                Report - Smith sets up trans-Tasman final
               </a>
              </li>
              <li>
               <span class="icon-font icon-play-solid">
               </span>
               <a href="/ci/content/video_audio/856193.html">
                #PoliteEnquiries: Worst effort at a chase?
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856017.html">
                Plays - Starc gets a talking-to
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/thestands/content/story/856251.html">
                Fan report - Groovy tunes and a sunset at the SCG
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/855921.html">
                Stats - Highest semi-final score
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/855809.html">
                Photo report
               </a>
              </li>
              <li>
               <span class="icon-font icon-play-solid">
               </span>
               <a href="/ci/content/video_audio/855907.html">
                Up for Challenge - Fishing for Oysters
               </a>
              </li>
             </ul>
            </div>
           </div>
          </li>
          <li>
           <div class="wrapper">
            <div class="headers">
             <h6 class="sub-headline">
              <a href="/icc-cricket-world-cup-2015/engine/match/656493.html" name="&amp;lpos=morenews_news">
               Aus v Ind, 2nd semi-final, Sydney
              </a>
             </h6>
             <h2 class="featured-link">
              <a href="/icc-cricket-world-cup-2015/content/story/856033.html" name="&amp;lpos=morenews_news">
               Dhoni won't rule out 2019, will decide next year
              </a>
             </h2>
            </div>
            <div class="related">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               2
              </span>
              <span class="more-news-close icon-font icon-boxed-close-outline">
              </span>
             </a>
            </div>
            <div class="link-list-wrapper">
             <ul class="link-list">
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/856033.html">
                'Still running, still fit', may take call after 2016 World T20
               </a>
              </li>
              <li>
               <span class="icon-font icon-play-solid">
               </span>
               <a href="/ci/content/video_audio/856377.html">
                Can you enact Dhoni's helicopter shot?
               </a>
              </li>
             </ul>
            </div>
           </div>
          </li>
          <li>
           <div class="wrapper">
            <div class="headers">
             <h6 class="sub-headline">
              <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&amp;lpos=morenews_news">
               Aus v Ind, 2nd semi-final, Sydney
              </a>
             </h6>
             <h2 class="featured-link">
              <a href="/icc-cricket-world-cup-2015/content/story/855495.html" name="&amp;lpos=morenews_news">
               Dhoni masters numbers game to crack ODI code
              </a>
             </h2>
            </div>
            <div class="related">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               3
              </span>
              <span class="more-news-close icon-font icon-boxed-close-outline">
              </span>
             </a>
            </div>
            <div class="link-list-wrapper">
             <ul class="link-list">
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/855495.html">
                Ugra: Captain moving from flashy finisher to measured risk manager
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/855405.html">
                India not carrying any scars, says Rohit
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/855441.html">
                'Always wanted to be best player in the world' - Kohli
               </a>
              </li>
             </ul>
            </div>
           </div>
          </li>
          <li>
           <div class="wrapper">
            <div class="headers">
             <h6 class="sub-headline">
              <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&amp;lpos=morenews_news">
               Aus v Ind, 2nd semi-final, Sydney
              </a>
             </h6>
             <h2 class="featured-link">
              <a href="/icc-cricket-world-cup-2015/content/story/855383.html" name="&amp;lpos=morenews_news">
               His team on a roll, Clarke defends captaincy
              </a>
             </h2>
            </div>
            <div class="related">
             <a href="#">
              <span class="icon-font icon-boxed-plus-outline">
              </span>
              <span class="more-news-count">
               8
              </span>
              <span class="more-news-close icon-font icon-boxed-close-outline">
              </span>
             </a>
            </div>
            <div class="link-list-wrapper">
             <ul class="link-list">
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/855383.html">
                'I think my record stacks up against just about anyone'
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/855437.html">
                How Faulkner, Maxwell turned into each other
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/854925.html">
                Smith strays down leg to reap the runs
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/854697.html">
                Finch calls in familiar help
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/854317.html">
                Sledging inevitable in semi-final - Faulkner
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/magazine/content/story/854289.html">
                Former PM Hawke livens up Australia's training
               </a>
              </li>
              <li>
               <span class="">
               </span>
               <a href="/icc-cricket-world-cup-2015/content/story/854433.html">
                The coach with a hand in both camps
               </a>
              </li>
              <li>
               <span class="icon-font icon-play-solid">
               </span>
               <a href="/ci/content/video_audio/854431.html">
                Sadist Hour: The continuum of risk
               </a>
              </li>
             </ul>
            </div>
           </div>
          </li>
         </ul>
        </div>
       </section>
       <!-- More news module end -->
       <div class="ad-slot show-mobile">
        <div class="espni-ad-slot ad-incontent" data-exclude-bp="m,l,xl" data-kvpos="top" data-slot-type="incontent">
        </div>
       </div>
       <section class="quotes module content-padding">
        <h5 class="section-name">
         <a href="/icc-cricket-world-cup-2015/content/quote/index.html?object=509587">
          Quote Unquote
         </a>
        </h5>
        <blockquote>
         <span class="icon icon-quotes">
         </span>
         <a href="/icc-cricket-world-cup-2015/content/quote/index.html?object=509587">
          I would have called you a bunch of losers if you lost against Sri Lanka. You are championship material.
         </a>
        </blockquote>
       </section>
       <section class="two-col-wrapper">
        <section class="eq-2-col">
         <!-- Cordon module start -->
         <section class="cordon module blogs">
          <div class="content-padding all-link">
           <h5 class="section-name">
            <a href="/blogs/content/story/blogs/cordon.html">
             BLOGS
            </a>
           </h5>
           <a href="/blogs/content/story/blogs/cordon.html" name="&amp;lpos=cordon_all">
            All →
           </a>
          </div>
          <ul class="list-block">
           <li>
            <div class="thumbnail">
             <div class="img-wrap">
              <picture>
               <a href="/blogs/content/story/857349.html" name="&amp;lpos=cordon_1">
                <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209700/209793.4.jpg" title=""/>
               </a>
              </picture>
             </div>
            </div>
            <div class="content">
             <h4>
              <a href="/blogs/content/story/857349.html" name="&amp;lpos=cordon_1">
               Fifty-over cricket, I was wrong
              </a>
             </h4>
            </div>
            <p class="text">
             <b>
              Jon Hotten:
             </b>
             This World Cup gave us driven, vibrant, electric ODI cricket, played at the limit of current ability, and it was magnificent
            </p>
           </li>
           <li>
            <div class="thumbnail">
             <div class="img-wrap">
              <picture>
               <a href="/blogs/content/story/855685.html" name="&amp;lpos=cordon_2">
                <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/129500/129519.4.jpg" title=""/>
               </a>
              </picture>
             </div>
            </div>
            <div class="content">
             <h4>
              <a href="/blogs/content/story/855685.html" name="&amp;lpos=cordon_2">
               Why ball-tracking can't be trusted
              </a>
             </h4>
            </div>
            <p class="text">
             <b>
              Russell Jackson:
             </b>
             It's hard for us to shake doubt about why what we're seeing with our eyes differs significantly from the reading of a computer
            </p>
           </li>
          </ul>
         </section>
         <!-- Cordon module end -->
         <!-- Poll module start -->
         <section class="poll module">
          <div class="content-padding">
           <h5 class="section-name">
            Poll
           </h5>
          </div>
          <div class="content" id="poll_content">
           <form action="" id="home_poll" method="get">
            <h6>
             Which was the most boring World Cup of all?
            </h6>
            <ul>
             <li>
              <input id="poll_3959" name="poll" type="radio" value="3959"/>
              <label for="poll_3959">
               2015
              </label>
             </li>
             <li>
              <input id="poll_3961" name="poll" type="radio" value="3961"/>
              <label for="poll_3961">
               2011
              </label>
             </li>
             <li>
              <input id="poll_3963" name="poll" type="radio" value="3963"/>
              <label for="poll_3963">
               2007
              </label>
             </li>
             <li>
              <input id="poll_3965" name="poll" type="radio" value="3965"/>
              <label for="poll_3965">
               2003
              </label>
             </li>
             <li>
              <input id="poll_3967" name="poll" type="radio" value="3967"/>
              <label for="poll_3967">
               1999
              </label>
             </li>
             <li>
              <input id="poll_3969" name="poll" type="radio" value="3969"/>
              <label for="poll_3969">
               1996
              </label>
             </li>
             <li>
              <input id="poll_3971" name="poll" type="radio" value="3971"/>
              <label for="poll_3971">
               1992
              </label>
             </li>
             <li>
              <input id="poll_3973" name="poll" type="radio" value="3973"/>
              <label for="poll_3973">
               1987
              </label>
             </li>
             <li>
              <input id="poll_3975" name="poll" type="radio" value="3975"/>
              <label for="poll_3975">
               1983
              </label>
             </li>
             <li>
              <input id="poll_3977" name="poll" type="radio" value="3977"/>
              <label for="poll_3977">
               1979
              </label>
             </li>
             <li>
              <input id="poll_3979" name="poll" type="radio" value="3979"/>
              <label for="poll_3979">
               1975
              </label>
             </li>
            </ul>
            <p>
             <input id="button" type="submit" value="Submit"/>
            </p>
            <input id="hdnPollId" name="hdnPollId" type="hidden" value="1067"/>
            <input id="hdnSiteId" name="hdnSiteId" type="hidden" value="1530"/>
            <input id="hdnPollTitle" name="hdnPollTitle" type="hidden" value="Which was the most boring World Cup of all?"/>
           </form>
          </div>
         </section>
         <!-- Poll module end -->
         <script type="text/javascript">
          var _ENDPOINT = "http://submit.espncricinfo.com/";
        var _BRANDING = "icc-cricket-world-cup-2015";
         </script>
        </section>
        <section class="eq-2-col">
         <div class="photo-gallery-wrapper">
          <section class="photo-gallery module">
           <div class="pg-tabs content-padding">
            <ul class="section-name tabs " data-contentparent=".photo-gallery">
             <li class="selected">
              <a class="ci-tabs" href="#photo-wrapper">
               Photos
              </a>
             </li>
             <li>
              <a class="ci-tabs" href="#gallery-wrapper">
               Gallery
              </a>
             </li>
            </ul>
            <a class="pg-all-link" href="/ci/content/image/index.html?object=509587">
             All
            </a>
           </div>
           <div class="content-padding all-link pg-section-title">
            <h5 class="section-name">
             Photos
            </h5>
            <a href="/ci/content/image/index.html?object=509587" name="&amp;lpos=photos_all">
             All →
            </a>
           </div>
           <div class="photo slider-wrapper tabs-content " id="photo-wrapper">
            <ul class="list-block slider-chphoto">
             <li>
              <div class="thumbnail">
               <a href="/ci/content/image/1099932.html?object=509587;dir=next" name="&amp;lpos=photos_1">
                <picture>
                 <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/263700/263707.png" title=""/>
                </picture>
               </a>
              </div>
              <div class="content">
               <p>
                <span class="bold">
                 May 29, 2017
                </span>
                England's ODI batting has undergone a massive transformation
                <span class="copyright">
                 © ESPNcricinfo Ltd
                </span>
               </p>
              </div>
             </li>
             <li>
              <div class="thumbnail">
               <a href="/newzealand/content/image/970143.html?object=509587;dir=next" name="&amp;lpos=photos_2">
                <picture>
                 <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/233600/233601.3.jpg" title=""/>
                </picture>
               </a>
              </div>
              <div class="content">
               <p>
                <span class="bold">
                 Mar 31, 2015
                </span>
                Brendon McCullum obliges fans
                <span class="copyright">
                 © Getty Images
                </span>
               </p>
              </div>
             </li>
             <li>
              <div class="thumbnail">
               <a href="/ci/content/image/932093.html?object=509587;dir=next" name="&amp;lpos=photos_3">
                <picture>
                 <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/224900/224977.4.jpg" title=""/>
                </picture>
               </a>
              </div>
              <div class="content">
               <p>
                <span class="bold">
                 Mar 31, 2015
                </span>
                Brendon McCullum looks on
                <span class="copyright">
                 © Getty Images
                </span>
               </p>
              </div>
             </li>
             <li>
              <div class="thumbnail">
               <a href="/ci/content/image/895759.html?object=509587;dir=next" name="&amp;lpos=photos_4">
                <picture>
                 <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/217100/217109.4.jpg" title=""/>
                </picture>
               </a>
              </div>
              <div class="content">
               <p>
                <span class="bold">
                 Mar 31, 2015
                </span>
                Fans welcome the New Zealand team home
                <span class="copyright">
                 © Getty Images
                </span>
               </p>
              </div>
             </li>
            </ul>
           </div>
           <div class="content-padding all-link pg-section-title">
            <h5 class="section-name">
             Galleries
            </h5>
            <a href="/ci/content/gallery/index.html?object=509587" name="&amp;lpos=gallery_all">
             All →
            </a>
           </div>
           <div class="gallery tabs-content" id="gallery-wrapper">
            <ul class="list-block slider-chgallery">
             <li>
              <div class="thumbnail">
               <a href="/icc-cricket-world-cup-2015/content/gallery/856445.html" name="&amp;lpos=gallery_1">
                <picture>
                 <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/205500/205511.3.jpg" title=""/>
                </picture>
               </a>
              </div>
              <div class="content">
               <p>
                <span class="bold">
                 Mar 28, 2015
                </span>
                World Cup 2015 in photos
                <span class="copyright">
                 © Getty Images
                </span>
               </p>
              </div>
             </li>
             <li>
              <div class="thumbnail">
               <a href="/ci/content/gallery/855221.html" name="&amp;lpos=gallery_2">
                <picture>
                 <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209400/209433.3.jpg" title=""/>
                </picture>
               </a>
              </div>
              <div class="content">
               <p>
                <span class="bold">
                 Mar 24, 2015
                </span>
                NZ v SA, 1st semi-final, Auckland
                <span class="copyright">
                 © Getty Images
                </span>
               </p>
              </div>
             </li>
             <li>
              <div class="thumbnail">
               <a href="/icc-cricket-world-cup-2015/content/gallery/839573.html" name="&amp;lpos=gallery_3">
                <picture>
                 <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/207000/207015.3.jpg" title=""/>
                </picture>
               </a>
              </div>
              <div class="content">
               <p>
                <span class="bold">
                 Feb 26, 2015
                </span>
                Afghanistan v Scotland, World Cup 2015, Group A, Dunedin
                <span class="copyright">
                 © ICC
                </span>
               </p>
              </div>
             </li>
             <li>
              <div class="thumbnail">
               <a href="/ci/content/gallery/836947.html" name="&amp;lpos=gallery_4">
                <picture>
                 <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/207600/207615.3.jpg" title=""/>
                </picture>
               </a>
              </div>
              <div class="content">
               <p>
                <span class="bold">
                 Feb 22, 2015
                </span>
                World Cup 2015
                <span class="copyright">
                 © Jarrod Kimber/ ESPNcricinfo
                </span>
               </p>
              </div>
             </li>
            </ul>
           </div>
          </section>
         </div>
         <!-- Top videos module start -->
         <section class="top-videos module">
          <div class="content-padding all-link">
           <h5 class="section-name">
            Videos
           </h5>
           <a href="/ci/content/video_audio/index.html?object=509587">
            All →
           </a>
          </div>
          <ul class="list-block">
           <li>
            <div class="featured">
             <div class="thumbnail">
              <div class="img-wrap">
               <div class="espni-vid-wrapper img-wrap">
                <div class="espni-vid-placeholder" data-assetname="" data-autostart="false" data-brightcoveurl="" data-duration="" data-embargodate="" data-expirationdate="" data-genre="index:interviews" data-headlineshort="" data-height="168" data-id="553fcdc2e4b053acb697a95c" data-mezzaninepathimage="http://a.espncdn.com/combiner/i?img=/media/motion/2015/0428/dm_150428_I-smith-watto-full/dm_150428_I-smith-watto-full.jpg?w=300&amp;h=168" data-sharebuttons="true" data-unicornurl="" data-width="100%">
                </div>
                <picture>
                 <img alt="" class="espni-vid-thumb full" src="http://a.espncdn.com/combiner/i?img=/media/motion/2015/0428/dm_150428_I-smith-watto-full/dm_150428_I-smith-watto-full.jpg?w=300&amp;h=168"/>
                </picture>
                <span class="video-play-button espni-vid-thumb ">
                 Play
                </span>
                <span class="video-length espni-vid-thumb">
                 20:18
                </span>
               </div>
              </div>
             </div>
             <div class="content">
              <h1>
               <a href="/ci/content/video_audio/867843.html">
                'No doubt in my mind that Steve will be the next captain'
               </a>
              </h1>
             </div>
            </div>
           </li>
           <li>
            <div class="thumbnail">
             <div class="img-wrap">
              <div class="espni-vid-wrapper img-wrap">
               <div class="espni-vid-placeholder" data-assetname="" data-autostart="false" data-brightcoveurl="" data-duration="" data-embargodate="" data-expirationdate="" data-genre="index:interviews" data-headlineshort="" data-height="168" data-id="553fcdc2e4b053acb697a95c" data-mezzaninepathimage="http://a.espncdn.com/combiner/i?img=/media/motion/2015/0428/dm_150428_I-smith-watto-full/dm_150428_I-smith-watto-full.jpg?w=300&amp;h=168" data-sharebuttons="true" data-unicornurl="" data-width="100%">
               </div>
               <picture>
                <img alt="" class="espni-vid-thumb full" src="http://a.espncdn.com/combiner/i?img=/media/motion/2015/0428/dm_150428_I-smith-watto-full/dm_150428_I-smith-watto-full.jpg?w=300&amp;h=168"/>
               </picture>
               <span class="video-play-button espni-vid-thumb ">
                Play
               </span>
               <span class="video-length espni-vid-thumb">
                20:18
               </span>
              </div>
             </div>
            </div>
            <div class="content">
             <h4>
              <span class="icon-font icon-play-solid">
              </span>
              <a href="/ci/content/video_audio/857647.html">
               Sledging mars World Cup win as Ashes squad revealed
              </a>
             </h4>
            </div>
           </li>
           <li>
            <div class="thumbnail">
             <div class="img-wrap">
              <div class="espni-vid-wrapper img-wrap">
               <div class="espni-vid-placeholder" data-assetname="" data-autostart="false" data-brightcoveurl="" data-duration="" data-embargodate="" data-expirationdate="" data-genre="index:bowlatboycs" data-headlineshort="" data-height="168" data-id="551a9969e4b0ca1ce4ef03a2" data-mezzaninepathimage="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_BAB_20150331/cric_150331_COM_CRICKET_BAB_20150331.jpg?w=300&amp;h=168" data-sharebuttons="true" data-unicornurl="" data-width="100%">
               </div>
               <picture>
                <img alt="" class="espni-vid-thumb full" src="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_BAB_20150331/cric_150331_COM_CRICKET_BAB_20150331.jpg?w=300&amp;h=168"/>
               </picture>
               <span class="video-play-button espni-vid-thumb ">
                Play
               </span>
               <span class="video-length espni-vid-thumb">
                21:02
               </span>
              </div>
             </div>
            </div>
            <div class="content">
             <h4>
              <span class="icon-font icon-play-solid">
              </span>
              <a href="/ci/content/video_audio/857629.html">
               'Money is the key to the length of the World Cup'
              </a>
             </h4>
            </div>
           </li>
           <li>
            <div class="thumbnail">
             <div class="img-wrap">
              <div class="espni-vid-wrapper img-wrap">
               <div class="espni-vid-placeholder" data-assetname="" data-autostart="false" data-brightcoveurl="" data-duration="" data-embargodate="" data-expirationdate="" data-genre="index:newsandanalysis" data-headlineshort="" data-height="168" data-id="551a91b0e4b0ca1ce4ef0384" data-mezzaninepathimage="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_ASSO_201503031/cric_150331_COM_CRICKET_ASSO_201503031.jpg?w=300&amp;h=168" data-sharebuttons="true" data-unicornurl="" data-width="100%">
               </div>
               <picture>
                <img alt="" class="espni-vid-thumb full" src="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_ASSO_201503031/cric_150331_COM_CRICKET_ASSO_201503031.jpg?w=300&amp;h=168"/>
               </picture>
               <span class="video-play-button espni-vid-thumb ">
                Play
               </span>
               <span class="video-length espni-vid-thumb">
                04:35
               </span>
              </div>
             </div>
            </div>
            <div class="content">
             <h4>
              <span class="icon-font icon-play-solid">
              </span>
              <a href="/ci/content/video_audio/857625.html">
               Chappell: Game's got to globalise in T20
              </a>
             </h4>
            </div>
           </li>
           <li>
            <div class="thumbnail">
             <div class="img-wrap">
              <div class="espni-vid-wrapper img-wrap">
               <div class="espni-vid-placeholder" data-assetname="" data-autostart="false" data-brightcoveurl="" data-duration="" data-embargodate="" data-expirationdate="" data-genre="index:newsandanalysis" data-headlineshort="" data-height="168" data-id="551a91b0e4b0ca1ce4ef0384" data-mezzaninepathimage="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_ASSO_201503031/cric_150331_COM_CRICKET_ASSO_201503031.jpg?w=300&amp;h=168" data-sharebuttons="true" data-unicornurl="" data-width="100%">
               </div>
               <picture>
                <img alt="" class="espni-vid-thumb full" src="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_ASSO_201503031/cric_150331_COM_CRICKET_ASSO_201503031.jpg?w=300&amp;h=168"/>
               </picture>
               <span class="video-play-button espni-vid-thumb ">
                Play
               </span>
               <span class="video-length espni-vid-thumb">
                04:35
               </span>
              </div>
             </div>
            </div>
            <div class="content">
             <h4>
              <span class="icon-font icon-play-solid">
              </span>
              <a href="/ci/content/video_audio/857489.html">
               Kamal angered by World Cup trophy snub
              </a>
             </h4>
            </div>
           </li>
          </ul>
         </section>
         <!-- Top videos module end -->
         <div class="boundary-meter-wrapper module hide-portrait hide-for-mob">
          <div class="content-padding">
           <h5 class="section-name">
            Boundary Meter
           </h5>
          </div>
          <object align="middle" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=9,0,28,0" height="80" id="boundry_meter" name="boundry_meter" width="295">
           <param name="allowScriptAccess" value="always"/>
           <param name="movie" value="http://www.espncricinfo.com/navigation/cricinfo/IPL_boundaryMeter.swf"/>
           <param flasvars='value="high"'/>
           <param name="quality" value="high"/>
           <param name="bgcolor" value="#ffffff"/>
           <param name="FlashVars" value="xmlFile=http://www.espncricinfo.com/navigation/cricinfo/six4_6537.xml"/>
           <embed align="middle" allowscriptaccess="always" bgcolor="#ffffff" flashvars="xmlFile=http://www.espncricinfo.com/navigation/cricinfo/six4_6537.xml" height="80" name="audio_file" pluginspage="http://www.macromedia.com/go/getflashplayer" quality="high" src="http://www.espncricinfo.com/navigation/cricinfo/IPL_boundaryMeter.swf" type="application/x-shockwave-flash" width="295"/>
          </object>
          <div class="explore-links">
           <ul>
            <li>
             <a href="/icc-cricket-world-cup-2015/engine/records/batting/most_fours_career.html?id=6537;type=tournament">
              MOST FOURS
             </a>
            </li>
            <li>
             <a href="/icc-cricket-world-cup-2015/engine/records/batting/most_sixes_career.html?id=6537;type=tournament">
              MOST SIXES
             </a>
            </li>
           </ul>
          </div>
         </div>
         <style>
          .boundary-meter-wrapper{
        text-align: center;
    }
    .boundary-meter-wrapper .content-padding{
       padding: 0;
    }
    .boundary-meter-wrapper .content-padding h5{
       padding: 20px 0 20px 20px;
    }
    .boundary-meter-wrapper .content-padding img{
       float: right;
       width: 90px;
       height: 23px;
       margin: 15px 10px 20px 0;
    }
    .boundary-meter-wrapper .explore-links ul{
        float: none;
    }
    .boundary-meter-wrapper .explore-links ul li{
        float: none;
        display: inline-block;
    }
         </style>
        </section>
       </section>
       <section class="intermediate hide-all show-portrait ch-bottom-sections">
        <!-- Editor Picks module start -->
        <section class="editor-picks hide-all show-portrait module">
         <div class="content-padding">
          <h5 class="section-name">
           <a href="/ci/content/story/editors_pick.html">
            Editor's Picks
           </a>
          </h5>
         </div>
         <!-- Editor Picks Items start-->
         <ul>
          <li>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/magazine/content/story/857201.html" name="&amp;lpos=editorspick_1">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209800/209873.5.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="content">
            <h2>
             <a href="/magazine/content/story/857201.html" name="&amp;lpos=editorspick_1">
              What did we learn from this World Cup?
             </a>
            </h2>
           </div>
           <ul class="link-cluster">
            <li>
             <span class="">
             </span>
             That there is a place for proper batsmanship in ODIs, that New Zealand punch above their weight, and that wickets win you matches. By
             <b>
              Ed Smith
             </b>
            </li>
           </ul>
          </li>
          <li>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/magazine/content/story/856297.html" name="&amp;lpos=editorspick_2">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/209700/209725.6.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="content">
            <h2>
             <a href="/magazine/content/story/856297.html" name="&amp;lpos=editorspick_2">
              The greatest time of our lives
             </a>
            </h2>
           </div>
           <ul class="link-cluster">
            <li>
             <span class="">
             </span>
             <b>
              Martin Crowe:
             </b>
             Whatever happens, the Australia-New Zealand World Cup final at the MCG will be the most divine fun
            </li>
           </ul>
          </li>
          <li>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/magazine/content/story/852755.html" name="&amp;lpos=editorspick_3">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/207100/207187.4.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="content">
            <h2>
             <a href="/magazine/content/story/852755.html" name="&amp;lpos=editorspick_3">
              Left-arm quicks rule
             </a>
            </h2>
           </div>
           <ul class="link-cluster">
            <li>
             <span class="">
             </span>
             <b>
              Numbers Game:
             </b>
             Mitchell Starc and Trent Boult have led the way, but other left-arm fast bowlers have also shone in this World Cup
            </li>
           </ul>
          </li>
          <li>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/magazine/content/story/852631.html" name="&amp;lpos=editorspick_4">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/208900/208993.5.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="content">
            <h2>
             <a href="/magazine/content/story/852631.html" name="&amp;lpos=editorspick_4">
              The serenity of Kane Williamson
             </a>
            </h2>
           </div>
           <ul class="link-cluster">
            <li>
             <span class="">
             </span>
             <b>
              Martin Crowe:
             </b>
             He is assured, humble and intelligent: a player opposition captains and bowlers find hard to combat
            </li>
           </ul>
          </li>
          <li>
           <div class="thumbnail">
            <div class="img-wrap">
             <picture>
              <a href="/icc-cricket-world-cup-2015/content/story/851397.html" name="&amp;lpos=editorspick_5">
               <img alt="" class="img-full" src="http://p.imgci.com/db/PICTURES/CMS/207100/207117.4.jpg" title=""/>
              </a>
             </picture>
            </div>
           </div>
           <div class="content">
            <h2>
             <a href="/icc-cricket-world-cup-2015/content/story/851397.html" name="&amp;lpos=editorspick_5">
              Build, build, blast off
             </a>
            </h2>
           </div>
           <ul class="link-cluster">
            <li>
             <span class="">
             </span>
             <b>
              Sharda Ugra:
             </b>
             This World Cup,most teams seem to have followed a strategy of constructing an innings till about the last quarter and then gone full pelt
            </li>
           </ul>
          </li>
         </ul>
         <!-- Editor Picks Items end-->
         <!-- Ad center column start -->
         <div class="ad-container ad-center-column ad-140">
          <div class="espni-ad-slot ad-panel" data-exclude-bp="s,m" data-kvpos="top" data-slot-type="panel">
          </div>
         </div>
         <!-- Ad center column end -->
         <!-- mpu Ad -->
         <div class="ad-container ad-center-column hide show-landscape">
          <div class="espni-ad-slot ad-incontent" data-exclude-bp="s,m,xl" data-kvpos="top" data-slot-type="incontent">
          </div>
         </div>
         <!-- mpu Ad End -->
        </section>
        <!-- Editor Picks module end -->
        <div class="ad-slot show-mobile">
         <div class="espni-ad-slot ad-incontent" data-exclude-bp="m,l,xl" data-kvpos="bottom" data-slot-type="incontent">
         </div>
        </div>
        <section class="ch-points-table module ">
         <div class="content-padding all-link">
          <h5 class="section-name">
           Points Table
          </h5>
          <a href="/icc-cricket-world-cup-2015/engine/series/509587.html?view=pointstable">
           All →
          </a>
         </div>
         <div class="group">
          <p class="heading">
           Pool A
          </p>
          <table>
           <tbody>
            <tr>
             <th class="bold">
              T
              <span>
               eams
              </span>
             </th>
             <th class="bold">
              M
              <span>
               at
              </span>
             </th>
             <th class="bold">
              W
              <span>
               on
              </span>
             </th>
             <th class="bold">
              L
              <span>
               ost
              </span>
             </th>
             <th class="bold">
              T
              <span>
               ied
              </span>
             </th>
             <th class="bold">
              N/R
             </th>
             <th class="bold last">
              P
              <span>
               ts
              </span>
             </th>
            </tr>
            <tr>
             <td>
              NZ
             </td>
             <td>
              6
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              12
             </td>
            </tr>
            <tr>
             <td>
              AUS
             </td>
             <td>
              6
             </td>
             <td>
              4
             </td>
             <td>
              1
             </td>
             <td>
              0
             </td>
             <td>
              1
             </td>
             <td class="last">
              9
             </td>
            </tr>
            <tr>
             <td>
              SL
             </td>
             <td>
              6
             </td>
             <td>
              4
             </td>
             <td>
              2
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              8
             </td>
            </tr>
            <tr>
             <td>
              BDESH
             </td>
             <td>
              6
             </td>
             <td>
              3
             </td>
             <td>
              2
             </td>
             <td>
              0
             </td>
             <td>
              1
             </td>
             <td class="last">
              7
             </td>
            </tr>
            <tr>
             <td>
              ENG
             </td>
             <td>
              6
             </td>
             <td>
              2
             </td>
             <td>
              4
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              4
             </td>
            </tr>
            <tr>
             <td>
              AFG
             </td>
             <td>
              6
             </td>
             <td>
              1
             </td>
             <td>
              5
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              2
             </td>
            </tr>
            <tr>
             <td>
              SCOT
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              0
             </td>
            </tr>
           </tbody>
          </table>
          <p class="heading">
           Pool B
          </p>
          <table>
           <tbody>
            <tr>
             <th class="bold">
              T
              <span>
               eams
              </span>
             </th>
             <th class="bold">
              M
              <span>
               at
              </span>
             </th>
             <th class="bold">
              W
              <span>
               on
              </span>
             </th>
             <th class="bold">
              L
              <span>
               ost
              </span>
             </th>
             <th class="bold">
              T
              <span>
               ied
              </span>
             </th>
             <th class="bold">
              N/R
             </th>
             <th class="bold last">
              P
              <span>
               ts
              </span>
             </th>
            </tr>
            <tr>
             <td>
              INDIA
             </td>
             <td>
              6
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              12
             </td>
            </tr>
            <tr>
             <td>
              SA
             </td>
             <td>
              6
             </td>
             <td>
              4
             </td>
             <td>
              2
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              8
             </td>
            </tr>
            <tr>
             <td>
              PAK
             </td>
             <td>
              6
             </td>
             <td>
              4
             </td>
             <td>
              2
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              8
             </td>
            </tr>
            <tr>
             <td>
              WI
             </td>
             <td>
              6
             </td>
             <td>
              3
             </td>
             <td>
              3
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              6
             </td>
            </tr>
            <tr>
             <td>
              IRE
             </td>
             <td>
              6
             </td>
             <td>
              3
             </td>
             <td>
              3
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              6
             </td>
            </tr>
            <tr>
             <td>
              ZIM
             </td>
             <td>
              6
             </td>
             <td>
              1
             </td>
             <td>
              5
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              2
             </td>
            </tr>
            <tr>
             <td>
              UAE
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              6
             </td>
             <td>
              0
             </td>
             <td>
              0
             </td>
             <td class="last">
              0
             </td>
            </tr>
           </tbody>
          </table>
         </div>
        </section>
        <section class="ch-statistics module ">
         <div class="content-padding">
          <h5 class="section-name">
           Statistics
          </h5>
         </div>
         <div class="content">
          <ul>
           <li>
            <h2>
             ICC Cricket World Cup, 2014/15
            </h2>
            <p>
             (in Australia/New Zealand ODIs)
            </p>
            <span>
             <a href="/icc-cricket-world-cup-2015/engine/records/batting/most_runs_career.html?id=6537;type=tournament">
              Most runs
             </a>
             |
            </span>
            <span>
             <a href="/icc-cricket-world-cup-2015/engine/records/bowling/most_wickets_career.html?id=6537;type=tournament">
              Most wickets
             </a>
             |
            </span>
            <span>
             <a href="/icc-cricket-world-cup-2015/engine/records/batting/most_runs_innings.html?id=6537;type=tournament">
              High scores
             </a>
             |
            </span>
            <span>
             <a href="/icc-cricket-world-cup-2015/engine/records/bowling/best_figures_innings.html?id=6537;type=tournament">
              Best bowling
             </a>
             |
            </span>
            <span>
             <a href="/icc-cricket-world-cup-2015/engine/records/index.html?id=6537;type=tournament">
              More
             </a>
            </span>
           </li>
          </ul>
          <ul>
           <li>
            <h2>
             Chappell-Hadlee Trophy, 2014/15
            </h2>
            <p>
             (Australia in New Zealand ODIs)
            </p>
            <span>
             <a href="/ci/engine/records/batting/most_runs_career.html?id=9897;type=series">
              Most runs
             </a>
             |
            </span>
            <span>
             <a href="/ci/engine/records/bowling/most_wickets_career.html?id=9897;type=series">
              Most wickets
             </a>
             |
            </span>
            <span>
             <a href="/ci/engine/records/batting/most_runs_innings.html?id=9897;type=series">
              High scores
             </a>
             |
            </span>
            <span>
             <a href="/ci/engine/records/bowling/best_figures_innings.html?id=9897;type=series">
              Best bowling
             </a>
             |
            </span>
            <span>
             <a href="/ci/engine/records/index.html?id=9897;type=series">
              More
             </a>
            </span>
           </li>
          </ul>
          <h2>
           One-Day Internationals
          </h2>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2">
            Overall |
           </a>
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=40;type=team">
            Afghanistan
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2;type=team">
            Australia
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=25;type=team">
            Bangladesh
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=1;type=team">
            England
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=6;type=team">
            India
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=29;type=team">
            Ireland
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=5;type=team">
            New Zealand
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=7;type=team">
            Pakistan
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=30;type=team">
            Scotland
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=3;type=team">
            South Africa
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=8;type=team">
            Sri Lanka
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=27;type=team">
            United Arab Emirates
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=4;type=team">
            West Indies
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=9;type=team">
            Zimbabwe
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2;type=host">
            Australia
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=5;type=host">
            New Zealand
           </a>
           |
          </span>
          <span>
           <a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2017;type=year">
            Year 2017
           </a>
          </span>
         </div>
        </section>
        <section class="ch-squad module ">
         <div class="content-padding">
          <h5 class="section-name">
           Squad
          </h5>
         </div>
         <div class="squad-dd">
          <dl class="dropdown">
           <dt>
            <a>
             <span>
              Bangladesh Squad
             </span>
            </a>
           </dt>
           <dd>
            <ul>
             <li>
              <a class="default" data-value="10723">
               Bangladesh Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10737">
               South Africa Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10749">
               Zimbabwe Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10753">
               Scotland Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10771">
               Australia Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10775">
               United Arab Emirates Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10729">
               Ireland Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10733">
               India Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10747">
               Pakistan Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10681">
               England Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10699">
               Afghanistan Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10773">
               West Indies Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10745">
               Sri Lanka Squad
              </a>
             </li>
             <li>
              <a class="" data-value="10751">
               New Zealand Squad
              </a>
             </li>
            </ul>
           </dd>
          </dl>
         </div>
         <div class="content">
          <ul class="list-block" id="squad_list_d">
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56007.html">
             Mashrafe Mortaza (c)
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/380354.html">
             Anamul Haque
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56268.html">
             Arafat Sunny
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/280734.html">
             Imrul Kayes
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56025.html">
             Mahmudullah
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/373696.html">
             Mominul Haque
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56029.html">
             Mushfiqur Rahim (wk)
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/300618.html">
             Nasir Hossain
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/300619.html">
             Rubel Hossain
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/373538.html">
             Sabbir Rahman
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56143.html">
             Shakib Al Hasan
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/436677.html">
             Soumya Sarkar
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/401057.html">
             Taijul Islam
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/56194.html">
             Tamim Iqbal
            </a>
           </li>
           <li>
            <a href="/icc-cricket-world-cup-2015/content/player/538506.html">
             Taskin Ahmed
            </a>
           </li>
          </ul>
         </div>
        </section>
       </section>
      </div>
     </div>
     <script language="javascript">
      var omniPageName = "homepage";
    	var omniSiteSection1 = "ICC Cricket World Cup 2015";
    	var omniCt = "section homepage";
    	var _PAGETYPE = "country_home";
    	var _SERIES = "series_home";
    	var _branding = "icc-cricket-world-cup-2015";
    	var _TEAMID = "509587";
     </script>
    </div>
    <!-- Homepage wrapper End -->
    <!-- Footer Start -->
    <div class="site-container">
     <footer>
      <div>
       <div class="footer-links">
        <ul class="horizontal">
         <li>
          <a class="maintabs ui-link" href="/ci/content/page/866033.html">
           Sitemap
          </a>
         </li>
         <li>
          <a class="maintabs ui-link" href="/ci/content/submit/forms/feedback.html">
           Feedback
          </a>
         </li>
         <li>
          <a class="maintabs ui-link" href="/ci/content/rss/feeds_rss_cricket.html">
           RSS
          </a>
         </li>
         <li>
          <a class="maintabs ui-link" href="/ci/content/page/156066.html">
           About Us
          </a>
         </li>
         <li>
          <a class="maintabs ui-link" href="/ci/content/site/careers/careers.html">
           Careers
          </a>
         </li>
         <!-- <li><a href="/ci/content/page/407211.html" class="maintabs ui-link">Advertise</a></li> -->
         <li>
          <a class="maintabs ui-link" href="http://disneyprivacycenter.com/" target="_blank">
           Privacy Policy
          </a>
         </li>
         <li>
          <a class="maintabs ui-link" href="http://disneytermsofuse.com/" target="_blank">
           Terms of Use
          </a>
         </li>
        </ul>
        <ul>
         <li>
          <a href="http://preferences-mgr.truste.com/?pid=disney01&amp;aid=espnemea01&amp;type=espnemea" target="_blank">
           Interest Based Ads
          </a>
         </li>
         <li>
          <a href="https://disneyprivacycenter.com/notice-to-california-residents/" target="_blank">
           Your California Privacy Rights
          </a>
         </li>
         <li>
          <a href="https://disneyprivacycenter.com/kids-privacy-policy/english/" target="_blank">
           Children’s Online Privacy Policy
          </a>
         </li>
         <li>
          <a href="http://www.nielsen.com/digitalprivacy" target="_blank">
           About Nielsen Measurement
          </a>
         </li>
        </ul>
       </div>
       <div class="footer-other-sites">
        <div class="logo-sprite">
         <img alt="" border="0" src="http://i.imgci.com/espncricinfo/redesign/espn-family-logo_3x.png" usemap="#Map"/>
         <map id="Map" name="Map">
          <area alt="" coords="1,1,53,2,52,16,1,16" href="http://espn.go.com/" shape="poly" title=""/>
          <area alt="" coords="72,1,93,2,93,17,72,17" href="http://www.espnf1.com/" shape="poly" title=""/>
          <area alt="" coords="109,1,180,1,181,16,109,16" href="http://www.espnscrum.com/" shape="poly" title=""/>
          <area alt="" coords="198,1,280,1,281,15,198,15" href="http://www.espnfc.com/" shape="poly" title=""/>
          <area alt="" coords="296,1,370,1,372,17,298,17" href="http://footytips.com.au" shape="poly" title=""/>
         </map>
        </div>
        <div class="logo-sprite copyright">
         <span>
          © ESPN Sports Media Ltd.
         </span>
        </div>
       </div>
      </div>
     </footer>
     <style>
      @media screen and (min-width: 769px){
        footer .footer-links ul{
            margin-bottom:10px;
        }
    }
     </style>
    </div>
    <!-- Footer End -->
    <script type="text/javascript">
     var endpoint = "submit.espncricinfo.com";
    	var server = "www.espncricinfo.com";
    </script>
    <!-- React.js -->
    <!--[if lte IE 8]>
                <script src="/navigation/cricinfo/ci/assets/js/plugins/shims.js"></script>
            <![endif]-->
    <script>
     __proxy = "http://54.186.53.178:1377/proxy/?url=";
                window.espni = window.espni || {};
                window.espni.vars = window.espni.vars || {};
                window.espni.vars.edition = '' || 'www';
                window.espni.vars.cluster = 'ind';
                window.espni.vars.country = 'in';
                window.espni.vars._cluster = '';
                window.espni.vars.version = '' || 'web';
                window.espni.vars.proxy = '' || false;
                window.espni.vars.headlinesPollInterval = '' || '300000';
                window.espni.vars.liveScoresPollInterval = '' || '20000';
                window.espni.vars.disableMEM = '';
                window.espni.vars.layoutId = '';
                
                window.espni.vars.netstorageUrl = 'http://ns.espncricinfo.com/netstorage/summary.json';
    </script>
    <!-- React.js -->
    <!-- VOD player dependencies -->
    <script src="http://i.imgci.com/navigation/cricinfo/ci/video/js/min/libs.min.js">
    </script>
    <script src="http://a.espncdn.com/combiner/c?js=jquery-1.7.1.js,plugins/jquery.metadata.js,plugins/jquery.pubsub.r5.js,plugins/ba-debug-0.4.js,espn.l10n.r12.js,espn.core.duo.r55.js,espn.storage.r6.js,espn.p13n.r16.js,espn.geo.r4.js">
    </script>
    <script type="text/javascript">
     var _u = _.noConflict();
    </script>
    <script type="text/javascript">
     window.espn_ui = window.espn_ui || {};
      espn_ui.Helpers = espn_ui.Helpers || {};
      espn_ui.Helpers.nav = espn_ui.Helpers.nav || {};
      espn_ui.Helpers.nav.isNavExternal = true;
      /*
      if(window.location.hostname.indexOf('kilimanjaro')<0) {
        window.espn = window.espn || {};
        espn.i18n = espn.i18n || {};
        espn.i18n.domain = window.location.hostname;
    }*/
    </script>
    <script src="http://a.espncdn.com/redesign/0.371.5/js/espncricinfo-critical.js">
    </script>
    <script>
     (function(){
          var deferLoaded = false;
          function loadDefer(){
              if( !deferLoaded ){
                  var deferScripts = ['http://a.espncdn.com/redesign/0.371.5/js/espn-defer-low.js'];
                  for( var s = 0; s < deferScripts.length; s++ ){
                      var script = document.createElement('script');
                      script.src = deferScripts[s];
                      document.getElementsByTagName('head')[0].appendChild(script);
                  }
                  deferLoaded = true;
              }
          }
          if( window.espn.loadType === "loadEnd" && ( espn_ui.device.isMobile === true || espn_ui.device.isTablet === true ) ){
              $( window ).load(function() {
                  setTimeout( loadDefer, 0 )
              });
              setTimeout( loadDefer, 5000 )
          }else{
              loadDefer();
          }
      })();
    </script>
    <script src="http://a.espncdn.com/redesign/0.371.5/external/release/js/nav-external.js">
    </script>
    <script>
     espn_ui.onefeed =true;
        espn_ui.externalRef = ""
        $(window).load(function() {
            $('#global-viewport img').each(function() {
                if (!this.complete || typeof this.naturalWidth == "undefined" || this.naturalWidth == 0) {
                    $(this).parent().remove();
                }
            });
        });
    </script>
    <script>
     if($.cookie('scoresLinkAlertCookie') !== "true"){
            $(".scores-link-alert").removeClass("hidden");
        }
    
        $('#global-scoreboard-trigger').add('.scores-link-alert').on("click", function(){
            // Cookie expiry time set to 1 year
            $.cookie('scoresLinkAlertCookie', true, {expires: 365});
            $(".scores-link-alert").remove();
        })
    </script>
    <script src="http://i.imgci.com/navigation/cricinfo/ci/assets/js/src/sub_nav.min.js?v=1500526114">
    </script>
    <!-- VOD Shim starts -->
    <script src="http://a.espncdn.com/prod/scripts/video/2.14.1-5/espn.video.universal.min.js">
    </script>
    <script src="http://i.imgci.com/navigation/cricinfo/ci/vod_player/js/min/video.1.0.0.js">
    </script>
    <!-- VOD Shim ends -->
    <script src="http://i.imgci.com/navigation/cricinfo/ci/assets/js/src/espncricinfo.gpt.1.0.0.min.js?v=1501244305">
    </script>
    <link href="http://i.imgci.com/navigation/cricinfo/ci/packager/dist/css/home-0.0.1.min.css?v=1459760940"/>
    <link href="http://a.espncdn.com/combiner/c?css=fonts/bentonsans.css,fonts/bentonsansbold.css,fonts/bentonsanslight.css,fonts/bentonsansmedium.css,fonts/bentonsanscond.css,fonts/bentonsanscondbold.css,fonts/bentonsanscondmedium.css">
     <script src="http://i.imgci.com/navigation/cricinfo/ci/packager/dist/js/countryhome-0.0.4.min.js?v=1500549439">
     </script>
     <script type="text/javascript">
      (function(){
                $(".multimedia img, li img:not(img[data-hide-thumbs]), .cric-facebook iframe").unveil();
                //delayed gpt initialization till everything loads
                var everythingLoaded = setInterval(function() {
                  if (/loaded|complete/.test(document.readyState)) {
                    clearInterval(everythingLoaded);
                    initGPTBundle();
                  }
                }, 10);
    
                $(function(){
                    $(".multimedia").on("click", ".bx-next",function(){$('.multimedia li img').unveil();});
                    $('#tcmslider-300-190').bxSlider({
                        responsive: 'true',
                        prevSelector: 'false',
                        nextSelector: 'false',
                        mode: 'fade',
                        preloadImages: 'visible',
                        autoStart: true,
                        randomStart: true,
                        autoDirection: 'next',
                        auto: true
                    });
                    $('.medium-image.slider-wrapper').css({'visibility':'visible', 'max-height':''});
                    $.cookie('bentonLoaded', true, {path:'/', domain:'espncricinfo.com'});
                });
    
                   //Initialize GPT for new redesigned templates with bundle
                function initGPTBundle(){
                    //return if Parms.debug is not true
                    if (typeof __GPTenabled === 'undefined'){
                        return;
                    }
                   //pubsub plugin init
                   (function(d){var cache={};d.publish=function(topic,args){cache[topic]&&d.each(cache[topic],function(){this.apply(d,args||[])})};d.subscribe=function(topic,callback){if(!cache[topic]){cache[topic]=[]}cache[topic].push(callback);return[topic,callback]};d.unsubscribe=function(handle){var t=handle[0];cache[t]&&d.each(cache[t],function(idx){if(this==handle[1]){cache[t].splice(idx,1)}})}})(jQuery);
    
                  $.subscribe("ad.rendered", function(key, event) {
                     // if the overlay slot is empty, we can load in-page ad units immediately
                     if(event.slot.type === "overlay") {
                        if(event.isEmpty === true && espni.ads.config.delayInPageAdSlots === true) {
                           espni.ads.refreshInPageAdSlots();
                        }
                     }
                  });
    
                  // overlays will publish an event when auto-closed or closed by user
                  $.subscribe("ad.overlay.close", function() {
                     if(espni.ads.config.delayInPageAdSlots === true) {
                        espni.ads.refreshInPageAdSlots();
                     }
                  });
    
                  //__GPTconfig variable comes from the espncricinfogpt api;
                  if(typeof __GPTconfig !== 'undefined'){
                    espni.ads.init(__GPTconfig);
                  }
               }
            }());
     </script>
     <script type="text/javascript">
      (function(d, s, id) {
              var js, fjs = d.getElementsByTagName(s)[0];
              if (d.getElementById(id)) return;
              js = d.createElement(s); js.id = id;
              js.src = "//connect.facebook.net/en_US/sdk.js?&appId=260890547115&version=v2.1";
              fjs.parentNode.insertBefore(js, fjs);
            }(document, 'script', 'facebook-jssdk'));
            window.fbAsyncInit = function() {
                FB.init({
                    appId      : '260890547115',
                    xfbml      : true,
                    status     : true, // check login status
                    cookie     : true, // enable cookies to allow the server to access the session
                    oauth      : true,
                    version    : 'v2.1'
                });
                if(typeof cilogin !== "undefined"){
                    cilogin.facebookapiinit();
                }
            };
     </script>
     <!-- add analytics script to HEAD tag -->
     <script src="http://a.espncdn.com/combiner/c?js=analytics/VisitorAPI.js,analytics/sOmni.2.js" type="text/javascript">
     </script>
     <script type="text/javascript">
      (function() {
            var countryRegion = "in";
            var editionKeyMap = {
                "espncricinfo-en-uk":"United Kingdom",
                "espncricinfo-en-us":"United States",
                "espncricinfo-en-in":"India",
                "espncricinfo-en-au": "Australia",
                "espncricinfo-en-za": "Africa",
                "espncricinfo-en-bd": "Bangladesh",
                "espncricinfo-en-ww": "Global",
                "espncricinfo-en-nz": "New Zealand",
                "espncricinfo-en-pk": "Pakistan",
                "espncricinfo-en-lk": "Srilanka"
                };
    
            if(typeof omniSiteSection1 == 'undefined') {
                omniSiteSection1 = '';
            }
            if(typeof omniPageName == 'undefined') {
                omniPageName = '';
            }
            if(typeof omniCt == 'undefined') {
                omniCt = '';
            }
    
            if(typeof omniSiteSection2 != 'undefined' && typeof omniSiteSubSection3 != 'undefined'){
                omni_PageName = omniSiteSection1.toLowerCase() + ":" + omniSiteSection2.toLowerCase() + ":" + omniSiteSection3.toLowerCase() + ":" + omniPageName.toLowerCase();  //Page name
            }else if(typeof omniSiteSection2 != 'undefined'){
                omni_PageName = omniSiteSection1.toLowerCase() + ":" + omniSiteSection2.toLowerCase() + ":" + omniPageName.toLowerCase();                                                 //Page name
            }else{
                omni_PageName = omniSiteSection1.toLowerCase() + ":" + omniPageName.toLowerCase();                                                                                                    //Page name
            }
    
            if(typeof omniSiteSection1 != 'undefined'){
                omni_channel = omniSiteSection1.toLowerCase();
            }
    
            var orient = (window.innerWidth >= window.innerHeight) ? 'landscape' : 'portrait';
            if ( espn && espn.track ) {
                if(espn.i18n.editionKey) {
                    countryRegion = editionKeyMap[espn.i18n.editionKey];
                }
                espn.track.init = function () {
                    var data = {
                        omniture: {
                            site: "cricinfo",
                            pageName: omni_PageName,
                            sections: omni_channel,
                            premium: "n",
                            account: "wdgespcricinfo",
                            sport: "cricket",
                            contentType: omniCt,
                            convrSport: "cricket",
                            lang: "en",
                            section: omniSiteSection1,
                            countryRegion: countryRegion,
                            fantasyPersonalize: "has favorites:no-fantasy:no-notifications:no-docking:no-autostart:no",
                            orientation : orient,
                            deviceInfo : orient,
                            prop5:omni_PageName,
                            prop29:"anonymous",
                            enableCB: false
                        },
                        chartbeat: {
                            uid: 26455,
                            domain: "espncricinfo.com",
                            useCanonical: true,
                            sections: "Series",
                            authors: "in",
                            path: window.location.pathname,
                            title: "ICC Cricket World Cup 2015 | Cricket news, live scores, fixtures, features and statistics on ESPN Cricinfo",
                            loadPubJS: false,
                            loadVidJS: false
                        },
                    };
                    // Init Chartbeat
                    espn.track.initCB( data.chartbeat );
                    // Init Comscore (optional - trackpage call )
                    //espn.track.comscore(); 
                    // Global analytics properties used by video players
                    var anData = data.omniture;
                    window.s_account = anData.account;
                    window.omniSite = anData.site;
                    window.omniPageName = anData.site+":"+anData.pageName;
                    window.omniTrackingName = "cities";
                     
                    // slight delay
                    function init_o() {
                        // detect device orientation however before page track call
                        var isMobile = function() {
                            return ( /android|webos|iphone|ipad|ipod|blackberry|iemobile|opera mini/i.test(navigator.userAgent.toLowerCase() ) ) ? true : false;
                        };
                        var orient = isMobile()  ? ( (window.innerWidth >= window.innerHeight) ? 'landscape' : 'portrait' ) : 'desktop';
                        // page track call
                        espn.track.trackPage( anData );
                  
                        $.subscribe("omni.pageTracked", function(s_omni) {
                            // do something trigger after tracing
                        });
                    }
                    // can inite events to add additional tracking or trigger publish evient
                    if(typeof s_omni === "undefined") {
                        
                        setTimeout(function(){
                  
                            $.ajaxSetup({ cache: true });
                            // we want to load the analytics files from the cache if possible - so, let's use full $.ajax() calls
                            $.getScript("http://a.espncdn.com/combiner/c?js=analytics/VisitorAPI.js,analytics/sOmni.2.js,analytics/analytics.2.js,analytics/externalnielsen.js", init_o);
                  
                            $.subscribe("omni.loaded", function(s_omni) {
                                // do something
                            });
                  
                        }, 500);
                    }
                    else {
                        init_o();
                    }
                };
                espn.track.init();
            }
        })();
     </script>
     <style type="text/css">
      .cookie-overlay{background-color:rgba(0,0,0,.75);bottom:0;position:fixed;width:100%;padding:15px 0;text-align:center;z-index:9999;display:none;}.cookie-overlay h1{font-size:18px;font-weight:600;text-transform:uppercase;width:90%;margin:0 auto;color:#fff}.cookie-overlay p.message{width:90%;margin:0 auto;padding:5px 0 10px;color:#fff}.cookie-overlay p.message a{font-family:BentonSansBold,Arial,sans-serif;color:#fff}.cookie-overlay p.message a:hover{color:#439ec9;text-decoration:underline}.cookie-overlay .cookie-continue{display:block;font-size:11px;margin:auto;width:140px;line-height:2.8;padding:0 15px;max-width:320px;min-width:120px;border-radius:4px;background-color:#06c;color:#fff;cursor:pointer;border:0;font-family:BentonSansMedium,Arial,sans-serif}.cookie-overlay .cookie-continue:hover{background-color:#007aff}@media screen and (min-width:0) and (max-width:768px){.cookie-overlay p.message{font-size:12px}}
     </style>
     <div class="cookie-overlay">
      <h1>
       ABOUT COOKIES
      </h1>
      <p class="message">
       We use cookies to help make this website better, to improve our services and for advertising purposes. You can learn more about our use of cookies and change your browser settings in order to avoid cookies by clicking
       <a href="https://disneyprivacycenter.com/cookies-policy-translations/cookies-policy/">
        here
       </a>
       . Otherwise, we'll assume you are OK to continue.
      </p>
      <button class="cookie-continue">
       Continue
      </button>
     </div>
     <script type="text/javascript">
      /**
       * Should cookie message get displayed for this cluster?
       * @return {boolean} true if cookie message needs to be shown else false
       */
      function __validateCookieClusters(){
        var __isUKCluster = (location_cluster == 'uk') ? true : false
        var __isEuroCluster = (location_cluster == 'eur') ? true : false
        var __countryArr = ["at","be","bg","hr","cy","cz","dk","ee","fi","fr",
                            "de","gr","hu","ie","it","lv","lt","lu","mt","nl",
                            "pl","pt","ro","sk","si","es","se"];
    
        // Show cookie message only for uk and other eu countries
        var showCookieMessage = __isUKCluster || (__isEuroCluster && (__countryArr.indexOf(location_country) > -1) )
        return showCookieMessage;
      }
    
      /**
       * Display cookie message for specified clusters
       */
      function __displayCookie(){
        // Check if jQuery is present on page else return
        if (typeof $ === 'undefined') {
          return
        }
    
        if($.cookie('about_cookie') == "1"){
            $(".cookie-overlay").hide();
        }else{
            $(".cookie-overlay").show();
        }
    
        $(".cookie-continue").on("click", function(){
            //var __expireTime = new Date;
            //__expireTime.setFullYear(__expireTime.getFullYear( ) +1);
            //$.cookie('about_cookie', '1', {expires: __expireTime, path:'/', domain:'.espncricinfo.com'});
    
            // Cookie expiry time set to 1 year
            $.cookie('about_cookie', '1', {expires: 365, path:'/', domain:'.espncricinfo.com'});
            $(".cookie-overlay").hide();
        })
      }
    
      if (typeof jQuery !== 'undefined') {
        // Setup on ready callback only for specified clusters
        if (__validateCookieClusters() === true) {
          $(document).ready(__displayCookie);
        }
      }
     </script>
     <script type="text/javascript">
      (function(espn, $, navigator) {
    	window.espn = espn || {};
    	espn.bandwidth = {
    		test: function(callback, options) {
    			var count = 0,
    				files = [
    					{ url: 'http://a.espncdn.com/i/data-lite/2.jpg', bytes: 65536 }
    				];
    
    			callback = callback || function() {};
    			options = options || {};
    
    			function _done() {
    				var bandwidth = Math.max.apply(Math, files.map(function(file) {
    					return file.bandwidth || -Infinity;
    				}));
    				callback(bandwidth, {downloads: count});
    			}
    
    			function _calculateBandwidthForFile(file) {
    				var seconds = (file.endTime - file.startTime) / 1000,
    					kb = file.bytes * 8 / 1000;
    				if (seconds > 0) {
    					return Math.floor(kb / seconds);
    				}
    			}
    
    			function _download(file) {
    				var xhr = new XMLHttpRequest();
    				xhr.onloadstart = function() {
    					file.startTime = +new Date;
    				};
    				xhr.onreadystatechange = function() {
    					if (xhr.readyState === 4 && xhr.status === 200) {
    						file.endTime = +new Date;
    						file.bandwidth = _calculateBandwidthForFile(file);
    
    						if (++count === files.length) {
    							_done();
    						} else if (options && options.max && file.bandwidth >= options.max) {
    							// no need to complete the bandwidth test as the bandwidth is higher than the specified max (threshold)
    							_done();
    						} else if (options && options.min && file.bandwidth < options.min) {
    							// the download was too slow - don't continue the bandwidth test
    							_done();
    						} else {
    							_download(files[count]);
    						}
    					}
    				};
    
    				xhr.open('GET', file.url + '?rand=' + Math.floor(Math.random()*100000000)); // append rand param to avoid caching locally
    				file.startTime = +new Date; // in case onloadstart (above) isn't supported
    				xhr.send();
    			}
    
    			// init serial download
    			_download(files[0]);
    		}
    	};
    	
    	if ($) {
    		$(window).on('load', function() {
    			if (window.espn && espn.geo && espn.bandwidth && espn.track) {
    				var cookieName = '_data-lite-mobile-bw',
    					SAMPLE_PCT = 20;
    				if (!$.cookie(cookieName) && ((Math.random()*100) <= SAMPLE_PCT)) {
    					espn.geo.getLocation().done(function(geo) {
    						if (geo.connection === 'mobile') {
    							espn.bandwidth.test(function(bandwidth) {
    								
    								var ROUND_TO_NEAREST = 100,
    								    rounded = ROUND_TO_NEAREST * Math.round(bandwidth/ROUND_TO_NEAREST);
    								
    								espn.track.userClicked = true;
    								espn.track.trackLink({
    									site: 'cricinfo',
    									link: true,
    									prop58: rounded
    								});
    								$.cookie(cookieName, true);
    							});
    						}
    					});
    				}
    			}
    		});
    	}
    })(window.espn, window.jQuery, window.navigator);
     </script>
    </link>
    
    


```python
display(HTML(page.text))
```


<!DOCTYPE html>
<!-- hostname: web005, edition-view: espncricinfo-en-in, country: in, cluster: ind, created: 2017-12-24 19:04:52 -->
<!--[if IE 8]><html class="lt-ie9" lang="en" xmlns="http://www.w3.org/1999/xhtml" xmlns:fb="http://www.facebook.com/2008/fbml"> <![endif]-->
<!--[if IE 9]><html class="ie9" lang="en" xmlns="http://www.w3.org/1999/xhtml" xmlns:fb="http://www.facebook.com/2008/fbml"> <![endif]-->
<!--[if gt IE 9]><!--> <html lang="en"> <!--<![endif]-->
<head>
    <meta http-equiv="Content-Type" content="text/html;charset=utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <script type="text/javascript">var _sf_startpt=(new Date()).getTime()</script>
    <meta name="google-site-verification" content="ZxdgH3XglRg0Bsy-Ho2RnO3EE4nRs53FloLS6fkt_nc" />
    <title>ICC Cricket World Cup 2015 | Cricket news, live scores, fixtures, features and statistics on ESPN Cricinfo</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    
    
    <meta name="keywords" content="ICC Cricket World Cup 2015, Live scores, Ball by ball update, Live scores, Live statistics, Fantasy Cricket, Player profile, Squads, Ground profile, Latest news, Latest features." />
    
    <meta name="description" content="ICC Cricket World Cup 2015 with live cricket scores and the latest news and features throughout the series." />

    <!-- Include isMobile script inline -->
    <script>
/**
 * isMobile.js v0.3.6
 *
 * @author: Kai Mallea (kmallea@gmail.com)
 *
 * @license: http://creativecommons.org/publicdomain/zero/1.0/
 */
!function(a){var b=/iPhone/i,c=/iPod/i,d=/iPad/i,e=/(?=.*\bAndroid\b)(?=.*\bMobile\b)/i,f=/Android/i,g=/IEMobile/i,h=/(?=.*\bWindows\b)(?=.*\bARM\b)/i,i=/BlackBerry/i,j=/BB10/i,k=/Opera Mini/i,l=/(?=.*\bFirefox\b)(?=.*\bMobile\b)/i,m=new RegExp("(?:Nexus 7|BNTV250|Kindle Fire|Silk|GT-P1000)","i"),n=function(a,b){return a.test(b)},o=function(a){var o=a||navigator.userAgent;return this.apple={phone:n(b,o),ipod:n(c,o),tablet:n(d,o),device:n(b,o)||n(c,o)||n(d,o)},this.android={phone:n(e,o),tablet:!n(e,o)&&n(f,o),device:n(e,o)||n(f,o)},this.windows={phone:n(g,o),tablet:n(h,o),device:n(g,o)||n(h,o)},this.other={blackberry:n(i,o),blackberry10:n(j,o),opera:n(k,o),firefox:n(l,o),device:n(i,o)||n(j,o)||n(k,o)||n(l,o)},this.seven_inch=n(m,o),this.any=this.apple.device||this.android.device||this.windows.device||this.other.device||this.seven_inch,this.phone=this.apple.phone||this.android.phone||this.windows.phone,this.tablet=this.apple.tablet||this.android.tablet||this.windows.tablet,"undefined"==typeof window?this:void 0},p=function(){var a=new o;return a.Class=o,a};"undefined"!=typeof module&&module.exports&&"undefined"==typeof window?module.exports=o:"undefined"!=typeof module&&module.exports&&"undefined"!=typeof window?module.exports=p():"function"==typeof define&&define.amd?define("isMobile",[],a.isMobile=p()):a.isMobile=p()}(this);
</script>


<!--[if IE 9]>
<script language="javascript" type="text/javascript">
function fnCreateJumpList(iScenario) {
fnClearJumpList();
window.external.msSiteModeCreateJumpList("Quick Links")
window.external.msSiteModeAddJumpListItem('TCM', 'http://www.espncricinfo.com/ci/content/current/url/784439.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('Both Ends', 'http://www.espncricinfo.com/ci/content/current/url/1094910.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('MATCH DAY', 'http://www.espncricinfo.com/ci/content/current/url/1065560.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('CRICIQ', 'http://www.espncricinfo.com/ci/content/current/url/1095890.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('Insights', 'http://www.espncricinfo.com/ci/content/current/url/834953.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('WI v IND', 'http://www.espncricinfo.com/west-indies-v-india-2017/content/current/series/1098203.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('ENG v SA', 'http://www.espncricinfo.com/england-v-south-africa-2017/content/current/series/1031417.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('WWC', 'http://www.espncricinfo.com/icc-womens-world-cup-2017/content/current/series/1085935.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeAddJumpListItem('Match Highlights', 'http://www.espncricinfo.com/ci/content/current/url/1107251.html', 'http://a.espncdn.com/espncricinfo/favicon.ico');window.external.msSiteModeShowJumpList();
}
function fnClearJumpList() {
window.external.msSiteModeClearJumplist();
}
</script>

<meta name="msapplication-task" content="name=Live Scores;action-uri=http://www.espncricinfo.com/ci/engine/current/match/scores/live.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
<meta name="msapplication-task" content="name=Latest News;action-uri=http://www.espncricinfo.com/ci/content/current/story/news.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
<meta name="msapplication-task" content="name=Fixtures;action-uri=http://www.espncricinfo.com/ci/content/current/match/fixtures/index.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
<meta name="msapplication-task" content="name=Results;action-uri=http://www.espncricinfo.com/ci/engine/current/match/scores/recent.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
<meta name="msapplication-task" content="name=Photos;action-uri=http://www.espncricinfo.com/ci/content/current/image/index.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
<meta name="msapplication-task" content="name=Audio/Video;action-uri=http://www.espncricinfo.com/ci/content/video_audio/index.html;icon-uri=http://a.espncdn.com/espncricinfo/favicon.ico"/>
<script language="javascript" type="text/javascript">
        fnCreateJumpList(2);
</script>
<![endif]-->
<meta name="robots" content="index, follow" />
<meta name="googlebot" content="index, follow" />
<meta property="fb:app_id" content="260890547115" />
<meta property="og:site_name" content="Cricinfo" />
<link rel="shortcut icon" href="http://a.espncdn.com/espncricinfo/favicon.ico" />
<link rel="icon" type="image/png" href="http://a.espncdn.com/espncricinfo/favicon.png" />
<link rel="icon" type="image/gif" href="http://a.espncdn.com/espncricinfo/favicon.gif" />
<link rel="apple-touch-icon" href="http://a.espncdn.com/wireless/mw5/r1/images/bookmark-icons/espncricinfo_icon-57x57.min.png" />
<link rel="apple-touch-icon-precomposed" href="http://a.espncdn.com/wireless/mw5/r1/images/bookmark-icons/espncricinfo_icon-57x57.min.png">
<link rel="apple-touch-icon-precomposed" sizes="72x72" href="http://a.espncdn.com/wireless/mw5/r1/images/bookmark-icons/espncricinfo_icon-72x72.min.png">
<link rel="apple-touch-icon-precomposed" sizes="114x114" href="http://a.espncdn.com/wireless/mw5/r1/images/bookmark-icons/espncricinfo_icon-114x114.min.png">
<link rel="apple-touch-icon-precomposed" sizes="152x152" href="http://a.espncdn.com/wireless/mw5/r1/images/bookmark-icons/espncricinfo_icon-152x152.min.png">
<meta name="application-name" content="ESPNcricinfo"/>
<meta name="msapplication-TileColor" content="#266ab4"/>
<meta name="msapplication-TileImage" content="http://i.imgci.com/espncricinfo/6b245241-3938-499c-8c79-9b80f97bed96.png"/>

<meta property="og:title" content="ICC Cricket World Cup"/>
<meta property="og:type" content="sport"/>
<meta property="og:image" content="http://i.imgci.com/espncricinfo/facebook/2.jpg" />
<link rel="image_src" href="http://i.imgci.com/espncricinfo/facebook/2.jpg" />
<meta property="og:description" content="ICC Cricket World Cup 2015 with live cricket scores and the latest news and features throughout the series."/>

<link rel="canonical" href="http://www.espncricinfo.com/icc-cricket-world-cup-2015/content/series/509587.html"/>
<meta property="og:url" content="http://www.espncricinfo.com/icc-cricket-world-cup-2015/content/series/509587.html" />

<!-- Load GPT JavaScript library used by DFP -->
<script type="text/javascript">
  var googletag = googletag || {};
  googletag.cmd = googletag.cmd || [];
  (function() {
    var gads = document.createElement("script");
    gads.async = true;
    gads.type = "text/javascript";
    var useSSL = true;
    // var useSSL = "https:" == document.location.protocol;
    gads.src = (useSSL ? "https:" : "http:") + "//www.googletagservices.com/tag/js/gpt.js";
    var node =document.getElementsByTagName("script")[0];
    node.parentNode.insertBefore(gads, node);
   })();
</script>
<script type="text/javascript" src="http://a.espncdn.com/combiner/c?js=swfobject/2.2/swfobject.js?minify=false"></script>


<style>
.espni-ad-slot{line-height: 0;}
pre.gpt-debug{padding: 10px; background: burlywood;}
</style>

  <script type='text/javascript'> var ad_setting = {"adtar":"","kvbrand":"icccricketworldcup","kvcluster":"ind","kvnavtype":"serieshome","kvpt":"serieshome","kvseriesid":"509587","kvsite":"icccricketworldcup","networkid":"6444","path":"/6444/espn.cricinfo.com/series/icccricketworldcup/homepage","sp":"cricinfo","template":"desktop","template_type":"responsive"}; var __GPTenabled = true; </script>




    
	<link rel="stylesheet" href="http://a.espncdn.com/combiner/c?css=fonts/bentonsans.css,fonts/bentonsansbold.css,fonts/bentonsanslight.css,fonts/bentonsansmedium.css,fonts/bentonsanscond.css,fonts/bentonsanscondbold.css,fonts/bentonsanscondmedium.css" type="text/css" media="screen" charset="utf-8">
    <link rel="stylesheet" href="http://i.imgci.com/navigation/cricinfo/ci/assets/css/sub_nav.css?v=1500525949" type="text/css" media="screen" charset="utf-8">
    <link type="text/css" rel="stylesheet" href="http://i.imgci.com/navigation/cricinfo/ci/assets/css/cilogin.1.0.4.css?v=1443427381" media="all" />

    <link rel="stylesheet" href="http://i.imgci.com/navigation/cricinfo/ci/redesign/css/homepage/homepage.css?v=1479985888">
    <link rel="stylesheet" href="http://i.imgci.com/navigation/cricinfo/ci/assets/css/cilogin.1.0.4.css?v=1">

<link type="text/css" rel="stylesheet" href="http://i.imgci.com/navigation/cricinfo/ci/redesign/css/oneid/forms.css?v=" media="all" />


<link rel="stylesheet" href="http://i.imgci.com/navigation/cricinfo/ci/vod_player/css/espn-web-player-bundle.1.0.1.css?v=1510209320" />






<link rel="stylesheet" media="print" href="http://i.imgci.com/navigation/cricinfo/ci/redesign/css/common/print.css?v=1506494239">

    <!--[if lt IE 9]>
        <script src="/navigation/cricinfo/ci/assets/js/plugins/html5.js"></script>
        <link rel="stylesheet" href="/navigation/cricinfo/ci/redesign/css/common/ie8.css?v=1432640863">
    <![endif]-->
    
    <script src="http://i.imgci.com/navigation/cricinfo/ci/assets/js/libs/modernizr-2.8.3.min.js"></script>
    
    <script language="javascript"  type="text/javascript">
                    cqanswer = 'in';
                    location_country = 'in';
            location_cluster = 'ind';
        ord=Math.random()*10000000000000000;
        var __hpad300_100Web = 1;
        var ad_counter = 1;
        var ad_counter_mobile = 1;
        var devicenames = ['iPhone','BB10','Android','Windows Phone','Fennec','GoBrowser','NokiaBrowser','S40OviBrowser','SymbianOS','NokiaN73','Symbian','Opera Mini','Opera Mobi','Minimo','IEMobile','Mobile Safari','BlackBerry','sonyericsson'];
    </script>


<style type="text/css">
#adform-container { width: auto!important; }
</style>

</head>
<body class="country-home pagetype-home">

<div id="viewport">
<div class="espni-ad-slot ad0" data-slot-type="overlay" data-kvpos="outofpage" data-out-of-page="true"></div><div class="espni-ad-slot ad5" data-slot-type="wallpaper" data-kvpos="top" data-exclude-bp="s,m,l"></div>

    <div id="fb-root"></div>

    <!-- Win8 Prompt (existing code) -->
    
    <!-- Win8 Prompt (existing code) -->

    <div class="hide-for-mob">
        
    </div>



<script type="text/javascript">
  window.espn = window.espn || {};
  espn.isOneSite = true;
</script>

<script src="http://cdn.espn.com/core/format/modules/head/i18n?edition-host=espncricinfo.com&type=ext&site=espncricinfo&region=in&lang=en&build=0.371.5"></script>
<link href='http://a.espncdn.com' rel='preconnect' crossorigin>
<!--<link rel="stylesheet" href="http://local.espncricinfo.com:8080/static/css/transitional-header-cricinfo.css" />
<link rel="stylesheet" href="/navigation/cricinfo/ci/redesign/css/transitional-header/transitional-header-cricinfo.css">-->
<link rel="stylesheet" href="http://a.espncdn.com/redesign/0.371.5/css/transitional-header-cricinfo.css" />
<style>
  #global-viewport #header-wrapper{
    position: relative !important;
  }
  #global-viewport{    
    font-size: 16px !important;
    font-family: sans-serif !important;
  }
  body {
    background: #edeef0 !important;
  }
  #global-viewport .footer{
    background: transparent;
    text-transform: initial !important;
  }
  .global-search .search-results ul a {
    font-size: 12px;
    line-height: 16px;    
  }
  .global-search .search-results ul a b {
    font-family: BentonSans, -apple-system, Roboto, Helvetica, Arial, sans-serif !important;
  }
  #global-nav .pillar.editions > div{
    left:auto !important;
  }
  .feed-title {
    line-height: 24px !important;
    text-transform: uppercase;
    font-family: "BentonSans",-apple-system,"Roboto",Helvetica,Arial,sans-serif !important;
  }
  .feed-title.favorites span {
    color: #06c;
    cursor: pointer;
    display: block;
    font-weight: 400;
    line-height: 13px;
    position: absolute;
    right: 10px;
    text-transform: none;
    top: 5px;
  }
  #global-scoreboard-trigger+.alert{
    padding: 12px 35px 12px 18px;
    font-size: 12px;
  }
  .hidden{
    display: none !important;
  }
  @media screen and (min-width: 768px) {
    .scores-link-alert{
      display: none !important;
    }
  }
  @media screen and (min-width: 1024px) {
    .global-user .current-favorites {
      padding: 0 10px 34px 20px !important;
    }
  }
</style>

<script src="http://a.espncdn.com/redesign/0.371.5/js/espn-head.js"></script>
<div id="global-viewport" data-behavior="global_nav_condensed global_nav_full"  class ="espncricinfo transitional-header legacy-pages">
  <nav id="global-nav-mobile" data-loadtype="client"></nav>
  <div class="menu-overlay-primary"></div>
  <div id="header-wrapper" data-behavior="global_header" class="hidden-print">
    <header id="global-header" class="espncricinfo-en user-account-management has-search">
      <div class="menu-overlay-secondary"></div>
        <div class="container">
          <a id="global-nav-mobile-trigger" href="#" data-route="false"><span>Menu</span></a><h1><a href="/"  name="&lpos=sitenavdefault&lid=sitenav_main-logo">ESPN</a></h1>
          <ul class="tools">
            <li class="search">
              <a href="#" class="icon-font-after icon-search-solid-after" id="global-search-trigger"></a>
                <div id="global-search" class="global-search">
                    <input type="text" class="search-box" placeholder="Search Sports, Teams or Players..."><input type="submit" class="btn-search">
                </div>
            </li>
            <li class="user" data-behavior="favorites_mgmt"></li>
            <li>
              <a href="#" id="global-scoreboard-trigger" data-route="false">Scores</a>
              <div class="hidden scores-link-alert scores-link-alert alert alert_button--close icon-font-after icon-close-solid-after" data-behavior="scores_link_alert">All cricket scores, fixtures and results here.</div></li>
            </li>
          </ul>
        </div>
        <nav id="global-nav" data-loadtype="server">
          <ul itemscope="" >

          </ul>
        </nav>
        <nav id="global-nav-secondary" data-loadtype="server" >
          <div class="global-nav-container">    
          </div>
        </nav>

    </header>
  </div>
</div>


	   <div class="subnav-wrap sub-nav  icc-cricket-world-cup-2015">
        <div class="row">
          <div class="large-20 sub-nav-wrap" id="sub-nav-wrap">

            <div class="icc-home">
				
				<a href="/icc-cricket-world-cup-2015/content/series/509587.html">
ICC Cricket World Cup 2015
				</a>
	        </div>


            <ul class="subnav_tier1 subnav-item-wrap" id="subnav_tier1" data-role="none">

                <li ><a name="&lpos=quicklink_News" href="/icc-cricket-world-cup-2015/content/story/news.html?object=509587">News</a>

                </li>

                <li ><a name="&lpos=quicklink_Features" href="/icc-cricket-world-cup-2015/content/story/features.html?object=509587">Features</a>

                </li>

                <li ><a name="&lpos=quicklink_Photos" href="/icc-cricket-world-cup-2015/content/image/index.html?object=509587">Photos</a>

                </li>

                <li ><a name="&lpos=quicklink_Fixtures" href="/icc-cricket-world-cup-2015/content/series/509587.html?template=fixtures">Fixtures</a>

					<div class="dd_wrap">

						<ul class="subnav_grp">

							<li class="subnav_grpitm">
								<ul class="subnav_tire2">

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Groups" href="/ci/content/page/817521.html">Groups</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Fixtures" href="/icc-cricket-world-cup-2015/content/series/509587.html?template=fixtures">Fixtures</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Download the fixtures" href="/icc-cricket-world-cup-2015/content/series/509587.html?template=download_fixtures">Download the fixtures</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Warm-up matches" href="/icc-cricket-world-cup-2015/content/series/806105.html?template=fixtures">Warm-up matches</a>
									</li>

								</ul>
							</li>

						</ul>

					</div>

                </li>

                <li ><a name="&lpos=quicklink_Results" href="/icc-cricket-world-cup-2015/engine/series/509587.html">Results</a>

					<div class="dd_wrap">

						<ul class="subnav_grp">

							<li class="subnav_grpitm">
								<ul class="subnav_tire2">

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Tournament" href="/icc-cricket-world-cup-2015/engine/series/509587.html">Tournament</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Warm-up matches" href="/icc-cricket-world-cup-2015/engine/series/806105.html">Warm-up matches</a>
									</li>

								</ul>
							</li>

						</ul>

					</div>

                </li>

                <li ><a name="&lpos=quicklink_Table" href="/icc-cricket-world-cup-2015/engine/series/509587.html?view=pointstable">Table</a>

                </li>

                <li ><a name="&lpos=quicklink_Squads" href="/icc-cricket-world-cup-2015/content/squad?object=509587">Squads</a>

					<div class="dd_wrap">

						<ul class="subnav_grp">
							<li class="subnav_grpitm">
								<ul class="subnav_tire2">

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Afghanistan" href="/icc-cricket-world-cup-2015/content/squad/814789.html">Afghanistan</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Australia" href="/icc-cricket-world-cup-2015/content/squad/819741.html">Australia</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Bangladesh" href="/icc-cricket-world-cup-2015/content/squad/816431.html">Bangladesh</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_England" href="/icc-cricket-world-cup-2015/content/squad/812413.html">England</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_India" href="/icc-cricket-world-cup-2015/content/squad/817409.html">India</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Ireland" href="/icc-cricket-world-cup-2015/content/squad/816765.html">Ireland</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_New Zealand" href="/icc-cricket-world-cup-2015/content/squad/818117.html">New Zealand</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Pakistan" href="/icc-cricket-world-cup-2015/content/squad/817901.html">Pakistan</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Scotland" href="/icc-cricket-world-cup-2015/content/squad/818887.html">Scotland</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_South Africa" href="/icc-cricket-world-cup-2015/content/squad/817889.html">South Africa</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Sri Lanka" href="/icc-cricket-world-cup-2015/content/squad/817899.html">Sri Lanka</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_United Arab Emirates" href="/icc-cricket-world-cup-2015/content/squad/819825.html">United Arab Emirates</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_West Indies" href="/icc-cricket-world-cup-2015/content/squad/819743.html">West Indies</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Zimbabwe" href="/icc-cricket-world-cup-2015/content/squad/817903.html">Zimbabwe</a>
									</li>

								</ul>
							</li>
						</ul>

					</div>

                </li>

                <li ><a name="&lpos=quicklink_Grounds" href="/icc-cricket-world-cup-2015/content/series/509587.html?template=ground">Grounds</a>

					<div class="dd_wrap">

						<ul class="subnav_grp">

							<li class="subnav_grpitm">
								<ul class="subnav_tire2">

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Adelaide" href="/icc-cricket-world-cup-2015/content/ground/56293.html">Adelaide</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Auckland" href="/icc-cricket-world-cup-2015/content/ground/58792.html">Auckland</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Brisbane" href="/icc-cricket-world-cup-2015/content/ground/56336.html">Brisbane</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Christchurch" href="/icc-cricket-world-cup-2015/content/ground/58810.html">Christchurch</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Canberra" href="/icc-cricket-world-cup-2015/content/ground/56370.html">Canberra</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Dunedin" href="/icc-cricket-world-cup-2015/content/ground/58827.html">Dunedin</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Hamilton" href="/icc-cricket-world-cup-2015/content/ground/58831.html">Hamilton</a>
									</li>

								</ul>
							</li>

							<li class="subnav_grpitm">
								<ul class="subnav_tire2">

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Hobart" href="/icc-cricket-world-cup-2015/content/ground/56407.html">Hobart</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Melbourne" href="/icc-cricket-world-cup-2015/content/ground/56441.html">Melbourne</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Napier" href="/icc-cricket-world-cup-2015/content/ground/58857.html">Napier</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Nelson" href="/icc-cricket-world-cup-2015/content/ground/424917.html">Nelson</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Perth" href="/icc-cricket-world-cup-2015/content/ground/56490.html">Perth</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Sydney" href="/icc-cricket-world-cup-2015/content/ground/56544.html">Sydney</a>
									</li>

									<li class="sub_nav_item">
										<a name="&lpos=quicklink_Wellington" href="/icc-cricket-world-cup-2015/content/ground/58899.html">Wellington</a>
									</li>

								</ul>
							</li>

						</ul>

					</div>

                </li>

                <li ><a name="&lpos=quicklink_Stats" href="/icc-cricket-world-cup-2015/engine/series/509587.html?view=records">Stats</a>

                </li>

                <li ><a name="&lpos=quicklink_Records" href="/icc-cricket-world-cup-2015/engine/records/index.html?id=12;type=trophy">Records</a>

                </li>

                <li ><a name="&lpos=quicklink_Timeline" href="/wctimeline/content/site/wctimeline/index.html">Timeline</a>

                </li>

                <li ><a name="&lpos=quicklink_Vignettes" href="/wctimeline/content/site/wctimeline/genre.html?id=639">Vignettes</a>

                </li>

                <li ><a name="&lpos=quicklink_Men of the final" href="/ci/content/video_audio/index.html?genre=111">Men of the final</a>

                </li>

            </ul>

            </div>
          </div>
        </div>




    <div class="site-container ad-banner-1280">
        <div class="leaderboard-ad">
            <div class="espni-ad-slot ad-banner" data-slot-type="banner" data-kvpos="top"></div>
        </div>
    </div>

    <!-- Homepage wrapper Start -->
    <div class="hp-wrapper">

        <div class="site-container">
			<div class="pencil-ad">

	<div class="slot-1 hide-for-mob">
	
<div class="espni-ad-slot ad-panel" data-slot-type="longstrip" data-kvpos="left" data-exclude-bp="s"></div>

	</div>

	<div class="slot-2 hide-for-mob">
	<div class="espni-ad-slot" data-slot-type="incontentstrip" data-kvpos="top" data-exclude-bp="s"></div>

	</div>

			</div>
		</div>

		
<div class="hp-container">
    <div class="secondary">
       <section class="livescores module" data-seriesid="509587">	

	<div class="tabs-web-below" data-query="" data-isCounty="0" data-lscount="5">
	    <ul class="tabs" data-contentparent="#livescores-full">
	    	
	        <li class="selected"><a href="#results" class="ci-tabs" data-tabname="results">Results</a></li>
	        
	    </ul>
	</div>
	<div id="livescores-full" class="tabs-wrapper slider-wrapper">
	
		<div id="fixtures" class="tabs-content" style='display:none;'>
	
		<ul class="scoreline-list"><li>No Live Matches at the moment</li></ul>

		</div>
		<div id="results" class="tabs-content" style='display:block;'>

		<ul>
			<li class="others">
				<ul class="scoreline-list">
				
					<li>
					<p><span class="result-text"><a href="http://www.espncricinfo.com/series/509587/scorecard/656495/Australia-vs-New-Zealand-Final-ICC-Cricket-World-Cup">
					New Zealand v Australia 
					 at Melbourne
					 on Mar 29, 2015
					</a></span></p>
					<p>Australia won by 7 wickets (with 101 balls remaining)</p> 
					</li>
				
					<li>
					<p><span class="result-text"><a href="http://www.espncricinfo.com/series/509587/scorecard/656493/Australia-vs-India-2nd Semi-Final-ICC-Cricket-World-Cup">
					Australia v India 
					 at Sydney
					 on Mar 26, 2015
					</a></span></p>
					<p>Australia won by 95 runs</p> 
					</li>
				
					<li>
					<p><span class="result-text"><a href="http://www.espncricinfo.com/series/509587/scorecard/656491/New-Zealand-vs-South-Africa-1st Semi-Final-ICC-Cricket-World-Cup">
					South Africa v New Zealand 
					 at Auckland
					 on Mar 24, 2015
					</a></span></p>
					<p>New Zealand won by 4 wickets (with 1 ball remaining) (D/L method)</p> 
					</li>
				
					<li>
					<p><span class="result-text"><a href="http://www.espncricinfo.com/series/509587/scorecard/656489/New-Zealand-vs-West-Indies-4th Quarter-Final-ICC-Cricket-World-Cup">
					New Zealand v West Indies 
					 at Wellington
					 on Mar 21, 2015
					</a></span></p>
					<p>New Zealand won by 143 runs</p> 
					</li>
				
					<li>
					<p><span class="result-text"><a href="http://www.espncricinfo.com/series/509587/scorecard/656487/Australia-vs-Pakistan-3rd Quarter-Final-ICC-Cricket-World-Cup">
					Pakistan v Australia 
					 at Adelaide
					 on Mar 20, 2015
					</a></span></p>
					<p>Australia won by 6 wickets (with 97 balls remaining)</p> 
					</li>
				
				</ul>
			</li>
		</ul>
		
		<ul class="explore-links">
			<li><a href="/icc-cricket-world-cup-2015/engine/series/509587.html">Results page</a></li>		
		</ul>
		

		</div>
	</div>
	<div id="livescores-compact" class="tabs-wrapper slider-wrapper">
		<div id="fixtures-compact" class="tabs-content" style='display:none;'>

			<ul class="scoreline-list"><li>No Live Matches at the moment</li></ul>
			
			<ul class="explore-links results-link">
				
				<li><a href="/icc-cricket-world-cup-2015/content/series/509587.html?template=fixtures">All fixtures</a></li>
				
			</ul>
		</div>
		<div id="results-compact" class="tabs-content" style='display:block;'>
			
			<ul class="scoreline-list international">
			
				<li>
				<p><span class="result-text"><a href="http://www.espncricinfo.com/series/509587/scorecard/656495/Australia-vs-New-Zealand-Final-ICC-Cricket-World-Cup">
				New Zealand v Australia 
				 at Melbourne
				 on Mar 29, 2015
				</a></span></p>
				<p>Australia won by 7 wickets (with 101 balls remaining)</p>
				</li>
			
				<li>
				<p><span class="result-text"><a href="http://www.espncricinfo.com/series/509587/scorecard/656493/Australia-vs-India-2nd Semi-Final-ICC-Cricket-World-Cup">
				Australia v India 
				 at Sydney
				 on Mar 26, 2015
				</a></span></p>
				<p>Australia won by 95 runs</p>
				</li>
			
				<li>
				<p><span class="result-text"><a href="http://www.espncricinfo.com/series/509587/scorecard/656491/New-Zealand-vs-South-Africa-1st Semi-Final-ICC-Cricket-World-Cup">
				South Africa v New Zealand 
				 at Auckland
				 on Mar 24, 2015
				</a></span></p>
				<p>New Zealand won by 4 wickets (with 1 ball remaining) (D/L method)</p>
				</li>
			
				<li>
				<p><span class="result-text"><a href="http://www.espncricinfo.com/series/509587/scorecard/656489/New-Zealand-vs-West-Indies-4th Quarter-Final-ICC-Cricket-World-Cup">
				New Zealand v West Indies 
				 at Wellington
				 on Mar 21, 2015
				</a></span></p>
				<p>New Zealand won by 143 runs</p>
				</li>
			
				<li>
				<p><span class="result-text"><a href="http://www.espncricinfo.com/series/509587/scorecard/656487/Australia-vs-Pakistan-3rd Quarter-Final-ICC-Cricket-World-Cup">
				Pakistan v Australia 
				 at Adelaide
				 on Mar 20, 2015
				</a></span></p>
				<p>Australia won by 6 wickets (with 97 balls remaining)</p>
				</li>
			
			</ul>				
			
			<ul class="explore-links results-link">
				<li><a href="/icc-cricket-world-cup-2015/engine/series/509587.html">All Results</a></li>		
			</ul>
		</div>
	</div>
	
</section>
<!-- 1st MPU start: Excluding from small, medium profiles and excluding the story pages -->
<section class="ad-container">
<div class="espni-ad-slot ad-incontent" data-slot-type="incontent" data-kvpos="top" data-exclude-bp="s,m"></div>
</section>
<!-- 1st MPU end -->


<!-- Panel Ad start -->
<section class="ad-container ad-140">
	<div class="espni-ad-slot ad-panel" data-slot-type="panel" data-kvpos=""></div>
</section>
<!-- Panel Ad end -->
<!-- Cricket on Twitter start -->
<section class="reader_rec hide-for-mob hide-landscape hide-portrait" style="border-top: 8px solid #dddddd;margin-bottom: 10px;">
	<a class="twitter-timeline" href="/ESPNcricinfo/timelines/585813293161406464" data-widget-id="586402209283227648">Readers recommend</a> 
	<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>
</section>
<!-- Cricket on Twitter end -->
<!-- 2nd MPU start -->
<section class="ad-container">
    <div class="espni-ad-slot ad-incontent" data-slot-type="incontent" data-kvpos="bottom" data-exclude-bp="s,m"></div>
</section>
<!-- MPU end -->

		<section class="ad-container">
		<div class="espni-ad-slot ad-panel" data-slot-type="panel" data-kvpos="top" data-exclude-bp="s,m"></div>
		</section>


	<!-- Facebook start -->
	<section class="cric-facebook module">
		<div class="content-padding">
			<h5 class="section-name">Facebook</h5>
		</div>
		<iframe src="http://www.facebook.com/plugins/likebox.php?href=http%3A%2F%2Fwww.facebook.com%2Fcricinfo&amp;width=300&amp;height=245&amp;show_faces=true&amp;colorscheme=light&amp;stream=false&amp;border_color=0&amp;header=false&amp;show_border=false" scrolling="no" frameborder="0" style="border:none; overflow:hidden; width:300px; height:245px;  background:#FFFFFF;" allowTransparency="true"></iframe>
	</section>
	<!-- Facebook end -->


    </div>
    <div class="primary">
    	
<!-- Multimedia module start -->
<!-- use col-3 for option 1, option 2 and col-2 for option 3, option4 | connected class would connect col-2 with headline,
remove it when a gap needed-->
<section class="multimedia col-2 module">
	<div class="featured headline-before content">
        <div class="headers">
            <h6 class="sub-headline">
            <a href="/icc-cricket-world-cup-2015/content/series/509587.html" name="&lpos=headline_row0area0">
            ICC news
            </a>
            </h6>
            <h1 class="featured-link"><a href="/ci-icc/content/story/857811.html" name="&lpos=headline_row0area0">Mustafa Kamal quits as ICC president</a></h1>
        </div>
    </div>
 	<div class="option-1" style="">

 	</div>
 	<div class="option-2" style="">

 	</div>
 	<div class="option-3">

	</div>
	<div class="option-4">

<!-- Option 4 - Medium Image Start-->

    <div class="medium-image slider-wrapper" style="">
        <ul class="site-photo">
        
            <li data-pid="857245" data-pcaption="Golden glory: The Australia team celebrate their fifth World Cup win under a shower of confetti">
                <div class="img-wrap">
                    <picture>
                        
                        <img src="http://p.imgci.com/db/PICTURES/CMS/209800/209891.3.jpg" alt="" title="" class="img-full"  />
                        
                    </picture>
                    
                    <div class="img-caption ">
                        <p>
                        Golden glory: The Australia team celebrate their fifth World Cup win under a shower of confetti
                        <span class="copyright"> 
                         &copy; ICC</span></p>
                    </div>
                    
                </div>
            </li>
            
        </ul>
    </div>

<!-- Medium Image End-->

	</div>

</section>
<!-- Multimedia module end -->

        <div class="tabs-mobile">
            <ul class="tabs">
                <li class="selected"><a href="#" data-tname="news">News</a></li>
                
                <li><a href="#" data-tname="epicks">Editor's Picks</a></li>
                
            </ul>
        </div>
        <section class="intermediate">
        <!-- Editor Picks module start -->
<section class="editor-picks hide-portrait module">	<div class="content-padding">
		<h5 class="section-name"><a href="/ci/content/story/editors_pick.html">Editor's Picks</a></h5>
	</div>
	<!-- Editor Picks Items start-->
	<ul>
		<li>
			<div class="thumbnail">
				<div class="img-wrap">
					
					
						<picture>
							<a name="&lpos=editorspick_1" href="/magazine/content/story/857201.html"><img src="http://p.imgci.com/db/PICTURES/CMS/209800/209873.5.jpg" alt="" title="" class="img-full" /></a>
						</picture>
					
				</div>
			</div>
			<div class="content">
				
				<h2><a name="&lpos=editorspick_1" href="/magazine/content/story/857201.html">What did we learn from this World Cup?</a></h2>
			</div>
			<ul class="link-cluster">
			
				<li><span class=""></span>That there is a place for proper batsmanship in ODIs, that New Zealand punch above their weight, and that wickets win you matches. By <b>Ed Smith</b></li>
				
            </ul>
		</li>
	
		<li>
			<div class="thumbnail">
				<div class="img-wrap">
					
					
						<picture>
							<a name="&lpos=editorspick_2" href="/magazine/content/story/856297.html"><img src="http://p.imgci.com/db/PICTURES/CMS/209700/209725.6.jpg" alt="" title="" class="img-full" /></a>
						</picture>
					
				</div>
			</div>
			<div class="content">
				
				<h2><a name="&lpos=editorspick_2" href="/magazine/content/story/856297.html">The greatest time of our lives</a></h2>
			</div>
			<ul class="link-cluster">
			
				<li><span class=""></span><b>Martin Crowe:</b> Whatever happens, the Australia-New Zealand World Cup final at the MCG will be the most divine fun</li>
				
            </ul>
		</li>
	
		<li>
			<div class="thumbnail">
				<div class="img-wrap">
					
					
						<picture>
							<a name="&lpos=editorspick_3" href="/magazine/content/story/852755.html"><img src="http://p.imgci.com/db/PICTURES/CMS/207100/207187.4.jpg" alt="" title="" class="img-full" /></a>
						</picture>
					
				</div>
			</div>
			<div class="content">
				
				<h2><a name="&lpos=editorspick_3" href="/magazine/content/story/852755.html">Left-arm quicks rule</a></h2>
			</div>
			<ul class="link-cluster">
			
				<li><span class=""></span><b>Numbers Game:</B> Mitchell Starc and Trent Boult have led the way, but other left-arm fast bowlers have also shone in this World Cup</li>
				
            </ul>
		</li>
	
		<li>
			<div class="thumbnail">
				<div class="img-wrap">
					
					
						<picture>
							<a name="&lpos=editorspick_4" href="/magazine/content/story/852631.html"><img src="http://p.imgci.com/db/PICTURES/CMS/208900/208993.5.jpg" alt="" title="" class="img-full" /></a>
						</picture>
					
				</div>
			</div>
			<div class="content">
				
				<h2><a name="&lpos=editorspick_4" href="/magazine/content/story/852631.html">The serenity of Kane Williamson</a></h2>
			</div>
			<ul class="link-cluster">
			
				<li><span class=""></span><b>Martin Crowe:</b> He is assured, humble and intelligent: a player opposition captains and bowlers find hard to combat</li>
				
            </ul>
		</li>
	
		<li>
			<div class="thumbnail">
				<div class="img-wrap">
					
					
						<picture>
							<a name="&lpos=editorspick_5" href="/icc-cricket-world-cup-2015/content/story/851397.html"><img src="http://p.imgci.com/db/PICTURES/CMS/207100/207117.4.jpg" alt="" title="" class="img-full" /></a>
						</picture>
					
				</div>
			</div>
			<div class="content">
				
				<h2><a name="&lpos=editorspick_5" href="/icc-cricket-world-cup-2015/content/story/851397.html">Build, build, blast off</a></h2>
			</div>
			<ul class="link-cluster">
			
				<li><span class=""></span><b>Sharda Ugra:</B> This World Cup,most teams seem to have followed a strategy of constructing an innings till about the last quarter and then gone full pelt</li>
				
            </ul>
		</li>
		</ul>
	<!-- Editor Picks Items end-->
	

	
	<!-- Ad center column start -->
	<div class="ad-container ad-center-column ad-140">
		<div class="espni-ad-slot ad-panel" data-slot-type="panel" data-kvpos="top" data-exclude-bp="s,m"></div>
	</div>
	<!-- Ad center column end -->
	<!-- mpu Ad -->
	<div class="ad-container ad-center-column hide show-landscape">
		<div class="espni-ad-slot ad-incontent" data-slot-type="incontent" data-kvpos="top" data-exclude-bp="s,m,xl"></div>
	</div>
	<!-- mpu Ad End -->
</section>
<!-- Editor Picks module end -->
  <section class="ch-points-table module hide-portrait hide-for-mob">
      <div class="content-padding all-link">
          <h5 class="section-name">Points Table</h5>
          <a href="/icc-cricket-world-cup-2015/engine/series/509587.html?view=pointstable">All &rarr;</a>
      </div>
      <div class="group">
        <p class="heading">Pool A</p>
<table>
   	<tbody>
		<tr>
	      	<th class="bold">T<span>eams</span></th>
	      	<th class="bold">M<span>at</span></th>
	      	<th class="bold">W<span>on</span></th>
	      	<th class="bold">L<span>ost</span></th>
	      	<th class="bold">T<span>ied</span></th>
  			<th class="bold">N/R</th>
      		<th class="bold last">P<span>ts</span></th>

 		</tr>
		<tr>
			<td>
NZ
	 		</td>
			<td>6</td>
			<td>6</td>
			<td>0</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">12</td>
     	</tr>
		<tr>
			<td>
AUS
	 		</td>
			<td>6</td>
			<td>4</td>
			<td>1</td>
			<td>0</td>
      		<td>1</td>
     		<td class="last">9</td>
     	</tr>
		<tr>
			<td>
SL
	 		</td>
			<td>6</td>
			<td>4</td>
			<td>2</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">8</td>
     	</tr>
		<tr>
			<td>
BDESH
	 		</td>
			<td>6</td>
			<td>3</td>
			<td>2</td>
			<td>0</td>
      		<td>1</td>
     		<td class="last">7</td>
     	</tr>
		<tr>
			<td>
ENG
	 		</td>
			<td>6</td>
			<td>2</td>
			<td>4</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">4</td>
     	</tr>
		<tr>
			<td>
AFG
	 		</td>
			<td>6</td>
			<td>1</td>
			<td>5</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">2</td>
     	</tr>
		<tr>
			<td>
SCOT
	 		</td>
			<td>6</td>
			<td>0</td>
			<td>6</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">0</td>
     	</tr>
	</tbody>
</table>

  <p class="heading">Pool B</p>
<table>
   	<tbody>
		<tr>
	      	<th class="bold">T<span>eams</span></th>
	      	<th class="bold">M<span>at</span></th>
	      	<th class="bold">W<span>on</span></th>
	      	<th class="bold">L<span>ost</span></th>
	      	<th class="bold">T<span>ied</span></th>
  			<th class="bold">N/R</th>
      		<th class="bold last">P<span>ts</span></th>

 		</tr>
		<tr>
			<td>
INDIA
	 		</td>
			<td>6</td>
			<td>6</td>
			<td>0</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">12</td>
     	</tr>
		<tr>
			<td>
SA
	 		</td>
			<td>6</td>
			<td>4</td>
			<td>2</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">8</td>
     	</tr>
		<tr>
			<td>
PAK
	 		</td>
			<td>6</td>
			<td>4</td>
			<td>2</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">8</td>
     	</tr>
		<tr>
			<td>
WI
	 		</td>
			<td>6</td>
			<td>3</td>
			<td>3</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">6</td>
     	</tr>
		<tr>
			<td>
IRE
	 		</td>
			<td>6</td>
			<td>3</td>
			<td>3</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">6</td>
     	</tr>
		<tr>
			<td>
ZIM
	 		</td>
			<td>6</td>
			<td>1</td>
			<td>5</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">2</td>
     	</tr>
		<tr>
			<td>
UAE
	 		</td>
			<td>6</td>
			<td>0</td>
			<td>6</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">0</td>
     	</tr>
	</tbody>
</table>


      </div>
  </section>
  
    <section class="ch-statistics module hide-portrait hide-for-mob">
        <div class="content-padding">
            <h5 class="section-name">Statistics</h5>
        </div>
        <div class="content">
        
            <ul>
                <li>
                    <h2>ICC Cricket World Cup, 2014/15</h2>
                    <p> (in Australia/New Zealand ODIs)</p>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/batting/most_runs_career.html?id=6537;type=tournament">Most runs</a> | </span>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/bowling/most_wickets_career.html?id=6537;type=tournament">Most wickets</a> | </span>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/batting/most_runs_innings.html?id=6537;type=tournament">High scores</a> | </span>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/bowling/best_figures_innings.html?id=6537;type=tournament">Best bowling</a> | </span>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?id=6537;type=tournament">More</a></span>
                    
                </li>
            </ul>
            
            <ul>
                <li>
                    <h2>Chappell-Hadlee Trophy, 2014/15</h2>
                    <p> (Australia in New Zealand ODIs)</p>
                    <span><a href="/ci/engine/records/batting/most_runs_career.html?id=9897;type=series">Most runs</a> | </span>
                    <span><a href="/ci/engine/records/bowling/most_wickets_career.html?id=9897;type=series">Most wickets</a> | </span>
                    <span><a href="/ci/engine/records/batting/most_runs_innings.html?id=9897;type=series">High scores</a> | </span>
                    <span><a href="/ci/engine/records/bowling/best_figures_innings.html?id=9897;type=series">Best bowling</a> | </span>
                    <span><a href="/ci/engine/records/index.html?id=9897;type=series">More</a></span>
                    
                </li>
            </ul>
            
                    <h2>One-Day Internationals</h2>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2">Overall | </a></span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=40;type=team">Afghanistan</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2;type=team">Australia</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=25;type=team">Bangladesh</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=1;type=team">England</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=6;type=team">India</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=29;type=team">Ireland</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=5;type=team">New Zealand</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=7;type=team">Pakistan</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=30;type=team">Scotland</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=3;type=team">South Africa</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=8;type=team">Sri Lanka</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=27;type=team">United Arab Emirates</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=4;type=team">West Indies</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=9;type=team">Zimbabwe</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2;type=host">Australia</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=5;type=host">New Zealand</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2017;type=year">Year 2017</a></span>
                
        </div>
    </section>
    

<section class="ch-squad module hide-portrait hide-for-mob">
    <div class="content-padding">
        <h5 class="section-name">Squad</h5>
    </div>
    <div class="squad-dd">
      <dl class="dropdown">
        <dt><a><span>Bangladesh Squad</span></a></dt>
        <dd>
        <ul>
    
        <li><a data-value="10723" class="default">Bangladesh Squad</a></li>
      
        <li><a data-value="10737" class="">South Africa Squad</a></li>
      
        <li><a data-value="10749" class="">Zimbabwe Squad</a></li>
      
        <li><a data-value="10753" class="">Scotland Squad</a></li>
      
        <li><a data-value="10771" class="">Australia Squad</a></li>
      
        <li><a data-value="10775" class="">United Arab Emirates Squad</a></li>
      
        <li><a data-value="10729" class="">Ireland Squad</a></li>
      
        <li><a data-value="10733" class="">India Squad</a></li>
      
        <li><a data-value="10747" class="">Pakistan Squad</a></li>
      
        <li><a data-value="10681" class="">England Squad</a></li>
      
        <li><a data-value="10699" class="">Afghanistan Squad</a></li>
      
        <li><a data-value="10773" class="">West Indies Squad</a></li>
      
        <li><a data-value="10745" class="">Sri Lanka Squad</a></li>
      
        <li><a data-value="10751" class="">New Zealand Squad</a></li>
      
      </ul>
      </dd>
    </dl>
  </div>
  <div class="content">
    <ul class="list-block" id="squad_list_w">
  
      <li><a href="/icc-cricket-world-cup-2015/content/player/56007.html">Mashrafe Mortaza (c)</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/380354.html">Anamul Haque</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/56268.html">Arafat Sunny</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/280734.html">Imrul Kayes</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/56025.html">Mahmudullah</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/373696.html">Mominul Haque</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/56029.html">Mushfiqur Rahim (wk)</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/300618.html">Nasir Hossain</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/300619.html">Rubel Hossain</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/373538.html">Sabbir Rahman</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/56143.html">Shakib Al Hasan</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/436677.html">Soumya Sarkar</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/401057.html">Taijul Islam</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/56194.html">Tamim Iqbal</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/538506.html">Taskin Ahmed</a></li>
      
    </ul>
  </div>
</section>

        </section>
        
<!-- Headlines module start -->
<section class="headlines module">
    <ul class="blocks">
        
        <li>
            <div class="featured">                
                
                <div class="headers ">
                                
                    
                    <h6 class="sub-headline"><a name="&lpos=headline_row0area0" href="/icc-cricket-world-cup-2015/content/series/509587.html">ICC news</a></h6>
                    
                    
                        <h1 class="featured-link">
                        
                        <a name="&lpos=headline_row0area0" href="/ci-icc/content/story/857811.html">Mustafa Kamal quits as ICC president</a>
                        
                        </h1>
                        
                </div>
                <div class="thumbnail">
                    <div class="img-wrap">
                        
                        
                            <picture>
                            
                                <a name="&lpos=headline_row0area0" href="/ci-icc/content/story/857811.html"><img src="http://p.imgci.com/db/PICTURES/CMS/150800/150825.4.jpg" alt="" title="" class="img-full" /></a>
                            </picture>
                        
                    </div>                 
                </div>
                <div class="link-list-wrapper">
                
                    <ul class="link-list">
                        
                        <li><span class=""></span><a name="&lpos=headline_row0area0" href="/ci-icc/content/story/857811.html">Steps down two days after threatening to expose ICC actions</a></li>
                        
                        <li><span class=""></span><a name="&lpos=headline_row0area0" href="/icc-cricket-world-cup-2015/content/story/857327.html">Kamal rails at World Cup trophy snub</a></li>
                        
                        <li><span class=""></span><a name="&lpos=headline_row0area0" href="/ci-icc/content/story/857977.html">'Whole episode is unfortunate' - Nazmul Hassan</a></li>
                        
                        <li><span class=""></span><a name="&lpos=headline_row0area0" href="/magazine/content/story/858223.html">Kalra: Mustafa Kamal's own goal</a></li>
                        
                    </ul>
                    
                    <p class="related related-mobile">
                        <a href="#">
                            <span class="icon-font icon-boxed-plus-outline"></span>
                            <span class="more-news-count">4 Related</span>
                            <span class="more-news-close">Close</span>
                        </a>
                    </p>
                    
                
                </div>
            </div>
        </li>
        
        <li>
            <div class="others">                
                
                <div class="headers">
                                
                    
                    <h6 class="sub-headline"><a name="&lpos=headline_row1area1" href="/icc-cricket-world-cup-2015/content/series/509587.html">World Cup 2015 review</a></h6>
                    
                    
                        <h2 class="featured-link">
                        
                        <a name="&lpos=headline_row1area1" href="/magazine/content/story/857425.html">Advantage batsmen, game to bowlers</a>
                        
                        </h2>
                        
                </div>
                <div class="thumbnail">
                    <div class="img-wrap">
                        
                        
                            <picture>
                            
                                <a name="&lpos=headline_row1area1" href="/magazine/content/story/857425.html"><img src="http://p.imgci.com/db/PICTURES/CMS/209900/209935.4.jpg" alt="" title="" class="img-full" /></a>
                            </picture>
                        
                    </div>                 
                </div>
                <div class="link-list-wrapper">
                
                        <ul class="link-list">
                        
                            <li><span class=""></span><a name="&lpos=headline_row1area1" href="/magazine/content/story/857425.html">Bal: Bowlers had the last word despite string of high scores</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area1" href="/icc-cricket-world-cup-2015/content/story/857187.html">NZ 5, Australia 4 in our World Cup team</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area1" href="/icc-cricket-world-cup-2015/content/story/857203.html">Best bowling  - Wahab, Southee, Starc</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area1" href="/icc-cricket-world-cup-2015/content/story/857261.html">Best innings - Guptill, Smith, Shenwari</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area1" href="/icc-cricket-world-cup-2015/content/story/857339.html">Stats - Blazing Brendon, Sensational Starc</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area1" href="/icc-cricket-world-cup-2015/content/story/857341.html">Controversies - Mooney's catch, Rohit's no-ball</a></li>
                            
                        </ul>
                        

                        <ul class="related-list">
                        
                            <li><span class=""></span><a name="&lpos=headline_row1area1" href="/icc-cricket-world-cup-2015/content/story/857249.html">Memories - The nice guys from New Zealand</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area1" href="/icc-cricket-world-cup-2015/content/story/856295.html">Best quotes - Dhoni's citizenship quip</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area1" href="/icc-cricket-world-cup-2015/content/gallery/856445.html">World Cup in pictures</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area1" href="/icc-cricket-world-cup-2015/content/story/857333.html">World Cup in tweets - Less boring than 2007</a></li>
                            
                        </ul>
                        

                        <!-- Related items for web Start -->
                        <p class="related">
                            <a href="#">
                                <span class="icon-font icon-boxed-plus-outline"></span>
                                <span class="more-news-count">4 Related</span>
                                <span class="more-news-close">Close</span>
                            </a>
                        </p>
                        <!-- Related items for web End -->
                        
                        
                        <!-- Related items for mobile Start -->
                        <p class="related related-mobile">
                            <a href="#">
                                <span class="icon-font icon-boxed-plus-outline"></span>
                                <span class="more-news-count">10 Related</span>
                                <span class="more-news-close">Close</span>
                            </a>
                        </p>
                        <!-- Related items for mobile End -->

                        
                </div>
            </div>
        </li>
        
        <li>
            <div class="others">                
                
                <div class="headers">
                                
                    
                    <h6 class="sub-headline"><a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/engine/match/656495.html">Aus v NZ, Final, Melbourne</a></h6>
                    
                    
                        <h2 class="featured-link">
                        
                        <a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/content/story/857209.html">Australia in a World Cup final, business as usual</a>
                        
                        </h2>
                        
                </div>
                <div class="thumbnail">
                    <div class="img-wrap">
                        
                        
                            <picture>
                            
                                <a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/content/story/857209.html"><img src="http://p.imgci.com/db/PICTURES/CMS/209700/209773.4.jpg" alt="" title="" class="img-full" /></a>
                            </picture>
                        
                    </div>                 
                </div>
                <div class="link-list-wrapper">
                
                        <ul class="link-list">
                        
                            <li><span class=""></span><a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/content/story/857209.html">Ugra: Their A-game gene overrides all obstacles</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/engine/match/656495.html">SCORECARD</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/content/story/856581.html">Report - Majestic Australia claim magical fifer</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/content/story/857229.html">Zaltzman: NZ drown in duck nightmare</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/content/story/856849.html">Stats - Oldest Australian in a final</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area2" href="/thestands/content/story/856789.html">#report - Ruining finals since 1999</a></li>
                            
                        </ul>
                        

                        <ul class="related-list">
                        
                            <li><span class=""></span><a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/content/story/857045.html">Reactions - 'I'll try to have a beer with everyone in the place'</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/content/story/856811.html">Plays - McCullum's dream turns to nightmare</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/content/story/856599.html">Moments - Johnson owns Spartacus</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row1area2" href="/icc-cricket-world-cup-2015/content/story/856837.html">Photo report</a></li>
                            
                            <li><span class="icon-font icon-play-solid"></span><a name="&lpos=headline_row1area2" href="/ci/content/video_audio/856645.html">Holding: How could anyone think this is the best World Cup ever?</a></li>
                            
                            <li><span class="icon-font icon-play-solid"></span><a name="&lpos=headline_row1area2" href="/ci/content/video_audio/856623.html">On the road - Rugby nation's cricket moment</a></li>
                            
                            <li><span class="icon-font icon-play-solid"></span><a name="&lpos=headline_row1area2" href="/ci/content/video_audio/856649.html">Match Point - Behind the scenes</a></li>
                            
                        </ul>
                        

                        <!-- Related items for web Start -->
                        <p class="related">
                            <a href="#">
                                <span class="icon-font icon-boxed-plus-outline"></span>
                                <span class="more-news-count">7 Related</span>
                                <span class="more-news-close">Close</span>
                            </a>
                        </p>
                        <!-- Related items for web End -->
                        
                        
                        <!-- Related items for mobile Start -->
                        <p class="related related-mobile">
                            <a href="#">
                                <span class="icon-font icon-boxed-plus-outline"></span>
                                <span class="more-news-count">13 Related</span>
                                <span class="more-news-close">Close</span>
                            </a>
                        </p>
                        <!-- Related items for mobile End -->

                        
                </div>
            </div>
        </li>
        
        <li class="hr"><hr></li>
        <li class="hide-all"></li>
        
        <li>
            <div class="others">                
                
                <div class="headers">
                                
                    
                    <h6 class="sub-headline"><a name="&lpos=headline_row2area1" href="/icc-cricket-world-cup-2015/content/series/509587.html">Aus v NZ, final, Melbourne</a></h6>
                    
                    
                        <h2 class="featured-link">
                        
                        <a name="&lpos=headline_row2area1" href="/icc-cricket-world-cup-2015/content/story/856971.html">Young Australia suggest years of dominance</a>
                        
                        </h2>
                        
                </div>
                <div class="thumbnail">
                    <div class="img-wrap">
                        
                        
                            <picture>
                            
                                <a name="&lpos=headline_row2area1" href="/icc-cricket-world-cup-2015/content/story/856971.html"><img src="http://p.imgci.com/db/PICTURES/CMS/209800/209813.4.jpg" alt="" title="" class="img-full" /></a>
                            </picture>
                        
                    </div>                 
                </div>
                <div class="link-list-wrapper">
                
                        <ul class="link-list">
                        
                            <li><span class=""></span><a name="&lpos=headline_row2area1" href="/icc-cricket-world-cup-2015/content/story/856971.html">Brettig: Top performers will be around for 2019</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row2area1" href="/icc-cricket-world-cup-2015/content/story/857181.html">Plans fall in place for Australia's A-Team</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row2area1" href="/icc-cricket-world-cup-2015/content/story/857177.html">Clarke speaks of emotional toll</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row2area1" href="/icc-cricket-world-cup-2015/content/story/857167.html">Gallery - Glee and glory on a golden evening</a></li>
                            
                            <li><span class="icon-font icon-play-solid"></span><a name="&lpos=headline_row2area1" href="/ci/content/video_audio/856401.html">Ponting: Smith makes batting look easier</a></li>
                            
                        </ul>
                        
                        
                        <!-- Related items for mobile Start -->
                        <p class="related related-mobile">
                            <a href="#">
                                <span class="icon-font icon-boxed-plus-outline"></span>
                                <span class="more-news-count">5 Related</span>
                                <span class="more-news-close">Close</span>
                            </a>
                        </p>
                        <!-- Related items for mobile End -->

                        
                </div>
            </div>
        </li>
        
        <li>
            <div class="others">                
                
                <div class="headers">
                                
                    
                    <h6 class="sub-headline"><a name="&lpos=headline_row2area2" href="/icc-cricket-world-cup-2015/content/series/509587.html">Aus v NZ, final, Melbourne</a></h6>
                    
                    
                        <h2 class="featured-link">
                        
                        <a name="&lpos=headline_row2area2" href="/icc-cricket-world-cup-2015/content/story/856963.html">New Zealand lost final by abandoning aggression</a>
                        
                        </h2>
                        
                </div>
                <div class="thumbnail">
                    <div class="img-wrap">
                        
                        
                            <picture>
                            
                                <a name="&lpos=headline_row2area2" href="/icc-cricket-world-cup-2015/content/story/856963.html"><img src="http://p.imgci.com/db/PICTURES/CMS/209700/209767.4.jpg" alt="" title="" class="img-full" /></a>
                            </picture>
                        
                    </div>                 
                </div>
                <div class="link-list-wrapper">
                
                        <ul class="link-list">
                        
                            <li><span class=""></span><a name="&lpos=headline_row2area2" href="/icc-cricket-world-cup-2015/content/story/856963.html">Coverdale: Team found wanting for lack of plan B</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row2area2" href="/magazine/content/story/857205.html">Kimber: NZ's greatest almost</a></li>
                            
                            <li><span class=""></span><a name="&lpos=headline_row2area2" href="/icc-cricket-world-cup-2015/content/story/857165.html">Gracious McCullum hopes run will leave legacy</a></li>
                            
                            <li><span class="icon-font icon-play-solid"></span><a name="&lpos=headline_row2area2" href="/ci/content/video_audio/856375.html"><b>Two Men Out</b> - NZ's match-winners</a></li>
                            
                        </ul>
                        
                        
                        <!-- Related items for mobile Start -->
                        <p class="related related-mobile">
                            <a href="#">
                                <span class="icon-font icon-boxed-plus-outline"></span>
                                <span class="more-news-count">4 Related</span>
                                <span class="more-news-close">Close</span>
                            </a>
                        </p>
                        <!-- Related items for mobile End -->

                        
                </div>
            </div>
        </li>
        
        <li class="hr"><hr></li>
        <li class="hide-all"></li>
        
    </ul>
</section>
<!-- Headlines module end -->


<!-- More news module start -->
<section class="more-news module">
    <div class="content-padding all-link">
    
        <h5 class="section-name"><a href="/icc-cricket-world-cup-2015/content/story/news.html?object=509587">More News</a></h5>
        <a name="&lpos=morenews_all" href="/icc-cricket-world-cup-2015/content/story/news.html?object=509587">All &rarr;</a>
    </div>
    <div id="espni-moreNews">
    <ul class="blocks">
        
        <li>
            <div class="wrapper">                
                <div class="headers">
                    
                    <h6 class="sub-headline"><a name="&lpos=morenews_news" href="/ci/content/site/297120.html">ICC news</a></h6>
                    
                    <h2 class="featured-link"><a name="&lpos=morenews_news" href="/ci-icc/content/story/857265.html">ICC chiefs voice support for Associates</a></h2>
                </div>
                
            </div>
        </li>
        
        <li>
            <div class="wrapper">                
                <div class="headers">
                    
                    <h6 class="sub-headline"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/series/509587.html">Aus v NZ, Final, Melbourne</a></h6>
                    
                    <h2 class="featured-link"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/story/856521.html">Australia look to heal an old wound</a></h2>
                </div>
                
                <div class="related">
                    <a href="#">
                        <span class="icon-font icon-boxed-plus-outline"></span><span class="more-news-count">5</span><span class="more-news-close icon-font icon-boxed-close-outline"></span>
                    </a>
                </div>
                <div class="link-list-wrapper">
                    <ul class="link-list">
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856521.html">Brettig: Team still bears scars of 1992</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856301.html">Australia's journey to final began in 2013</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856169.html">NZ loss 'kick up the backside' - Clarke</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856347.html">Time to have the neighbours over</a></li>
                    
                        <li><span class="icon-font icon-play-solid"></span><a href="/ci/content/video_audio/856355.html">Will MCG be a full house for final?</a></li>
                    
                    </ul>
                </div>
                
            </div>
        </li>
        
        <li>
            <div class="wrapper">                
                <div class="headers">
                    
                    <h6 class="sub-headline"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/series/509587.html">Aus v India, 2nd semi-final, SCG</a></h6>
                    
                    <h2 class="featured-link"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/story/856171.html">Johnson drowns out tumult to down India</a></h2>
                </div>
                
                <div class="related">
                    <a href="#">
                        <span class="icon-font icon-boxed-plus-outline"></span><span class="more-news-count">3</span><span class="more-news-close icon-font icon-boxed-close-outline"></span>
                    </a>
                </div>
                <div class="link-list-wrapper">
                    <ul class="link-list">
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856171.html">Brettig: Overcomes a hostile crowd to strike telling blows</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856209.html">Zaltzman: Smith, Starc the talk of SCG</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/855999.html">Australia arrives at its own party</a></li>
                    
                    </ul>
                </div>
                
            </div>
        </li>
        
        <li>
            <div class="wrapper">                
                <div class="headers">
                    
                    <h6 class="sub-headline"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/series/509587.html">Aus v Ind, 2nd semi-final, Sydney</a></h6>
                    
                    <h2 class="featured-link"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/story/856195.html">India swept away by Australia's depth</a></h2>
                </div>
                
                <div class="related">
                    <a href="#">
                        <span class="icon-font icon-boxed-plus-outline"></span><span class="more-news-count">4</span><span class="more-news-close icon-font icon-boxed-close-outline"></span>
                    </a>
                </div>
                <div class="link-list-wrapper">
                    <ul class="link-list">
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856195.html">Ugra: Team's set patterns went awry</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856045.html">Pressure makes you do things you don't want to - Dhoni</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856099.html">Kimber: No helicopter ride to glory</a></li>
                    
                        <li><span class="icon-font icon-play-solid"></span><a href="/ci/content/video_audio/856145.html">Let's Talk About: A message for India</a></li>
                    
                    </ul>
                </div>
                
            </div>
        </li>
        
        <li>
            <div class="wrapper">                
                <div class="headers">
                    
                    <h6 class="sub-headline"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/engine/match/656493.html">Aus v Ind, 2nd semi-final, Sydney</a></h6>
                    
                    <h2 class="featured-link"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/story/855539.html">Australia win big as India run out of gas</a></h2>
                </div>
                
                <div class="related">
                    <a href="#">
                        <span class="icon-font icon-boxed-plus-outline"></span><span class="more-news-count">8</span><span class="more-news-close icon-font icon-boxed-close-outline"></span>
                    </a>
                </div>
                <div class="link-list-wrapper">
                    <ul class="link-list">
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/engine/match/656493.html">SCORECARD</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/855539.html">Report - Smith sets up trans-Tasman final</a></li>
                    
                        <li><span class="icon-font icon-play-solid"></span><a href="/ci/content/video_audio/856193.html">#PoliteEnquiries: Worst effort at a chase?</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856017.html">Plays - Starc gets a talking-to</a></li>
                    
                        <li><span class=""></span><a href="/thestands/content/story/856251.html">Fan report - Groovy tunes and a sunset at the SCG</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/855921.html">Stats - Highest semi-final score</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/855809.html">Photo report</a></li>
                    
                        <li><span class="icon-font icon-play-solid"></span><a href="/ci/content/video_audio/855907.html">Up for Challenge - Fishing for Oysters</a></li>
                    
                    </ul>
                </div>
                
            </div>
        </li>
        
        <li>
            <div class="wrapper">                
                <div class="headers">
                    
                    <h6 class="sub-headline"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/engine/match/656493.html">Aus v Ind, 2nd semi-final, Sydney</a></h6>
                    
                    <h2 class="featured-link"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/story/856033.html">Dhoni won't rule out 2019, will decide next year</a></h2>
                </div>
                
                <div class="related">
                    <a href="#">
                        <span class="icon-font icon-boxed-plus-outline"></span><span class="more-news-count">2</span><span class="more-news-close icon-font icon-boxed-close-outline"></span>
                    </a>
                </div>
                <div class="link-list-wrapper">
                    <ul class="link-list">
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/856033.html">'Still running, still fit', may take call after 2016 World T20</a></li>
                    
                        <li><span class="icon-font icon-play-solid"></span><a href="/ci/content/video_audio/856377.html">Can you enact Dhoni's helicopter shot?</a></li>
                    
                    </ul>
                </div>
                
            </div>
        </li>
        
        <li>
            <div class="wrapper">                
                <div class="headers">
                    
                    <h6 class="sub-headline"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/series/509587.html">Aus v Ind, 2nd semi-final, Sydney</a></h6>
                    
                    <h2 class="featured-link"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/story/855495.html">Dhoni masters numbers game to crack ODI code</a></h2>
                </div>
                
                <div class="related">
                    <a href="#">
                        <span class="icon-font icon-boxed-plus-outline"></span><span class="more-news-count">3</span><span class="more-news-close icon-font icon-boxed-close-outline"></span>
                    </a>
                </div>
                <div class="link-list-wrapper">
                    <ul class="link-list">
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/855495.html">Ugra: Captain moving from flashy finisher to measured risk manager</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/855405.html">India not carrying any scars, says Rohit</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/855441.html">'Always wanted to be best player in the world' - Kohli</a></li>
                    
                    </ul>
                </div>
                
            </div>
        </li>
        
        <li>
            <div class="wrapper">                
                <div class="headers">
                    
                    <h6 class="sub-headline"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/series/509587.html">Aus v Ind, 2nd semi-final, Sydney</a></h6>
                    
                    <h2 class="featured-link"><a name="&lpos=morenews_news" href="/icc-cricket-world-cup-2015/content/story/855383.html">His team on a roll, Clarke defends captaincy</a></h2>
                </div>
                
                <div class="related">
                    <a href="#">
                        <span class="icon-font icon-boxed-plus-outline"></span><span class="more-news-count">8</span><span class="more-news-close icon-font icon-boxed-close-outline"></span>
                    </a>
                </div>
                <div class="link-list-wrapper">
                    <ul class="link-list">
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/855383.html">'I think my record stacks up against just about anyone'</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/855437.html">How Faulkner, Maxwell turned into each other</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/854925.html">Smith strays down leg to reap the runs</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/854697.html">Finch calls in familiar help</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/854317.html">Sledging inevitable in semi-final - Faulkner</a></li>
                    
                        <li><span class=""></span><a href="/magazine/content/story/854289.html">Former PM Hawke livens up Australia's training</a></li>
                    
                        <li><span class=""></span><a href="/icc-cricket-world-cup-2015/content/story/854433.html">The coach with a hand in both camps</a></li>
                    
                        <li><span class="icon-font icon-play-solid"></span><a href="/ci/content/video_audio/854431.html">Sadist Hour: The continuum of risk</a></li>
                    
                    </ul>
                </div>
                
            </div>
        </li>
        
    </ul>
    </div>   

    
</section>
<!-- More news module end -->
<div class="ad-slot show-mobile"><div class="espni-ad-slot ad-incontent" data-slot-type="incontent" data-kvpos="top" data-exclude-bp="m,l,xl"></div></div>
	<section class="quotes module content-padding">
	    <h5 class="section-name"><a href="/icc-cricket-world-cup-2015/content/quote/index.html?object=509587">Quote Unquote</a></h5>
	    <blockquote><span class="icon icon-quotes"></span><a href="/icc-cricket-world-cup-2015/content/quote/index.html?object=509587">I would have called you a bunch of losers if you lost against Sri Lanka. You are championship material.</a></blockquote>    
	</section>
	        
        <section class="two-col-wrapper">
            
<section class="eq-2-col">
	
<!-- Cordon module start -->
<section class="cordon module blogs">
    <div class="content-padding all-link">
    
        <h5 class="section-name"><a href="/blogs/content/story/blogs/cordon.html">BLOGS</a></h5>
    
        <a name="&lpos=cordon_all" href="/blogs/content/story/blogs/cordon.html">All &rarr;</a>
    </div>
    <ul class="list-block">
    
        <li>
            <div class="thumbnail">
                <div class="img-wrap">
                    
                    <picture>
                    
                        <a name="&lpos=cordon_1"  href="/blogs/content/story/857349.html"><img src="http://p.imgci.com/db/PICTURES/CMS/209700/209793.4.jpg" alt="" title="" class="img-full" /></a>                    
                    
                    </picture>
                    
                </div>             
            </div>
            <div class="content">
                
                <h4><a name="&lpos=cordon_1" href="/blogs/content/story/857349.html">Fifty-over cricket, I was wrong</a></h4>
            </div>
            <p class="text"><b>Jon Hotten:</b> This World Cup gave us driven, vibrant, electric ODI cricket, played at the limit of current ability, and it was magnificent</p>
        </li>
    
        <li>
            <div class="thumbnail">
                <div class="img-wrap">
                    
                    <picture>
                    
                        <a name="&lpos=cordon_2"  href="/blogs/content/story/855685.html"><img src="http://p.imgci.com/db/PICTURES/CMS/129500/129519.4.jpg" alt="" title="" class="img-full" /></a>                    
                    
                    </picture>
                    
                </div>             
            </div>
            <div class="content">
                
                <h4><a name="&lpos=cordon_2" href="/blogs/content/story/855685.html">Why ball-tracking can't be trusted</a></h4>
            </div>
            <p class="text"><b>Russell Jackson:</b> It's hard for us to shake doubt about why what we're seeing with our eyes differs significantly from the reading of a computer</p>
        </li>
    
    </ul>

    

</section>
<!-- Cordon module end -->

<!-- Poll module start -->
<section class="poll module">
    <div class="content-padding">
        <h5 class="section-name">Poll</h5>
    </div>
    <div class="content" id="poll_content">
        <form action="" method="get" id="home_poll">
            <h6>Which was the most boring World Cup of all?</h6>
            
            <ul>
            
            <li><input id="poll_3959" type="radio" value="3959" name="poll"><label for="poll_3959">2015</label></li>
            
            <li><input id="poll_3961" type="radio" value="3961" name="poll"><label for="poll_3961">2011</label></li>
            
            <li><input id="poll_3963" type="radio" value="3963" name="poll"><label for="poll_3963">2007</label></li>
            
            <li><input id="poll_3965" type="radio" value="3965" name="poll"><label for="poll_3965">2003</label></li>
            
            <li><input id="poll_3967" type="radio" value="3967" name="poll"><label for="poll_3967">1999</label></li>
            
            <li><input id="poll_3969" type="radio" value="3969" name="poll"><label for="poll_3969">1996</label></li>
            
            <li><input id="poll_3971" type="radio" value="3971" name="poll"><label for="poll_3971">1992</label></li>
            
            <li><input id="poll_3973" type="radio" value="3973" name="poll"><label for="poll_3973">1987</label></li>
            
            <li><input id="poll_3975" type="radio" value="3975" name="poll"><label for="poll_3975">1983</label></li>
            
            <li><input id="poll_3977" type="radio" value="3977" name="poll"><label for="poll_3977">1979</label></li>
            
            <li><input id="poll_3979" type="radio" value="3979" name="poll"><label for="poll_3979">1975</label></li>
              
            </ul>
            
            <p><input type="submit" id="button" value="Submit"></p>
            <input type="hidden" id="hdnPollId" name="hdnPollId" value="1067">
            <input type="hidden" id="hdnSiteId" name="hdnSiteId" value="1530">
            <input type="hidden" id="hdnPollTitle" name="hdnPollTitle" value="Which was the most boring World Cup of all?">
        </form>
    </div>

</section>
<!-- Poll module end -->
<script type="text/javascript">
    var _ENDPOINT = "http://submit.espncricinfo.com/";
    var _BRANDING = "icc-cricket-world-cup-2015";
</script>
	
</section>
<section class="eq-2-col">
	
		<div class="photo-gallery-wrapper">
	        <section class="photo-gallery module">

	            <div class="pg-tabs content-padding">
	                <ul class="section-name tabs " data-contentparent=".photo-gallery">
	                    <li class="selected"><a href="#photo-wrapper" class="ci-tabs">Photos</a></li>
	                    
	                    <li><a href="#gallery-wrapper" class="ci-tabs">Gallery</a></li>
	                    
	                </ul>
	                <a href="/ci/content/image/index.html?object=509587" class="pg-all-link">All</a>
	            </div>
	            
<div class="content-padding all-link pg-section-title">
    <h5 class="section-name">Photos</h5>
    <a name="&lpos=photos_all" href="/ci/content/image/index.html?object=509587">All &rarr;</a>
</div>
<div id="photo-wrapper" class="photo slider-wrapper tabs-content ">
    <ul class="list-block slider-chphoto">
    
        <li>
            <div class="thumbnail">
                <a name="&lpos=photos_1" href="/ci/content/image/1099932.html?object=509587;dir=next"><picture>
                    
                    <img src="http://p.imgci.com/db/PICTURES/CMS/263700/263707.png" alt="" title="" class="img-full" />
                    
                </picture></a>
            </div>
            <div class="content">
                <p>
                    <span class="bold">May 29, 2017 </span>England's ODI batting has undergone a massive transformation
                    <span class="copyright"> &copy; ESPNcricinfo Ltd</span>
                </p>
            </div>
        </li>
    
        <li>
            <div class="thumbnail">
                <a name="&lpos=photos_2" href="/newzealand/content/image/970143.html?object=509587;dir=next"><picture>
                    
                    <img src="http://p.imgci.com/db/PICTURES/CMS/233600/233601.3.jpg" alt="" title="" class="img-full" />
                    
                </picture></a>
            </div>
            <div class="content">
                <p>
                    <span class="bold">Mar 31, 2015 </span>Brendon McCullum obliges fans
                    <span class="copyright"> &copy; Getty Images</span>
                </p>
            </div>
        </li>
    
        <li>
            <div class="thumbnail">
                <a name="&lpos=photos_3" href="/ci/content/image/932093.html?object=509587;dir=next"><picture>
                    
                    <img src="http://p.imgci.com/db/PICTURES/CMS/224900/224977.4.jpg" alt="" title="" class="img-full" />
                    
                </picture></a>
            </div>
            <div class="content">
                <p>
                    <span class="bold">Mar 31, 2015 </span>Brendon McCullum looks on
                    <span class="copyright"> &copy; Getty Images</span>
                </p>
            </div>
        </li>
    
        <li>
            <div class="thumbnail">
                <a name="&lpos=photos_4" href="/ci/content/image/895759.html?object=509587;dir=next"><picture>
                    
                    <img src="http://p.imgci.com/db/PICTURES/CMS/217100/217109.4.jpg" alt="" title="" class="img-full" />
                    
                </picture></a>
            </div>
            <div class="content">
                <p>
                    <span class="bold">Mar 31, 2015 </span>Fans welcome the New Zealand team home
                    <span class="copyright"> &copy; Getty Images</span>
                </p>
            </div>
        </li>
    
    </ul>
</div>

<div class="content-padding all-link pg-section-title">
    <h5 class="section-name">Galleries</h5>
    <a name="&lpos=gallery_all" href="/ci/content/gallery/index.html?object=509587">All &rarr;</a>
</div>
<div id="gallery-wrapper" class="gallery tabs-content">
    <ul class="list-block slider-chgallery">
    
        <li>
            <div class="thumbnail">
                <a name="&lpos=gallery_1" href="/icc-cricket-world-cup-2015/content/gallery/856445.html"><picture>
                    
                    <img src="http://p.imgci.com/db/PICTURES/CMS/205500/205511.3.jpg" alt="" title="" class="img-full" />
                    
                </picture></a>
            </div>
            <div class="content">
                <p>
                    <span class="bold">Mar 28, 2015 </span>World Cup 2015 in photos
                    <span class="copyright"> &copy; Getty Images</span>
                </p>
            </div>
        </li>
    
        <li>
            <div class="thumbnail">
                <a name="&lpos=gallery_2" href="/ci/content/gallery/855221.html"><picture>
                    
                    <img src="http://p.imgci.com/db/PICTURES/CMS/209400/209433.3.jpg" alt="" title="" class="img-full" />
                    
                </picture></a>
            </div>
            <div class="content">
                <p>
                    <span class="bold">Mar 24, 2015 </span>NZ v SA, 1st semi-final, Auckland
                    <span class="copyright"> &copy; Getty Images</span>
                </p>
            </div>
        </li>
    
        <li>
            <div class="thumbnail">
                <a name="&lpos=gallery_3" href="/icc-cricket-world-cup-2015/content/gallery/839573.html"><picture>
                    
                    <img src="http://p.imgci.com/db/PICTURES/CMS/207000/207015.3.jpg" alt="" title="" class="img-full" />
                    
                </picture></a>
            </div>
            <div class="content">
                <p>
                    <span class="bold">Feb 26, 2015 </span>Afghanistan v Scotland, World Cup 2015, Group A, Dunedin
                    <span class="copyright"> &copy; ICC</span>
                </p>
            </div>
        </li>
    
        <li>
            <div class="thumbnail">
                <a name="&lpos=gallery_4" href="/ci/content/gallery/836947.html"><picture>
                    
                    <img src="http://p.imgci.com/db/PICTURES/CMS/207600/207615.3.jpg" alt="" title="" class="img-full" />
                    
                </picture></a>
            </div>
            <div class="content">
                <p>
                    <span class="bold">Feb 22, 2015 </span>World Cup 2015
                    <span class="copyright"> &copy; Jarrod Kimber/ ESPNcricinfo</span>
                </p>
            </div>
        </li>
    
    </ul>

</div>

	        </section>
	    </div>
		
<!-- Top videos module start -->
<section class="top-videos module">
    <div class="content-padding all-link">
        <h5 class="section-name">Videos</h5>
        <a href="/ci/content/video_audio/index.html?object=509587">All &rarr;</a>
    </div>
    <ul class="list-block">
    
        <li>
            
            <div class="featured">
            
                <div class="thumbnail">
                    <div class="img-wrap">
                       

<div class="espni-vid-wrapper img-wrap">
	<div class="espni-vid-placeholder" data-width="100%" data-height="168" data-id="553fcdc2e4b053acb697a95c" data-genre="index:interviews" data-sharebuttons="true" data-autostart="false" data-expirationDate="" data-embargoDate="" data-mezzaninepathimage="http://a.espncdn.com/combiner/i?img=/media/motion/2015/0428/dm_150428_I-smith-watto-full/dm_150428_I-smith-watto-full.jpg?w=300&h=168" data-unicornurl="" data-brightcoveurl="" data-assetName="" data-duration="" data-headlineShort="" ></div>	
	<picture>
		<img class="espni-vid-thumb full" src="http://a.espncdn.com/combiner/i?img=/media/motion/2015/0428/dm_150428_I-smith-watto-full/dm_150428_I-smith-watto-full.jpg?w=300&h=168" alt="">
	</picture>
	<span class="video-play-button espni-vid-thumb ">Play</span>
	
	<span class="video-length espni-vid-thumb">20:18</span>
	
</div>


                    </div>
                </div>
                <div class="content">
                    
                    <h1><a href="/ci/content/video_audio/867843.html">'No doubt in my mind that Steve will be the next captain'</a></h1>
                    
                </div>
            
            </div>
            
        </li>
    
        <li>
            
                <div class="thumbnail">
                    <div class="img-wrap">
                       

<div class="espni-vid-wrapper img-wrap">
	<div class="espni-vid-placeholder" data-width="100%" data-height="168" data-id="553fcdc2e4b053acb697a95c" data-genre="index:interviews" data-sharebuttons="true" data-autostart="false" data-expirationDate="" data-embargoDate="" data-mezzaninepathimage="http://a.espncdn.com/combiner/i?img=/media/motion/2015/0428/dm_150428_I-smith-watto-full/dm_150428_I-smith-watto-full.jpg?w=300&h=168" data-unicornurl="" data-brightcoveurl="" data-assetName="" data-duration="" data-headlineShort="" ></div>	
	<picture>
		<img class="espni-vid-thumb full" src="http://a.espncdn.com/combiner/i?img=/media/motion/2015/0428/dm_150428_I-smith-watto-full/dm_150428_I-smith-watto-full.jpg?w=300&h=168" alt="">
	</picture>
	<span class="video-play-button espni-vid-thumb ">Play</span>
	
	<span class="video-length espni-vid-thumb">20:18</span>
	
</div>


                    </div>
                </div>
                <div class="content">
                    
                    <h4><span class="icon-font icon-play-solid"></span><a href="/ci/content/video_audio/857647.html">Sledging mars World Cup win as Ashes squad revealed</a></h4>
                    
                </div>
            
        </li>
    
        <li>
            
                <div class="thumbnail">
                    <div class="img-wrap">
                       

<div class="espni-vid-wrapper img-wrap">
	<div class="espni-vid-placeholder" data-width="100%" data-height="168" data-id="551a9969e4b0ca1ce4ef03a2" data-genre="index:bowlatboycs" data-sharebuttons="true" data-autostart="false" data-expirationDate="" data-embargoDate="" data-mezzaninepathimage="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_BAB_20150331/cric_150331_COM_CRICKET_BAB_20150331.jpg?w=300&h=168" data-unicornurl="" data-brightcoveurl="" data-assetName="" data-duration="" data-headlineShort="" ></div>	
	<picture>
		<img class="espni-vid-thumb full" src="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_BAB_20150331/cric_150331_COM_CRICKET_BAB_20150331.jpg?w=300&h=168" alt="">
	</picture>
	<span class="video-play-button espni-vid-thumb ">Play</span>
	
	<span class="video-length espni-vid-thumb">21:02</span>
	
</div>


                    </div>
                </div>
                <div class="content">
                    
                    <h4><span class="icon-font icon-play-solid"></span><a href="/ci/content/video_audio/857629.html">'Money is the key to the length of the World Cup'</a></h4>
                    
                </div>
            
        </li>
    
        <li>
            
                <div class="thumbnail">
                    <div class="img-wrap">
                       

<div class="espni-vid-wrapper img-wrap">
	<div class="espni-vid-placeholder" data-width="100%" data-height="168" data-id="551a91b0e4b0ca1ce4ef0384" data-genre="index:newsandanalysis" data-sharebuttons="true" data-autostart="false" data-expirationDate="" data-embargoDate="" data-mezzaninepathimage="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_ASSO_201503031/cric_150331_COM_CRICKET_ASSO_201503031.jpg?w=300&h=168" data-unicornurl="" data-brightcoveurl="" data-assetName="" data-duration="" data-headlineShort="" ></div>	
	<picture>
		<img class="espni-vid-thumb full" src="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_ASSO_201503031/cric_150331_COM_CRICKET_ASSO_201503031.jpg?w=300&h=168" alt="">
	</picture>
	<span class="video-play-button espni-vid-thumb ">Play</span>
	
	<span class="video-length espni-vid-thumb">04:35</span>
	
</div>


                    </div>
                </div>
                <div class="content">
                    
                    <h4><span class="icon-font icon-play-solid"></span><a href="/ci/content/video_audio/857625.html">Chappell: Game's got to globalise in T20</a></h4>
                    
                </div>
            
        </li>
    
        <li>
            
                <div class="thumbnail">
                    <div class="img-wrap">
                       

<div class="espni-vid-wrapper img-wrap">
	<div class="espni-vid-placeholder" data-width="100%" data-height="168" data-id="551a91b0e4b0ca1ce4ef0384" data-genre="index:newsandanalysis" data-sharebuttons="true" data-autostart="false" data-expirationDate="" data-embargoDate="" data-mezzaninepathimage="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_ASSO_201503031/cric_150331_COM_CRICKET_ASSO_201503031.jpg?w=300&h=168" data-unicornurl="" data-brightcoveurl="" data-assetName="" data-duration="" data-headlineShort="" ></div>	
	<picture>
		<img class="espni-vid-thumb full" src="http://a.espncdn.com/combiner/i?img=/media/motion/cricinfo/2015/0331/cric_150331_COM_CRICKET_ASSO_201503031/cric_150331_COM_CRICKET_ASSO_201503031.jpg?w=300&h=168" alt="">
	</picture>
	<span class="video-play-button espni-vid-thumb ">Play</span>
	
	<span class="video-length espni-vid-thumb">04:35</span>
	
</div>


                    </div>
                </div>
                <div class="content">
                    
                    <h4><span class="icon-font icon-play-solid"></span><a href="/ci/content/video_audio/857489.html">Kamal angered by World Cup trophy snub</a></h4>
                    
                </div>
            
        </li>
    
    </ul>
</section>
<!-- Top videos module end -->

<div class="boundary-meter-wrapper module hide-portrait hide-for-mob">
    <div class="content-padding">
       <h5 class="section-name">Boundary Meter</h5>
   </div>
    <object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=9,0,28,0" name="boundry_meter" width="295" height="80" align="middle" id="boundry_meter">
        <param name="allowScriptAccess" value="always" />
        <param name="movie" value="http://www.espncricinfo.com/navigation/cricinfo/IPL_boundaryMeter.swf" />
        <param flasvars=value="high" />
        <param name="quality" value="high" />
        <param name="bgcolor" value="#ffffff" />
        <param name="FlashVars" value="xmlFile=http://www.espncricinfo.com/navigation/cricinfo/six4_6537.xml" /> 
        <embed src="http://www.espncricinfo.com/navigation/cricinfo/IPL_boundaryMeter.swf" FlashVars="xmlFile=http://www.espncricinfo.com/navigation/cricinfo/six4_6537.xml" quality="high" bgcolor="#ffffff" width="295" height="80" name="audio_file" align="middle" allowScriptAccess="always" type="application/x-shockwave-flash" pluginspage="http://www.macromedia.com/go/getflashplayer" />
    </object>
    <div class="explore-links">
        <ul>
            <li><a href="/icc-cricket-world-cup-2015/engine/records/batting/most_fours_career.html?id=6537;type=tournament">MOST FOURS</a></li>
            <li><a href="/icc-cricket-world-cup-2015/engine/records/batting/most_sixes_career.html?id=6537;type=tournament">MOST SIXES</a></li> 
        </ul>
    </div>
</div>
<style>
.boundary-meter-wrapper{
    text-align: center;
}
.boundary-meter-wrapper .content-padding{
   padding: 0;
}
.boundary-meter-wrapper .content-padding h5{
   padding: 20px 0 20px 20px;
}
.boundary-meter-wrapper .content-padding img{
   float: right;
   width: 90px;
   height: 23px;
   margin: 15px 10px 20px 0;
}
.boundary-meter-wrapper .explore-links ul{
    float: none;
}
.boundary-meter-wrapper .explore-links ul li{
    float: none;
    display: inline-block;
}
</style>
	
</section>
        	
        </section>
        <section class="intermediate hide-all show-portrait ch-bottom-sections">
        <!-- Editor Picks module start -->
<section class="editor-picks hide-all show-portrait module">	<div class="content-padding">
		<h5 class="section-name"><a href="/ci/content/story/editors_pick.html">Editor's Picks</a></h5>
	</div>
	<!-- Editor Picks Items start-->
	<ul>
		<li>
			<div class="thumbnail">
				<div class="img-wrap">
					
					
						<picture>
							<a name="&lpos=editorspick_1" href="/magazine/content/story/857201.html"><img src="http://p.imgci.com/db/PICTURES/CMS/209800/209873.5.jpg" alt="" title="" class="img-full" /></a>
						</picture>
					
				</div>
			</div>
			<div class="content">
				
				<h2><a name="&lpos=editorspick_1" href="/magazine/content/story/857201.html">What did we learn from this World Cup?</a></h2>
			</div>
			<ul class="link-cluster">
			
				<li><span class=""></span>That there is a place for proper batsmanship in ODIs, that New Zealand punch above their weight, and that wickets win you matches. By <b>Ed Smith</b></li>
				
            </ul>
		</li>
	
		<li>
			<div class="thumbnail">
				<div class="img-wrap">
					
					
						<picture>
							<a name="&lpos=editorspick_2" href="/magazine/content/story/856297.html"><img src="http://p.imgci.com/db/PICTURES/CMS/209700/209725.6.jpg" alt="" title="" class="img-full" /></a>
						</picture>
					
				</div>
			</div>
			<div class="content">
				
				<h2><a name="&lpos=editorspick_2" href="/magazine/content/story/856297.html">The greatest time of our lives</a></h2>
			</div>
			<ul class="link-cluster">
			
				<li><span class=""></span><b>Martin Crowe:</b> Whatever happens, the Australia-New Zealand World Cup final at the MCG will be the most divine fun</li>
				
            </ul>
		</li>
	
		<li>
			<div class="thumbnail">
				<div class="img-wrap">
					
					
						<picture>
							<a name="&lpos=editorspick_3" href="/magazine/content/story/852755.html"><img src="http://p.imgci.com/db/PICTURES/CMS/207100/207187.4.jpg" alt="" title="" class="img-full" /></a>
						</picture>
					
				</div>
			</div>
			<div class="content">
				
				<h2><a name="&lpos=editorspick_3" href="/magazine/content/story/852755.html">Left-arm quicks rule</a></h2>
			</div>
			<ul class="link-cluster">
			
				<li><span class=""></span><b>Numbers Game:</B> Mitchell Starc and Trent Boult have led the way, but other left-arm fast bowlers have also shone in this World Cup</li>
				
            </ul>
		</li>
	
		<li>
			<div class="thumbnail">
				<div class="img-wrap">
					
					
						<picture>
							<a name="&lpos=editorspick_4" href="/magazine/content/story/852631.html"><img src="http://p.imgci.com/db/PICTURES/CMS/208900/208993.5.jpg" alt="" title="" class="img-full" /></a>
						</picture>
					
				</div>
			</div>
			<div class="content">
				
				<h2><a name="&lpos=editorspick_4" href="/magazine/content/story/852631.html">The serenity of Kane Williamson</a></h2>
			</div>
			<ul class="link-cluster">
			
				<li><span class=""></span><b>Martin Crowe:</b> He is assured, humble and intelligent: a player opposition captains and bowlers find hard to combat</li>
				
            </ul>
		</li>
	
		<li>
			<div class="thumbnail">
				<div class="img-wrap">
					
					
						<picture>
							<a name="&lpos=editorspick_5" href="/icc-cricket-world-cup-2015/content/story/851397.html"><img src="http://p.imgci.com/db/PICTURES/CMS/207100/207117.4.jpg" alt="" title="" class="img-full" /></a>
						</picture>
					
				</div>
			</div>
			<div class="content">
				
				<h2><a name="&lpos=editorspick_5" href="/icc-cricket-world-cup-2015/content/story/851397.html">Build, build, blast off</a></h2>
			</div>
			<ul class="link-cluster">
			
				<li><span class=""></span><b>Sharda Ugra:</B> This World Cup,most teams seem to have followed a strategy of constructing an innings till about the last quarter and then gone full pelt</li>
				
            </ul>
		</li>
		</ul>
	<!-- Editor Picks Items end-->
	

	
	<!-- Ad center column start -->
	<div class="ad-container ad-center-column ad-140">
		<div class="espni-ad-slot ad-panel" data-slot-type="panel" data-kvpos="top" data-exclude-bp="s,m"></div>
	</div>
	<!-- Ad center column end -->
	<!-- mpu Ad -->
	<div class="ad-container ad-center-column hide show-landscape">
		<div class="espni-ad-slot ad-incontent" data-slot-type="incontent" data-kvpos="top" data-exclude-bp="s,m,xl"></div>
	</div>
	<!-- mpu Ad End -->
</section>
<!-- Editor Picks module end --><div class="ad-slot show-mobile"><div class="espni-ad-slot ad-incontent" data-slot-type="incontent" data-kvpos="bottom" data-exclude-bp="m,l,xl"></div></div>
  <section class="ch-points-table module ">
      <div class="content-padding all-link">
          <h5 class="section-name">Points Table</h5>
          <a href="/icc-cricket-world-cup-2015/engine/series/509587.html?view=pointstable">All &rarr;</a>
      </div>
      <div class="group">
        <p class="heading">Pool A</p>
<table>
   	<tbody>
		<tr>
	      	<th class="bold">T<span>eams</span></th>
	      	<th class="bold">M<span>at</span></th>
	      	<th class="bold">W<span>on</span></th>
	      	<th class="bold">L<span>ost</span></th>
	      	<th class="bold">T<span>ied</span></th>
  			<th class="bold">N/R</th>
      		<th class="bold last">P<span>ts</span></th>

 		</tr>
		<tr>
			<td>
NZ
	 		</td>
			<td>6</td>
			<td>6</td>
			<td>0</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">12</td>
     	</tr>
		<tr>
			<td>
AUS
	 		</td>
			<td>6</td>
			<td>4</td>
			<td>1</td>
			<td>0</td>
      		<td>1</td>
     		<td class="last">9</td>
     	</tr>
		<tr>
			<td>
SL
	 		</td>
			<td>6</td>
			<td>4</td>
			<td>2</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">8</td>
     	</tr>
		<tr>
			<td>
BDESH
	 		</td>
			<td>6</td>
			<td>3</td>
			<td>2</td>
			<td>0</td>
      		<td>1</td>
     		<td class="last">7</td>
     	</tr>
		<tr>
			<td>
ENG
	 		</td>
			<td>6</td>
			<td>2</td>
			<td>4</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">4</td>
     	</tr>
		<tr>
			<td>
AFG
	 		</td>
			<td>6</td>
			<td>1</td>
			<td>5</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">2</td>
     	</tr>
		<tr>
			<td>
SCOT
	 		</td>
			<td>6</td>
			<td>0</td>
			<td>6</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">0</td>
     	</tr>
	</tbody>
</table>

  <p class="heading">Pool B</p>
<table>
   	<tbody>
		<tr>
	      	<th class="bold">T<span>eams</span></th>
	      	<th class="bold">M<span>at</span></th>
	      	<th class="bold">W<span>on</span></th>
	      	<th class="bold">L<span>ost</span></th>
	      	<th class="bold">T<span>ied</span></th>
  			<th class="bold">N/R</th>
      		<th class="bold last">P<span>ts</span></th>

 		</tr>
		<tr>
			<td>
INDIA
	 		</td>
			<td>6</td>
			<td>6</td>
			<td>0</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">12</td>
     	</tr>
		<tr>
			<td>
SA
	 		</td>
			<td>6</td>
			<td>4</td>
			<td>2</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">8</td>
     	</tr>
		<tr>
			<td>
PAK
	 		</td>
			<td>6</td>
			<td>4</td>
			<td>2</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">8</td>
     	</tr>
		<tr>
			<td>
WI
	 		</td>
			<td>6</td>
			<td>3</td>
			<td>3</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">6</td>
     	</tr>
		<tr>
			<td>
IRE
	 		</td>
			<td>6</td>
			<td>3</td>
			<td>3</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">6</td>
     	</tr>
		<tr>
			<td>
ZIM
	 		</td>
			<td>6</td>
			<td>1</td>
			<td>5</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">2</td>
     	</tr>
		<tr>
			<td>
UAE
	 		</td>
			<td>6</td>
			<td>0</td>
			<td>6</td>
			<td>0</td>
      		<td>0</td>
     		<td class="last">0</td>
     	</tr>
	</tbody>
</table>


      </div>
  </section>
  
    <section class="ch-statistics module ">
        <div class="content-padding">
            <h5 class="section-name">Statistics</h5>
        </div>
        <div class="content">
        
            <ul>
                <li>
                    <h2>ICC Cricket World Cup, 2014/15</h2>
                    <p> (in Australia/New Zealand ODIs)</p>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/batting/most_runs_career.html?id=6537;type=tournament">Most runs</a> | </span>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/bowling/most_wickets_career.html?id=6537;type=tournament">Most wickets</a> | </span>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/batting/most_runs_innings.html?id=6537;type=tournament">High scores</a> | </span>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/bowling/best_figures_innings.html?id=6537;type=tournament">Best bowling</a> | </span>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?id=6537;type=tournament">More</a></span>
                    
                </li>
            </ul>
            
            <ul>
                <li>
                    <h2>Chappell-Hadlee Trophy, 2014/15</h2>
                    <p> (Australia in New Zealand ODIs)</p>
                    <span><a href="/ci/engine/records/batting/most_runs_career.html?id=9897;type=series">Most runs</a> | </span>
                    <span><a href="/ci/engine/records/bowling/most_wickets_career.html?id=9897;type=series">Most wickets</a> | </span>
                    <span><a href="/ci/engine/records/batting/most_runs_innings.html?id=9897;type=series">High scores</a> | </span>
                    <span><a href="/ci/engine/records/bowling/best_figures_innings.html?id=9897;type=series">Best bowling</a> | </span>
                    <span><a href="/ci/engine/records/index.html?id=9897;type=series">More</a></span>
                    
                </li>
            </ul>
            
                    <h2>One-Day Internationals</h2>
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2">Overall | </a></span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=40;type=team">Afghanistan</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2;type=team">Australia</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=25;type=team">Bangladesh</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=1;type=team">England</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=6;type=team">India</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=29;type=team">Ireland</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=5;type=team">New Zealand</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=7;type=team">Pakistan</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=30;type=team">Scotland</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=3;type=team">South Africa</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=8;type=team">Sri Lanka</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=27;type=team">United Arab Emirates</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=4;type=team">West Indies</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=9;type=team">Zimbabwe</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2;type=host">Australia</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=5;type=host">New Zealand</a> | </span>
                    
                    <span><a href="/icc-cricket-world-cup-2015/engine/records/index.html?class=2;id=2017;type=year">Year 2017</a></span>
                
        </div>
    </section>
    

<section class="ch-squad module ">
    <div class="content-padding">
        <h5 class="section-name">Squad</h5>
    </div>
    <div class="squad-dd">
      <dl class="dropdown">
        <dt><a><span>Bangladesh Squad</span></a></dt>
        <dd>
        <ul>
    
        <li><a data-value="10723" class="default">Bangladesh Squad</a></li>
      
        <li><a data-value="10737" class="">South Africa Squad</a></li>
      
        <li><a data-value="10749" class="">Zimbabwe Squad</a></li>
      
        <li><a data-value="10753" class="">Scotland Squad</a></li>
      
        <li><a data-value="10771" class="">Australia Squad</a></li>
      
        <li><a data-value="10775" class="">United Arab Emirates Squad</a></li>
      
        <li><a data-value="10729" class="">Ireland Squad</a></li>
      
        <li><a data-value="10733" class="">India Squad</a></li>
      
        <li><a data-value="10747" class="">Pakistan Squad</a></li>
      
        <li><a data-value="10681" class="">England Squad</a></li>
      
        <li><a data-value="10699" class="">Afghanistan Squad</a></li>
      
        <li><a data-value="10773" class="">West Indies Squad</a></li>
      
        <li><a data-value="10745" class="">Sri Lanka Squad</a></li>
      
        <li><a data-value="10751" class="">New Zealand Squad</a></li>
      
      </ul>
      </dd>
    </dl>
  </div>
  <div class="content">
    <ul class="list-block" id="squad_list_d">
  
      <li><a href="/icc-cricket-world-cup-2015/content/player/56007.html">Mashrafe Mortaza (c)</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/380354.html">Anamul Haque</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/56268.html">Arafat Sunny</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/280734.html">Imrul Kayes</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/56025.html">Mahmudullah</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/373696.html">Mominul Haque</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/56029.html">Mushfiqur Rahim (wk)</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/300618.html">Nasir Hossain</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/300619.html">Rubel Hossain</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/373538.html">Sabbir Rahman</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/56143.html">Shakib Al Hasan</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/436677.html">Soumya Sarkar</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/401057.html">Taijul Islam</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/56194.html">Tamim Iqbal</a></li>
      
      <li><a href="/icc-cricket-world-cup-2015/content/player/538506.html">Taskin Ahmed</a></li>
      
    </ul>
  </div>
</section>

        </section>
        
    </div>
</div>
<script language="javascript">
	var omniPageName = "homepage";
	var omniSiteSection1 = "ICC Cricket World Cup 2015";
	var omniCt = "section homepage";
	var _PAGETYPE = "country_home";
	var _SERIES = "series_home";
	var _branding = "icc-cricket-world-cup-2015";
	var _TEAMID = "509587";
</script>	 

    </div>
    <!-- Homepage wrapper End -->

    <!-- Footer Start -->
    <div class="site-container">
    <footer>
    <div>
        <div class="footer-links">
            <ul class="horizontal">
                <li><a href="/ci/content/page/866033.html" class="maintabs ui-link">Sitemap</a></li>
                <li><a href="/ci/content/submit/forms/feedback.html" class="maintabs ui-link">Feedback</a></li>
                <li><a href="/ci/content/rss/feeds_rss_cricket.html" class="maintabs ui-link">RSS</a></li>
                <li><a href="/ci/content/page/156066.html" class="maintabs ui-link">About Us</a></li>
                <li><a href="/ci/content/site/careers/careers.html" class="maintabs ui-link">Careers</a></li>
                <!-- <li><a href="/ci/content/page/407211.html" class="maintabs ui-link">Advertise</a></li> -->
                <li><a href="http://disneyprivacycenter.com/" class="maintabs ui-link" target="_blank">Privacy Policy</a></li>
                <li><a href="http://disneytermsofuse.com/" class="maintabs ui-link" target="_blank">Terms of Use</a></li>
            </ul>
			<ul>
				<li><a target="_blank" href="http://preferences-mgr.truste.com/?pid=disney01&aid=espnemea01&type=espnemea">Interest Based Ads</a></li>
				<li><a target="_blank" href="https://disneyprivacycenter.com/notice-to-california-residents/">Your California Privacy Rights</a></li>
				<li><a target="_blank" href="https://disneyprivacycenter.com/kids-privacy-policy/english/">Children’s Online Privacy Policy</a></li>
                <li><a target="_blank" href="http://www.nielsen.com/digitalprivacy">About Nielsen Measurement</a></li>
			</ul>
        </div>
        <div class="footer-other-sites">
            <div class="logo-sprite">
                <img src="http://i.imgci.com/espncricinfo/redesign/espn-family-logo_3x.png" alt="" usemap="#Map" border="0"/>
                <map name="Map" id="Map">
                    <area alt="" title="" href="http://espn.go.com/" shape="poly" coords="1,1,53,2,52,16,1,16" />
                    <area alt="" title="" href="http://www.espnf1.com/" shape="poly" coords="72,1,93,2,93,17,72,17" />
                    <area alt="" title="" href="http://www.espnscrum.com/" shape="poly" coords="109,1,180,1,181,16,109,16" />
                    <area alt="" title="" href="http://www.espnfc.com/" shape="poly" coords="198,1,280,1,281,15,198,15" />
                    <area alt="" title="" href="http://footytips.com.au" shape="poly" coords="296,1,370,1,372,17,298,17" />
                </map>
            </div>
			<div class="logo-sprite copyright">
				<span>
					&copy; ESPN Sports Media Ltd.&#32;
				</span>
			</div>
        </div>
    </div>
</footer>
<style>
@media screen and (min-width: 769px){
    footer .footer-links ul{
        margin-bottom:10px;
    }
}
</style>

    </div>
    <!-- Footer End -->



    

<script type="text/javascript">
	var endpoint = "submit.espncricinfo.com";
	var server = "www.espncricinfo.com";
</script>

    
        <!-- React.js -->
        <!--[if lte IE 8]>
            <script src="/navigation/cricinfo/ci/assets/js/plugins/shims.js"></script>
        <![endif]-->
        <script>
            __proxy = "http://54.186.53.178:1377/proxy/?url=";
            window.espni = window.espni || {};
            window.espni.vars = window.espni.vars || {};
            window.espni.vars.edition = '' || 'www';
            window.espni.vars.cluster = 'ind';
            window.espni.vars.country = 'in';
            window.espni.vars._cluster = '';
            window.espni.vars.version = '' || 'web';
            window.espni.vars.proxy = '' || false;
            window.espni.vars.headlinesPollInterval = '' || '300000';
            window.espni.vars.liveScoresPollInterval = '' || '20000';
            window.espni.vars.disableMEM = '';
            window.espni.vars.layoutId = '';
            
            window.espni.vars.netstorageUrl = 'http://ns.espncricinfo.com/netstorage/summary.json';
            
        </script>
        <!-- React.js -->
    

    <!-- VOD player dependencies -->
    <script src="http://i.imgci.com/navigation/cricinfo/ci/video/js/min/libs.min.js"></script>    
    <script src="http://a.espncdn.com/combiner/c?js=jquery-1.7.1.js,plugins/jquery.metadata.js,plugins/jquery.pubsub.r5.js,plugins/ba-debug-0.4.js,espn.l10n.r12.js,espn.core.duo.r55.js,espn.storage.r6.js,espn.p13n.r16.js,espn.geo.r4.js"></script>
    <script type="text/javascript">
        var _u = _.noConflict();
    </script>
    
<script type="text/javascript">
  window.espn_ui = window.espn_ui || {};
  espn_ui.Helpers = espn_ui.Helpers || {};
  espn_ui.Helpers.nav = espn_ui.Helpers.nav || {};
  espn_ui.Helpers.nav.isNavExternal = true;
  /*
  if(window.location.hostname.indexOf('kilimanjaro')<0) {
    window.espn = window.espn || {};
    espn.i18n = espn.i18n || {};
    espn.i18n.domain = window.location.hostname;
}*/
</script>

<script src="http://a.espncdn.com/redesign/0.371.5/js/espncricinfo-critical.js"></script>

<script>
  
  (function(){
      var deferLoaded = false;
      function loadDefer(){
          if( !deferLoaded ){
              var deferScripts = ['http://a.espncdn.com/redesign/0.371.5/js/espn-defer-low.js'];
              for( var s = 0; s < deferScripts.length; s++ ){
                  var script = document.createElement('script');
                  script.src = deferScripts[s];
                  document.getElementsByTagName('head')[0].appendChild(script);
              }
              deferLoaded = true;
          }
      }
      if( window.espn.loadType === "loadEnd" && ( espn_ui.device.isMobile === true || espn_ui.device.isTablet === true ) ){
          $( window ).load(function() {
              setTimeout( loadDefer, 0 )
          });
          setTimeout( loadDefer, 5000 )
      }else{
          loadDefer();
      }
  })();
</script>


<script src="http://a.espncdn.com/redesign/0.371.5/external/release/js/nav-external.js"></script>


<script>
    espn_ui.onefeed =true;
    espn_ui.externalRef = ""
    $(window).load(function() {
        $('#global-viewport img').each(function() {
            if (!this.complete || typeof this.naturalWidth == "undefined" || this.naturalWidth == 0) {
                $(this).parent().remove();
            }
        });
    });
</script>

<script>
    if($.cookie('scoresLinkAlertCookie') !== "true"){
        $(".scores-link-alert").removeClass("hidden");
    }

    $('#global-scoreboard-trigger').add('.scores-link-alert').on("click", function(){
        // Cookie expiry time set to 1 year
        $.cookie('scoresLinkAlertCookie', true, {expires: 365});
        $(".scores-link-alert").remove();
    })
</script>
        <script src="http://i.imgci.com/navigation/cricinfo/ci/assets/js/src/sub_nav.min.js?v=1500526114"></script>
    <!-- VOD Shim starts -->

<script src="http://a.espncdn.com/prod/scripts/video/2.14.1-5/espn.video.universal.min.js"></script>
<script src="http://i.imgci.com/navigation/cricinfo/ci/vod_player/js/min/video.1.0.0.js"></script>
<!-- VOD Shim ends -->
    <script src="http://i.imgci.com/navigation/cricinfo/ci/assets/js/src/espncricinfo.gpt.1.0.0.min.js?v=1501244305"></script>
    <link href="http://i.imgci.com/navigation/cricinfo/ci/packager/dist/css/home-0.0.1.min.css?v=1459760940">
    
        <link href='http://a.espncdn.com/combiner/c?css=fonts/bentonsans.css,fonts/bentonsansbold.css,fonts/bentonsanslight.css,fonts/bentonsansmedium.css,fonts/bentonsanscond.css,fonts/bentonsanscondbold.css,fonts/bentonsanscondmedium.css'/>
    
    
        <script src="http://i.imgci.com/navigation/cricinfo/ci/packager/dist/js/countryhome-0.0.4.min.js?v=1500549439"></script>
    
    <script type='text/javascript'>
        (function(){
            $(".multimedia img, li img:not(img[data-hide-thumbs]), .cric-facebook iframe").unveil();
            //delayed gpt initialization till everything loads
            var everythingLoaded = setInterval(function() {
              if (/loaded|complete/.test(document.readyState)) {
                clearInterval(everythingLoaded);
                initGPTBundle();
              }
            }, 10);

            $(function(){
                $(".multimedia").on("click", ".bx-next",function(){$('.multimedia li img').unveil();});
                $('#tcmslider-300-190').bxSlider({
                    responsive: 'true',
                    prevSelector: 'false',
                    nextSelector: 'false',
                    mode: 'fade',
                    preloadImages: 'visible',
                    autoStart: true,
                    randomStart: true,
                    autoDirection: 'next',
                    auto: true
                });
                $('.medium-image.slider-wrapper').css({'visibility':'visible', 'max-height':''});
                $.cookie('bentonLoaded', true, {path:'/', domain:'espncricinfo.com'});
            });

               //Initialize GPT for new redesigned templates with bundle
            function initGPTBundle(){
                //return if Parms.debug is not true
                if (typeof __GPTenabled === 'undefined'){
                    return;
                }
               //pubsub plugin init
               (function(d){var cache={};d.publish=function(topic,args){cache[topic]&&d.each(cache[topic],function(){this.apply(d,args||[])})};d.subscribe=function(topic,callback){if(!cache[topic]){cache[topic]=[]}cache[topic].push(callback);return[topic,callback]};d.unsubscribe=function(handle){var t=handle[0];cache[t]&&d.each(cache[t],function(idx){if(this==handle[1]){cache[t].splice(idx,1)}})}})(jQuery);

              $.subscribe("ad.rendered", function(key, event) {
                 // if the overlay slot is empty, we can load in-page ad units immediately
                 if(event.slot.type === "overlay") {
                    if(event.isEmpty === true && espni.ads.config.delayInPageAdSlots === true) {
                       espni.ads.refreshInPageAdSlots();
                    }
                 }
              });

              // overlays will publish an event when auto-closed or closed by user
              $.subscribe("ad.overlay.close", function() {
                 if(espni.ads.config.delayInPageAdSlots === true) {
                    espni.ads.refreshInPageAdSlots();
                 }
              });

              //__GPTconfig variable comes from the espncricinfogpt api;
              if(typeof __GPTconfig !== 'undefined'){
                espni.ads.init(__GPTconfig);
              }
           }
        }());
    </script>













    <script type="text/javascript">
        (function(d, s, id) {
          var js, fjs = d.getElementsByTagName(s)[0];
          if (d.getElementById(id)) return;
          js = d.createElement(s); js.id = id;
          js.src = "//connect.facebook.net/en_US/sdk.js?&appId=260890547115&version=v2.1";
          fjs.parentNode.insertBefore(js, fjs);
        }(document, 'script', 'facebook-jssdk'));
        window.fbAsyncInit = function() {
            FB.init({
                appId      : '260890547115',
                xfbml      : true,
                status     : true, // check login status
                cookie     : true, // enable cookies to allow the server to access the session
                oauth      : true,
                version    : 'v2.1'
            });
            if(typeof cilogin !== "undefined"){
                cilogin.facebookapiinit();
            }
        };
    </script>



    <!-- add analytics script to HEAD tag -->

<script type="text/javascript" src="http://a.espncdn.com/combiner/c?js=analytics/VisitorAPI.js,analytics/sOmni.2.js"></script>
<script type="text/javascript">
    (function() {
        var countryRegion = "in";
        var editionKeyMap = {
            "espncricinfo-en-uk":"United Kingdom",
            "espncricinfo-en-us":"United States",
            "espncricinfo-en-in":"India",
            "espncricinfo-en-au": "Australia",
            "espncricinfo-en-za": "Africa",
            "espncricinfo-en-bd": "Bangladesh",
            "espncricinfo-en-ww": "Global",
            "espncricinfo-en-nz": "New Zealand",
            "espncricinfo-en-pk": "Pakistan",
            "espncricinfo-en-lk": "Srilanka"
            };

        if(typeof omniSiteSection1 == 'undefined') {
            omniSiteSection1 = '';
        }
        if(typeof omniPageName == 'undefined') {
            omniPageName = '';
        }
        if(typeof omniCt == 'undefined') {
            omniCt = '';
        }

        if(typeof omniSiteSection2 != 'undefined' && typeof omniSiteSubSection3 != 'undefined'){
            omni_PageName = omniSiteSection1.toLowerCase() + ":" + omniSiteSection2.toLowerCase() + ":" + omniSiteSection3.toLowerCase() + ":" + omniPageName.toLowerCase();  //Page name
        }else if(typeof omniSiteSection2 != 'undefined'){
            omni_PageName = omniSiteSection1.toLowerCase() + ":" + omniSiteSection2.toLowerCase() + ":" + omniPageName.toLowerCase();                                                 //Page name
        }else{
            omni_PageName = omniSiteSection1.toLowerCase() + ":" + omniPageName.toLowerCase();                                                                                                    //Page name
        }

        if(typeof omniSiteSection1 != 'undefined'){
            omni_channel = omniSiteSection1.toLowerCase();
        }

        var orient = (window.innerWidth >= window.innerHeight) ? 'landscape' : 'portrait';
        if ( espn && espn.track ) {
            if(espn.i18n.editionKey) {
                countryRegion = editionKeyMap[espn.i18n.editionKey];
            }
            espn.track.init = function () {
                var data = {
                    omniture: {
                        site: "cricinfo",
                        pageName: omni_PageName,
                        sections: omni_channel,
                        premium: "n",
                        account: "wdgespcricinfo",
                        sport: "cricket",
                        contentType: omniCt,
                        convrSport: "cricket",
                        lang: "en",
                        section: omniSiteSection1,
                        countryRegion: countryRegion,
                        fantasyPersonalize: "has favorites:no-fantasy:no-notifications:no-docking:no-autostart:no",
                        orientation : orient,
                        deviceInfo : orient,
                        prop5:omni_PageName,
                        prop29:"anonymous",
                        enableCB: false
                    },
                    chartbeat: {
                        uid: 26455,
                        domain: "espncricinfo.com",
                        useCanonical: true,
                        sections: "Series",
                        authors: "in",
                        path: window.location.pathname,
                        title: "ICC Cricket World Cup 2015 | Cricket news, live scores, fixtures, features and statistics on ESPN Cricinfo",
                        loadPubJS: false,
                        loadVidJS: false
                    },
                };
                // Init Chartbeat
                espn.track.initCB( data.chartbeat );
                // Init Comscore (optional - trackpage call )
                //espn.track.comscore(); 
                // Global analytics properties used by video players
                var anData = data.omniture;
                window.s_account = anData.account;
                window.omniSite = anData.site;
                window.omniPageName = anData.site+":"+anData.pageName;
                window.omniTrackingName = "cities";
                 
                // slight delay
                function init_o() {
                    // detect device orientation however before page track call
                    var isMobile = function() {
                        return ( /android|webos|iphone|ipad|ipod|blackberry|iemobile|opera mini/i.test(navigator.userAgent.toLowerCase() ) ) ? true : false;
                    };
                    var orient = isMobile()  ? ( (window.innerWidth >= window.innerHeight) ? 'landscape' : 'portrait' ) : 'desktop';
                    // page track call
                    espn.track.trackPage( anData );
              
                    $.subscribe("omni.pageTracked", function(s_omni) {
                        // do something trigger after tracing
                    });
                }
                // can inite events to add additional tracking or trigger publish evient
                if(typeof s_omni === "undefined") {
                    
                    setTimeout(function(){
              
                        $.ajaxSetup({ cache: true });
                        // we want to load the analytics files from the cache if possible - so, let's use full $.ajax() calls
                        $.getScript("http://a.espncdn.com/combiner/c?js=analytics/VisitorAPI.js,analytics/sOmni.2.js,analytics/analytics.2.js,analytics/externalnielsen.js", init_o);
              
                        $.subscribe("omni.loaded", function(s_omni) {
                            // do something
                        });
              
                    }, 500);
                }
                else {
                    init_o();
                }
            };
            espn.track.init();
        }
    })();
</script><style type="text/css">
  .cookie-overlay{background-color:rgba(0,0,0,.75);bottom:0;position:fixed;width:100%;padding:15px 0;text-align:center;z-index:9999;display:none;}.cookie-overlay h1{font-size:18px;font-weight:600;text-transform:uppercase;width:90%;margin:0 auto;color:#fff}.cookie-overlay p.message{width:90%;margin:0 auto;padding:5px 0 10px;color:#fff}.cookie-overlay p.message a{font-family:BentonSansBold,Arial,sans-serif;color:#fff}.cookie-overlay p.message a:hover{color:#439ec9;text-decoration:underline}.cookie-overlay .cookie-continue{display:block;font-size:11px;margin:auto;width:140px;line-height:2.8;padding:0 15px;max-width:320px;min-width:120px;border-radius:4px;background-color:#06c;color:#fff;cursor:pointer;border:0;font-family:BentonSansMedium,Arial,sans-serif}.cookie-overlay .cookie-continue:hover{background-color:#007aff}@media screen and (min-width:0) and (max-width:768px){.cookie-overlay p.message{font-size:12px}}
</style>

<div class="cookie-overlay">
    <h1>ABOUT COOKIES</h1>
    <p class="message">
        We use cookies to help make this website better, to improve our services and for advertising purposes. You can learn more about our use of cookies and change your browser settings in order to avoid cookies by clicking <a href="https://disneyprivacycenter.com/cookies-policy-translations/cookies-policy/">here</a>. Otherwise, we'll assume you are OK to continue.
    </p>
    <button class="cookie-continue">Continue</button>
</div>
<script type="text/javascript">

  /**
   * Should cookie message get displayed for this cluster?
   * @return {boolean} true if cookie message needs to be shown else false
   */
  function __validateCookieClusters(){
    var __isUKCluster = (location_cluster == 'uk') ? true : false
    var __isEuroCluster = (location_cluster == 'eur') ? true : false
    var __countryArr = ["at","be","bg","hr","cy","cz","dk","ee","fi","fr",
                        "de","gr","hu","ie","it","lv","lt","lu","mt","nl",
                        "pl","pt","ro","sk","si","es","se"];

    // Show cookie message only for uk and other eu countries
    var showCookieMessage = __isUKCluster || (__isEuroCluster && (__countryArr.indexOf(location_country) > -1) )
    return showCookieMessage;
  }

  /**
   * Display cookie message for specified clusters
   */
  function __displayCookie(){
    // Check if jQuery is present on page else return
    if (typeof $ === 'undefined') {
      return
    }

    if($.cookie('about_cookie') == "1"){
        $(".cookie-overlay").hide();
    }else{
        $(".cookie-overlay").show();
    }

    $(".cookie-continue").on("click", function(){
        //var __expireTime = new Date;
        //__expireTime.setFullYear(__expireTime.getFullYear( ) +1);
        //$.cookie('about_cookie', '1', {expires: __expireTime, path:'/', domain:'.espncricinfo.com'});

        // Cookie expiry time set to 1 year
        $.cookie('about_cookie', '1', {expires: 365, path:'/', domain:'.espncricinfo.com'});
        $(".cookie-overlay").hide();
    })
  }

  if (typeof jQuery !== 'undefined') {
    // Setup on ready callback only for specified clusters
    if (__validateCookieClusters() === true) {
      $(document).ready(__displayCookie);
    }
  }

</script>








        <script type="text/javascript">
            (function(espn, $, navigator) {
	window.espn = espn || {};
	espn.bandwidth = {
		test: function(callback, options) {
			var count = 0,
				files = [
					{ url: 'http://a.espncdn.com/i/data-lite/2.jpg', bytes: 65536 }
				];

			callback = callback || function() {};
			options = options || {};

			function _done() {
				var bandwidth = Math.max.apply(Math, files.map(function(file) {
					return file.bandwidth || -Infinity;
				}));
				callback(bandwidth, {downloads: count});
			}

			function _calculateBandwidthForFile(file) {
				var seconds = (file.endTime - file.startTime) / 1000,
					kb = file.bytes * 8 / 1000;
				if (seconds > 0) {
					return Math.floor(kb / seconds);
				}
			}

			function _download(file) {
				var xhr = new XMLHttpRequest();
				xhr.onloadstart = function() {
					file.startTime = +new Date;
				};
				xhr.onreadystatechange = function() {
					if (xhr.readyState === 4 && xhr.status === 200) {
						file.endTime = +new Date;
						file.bandwidth = _calculateBandwidthForFile(file);

						if (++count === files.length) {
							_done();
						} else if (options && options.max && file.bandwidth >= options.max) {
							// no need to complete the bandwidth test as the bandwidth is higher than the specified max (threshold)
							_done();
						} else if (options && options.min && file.bandwidth < options.min) {
							// the download was too slow - don't continue the bandwidth test
							_done();
						} else {
							_download(files[count]);
						}
					}
				};

				xhr.open('GET', file.url + '?rand=' + Math.floor(Math.random()*100000000)); // append rand param to avoid caching locally
				file.startTime = +new Date; // in case onloadstart (above) isn't supported
				xhr.send();
			}

			// init serial download
			_download(files[0]);
		}
	};
	
	if ($) {
		$(window).on('load', function() {
			if (window.espn && espn.geo && espn.bandwidth && espn.track) {
				var cookieName = '_data-lite-mobile-bw',
					SAMPLE_PCT = 20;
				if (!$.cookie(cookieName) && ((Math.random()*100) <= SAMPLE_PCT)) {
					espn.geo.getLocation().done(function(geo) {
						if (geo.connection === 'mobile') {
							espn.bandwidth.test(function(bandwidth) {
								
								var ROUND_TO_NEAREST = 100,
								    rounded = ROUND_TO_NEAREST * Math.round(bandwidth/ROUND_TO_NEAREST);
								
								espn.track.userClicked = true;
								espn.track.trackLink({
									site: 'cricinfo',
									link: true,
									prop58: rounded
								});
								$.cookie(cookieName, true);
							});
						}
					});
				}
			}
		});
	}
})(window.espn, window.jQuery, window.navigator);
        </script>

</div>
</body>
</html>



```python
country_links=soup.find_all('a',href=re.compile('/icc-cricket-world-cup-2015/content/squad/'))
```


```python
conn=sqlite3.connect('Cricket.sqlite')
cur=conn.cursor()

cur.execute('''CREATE TABLE IF NOT EXISTS Countries
            (country_id INTEGER PRIMARY KEY,country TEXT)''')

cur.execute('''CREATE TABLE IF NOT EXISTS Players
            (country_id INTEGER,player_id INTEGER UNIQUE,player TEXT)''')

cur.execute('''CREATE TABLE IF NOT EXISTS Batting_Stats
                        (player TEXT,runs INTEGER,sixes INTEGER,fours INTEGER,ducks INTEGER,fifties INTEGER,hundreds INTEGER,balls_faced INTEGER,innings INTEGER)''')
cur.execute('''CREATE TABLE IF NOT EXISTS Bowling_Stats
                        (player TEXT,bowlinnings INTEGER ,overs INTEGER ,runsgiven INTEGER,maidens INTEGER,wickets INTEGER,fourw INTEGER,fivew INTEGER)''')
```




    <sqlite3.Cursor at 0x19309ee1b20>




```python
country_links
```




    [<a href="/icc-cricket-world-cup-2015/content/squad/814789.html" name="&amp;lpos=quicklink_Afghanistan">Afghanistan</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/819741.html" name="&amp;lpos=quicklink_Australia">Australia</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/816431.html" name="&amp;lpos=quicklink_Bangladesh">Bangladesh</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/812413.html" name="&amp;lpos=quicklink_England">England</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/817409.html" name="&amp;lpos=quicklink_India">India</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/816765.html" name="&amp;lpos=quicklink_Ireland">Ireland</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/818117.html" name="&amp;lpos=quicklink_New Zealand">New Zealand</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/817901.html" name="&amp;lpos=quicklink_Pakistan">Pakistan</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/818887.html" name="&amp;lpos=quicklink_Scotland">Scotland</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/817889.html" name="&amp;lpos=quicklink_South Africa">South Africa</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/817899.html" name="&amp;lpos=quicklink_Sri Lanka">Sri Lanka</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/819825.html" name="&amp;lpos=quicklink_United Arab Emirates">United Arab Emirates</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/819743.html" name="&amp;lpos=quicklink_West Indies">West Indies</a>,
     <a href="/icc-cricket-world-cup-2015/content/squad/817903.html" name="&amp;lpos=quicklink_Zimbabwe">Zimbabwe</a>]




```python
for i in country_links:
    country=i.text
    country_id=i.get('href').split('/')[-1].split('.')[0]
    cur.execute('INSERT OR IGNORE INTO Countries (country_id,country) VALUES (?,?)',(country_id,country))
```


```python
cur.execute('SELECT country_id FROM Countries')
countryid_list=list()
for row in cur:
    countryid_list.append(row[0])
```


```python
for cid in countryid_list:
    squad_url='http://www.espncricinfo.com/icc-cricket-world-cup-2015/content/squad/'+str(cid)+'.html'
    squad_page=requests.get(squad_url)
    soup1=BeautifulSoup(squad_page.text,"html.parser")
    player_links=soup1.findAll('a', href=re.compile('/icc-cricket-world-cup-2015/content/player/'))
    for i in player_links:
        if i.text:
            player_id=player_id=i.get('href').split('/')[-1].split('.')[0]
            player=i.text.strip()
            cur.execute('INSERT OR IGNORE INTO Players (country_id,player_id,player) VALUES (?,?,?)',(cid,player_id,player))
conn.commit()
```


```python
cur.execute('select a.country_id,a.country,b.player_id,b.player from Countries a,Players b where a.country_id=b.country_id')
play_list=list()
for row in cur:
    play_list.append(row)
play_action=['batting','bowling']
```


```python
year=2015
for action in play_action:
    for play in play_list:
        pid=play[2]
        country_name=play[1]
        player_name=play[3]
        stats_url='http://stats.espncricinfo.com/ci/engine/player/'+str(pid)+'.html?class=2;template=results;type='+str(action)+';view=innings;year='+str(year)
        stats_page=requests.get(stats_url)
        soup2=BeautifulSoup(stats_page.text,"html.parser")
        stats=soup2.findAll(text='filtered')[0].findParents('tr')[0].findAll("td")
        if action=="bowling":
            check=[3,4,6,5,7,12,13]
        else:
            check=[4,13,12,11,10,9,7,2]
        results=[]
        for i in check:
            try:
                results.append(float(stats[i].get_text()))
            except:
                results.append(0)
        if action=="batting":
            cur.execute('INSERT OR IGNORE INTO Batting_Stats (player,runs,sixes,fours,ducks,fifties,hundreds,balls_faced,innings)VALUES(?,?,?,?,?,?,?,?,?)',(player_name,results[0],results[1],results[2],results[3],results[4],results[5],results[6],results[7]))
        elif action=="bowling":
            cur.execute('INSERT OR IGNORE INTO Bowling_Stats (player,bowlinnings,overs,runsgiven,maidens,wickets,fourw,fivew)VALUES(?,?,?,?,?,?,?,?)',(player_name,results[0],results[1],results[2],results[3],results[4],results[5],results[6]))
conn.commit()
```


```python

```

    <td class="left">filtered</td>
    <td>4</td>
    <td>4</td>
    <td>0</td>
    <td>36</td>
    <td class="padAst">10</td>
    <td>9.00</td>
    <td>71</td>
    <td>50.70</td>
    <td>0</td>
    <td>0</td>
    <td>0</td>
    <td>3</td>
    <td>0</td>
    <td></td>
    


```python
conn.commit()
```
