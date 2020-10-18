---
layout: post
title: "Implementing Text-To-Speech Feature in Susper"
date: 2017-08-01
category: archive
comments: true
author: "Harshit Prasad"
tags: [transition, effects, web-development, animation, frontend, rxjs]
---


***This blog was originally posted on FOSSASIA Blog.***

Susper has been given a voice search feature through which it provides the user a better experience of search. We introduced to enhance the speech recognition by adding Speech Synthesis or Text-To-Speech feature. The speech synthesis feature should only work when a voice search is attempted.

The idea was to create speech synthesis similar to market leader. Here is the link to YouTube video showing the demo of the feature: Video link

In the video, it will show demo :

If a manual search is used then the feature should not work.
If voice search is used then the feature should work.
For implementing this feature, we used Speech Synthesis API which is provided with Google Chrome browser 33 and above versions.

window.speechSynthesis.speak(‘Hello world!’); can be used to check whether the browser supports this feature or not.

First, we created an interface:

```ts
interface IWindow extends Window {
    SpeechSynthesisUtterance: any;
    speechSynthesis: any;
};
```

Then under `@Injectable` we created a class for the `SpeechSynthesisService`

```ts
export class SpeechSynthesisService {
    utterence: any;
    
    constructor(private zone: NgZone) { }
    
    speak(text: string): void {
        const { SpeechSynthesisUtterance }: IWindow = <IWindow>window;
        const { speechSynthesis }: IWindow = <IWindow>window;
        this.utterence = new SpeechSynthesisUtterance();
        this.utterence.text = text; // utters text
        this.utterence.lang = "en-US"; // default language
        this.utterence.volume = 1; // it can be set between 0 and 1
        this.utterence.rate = 1; // it can be set between 0 and 1
        this.utterence.pitch = 1; // it can be set between 0 and 1
        
        (window as any).speechSynthesis.speak(this.utterence);
    }
    
    // to pause the queue of utterence
    pause(): void {
        const { speechSynthesis }: IWindow = <IWindow>window;
        const { SpeechSynthesisUtterance }: IWindow = <IWindow>window;
        this.utterence = new SpeechSynthesisUtterance();
  
        (window as any).speechSynthesis.pause();
    }
}
```

The above code will implement the feature ***Text-To-Speech***.

We call speech synthesis only when voice search mode is activated. Here, we have used redux to check whether the mode is ‘speech’ or not. When the mode is ‘speech’ then it should utter the description inside the infobox.

We did the following changes in `infobox.component.ts`:

```ts
import { SpeechSynthesisService } from "../speech–synthesis.service";

speechMode: any;

constructor(private synthesis: SpeechSynthesisService) { }

this.query$ = store.select(fromRoot.getwholequery);

this.query$.subscribe(query => {
    this.keyword = query.query;
    this.speechMode = query.mode;
});
```

And we added a conditional statement to check whether mode is ‘speech’ or not.

```ts
// conditional statement
// to check if mode is ‘speech’ or not
if (this.speechMode === "speech") {
    this.startSpeaking(this.results[0].description);
}

startSpeaking(description) {
    this.synthesis.speak(description);
    this.synthesis.pause();
}
```

### Resources

- [Getting started with the Speech Synthesis API](https://blog.teamtreehouse.com/getting-started-speech-synthesis-api)
- [Web Apps that talk – Introduction to Speech Synthesis API](https://developers.google.com/web/updates/2014/01/Web-apps-that-talk-Introduction-to-the-Speech-Synthesis-API)
