---
layout: post
title:  'GSoC Project Report : Quality Metrics for Projects listed on LibreCores.org'
date:   '2017-08-28 21:15:11+05:30'
categories: stories
tags: ['librecores', 'google summer of code']
comments: true
---

[LibreCores.org](https://www.librecores.org/) lists free and open source "IP Cores" on the website for the community to view and use. Currently the website extracts the project readme and license and renders them on the project page, along with links to the project homepage and git repository.

A user browsing for cores on LibreCores will be interested to know the quality of the project’s code so as to determine how useful the project will be to them. A part of such information can be inferred from the project's source code repository and activity in issue trackers. Also, these metrics may be put to use in search ranking in the future.

In this project I worked on collecting and visualizing metrics about project quality of projects listed on [LibreCores.org](https://www.librecores.org/). These metrics include the frequency of code commits, activity in issue trackers, code quality, presence of documentation in code and contributors to the project.

I worked on my GSoC project from May to August 2017. My work was divided into four phases:

1. Investigating the requirements, designing the solution (during community bonding)

2. Extracting metrics from code repository

3. Extracting metrics from repository hosting services (like GitHub).

4. Calculating a combined score of the the individual metrics and UI improvements.

The original proposal for the project is available [here](https://docs.google.com/document/d/1VC0kSVr9gEtBq-gL9vQtzjc7rut7VvXDaYgE2xtUP98/edit?usp=sharing). 

The entire tracking map for my project can be found  [here](https://github.com/librecores/librecores-web/projects/3). 

The list of all issues and PRs (tagged GSoC 2017) for my project can be found [here](https://github.com/librecores/librecores-web/issues?q=label%3A%22gsoc%202017%22).

The corresponding project on GSoC 2017 Website can be found [here](https://summerofcode.withgoogle.com/projects/#5672435520110592)

# Designing the solution

During the community bonding period, I identified metrics that are relevant for a user while making decisions and the methods to extract and store them. My mentors - Philipp and Oleg, and I decided to implement code for Git repositories. The planning also continued in later phases and the document is publicly available [here](https://gist.github.com/agathver/96655d078569fc397485e011b0d63546).

## What information can we get from the codebase ?

<div class="image">
    <a href="/public/img/metrics-1.png">
        <img alt="Information from a GitHub repo" src="/public/img/metrics-1.png" />
    </a>
    <div class="image-caption">
        Information from a GitHub repo
    </div>
</div>

<div class="image">
    <a href="/public/img/metrics-2.png">
        <img src="/public/img/metrics-2.png" />
    </a>
    <div alt="Information from the commit history of the project" class="image-caption">
        Information from the commit history of a project
    </div>
</div>

# Extracting metrics using Git CLI

The first phase of GSoC was to extract and present metrics from project source code repositories. This included cloning , parsing the reflog using Git CLI and extracting commit and contributor information. The extracted data is also being displayed as a commit graph and a contributor list on project pages on [LibreCores.org](https://www.librecores.org/).

In this phase I made the following PRs:

1. [#150](https://github.com/librecores/librecores-web/pull/150) - Extract and display project activity metrics

2. [#164](https://github.com/librecores/librecores-web/pull/164) - Add default value to languageStats column in DB [BUGFIX]

# Extracting metrics from GitHub

The second phase of GSoC was to extract data from repository hosting and issue tracking services like GitHub. I implemented integration with GitHub and GitHub Issues. The projects now have open issues and PRs mentioned in their respective project page as an indicator of their activity.

In this phase I made the following PRs:

1. [#161](https://github.com/librecores/librecores-web/pull/161) - Import additional metrics from GitHub (TODO: Get data from GitLab, BitBucket)

2. [#167](https://github.com/librecores/librecores-web/pull/167) - Extract release versions from Git tags

# Calculating combined score and improving UI

My third phase, was again sub-divided into two parts - calculating combined metrics and improving the UI.

In the first part, I implemented a heuristic formula to calculate a quality score of a project. We had [a lot of discussions in our mailing list](https://lists.librecores.org/pipermail/discussion/2017-August/000378.html) about the relevant metrics, and finally we came up with a [formula](https://github.com/librecores/librecores-web/issues/165). 

Here are the links to my pull requests:

1. [#169](https://github.com/librecores/librecores-web/pull/169) - Display a combined score of project quality

Since, addition of so much display elements crowded the project page, it was necessary to give it a refresh. On the mailing list I [posted some sketches](https://lists.librecores.org/pipermail/discussion/2017-August/000393.html) and received feedback from the community. I also implemented a new UI for LibreCores.org project page. The new UI prioritizes a few key metrics and adds a detailed section about numerous other project quality metrics.

Here are the links to my pull requests:

1. [#172](https://github.com/librecores/librecores-web/pull/172) - Add meta tag for correct viewport sizing

2. [#173](https://github.com/librecores/librecores-web/pull/173) - Add tab showing project metrics 

<div class="image">
    <a href="/public/img/ui-redesign.png">
        <img alt="'Project metrics' tab" src="/public/img/ui-redesign.png" />
    </a>
    <div class="image-caption">
        "Project metrics" tab
    </div>
</div>

<div class="image">
    <a href="/public/img/ui-redesign-2.png">
        <img alt="View in search page" src="/public/img/ui-redesign-2.png" />
    </a>
    <div class="image-caption">
        View in search page
    </div>
</div>

# Additional Contributions

Apart from the main deliverables above, I also contributed a few other patches.

1. [PR #145](https://github.com/librecores/librecores-web/pull/145) - Added means to customize local vagrant instance like changing RAM, CPU, network etc, without affecting essential project configuration.

2. [PR #147](https://github.com/librecores/librecores-web/pull/147) - Introduced testing using phpunit and continuous integration using Travis-CI

3. [PR #149](https://github.com/librecores/librecores-web/pull/149) - Tweaked nginx configuration to improve the website performance.

4. [PR #152](https://github.com/librecores/librecores-web/pull/152)  - Adds tests for routes to make sure every route at least renders. Does not cover much routes yet.

# Conclusion

## What tasks were accomplished

|Task|Planned|Completed|
|----|-------|---------|
|Extracting commits and contributors using Git CLI|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|
|Issue tracker info from GitHub issues|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|
|Pull requests info|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|
|Calculating a combined project quality score from all available metrics|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|
|Determine the major language used in the project project|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|
|Detecting license from text|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|<span class="cross"><i class="fa fa-times" aria-hidden="true"></i></span>|
|Improving the UI of project page|<span class="cross"><i class="fa fa-times" aria-hidden="true"></i></span>|<span class="tick"><i class="fa fa-check" aria-hidden="true"></i></span>|

## Future work

Although, this was my task for GSoC, there is still much scope for future work. The formulae for calculating repository quality score uses hard-coded constants. These are not extensively tested. An improved solution could be to integrate some regression algorithms to calculate the score.

## What did I learn in GSoC

1. Valuable experience in designing database schemas - denormalizing schemas for performance reasons.

2. Operation of message queue systems such as RabbitMQ.

3. Knowledge about UNIX process model, IO redirection and IPC using pipes, mitigating shell escape vulnerabilities.

4. GraphQL and its usage in a real world API.

5. Observed a lot of patterns, styles, conventions and best-practices of development in the real (open-source) world.

6. Learnt a lot about importance of linters, CI and code styles while working on a project with many contributors.

