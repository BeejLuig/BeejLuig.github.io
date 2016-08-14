---
layout: post
title:  "Viewport Units and Responsive Cubes"
date:   2016-08-14 01:12:11 +0000
---


I recently began playing with 3D transforms and shapes using CSS. One of the more complicated concepts is the CSS cube. There are plenty of great resources that explain how to make a cube using CSS, but this was my <a href="https://desandro.github.io/3dtransforms/docs/cube.html" target="_blank">favorite</a>. 

Once we have the cube, *what can we do with it*? With the help of viewport units, we can make it responsive!

A detailed explanation of viewport units is <a href="https://web-design-weekly.com/2014/11/18/viewport-units-vw-vh-vmin-vmax/" target="_blank">here</a>, but I will keep mine brief.

In the words of <a href="http://www.w3schools.com/css/css_rwd_viewport.asp" target="_blank">W3 Schools</a>, the **viewport** is the The viewport is the user's visible area of a web page. Obviously viewport sizes vary from screen to screen, so it's easy to see how viewport units can come in handy to create responsive elements.

There are four different types of viewport units, but I only used two to create a responsive cube. 

1vh = 1/100 viewport height
1vw = 1/100 viewport width

In short, one viewport unit acts as a percentage of the viewable axis. For example: 100vw is 100% of the viewport width, and 50vh is 50% of the viewport height. 

**Ok, so I have a cube**

<p data-height="495" data-theme-id="0" data-slug-hash="qNyByW" data-default-tab="result" data-user="BeejLuig" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/BeejLuig/pen/qNyByW/">CSS Cube with Javascript Button Navigation</a> by BJ Cantlupe (<a href="http://codepen.io/BeejLuig">@BeejLuig</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

I used a little Javascript to aid in the navigation of each side.

Taking a quick look at the CSS, we can see that the size of this cube is fixed:

```
.container {
  width: 200px;         // width of cube sides
  height: 200px;       //  height of cube sides
  position: relative;
  perspective: 1000px;   //pushes the cube back 1000px
  margin: auto;
  top: 100px;
}

#cube {
  width: 100%;         // fills container width (200px)
  height: 100%;       // fills container height (200px)
  position: absolute;
  transform-style: preserve-3d;
  transform: translateZ( -100px ); 
}

#cube figure {
  margin: 0;
  width: 196px;       // leaves 2px on either side for the border
  height: 196px;     //  leaves 2px on either side for the border
  display: block;
  position: absolute;
  border: 2px solid black;
}

#cube #front  { transform: rotateY(   0deg ) translateZ( 100px ); background: aqua;}
#cube #back   { transform: rotateX( 180deg ) translateZ( 100px ); background: magenta;}
#cube #right  { transform: rotateY(  90deg ) translateZ( 100px ); background: teal;}
#cube #left   { transform: rotateY( -90deg ) translateZ( 100px ); background: lightsalmon;}
#cube #top    { transform: rotateX(  90deg ) translateZ( 100px ); background: aquamarine;}
#cube #bottom { transform: rotateX( -90deg ) translateZ( 100px ); background: darkslateblue;}

/**** Cube Animation ****/

#cube.show-front  { transform: translateZ( -100px ) rotateY(    0deg ); }
#cube.show-back   { transform: translateZ( -100px ) rotateX( -180deg ); }
#cube.show-right  { transform: translateZ( -100px ) rotateY(  -90deg ); }
#cube.show-left   { transform: translateZ( -100px ) rotateY(   90deg ); }
#cube.show-top    { transform: translateZ( -100px ) rotateX(  -90deg ); }
#cube.show-bottom { transform: translateZ( -100px ) rotateX(   90deg ); }

#cube { transition: transform 1s; }
```

Check out the bunch of CSS code at the bottom. Each cube side has a rotation, and all have a Z-translation of 100px. 
At first, all six of our cube sides sit on top of each other. We rotate them on the X and Y axes to face the right way, then we push them out from the center to take their respective positions within the cube div. 

It is important to note that the `translateZ( 100px )` is pushing each side out 1/2 of the cube height/width from the center. If the cube width was 500px, we would need a Z-translation of 250px for each side.

## Getting Responsive

---

Now that we get what is happening with the cube, we just need to replace some of the CSS sizing with viewport units. 

First, we will adjust the `.container` of the cube

```
.container {
  width: 50vw;
  height: 50vw;
  position: relative;
  perspective: 1000px;
  margin: auto;
  top: 100px;
  transform: translateY(-5vh);
}
```

Notice that both the width and height are set to 50vw. That is important for keeping our sides square! 
Adding the `transform: translateY(-5vh);` is just there to push the cube up a little on the viewport. We keep the perspective at 1000px so that the cube doesn't get too close or far when the screen size changes.

Next, we'll take a look at the `#cube` selector

```
#cube {
  width: 100%;
  height: 100%;
  position: absolute;
  transform-style: preserve-3d;
  transform: translateZ( -50vw );
}
```

No need to change the width and height settings! The width and height settings will still fill the container effectively. We only change the Z-translation. Moving right along...

```
#cube figure {
  margin: 0;
  width: 50vw;
  height: 50vw;
  display: block;
  position: absolute;
}
```

All sides now have the same width and height of the container: 50% of the viewport width. The border is taken out to avoid confusion.

Now we adjust the individual sides and their corresponding animation classes

```
#cube #front  { transform: rotateY(   0deg ) translateZ( 25vw ); background: aqua;}
#cube #back   { transform: rotateX( 180deg ) translateZ( 25vw ); background: magenta;}
#cube #right  { transform: rotateY(  90deg ) translateZ( 25vw ); background: teal;}
#cube #left   { transform: rotateY( -90deg ) translateZ( 25vw ); background: lightsalmon;}
#cube #top    { transform: rotateX(  90deg ) translateZ( 25vw ); background: aquamarine;}
#cube #bottom { transform: rotateX( -90deg ) translateZ( 25vw ); background: darkslateblue;}

/**** Cube Animation ****/

#cube.show-front  { transform: translateZ( -25vw ) rotateY(    0deg ); }
#cube.show-back   { transform: translateZ( -25vw ) rotateX( -180deg ); }
#cube.show-right  { transform: translateZ( -25vw ) rotateY(  -90deg ); }
#cube.show-left   { transform: translateZ( -25vw ) rotateY(   90deg ); }
#cube.show-top    { transform: translateZ( -25vw ) rotateX(  -90deg ); }
#cube.show-bottom { transform: translateZ( -25vw ) rotateX(   90deg ); }

#cube { transition: transform 1s; }
```

Simply put, anywhere that used to have `100px` as a value now has `25vw`, and `-100px` is now `-25vw`. Remember, we get 25vw because it is half of 50vw, our cube width. All rotation values are the same as before.

And that's it! Now we have a responsive cube! Try resizing the screen to see the effect below.

<p data-height="652" data-theme-id="0" data-slug-hash="Eypovj" data-default-tab="result" data-user="BeejLuig" data-embed-version="2" class="codepen">See the Pen <a href="http://codepen.io/BeejLuig/pen/Eypovj/">Responsive CSS Cube with Javascript Button Navigation</a> by BJ Cantlupe (<a href="http://codepen.io/BeejLuig">@BeejLuig</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>



