---
layout: post
title: An ASP.NET Picasa Image Gallery
date: '2012-10-01T10:07:00.000+05:30'
author: aks
comments: true
tags: [Image Gallery, .Net, ASP.NET, Picasa]
modified_time: '2012-10-01T11:09:22.105+05:30'
thumbnail: http://2.bp.blogspot.com/-9pk01508_C8/UGksZI2I-wI/AAAAAAAAFxg/wzLnkv7KZ08/s72-c/gallery.PNG
blogger_id: tag:blogger.com,1999:blog-7475258030496424805.post-4367423925819788156
blogger_orig_url: http://techyfreak.blogspot.com/2012/10/an-aspnet-picasa-image-gallery.html
---

Few days back I was thinking of creating an Image Gallery of the collection of photos I have. Although, there are multiple options available over the internet that you can download and get ready on the go; most of them involves saving the images on your own server. But what I was more concerned was to just have a display only image in my web site to showcase my photos to the word. And, I wanted to make use of one of my social network account – Facebook, Google+, Twitter or Flickr to host the images. 


All the major social network sites provide API to get the photos from an album. First, I tried to make use of Facebook, but it’s access token gets expired in few hours and you need to get a new token. This normally involves a user to login to your application first, but I wanted a permanent access token or a way to refresh the access token without user’s intervention. I think it is possible to achieve it somehow, I tried my hand on Picasa (Google+) and it was much easier. 


