+++
title = "Please Stop Being Such Good Developers"
date = 2023-08-23T12:00:00-07:00
draft = false
categories = ["software", "humor"]
tags = ["overengineering", "enterprise hello world"]
+++

{{<imgwebp src="ai_art.png">}}

So, sometimes, in `FooApplication`, we need to extract some Nurble Data from the HTTP request, which is included as a base-64-encoded JSON blob in a cookie from the Nurble provider.

<!--more-->

```
/* extract nurbleData from the HTTP request */
function getNurbleData(request){
    let nurbleKey = `nurble_${NURBLE_API_KEY}`;
    if(request.cookies[nurbleKey]){
        /* nurble data is base64 encoded JSON */
        return JSON.parse(atob(request.cookies[nurbleKey]));
    }
}
```

Look at this function! It’s awful! Let’s do SOFTWARE ENGINEERING to it!

First of all, what if there’s a parsing error? Are we just going to bubble that up and let it get caught by our normal error handling machinery? Hell no!

```
/* extract nurbleData from the HTTP request */
function getNurbleData(request){
    let nurbleKey = `nurble_${NURBLE_API_KEY}`;
    if(request.cookies[nurbleKey]){
        /* nurble data is base64 encoded JSON */
        try{
            return JSON.parse(atob(request.cookies[nurbleKey]));
        }
        catch(err){
            console.error(`getNurbleData couldn't parse
                           nurble cookie: ${request.cookies[nurbleKey]}`);
        }

    }
}
```

Then, that nurble key prefix! What happens if it changes? We could be left editing code directly when we could simply adjust a constant! MAGIC VALUES BAD, YOU GUYS.

```
/* extract nurbleData from the HTTP request */
const NURBLE_KEY_PREFIX = "nurble_";
const NURBLE_KEY = `${NURBLE_KEY_PREFIX}_${NURBLE_API_KEY}`;
function getNurbleData(request){
    if(request.cookies[NURBLE_KEY]){
        /* nurble data is base64 encoded JSON */
        try{
            return JSON.parse(atob(request.cookies[NURBLE_KEY]));
        }
        catch(err){
            console.error(`getNurbleData couldn't parse
                           nurble cookie: ${request.cookies[NURBLE_KEY]}`);
        }
    }
}
```

Here’s a thing, though - what happens if I call getNurbleData twice in a row. Am I just going to inefficiently parse JSON two times? That’s so wasteful. Let’s save the result of this function against the request in case we need it again later.

```
/* extract nurbleData from the HTTP request */
const NURBLE_KEY_PREFIX = "nurble_";
const NURBLE_KEY = `${NURBLE_KEY_PREFIX}_${NURBLE_API_KEY}`;
function getNurbleData(request){
    if(request.nurbleData){
        return nurbleData;
    }
    if(request.cookies[NURBLE_KEY]){
        /* nurble data is base64 encoded JSON */
        try{
            let nurbleData = JSON.parse(atob(request.cookies[NURBLE_KEY]));
            request.nurbleData = nurbleData;
            return nurbleData;
        }
        catch(err){
            console.error(`getNurbleData couldn't parse
                           nurble cookie: ${request.cookies[NURBLE_KEY]}`);
        }
    }
}
```

Oh, that’s pretty sweet, but now we’re returning data from two different places in the code. Everybody knows that’s asking for trouble, better refactor it so that there’s just one return, at the end.

```
/* extract nurbleData from the HTTP request */
const NURBLE_KEY_PREFIX = "nurble_";
const NURBLE_KEY = `${NURBLE_KEY_PREFIX}_${NURBLE_API_KEY}`;
function getNurbleData(request){
    let nurbleData = null;
    if(request.nurbleData){
        nurbleData = request.nurbleData;
    }
    else if(request.cookies[NURBLE_KEY]){
        /* nurble data is base64 encoded JSON */
        try{
            nurbleData = JSON.parse(atob(request.cookies[NURBLE_KEY]));
            request.nurbleData = nurbleData;

        }
        catch(err){
            console.error(`getNurbleData couldn't parse
                           nurble cookie: ${request.cookies[NURBLE_KEY]}`);
        }
    }
    return nurbleData;
}
```

