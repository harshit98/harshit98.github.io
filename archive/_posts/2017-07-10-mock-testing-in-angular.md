---
layout: post
title: "Mock Testing in Angular using MockBackend"
date: 2017-07-10
category: archive
comments: true
author: "Harshit Prasad"
tags: [mock, testing, angular, web-development]
---


***This blog was originally posted on FOSSASIA Blog.***

In this blog, I’ll be sharing how we are testing services which we are using in Susper development.We’re using Angular 4 in our project as tech stack and we use Jasmine framework for testing purpose.

Tests are written to avoid issues which occur again and again. For example: Since we have implemented knowledge graph, we faced a lot of issues like:

 - When a user enters a query, search results appear but knowledge graph does not appear.
 - When a fresh query is entered or page is refreshed, knowledge graph does not appear.

We resolved such issue, by writing tests. The data is being taken with the help of an API. So, it will require testing using HTTP. Instead of testing like this, there is a better way by using `MockBackend` module which comes with Angular package.

Testing with `MockBackend` is a more sensible approach. This allows us to mock our responses and avoid hitting the actual backend which results in boosting our testing.

To use the MockBackend feature, it requires creating a mock data.

For `knowledge-service` it looks like this:
```js
export const MockKnowledgeApi {
  results: {
    uri: "http://dbpedia.org/resource/Berlin",
    label: "Berlin",
  }
  MaxHits: 5
};
```

To use the `MockBackend` feature, import `MockBackend`, `MockConnection`, `BaseRequestOptions` and `MockKnowledgeApi`

```ts
import { MockBackend, MockConnection } from "@angular/http/testing";
import { MockKnowledgeApi } from "./shared/mock-backend/knowledge.mock";
import { BaseRequestOptions } from "@angular/http";
```

Create a mock setup. In this case, we will create mock setup w.r.t HTTP because data from API is being returned as HTTP. If data, is being returned in JSON format, create a mock setup w.r.t jsonp.

```js
const mockHttp_provider = {
    provide: Http,
    deps: [MockBackend, BaseRequestOptions],
    useFactory: (backend: MockBackend,options: BaseRequestOptions) => {
        return new Http(backend, options);
    }
};
```

Now, describe the test suite. Inside, describe the function, don’t import `MockConnection`. It will throw error since it is only used to create a fake backend. It should look like this:

```js
providers: [
  KnowledgeapiService,
  MockBackend,
  BaseRequestOptions,
  mockHttp_provider,
]
```

Define service as `KnowledgeService` and backend as `MockBackend`. Inject both the services in `beforeEach()` function.

Now to actually test the service, create a query.
```js
const searchquery = "Berlin";
```

The written specs should look like this. I won’t go much in detail here, but I’ll cover up the key points of code.
```js
it("should call knowledge service API and return the result", () => {
    backend.connections.subscribe((connection: MockConnection) => {
        const options = new ResponseOptions({
            body: JSON.stringify(MockKnowledgeApi)
        });
        connection.mockRespond(new Response(options));
        expect(connection.request.method).toEqual(RequestMethod.Get);
    });
}
```

Here, mockRespond will mock our response and it will test whether the service is working or not. Already, we have defined a query.
It should perform an API call and should be equal to searchquery which we have defined already as ‘Berlin’.

```js
expect(connection.request.url).toBe(
    "http://lookup.dbpedia.org/api/search/KeywordSearch" + "?&QueryString=${searchquery}"
);
```

At last, it will check if it’s working or not. If it’s working, then test case will pass. Please note, it will not hit the actual backend.

```js
service.getsearchresults(searchquery).subscribe((res) => {
    expect(res).toEqual(MockKnowledgeApi);
);
```

In this way, we have written tests for knowledge graph to avoid feature breaking issues in future. We will be adding tests like these for other services as well which are being used in Susper project.

### Resources
- [Setting Up Angular-2 Mockbackend](https://keyholesoftware.com/2017/01/09/setting-up-angular-2-mockbackend/)
- [Angular-2 Mockbackend Example for Backendless Development](http://jasonwatmore.com/post/2016/11/24/angular-2-mockbackend-example-for-backendless-development)
