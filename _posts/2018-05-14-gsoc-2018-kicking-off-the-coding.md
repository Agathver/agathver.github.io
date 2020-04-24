---
layout: post
title:  'GSoC 2018: Kicking off the Coding'
date: '2018-05-14 21:25:34+05:30'
categories: stories
tags: [gsoc, fedora]
comments: true
redirect_to: 'https://medium.com/@agathver/gsoc-2018-kicking-off-the-coding-fc509c91e6e3'
---
It's May 14, and this is when we officially start coding for GSoC, 2018 edition. This time, I would be working on [improving the Fedora Community App]({{ site.baseurl }}{% post_url 2018-04-29-summer-code-and-fedora %}) with the Fedora project. This marks the beginning of a journey of 3 months of coding, patching, debugging, git (mess) and the awesome discussions with my mentors and the community.

The [Fedora App](https://pagure.io/Fedora-app) is a central location for Fedora users and innovators to stay updated on The Fedora Project. News updates, social posts, Ask Fedora, as well as articles from Fedora Magazine are all held under this app.

For the first month of my GSoC coding, I will be working of improving the code quality by completing the TypeScript conversion I started earlier as well as I will also be working on bring offline capabilities to Fedora Magazine reader and the Fedora Social.

## Why TypeScript?
[TypeScript (TS)](https://www.typescriptlang.org/)  is a super-set of JavaScript (JS) that brings optional static typing features to JS. TS is the language used by Ionic - the framework on which the Fedora Community App is built. I personally believe, in a medium to large scale project, a static type system is necessary - it especially helps to catch errors very early and allows to perform safe refactors, but I also agree that there are cases where it's useful to fallback to dynamic typing.

TS does just this. It is implemented as a syntax extension to JavaScript which is transpiled into JS using the TS compiler. Every valid JS syntax is also valid TS syntax, so it becomes easier to port a project - even partially to use TS features, without incurring the cost of a full project rewrite.

In my earlier work in updating the source code to the latest version of Ionic, I started a conversion from JS to TS. Essential parts are already in place, but still there is a big chunk left for conversion. In my first week of coding, I will be working on this conversion.

## Offline Capabilities in Fedora App
Currently, the app lacks any offline capabilities - you always require an internet connection to perform most of the actions.

My work will be to bring offline storage and sync to Fedora Magazine and Fedora Social sections of the app. This will both improve the apps usability and performance. From a UX perspective, we will start syncing data rather than blocking the user from doing anything (as it is currently). The user can continue to act on cached items, while we continue to fetch new items in the background.

Further in second month, I will be working on implementing a lightweight blog reader and caching complete offline copies of user-selected articles.
