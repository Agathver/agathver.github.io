---
layout: post
title:  "Summer, Code and Fedora"
date: '2018-04-29 16:11:19+05:30'
categories: stories
tags: [gsoc, fedora]
comments: true
---
I have been selected for [Google Summer of Code][1] (GSoC) 2018. I’ll be working on developing the backend for the [Fedora Community App][2].

[![Google Summer of Code](https://developers.google.com/open-source/gsoc/resources/downloads/GSoC-logo-horizontal.svg)](https://summerofcode.withgoogle.com)

[![Fedora Project]({{ "/public/img/fedora.png" | absolute_url }})](http://fedoraproject.org/)

Fedora has an android app which lets a user browse [Fedora Magazine][3], [Ask Fedora][4], [FedoCal][5] etc within it. This app is build using the Ionic Framework, Angular and Cordova. Essentially it is a cross-platform hybrid app.

## Project Description

In the current form, most of the functions in the app rely on an in-app browser to render content. This project aims to improve the existing Fedora App for Android for speed, utility and responsiveness, introduce a deeper native integration and make the app more personal for the user.

During the GSoC period I aim for the following deliverables:

1. Refactored, and restructured code for the android app, providing native experience to the fullest.
2. Deeper integration with system as well as various Fedora Infra apps.
3. An Ionic hybrid app that is publishable to app stores like Google Play Store and F-Droid
4. Providing an offline first experience – caching data from Fedora Magazine and Social and optionally, making some of the content accessible offline.

## What would I be working on

The Fedora android app is developed using the [Ionic framework][6] and [Cordova][7]. It is a framework for developing cross-platform mobile apps, also called as hybrid apps using HTML, CSS and JavaScript.

Here are some of the features I plan to integrate this summer:

1. Offine data for posts and calendar.
2. Syncing system calendar with events from FedoCal.
3. Fedora package search.
4. Bookmarks and offline reading.
5. [FMN][8] notifications.

## About Google Summer of Code

Google Summer of Code (GSoC) is a yearly program by Google to help the open source communities to reach out to student contributors. Organisations pitch projects, and when selected, pick up university students to work on these projects or their own ideas related to the organisation’s project(s).

This is my [second][9] GSoC participation. Last summer (2017), I worked with the [FOSSi Foundation][10] for bringing [code quality metrics for projects listed under LibreCores.org][11].

While, the summer this time is really really hot (I mean 40°C/104°F in April!), it is going to be interesting!

[1]:https://summerofcode.withgoogle.com
[2]:https://pagure.io/Fedora-app
[3]:https://fedoramagazine.org/
[4]:https://ask.fedoraproject.org
[5]:https://apps.fedoraproject.org/calendar/
[6]:http://ionicframework.com/
[7]:https://cordova.apache.org/
[8]:https://apps.fedoraproject.org/notifications
[9]:{{ site.baseurl }}{% post_url 2017-05-23-summer-code-and-librecores %}
[10]:https://fossi-foundation.org
[11]:{{ site.baseurl }}{% post_url 2017-07-02-one-month-of-gsoc-with-fossi %}
