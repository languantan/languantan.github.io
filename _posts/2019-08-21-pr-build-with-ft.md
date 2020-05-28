---
layout: post
title: "PR Pipeline Costs and Benefits"
date: 2019-08-21 01:59:40 +0800
categories: [pull-requests, functional-tests]
excerpt_separator: <!--more-->
---

### Question: Given a PR process, instead of just doing unit tests, linting in the PR build, why not do full suite of Functional tests as well?

<!--more-->

For me, when I started this, the aim was to give a _quick feedback_ to engineers to fix, namely, breaking regression unit tests or JS webpack issues when run in production mode. Because after a PR, usually there would be a few comments given and changes are made and we need a quick way to know that those changes will not break regression.

### Cost

I did not want to run UI Automation tests because of the following two costs:

1. Have multiple environments (including App, DB, domain), one for each PR (at least 6-7 active PRs at one time)

2. For each change that is made for a PR comment(s), wait for about 4mins to deploy the environment, and another 4mins for the FT to run. In comparison, my unit tests take at most 2mins to run.

### Benefits

With UI Automation in PR, it will make the master branch _clean_ and PRs will not be impeded by a failing build. This impediment is mostly felt if there is an urgent bug that needs to reach production.

### Costs v Benefits

When I weighed the two, I couldn’t come to terms with the costs of maintaining multiple environments. (Imagine if you cannot merge a PR for a text change, because the FT cannot connect to the DB?)

In addition, I wanted to let the team know the regression effect of their change, especially if they are doing frequent updates to their PR etc.

On the point of having last minute hot fixes that are unable to push in due to a failing build, I have two ways to mitigate this.

- One, build a sense in the team to fix a build immediately, even it means doing a revert immediately.

- Two, deploy frequent enough that you do not need hot fixes, instead you could just wait for the next deployment.

### Caveats

Granted, I could have decoupled my FT from the environment, e.g. having a local mysql docker container or isolating it so I do not need to deploy it. But even then, I strongly advocate that we shouldn’t rely too much on FT to catch regression bugs. Where possible, they should be caught in unit tests or some other test that is less costly.