That’s good, but that atob to parse in there is kinda complicated, let’s extract that and make it a separate function.

```
/* extract nurbleData from the HTTP request */
const NURBLE_KEY_PREFIX = "nurble_";
const NURBLE_KEY = `${NURBLE_KEY_PREFIX}_${NURBLE_API_KEY}`;
function getNurbleData(request){
    let nurbleData = null;
    if(request.nurbleData){
        nurbleData = request.nurbleData;
    }
    else if(request.cookies[NURBLE_KEY]){
        try{
            function parseNurbleCookieData(s){
                /* nurble data is base64 encoded JSON */
                let b = Buffer.from(s, 'base64');
                let o = JSON.parse(b);
                return o;
            }
            nurbleData = parseNurbleCookieData(request.cookies[NURBLE_KEY]);
            request.nurbleData = nurbleData;
        }
        catch(err){
            console.error(`getNurbleData couldn't parse
                           nurble cookie: ${request.cookies[NURBLE_KEY]}`);
        }
    }
    return nurbleData;
}
```

Mmm, now we’re cooking with good engineering. But what if some other function also needs to parse nurble cookie data? We can’t keep that as an inline function!

```
function parseNurbleCookieData(s){
    /* nurble data is base64 encoded JSON */
    let b = Buffer.from(s, 'base64');
    let o = JSON.parse(b);
    return o;
}

/* extract nurbleData from the HTTP request */
const NURBLE_KEY_PREFIX = "nurble_";
const NURBLE_KEY = `${NURBLE_KEY_PREFIX}_${NURBLE_API_KEY}`;
function getNurbleData(request){
    let nurbleData = null;
    if(request.nurbleData){
        nurbleData = request.nurbleData;
    }
    else if(request.cookies[NURBLE_KEY]){
        try{
            nurbleData = parseNurbleCookieData(request.cookies[NURBLE_KEY]);
            request.nurbleData = nurbleData;
        }
        catch(err){
            console.error(`getNurbleData couldn't parse
                           nurble cookie: ${request.cookies[NURBLE_KEY]}`);
        }
    }
    return nurbleData;
}
```

That `parseNurbleCookieData` function’s comment isn’t necessary any more, it’s all there in the title - but we could improve the comments in a few other parts of this function.

```
function parseNurbleCookieData(s){
    let b = Buffer.from(s, 'base64');
    let o = JSON.parse(b);
    return o;
}

/**
 * Sometimes Nurble will include nurbleData with the request cookie.
 * for example: nurble_983277382237=bGlnaHQgd29yaw...
*  We need this data to nurbleize the normandifferentiator.
*
*  memoizes nurble data in the request, for easier lookup
*/
const NURBLE_KEY_PREFIX = "nurble_";
const NURBLE_KEY = `${NURBLE_KEY_PREFIX}_${NURBLE_API_KEY}`;
function getNurbleData(request){
    let nurbleData = null;
    if(request.nurbleData){
        nurbleData = request.nurbleData;
    }
    else if(request.cookies[NURBLE_KEY]){
        try{
            nurbleData = parseNurbleCookieData(request.cookies[NURBLE_KEY]);
            request.nurbleData = nurbleData;
        }
        catch(err){
            console.error(`getNurbleData couldn't parse
                           nurble cookie: ${request.cookies[NURBLE_KEY]}`);
        }
    }
    return nurbleData;
}
```

oh, and those other bits are constants and utility functions, let’s hide them somewhere else in the application:

