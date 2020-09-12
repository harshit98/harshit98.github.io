---
layout: post
title: "Discovering Search Provider in Firefox"
date: 2017-06-29
category: archive
comments: true
author: "Harshit Prasad"
tags: [firefox, search-engine, search, google-chrome]
---


***This blog was originally posted on FOSSASIA Blog.***

Have you ever seen on the sites like Google and DuckDuckGo, when a user visits their website first time they provide an option to add their search engine as default search provider. Similarly, we raised up an issue regarding the implementation of this feature.

Currently, we have implemented this feature but facing some issues. Here is a screenshot:

![susper-default-search-engine](/assets/png/susper-default-search-engine.png){:width="720px"}
{: style="text-align: center;"}

Code for implementing this feature:

```
$(document).ready(function() {
    var isFirefox = typeof InstallTrigger !== 'undefined';

    if (!isFirefox) {
        $("#set-susper-default").remove();
        $(".input-group-btn").addClass("align-search-btn");
        $("#navbar-search").addClass("align-navsearch-btn");
    }

    if (window.external && window.external.IsSearchProviderInstalled) {
        var isInstalled = window.external.IsSearchProviderInstalled("http://susper.com");

        if (!isInstalled) {
            $("#set-susper-default").show();
        }
    }

    $("#install-susper").on("click", function() {
        window.external.AddSearchProvider("http://susper.com/susper.xml");
    });

    $("#cancel-installation").on("click", function() {
        $("#set-susper-default").remove();
    });
});
```
You can see, right side a box to add search provider as a default. But, this feature was not working for Chrome since it has some different approach to add search providers because of internal API method `AddSearchProvider()` and `window.external.IsSearchProviderInstalled()` works only with the newer version of Firefox and earlier they worked with Chrome as well, but now they don’t. So, to remove this feature from Chrome browser, we simply commented out the lines which are highlighted in the code as bold.

We are still having an issue like, if a user adds the search provider in Firefox by installing it and then he/she visits the site next time, that option still appears to be there. It should not appear again if Susper has been already added as default search provider. We’re working on it though via storing some metadata in local cache of the browser.

#### What else I did for other browsers compatibility?

I picked up the idea of adding an OpenSearch feature. It auto-discovers the search providers.

What I did simply:

1. Created an XML file as <yoursitename.xml> and configure it like this:
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <OpenSearchDescription xmlns="http://a9.com/-/spec/opensearch/1.1/">
    <ShortName>[Name of your website]</ShortName>
    <Description>[Description of your website]</Description>
    <Url type="text/html" template="http://www.yourwebsite.com/search?q=site:[Site host] {searchTerms}"/>
    </OpenSearchDescription>
    ```

2. Add an auto-discovery link in your main `index.html` file:
    ```
    <link rel="search" href="//susper.com/susper.xml" type="application/opensearchdescription+xml" title="susper.com"/>
    ```

We’re done! To see it in action, refresh the browser. The browser will auto-detect and tell you that a custom search engine was discovered and you can customise it according to your choice.

Check our susper.xml here: [susper.xml](https://github.com/fossasia/susper.com/blob/master/susper.xml)
