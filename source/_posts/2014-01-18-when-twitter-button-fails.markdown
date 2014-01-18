---
layout: post
title: "when twitter button fails"
date: 2014-01-18 15:07:54 -0500
comments: true
categories: [twitter api, html, bugs]
---

I was designing a webpage with [twitter buttons](https://about.twitter.com/resources/buttons) and [embedded tweets](https://dev.twitter.com/docs/embedded-tweets) when I noticed that twitter's javascript source *breaks* for some html documents I had written. 

<!--more-->

Inspecting my web browser's javascript console, I noticed ``Uncaught TypeError: ...`` occuring on my webpage:

{% img center /images/twtrbug_1.png %}

I started dissecting the html document that was causing this issue, and ended up stripping it down to the following html document.

``` html Breaking twitter button 
<!DOCTYPE html>
<html>
    <form>
      <input name="lang"></select>

      <!-- copy and paste twitter button -->
      <a href="https://twitter.com/<twtrid>" class="twitter-follow-button pull-left" data-show-count="false" data-size="large">Follow @bsbot_lol</a>
      <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
      <!-- end of copy and paste -->
    </form>
</html>
```

Intrepreting the result: 

> A twitter button placed inside html **form** tag along with an **input** tag that has its **name** attribute set to **lang**  breaks twitter's widget.js produces and error.


An easy solution to this problem is to change the ``name`` attribute to be something other than *lang*, for instance, *langs*.

``` html another html Breaking twitter button 

<!DOCTYPE html>
<html>

    <form>
      <input name="langs"></select>

      <!-- copy and paste twitter button -->
      <a href="https://twitter.com/<twtrid>" class="twitter-follow-button pull-left" data-show-count="false" data-size="large">Follow @bsbot_lol</a>
      <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
      <!-- end of copy and paste -->
    </form>
</html>
```

Twitter follow button, then, is rendered correctly. My guess is that there are other names that will conflit with twitter's widge javascript code.