```
// CONSTANTS/nurble.js
const NURBLE_KEY_PREFIX = "nurble_";
const NURBLE_KEY = `${NURBLE_KEY_PREFIX}_${NURBLE_API_KEY}`;
```
-----
```
// common/nurbleParser.js
function parseNurbleCookieData(s){
    /* nurble data is base64 encoded JSON */
    let b = Buffer.from(s, 'base64');
    let o = JSON.parse(b);
    return o;
}
```
------
```
// nurble.js
const {NURBLE_KEY_PREFIX, NURBLE_KEY} = require('../../CONSTANTS/nurble.js');
const {parseNurbleCookieData} = require('../../common/nurbleParser.js');

/**
 * Sometimes Nurble will include nurbleData with the request cookie.
 * for example: nurble_983277382237=bGlnaHQgd29yaw...
*  We need this data to nurbleize the normandifferentiator.
*
*  memoizes nurble data in the request, for easier lookup
*/
function getNurbleData(request){
    let nurbleData = null;
    if(request.nurbleData){
        nurbleData = request.nurbleData;
    }
    else if(request.cookies[NURBLE_KEY]){
        try{
            nurbleData = parseNurbleCookieData(request.cookies[NURBLE_KEY]);
            request.nurbleData = nurbleData;
        }
        catch(err){
            console.error(`getNurbleData couldn't parse
                           nurble cookie: ${request.cookies[NURBLE_KEY]}`);
        }
    }
    return nurbleData;
}
```

Mmm. Yeah. Can you feel it? Can you feel the engineering? This code is getting so tight and refactored. The power.

-------

![](./but_why.gif)

## Worse... or Better?

Okay, so, I’m going to come clean. I am being flippant and sarcastic, here.

We have very clearly made our `getNurbleData` function quite a bit worse, right?

We’ve taken a simple, clear, easy-to-understand function and bloated it with unnecessary features, weird suppositions about the future, and wild overengineering.

We’ve taken valid errors and hidden them in logs.

We’ve added a memoization feature that nobody asked for, to save fractions of a millisecond of parsing time in the unlikely event that two different functions in the same request stack both need nurble data at the same time and don’t just save the original nurble data.

We’ve committed an act of **Ravioli Coding**, one of the lesser known software pasta sins, where we break simple, understandable functions down into teeny tiny little pieces and scatter those pieces to the four winds, even if nobody else needs those pieces for anything. We’ve taken important context and hidden it far away from the function that needs that context.

And, in fact, everything we’ve done to the function has been a good idea or best practice. There are lots of legitimate reasons to use the techniques I outlined here for good, it’s just that they can become an antipattern when applied unnecessarily and automatically.

In fact, me and another senior engineer spend a lot of our refactoring time taking code that looks like the end result of this process and de-refactoring it - peeling away “good engineering ideas” in successive layers, like an onion, until the code is once again simple.

## DRY is less important than WET, YAGNI, KISS

> “Everyone knows that debugging is twice as hard as writing a program in the first place. So if you’re as clever as you can be when you write it, how will you ever debug it?” - Brian Kernighan

“DRY”, “Don’t Repeat Yourself”, is a core engineering instinct. Do not ever write code twice, because you can always, always build an abstraction that allows you not to do that.

Too-aggressive application of DRY, though, almost always results in just the gnarliest code.


```
0 -> I

1 -> Is

2 -> If

3 -> To

4 -> Like

5 -> This

6 -> Efficiency

7 -> Improve

8 -> Talked

9 -> Imagine

4, 9 2 0 8 4 5 3 7 6
```


```
Like, Imagine If I Talked Like This To Improve Efficiency
```

Look at how neatly I refactored that sentence! Using reusable, composable functions! Toss in some parens and I’m a Lisp programmer.

But… consider self-similarity, consistency, simplicity, readability. **Boilerplate can be good actually**.

---------

## Enterprise Hello World

This is just one function - if you apply this kind of aggressive ENGINEER THINKING to an entire library, you end up with ENGINEER CODE. It’s modular! It’s functional! It’s composable! It’s modifiable! It’s…

![](./its_worse.gif)

It's [Enterprise Hello World](https://gist.github.com/lolzballs/2152bc0f31ee0286b722).

There are a lot of antipatterns out there but this is one of the ones that I see _most often_, because - well, we’re surrounded by good engineers with extremely high tolerance for complexity and abstraction, and they just want to take their good code and make it into really good code. They want to foresee every possible use of their function and _have an answer prepared_.

Databases end up over-normalized. Machinery ends up tying things to config values that don’t need to be configured.

![](./am_i.png)

“Am I so out of touch with modern engineering practices?”

![](./no.png)

“No, this code did not need to be this complicated.”