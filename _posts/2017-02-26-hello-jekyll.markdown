---
layout: post
title:  "Hello Jekyll!"
date:   2017-02-26 12:08:50 +0530
categories: thoughts
comments: true
---
> Because nothing is sweet as loading a site full of goodies powered by static html from a server, rocket fast!

I _deprecated_ my [old blog](http://blog.amitosh.xyz) and moved everything to jekyll.
And I am sharing my experiences about it.

[Jekyll](https://jekyllrb.com) is a static site generator. It gulps plain text and transforms them
 into static websites and blogs. And it's really easy to configure. And for web developers like me
 who know their way around HTML, wordpress and other CMS are just too mmuch of a barrier.

 Well, I do know how to write a wordpress theme and etc, Wordpress has its own set of demerits - it
 requires a server to run PHP. It requires careful attention because Wordpress is _very secure_. And
 so _fast_.

I then poked around few more CMS, - [Anchor](http://anchorcms.com) [Ghost](http://ghost.org), these
are the ones I actually liked - but all of them required maintaining a sort of server. And I happily
did that for long. I manage a couple of Ghost and Wordpress installations, and mine own use to live
there among its neignbours.

And then came the day of misfortune, a friendly wordpress neighbour suddenly turned rougue (which was
infact pwned) and created disharmony on the server. I was forced to take a move and split all wordpress
into their own servers. (And subsequently brought them back again after the containers happened).

But Jekyll is unique and nice because you don't require to manage a ultra secure PHP installation or a
squeaking NodeJS joined with like wainscot. Simply make a directory, run nginx, rsync some files and site
is live.

Strictly speaking, with Cloudflare and AWS S3, you can host a SSL enabled Jekyll site @ few cents per month.
And if you decide to go the GitHub Pages or Firebase route, you can get hosting for free. Amazing, Isn't ?