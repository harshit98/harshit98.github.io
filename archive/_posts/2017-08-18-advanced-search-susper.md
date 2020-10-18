---
layout: post
title: "Implementing Advanced Search Feature in Susper"
date: 2017-08-18
category: archive
comments: true
author: "Harshit Prasad"
tags: [search, advanced-search, web-development, frontend]
---


***This blog was originally posted on FOSSASIA Blog.***

Susper has been provided ‘Advanced Search’ feature which provides the user a great experience to search for desired results. Advanced search has been implemented in such a way that it shows top authors, top providers, and distribution based on URL protocol. Users can choose any of these options to get best results.

![advanced_1](/assets/png/advance_1.png){:width="720px"}
{: style="text-align: center;"}

We receive data of each facet name from YaCy using yacy search endpoint. More about YaCy search endpoint can be found here:
```
http://yacy.searchlab.eu/solr/select?query=india&fl=last_modified&start=0&rows=15&facet=true&facet.mincount=1&facet.field=host_s&facet.field=url_protocol_s&facet.field=author_sxt&facet.field=collection_sxt&wt=yjson
```

![advanced_2](/assets/png/advance_2.png){:width="720px"}
{: style="text-align: center;"}

For implementing this feature, we created Actions and Reducers using concepts of Redux.

The implemented actions can be found here: `https://github.com/fossasia/susper.com/blob/master/src/app/actions/search.ts`

Actions have been implemented because these actually represent some kind of event. For example: like the beginning of an API call here.

We also have created an interface for search action which can be found here under reducers as filename `index.ts`:

`https://github.com/fossasia/susper.com/blob/master/src/app/reducers/index.ts`

Reducers are a pure type of function that takes the previous state and an action and returns the next state. We have used Redux to implement actions and reducers for the advanced search.

For advanced search, the reducer file can be found here: `https://github.com/fossasia/susper.com/blob/master/src/app/reducers/search.ts`

The main logic has been implemented under `advancedsearch.component.ts`:

```js
export class AdvancedsearchComponent implements OnInit {
    querylook = {}; // array of urls
    navigation$: Observable<any>;
    selectedelements: Array<any> = []; // selected urls by user

    changeURL(modifier, element) {
        // based on query urls are fetched
        // if an url is selected by user, it is decoded
        this.querylook["query"] = this.querylook["query"] + "+" + decodeURIComponent(modifier);
        this.selectedelements.push(element);
        
        // according to selected urls
        // results are loaded from yacy
        this.route.navigate(["/search"], {queryParams: this.querylook});
    }
    
    // same method is implemented for removing an url
    removeURL(modifier) {
        this.querylook["query"] = this.querylook["query"].replace("+" + decodeURIComponent(modifier), "");
        this.route.navigate(["/search"], {queryParams: this.querylook});
    }
}
```

The `changeURL()` function replaces the query with a query and selected URL and then searches for the results only from the URL provider. The `removeURL()` function removes URL from the query and works as a normal search, searching for the results from all providers.

### Resources
- [Using Postman for analyzing an endpoint of an API](https://www.getpostman.com/docs)
- [Redux documentation](http://redux.js.org/docs/introduction/)
