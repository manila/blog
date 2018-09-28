---
layout: post
title: Reader Mode without JavaScript
date: 2018-09-27 23:40:00
---

I learned about a really nifty CSS psuedo-class, the :target selector.

Using the :target selector you can style elements when they are "targeted" by a hyperlink (aka an [anchor](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a)).  This is denoted with the octothorp (#) + element name trailing the URL.

So in part to facilitate users without JavaScript enabled and because it's a neat trick, I used this CSS selector to make a reader mode toggle that doesn't require JavaScript.

<a class="enabled" href="#reader">Try it out</a>
<a class="disabled" href="#">Toggle it back</a> 

*\*this will not work if your browser has it's own "reader mode" enabled*

note the URL when toggling reader mode, when enabled the #reader element is being appended to the URL.

**How it works**

Here is the markup for the toggle link:

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      body 
      {
        background: #000;
        color: #FFF;
      }  

      :target 
      {
        color: #000;
        background: #FFF;
      }  

      :target .enabled, .disabled 
      {
        display: none;
      }  

      :target .disabled, .enabled
      {
        display: inline;
      }
    </style>
  </head>
  <body id="reader">
    <a class="enabled" href="#reader">Try it out</a>
    <a class="disabled" href="#">Toggle it back</a> 
  </body>
</html>
```

Starting in the CSS the body is styled with white (#FFF) text on a black (#00) background. Then there is the first :target selector that swaps the colors so that it's not black text on a white background.  This CSS becomes "active" when the URL has #reader appended to it. Because the body element has the id of reader then it becomes targeted and thus selected with the :target psuedo-class.

The .disabled and .enabled CSS classes are to "toggle" the link so that you can select and deselect the #reader element easily.

Copy the code from above, paste it into a file (e.g. index.html) and try it out for yourself!

Questions? Corrections? Critiques?

email me at hi @ this domain.
