---
title: "📛 Why you should remove momentjs from your application"
date: 2022-07-15T20:00:00+02:00
draft: false
---

# Why you should remove momentjs from your application

MomentJs is one if not the most used library to work with dates in JavaScript.  
As you probably know, dates are not a fun part of many languages and cleary not in vanilla JavaScript.

So most of the time if you don't want to recreate a library that doesn't exist you're probably going to look for a good one.  
And as a first result MomentJs seems a nice idea, but don't get fooled !

# But why is it a bad idea to use MomentJs ?

One of the first reason is the package size (235.4k and 66.2k zipped) and that's cleary a lot for a library that is only handling dates.

But the main reasons I'm saying that you should cleary not use it is performance issue.  
Of course if you're only working on one date per page it won't affect any performances, or at least it won't have a clear impact on your application.

But when you'll start manipulating more and more date the slower it gets, and it can get really bad on heavy charges...
So why would we use a library that is heavy to download and then is slow to process our date ?

Yes, you're right there's no reasons for that.

# Ok I won't use MomentJs but how can I replace it

The best thing for you is that you're asking this questions before you wrote your application code, even if you already did it don't worry it's not an enormous process to switch to a better thing.

As a personnal recommendation I will tell you to use date-fns (https://date-fns.org/).  
This is a way more compact lib and you can also only import the functions you need.

It means that if you only want to check if your string is matching a specifc date format you can only import your `isMatch` function like this :

```js
import { isMatch } from 'date-fns'

function isCorrectDateString(dateString) {
    return isMatch(stringDate, 'yyyy-MM-dd')
}
```

And here you go. Small code imported and fast result.

# How can I migrate from MomentJs to another library

If you already wrote your code previously with MomentJs here is a good repository that give you all the replacements for main date manipulating libraries :

https://github.com/you-dont-need/You-Dont-Need-Momentjs#millisecond--second--minute--hour
