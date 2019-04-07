---
layout: post
title: "Snake Hacks"
date: 2019-04-07 17:22:00
---

Reversing Google's Snake Game in order figure out how it works and to beat it.

[Google added a snake game to Google Maps](https://snake.googlemaps.com) to celebrate April Fools (2019). This was exciting because I am working on [my own snake game](https://snake.manila.me). I decided to take a deep dive into Google's version to learn something and to try to hack it.

I didn't know where to start so I first had to identify where the JavaScript was that was running the game. This was easy enough as there wern't many script on the page. After opening the debug tools in firefox on snake.googlemaps.com I found the script I was looking for was just before the closing body tag, it's named [v20.js](https://snake.googlemaps.com/static/js/v20.js).

After running this file through a Javascript beatifier I started looking for key objects and familiar strings. Since I wanted to create a simple bot that automatically made the snake/train turn when it got to the edge of the map I was looking for the .addEventListener() method that listened for keydown events. That would give me a better understanding of how the snake is told to turn when someone presses a specific key. I was looking for the function that is called when a keydown even is triggered, finding this could allow me to call the same function that would be called on a keydown even at anytime for any reason, say for example when the snake gets close to a wall. 

Here is the obsfucated function that I found:

```JavaScript
function wa() {
    var a = R;
    a.active || (document.addEventListener("keydown", a.s), document.addEventListener("touchstart", a.l, {
        passive: !1
    }),
    /* ... */
    a.active = !0, a.b.start(), p("game_started"))
}
```

This function gives a way a lot of informtation. the expression ```var a = R;``` is a wonderful hint that R is likly our game object since it has a property "active" and a method named "start". I could re-write this function to help clear some things up:

```JavaScript
function attachEventListeners() { \\formerly named wa()
    var game = R;
    game.active || (document.addEventListener("keydown", game.s), document.addEventListener("touchstart", game.l, {
        passive: !1
    }),
    /* ... */ 
    game.active = !0, game.b.start(), p("game_started"))
}
```
I still don't know what the ``` s() ``` method is that's called on the keydown event. After some searching I came across what looked like a constructor function with the switch statement that would check which key was pressed and then run it's corresponsing action. Here is the obsfucated constructor function:

```JavaScript
function ua(a) {
    /* ... */
    this.s = function (d) {
        var e = d.code || d.key;
        if (e) switch (e) {
        case "ArrowLeft":
        case "Left":
        case "KeyA":
            P(b.b, 3);
            break;
        case "ArrowRight":
        case "Right":
        case "KeyD":
            P(b.b, 1);
            break;
        case "ArrowUp":
        case "Up":
        case "KeyW":
            P(b.b, 2);
            break;
        case "ArrowDown":
        case "Down":
        case "KeyS":
            P(b.b, 4)
        }
    /* ... */
}
```
There is a lot of useful information here but importantly we see that``` this.s ``` is a method that translates keydown events into game actions. I don't need to know what the function ``` P() ``` is, now that I know what the game object is we can open the debug console and as the train is moving we can call ``` R.s({key: "Left"}) ``` and the snake will change direction as if we had pressed the left arrow key.

I went ahead and turned this basic reversed piece of code into a bot that will play and beat the game by following a simple pattern. You can [see it in action and try it out for yourself here](https://github.com/manila/googlemaps-snake-hacks)

Improvements? Issues? Comments?
emails me at hello at this domain.
