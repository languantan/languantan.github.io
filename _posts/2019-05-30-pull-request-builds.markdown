---
layout: post
title: "Faster Feedbacks for pull requests"
date: 2019-05-30 01:49:00 +0800
categories: [devops, pull-requests]
excerpt_separator: <!--more-->
---

<p> Recently, we re-introduced pull requests as a form of sharing knowledge in the team. But it came with its own set of pains </p>
<!--more-->

1. Shared build agent, often led to queues. Worse when agent is hung.

   > ![bamboo-queued-job](/assets/20190530/queued.png){:width="400px"}

2. Pull Requests had no checks, meant that commits could lead to breaking of the trunk. There wasn't a way to automatically enforce checks, the reviewer would have to check out the code and run tests himself. And we all know, we get lazy to get out of bed sometimes.

   > ![bro-merged](/assets/20190530/bro-merged.png){:width="400px"}

3. Pipeline was doing everything, testing, building, deploying, UI testing

- Whole process took 17 minutes
- which takes Forever, when it comes to pull request checks

## Results

<p>With pain points, we needed some criteria to measure if we have solved our pains right? so here they are, albeit its done retrospectively since there wasn't much benchmarks to begin with?</p>

1. Use bamboo elastic agents, spin up as many and when required.

- Save costs since instances are not running 24/7.
- Time saved trying to revive instances when it hangs.
  > ![elastic-agents](/assets/20190530/elastic-agents.png){:width="400px"}

2. Run jobs in parallel instead of sequential

- Multiple pull requests could also be built at the same time
  > ![parallel-jobs](/assets/20190530/parallel-jobs.png){:width="400px"}
- As bamboo jobs could only support 1 Code Coverage (Clover) report each, a fortunate by-product running the unit tests for the frontend/backend separately meant that I could gather the code coverage for each.
  > ![coverage-result](/assets/20190530/coverage-result.png){:width="400px"}

3. Lower build times as a result

- Elastic agents need warmup time, if not longest job in the stage takes 1 minute
- Caveat was that deploy step to CI server was removed as well, since that was unnecessary for Code Review

| Before                                                                               | After                                                                        |
| ------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------- |
| ![regression-build-time](/assets/20190530/regression-build-time.png){:width="400px"} | ![new-feedback-time](/assets/20190530/new-feedback-time.png){:width="400px"} |

## (17 - 4) / 17 = 76.47% faster feedback time

<p>So, there are definitely improvements to the process and if you would like to do the same, read on for the details!</p>

## Step-by-step

### Bamboo setup

<p> The first thing I did was to setup Bamboo Elastic Agents. As my requirement was to support running of tests for each pull request and their subsequent commits, using the existing shared agent was just not able to support this.</p>

<p> As our codebase was using Ruby on Rails for the backend and ReactJS with Typescript on the frontend, it made sense to split them into different jobs to run in parallel. I also ran tslint to ensure that basic hygience was there. With the jobs split, they could still queue behind the same agent, defeating the purpose of splitting them in the first place. So I setup custom requirements for each job that tied to their own Elastic Images.</p>
