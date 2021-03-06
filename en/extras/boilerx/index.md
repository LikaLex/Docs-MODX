---
title: "boilerX"
_old_id: "1316"
_old_uri: "revo/boilerx/"
---

## What is boilerX?

 ![](http://jpdevries.s3.amazonaws.com/assets/uploads/rtfm/HTML5_Logo_512.png)[HTML5 Boilerplate](https://github.com/jpdevries/boilerx) helps you build fast, robust, and adaptable web apps or sites. Kick-start your project with the combined knowledge and effort of 100s of developers, all in one little package.

 boilerX makes getting up and running with HTML5 Boilerplate a click away and is configurable using System Settings. Upon installation boilerplate assets are automatically placed in your assets folder.

## Installation

 Install via Package Manager.

 Make sure to remove "sample." from the names of the boilerX elements. This way, when installing the packages any changes you may make won't be overridden.

## Example Template

 boilerX comes with an example Template that simply includes several chunks in the correct order.

 ``` php
[[bx-head-open]]
[[bx-head-append]]
[[bx-head-close]]
[[bx-container-open]]
[[*content]]
[[bx-container-close]]
[[bx-bottom-open]]
[[bx-bottom-close]]
```

## boilerX Chunks

> bx-head-open

 The bulk of the header, includes CSS and modernizr as well as meta tags.

> bx-head-append

 Blank, meant to be an easy way to include CSS stylesheets, JavaScript, or meta tags before the closing head tag. This would be a good place to include MetaX.

> bx-head-close

 Simply closes the body tag.

> bx-container-close

 Blank, meant to be used for a wrapper div if needed.

> bx-container-close

 Blank, meant to close anything including in the bx-container-open Chunk.

> bx-bottom-open

 Deffered JavaScript and Google Analytics tracking code. **bx-bottom-close**

> bx-bottom-close

 Closes the body and html tags.

## System Settings

 | Name                | Description                                          | Default                                                    |
 | ------------------- | ---------------------------------------------------- | ---------------------------------------------------------- |
 | Google Analytics ID | Optional, if not set will comment out tracking code  | UAXXXXXXXX1                                                |
 | CSS Path            | Path to your main.css stylesheet                     | assets/components/boilerx/css/main.css                     |
 | Normalize CSS Path  | Path to your normalize.css stylesheet                | assets/components/boilerx/css/normalize.css                |
 | jQuery version      | Version of jQuery to use with HTML5 Boilerplate      | 1.9.1                                                      |
 | Modernizr.js Path   | Path to you modernizr.js path                        | assets/components/boilerx/js/vendor/modernizr–2.6.2.min.js |
 | Meta Author         | Sets meta authors tag                                | Site Authors                                               |
 | Meta Description    | Sets the meta description tag                        | Not Another Wordpress Site                                 |
 | Meta Viewport       | Sets iOS viewport settings                           | width=device-width,initial-scale=1                         |
 | IE Edge Mode        | Sets IE Edge mode tag                                | IE=edge,chrome=1                                           |
 | IE Chrome Frame     | Version of IE to start displaying Chrome Frame       | 7                                                          |
 | Show Comments       | Whether or not to show tips using comments in markup | true                                                       |

## Example Markup

 Default output generated by the sample.BoilerXTemplate. Replicates HTML5 Boilerplate 4.2.0 markup (h5bp-html5-boilerplate-defe483)

 ``` html
<!--[if lt IE 7]>      <html class="no-js lt-ie9 lt-ie8 lt-ie7"> <![endif]-->
<!--[if IE 7]>         <html class="no-js lt-ie9 lt-ie8"> <![endif]-->
<!--[if IE 8]>         <html class="no-js lt-ie9"> <![endif]-->
<!--[if gt IE 8]><!--> <html class="no-js"> <!--<![endif]-->
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible"  content="IE=edge,chrome=1">
    <title>MODX Revolution - Home</title>
    <meta name="description" content="Site Description">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <!-- Place favicon.ico and apple-touch-icon.png in the root directory -->
    <link rel="stylesheet" href="assets/components/boilerx/css/normalize.css">
    <link rel="stylesheet" href="assets/components/boilerx/css/main.css">
    <script src="assets/components/boilerx/js/vendor/modernizr-2.6.2.min.js"></script>
    <!-- Use this chunk to append to your head tag -->
</head>
    <!-- Use these body classes to target any combination of specific templates, ids, children, and class_keys -->
    <body class="t-2 id-1 p-0 ck-modDocument">
    <!--[if lt IE 8]>
    <p class="chromeframe">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> or <a href="http://www.google.com/chromeframe/?redirect=true">activate Google Chrome Frame</a> to improve your experience.</p>
    <![endif]-->
    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
    <script>window.jQuery || document.write('<script src="assets/components/boilerx/js/vendor/jquery-1.9.1.min.js"><\/script>')</script>
    <script src="assets/components/boilerx/js/plugins.js"></script>
    <script src="assets/components/boilerx/js/main.js"></script>
    <!-- Change UAXXXXXXXX1 to be your site's ID by setting bx.ga-id System Setting to auto-enable tracking -->
    <!--script>
        var _gaq=[ ['_setAccount','UAXXXXXXXX1'],['_trackPageview'] ];
        (function(d,t){var g=d.createElement(t),s=d.getElementsByTagName(t)[0];
        g.src=('https:'==location.protocol?'//ssl':'//www')+'.google-analytics.com/ga.js';
        s.parentNode.insertBefore(g,s)}(document,'script'));
    </script-->
</body>
</html>
```

## See Also

1. [bx-head-open](extras/boilerx/bx-head-open)
2. [bx-head-append](extras/boilerx/bx-head-append)
3. [bx-head-close](extras/boilerx/bx-head-close)
4. [bx-container-open](extras/boilerx/bx-container-open)
5. [bx-container-close](extras/boilerx/bx-container-close)
6. [bx-bottom-open](extras/boilerx/bx-bottom-open)
7. [bx-bottom-close](extras/boilerx/bx-bottom-close)
