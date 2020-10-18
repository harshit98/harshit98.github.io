---
layout: post
title: "Adding Transition Effect to Voice Search in Susper"
date: 2017-08-01
category: archive
comments: true
author: "Harshit Prasad"
tags: [transition, effects, web-development, animation, frontend, rxjs]
---


***This blog was originally posted on FOSSASIA Blog.***

![transition_1](/assets/png/transition_1.png){:width="720px"}
{: style="text-align: center;"}

***This blog was originally posted on FOSSASIA Blog.***

Susper has been given a voice search feature through which it provides a user with a better experience of search. We introduced to enhance the speech-recognition user interface by adding transition effects. The transition effect was required to display appropriate messages according to voice being detected or not. The following messages were:

- When a user should start a voice search, it should display ‘Speak Now’ message for 1-2 seconds and then show up with message ‘Listening…’ to acknowledge user that now it is ready to recognize the voice which will be spoken.

- If a user should do not speak anything, it should display ‘Please check audio levels or your microphone working’ message in 3-4 seconds and should exit the voice search interface.

The idea of speech UI was taken from the market leader and it was implemented in a similar way.

On the homepage, it looks like this:

![transition_2](/assets/png/transition_2.png){:width="720px"}
{: style="text-align: center;"}

On the results page, it looks like this:

![transition_3](/assets/png/transition_3.png){:width="720px"}
{: style="text-align: center;"}

For creating transitions like, ‘Listening…’ and ‘Please check audio levels and microphone’ messages, we used CSS, RxJS Observables and timer() function.

Let’s start with RxJS Observables and timer() function.

#### RxJS Observables and `timer()`

`timer()` is used to emit numbers in sequence in every specified duration or after a given duration. It acts as an observable. For example:

```js
let countdown = Observable.timer(2000);
```
The above code will emit value of countdown in 2000 milliseconds. Similarly, let’s see another example:

```js
let countdown = Observable.timer(2000, 6000);
```
The above code will emit value of countdown in 2000 milliseconds and subsequent values in every 6000 milliseconds.

```js
export class SpeechToTextComponent implements OnInit {
    message: any = "Speak Now";
    timer: any;
    subscription: any;
    ticks: any;
    miccolor: any = "#f44";
}

ngOnInit() {
    this.timer = Observable.timer(1500, 2000);
    this.subscription = this.timer.subscribe(t => {
    
        this.ticks = t;  // it will throw listening message after 1.5 seconds

        // subsequent events will be performed in 2 secs interval as
        // it has been defined in timer()
        if (t === 1) {
            this.message = "Listening...";
        }

        // if no voice is given, it will throw audio level message and
        // unsubscribe to the event to exit back on homepage
        if (t === 4) {
            this.message = "Please check your microphone audio levels.";
            this.miccolor = "#C2C2C2";
        }

        if (t === 6) {
            this.subscription.unsubscribe();
            this.store.dispatch(new speechactions.SearchAction(false));
        }
    });
}
```
The above code will throw following messages at a particular time. For creating the text-animation effect, most developers go for plain javascript. The text-animation effects can also be achieved by using pure CSS.

#### Text animation using CSS

```css
@–webkit–keyframes typing {from {width:0;}}
.spch {
    font–weight: normal;
    line–height: 1.2;
    pointer–events: none;
    position: none;
    text–align: left;
    –webkit–font–smoothing: antialiased;
    transition: opacity .1s ease–in, margin–left .5s ease–in, top  0s linear 0.218s;
    –webkit–animation: typing 2s steps(21,end), blink–caret .5s step–end infinite alternate;
    white–space: nowrap;
    overflow: hidden;
    animation–delay: 3.5s;
}
```

`@keyframes` specifies animation code. Here `width: 0;` tells that animation begins from 0% width and ends to 100% width of the message. Also, `animation-delay: 3.5s` has been adjusted w.r.t timer to display messages with animation at the same time.

This is how it works now:

![transition_4](/assets/png/transition_4.gif){:width="720px"}
{: style="text-align: center;"}

### Resources:
- [Keyframe animation syntax by Chris Coyier Las](https://css-tricks.com/snippets/css/keyframe-animation-syntax/)
- [Observable RxJS](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#static-method-timer)
