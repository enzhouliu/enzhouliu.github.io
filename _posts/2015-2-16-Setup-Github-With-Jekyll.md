---
layout: post
title: Setup Github With Jekyll
---

Today, I started my first tech blog here. The reason for starting it is that I found I have sloved many problems during work and side-projects.
Some of them requires lots of time and efford. But none of them seem left after a long time.
After a research for which blog site is the best, I found I can hold pages on github and it supports Jekyll which is a blog aware static site framework.

I would love to have a blog simple enough for myself to maintain and also flexible to add new things I want.
Then I choose this way but not blog services like [Blogger](https://www.blogger.com/). <b>More important, I love github and prefer to put all my stuff together.</b>

Basically, Jekyll is a framework with a ruby program which can generate html files based on your Markdown/Textile/Liquid files. There are layout files define the html structure.
And there are markdown files define the content/variables. For websites like blog, almost all the post can be applied to similiar template. the ruby linked them together create static html files.
I highly recommand you setup jekyll in your local and generate a clean project to see how it is structed.

The steps of setting up is pretty straight forward.
just follow this steps on
[github pages](https://pages.github.com/)

1. Create a account in github (I had done long time ago)
2. Create a github User or organization site. (create a branch called username.github.io)
3. Optional - install jekyll in your local, incase you want to test locally when you add new feature.
4. You can fork other people's pages code or create your clean jekyll using following command:
{% highlight ruby linenos %}
 $>> jekyll new [PROJECT]
{% endhighlight %}
5. Add your own user variables and start edit your post.
6. Submit && push your code and check them out on github

I did this quick without going into too much detail, please let me know if i say anything wrong, thanks!