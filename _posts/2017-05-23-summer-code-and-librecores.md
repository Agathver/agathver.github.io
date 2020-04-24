---
layout: post
title:  "Summer, Code and LibreCores"
date:   2017-05-23 11:27:10 +0530
categories: stories
tags: [gsoc, librecores]
comments: true
redirect_to: 'https://medium.com/@agathver/summer-code-and-librecores-e46212f6a289'
---
I have been selected for [Google Summer of Code][1] (GSoC) 2017. I’ll be working on displaying metrics about projects listed at [LibreCores.org][2] under the [Free and Open Source Sillicon Foundation][3].

<img src="https://developers.google.com/open-source/gsoc/resources/downloads/GSoC-logo-horizontal.svg" />
<img style="margin: auto;height: 128px" src="https://www.librecores.org/img/logo_small.svg" />

LibreCores is a place to find free and open-source hardware IP cores and to meet the community driving it. It lists various open-source IP cores for FPGAs and SoCs for the community to discover and use.

## Project Description
A user browsing for cores on LibreCores will be interested to know the quality of the project’s
code so as to determine how useful the project will be to them. Such information can be
obtained from feedback from users using the project, and also from the project’s code.

This projects aims to collect code quality data that can be extracted, from a project's git
repository. Such metrics may be from repository metadata as well as from the structure and
content of source files. Individual metrics would be combined to calculate the overall repository
code quality which may be further used for purposes such as search ranking.

During the GSoC period I aim for the following deliverables:
1. Display basic metrics such as commit activity and contributors etc.
2. Display additional metrics such as issues and pull requests using external sources such as GitHub API.
3. Presenting the collected metrics as charts.
4. Detecting license from the repositories license text.

## What would I be using
I will mostly be working on PHP using [Symfony][4] framework. I will also gain exposure to Git internals, various REST APIs of Github, Gitlab, Bitbucket etc., Async task runners using RabbitMQ and a lot more database and cache internals.

## About Google Summer of Code
Google Summer of Code (GSoC) is a yearly program by Google to help the open source communities to reach out to student contributors. Organisations pitch projects, and when selected, pick up university students to work on these projects or their own ideas related to the organisation’s project(s).

This is my first GSoC and this summer is going to be interesting!

[1]:https://summerofcode.withgoogle.com
[2]:https://www.librecores.org
[3]:https://fossi-foundation.org
[4]:https://symfony.com
