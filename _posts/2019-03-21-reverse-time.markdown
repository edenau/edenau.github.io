---
title: "️⏪ Scientists are Now Able to 'Reverse Time'"
layout: post
date: 2019-03-21 23:00
published: true
headerImage: false
tag:
- science
- critique
category: blog
author: edenau
description: "Is a time machine coming soon?"
# jemoji: '<img class="emoji" title=":open_file_folder:" alt=":open_file_folder:" src="https://assets.github.com/images/icons/emoji/unicode/1f5c2.png" height="20" width="20" align="absmiddle">'
---

***This post is now available on <a href="https://hackernoon.com/scientists-are-now-able-to-reverse-time-b507fb4395c5" target="_blank">Hacker Noon — Medium</a>. Check it out!***

## Dear Python programmers,

Given a Python list `l = [50, 99, 67, 99, 48]`, how do you find the ***argmax*** of the list? Turns out there is no built-in ***argmax*** function for Python list! You can’t just do `argmax(l)` or `max(l).index`. The first thing I can think of is to convert the list to a *numpy* array, or *pandas* DataFrame, or even *Tensorflow* tensor if you believe it is okay to overkill:

```
import numpy as np
l_np = np.asarray(l)
print(l_np.argmax())

import pandas as pd
l_pd = pd.DataFrame({'l': l})
print(l_pd.idxmax())

import tensorflow as tf
l_tf = tf.constant(l)
with tf.Session() as sess:
    print(sess.run(tf.argmax(l_tf)))
```

They all return `1`, as they should be. If multiple items are maximal, the function returns the first one encountered. But it feels like cheating transforming an object to another data type, and it also requires more execution time. Here are several ways to fix it:

<div class="breaker"></div> <a id="1"></a>

# So... What is Happening?

Are we able to reverse time? **Kind of**.
Are we one step closer to a time machine? **Not really**\*.
Did they violate any physics that we think we know of? Nah.

Scientists are now able to ‘simulate’ the previous state correctly given the current state 85% of the time in a two-qubit quantum computer, and only 50% in a three-qubit setting. Let’s be clear, that is by no means a violation of the second law of thermodynamics or any physics. We all could play a video backward and we would have ‘reversed time’, without violating any physics of course. In general, you could theoretically simulate any reversible process backward in time.

This is not about time travel.

<div class="breaker"></div> <a id="2"></a>



<div class="breaker"></div> <a id="3"></a>
