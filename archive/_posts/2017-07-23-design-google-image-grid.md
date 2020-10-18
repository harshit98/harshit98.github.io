---
layout: post
title: "Designing Google Like Image Search Grid"
date: 2017-07-12
category: archive
comments: true
author: "Harshit Prasad"
tags: [search, google, web-development, frontend, design]
---


***This blog was originally posted on FOSSASIA Blog.***

In this blog, I’ll be sharing the idea how we have perfectly aligned images to the same height in Susper without disturbing their ratio. When it comes to aligning images perfectly, they should have:

- Same height.
- A proper ratio to maintain the image quality.

Many developers apply same width and height without keeping in mind about image ratio which results in:

- Blurred image.
- Image with a lot of pixels.
- Cropping of an image.

Earlier Susper was having image layout like this:

![grid_1](/assets/png/grid_1.png){:width="720px"}
{: style="text-align: center;"}

In the screenshot, images are not properly aligned. They are also not having the same height. We wanted to improve the layout of images just like market leaders Google and DuckDuckGo.

#### How we implemented a better layout for images?

```html
<div class=“container”>
  <div class=“grid” *ngIf=“Display(‘images’)”>
    <div class=“cell” *ngFor=“let item of item$ | async”>
      <a class=“image-pointer” href=“{{item.link}}”>
        <img class=“responsive-image” src=“{{item.link}}”></a>
    </div>
  </div>
</div>
```

I have created a `container`, in which images will be loaded from YaCy server. Then, I have created a grid with an equal number of rows and column. I have adjusted the height and width of rows and columns to obtain a grid which contains each division as a cell. The image will load inside the cell. Each cell consists of one image.

![grid_2](/assets/png/grid_2.png){:width="720px"}
{: style="text-align: center;"}

```css
.grid {
  padding–left: 80px;
}

.container {
  width: 100%;
  margin: 0;
  top: 0;
  bottom: 0;
  padding: 0;
}
```

After implementing it, we were facing issues like cropping of the image inside a cell. So, to avoid cropping and maintaining the image ratio we introduced `.responsive-image` class which will avoid cropping of images inside cell.

```css
.responsive–image {
  max–width: 100%;
  height: 200px;
  padding–top: 20px;
  padding: 0.6%;
  display: inline–block;
  float: left;
}
```
This is how Susper’s image section looks now:

![grid_3](/assets/png/grid_3.png){:width="720px"}
{: style="text-align: center;"}

It took some time to align images correctly, but somehow we succeeded in creating a perfect layout for the images.

We are facing some issues regarding images. Some of them don’t appear due to broken link. This issue will be resolved soon from the server side.

### Resources
- [CSS aspect ratio to have images of same height](http://www.minimit.com/articles/tips-resources/css-aspect-ratio-to-have-images-of-same-height)
- [Control image aspect ratio using CSS3](http://www.creativebloq.com/css3/control-image-aspect-ratios-css3-2122968)
