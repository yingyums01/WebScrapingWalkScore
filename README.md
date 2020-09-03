{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# Method1: Use Selenium(webdriver)- work for Javasript"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## 1. install package"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "Requirement already satisfied: selenium in c:\\users\\yingy\\anaconda3\\lib\\site-packages (3.141.0)\n",
      "Requirement already satisfied: urllib3 in c:\\users\\yingy\\anaconda3\\lib\\site-packages (from selenium) (1.24.2)\n",
      "Note: you may need to restart the kernel to use updated packages.\n"
     ]
    }
   ],
   "source": [
    "pip install selenium"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [],
   "source": [
    "import requests"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 2. Use Driver and BeautifulSoup:\n",
    "\n",
    "Script ALL raw code on each page first, store it in a list temporarily, and clean it afterwards."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "#install\n",
    "from selenium import webdriver\n",
    "from selenium.common.exceptions import NoSuchElementException\n",
    "from selenium.webdriver.common.keys import Keys\n",
    "import time\n",
    "from bs4 import BeautifulSoup\n",
    "import re"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 3. Test, Extract, and Clean for ONE specific data\n",
    "Whether we could get the value on the page - ONE TRANSACTION ONE ZIP CODE ONLY"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'C:\\\\Users\\\\yingy\\\\OneDrive\\\\Desktop\\\\ND\\\\python\\\\web scraping\\\\walk project'"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import os\n",
    "os.getcwd()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "<html xmlns:fb=\"http://www.facebook.com/2008/fbml\" xmlns:og=\"http://ogp.me/ns#\"><head prefix=\"og: http://ogp.me/ns# fb: http://ogp.me/ns/fb# walk-score: http://ogp.me/ns/fb/walk-score#\"><script src=\"https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js\"></script><script src=\"https://pp.walk.sc/_/s/_p/listing/2d04fe87566d2f161ce8ece6237f36cc.js\"></script><script src=\"https://pp.walk.sc/_/s/_g/a06b42d0be6c33601fc0b16c97c9a777.js\"></script><script src=\"https://maps.googleapis.com/maps/api/js?callback=initialize&amp;libraries=geometry,places&amp;client=gme-redfin&amp;channel=walkscore-overview\"></script><script src=\"https://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js\"></script> <meta content=\"IE=edge,chrome=1\" http-equiv=\"X-UA-Compatible\"/> <meta content=\"width=device-width, initial-scale=1.0 user-scalable=yes\" name=\"viewport\"/> <meta content=\"text/html; charset=utf-8\" http-equiv=\"content-type\"/> <meta content=\"en\" http-equiv=\"content-language\"/> <meta content=\"4B461FBFB34199E256F083A963A8CE34\" name=\"msvalidate.01\"/> <link href=\"https://pp.walk.sc/_/s/_g/de157ed2fdf07e37c2d16618ab159a1f.css\" rel=\"stylesheet\"/> <style type=\"text/css\"> </style> <title>Higashimurayama Tokyo - Walk Score</title> <meta content=\"See the Walk Score of , Higashimurayama Tokyo. View map of nearby restaurants, parks, and schools. See photos of 189-0012.\" name=\"description\"/> <!--[if gte IE 9]> <style type=\"text/css\"> .gradient { filter: none; } </style> <![endif]--> <!--[if lt IE 9]> <style type=\"text/css\"> .visible-smalltablet, .visible-phone, .visible-smallphone, .go-to-sidebar { display: none !important; } .simple-place { display: block !important; } .img-shadow { float: none !important; } .magazine [class*=\"span\"] .img-shadow img { width: 100%; } .magazine [class*=\"span\"] .profile img { max-width: none; } </style> <![endif]--> <meta content=\"133264856724753\" property=\"fb:app_id\"/> <meta content=\"639335198,1009098,1356526099,506749889,733592108,100000351611452\" property=\"fb:admins\"/> <meta content=\"website\" property=\"og:type\"/> <meta content=\"Walk Score\" property=\"og:site_name\"/> <meta content=\"https://pp.walk.sc/_/s/_i/images/fb-walkscore-180.jpg\" property=\"og:image\"/> </head> <body class=\"pypage responsive magazine with-sidebar cards ab-MQM0GUKLSF2jaOjI09jEGw-0 loaded-with-sidebar\" onmouseover=\"\" ontouchstart=\"\"> <style type=\"text/css\"> .tile-promo { display: none; } #ws-smartbanner-b, a.map-enticement-link, .app-link-sms { display: none !important; } </style> <script>\n",
       "  (function() {\n",
       "    localStorage.removeItem('sb-skip-count'); \n",
       "\n",
       "    setInterval(function() {\n",
       "      document.body.classList.remove('iphone', 'android'); \n",
       "      if (document.body.style.position === \"fixed\") document.body.style.position = \"\"; \n",
       "    }, 100);\n",
       "  })();\n",
       "</script> <script src=\"//www.google-analytics.com/cx/api.js\"></script><script>!function(p,s,d,A,C,G,w,i,v,V){var t=w[A]=w[A]||{},a=cxApi,n=a.NO_CHOSEN_VARIATION,b=document.body,r=Math.random;i=\"MQM0GUKLSF2jaOjI09jEGw\";v=a[G](i);if(v==n){V=[0, 1];v=V[(r()*V.length)|0];}t[i]=v;b[C]+=s+[p,i,v].join(d);}('ab',' ','-','AB_VARIANTS','className','getChosenVariation',window)</script> <script>!function(l){try{l&&l.lead_gen_sms=='requested'&&(document.body.className+=' sms-requested')}catch(e){}}(window.localStorage)</script> <div id=\"fb-root\"></div> <div id=\"top\"> <div class=\"root-container\"> <div class=\"visible-phone\" id=\"phone-address-bar\"> <a class=\"logo\" data-ajax=\"false\" href=\"/\"> </a> <ul class=\"nav nav-pills\"> <li class=\"dropdown pull-left\"> <a class=\"dropdown-toggle\" data-toggle=\"dropdown\" href=\"#\"> </a> <ul class=\"dropdown-menu\"> <li><a data-ajax=\"false\" href=\"/\">Search</a></li> <li><a class=\"rentals\" data-ajax=\"false\" href=\"/apartments/\" id=\"mobile-nav-find-apartments\">Find Apartments</a></li> <li><a data-ajax=\"false\" href=\"/compare\" rel=\"nofollow\">My Favorites</a></li> </ul> </li> </ul> </div> <style> #phone-address-bar a.logo { top: 7px; width: 132px; height: 26px; background: url(\"https://pp.walk.sc/_/s/_i/images/ws-logo/walkscore-logo-132x26.png\") 0 0 no-repeat; } @media only screen and (-webkit-min-device-pixel-ratio: 1.5), only screen and (min-device-pixel-ratio:1.5){ #phone-address-bar a.logo { background: url(\"https://pp.walk.sc/_/s/_i/images/ws-logo/walkscore-logo-264x52.png\") 0 0 no-repeat; background-size: 132px 26px; -webkit-background-size: 132px 26px; -moz-background-size: 132px 26px; } } </style> <div class=\"hidden-phone\" id=\"respo-header\"> <div id=\"branding\"> <a aria-label=\"Walk Score Logo\" href=\"/\"><img alt=\"Walk Score Logo\" height=\"33\" src=\"https://pp.walk.sc/_/s/_i/images/walk-score-2-sm.png\" width=\"191\"/></a> </div> <div id=\"navigation\"> <a class=\"w-btn\" href=\"/cities-and-neighborhoods/\">Get Scores</a> <a class=\"w-btn\" href=\"/apartments/\" id=\"top-nav-find-apartments\">Find Apartments</a> <a class=\"w-btn\" href=\"/compare\" rel=\"nofollow\">My Favorites</a> <a class=\"w-btn\" href=\"/professional\" rel=\"nofollow\">Add to Your Site</a> </div> <style> #branding { height: 33px; } #branding img { width: 191px; height: 33px; } #respo-header #navigation { left: 214px; top: 21px; } </style> <div id=\"nav-links\"> <div class=\"menu-button deactivated emherit\" id=\"btn-login\"> <span class=\"avatar\"><img id=\"default-login-head\" src=\"https://pp.walk.sc/_/s/_i/images/search/login-head.png\" width=\"30\"/><img alt=\"Login default user image\" id=\"login-head\" src=\"https://pp.walk.sc/_/s/_i/images/search/login-head.png\"/></span><button class=\"label\" id=\"login-name\"><span class=\"name noselect\">Log in</span> <span aria-label=\"Menu\" class=\"toggle\"></span></button> <div class=\"shim\"></div> <div class=\"menu\" id=\"login-menu\"> <div id=\"logged-in\"> <p><a href=\"/compare\" id=\"my-faves-link\" rel=\"nofollow\"><strong>Favorites</strong></a></p> <p><a id=\"my-places-link\"><strong>Profile</strong></a></p> <p><button class=\"link\" id=\"ws-fb-logout\">Log out</button></p> </div> <div id=\"logged-out\"> <p>Log in to save favorites.</p> <p><button aria-label=\"Sign in with Facebook\" class=\"fb-login\" id=\"ws-fb-login\"></button> </p><p><button aria-label=\"Sign in with Google\" class=\"oid-login\" id=\"ws-oid-login\"></button> </p></div> </div> </div> </div> </div></div> <div id=\"address-bar\"> <div class=\"root-container\"> <div id=\"get-score-form\"> <div class=\"input-wrap\"> <form class=\"addrbar-query address oneline\" id=\"get-walkscore-form\" name=\"address-query\" onsubmit='document.location.href = \"/score/\" + encodeAddress($(\"#addrbar-street\").val()); return false;'> <div class=\"field-sizer with-geo\"> <a aria-label=\"Current Location\" class=\"b-btn light geolocate\" href=\"\" role=\"button\" style=\"display: block;\"><span class=\"icon\"></span></a> <input aria-autocomplete=\"list\" aria-haspopup=\"true\" autocomplete=\"off\" class=\"ui-autocomplete-input street example-text\" id=\"addrbar-street\" name=\"street\" placeholder=\"Type an address, neighborhood or city\" role=\"textbox\" type=\"text\" value=\"\"/> </div> <a class=\"b-btn go-btn\" href=\"#\" id=\"gs-address-go\" onclick=\"$('#get-walkscore-form').submit(); return false;\">Go</a> <div class=\"geolocation-api small-bullet\"><span class=\"icon bullet-target\"></span>Locate me</div> </form> </div> </div> <div id=\"address-bar-links\"> <button class=\"icons-share-button menu-button no-highlight\" id=\"share-button\"> <span class=\"ico\"></span><span class=\"label\">Share</span> <div id=\"share-menu\"></div> </button></div> </div> </div> </div> <div class=\"all-blocks\"> <div class=\"container-wrap\"><div class=\"container-fluid margins-phone\"> <div class=\"block-wrap block-float-bar no-card\" id=\"float-bar\"> <div class=\"float-bar hidden-phone hide\"> <div class=\"container-wrap\"> <div class=\"content\"> <p class=\"buttons\"> <a class=\"nearby-apts-btn b-btn light tall\" href=\"/apartments/search/loc/lat=35.743049/lng=139.478384\" onclick=\"trackEvent(ACTIVE_COMPONENT, 'apt search button', 'from float-bar')\" rel=\"nofollow\">Nearby Apartments</a><a class=\"b-btn light tall fave-btn not-ready\" data-eventsrc=\"float-bar fave btn\" onclick=\"return false\"><span class=\"icon fave\"></span><span class=\"text\">Favorite</span></a> </p> <p></p> </div> </div> </div> </div><div id=\"sidebar-anchor-0\"></div> <div class=\"block-wrap block-address-header\" id=\"address-header\"> <div> <div class=\"float-left-noncleared\"> <h1 class=\"tight-bot pad-bot-sm\">Somewhat Walkable<a class=\"score-info-link b-btn light super\" data-eventsrc=\"score page info icon\"><span class=\"icon question\"></span></a></h1> <p class=\"tight-top tight-bot clear-smallphone dull-link unstyled\">A location in Higashimurayama </p> </div> <p class=\"badges-link-upper-right float-right-noncleared badges-link medsmallfont small-pad-top\"><a href=\"/professional/badges.php?address= 189-0012\">Add scores to your site</a></p> <div class=\"clear-all\"> <div class=\"smallfont\" id=\"commutes-enticement-socket\"> </div> </div> </div> <div class=\"row-fluid clear-all\"> <div class=\"span12 align-left wrappy-btns pad-top\" id=\"detail_actions\"> <button class=\"b-btn light tall fave-btn not-ready mobile-50w first-50w\" data-eventsrc=\"header fave btn\" onclick=\"return false\"><span class=\"icon fave\"></span><span class=\"text\">Favorite</span></button> <a class=\"b-btn light tall mobile-50w fullscreen-map\" data-action=\"button click\" href=\"#\" id=\"fullscreen-map-btn\" role=\"button\"><span class=\"map icon\"></span>Map</a> <a class=\"address_nearby_apartments nearby-apts-btn b-btn light tall mobile-100w\" href=\"/apartments/search/loc/lat=35.743049/lng=139.478384\" onclick=\"trackEvent(ACTIVE_COMPONENT, 'apt search button', 'from header')\" rel=\"nofollow\"><span class=\"mag-glass icon\"></span>Nearby Apartments</a></div> </div> <div class=\"row-fluid clear-all\"> <div class=\"span12 group-of-2\"> <div class=\"clearfix score-div\"> <div class=\"block-header-badge score-info-link\" data-eventsrc=\"score page walk badge\"> <!--[if lte IE 8]><img src=\"//pp.walk.sc/badge/walk/score/60.png\" alt=\"60 Walk Score of this location\"><![endif]--> <!--[if gt IE 8]><img src=\"//pp.walk.sc/badge/walk/score/60.svg\" alt=\"60 Walk Score of this location\"><![endif]--> <!--[if !IE]> <!-- --><img alt=\"60 Walk Score of this location\" src=\"//pp.walk.sc/badge/walk/score/60.svg\"/><!-- <![endif]--> </div> <h5 class=\"tight-bot\">Somewhat Walkable</h5> <p class=\"med-tight-top tight-bot medsmallfont light\">Some errands can be accomplished on foot.</p> <p class=\"score-alert med-tight-top medsmallfont light\" data-country=\"Japan\"><span class=\"icon alert-ico mono\"></span>Unsupported Country</p> </div> <button class=\"score-info-link medsmallfont buttonLink\" data-eventsrc=\"score page score details link\">About your score</button> <p class=\"badges-link-lower-left badges-link medsmallfont\"><a href=\"/professional/badges.php?address= 189-0012\">Add scores to your site</a></p> </div> <div class=\"span12 hidden-phone small-pad-bot align-left-phone relative wide-map\"> <div class=\"clippy-frame titled-map\"> <a class=\"fullscreen-map\" data-action=\"map click\" href=\"#\" role=\"button\"><img alt=\"map of restaurants, bars, coffee shops, grocery stores, and more near in Higashimurayama\" class=\"clippy-inner address-header-static-tile\" src=\"//pp.walk.sc/tile/e/0/1496x1200/loc/lat=35.7430487/lng=139.4783835.png\" width=\"748\"/></a> <div class=\"map-house fullscreen-map\" data-action=\"marker click\" role=\"button\"></div> </div> </div> </div> <div aria-hidden=\"true\" aria-labelledby=\"myModalLabel\" class=\"modal hide fade fullscreen preserve thin-frame\" id=\"fullscreen-map\" role=\"dialog\" tabindex=\"-1\"> <div class=\"modal-header\"> <button aria-hidden=\"true\" class=\"close\" data-dismiss=\"modal\" type=\"button\">×</button> <h3 id=\"myModalLabel\">What's Nearby</h3> </div> <div class=\"modal-body\"> <div id=\"modal-fullscreen-map\"></div> <p class=\"float-right hidden-phone smallfont tight-bot r-indent3 hide add-place-text\">Something missing? <a class=\"add-place\">Add a place</a></p> </div> </div> </div> <div class=\"block-wrap block-mobile-static-map\" id=\"mobile-static-map\"> <div class=\"row-fluid\"> <div class=\"span12 small-pad-bot align-left-phone relative\"> <div class=\"clippy-frame titled-map\"> <a class=\"fullscreen-map\" data-action=\"map click\" href=\"#\" role=\"button\"><img alt=\"map of restaurants, bars, coffee shops, grocery stores, and more near in Higashimurayama\" class=\"clippy-inner address-header-static-tile\" src=\"//pp.walk.sc/tile/e/0/748x600/loc/lat=35.7430487/lng=139.4783835.png\" width=\"374\"/></a> <div class=\"map-house fullscreen-map\" data-action=\"marker click\" role=\"button\"></div> </div> </div> </div> </div> <div class=\"block-wrap block-text-ad no-card\" id=\"text-ad\"> <!-- AddressPage_ATF_1stText_600x15 --> <div class=\"extract-dfp-content\" id=\"div-gpt-ad-1403560076948-0\"></div> </div> <div class=\"block-wrap block-address-summary\" id=\"address-summary\"> <h2>About this Location</h2> <div class=\"row-fluid\"> <div class=\"span6\"> <div class=\"min-300 unbootstrap\" id=\"addr-streetview\"></div> </div> <div class=\"span6\" id=\"apt-description\"> <div id=\"loc-description\"> <div> <div class=\"microdata\" itemscope=\"\" itemtype=\"http://schema.org/Place\"> <div itemprop=\"address\" itemscope=\"\" itemtype=\"http://schema.org/PostalAddress\"> <div itemprop=\"streetAddress\"></div> <div itemprop=\"addressLocality\">Higashimurayama</div> <div itemprop=\"addressRegion\">Tokyo</div> <div itemprop=\"postalCode\">189-0012</div> </div> <div itemprop=\"geo\" itemscope=\"\" itemtype=\"http://schema.org/GeoCoordinates\"> <meta content=\"35.7430487\" itemprop=\"latitude\"/> <meta content=\"139.4783835\" itemprop=\"longitude\"/> </div> </div> <div class=\"content\"> <p> <span id=\"score-description-sentence\"> This location has a Walk Score of 60 out of 100. This location is Somewhat Walkable so some errands can be accomplished on foot. </span> </p> <p> Nearby parks include 萩山第1児童遊園, 萩山町2丁目第1仲よし広場 and 三角公園. </p> </div> </div> </div> </div> </div> </div> <div class=\"block-wrap block-text-ad no-card\" id=\"text-ad\"> <!-- AddressPage_ATF_2ndText_600x15 --> <div class=\"extract-dfp-content\" id=\"div-gpt-ad-1403560076948-1\"></div> </div><div id=\"sidebar-anchor-1\"></div> <div class=\"block-wrap block-getting-around\" id=\"getting-around\"> <div class=\"mag-block\"> <h2 class=\"float-left-noncleared\">Travel Time Map</h2> <p class=\"float-right-noncleared add-travel-time-link medsmallfont small-pad-top\"><a href=\"/professional/travel-time-js-api.php#widget-example\" onclick=\"trackEvent(ACTIVE_COMPONENT, 'add to your site: travel time')\">Add to your site</a></p> <p class=\"subtitle clear-all\"> Explore how far you can travel by car, bus, bike and foot from this location. </p> <div class=\"relative\"> <div class=\"map min-300\" id=\"map-getting-around\"></div> </div> </div> </div> <div class=\"block-wrap block-text-ad no-card\" id=\"text-ad\"> <!-- AddressPage_ATF_3rdText_600x15 --> <div class=\"extract-dfp-content\" id=\"div-gpt-ad-1403560076948-2\"></div> </div> <div class=\"block-wrap block-text-ad no-card\" id=\"text-ad\"> <!-- AddressPage_ATF_4thText_600x15 --> <div class=\"extract-dfp-content\" id=\"div-gpt-ad-1403560076948-3\"></div> </div><div id=\"sidebar-anchor-2\"></div> <div class=\"block-wrap block-responsive-ad\" id=\"responsive-ad\"> <div class=\"mag-block borderless overflow-banner\"> <style> .aptlisting-bottomleader-responsive { width: 320px; height: 50px; } @media(min-width: 500px) {.aptlisting-bottomleader-responsive { width: 468px; height: 60px; } } @media(min-width: 728px) {.aptlisting-bottomleader-responsive { width: 728px; height: 90px; } } </style> <ins class=\"adsbygoogle aptlisting-bottomleader-responsive\" data-ad-client=\"ca-pub-0760627883975840\" data-ad-slot=\"7489661938\" style=\"display:inline-block\"></ins> </div> </div> <div class=\"respo-sidebar-wrap\"> <div class=\"respo-sidebar clearfix\"> <div class=\"block-wrap block-address-ad no-card\" id=\"address-ad\"> <!-- AddressPage_Sidebar_HalfPage_300x600 --> <div class=\"go-to-sidebar\" data-sidebar-anchor=\"sidebar-anchor-0\" data-sidebar-weight=\"1\" id=\"div-gpt-ad-1404769932767-0\" style=\"width:300px; height:600px;\"></div> </div> </div> <div class=\"block-wrap block-nearby-and-faves\" id=\"nearby-and-faves\"> <div class=\"go-to-sidebar\" data-sidebar-anchor=\"sidebar-anchor-2\" data-sidebar-weight=\"5\" id=\"feat-apartments\"> <h6 class=\"indent20 small-pad-top tight-bot\">Nearby Apartments</h6> <div class=\"sidebar results-list large\" id=\"sidebar\"> <div class=\"sidebar_outer\" id=\"sidebar_outer\"> <div id=\"sidebar_list\"> </div> </div> </div> </div> </div> <div class=\"block-wrap block-responsive-ad\" id=\"responsive-ad\"> <div class=\"mag-block borderless go-to-sidebar ad2\" data-sidebar-anchor=\"sidebar-anchor-1\" data-sidebar-weight=\"10\"> <style> .aptlisting-bottomright-halfpage-responsive { width: 320px; height: 50px; } @media(min-width: 500px) {.aptlisting-bottomright-halfpage-responsive { width: 468px; height: 60px; } } @media(min-width: 728px) {.aptlisting-bottomright-halfpage-responsive { width: 728px; height: 90px; } } @media(min-width: 850px) {.aptlisting-bottomright-halfpage-responsive { width: 300px; height: 250px; } } </style> <ins class=\"adsbygoogle aptlisting-bottomright-halfpage-responsive\" data-ad-client=\"ca-pub-0760627883975840\" data-ad-slot=\"6012928739\" style=\"display:inline-block\"></ins> </div> </div> </div> </div> </div></div> <script>window.initialize = function(){ window._goodToGo = true }</script> <script src=\"https://pp.walk.sc/_/s/_g/746b7be055773d3812ba191babd77ac7.js\"></script> <script>$LAB.setGlobalDefaults({AllowDuplicates:false});</script> <!--[if IE]><script>$LAB.setGlobalDefaults({AlwaysPreserveOrder:true});</script><![endif]--> <div id=\"footer\"><div id=\"city-footer\"> <div class=\"container-fluid margins-phone\"> <div class=\"root-container clearfix row-fluid\"> <div class=\"span6 clearfix\"> <div class=\"clear-all\"> <div class=\"section clear-all with-social\"> <h5><a href=\"/\">Walk Score</a></h5> <div class=\"social-buttons\" id=\"social-media-buttons\"> <button aria-label=\"Twitter\" class=\"s-btn friend-twitter\" onclick='trackNavigationNewWindow(\"http://twitter.com/walkscore\", ACTIVE_COMPONENT, \"follow us\", \"twitter\")'> </button> <button aria-label=\"Facebook\" class=\"s-btn friend-facebook\" onclick='trackNavigationNewWindow(\"http://www.facebook.com/walkscore\", ACTIVE_COMPONENT, \"follow us\", \"facebook\")'> </button> <button aria-label=\"Email\" class=\"s-btn friend-email-list\" onclick='trackNavigation(\"/how-it-works/#join-list\", ACTIVE_COMPONENT, \"follow us\", \"email list\")'> </button><div class=\"s-btn friend-google\" onclick='trackNavigationNewWindow(\"http://plus.google.com/+walkscore/\", ACTIVE_COMPONENT, \"follow us\", \"google plus\")'> </div></div> <ul> <li><a href=\"http://blog.walkscore.com/\">Blog</a></li> <li><a href=\"/about.shtml\">About</a></li> <li><a href=\"/how-it-works/\">How It Works</a></li> <li><a href=\"/press/\">Press</a></li> <li><a href=\"/terms-of-use.shtml\" rel=\"nofollow\">Terms &amp; Privacy</a></li> <li><div style=\"visibility:visible;\"><a href=\"/contact\" rel=\"nofollow\" target=\"_blank\">Feedback</a></div></li> </ul> </div> <div class=\"section wide\"> <h5><a href=\"/professional/\">Professional</a></h5> <ul> <li><a href=\"/professional/walk-score-widget.php\">Walk Score Widget</a></li> <li><a href=\"/professional/walk-score-apis.php\">Walk Score APIs</a></li> <li><a href=\"/professional/research.php\">Data Services</a></li> <li><a href=\"/professional/real-estate-professionals.php\">Real Estate Professionals</a></li> <li><a href=\"/professional/walkability-research.php\">Walkability Research</a></li> <li><a href=\"/professional/badges.php\">Badges</a></li> </ul> </div> </div> <p class=\"accessibility-contact\">If you are using a screen reader or having trouble reading this website, please call Walk Score customer service at (253) 256-1634.</p> <p class=\"credit\"><br/>© 2020 Walk Score</p> </div> </div> <noscript> <img src=\"http://b.scorecardresearch.com/p?c1=2&amp;c2=15053602&amp;cv=2.0&amp;cj=1\"> </img></noscript></div> </div> <script>(function(){var start=function(){(window.wLAB = $LAB).wait(function(){window.ACTIVE_COMPONENT = \"overview\";window.isMobile = false;}).script(\"https://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js\").wait(function(){window.trueWinWidth = function(){return Math.max($(window).outerWidth(true), window.outerWidth);}}).wait(function(){;(function() {'use strict';$('body').addClass('loaded-' + (trueWinWidth()>=850 ? \"with\":\"without\") + '-sidebar');var sidebarBlocks = $('.go-to-sidebar').toArray();var anchors = [];if (sidebarBlocks.length) {var sortByWeight = function(a, b){var aWeight = Number($(a).attr('data-sidebar-weight') || 10);var bWeight = Number($(b).attr('data-sidebar-weight') || 10);return ((aWeight < bWeight) ? -1 : ((aWeight > bWeight) ? 1 : 0));};sidebarBlocks.sort(sortByWeight);$.each(sidebarBlocks, function(i,e) {anchors.push($('#'+$(e).attr('data-sidebar-anchor')));});var sidebar = $('.respo-sidebar');window.isShowingSidebar = true;var alertSidebarShown = function(){if (window.isShowingSidebar){$.event.trigger(\"sidebarShown\");}};var resizeFunc = function(){var showSidebar = (trueWinWidth() >= 850);if (showSidebar != window.isShowingSidebar){window.isShowingSidebar = showSidebar;if (showSidebar){$.each(sidebarBlocks, function(i,e) {sidebar.append($(e).parent());});alertSidebarShown();}else {$.each(sidebarBlocks, function(i,e) {anchors[i].after($(e).parent());});}}};alertSidebarShown();$(window).resize(resizeFunc);resizeFunc();}})();}).script(\"https://maps.googleapis.com/maps/api/js?callback=initialize&libraries=geometry,places&client=gme-redfin&channel=walkscore-overview\").script(\"https://pp.walk.sc/_/s/_g/a06b42d0be6c33601fc0b16c97c9a777.js\").script(\"https://pp.walk.sc/_/s/_p/listing/2d04fe87566d2f161ce8ece6237f36cc.js\");$LAB.script(\"//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js\")};if(window.addEventListener)window.addEventListener(\"load\",start,false);else window.attachEvent(\"onload\",start);})()</script> </div></body></html>"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#open driver\n",
    "driver = webdriver.Firefox()\n",
    "driver.get(\"https://www.walkscore.com/score/1890012\")\n",
    "\n",
    "#change window size to show whole content\n",
    "driver.set_window_size(1280, 796)\n",
    "\n",
    "#find element by class name\n",
    "driver.find_element_by_css_selector('.score-info-link, .unstyled')\n",
    "\n",
    "#the context was in driver, store it in soup\n",
    "soup = BeautifulSoup(driver.page_source,'html.parser')\n",
    "\n",
    "#close drive\n",
    "driver.close()\n",
    "\n",
    "soup"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'Higashimurayama '"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Get loaction name\n",
    "location = soup.find('p',{'class':\"tight-top tight-bot clear-smallphone dull-link unstyled\"}).get_text().split(\"in \")[1]\n",
    "location"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[<img alt=\"Walk Score Logo\" height=\"33\" src=\"https://pp.walk.sc/_/s/_i/images/walk-score-2-sm.png\" width=\"191\"/>,\n",
       " <img alt=\"Login default user image\" id=\"login-head\" src=\"https://pp.walk.sc/_/s/_i/images/search/login-head.png\"/>,\n",
       " <img alt=\"60 Walk Score of this location\" src=\"//pp.walk.sc/badge/walk/score/60.svg\"/>,\n",
       " <img alt=\"map of restaurants, bars, coffee shops, grocery stores, and more near in Higashimurayama\" class=\"clippy-inner address-header-static-tile\" src=\"//pp.walk.sc/tile/e/0/1496x1200/loc/lat=35.7430487/lng=139.4783835.png\" width=\"748\"/>,\n",
       " <img alt=\"map of restaurants, bars, coffee shops, grocery stores, and more near in Higashimurayama\" class=\"clippy-inner address-header-static-tile\" src=\"//pp.walk.sc/tile/e/0/748x600/loc/lat=35.7430487/lng=139.4783835.png\" width=\"374\"/>]"
      ]
     },
     "execution_count": 8,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Get Walk Score in the IMG src/alt\n",
    "all_image_with_alt = soup.findAll('img', alt=True)\n",
    "all_image_with_alt\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'60'"
      ]
     },
     "execution_count": 9,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# get the string from string after \"src=\"\n",
    "# then get split by \"score/\"\n",
    "walk_score = all_image_with_alt[2]['src'].split(\"score/\")[1].replace(\".svg\",\"\")\n",
    "walk_score"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 4. Bulk WebScraping\n",
    "After assure all the code and clean up the content extracted. Apply the code to multiple iteration"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0       https://www.walkscore.com/score/1010032\n",
       "1       https://www.walkscore.com/score/1890012\n",
       "2       https://www.walkscore.com/score/9840054\n",
       "3       https://www.walkscore.com/score/1320024\n",
       "4       https://www.walkscore.com/score/9980111\n",
       "5       https://www.walkscore.com/score/1820024\n",
       "6       https://www.walkscore.com/score/1860004\n",
       "7       https://www.walkscore.com/score/7910112\n",
       "8       https://www.walkscore.com/score/3111313\n",
       "9       https://www.walkscore.com/score/1780063\n",
       "10      https://www.walkscore.com/score/5300003\n",
       "11      https://www.walkscore.com/score/3790114\n",
       "12      https://www.walkscore.com/score/4360342\n",
       "13      https://www.walkscore.com/score/1230864\n",
       "14      https://www.walkscore.com/score/9480012\n",
       "15      https://www.walkscore.com/score/1160001\n",
       "16      https://www.walkscore.com/score/3490101\n",
       "17      https://www.walkscore.com/score/4480803\n",
       "18      https://www.walkscore.com/score/5640062\n",
       "19      https://www.walkscore.com/score/6600064\n",
       "20      https://www.walkscore.com/score/1006611\n",
       "21      https://www.walkscore.com/score/7392626\n",
       "22      https://www.walkscore.com/score/1010051\n",
       "23      https://www.walkscore.com/score/2310005\n",
       "24      https://www.walkscore.com/score/5150044\n",
       "25      https://www.walkscore.com/score/7200841\n",
       "26      https://www.walkscore.com/score/4600002\n",
       "27      https://www.walkscore.com/score/1660013\n",
       "28      https://www.walkscore.com/score/1028573\n",
       "29      https://www.walkscore.com/score/4000306\n",
       "                         ...                   \n",
       "1887    https://www.walkscore.com/score/6650033\n",
       "1888    https://www.walkscore.com/score/3792217\n",
       "1889    https://www.walkscore.com/score/1000002\n",
       "1890    https://www.walkscore.com/score/7993101\n",
       "1891    https://www.walkscore.com/score/1050001\n",
       "1892    https://www.walkscore.com/score/7993101\n",
       "1893    https://www.walkscore.com/score/9451117\n",
       "1894    https://www.walkscore.com/score/2710052\n",
       "1895    https://www.walkscore.com/score/1700011\n",
       "1896    https://www.walkscore.com/score/1680063\n",
       "1897    https://www.walkscore.com/score/1440035\n",
       "1898    https://www.walkscore.com/score/3510011\n",
       "1899    https://www.walkscore.com/score/7900934\n",
       "1900    https://www.walkscore.com/score/1460085\n",
       "1901    https://www.walkscore.com/score/5580023\n",
       "1902    https://www.walkscore.com/score/1410031\n",
       "1903    https://www.walkscore.com/score/8000007\n",
       "1904    https://www.walkscore.com/score/5310074\n",
       "1905    https://www.walkscore.com/score/9011304\n",
       "1906    https://www.walkscore.com/score/5610828\n",
       "1907    https://www.walkscore.com/score/9320042\n",
       "1908    https://www.walkscore.com/score/1620816\n",
       "1909    https://www.walkscore.com/score/8891411\n",
       "1910    https://www.walkscore.com/score/2728560\n",
       "1911    https://www.walkscore.com/score/7000024\n",
       "1912    https://www.walkscore.com/score/1730016\n",
       "1913    https://www.walkscore.com/score/2750072\n",
       "1914    https://www.walkscore.com/score/6800053\n",
       "1915    https://www.walkscore.com/score/4110034\n",
       "1916    https://www.walkscore.com/score/1770034\n",
       "Name: Postcode, Length: 1917, dtype: object"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "import pandas as pd\n",
    "df = pd.read_excel(\"Walk Score_workplace postcode.xlsx\")\n",
    "\n",
    "# Paste url string before all postcode element in the series\n",
    "postcode_url = \"https://www.walkscore.com/score/\" + df[\"Postcode\"].astype(\"str\")\n",
    "\n",
    "#Get all url\n",
    "postcode_url"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 92,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "'https://www.walkscore.com/score/9000004'"
      ]
     },
     "execution_count": 92,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "postcode_url[252]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [],
   "source": [
    "soup_list=[]\n",
    "\n",
    "k=0"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "1\n",
      "2\n",
      "3\n"
     ]
    }
   ],
   "source": [
    "import numpy as np\n",
    "#open driver\n",
    "\n",
    "for link in postcode_url[:3]:\n",
    "    # Open WebDriver\n",
    "    driver = webdriver.Firefox()\n",
    "    driver.get(link)\n",
    "\n",
    "    #change window size to show whole content\n",
    "    driver.set_window_size(1280, 796)\n",
    "    \n",
    "    try: \n",
    "        #find element by class name\n",
    "        driver.find_element_by_css_selector('.score-info-link, .unstyled')\n",
    "    \n",
    "        #the context was in driver, store it in soup\n",
    "        soup = BeautifulSoup(driver.page_source,'html.parser')\n",
    "\n",
    "    except:\n",
    "        soup = 0\n",
    "    soup_list.append(soup)\n",
    "    #close drive\n",
    "    driver.close()\n",
    "    soup_len = len(soup_list)\n",
    "    print(soup_len)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 108,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1917"
      ]
     },
     "execution_count": 108,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(soup_list)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 106,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "[2, 3]"
      ]
     },
     "execution_count": 106,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "test=[0,1,2,3]\n",
    "len(test)\n",
    "test[2:]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 103,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1640"
      ]
     },
     "execution_count": 103,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(soup_list)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 5. Clean the HTML code & Extract Value"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 115,
   "metadata": {},
   "outputs": [],
   "source": [
    "location_list = []\n",
    "walk_score_list = []"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 116,
   "metadata": {},
   "outputs": [],
   "source": [
    "for soup in soup_list:\n",
    "    # 1. Page Found\n",
    "    if soup !=0:\n",
    "        # Get loaction name\n",
    "        try:\n",
    "            location = soup.find('p',{'class':\"tight-top tight-bot clear-smallphone dull-link unstyled\"}).get_text().split(\"in \")[1]\n",
    "        except:\n",
    "            location = \"index error\"\n",
    "        try:\n",
    "            # Get Walk Score in the IMG src/alt\n",
    "            all_image_with_alt = soup.findAll('img', alt=True)\n",
    "\n",
    "            # get the string from string after \"src=\"\n",
    "            # then get split by \"score/\"\n",
    "            walk_score = all_image_with_alt[2]['src'].split(\"score/\")[1].replace(\".svg\",\"\")\n",
    "        except:\n",
    "            walk_score = \"index error\"\n",
    "        \n",
    "    \n",
    "    # 2. Page not Found\n",
    "    else:\n",
    "        location = \"page not found\"\n",
    "        walk_score = np.nan\n",
    "        \n",
    "    location_list.append(location)\n",
    "    walk_score_list.append(walk_score)\n",
    "    \n",
    "    "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 117,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1917"
      ]
     },
     "execution_count": 117,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(location_list)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 118,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "1917"
      ]
     },
     "execution_count": 118,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "len(walk_score_list)"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "# 6. Convert Data type to final DF format"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 119,
   "metadata": {},
   "outputs": [],
   "source": [
    "location_Series = pd.Series(location_list)\n",
    "walk_score_Series = pd.Series(walk_score_list)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 120,
   "metadata": {
    "scrolled": true
   },
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Postcode</th>\n",
       "      <th>0</th>\n",
       "      <th>1</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>0</th>\n",
       "      <td>1010032</td>\n",
       "      <td>Chiyoda City</td>\n",
       "      <td>99</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1</th>\n",
       "      <td>1890012</td>\n",
       "      <td>Higashimurayama</td>\n",
       "      <td>60</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>2</th>\n",
       "      <td>9840054</td>\n",
       "      <td>Sendai</td>\n",
       "      <td>76</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>3</th>\n",
       "      <td>1320024</td>\n",
       "      <td>Edogawa City</td>\n",
       "      <td>86</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>4</th>\n",
       "      <td>9980111</td>\n",
       "      <td>Sakata</td>\n",
       "      <td>33</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>5</th>\n",
       "      <td>1820024</td>\n",
       "      <td>Chofu</td>\n",
       "      <td>88</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>6</th>\n",
       "      <td>1860004</td>\n",
       "      <td>Kunitachi</td>\n",
       "      <td>89</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>7</th>\n",
       "      <td>7910112</td>\n",
       "      <td>Matsuyama</td>\n",
       "      <td>38</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>8</th>\n",
       "      <td>3111313</td>\n",
       "      <td>Oarai</td>\n",
       "      <td>0</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>9</th>\n",
       "      <td>1780063</td>\n",
       "      <td>Nerima City</td>\n",
       "      <td>94</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>10</th>\n",
       "      <td>5300003</td>\n",
       "      <td>Osaka</td>\n",
       "      <td>100</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>11</th>\n",
       "      <td>3790114</td>\n",
       "      <td>Annaka</td>\n",
       "      <td>16</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>12</th>\n",
       "      <td>4360342</td>\n",
       "      <td>Kakegawa</td>\n",
       "      <td>1</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>13</th>\n",
       "      <td>1230864</td>\n",
       "      <td>Adachi City</td>\n",
       "      <td>71</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>14</th>\n",
       "      <td>9480012</td>\n",
       "      <td>Tokamachi</td>\n",
       "      <td>63</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>15</th>\n",
       "      <td>1160001</td>\n",
       "      <td>Arakawa City</td>\n",
       "      <td>90</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>16</th>\n",
       "      <td>3490101</td>\n",
       "      <td>Hasuda</td>\n",
       "      <td>66</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>17</th>\n",
       "      <td>4480803</td>\n",
       "      <td>Kariya</td>\n",
       "      <td>64</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>18</th>\n",
       "      <td>5640062</td>\n",
       "      <td>Suita</td>\n",
       "      <td>95</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>19</th>\n",
       "      <td>6600064</td>\n",
       "      <td>Amagasaki</td>\n",
       "      <td>75</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>20</th>\n",
       "      <td>1006611</td>\n",
       "      <td>page not found</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>21</th>\n",
       "      <td>7392626</td>\n",
       "      <td>Higashihiroshima</td>\n",
       "      <td>5</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>22</th>\n",
       "      <td>1010051</td>\n",
       "      <td>Chiyoda City</td>\n",
       "      <td>100</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>23</th>\n",
       "      <td>2310005</td>\n",
       "      <td>Yokohama</td>\n",
       "      <td>100</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>24</th>\n",
       "      <td>5150044</td>\n",
       "      <td>Matsusaka</td>\n",
       "      <td>73</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25</th>\n",
       "      <td>7200841</td>\n",
       "      <td>Fukuyama</td>\n",
       "      <td>8</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>26</th>\n",
       "      <td>4600002</td>\n",
       "      <td>Nagoya</td>\n",
       "      <td>98</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>27</th>\n",
       "      <td>1660013</td>\n",
       "      <td>Suginami City</td>\n",
       "      <td>74</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>28</th>\n",
       "      <td>1028573</td>\n",
       "      <td>page not found</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>29</th>\n",
       "      <td>4000306</td>\n",
       "      <td>Minami</td>\n",
       "      <td>81</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>...</th>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "      <td>...</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1887</th>\n",
       "      <td>6650033</td>\n",
       "      <td>Takarazuka</td>\n",
       "      <td>16</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1888</th>\n",
       "      <td>3792217</td>\n",
       "      <td>Isesaki</td>\n",
       "      <td>29</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1889</th>\n",
       "      <td>1000002</td>\n",
       "      <td>page not found</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1890</th>\n",
       "      <td>7993101</td>\n",
       "      <td>Iyo</td>\n",
       "      <td>31</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1891</th>\n",
       "      <td>1050001</td>\n",
       "      <td>page not found</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1892</th>\n",
       "      <td>7993101</td>\n",
       "      <td>Iyo</td>\n",
       "      <td>31</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1893</th>\n",
       "      <td>9451117</td>\n",
       "      <td>Kashiwazaki</td>\n",
       "      <td>13</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1894</th>\n",
       "      <td>2710052</td>\n",
       "      <td>Matsudo</td>\n",
       "      <td>79</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1895</th>\n",
       "      <td>1700011</td>\n",
       "      <td>Toshima City</td>\n",
       "      <td>95</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1896</th>\n",
       "      <td>1680063</td>\n",
       "      <td>Suginami City</td>\n",
       "      <td>85</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1897</th>\n",
       "      <td>1440035</td>\n",
       "      <td>Ota City</td>\n",
       "      <td>77</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1898</th>\n",
       "      <td>3510011</td>\n",
       "      <td>Asaka</td>\n",
       "      <td>94</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1899</th>\n",
       "      <td>7900934</td>\n",
       "      <td>Matsuyama</td>\n",
       "      <td>78</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1900</th>\n",
       "      <td>1460085</td>\n",
       "      <td>Ota City</td>\n",
       "      <td>74</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1901</th>\n",
       "      <td>5580023</td>\n",
       "      <td>Osaka</td>\n",
       "      <td>91</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1902</th>\n",
       "      <td>1410031</td>\n",
       "      <td>Shinagawa City</td>\n",
       "      <td>97</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1903</th>\n",
       "      <td>8000007</td>\n",
       "      <td>page not found</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1904</th>\n",
       "      <td>5310074</td>\n",
       "      <td>Osaka</td>\n",
       "      <td>97</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1905</th>\n",
       "      <td>9011304</td>\n",
       "      <td>Yonabaru</td>\n",
       "      <td>69</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1906</th>\n",
       "      <td>5610828</td>\n",
       "      <td>Toyonaka</td>\n",
       "      <td>85</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1907</th>\n",
       "      <td>9320042</td>\n",
       "      <td>Oyabe</td>\n",
       "      <td>64</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1908</th>\n",
       "      <td>1620816</td>\n",
       "      <td>Shinjuku City</td>\n",
       "      <td>97</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1909</th>\n",
       "      <td>8891411</td>\n",
       "      <td>page not found</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1910</th>\n",
       "      <td>2728560</td>\n",
       "      <td>page not found</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1911</th>\n",
       "      <td>7000024</td>\n",
       "      <td>Okayama</td>\n",
       "      <td>98</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1912</th>\n",
       "      <td>1730016</td>\n",
       "      <td>Itabashi City</td>\n",
       "      <td>88</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1913</th>\n",
       "      <td>2750072</td>\n",
       "      <td>page not found</td>\n",
       "      <td>NaN</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1914</th>\n",
       "      <td>6800053</td>\n",
       "      <td>Tottori</td>\n",
       "      <td>97</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1915</th>\n",
       "      <td>4110034</td>\n",
       "      <td>Mishima</td>\n",
       "      <td>85</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>1916</th>\n",
       "      <td>1770034</td>\n",
       "      <td>Nerima City</td>\n",
       "      <td>77</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "<p>1917 rows × 3 columns</p>\n",
       "</div>"
      ],
      "text/plain": [
       "      Postcode                  0    1\n",
       "0      1010032      Chiyoda City    99\n",
       "1      1890012   Higashimurayama    60\n",
       "2      9840054            Sendai    76\n",
       "3      1320024      Edogawa City    86\n",
       "4      9980111            Sakata    33\n",
       "5      1820024             Chofu    88\n",
       "6      1860004         Kunitachi    89\n",
       "7      7910112         Matsuyama    38\n",
       "8      3111313             Oarai     0\n",
       "9      1780063       Nerima City    94\n",
       "10     5300003             Osaka   100\n",
       "11     3790114            Annaka    16\n",
       "12     4360342          Kakegawa     1\n",
       "13     1230864       Adachi City    71\n",
       "14     9480012         Tokamachi    63\n",
       "15     1160001      Arakawa City    90\n",
       "16     3490101            Hasuda    66\n",
       "17     4480803            Kariya    64\n",
       "18     5640062             Suita    95\n",
       "19     6600064         Amagasaki    75\n",
       "20     1006611     page not found  NaN\n",
       "21     7392626  Higashihiroshima     5\n",
       "22     1010051      Chiyoda City   100\n",
       "23     2310005          Yokohama   100\n",
       "24     5150044         Matsusaka    73\n",
       "25     7200841          Fukuyama     8\n",
       "26     4600002            Nagoya    98\n",
       "27     1660013     Suginami City    74\n",
       "28     1028573     page not found  NaN\n",
       "29     4000306            Minami    81\n",
       "...        ...                ...  ...\n",
       "1887   6650033        Takarazuka    16\n",
       "1888   3792217           Isesaki    29\n",
       "1889   1000002     page not found  NaN\n",
       "1890   7993101               Iyo    31\n",
       "1891   1050001     page not found  NaN\n",
       "1892   7993101               Iyo    31\n",
       "1893   9451117       Kashiwazaki    13\n",
       "1894   2710052           Matsudo    79\n",
       "1895   1700011      Toshima City    95\n",
       "1896   1680063     Suginami City    85\n",
       "1897   1440035          Ota City    77\n",
       "1898   3510011             Asaka    94\n",
       "1899   7900934         Matsuyama    78\n",
       "1900   1460085          Ota City    74\n",
       "1901   5580023             Osaka    91\n",
       "1902   1410031    Shinagawa City    97\n",
       "1903   8000007     page not found  NaN\n",
       "1904   5310074             Osaka    97\n",
       "1905   9011304          Yonabaru    69\n",
       "1906   5610828          Toyonaka    85\n",
       "1907   9320042             Oyabe    64\n",
       "1908   1620816     Shinjuku City    97\n",
       "1909   8891411     page not found  NaN\n",
       "1910   2728560     page not found  NaN\n",
       "1911   7000024           Okayama    98\n",
       "1912   1730016     Itabashi City    88\n",
       "1913   2750072     page not found  NaN\n",
       "1914   6800053           Tottori    97\n",
       "1915   4110034           Mishima    85\n",
       "1916   1770034       Nerima City    77\n",
       "\n",
       "[1917 rows x 3 columns]"
      ]
     },
     "execution_count": 120,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "series_list = [df[\"Postcode\"],location_Series,walk_score_Series]\n",
    "result_df = pd.concat(series_list,axis=1)\n",
    "result_df"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 122,
   "metadata": {},
   "outputs": [],
   "source": [
    "result_df.to_excel(\"walkscore_final.xlsx\",index=False)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.3"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