Using [Picasa Web Albums Data API](https://developers.google.com/picasa-web/), you can query any public album and get the photos. You need two things for this – Album Id and User Name. In case you need to show photos from one of your private albums, you need to authenticate yourself using the API with your Google account credentials. 


Let’s go step by step and see how to achieve this. To achieve this I am using [GoogleData API for .NET](https://code.google.com/p/google-gdata/downloads/list) and [Galleriffic](http://www.twospy.com/galleriffic/) jquery plugin for the image library.

## Step 1: Design your gallery

First step is to create your image gallery base. I am using the [Galleriffic](http://www.twospy.com/galleriffic/) jquery plugin and for this there should be few style sheet files (css) and javascript files need to be included in the project.

First, go to this link [http://www.twospy.com/galleriffic/](http://www.twospy.com/galleriffic/) and download the plugin from there. You should copy the css and js folders into your ASP.Net website/application project. Above link showcases 5 examples and I am using the second one [Thumbnail rollover effects and slideshow crossfades](http://www.twospy.com/galleriffic/example-2.html), as it is closest to the image gallery looks and feel I wanted.

CSS file used for example 2 is galleriffic-2.css. You can exclude other css files named such as galleriffic-1.css, galleriffic-3.css etc, but you will need other remaining css and image files.

After you are done with including css and js folders, next step is to include them in you page. In this example, I am using Default.aspx to be my image gallery page, but it can be anything for you such as Gallery.aspx. In the head tag include the following tags:

```

<link rel="stylesheet" href="css/basic.css" type="text/css" />
    <link rel="stylesheet" href="css/galleriffic-2.css" type="text/css" />
    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.8.0/jquery.min.js" type="text/javascript"></script>
    <script type="text/javascript" src="js/jquery.galleriffic.js"></script>
    <script type="text/javascript" src="js/jquery.opacityrollover.js"></script>
   
    <script type="text/javascript">
        document.write('');
    </script>

```

Next, include the following script in your page:


```

    <script type="text/javascript">
        jQuery(document).ready(function ($) {
            // We only want these styles applied when javascript is enabled
            $('div.navigation').css({ 'width': '300px', 'float': 'left' });
            $('div.content').css('display', 'block');

            // Initially set opacity on thumbs and add
            // additional styling for hover effect on thumbs
            var onMouseOutOpacity = 0.67;
            $('#thumbs ul.thumbs li').opacityrollover({
                mouseOutOpacity: onMouseOutOpacity,
                mouseOverOpacity: 1.0,
                fadeSpeed: 'fast',
                exemptionSelector: '.selected'
            });

            // Initialize Advanced Galleriffic Gallery
            var gallery = $('#thumbs').galleriffic({
                delay: 2500,
                numThumbs: 15,
                preloadAhead: 10,
                enableTopPager: true,
                enableBottomPager: true,
                maxPagesToShow: 7,
                imageContainerSel: '#slideshow',
                controlsContainerSel: '#controls',
                captionContainerSel: '#caption',
                loadingContainerSel: '#loading',
                renderSSControls: true,
                renderNavControls: true,
                playLinkText: 'Play Slideshow',
                pauseLinkText: 'Pause Slideshow',
                prevLinkText: '‹ Previous Photo',
                nextLinkText: 'Next Photo ›',
                nextPageLinkText: 'Next ›',
                prevPageLinkText: '‹ Prev',
                enableHistory: false,
                autoStart: false,
                syncTransitions: true,
                defaultTransitionDuration: 900,
                onSlideChange: function (prevIndex, nextIndex) {
                    // 'this' refers to the gallery, which is an extension of $('#thumbs')
                    this.find('ul.thumbs').children()
                                                .eq(prevIndex).fadeTo('fast', onMouseOutOpacity).end()
                                                .eq(nextIndex).fadeTo('fast', 1.0);
                },
                onPageTransitionOut: function (callback) {
                    this.fadeTo('fast', 0.0, callback);
                },
                onPageTransitionIn: function () {
                    this.fadeTo('fast', 1.0);
                }
            });
        });
    </script>

```


Next step is to have necessary div tags as detailed in the documentation for Galleriffic (http://www.twospy.com/galleriffic/). The galleriffic plugin shows each image’s thumbnail as a list item in an HTML unordered list (ul tag). As we will be getting our images at run time only via call to Picasa API we need to have the li items generated from within code. Therefore, I have added a div (divSlider) and marked it to runat=”server” so that I can assign a value to its innerHTML with constructed html.


Within you form tag in your aspx include following:

```

  <div id="page">
      <div id="container">
          <h1>
              <a href="#">My Website</a></h1>
          <h2>
              Gallery</h2>
         
          <div id="gallery" class="content">
              <div id="controls" class="controls">
              </div>
              <div class="slideshow-container">
                  <div id="loading" class="loader">
                  </div>
                  <div id="slideshow" class="slideshow">
                  </div>
              </div>
              <div id="caption" class="caption-container">
              </div>
          </div>
          <div id="thumbs" class="navigation">
              <div id="divSlider" runat="server">
              </div>
          </div>
          <div style="clear: both;"></div>

      </div>
  </div>

```


Next, we will make a call to Picasa API, get the images within a specific album and construct an unordered list html and assign this html to divSlider.


## Step 2: Query the API

Download and install [Google Data API SDK for .NET](https://code.google.com/p/google-gdata/downloads/detail?name=Google_Data_API_Setup_2.1.0.0.msi&can=2&q=).  Once installed add references to the following dlls in your ASP.NET project:

```
Google.GData.Client.dll
Google.GData.Photos.dll
Google.GData.Extensions.dll
```


After you have installed the SDK, the default location for these dlls will be `C:\Program Files (x86)\Google\Google Data API SDK\Sample` on 64 bit system and `C:\Program Files\Google\Google Data API SDK\Samples` on x86.


Next include the following namespaces in aspx.cs file:


```
using Google.GData.Photos;
using Google.GData.Client;
using Google.GData.Extensions;
using Google.GData.Extensions.Location;
```


There can be two scenarios you can use to display the images:
1.  You can display **any public album** which can belong to you or any other user. For this you will need Google account username (yours or the username of person whose album you are accessing) and the album id.
2.  **Accessing your own private album.** Here you will need to provide your username, password and album id. You will need to authenticate with API using your credentials and then access the album.


For any of the above cases, it’s a good idea to have username and album id information in Web.config or in a Constant class.

```
  <appSettings>
    <add key="albumid" value="5792668263385651889"/>
    <add key="user" value="username@gmail.com"/>
    <add key="password" value=""/>
  </appSettings>
 ``` 



To get album id for the album that contains the photos you want to display, you can simply brose to the album and from the url in the address bar, you can get the album id. For example in case of [https://plus.google.com/u/0/photos?tab=mq#photos/114107981519387242086/albums/5792668263385651889](https://plus.google.com/u/0/photos?tab=mq#photos/114107981519387242086/albums/5792668263385651889), the album id is 5792668263385651889.


Now, include the following in your `Page_Load`:


```
  string userName = ConfigurationManager.AppSettings["user"];
  string password = ConfigurationManager.AppSettings["password"];

  PicasaService service = new PicasaService("freak.roach-sample");
  //service.setUserCredentials(userName, password);  //-- needed when you need to show albums with private visibility

  PhotoQuery query = new PhotoQuery(PicasaQuery.CreatePicasaUri(userName, ConfigurationManager.AppSettings["albumid"]));
  PicasaFeed feed = service.Query(query);

  StringBuilder html = new StringBuilder();

  html.Append("<ul class=\"thumbs noscript\">");

  foreach (PicasaEntry entry in feed.Entries)
  {
      string title = entry.Title.Text.Substring(0, entry.Title.Text.LastIndexOf("."));

      html.Append(String.Format("<li><a class=\"thumb\" name={0} href=\"{1}\" title=\"{2}\"><img src=\"{3}\" alt=\"{4}\"/></a>",
          title, entry.Media.Content.Url, title, entry.Media.Thumbnails[0].Url, title));
      html.Append(String.Format("<div class=\"caption\"><div class=\"image-title\">{0}</div><div class=\"image-desc\">{1}</div></div></li>",
          title, entry.Summary.Text));

  }

  html.Append("</ul>");
  divSlider.InnerHtml = html.ToString();  

```            




That’s it, now when you run this project you will be able to view nice image gallery as below:

[<img 
border="0" height="277" 
src="http://2.bp.blogspot.com/-9pk01508_C8/UGksZI2I-wI/AAAAAAAAFxg/wzLnkv7KZ08/s400/gallery.PNG" 
width="400" 
/>](http://2.bp.blogspot.com/-9pk01508_C8/UGksZI2I-wI/AAAAAAAAFxg/wzLnkv7KZ08/s1600/gallery.PNG) 


### Next Steps

The above approach can be extended to show images from Twitter, Flickr, Facebook,  Photobucket etc as well.

### Source Code

Source code is available at the MSDN Samples Gallery below and you can use it as is; it is ready to go after you change the configuration key values

source code - 
[http://code.msdn.microsoft.com/Picasa-Google-Image-bc8bc8d6](http://code.msdn.microsoft.com/Picasa-Google-Image-bc8bc8d6)