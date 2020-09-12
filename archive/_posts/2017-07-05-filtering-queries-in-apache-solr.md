---
layout: post
title: "Filtering Queries using Apache Solr in Susper"
date: 2017-07-03
category: archive
comments: true
author: "Harshit Prasad"
tags: [filter, query, apache, solr, web-development]
---


***This blog was originally posted on FOSSASIA Blog.***

In this blog, I would like to share you how I have implemented tabs feature in Susper.

When a search is attempted, results are fetched in `results.component`. This is a separate component implemented using Angular where results are loaded.

Since there are many kinds of results being fetched from the YaCy server (web crawler), how should we filter them in separate tabs?

For example:- Under the tab ‘All’: All kind of results are being fetched including .png or .mp3 files which should be loaded under tabs ‘Images’ and ‘Videos’. The YaCy server is written in Java on Apache Solr technology.

Going through Solr documents, I found the solution to filter queries using ‘fq’ parameter.

#### What is `fq` parameter?

‘fq’ stands for filter query. It returns the document which we want or in simple words returns filtered document. Documents will only be included in the result if they are at the intersection of the document sets resulting from each fq.

#### How we did we use `fq` parameter in Susper?

To explain it better, I’m sharing a code snippet here which has been written to filter out queries:

```
if (query[‘fq’]) {
    if (query[‘fq’].includes(‘png’)) {
        this.resultDisplay = ‘images’;
        urldata.fq = ‘url_file_ext_s: (png + OR + jpeg + OR + jpg + OR + gif)’;
    } else if (query[‘fq’].includes(‘avi’)) {
        this.resultDisplay = ‘videos’;
    } else {
        this.resultDisplay = ‘all’;
    }
}
```

What is ongoing here is that we have subscribed to a query and used if and else conditions. query[‘fq’] simply filters out the query which has been subscribed. include(‘png’) and include(.avi) is clear that we are filtering out the documents with these tabs. This action happens when the user clicks on a tab.

If the user clicks on images tab: files with .png are displayed. If the user clicks on videos tab: files with .avi are displayed.

`url_file_ext_s:()` is simply another solr syntax to provide the document format.

![filtering-queries-in-apache-solr](/assets/png/filtering_queries_in_apache_solr.png){:width="720px"}
{: style="text-align: center;"}

The flowchart above explains more clearly, how fq parameter is filtering out documents without affecting the total number of documents which yacy fetches by indexing web pages based on the query.

#### Resources:

- [Apache Solr - Common Query Parameters](https://wiki.apache.org/solr/CommonQueryParameters)
- [Apache Solr Query Parameters - Wiki](https://cwiki.apache.org/confluence/display/solr/Common+Query+Parameters)
