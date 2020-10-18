---
layout: post
title: "Refactor Dropdown in Susper"
date: 2017-07-12
category: archive
comments: true
author: "Harshit Prasad"
tags: [refactore, frontend, web-development]
---


***This blog was originally posted on FOSSASIA Blog.***

The first version of the Susper top menu was providing links to resources and tutorials. In the next version of the menu, we were looking for a menu with more colorful icons, a cleaner UI design and a menu that should appear on the homepage as well. In this blog, I will discuss about refactoring the dropdown menu. This is how earlier dropdown of Susper looks like:

![dropdown_1](/assets/png/dropdown_1.png){:width="720px"}
{: style="text-align: center;"}

We decided to create a separate component for the menu DropdownComponent.

At first, I created a drop down menu with matching dimensions similar to what Google follows. Then, I gave padding: 28px to create similar UI to market leader. This will make a dropdown menu with clean UI design. I replaced the old icons with colorful icons. In the dropdown we have:

Added more projects of FOSSASIA like eventyay, loklak, susi and main website of FOSSASIA.

![dropdown_2](/assets/png/dropdown_2.png){:width="720px"}
{: style="text-align: center;"}

The main problem I faced was aligning the content inside the dropdown and they should not get disturbed when the screen size changes.

I kept the each icon dimensions as 48 x 48 inside drop down menu. I also arranged them in a row. It was easy to use `div` element to create rows rather than using `ul` and `li` tags which were implemented earlier.

To create a horizontal grey line effect, I used the `hr` element. I made sure, padding remained the same above and below the horizontal line.

![dropdown_3](/assets/png/dropdown_3.png){:width="720px"}
{: style="text-align: center;"}

This is how I worked on refactoring dropdown menu and added it on the homepage as well.

### Resources
- [CSS Dropdown](https://www.w3schools.com/css/css_dropdowns.asp)
- [Creating Pure CSS Dropdown](https://csswizardry.com/2011/02/creating-a-pure-css-dropdown-menu/)
