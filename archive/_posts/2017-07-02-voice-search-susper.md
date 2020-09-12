---
layout: post
title: "Implementing Voice Search In Susper"
date: 2017-07-02
category: archive
comments: true
author: "Harshit Prasad"
tags: [voice-search, google-search, angular, web-development]
---


***This blog was originally posted on FOSSASIA Blog.***

Last week, as a team we decided to implement voice search in Susper. Google Chrome provides an API to integrate Speech recognition feature with any website.

More about API can be read here: [html5-speech-recognition-api](https://shapeshed.com/html5-speech-recognition-api/)

The explanation might be in Javascript but it has been written following syntax of Angular 4 and Typescript. So, I created a speech-service including files:

- speech-service.ts
- speech-service.spec.ts

Code for `speech-service.ts`:

This is the code which will control the working of voice search.
```
import { Injectable, NgZone } from ‘@angular/core’;
import { Observable } from ‘rxjs/Rx’;

interface IWindow extends Window {
  webkitSpeechRecognition: any;
}

@Injectable()
export class SpeechService {
    constructor(private zone: NgZone) { }

    record(lang: string): Observable<string> {
        return Observable.create(observe => {
            const { webkitSpeechRecognition }: IWindow = <IWindow>window;
            const recognition = new webkitSpeechRecognition();

            recognition.continuous = true;
            recognition.interimResults = true;
            recognition.onresult = take => this.zone.run(() => observe.next(take.results.item(take.results.length – 1).item(0).transcript));

            recognition.onerror = err =>observe.error(err);
            recognition.onend = () => observe.complete();
            recognition.lang = lang;
            recognition.start();
        });
    }
}
```

You can find more details about API following the link which I have provided above in starting. Here `recognition.onend() => observe.complete()` works as an important role here. Many developers forget to use it when working on voice search feature. ***It works like: whenever a user stops speaking, it will automatically understand that voice action has now been completed and the search can be attempted.*** And for this:

```
speechRecognition() {
  this.speech.record(‘en_US’).subscribe(voice => this.onquery(voice));
}
```

We have used `speechRecognition()` API function. `onquery()` API function is called when a query is entered in a search bar.

Default language has been set up as ‘en_US’ i.e English. We also created an interface to link it with the API which Google Chrome provides for adding voice search feature on any website.

I have also used a separate module by name `NgZone`.

#### What is the role of NgZone module? 

***It is used as an injectable service for executing working inside or outside of the Angular zone. I won’t go into detail about this module much here. More about it can be found on angular-docs website.***

This feature only works in Google Chrome browser and for Firefox it doesn’t. So, for Firefox browser there was no need to show this feature since voice search does not work Firefox. What we did simply use CSS code like this:

```
@–moz–document url–prefix() {
  .microphone {
    display: none;
  }
}
```

`@-moz-document url-prefix()` is used to target elements for Firefox browser only.

Hence using, this feature we made it possible to hide microphone icon (part of voice search feature) from Firefox and make it appear in Chrome.

For first time users: To use voice search feature select the microphone feature which will trigger `speechRecognition()` function and will ask you permission to allow your laptop/desktop microphone to detect your voice. Once allowing it, we’re done! Now the user can easily, use voice search feature on Susper to search for a random thing they wish for!
