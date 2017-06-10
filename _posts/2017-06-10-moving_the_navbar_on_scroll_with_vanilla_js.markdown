---
layout: post
title:  "Moving the Navbar On Scroll with Vanilla JS"
date:   2017-06-10 22:33:08 +0000
---


This week, I had a little front end problem to solve at work. The problem is this:

> Slide a fixed-position navbar header out of view on scroll down, and back into view on scroll up.

This exercise felt like it was fun enough to share, so here goes!

**The HTML**

For the sake of this exercise, we a super basic layout. We have a `nav`, a header title, and a `p` tag with some filler text.

<p data-height="265" data-theme-id="0" data-slug-hash="RgaaRY" data-default-tab="html" data-user="BeejLuig" data-embed-version="2" data-pen-title="Navbar Slide Up/Down on Scroll Example" class="codepen">See the Pen <a href="https://codepen.io/BeejLuig/pen/RgaaRY/">Navbar Slide Up/Down on Scroll Example</a> by BJ Cantlupe (<a href="https://codepen.io/BeejLuig">@BeejLuig</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

**The CSS**

To get the fixed-position nav set up, we need to do a few things. First, we set the nav `position` to `fixed`. This will mean that that no matter where you navigate on the page, the nav will stay put in the window. We then need to set the `left` and `top` attributes to 0 so that the nav stays on the top of the screen. We then need to make sure that the width of the navbar covers the entire window. I like using the `vw`, or "viewport width" unit for this. If you want to know more about viewport units, [this](https://web-design-weekly.com/2014/11/18/viewport-units-vw-vh-vmin-vmax/) is a good article to check out.

<p data-height="265" data-theme-id="0" data-slug-hash="RgaaRY" data-default-tab="css" data-user="BeejLuig" data-embed-version="2" data-pen-title="Navbar Slide Up/Down on Scroll Example" class="codepen">See the Pen <a href="https://codepen.io/BeejLuig/pen/RgaaRY/">Navbar Slide Up/Down on Scroll Example</a> by BJ Cantlupe (<a href="https://codepen.io/BeejLuig">@BeejLuig</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<small>I have been forcing myself to use Sass for all of my CSS needs recently, even little examples like this</small>

Last but not least, we want to set the `transition` property to react to `transform` for a smooth animation effect. Then, we add a `transform: translateY(-100px)` to a `slide-up` class that we aren't using yet. That comes next.


**The JavaScript**

To get this effect working, we need to do two things with JavaScript. First, we need to figure out when we are scrolling up or down. We then adjust the navbar accordingly. 

We can access the scroll height of the window with `window.scrollY`. A value of 0 means that the window is entirely scrolled up. Therefore, we know that we are scrolling down if the value of `window.scrollY` is greater than the current scroll value.

`window.onscroll` is an easy shortcut to create a function reacting to scroll events.

```javascript
let scrollHeight = window.scrollY;

window.onscroll = function() {
    if (window.scrollY > scrollHeight) {
      // Scrolling down!
    } else if (window.scrollY < scrollHeight) {
      // Scrolling up!
    }
    scrollHeight = window.scrollY;
  }
```

With the scroll event set up, all that is left to do is applying the transition effect! Our `.slide-up` CSS is actually doing the heavy lifting already. If we translate the navbar by -100px, it will move up and out of the viewport. So all we need to do is apply the `slide-up` class on scroll down and remove it on scroll up.

<p data-height="265" data-theme-id="0" data-slug-hash="RgaaRY" data-default-tab="js" data-user="BeejLuig" data-embed-version="2" data-pen-title="Navbar Slide Up/Down on Scroll Example" class="codepen">See the Pen <a href="https://codepen.io/BeejLuig/pen/RgaaRY/">Navbar Slide Up/Down on Scroll Example</a> by BJ Cantlupe (<a href="https://codepen.io/BeejLuig">@BeejLuig</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

That's it! You can fork and edit the source code for this example on [CodePen](https://codepen.io/BeejLuig/pen/RgaaRY).

<p data-height="265" data-theme-id="0" data-slug-hash="RgaaRY" data-default-tab="result" data-user="BeejLuig" data-embed-version="2" data-pen-title="Navbar Slide Up/Down on Scroll Example" class="codepen">See the Pen <a href="https://codepen.io/BeejLuig/pen/RgaaRY/">Navbar Slide Up/Down on Scroll Example</a> by BJ Cantlupe (<a href="https://codepen.io/BeejLuig">@BeejLuig</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<small><em>Thanks for reading! If you have questions or comments, you can find me on <a target="_blank" href="https://twitter.com/BeejLuig">Twitter</a>, <a href="https://www.linkedin.com/in/bj-cantlupe/" target="_blank">LinkedIn</a>, or just <a href="mailto:bjcantlupe@gmail.com">email</a> me.</em></small>
